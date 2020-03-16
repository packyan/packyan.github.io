

# ICCV 2019 文献读后感

找能**解决问题**的文献。

 

 

直接处理点云的网络（除开 体素化、多视图 ）基本分为两派：

延续pointnet的变体：

Pointnet++、 shellNet、 DGCNN、 geometry sharing net 等等

 

基于稀疏卷积的：

2019_ICCV Oral_Interpolated Convolutional Networks for 3D Point Cloud Understanding

2019_ICCV_KPConv: Flexible and Deformable Convolution for Point Clouds

 

目前Pointnet及其变体，工作方向主要有：

 

建图：Geometry sharing net， 提出patch特征值图，eigen Graph，

 

特征聚合：shellNet，多层壳卷积

 

点云采样：2019_ICCV_Dynamic Points Agglomeration for Hierarchical Point Sets Learning

 

上下文特征：2019_ICCV_Hierarchical Point-Edge Interaction Network for Point Cloud Semantic Segmentation ： 提出利用上一层的信息，对边特征进行差值。

 

 