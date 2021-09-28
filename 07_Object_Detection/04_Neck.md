# Neck

YOLO v1, SSD, YOLO v2 다음에 FRN(Neck)

<img width="1035" alt="image1" src="https://user-images.githubusercontent.com/60209937/135028147-dab1713d-4326-4857-832a-2b7025016349.png" style="zoom:67%;" >

<img width="1026" alt="image2" src="https://user-images.githubusercontent.com/60209937/135028154-e307cbe6-f100-4066-bdee-1c83e11b3673.png" style="zoom:67%;" >

중간 feature map도 활용하자

끝 feature map만 활용 시, 작은 객체 포착 못 함

=> 다양한 feature map 활용하자

- Neck 필요성

  다양한 크기의 객체를 더 잘 탐지하기 위해서 **Neck**을 사용

  하위 level의 feature는 semantic이 약함 => 상대적으로 semantic이 강한 상위 feature와의 교환이 필요

---

# FPN (Feature Pyramid Network)

high level에서 low level로 semantic 정보 전달 필요

=> top-down path way 추가

Pyramid 구조를 통해서 high level 정보를 low level에 순차적 전달

- Low level = Early stage = Bottom
- High level = Late stage = Top

사진

사진

Lateral connections

사진

Nearest Neighbor Upsampling

사진

### Pipeline

Backbone: ResNet

사진

사진

RoI 대상이 되는 feature map 선택: 사진

### Summary

Bottom up (backbone)에서 다양한 크기의 feature map 추출

다양한 크기의 feature map의 semantic을 교환하기 위해 top-down 방식 사용

사진

**=> small box 예측에 있어 유리함**

### Code

- Build laterals

  각 feature map 마다 다른 채널을 맞춰주는 단계

- Build Top-down

  channel을 맞춘 후 top-down 형식으로 feature map 교환

- Build outputs

  최종 3x3 convolution을 통과하여 RPN으로 들어갈 feature 완성

# PANet (Path Aggregation Network)

lower level feature map이 hight level feature map에 제대로 전달 안되는 문제 해결

Bottom-up Path Augmentation (b부분 / 붉은 부분)

사진

Adaptive Feature Pooling

사진

> ROIAlign을 ROI Pooling으로 이해

### Code

- FPN

  Top-down에 3 x 3 convolution layer 통과하는 것 까지 동일

- Add bottom-up

  FPN을 통과한 후, bottom-up path 를 더해줌

- Build outputs

  이후 FPN과 마찬가지로 학습을 위해 3 x 3 convolution layer 통과

---

# DetectoRS

- Region proposal networks (RPN)
- Cascade R-CNN

=> 반복적으로 하면 좋은 성능이 나오는가에서 시작

### Recursive Feature Pyramid (RFP)

사진

Neck 정보를 backbone에 전달, backbone도 다시 학습

=> backbone 연산 증가 => 학습 속도 느림

