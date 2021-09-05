# Image Classification

## :one: EDA (Exploratory Data Analysis, 탐색적 데이터 분석)

- 전체 사람 수: 4500명 / 한 사람당 사진 수: 7장

- train 사람 수: 2700명 / train 사진 수: 18900장

- 이미지 크기: (384, 512)

- Evaluation: F1 Score (macro)

  <img src="https://user-images.githubusercontent.com/60209937/132088440-2cee50a0-b98e-4176-9e9e-fc21e8b57931.png" alt="01" style="zoom:45%;" />

- Mask: [5:1:1] 모두 같은 비율, 마스크 색이 다 다르고, 손수건 착용, 턱스크, 코스크, 눈스크 등 다양한 사진 존재
- Age: 30, 59 비율 비슷, 60대 비율 매우 적음, data imbalance 심하다.
- Gender: 여성 데이터가 많음, data imbalance까지는 아니다.
- Insight through EDA: Age가 특히 불균형이 심해 Age 불균형 해소에 집중할 필요가 있어 보인다.


## :two: Labeling

- Mask

  normal: 0, incorrect: 1, mask: 2

- Age

  <30: 0, >=30, <60: 1, >=60: 2

- Gender

  female: 0, male: 1

- data labeling 고치기

  > 000020, 004418, 005227
  >
  > - Mask Labeling: 0 => 1, 1 => 0 (normal, incorrect 바뀜)

  > 006359, 006360, 006361, 006362, 006363, 006364
  >
  > - Gender: 0 => 1 (female => male)

  > 4432, 1498-1
  >
  > - Gender: 1 => 0 (male => female)

## :three: Data augmentation

- ALL: Perspective, Rotate(limit=20), Resize(312, 312), Normalize
- Mask: RandomBrightness, HueSaturationValue, RandomContrast
- Age: RandomGridShuffle, GaussNoise
- Gender: GaussNoise

> (color, blur, grid 관련 augmentation도 사용했었다가, overfitting으로 제외)

## :four: Testing

- Seed: 777 고정

- 모델을 mask, age, gender 3개로 나눔

- K-fold 교차검증 사용, 5-Fold 중 1개의 Fold를 선택하여 Train Set, Valid set 분할

| EfficientNet | Mask                | Age                 | Gender              | Result (accuracy / f1)      | ETC                                                          |
| :----------: | ------------------- | ------------------- | ------------------- | --------------------------- | ------------------------------------------------------------ |
|      b4      | 32, 3e-4, 10        | 32, 1e-4, 10        | 32, 3e-4, 10        | 75.905 / 0.6947             | loss가 너무 커서 바꿔서 시작                                 |
|      b4      | 32, 3e-4, 10        | 32, 1e-4, 10        | 32, 3e-4, 10        | **78.206 / 0.7263**         | +) crop, mtcnn 적용, 적용 안되면 직접 crop [100:400, 50:350, :] // 이후 crop 적용 |
|      b4      | 32, 3e-4, 10        | **64, 1e-4, 10**    | 32, 3e-4, 10        | **79.6825 / 0.7461**        | +) lr_scheduler StepLR에서 ReduceLROnPlateau로 변경 // 이후 age, batch_size 적용 |
|      b4      | 32, 3e-4, 10        | **64, 1e-4, 15**    | 32, 3e-4, 10        | **80.5079 / 0.7527**        | 이후 age, epoch 적용                                         |
|      b4      | 32, 3e-4, 6         | 64, 1e-4, 13        | 32, 3e-4, 6         | 77.3333 / 0.7057            | +) Dataloader: shuffle=False, sampler=RandomSampler(data) / Focal loss(age) // 일단 둘 다 이후 적용 |
|      b3      | 64, 3e-4, 9         | 64, 1e-4, 4         | 64, 1e-4, 8         | 76.0635 / 0.6804            | +) b4에서 가벼운 b3로 변경, batch_size, lr 수정 / gender 수정 / 논문(상하님)에 맞춰서 Resize(312, 312) 적용 // 모두 이후 적용 |
|    **b3**    | 64, 3e-4, 10        | **64, 1e-4, 4 * 5** | 64, 1e-4, 10        | **81.1905 / 0.7535**        | +) crop 30에서 15 간격 줄임 / age  불균형 심해서 59세를 60세에 넣어봄 // 모두 이후 적용 |
|      b3      | **64, 1e-4, 2 * 5** | 64, 1e-4, 4 * 5     | 64, 1e-4, 7 * 5     | **81.5714 / 0.7586**        | 이후 mask, epoch 적용                                        |
|      b3      | 64, 1e-4, 2 * 5     | 64, 1e-4, 4 * 5     | 64, 1e-4, 7 * 5     | 자체 f1-score 낮아서 시도 X | 5fold, 사람별로 돌리기 / overfitting 확인 용이 (같은 사람 여러 장, 결국 데이터 수  적음, 확일화되어서 overfitting이 더 쉽게 일어남) |
|      b3      | 64, 1e-4, 2 * 5     | 64, 1e-4, 4 * 5     | **64, 1e-4, 4 * 5** | **81.7619 / 0.7612**        | age 59, 60를 한 클래스로 묶음                                |
|      b3      | 64, 1e-4, 2 * 5     | 64, 1e-4, 4 * 5     | 64, 1e-4, 4 * 5     | 자체 f1-score 낮아서 시도 X | age 58, 59, 60를 한 클래스로 묶음 => age 59 유지             |
|      b3      | 64, 1e-4, 2 * 5     | 64, 1e-4, 9 * 5     | 64, 1e-4, 4 * 5     | 81.5079 / 0.7511            | valid[0 - 4]                                                 |
|      b3      | 64, 1e-4, 2 * 5     | 64, 1e-4, 4 * 5     | 64, 1e-4, 4 * 5     | **81.6190 / 0.7627**        | valid[4]로 통일                                              |

