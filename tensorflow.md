使用方法
==
1.采集图片并打标签
a.使用摄像头采集数据
b.用image lab给图片上对应的目标打标签

2.生成如下的目录结构
![](/home/xh/图片/选区_001.png) 
data目录：
![](/home/xh/图片/选区_002.png) 
其中JPEGImages里放所有采集的图片
Annotations里放每个图片对应的xml
ImageSets里放训练数据集合
3.生成tfrecord
a.首先将Annotations里的xml重定向到label里的train.txt,调用label里的spli_.txt将其打乱顺序，生成result.txt,最后将其拷贝到ImageSets/Main/result.txt
b.cd models/object_dection 下执行
python create_pascal_tf_record.py \
- -data_dir=/home/ics/xiaoche_test \
- -outputpath=/home/ics/xiaoche_test/train.record \
- -label_map_path=/home/ics/xiaoche_test/data/pascal_label_rxy.pbtxt
即可生成对应的tfrecord

4.修改模型配置文件
pre_model目录：
![](/home/xh/图片/选区_003.png) 
主要修改以下的参数：
num_classes:3
 fine_tune_checkpoint: "/home/ics/xiaoche_test/pre_model/ssd_mobilenet_v1_coco_11_06_2017/model.ckpt"
  input_path: "/home/ics/xiaoche_test/data/train.record"
   label_map_path: "/home/ics/xiaoche_test/data/pascal_label_rxy.pbtxt"
   
   5.训练模型
   cd models/object_detections 目录
   python train.py   --train_dir=path/to/train_dir \
        --pipeline_config_path=pipeline_config.pbtxt
    
   6.导出模型
    ![](/home/xh/图片/选区_004.png) 
    
  7.加载模型进行预测
  cd models/object_detection目录
  python predict.py
  即可实现对视频流里面的每一帧图像进行预测
  同时还可以实现将每一帧图片生成对应的图像和.xml文件实现自动打标签的功能以便作为新的训练数据继续修正模型。
  
  
  
   
  