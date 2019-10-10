1 用Azure AI 服务，预训练网络，将模型转成ONNX格式

https://elbruno.com/2018/06/28/winml-how-to-create-a-windows10-app-using-yolo-for-object-detection/

 

2 本地服务器处理， photocapture, 压缩图像为jpge，利用UDP传输图像，

MixedReality-Toolkit进行可视化。https://www.youtube.com/watch?v=PVQw79w0ss0 2018-yolov2

 

3 本地服务器，开一个线程获取HoloLens摄像头帧，

（没有使用普通的Unity“可定位摄像机”或所谓的“可定位摄像机”，而是使用Windows Universal API中的MediaCapture），然后发送到服务器，进行2D检测并将其投影到空间映射网格上。只需要确保获得正确的投影和相机矩阵，即可将2D边界框角正确地放入世界空间。，HoloLens API获得将屏幕点转换为世界空间的矩阵。

 

4 完整的利用ONNX格式模型在hololens上进行目标检测的例子，这种在云中训练模型并在本地执行模型的方法为混合现实设备提供了现有的非常有用的新功能。

http://dotnetbyexample.blogspot.com/2019/01/labeling-toy-aircraft-in-3d-space-using.html

 

https://github.com/LocalJoost/ToyAircraftFinder/tree/WinML

5 一个日本人用Windows ML API 做的，基于ONNXhttps://qiita.com/miyaura/items/f270d0c593a649faafce

 

6 利用第三方sdk，vuforia等https://blogs.windows.com/devices/2016/11/17/innovating-with-microsoft-hololens-vuforia-sdk-is-here/

 

7 Windows ML， Azure云端训练模型（选择模型，上传数据，训练，下载一气呵成），Hololens执行，https://blogs.msdn.microsoft.com/appconsult/2018/11/15/23535/

 

 

总结：

1 在16-17较多用的是自己在服务器端做识别，或借助vuforia等sdk。

2 18年之后多用onxx格式的模型，利用windows ml api做识别，微软在推广ONXX格式及其机器学习的平台。