## :five: Result

- age: 59, 60 한 세트로 묶음

  > 58, 59, 60은 성능 저하
  >
  > label smoothing 사용 못 함

- Input Image Size : (312, 312)

- crop별 성능

  > crop 안함: 75.905 / 0.6947
  >
  > cutmix: 77.6508 / 0.7040
  >
  > facenet(mtcnn): 78.206 / 0.7263
  >
  > - 간격 확인, 30보다 15가 성능 상승

- KFold

  > 5-fold label별 (사람별 성능 저하, overfitting 확인은 쉬웠다.)
  >
  > 사람별로 돌려, best epoch 찾아 label별로 돌렸다.
  >
  > 결과는 train, valid 구분 없이 모든 데이터 학습 후, 제출
  >
  > valid_data = valid_ids[4] 통일
  >
  > > But, 최종 결과를 봤을 때,  valid_ids[0 - 4] 차례로 다 하는게 성능 상승했다. 아마, 더 다양한 set을 테스트하는 것이 정답이었던 듯 싶다.

- efficientNet-b3 (b4보다 가볍고, 성능 상승)

- best valid loss로 model parameter 업데이트

- Optimizer: Adam

  > SGD 속도 너무 느림, 포기

- lr_scheduler: ReduceLROnPlateau(mode='min', factor=0.2, patience=3)

  > StepLR에서 넘어감, 성능 상승

- Multi Sample Dropout 성능 저하

  > 하이퍼파라미터를 세밀하게 조정하거나 하이퍼파라미터 조절하기 전에 넣었으면 성능 상승했을 듯 싶다.

- Mask: CrossEntropy Loss / 64 / 1e-4 / 2 * 5

- Age: FocalLoss / 64 / 1e-4 / 4 * 5

- Gender: CrossEntropy Loss / 64 / 1e-4 / 4 * 5

- TensorBoard 사용

  > wandb 사용 못 함

## :six: Furthermore...

기간이 하루 이틀 더 있었으면 하는 아쉬움이 있었고, 그래도 짧은 기간 안에 많은 것을 코드로 짜고 배울 수 있어서 좋았다.

못 해본 것들은 다음 pstage에서 시도해봐야겠다.



[↩️ Go Back](https://github.com/lisy0123/Boostcamp_AI_Tech)
