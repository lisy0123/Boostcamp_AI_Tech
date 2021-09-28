# R-CNN

Sliding Window 사용 안함

Selective Search 이미지를 무수히 많이 나눴다가 점차 통합하는 방식

### Pipeline

1. 입력 이미지 받기

2. Selective Search를 통해 약 2000개의 Rol(Region of Interest) 추출하기

3. Rol의 크기를 조절해서 모두 동일한 사이즈로 변형 (Wraping)

   - CNN의 마지막, FC layer의 입력 사이즈 고정이므로, 위 과정 수행

4. Rol를 CNN에 넣어, feature 추출

   - 각 region마다 4096-dim feature vector 추출 (2000X4096)
   - Pretrained AlexNet 구조 활용
     - AlexNet 마지막에 FC layer 추가
     - 필요에 따라 Finetuning 진행

5. CNN을 통해 나온 feature를 SVM에 넣어 분류 진행

   - Input: 2000X4096 features
   - Output
     - Class (C+1) + Condifence scores
     - 클래스 개수(C개) + 배경 여부 (1개)

6. CNN을 통해 나온 feature를 regression을 통해 bounding box 예측

   <img width="413" alt="image8" src="https://user-images.githubusercontent.com/60209937/135014086-0de0f95c-eda9-4883-bdda-7a4fa1fd5a7b.png" style="zoom:60%;" >

### Training

AlexNet

- Domain specific finetuning
- Dataset 구성
  - IoU > 0.5: positive samples / IoU < 0.5: negative samples
  - Positive samples 32, negative samples 96

Linear SVM

- Dataset 구성

  Ground truth: positive samples / IoU < 0.3: negative samples

  Positive samples 32, negative samples 96

- Hard negative mining

  Hard negative: False positive

  배경으로 식별하기 어려운 샘플들을 강제로 다음 배치의 negative sample로 mining하는 방법

 Bbox regressor

- Dataset 구성

  IoU > 0.6: positive samples

- Loss function: MSE Loss

### Shortcomings

- 2000개의 Region 각각 CNN 통과
- 강제 Warping, 성능 하락 가능성
- CNN, SVM classifier, bounding box regressor, 따로 학습
- End-to-End X

---

# SPPNet

<img src="https://user-images.githubusercontent.com/60209937/135014070-b678e91b-47d2-43ae-b0e2-b4e684dfacb7.png" alt="image1" style="zoom:80%;" />

