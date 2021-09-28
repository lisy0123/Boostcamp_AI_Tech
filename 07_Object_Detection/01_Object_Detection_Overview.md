# Object Detection Overview

<img src="https://user-images.githubusercontent.com/60209937/134850076-05e79e30-1477-40b9-835a-8c179bfa5f01.png" alt="3" style="zoom:50%;" />

### Evaluation

**성능**

- Confusion matrix

  <img src="https://user-images.githubusercontent.com/60209937/134849619-ba9a897d-8327-44df-8940-eaeb7f3ddf8a.png" alt="1" style="zoom:60%;" />

- Precision

  <img src="https://user-images.githubusercontent.com/60209937/134849703-05b3bc65-3f3e-4252-83e7-d6f0eedafa9f.png" alt="2" style="zoom:33%;" />

- Recall

  <img src="https://user-images.githubusercontent.com/60209937/134850373-0aebdeee-e6ab-4e32-bc11-4315322f4e9b.png" alt="4" style="zoom:33%;" />

- PR Curve

  <img src="https://user-images.githubusercontent.com/60209937/134850987-560cc6d9-77ba-4dbf-835f-45364b3f900f.png" alt="1" style="zoom:40%;" /> <img src="https://user-images.githubusercontent.com/60209937/134851004-08f5d082-ecbb-4f08-b0c0-bacdcf1e009d.png" alt="2" style="zoom:40%;" />

- AP

  <img src="https://user-images.githubusercontent.com/60209937/134851006-fc03fa1b-0264-48c7-be0e-a2a59045b193.png" alt="3" style="zoom:50%;" />

- **mAP (mean Average Precision)**

<img src="https://user-images.githubusercontent.com/60209937/134851159-78a720cd-889c-40e2-86d2-c5bc26bbdfdf.png" alt="7" style="zoom: 40%;" />

- IOU (Intersection Over Union)

  <img src="https://user-images.githubusercontent.com/60209937/134851165-999cc696-d2a6-455f-ae6d-d095cdb3c24d.png" alt="8" style="zoom:40%;" />

**속도**

- FPS (Frames Per Second)

- FLOPs (Floating Point Operations)

  Model이 얼마나 빠르게 동작하는 측정하는 metric

  연산량 횟수 (곱하기, 더하기, 빼기, 등)

  작으면 작을수록 연산 적은 모델


### Libraray

- MMDetection

  pytorch기반인 object detection 오픈소스

- Detectron2

  페이스북 AI 리서치의 라이브러리, pytorch 기반의 Object detection과 segmentation의 알고리즘을 제공

- YOLOv5

  coco 데이터셋으로 사전 학습된 모델로 수천 시간의 연구와 개발에 걸쳐 발전된 Object Detection 모델

  Colab, kaggle, Docker, AWS, Google Cloud Platform 등 에서 오픈 소스 제공

- EfficientDet

  Google Research & Brain에서 연구한 모델로 EfficientNet을 응용해 만든 Object Detection 모델

  Tensorflow로 제공되는 EfficientDet을 사용 가능

  github에 pytorch 기반으로 구현된 라이브러리 역시 존재
