# 人脸识别

## 概念

流程: face detection -> face alignment -> face verification -> face identification




- 人脸检测/跟踪 /定位 face detection

  在图像/视频中找到各个人脸所在的位置和大小, 框出来；对于跟踪而言，还需要确定帧间不同人脸间的对应关系

  在OpenCV中有直接能拿出来用的Harr分类器

- 人脸特征点定位 

  确定脸部特征点（眼睛、嘴巴中心点、眼睛、嘴巴轮廓特征点、器官轮廓特征点等）的位置。人脸特征点定位的基本思路，主要是将人脸局部器官的纹理特征和器官特征点之间的位置约束进行结合来进行处理

- 人脸校准（face alignment）

  检测出人脸特征点后, 进行姿态的校正，使其人脸尽可能的”正”, 可以提高人脸识别的精度

- 人脸确认（face verification）

  pair matching: 比对输入的人脸和 已有的人脸是否匹配

- 人脸鉴别（face identification/recognition）

  对进行人脸检测、人脸校正后的图像（人脸）进行分类(face grouping)


## 活体检测

一般有 眨眼检测, 嘴巴张/闭检测

- 眨眼检测方法

  抓取摄像头传来的连续帧, 人脸特征点定位,某个时刻为睁眼, 下一帧为闭眼, 接着为睁眼, 视为一次眨眼

  或者可以考虑将这个问题当做图像分类问题，类别为：睁眼和闭眼。先通过人脸检测模型，比如dlib、face_recognition，从人脸照片中裁剪出眼部图片，再对眼部图片进行分类。


## 开源方案

- https://github.com/ageitgey/face_recognition  基于 dlib
  - dlib,opencv (https://zhuanlan.zhihu.com/p/79784400) dlib 19.7.0 和face_recognition 1.2.1 ; https://zhuanlan.zhihu.com/p/45827914
- https://github.com/deepinsight/insightface MXNet and PyTorch
- https://github.com/davidsandberg/facenet Tensorflow
- https://github.com/cmusatyalab/openface Torch
- https://github.com/justadudewhohacks js 库

## 如何存储


https://shentuzhigang.blog.csdn.net/article/details/103793031
https://blog.csdn.net/xia4215/article/details/113888148

## ref

https://www.cnblogs.com/GarfieldEr007/p/5372345.html
https://blog.csdn.net/u014696921/article/details/74779798
https://www.face-rec.org/general-info/
https://www.zhihu.com/question/64860792/answer/233782977

基础库
https://github.com/opencv/opencv
https://github.com/opencv/opencv_contrib
http://dlib.net/


https://ai.arcsoft.com.cn/course/index.html 公开课

https://github.com/ChanChiChoi/awesome-Face_Recognition#face-application 论文集


## 代码实现

https://blog.csdn.net/libo1004/article/details/110874855 base64 图片 api

https://blog.csdn.net/weixin_43796192/article/details/90111437 图片传输 协议选择 
https://blog.csdn.net/ymaini/article/details/80645917
https://blog.csdn.net/sinat_29552923/article/details/70833455