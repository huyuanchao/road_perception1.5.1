## Introduction

vision: A Yolov5 TensorRT + SORT implementation c++ algorithm.  GPU are supported.
lidar: 32 lines, pointpillar

## Dependencies

- Docker ros Ubuntu 18.04
- CUDA 10.2
- OpenCV 4.1.0
- TensorRT 7.2.2.3

## Engine & calibrate tabel Path



## TorchScript Model Export

Please refer to the official document here: https://github.com/ultralytics/yolov5/issues/251


## framework
```
src
├── 3rdparty           第三方库
│   └── cpptoml            配置文件toml
├── common_rs          路侧共用库
├── conf               各路口配置文件
├── drivers            驱动模块
│   ├── camera             相机驱动
│   └── lidar              激光雷达驱动
├── hadmap_engine      地图数据发布
├── model_publisher    地图rviz显示
├── perception         感知模块
│   ├── base               基础库
│   ├── camera             图像感知
│   ├── fusion             多相机融合
│   ├── lidar              激光雷达感知
│   └── pubdata            rviz显示
├── send_data          发送数据到云端
├── system             系统模块
└── tools              工具包
    └── record_cache       bag包采集
```

## build

```bash
$ sudo docker run -d --privileged --runtime=nvidia --gpus all --net=host -v /home/road_side:/home/road_side --name rs mogohub.tencentcloudcr.com/ai-qa/x86-ai-road-perception:210616
$ docker exec -it rs /bin/bash
$ cd /home/road_side
$ ./build.sh
```

## run

```bash
$ ./display.sh
$ ./perception_camera_gst.sh #图像感知与融合
$ ./perception_lidar.sh #激光雷达感知
```

## 生产环境
### build
```bash
$ cd road_perception
$ ./deploy.sh
```

### deploy
```bash
$ sudo mkdir -p /data/road_side && sudo chown -R cookoo:cookoo /data/road_side && mv /tmp/conf_*.zip /data/road_side && cd /data/road_side && unzip *.zip
$ sudo docker run -d --privileged --runtime=nvidia --gpus all --net=host --restart=always -v /data/autocar:/data/autocar -v /data/road_side/conf:/autocar-code/install/share/conf --name rs_default mogohub.tencentcloudcr.com/autocar/road:220209
```

## Demo

![17cross.png](weights/17cross.png)



## FAQ


## References

1. https://github.com/ultralytics/yolov5
2. https://github.com/walktree/libtorch-yolov3
3. https://pytorch.org/cppdocs/index.html
4. https://github.com/pytorch/vision
5. [PyTorch.org - CUDA SEMANTICS](https://pytorch.org/docs/stable/notes/cuda.html)
6. [PyTorch.org - add synchronization points](https://discuss.pytorch.org/t/why-is-the-const-time-with-fp32-and-fp16-almost-the-same-in-libtorchs-forward/45792/5)
7. [PyTorch - why first inference is slower](https://github.com/pytorch/pytorch/issues/2694)
8. https://github.com/wang-xinyu/tensorrtx/tree/master/yolov5