![image3](https://user-images.githubusercontent.com/60209937/135014075-05ed468d-d7a9-485f-b1b5-ef09fafa17bc.png)

위 사진: R-CNN / SPPNet

### Spatial Pyramid Pooling Layer

<img src="https://user-images.githubusercontent.com/60209937/135014073-cd5b4f71-f814-4442-8a2f-62f38030ed70.png" alt="image2" style="zoom:80%;" />

Rol 사이즈에 상관없이 고정된 feature size 뽑아냄

**=> 2000개의 Region 각각 CNN 통과 : 해결**

**=> 강제 Warping, 성능 하락 가능성 : 해결**

---

# Fast R-CNN

<img src="https://user-images.githubusercontent.com/60209937/135014079-9a44a462-cd4a-4e32-bbbb-7758078c44e3.png" alt="image4" style="zoom:80%;" />

### Pipeline

1. 이미지 CNN에 넣어 feature 추출 (CNN 한 번만 사용)

   - VGG16 사용

2. Rol Projection 통해 feature map 상에서 Rol 계산

   - Rol Projection

     <img width="922" alt="image9" src="https://user-images.githubusercontent.com/60209937/135014089-6c6975b0-fce8-4a14-9ad8-37059bbfbe3a.png" style="zoom:60%;" >

3. Rol Pooling 통해 일정한 크기의 feature 추출

   - 고정된 vector 얻기 위한 과정

   - SPP 사용

     - pyramid level: 1
     - Target grid size: 7X7

     <img src="https://user-images.githubusercontent.com/60209937/135014081-10719934-7402-445d-986d-2a303eb498ab.png" alt="image5" style="zoom:80%;" />

     Spatial Pyramid Pooling와 동일

4. Fully connected layer 이후, Softmax Classifier과 Bounding Box Regressor

   - 클래스 개수(C개) + 배경 여부 (1개)

### Training

multi task loss 사용

- (classification loss + bounding box regression)

Loss function

- Classification : Cross entropy

- BB regressor : Smooth L1

   <img src="https://user-images.githubusercontent.com/60209937/135014084-c4b097ac-5f03-4c87-a86a-9bc2ce846440.png" alt="image6" style="zoom:70%;" />

Dataset 구성

- IoU > 0.5: positive samples / 0.1 < IoU < 0.5: negative samples
- Positive samples 25%, negative samples 75%

Hierarchical sampling

- R-CNN의 경우 이미지에 존재하는 RoI를 전부 저장해 사용
- 한 배치에 서로 다른 이미지의 RoI 포함
- Fast R-CNN의 경우 한 배치에 한 이미지의 RoI만 포함
- 한 배치 안에서 연산과 메모리를 공유 가능

**=> CNN, SVM classifier, bounding box regressor, 따로 학습 : 해결**

---

# Faster R-CNN

<img src="https://user-images.githubusercontent.com/60209937/135014085-70368401-330f-4b08-8510-b370038a469e.png" alt="image7" style="zoom:80%;" />

### Pipeline

1. 이미지를 CNN에 넣어 feature maps 추출 (CNN을 한 번만 사용)

2. RPN을 통해 RoI 계산

   - 기존의 selective search 대체

   - Anchor box 개념 사용

     <img width="387" alt="image10" src="https://user-images.githubusercontent.com/60209937/135014092-831128be-dadf-4870-bfa8-cbd72665fab4.png" style="zoom:60%;" >

3. Region Proposal Network (RPN)

   <img width="1068" alt="image11" src="https://user-images.githubusercontent.com/60209937/135014097-61ffc0fb-bcce-430b-b120-b165e3f9b1c5.png" style="zoom:67%;" >

   1. CNN에서 나온 feature map을 input으로 받음. (𝐻: 세로, 𝑊: 가로, 𝐶: 채널)

   2. 3x3 conv 수행하여 intermediate layer 생성

   3. 1x1 conv 수행하여 binary classification 수행

      - 2 (object or not) x 9 (num of anchors) 채널 생성
      - 4 (bounding box) x9 (num of anchors) 채널 생성

      <img width="769" alt="image12" src="https://user-images.githubusercontent.com/60209937/135014099-77ceb708-4409-4305-a39b-2ac3aee983b7.png" style="zoom:60%;" >

<img width="1061" alt="image13" src="https://user-images.githubusercontent.com/60209937/135014100-8324da68-8aa0-4b89-bd0d-24bbf30e18ec.png" style="zoom:60%;" >

NMS

- 유사한 RPN Proposals 제거하기 위해 사용
- Class score 기준, proposals 분류
- IoU가 0.7 이상인 proposals 영역: 중복된 영역으로 판단한 뒤 제거

### Training

RPN

- RPN 단계에서 classification과 regressor학습을 위해 앵커박스를 positive/negative samples 구분
- 데이터셋 구성
  - IoU > 0.7 or highest IoU with GT: positive samples / IoU < 0.3: negative samples
  - Otherwise : 학습데이터로 사용 X

- Loss 함수

  <img width="527" alt="image14" src="https://user-images.githubusercontent.com/60209937/135014101-eea7841e-640c-4365-8fd8-219e4f1ddfb2.png" style="zoom:50%;" >

Region proposal 이후 Fast RCNN 학습을 위해 positive/negative samples로 구분

- 데이터셋 구성

  - IoU > 0.5: positive samples: 32개

  - IoU < 0.5: negative samples: 96개

    => 128개 samples로 mini-bath 구성

- Loss함수: Fast RCNN과 동일

- RPN과 Fast RCNN 학습을 위해 4 steps alternative training 활용

  1. Imagenet pretrained backbone load + RPN 학습
  2. Imagenet pretrained backbone load + RPN from step 1 + Fast RCNN 학습
  3. Step 2 finetuned backbone load & freeze + RPN 학습
  4. Step 2 finetuned backbone load & freeze + RPN from step 3 + Fast RCNN 학습

- 학습 과정이 복잡

  => 최근에는 Approximate Joint Training 활용

**=> End-to-End X : 해결**

<img width="999" alt="image15" src="https://user-images.githubusercontent.com/60209937/135014104-f9487ace-6f9b-4318-aeca-1547f6c886f9.png" style="zoom:60%;" >

여전히 속도가 느림, realtime에 도입 어려움

---

# Summary

|                 |      R-CNN       |    Fast R_CNN    |           Faster R_CNN           |
| :-------------: | :--------------: | :--------------: | :------------------------------: |
| Classification  |       SVMs       |    **Linear**    |              Linear              |
|     Resize      |       Warp       | **Rol Pooling**  |           Rol Pooling            |
| Region Proposal | Selective Search | Selective Search | **Region Proposal Network(RPN)** |
|   End-to-End    |        X         |        X         |              **O**               |

