- 전체 사람 수: 4500명

- 한 사람당 사진 수: 7장

  - Mask

    normal:1장, incorrect: 1장, maks: 5장

- train 사람 수: 2700명

- train 사진 수: 18900장

- 이미지 크기: (384, 512)

- Evaluation: F1 Score (macro)



## EDA (Exploratory Data Analysis, 탐색적 데이터 분석)

- Age

  30, 59 비율 비슷, 60대 비율 매우 적음 => data imbalance 심함

- Gender

  여성 데이터가 많음 => data imbalance까지는 아님

- Age, base on gender

- Mask

  [5:1:1] 모두 같은 비율

  마스크 색 다름, 손수건 착용 이미지도 존재

  - normal: 마스크 없음

  - incorrect: 턱스크, 코스크, 눈스크 등 다양

- Age, base on mask

- Gender, base on mask



## Labeling

- Mask

  normal: 0, incorrect: 1, mask: 2

- Age

  <30: 0, >=30, <60: 1, >=60: 2

- Gender

  female: 0, male: 1

- data labeling 고치기

  - 000020, 004418, 005227

    Mask Labeling: 0 => 1, 1 => 0 (normal, incorrect 바뀜)

  - 006359, 006360, 006361, 006362, 006363, 006364

    Gender: 0 => 1 (female => male)

  - 4432, 1498-1

    Gender: 1 => 0 (male => female)



## Data augmentation

>  **torchvision 대신 albumentations 사용**
>
> - albumentations이 torchvision보다 더 빠르고 다양한 data augmentation 제공
> - 객체 탐지 모델에서 이미지 데이터에 그려져 있는 b-box도 같이 transform해주기도 함

- Mask

  Perspective, Rotate, Noise, Blur, Color

- Age, Gender

  Perspective, Rotate, Noise, Color, Grid

- Resize(224,224), Normalize()



TTA

Seed: 777 고정

K-fold 교차검증 사용



---

## Testing

### 1차

- 5_Fold 중 1개의 Fold를 선택하여 Train Set, Valid set 분할
- Dataloader, num_workers=6
- efficientNet, b4
- Optimizer: Adam, lr=3e-4
- 10 epoch, 32 batch_size
- crop X



#### Result

- mask: 29.435 / 29.487 // 0.001

- age: 27.810 / 23.865 // 0.029

  - lr=1e-4

    age: 28.778 / 27.650 // 0.045

- gender: 29.274 / 29.358 // 0.001

#### Modification required

- age, loss 튐, 수정 필요
- crop해서 성능 높이기
  - center crop
  - facenet 이용해서 crop

#### Test Required

- efficientNet, b3, 5 테스트
- KFold n_splits 4, 6 테스트
- Dataloader, num_workers 5, 7 테스트
- Optimizer

- lr 테스트 2e-4, 1e-4 테스트
- batch_size 64, 16 테스트
- epoch 5, 8 테스트

---

### 2차

- mtcnn 이용, crop 이미지 사용



#### Result

- mask: 29.413 / 29.425 // 0.000

- age: 27.810 / 23.865 // 0.029

  - lr=1e-4

    age: 28.778 / 27.650 // 0.045

- gender: 29.274 / 29.358 // 0.001
