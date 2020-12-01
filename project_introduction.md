## 标定项目情况介绍
需要交付的项目：
标定分为产线和售后标定，产线标定在墙上固定靶标的方式标定，售后是通过布置可移动靶标的方式标定，布置精度低于产线。产线标定是一次性标定4个相机，售后是修完哪个标哪个，所以产线的四个相机视野内总会有靶标出现，售后只有要标定的相机视野内才会出现靶标

### TODO list
+ 配置参数读取开发
开发阶段是通过json文件的方式设置标定相关参数，量产阶段是通过把相关参数烧录到IECU里面，所以需要开发一个函数去读取IECU配置参数，以及了解如何把标定结果存储到IECU里面，这些需要找软工李智强协商。读取IECU配置参数可以在 https://gitlab.deepmotion.ai/SAIC_H2_CALIBRATION/SAIC_CALIBRATION/blob/master/include/calibrator/calibrator.cpp#L108上面修改
+ 从相机固件中读取相机内参数据
量产阶段的相机参数都是烧录到相机固件中，所以需要开发一个函数能够读取相机固件参数，这需要欧菲那边准备好，能拿到读取示例后，开发，然后集成近标定工程中
+ 诊断仪流程开发
量产标定是通过一个诊断仪向IECU发送命令，然后启动标定，如果中途失败发送对应的诊断码，用于排查问题，所以需要有对应的诊断仪响应流程开发，接收诊断仪指令及附带参数，实现特定的功能，并能够根据故障发送对应的故障码，一些功能列表如下
	+ 接收诊断仪启动标定指令，启动标定程序
	+ 接收诊断仪查询指令，反馈标定是否完成
	+ 返回标定完成状态，如果中间出现故障，导致标定失败，则需要返回对应的故障码及相机编号
+ 车量状态判断开发
根据can信号接口读取车辆状态，确定车门、后视镜、后备箱是否关闭，胎压是否正常
+ H2样车开发调试过程中的内外参标定，约车、内参标定、车辆定位相关的可以找杨建松协助
+ 算法验证 (安亭、临港)
+ devision model开发调试
开发阶段相机使用的是针孔成像模型，但最终量产阶段相机成像模型是使用devision model，需要在能拿到devision model参数后，调试，确定是否正确使用，目前只是写了，但没有参数验证

### 上汽还未给我们的接口(找于跃龙（于博）或者李智强）
1，量产阶段从IECU读取配置参数（内参，相机安装，靶标位置）的接口。

2, 诊断仪启动标定程序的接口，诊断仪查询标定状态以及反馈故障代码的接口，
3，can信号接口（读取车辆状态，车门，后视镜，后备箱是否关闭，胎压是否正常）

### 标定方式
目前SAIC_CALIBRATION工程中包含了三种标定方式，分别是读相机标定，读离线图像标定，读2d-3d匹配点对txt文件标定

## 不需要交付，暂时支持的项目
安亭一楼通道测试车标定：
   the camera resolution of H2 is 1280*960

	一楼通道采数据，楼上处理数据
   1.将车开到一楼通道特定位置，前轮中心压到前轮定位线（红线），四轮中心压白线中心（详见，./DOCUMENTS/calibration_place_params/anting/安亭环视标定间参数.png)
   
   2, 开始标定外参（saic_calibration project)，并记录相机是否模糊，相机安装位置是否偏离，相机顺序是否正常等特殊情况。

   3，据棋盘格，录内参标定图像(dm_logger),

   4,采完数据，上楼离线标定

## Project
	内参标定
		https://gitlab.deepmotion.ai/SAIC_H2_CALIBRATION/INTRINSICS_METHOD.git
		git checkout huikang（huikang branch）

	外参标定
		https://gitlab.deepmotion.ai/SAIC_H2_CALIBRATION/SAIC_CALIBRATION.git
		git checkout huikang（huikang branch）
	
	安亭通道标定集成python与shell脚本
		https://gitlab.deepmotion.ai/HuikangZhang/AntingCalibrationIntegration.git
	
	doucument:
		https://gitlab.deepmotion.ai/SAIC_H2_CALIBRATION/DOCUMENTS.git
		git checkout huikang（huikang branch）
	数采车标定
		https://gitlab.deepmotion.ai/HuikangZhang/AntingCalibrationIntegration.git
			branch:  data_collection_car_calibration
	
	海康相机采集程序：
		https://gitlab.deepmotion.ai/HuikangZhang/Hik_camera_cv.git

	海康相机计算靶标相对位置程序：
		https://gitlab.deepmotion.ai/saic_calibration/saic_pano_calibration.git
	
		
		

