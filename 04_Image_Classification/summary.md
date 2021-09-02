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

  여성 데이터가 많음

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
>  - albumentations이 torchvision보다 더 빠르고 다양한 data augmentation 제공
>  - 객체 탐지 모델에서 이미지 데이터에 그려져 있는 b-box도 같이 transform해주기도 함

**수정 중...** 

1. Mask

   Perspective, Rotate, Noise, Color, Blur

2. Age, Gender

   Perspective, Rotate, Noise, Color, Grid

3. ALL

   Resize(224,224), Normalize()

- Seed: 777 고정

- K-fold 교차검증 사용

> TTA (??)

## :four: Testing

#### Test Required

- efficientNet, b3, 2 테스트
- KFold 4, 6 테스트
- lr 테스트 테스트
- batch_size 테스트
- epoch 테스트

---

- 5_Fold 중 1개의 Fold를 선택하여 Train Set, Valid set 분할
- Optimizer: Adam
- loss: CrossEntropyLoss
- lr_scheduler: ReduceLROnPlateau

> mask, age, gender(batch_size, lr, epoch)

| efficientNet | mask                | age                 | gender              | MY f1-score        | result (accuracy / f1) | etc                                                          |
| :----------: | ------------------- | ------------------- | ------------------- | ------------------ | ---------------------- | ------------------------------------------------------------ |
|      b4      | 32, 3e-4, 10        | 32, 1e-4, 10        | 32, 3e-4, 10        |                    | 75.905 / 0.6947        | loss가 너무 커서 바꿔서 시작                                 |
|      b4      | 32, 3e-4, 10        | 32, 1e-4, 10        | 32, 3e-4, 10        |                    | **78.206 / 0.7263**    | +) crop, mtcnn 적용, 적용 안되면 직접 crop [100:400, 50:350, :] // 이후 crop 적용 |
|      b4      | 32, 3e-4, 10        | 32, 1e-4, 10        | 32, 3e-4, 10        |                    | **78.206 / 0.726**     |                                                              |
|      b4      | 32, 3e-4, 10        | 16, 1e-4, 10        | 32, 3e-4, 10        |                    | 73.2857 / 0.6087       |                                                              |
|      b4      | 32, 3e-4, 10        | **64, 1e-4, 10**    | 32, 3e-4, 10        |                    | **79.6825 / 0.7461**   | 이후 age, batch_size 적용                                    |
|      b4      | 64, 3e-4, 10        | 64, 1e-4, 10        | 32, 3e-4, 10        |                    | 79.2381 / 0.7355       |                                                              |
|      b4      | 64, 1e-4, 10        | 64, 1e-4, 10        | 32, 3e-4, 10        |                    | 79.2857 / 0.7366       |                                                              |
|      b4      | 32, 1e-4, 10        | 64, 1e-4, 10        | 32, 3e-4, 10        |                    | 79.2381 / 0.7354       |                                                              |
|      b4      | 32, 3e-4, 10        | 64, 1e-4, 10        | 64, 1e-4, 10        |                    | 78.9683 / 0.7311       |                                                              |
|      b4      | 32, 3e-4, 10        | **64, 1e-4, 15**    | 32, 3e-4, 10        | 0.9973389355742298 | **80.5079 / 0.7527**   | 이후 age, epoch 적용                                         |
|      b4      | 32, 3e-4, 10        | 64, 1e-4, 19        | 32, 3e-4, 10        | 0.9984593837535015 | 77.3492 / 0.7036       |                                                              |
|      b4      | 32, 3e-4, 6         | 64, 1e-4, 13        | 32, 3e-4, 6         | 0.9983193277310926 | 77.3333 / 0.7057       | +) Dataloader: shuffle=False, sampler=RandomSampler(data) / Focal loss(age) // 일단 둘 다 이후 적용 |
|      b3      | 64, 3e-4, 9         | 64, 1e-4, 4         | 64, 1e-4, 8         | 0.9984593837535015 | 76.0635 / 0.6804       | +) b4에서 가벼운 b3로 변경, batch_size, lr 수정 / gender 수정 / 논문(상하님)에 맞춰서 Resize(312, 312) 적용 // 모두 이후 적용 |
|    **b3**    | 64, 3e-4, 10        | **64, 1e-4, 4 * 5** | 64, 1e-4, 10        | 0.9985994397759105 | **81.1905 / 0.7535**   | +) crop 30에서 15 간격 줄임 / age  불균형 심해서 59세를 60세에 넣어봄 // 모두 이후 적용 |
|      b3      | **64, 1e-4, 2 * 5** | 64, 1e-4, 4 * 5     | 64, 1e-4, 7 * 5     | 0.9987394957983194 | **81.5714 / 0.7586**   | 이후 mask, epoch 적용                                        |
|      b3      | 64, 1e-4, 2 * 5     | 64, 1e-4, 4 * 5     | 64, 1e-4, 7 * 5     | 0.9952380952380955 |                        | 사람별로 돌리기 / overfitting 확인 용이 / 같은 사람 여러 장, 결국 데이터 수  적음, 확일화되어서 overfitting이 더 쉽게 일어남 |
|      b3      | 64, 1e-4, 2 * 5     | 64, 1e-4, 4 * 5     | **64, 1e-4, 4 * 5** | 0.9987394957983194 | **81.7619 / 0.7612**   |                                                              |
|      b3      | 64, 1e-4, 2 * 5     | 64, 1e-4, 4 * 5     | 64, 1e-4, 4 * 5     | 0.9784313725490206 |                        | age 58 // age 59 유지                                        |
|      b3      | 64, 1e-4, 2 * 5     | 64, 1e-4, 9 * 5     | 64, 1e-4, 4 * 5     | 0.9997198879551821 | 81.5079 / 0.7511       |                                                              |
|      b3      | 64, 1e-4, 2 * 5     | 64, 1e-4, 4 * 5     | 64, 1e-4, 4 * 5     | 0.9984593837535015 | **81.6190 / 0.7627**   | valid[4]로 통일                                              |





f1-score로 save file

weight

label smoothing

dropout

cutmix facenet => facenet이 나음



stacking 앙상블

wandb sweep

멀티레이블

멀티클래스



cutmix 77.6508 / 0.7040



efficientNet-b3

5-Kfold label별 (사람별 성능 저하, overfitting 확인은 쉬움)

crop: facenet, 15 (cutmix 성능 저하)

Resize: 312, 312

Optimizer: Adam

lr_scheduler: ReduceLROnPlateau

age: 59, 60 한 세트로 묶음 (58, 59, 60은 성능 저하)

> loss, batch_size, lr, epoch

mask: CrossEntropyLoss, 64, 1e-4, 2 * 5

age: FocalLoss, 64, 1e-4, 4 * 5

gender: CrossEntropyLoss, 64, 1e-4, 4 * 5



