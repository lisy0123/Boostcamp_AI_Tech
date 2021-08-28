- 전체 사람 수: 4500명

- 한 사람당 사진 수: 7장

  - Mask

    normal:1장, incorrect: 1장, maks: 5장

- train 사람 수: 2700명

- train 사진 수: 18900장

- 이미지 크기: (384, 512)

- Evaluation: F1 Score (macro)

> [CODE (Private)]()

## :one: EDA (Exploratory Data Analysis, 탐색적 데이터 분석)

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

## :two: Labeling

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

## :three: Data augmentation

>  **torchvision 대신 albumentations 사용**
>
> - albumentations이 torchvision보다 더 빠르고 다양한 data augmentation 제공
> - 객체 탐지 모델에서 이미지 데이터에 그려져 있는 b-box도 같이 transform해주기도 함

1. Mask

   Perspective, Rotate, Noise, Color, Blur

2. Age, Gender

   Perspective, Rotate, Noise, Color, Grid

3. ALL

   Resize(224,224), Normalize()

- Seed: 777 고정

- K-fold 교차검증 사용

> TTA (??)

---

## :four: Testing

#### Test Required

- efficientNet, b3, 5 테스트
- KFold 4, 6 테스트
- Dataloader, num_workers 5, 7 테스트
- lr 테스트 2e-4, 1e-4 테스트
- batch_size 64, 16 테스트
- epoch 5, 8 테스트

---

### 01차

- 5_Fold 중 1개의 Fold를 선택하여 Train Set, Valid set 분할
- Dataloader, num_workers=6
- efficientNet, b4
- Optimizer: Adam, lr=3e-4
  - age lr=1e-4 (loss가 너무 커서 바꿈)
- epoch=10, batch_size=32
- loss: CrossEntropyLoss
- lr_scheduler: ReduceLROnPlateau
- crop X

#### Result (accuracy / f1)

75.905 / 0.695

#### Modification required

age, loss 튐, 수정 필요

crop해서 성능 높이기

1. center crop
2. facenet 이용해서 crop

---

### 02차

- crop 이미지 사용

  mtcnn 적용, 적용 안되면 직접 crop [100:400, 50:350, :]

#### Result (accuracy / f1)

78.206 / 0.726

> crop 이미지 적용!

---

### 03차, 04차

- age, batch_size=16, 64

#### Result (accuracy / f1)

|        Age        |  batch_size=16   |   batch_size=32    |  batch_size=64   |
| :---------------: | :--------------: | :----------------: | :--------------: |
| **accuracy / f1** | 73.2857 / 0.6087 | **78.206 / 0.726** | 79.6825 / 0.7461 |

> age, batch_size=64 적용!

---

### 05차

- ALL batch_size=64

#### Result (accuracy / f1)

77.6508 / 0.7040

> 취소!

---

### 06차, 07차, 08차, 09차

- mask, gender batch_size=32, 64 / lr=3e-4, 1e-4

**Gender \ Mask, 자체 f1 score 결과**

|                            | batch_szie=32, lr=1e-4 | batch_size=32, lr=3e-4 | batch_size=64, lr=1e-4 | batch_size=64, lr=3e-4 |
| :------------------------: | :--------------------: | :--------------------: | :--------------------: | :--------------------: |
| **batch_size=32, lr=1e-4** |   0.996638655462185    |   0.996638655462185    |   0.996778711484594    |   0.996778711484594    |
| **batch_size=32, lr=3e-4** |   0.996778711484594    | **0.996778711484594**  |   0.9969187675070029   |   0.9969187675070029   |
| **batch_size=64, lr=1e-4** |   0.996638655462185    |   0.996638655462185    |   0.996778711484594    |   0.996778711484594    |
| **batch_size=64, lr=3e-4** |           -            |           -            |   0.996778711484594    |   0.996778711484594    |

#### Result (accuracy / f1)

|  회차  |             mask / gender             | accuracy |   f1   |
| :----: | :-----------------------------------: | :------: | :----: |
| **06** |  mask **64**, 3e-4 / gender 32, 3e-4  | 79.2381  | 0.7355 |
| **07** |   mask **64 1e-4** / gender 32 3e-4   | 79.2857  | 0.7366 |
| **08** |   mask 32 **1e-4** / gender 32 3e-4   | 79.2381  | 0.7354 |
| **09** | mask **64 1e-4** / gender **64 1e-4** | 78.9683  | 0.7311 |

> 취소!
>
> 그대로 mask 32, 3e-4 / age 64, 1e-4 / gender 32, 3e-4 유지!

---

### 10차, 11차

mask, epoch=3 => **Result(acc / f1): 79.2063 / 0.7350**

0.996638655462185

age, epoch=15

0.9973389355742298 => **Result(acc /f1): 80.5079 / 0.7527**

mask, 10 / age, 19 / gender, 10

0.9984593837535015 => 77.3492 / 0.7036

mask, 10 / age, 19 / gender, 12

0.9984593837535015 => 77.2698 / 0.7015

Dataloader: shuffle=False, sampler=RandomSampler(data)

mask 6 / focal loss, age 13 / gender 6

0.9983193277310926 => 77.3333 / 0.7057





#### Result (accuracy / f1)







train_sampler = RandomSampler(train_data)

valid_sampler = RandomSampler(valid_data)

​    train_dl = DataLoader(train_data, batch_size=args.batch_size,

​                          shuffle=False, sampler=train_sampler, num_workers=4)

​    valid_dl = DataLoader(valid_data, batch_size=args.batch_size,

​                          shuffle=False, sampler=train_sampler, num_workers=4)
