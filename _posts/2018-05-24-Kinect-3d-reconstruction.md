---
layout: post
title: Kinect 三维重建
date: 2018-5-24
categories: blog
tags: [Kinect,三维]
description: Kinect 三维重建 基本原理 
---

# Kinect FACE Reconstruction

## 三维重建

三维重建就是通过建立模型去描述真实物体的三维外形的技术。
1. 相较于二维图像，拥有更多的信息
2. 在AR\VR中实现真实与虚拟的连接
3. 让机器更好的认识世界

## KinectFusion
![KinectFusion 算法流程图](https://images2017.cnblogs.com/blog/1107868/201801/1107868-20180121222920584-1587043074.png "图 1 KinectFusion 算法流程图")
![pic1]({{site.baseurl}}/blogimg/kinectfusion.png)

KinectFusion主要有四部分：
1. 采集原始RBGD数据，得到深度图像，获取点云voxel坐标和法向量
2. 根据上一帧预测出的点云和当前数据计算当前相机姿态
3. 根据相机姿态更新TSDF 
4. Ray-cast? TSDF来计算当前预测的曲面


## Kinect 点云获取及去噪

在背景比较简单的环境下，控制深度信息就可以把人脸区域提取出来。原始数据的噪声较大，可考虑均值滤波等方法去噪。
为了保持清晰的边缘，一般采用双边滤波。

> 一般的滤波是在空间域做加权平均，像素越靠近中心点，权重越高。双边滤波是在空间域加权平均的基础上再对值域加权平均，即像素灰度值越靠近中心像素的灰度值，权重越高。在边界附近，灰度值差异很大，所以边界两边的像素在空间域靠在一起，但是由于灰度值差别非常大，对于互相的权重很低，所以可以保持清晰的边界。

![双边滤波](https://images2017.cnblogs.com/blog/1107868/201801/1107868-20180121222957193-187410483.png)
![pic2]({{site.baseurl}}/blogimg/dualfilter.png)

![原始数据处理流程](https://images2017.cnblogs.com/blog/1107868/201801/1107868-20180121222943396-1329037724.png)
![pic3]({{site.baseurl}}/blogimg/odataprocess.png)

拿到降噪后的深度图$D_k$之后，再根据相机内参$K$，可以反投影出每个像素点的三维坐标，这就是Vertex map $Vk_$。公式中u是像素坐标，u˙是对应的齐次坐标。每个vertex的法向量可以很方便的通过相邻vertex用叉乘得到。然后对深度图降采样，行数、列数各减一半。降采样使用的是均值降采样，即深度图上四个相邻像素的深度值被平均成一个值。构建三层金字塔的目的是为了从粗到细地计算相机位置姿态，有加速计算的效果。

## 相机姿态

相机的姿态是用ICP（Iterative Closest Point）算法求解的。
ICP算法对待拼接的2片点云，首先根据一定的准则确立对应点集P与Q，其中对应点对的个数，然后通过最小乘法迭代计算最优的坐标变换，即旋转矩阵R和平移矢量t，使得两块点云的差别最小，迭代求解拍摄两块点云的相对位置。
有两种方式，一种是point-to-point和point-to-plane两种,后者收敛速度快，而且鲁棒性更好[4]。

![point-to-plain](https://images2017.cnblogs.com/blog/1107868/201801/1107868-20180121223016771-1650719820.png)
![point-to-plain2]({{site.baseurl}}/blogimg/point-to-plain.png)







## TSDF update

先介绍一个概念SDF(Signed Distance Function)，SDF描述的是点到面的距离，在面上为0，在面的一边为正，另一边为负。TSDF(Truncated SDF)是只考虑面的邻域内的SDF值，邻域的最大值是max truncation的话，则实际距离会除以max truncation这个值，达到归一化的目的，所以TSDF的值在-1到+1之间
![](https://images2017.cnblogs.com/blog/1107868/201801/1107868-20180121223052459-1534928814.png)
![tsdf]({{site.baseurl}}/blogimg/TSDF.png)

## surface prediction

更新完TSDF值之后，就可以用TSDF来估计voxel/normal map。这样估计出来的voxel/normal map比直接用RGBD相机得到的深度图有更少的噪音，更少的孔洞（RGBD相机会有一些无效的数据，点云上表现出来的就是黑色的孔洞）。估计出的voxel/normal map与新一帧的测量值一起可以估算相机的位置姿态。具体的表面估计方法叫Raycasting。这种方法模拟观测位置有一个相机，从每个像素按内参K投射出一条射线，射线穿过一个个voxel，在射线击中表面时，必然穿过TSDF值为一正一负的两个紧邻的voxel（因为射线和表面的交点的TSDF值为0），表面就夹在这两个voxel里面。然后可以利用线性插值，根据两个voxel的位置和TSDF值求出精确的交点位置。这些交点的集合就呈现出三维模型的表面。

## 后续章节都是ICP方法

在提取深度信息后，要进行一定的坐标系转换。
`OpenNI2 SDK`库中 `ConvertDepthToWorld()`
在获取到三维模型`*.obj`文件后可以再采用一些滤波算法进行去噪，如双边滤波。
knn算法可以使用`ANN`库来计算。

## 点云配准

点云配准`(Point Cloud Registration)`广泛用于三维重建，Kinect只能扫描到正面，需要对物体进行多角度扫描。两种方式，深度相机不动物体转动；或者物体不动，深度相机转动。
上述两种方式扫描出来的各个面的深度信息的坐标系发生了平移或旋转，涉及的参数叫做运动参数，利用三维模型重叠区域的数据来估计运动参数，继而计算出旋转矩阵和平移向量，然后就可以得到精确的配准模型。

运动参数主要有**6**个自由度（旋转矩阵3个，平移向量3个），需要找到一个全局最优的6纬向量，这事一个非线性优化问题。通过最小化两个点集最近点的距离的平方和来获得旋转矩阵，然后利用旋转矩阵获得平移向量。

## ICP算法 

 ICP算法对待拼接的2片点云，首先根据一定的准则确立对应点集P与Q，其中对应点对的个数，然后通过最小乘法迭代计算最优的坐标变换，即旋转矩阵R和平移矢量t，使得误差函数最小，ICP处理流程分为四个主要的步骤：

1. 对原始点云数据进行采样
2. 确定初始对应点集
3. 去除错误对应点对
4. 坐标变换求解




## 点云网格化

可以用Poisson表面重建计算获取，误差较大，如果要更加精确可以通过meshlab处理后再导入3ds max 或 Maya处理。





## 参考文献
[1] 基于Kinect的三维物体重建.杨红庄.郑州大学.2013

[2] [基于PCL的三维重建——点云配准（一）ICP算法实现](https://blog.csdn.net/qq_33933704/article/details/78652633)

[3] [KinectFusion解析](https://www.cnblogs.com/zonghaochen/p/8325905.html)

[4] Rusinkiewicz S, Levoy M. Efficient variants of the ICP algorithm[C]//3-D Digital Imaging and Modeling, 2001. Proceedings. Third International Conference on. IEEE, 2001: 145-152
## 代码

```c++
#include<iostream>
#include<pcl/io/pcd_io.h>
#include<pcl/point_types.h>
#include<pcl/registration/icp.h>
#include <pcl/visualization/pcl_visualizer.h>//可视化头文件
typedef pcl::PointXYZ PointT;
typedef pcl::PointCloud<PointT> PointCloudT;
bool next_iteration = false;
void print4x4Matrix(const Eigen::Matrix4d & matrix)//打印旋转矩阵和平移矩阵
{
	printf("Rotation matrix :\n");
	printf("    | %6.3f %6.3f %6.3f | \n", matrix(0, 0), matrix(0, 1), matrix(0, 2));
	printf("R = | %6.3f %6.3f %6.3f | \n", matrix(1, 0), matrix(1, 1), matrix(1, 2));
	printf("    | %6.3f %6.3f %6.3f | \n", matrix(2, 0), matrix(2, 1), matrix(2, 2));
	printf("Translation vector :\n");
	printf("t = < %6.3f, %6.3f, %6.3f >\n\n", matrix(0, 3), matrix(1, 3), matrix(2, 3));
}

void KeyboardEventOccurred(const pcl::visualization::KeyboardEvent& event,void* nothing)
{  //使用空格键来增加迭代次数，并更新显示
	if (event.getKeySym() == "space" && event.keyDown())
		next_iteration = true;
}


int main(int argc, char** argv)
{
	// 创建点云指针
	PointCloudT::Ptr cloud_in(new PointCloudT);  // 原始点云
	PointCloudT::Ptr cloud_tr(new PointCloudT);  // 转换后的点云
	PointCloudT::Ptr cloud_icp(new PointCloudT);  // ICP 输出点云

	//读取pcd文件
	if (pcl::io::loadPCDFile<pcl::PointXYZ>("you.pcd", *cloud_in) == -1)
	{
		PCL_ERROR("Couldn't read file1 \n");
		return (-1);
	}
	std::cout << "Loaded " << cloud_in->size() << " data points from file1" << std::endl;

	if (pcl::io::loadPCDFile<pcl::PointXYZ>("zuo.pcd", *cloud_icp) == -1)
	{
		PCL_ERROR("Couldn't read file2 \n");
		return (-1);
	}
	std::cout << "Loaded " << cloud_icp->size() << " data points from file2" << std::endl;


	int iterations = 1;  // 默认的ICP迭代次数


	//定义旋转矩阵和平移向量Matrix4d是为4*4的矩阵
	Eigen::Matrix4d transformation_matrix = Eigen::Matrix4d::Identity();  


	//初始化
	double theta = M_PI / 8;  // 旋转的角度用弧度的表示方法
	transformation_matrix(0, 0) = cos(theta);
	transformation_matrix(0, 1) = -sin(theta);
	transformation_matrix(1, 0) = sin(theta);
	transformation_matrix(1, 1) = cos(theta);

	// Z轴的平移向量 (0.4 meters)
	transformation_matrix(2, 3) = 0.4;

	//打印转换矩阵
	std::cout << "Applying this rigid transformation to: cloud_in -> cloud_icp" << std::endl;
	print4x4Matrix(transformation_matrix);

	// 执行点云转换
	pcl::transformPointCloud(*cloud_in, *cloud_icp, transformation_matrix);
	*cloud_tr = *cloud_icp;  // 备份cloud_icp赋值给cloud_tr为后期使用

	//icp配准
	pcl::IterativeClosestPoint<pcl::PointXYZ, pcl::PointXYZ> icp; //创建ICP对象，用于ICP配准
	icp.setMaximumIterations(iterations);    //设置最大迭代次数iterations=true
	icp.setInputCloud(cloud_icp); //设置输入点云
	icp.setInputTarget(cloud_in); //设置目标点云（输入点云进行仿射变换，得到目标点云）
	icp.align(*cloud_icp);          //匹配后源点云
	icp.setMaximumIterations(1);  // 设置为1以便下次调用
	std::cout << "Applied " << iterations << " ICP iteration(s)"<< std::endl;
	if (icp.hasConverged())//icp.hasConverged ()=1（true）输出变换矩阵的适合性评估
	{
		std::cout << "\nICP has converged, score is " << icp.getFitnessScore() << std::endl;
		std::cout << "\nICP transformation " << iterations << " : cloud_icp -> cloud_in" << std::endl;
		transformation_matrix = icp.getFinalTransformation().cast<double>();
		print4x4Matrix(transformation_matrix);
	}
	else
	{
		PCL_ERROR("\nICP has not converged.\n");
		return (-1);
	}


	//可视化
	pcl::visualization::PCLVisualizer viewer("ICP demo");
	// 创建两个观察视点
	int v1(0);
	int v2(1);
	viewer.createViewPort(0.0, 0.0, 0.5, 1.0, v1);
	viewer.createViewPort(0.5, 0.0, 1.0, 1.0, v2);
	// 定义显示的颜色信息
	float bckgr_gray_level = 0.0;  // Black
	float txt_gray_lvl = 1.0 - bckgr_gray_level;
	// 原始的点云设置为白色的
	pcl::visualization::PointCloudColorHandlerCustom<PointT> cloud_in_color_h(cloud_in, (int)255 * txt_gray_lvl, (int)255 * txt_gray_lvl,
		(int)255 * txt_gray_lvl);

	viewer.addPointCloud(cloud_in, cloud_in_color_h, "cloud_in_v1", v1);//设置原始的点云都是显示为白色
	viewer.addPointCloud(cloud_in, cloud_in_color_h, "cloud_in_v2", v2);

	// 转换后的点云显示为绿色
	pcl::visualization::PointCloudColorHandlerCustom<PointT> cloud_tr_color_h(cloud_tr, 20, 180, 20);
	viewer.addPointCloud(cloud_tr, cloud_tr_color_h, "cloud_tr_v1", v1);

	// ICP配准后的点云为红色
	pcl::visualization::PointCloudColorHandlerCustom<PointT> cloud_icp_color_h(cloud_icp, 180, 20, 20);
	viewer.addPointCloud(cloud_icp, cloud_icp_color_h, "cloud_icp_v2", v2);

	// 加入文本的描述在各自的视口界面
	//在指定视口viewport=v1添加字符串“white 。。。”其中"icp_info_1"是添加字符串的ID标志，（10，15）为坐标16为字符大小 后面分别是RGB值
	viewer.addText("White: Original point cloud\nGreen: Matrix transformed point cloud", 10, 15, 16, txt_gray_lvl, txt_gray_lvl, txt_gray_lvl, "icp_info_1", v1);
	viewer.addText("White: Original point cloud\nRed: ICP aligned point cloud", 10, 15, 16, txt_gray_lvl, txt_gray_lvl, txt_gray_lvl, "icp_info_2", v2);

	std::stringstream ss;
	ss << iterations;            //输入的迭代的次数
	std::string iterations_cnt = "ICP iterations = " + ss.str();
	viewer.addText(iterations_cnt, 10, 60, 16, txt_gray_lvl, txt_gray_lvl, txt_gray_lvl, "iterations_cnt", v2);

	// 设置背景颜色
	viewer.setBackgroundColor(bckgr_gray_level, bckgr_gray_level, bckgr_gray_level, v1);
	viewer.setBackgroundColor(bckgr_gray_level, bckgr_gray_level, bckgr_gray_level, v2);

	// 设置相机的坐标和方向
	viewer.setCameraPosition(-3.68332, 2.94092, 5.71266, 0.289847, 0.921947, -0.256907, 0);
	viewer.setSize(1280, 1024);  // 可视化窗口的大小

	// 注册按键回调函数
	viewer.registerKeyboardCallback(&KeyboardEventOccurred, (void*)NULL);

	//显示
	while (!viewer.wasStopped())
	{
		viewer.spinOnce();

		//按下空格键的函数
		if (next_iteration)
		{
			// 最近点迭代算法
			icp.align(*cloud_icp);
			if (icp.hasConverged())
			{
				printf("\033[11A");  // Go up 11 lines in terminal output.
				printf("\nICP has converged, score is %+.0e\n", icp.getFitnessScore());
				std::cout << "\nICP transformation " << ++iterations << " : cloud_icp -> cloud_in" << std::endl;
				transformation_matrix *= icp.getFinalTransformation().cast<double>();  // WARNING /!\ This is not accurate!
				print4x4Matrix(transformation_matrix);  // 打印矩阵变换

				ss.str("");
				ss << iterations;
				std::string iterations_cnt = "ICP iterations = " + ss.str();
				viewer.updateText(iterations_cnt, 10, 60, 16, txt_gray_lvl, txt_gray_lvl, txt_gray_lvl, "iterations_cnt");
				viewer.updatePointCloud(cloud_icp, cloud_icp_color_h, "cloud_icp_v2");
			}
			else
			{
				PCL_ERROR("\nICP has not converged.\n");
				return (-1);
			}
		}
		next_iteration = false;
	}


	return 0;
}

```
