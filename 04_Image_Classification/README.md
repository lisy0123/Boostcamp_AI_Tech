# Image Classification

> [Reference](https://github.com/lisy0123/Boostcamp_AI_Tech/blob/main/04_Image_Classification/reference.md)
>
> [Pstage summary](https://github.com/lisy0123/Boostcamp_AI_Tech/blob/main/04_Image_Classification/summary.md)

## Intro

- Simple Machine Learning Pipeline

  Domain Understanding => Data Mining => Data Analysis => Data Processing => Modeling => Training => Deploy

- Competition

  **Domain Understanding =>** Data Mining => **Data Analysis => Data Processing => Modeling => Training** => Deploy

---

## EDA (Exploratory Data Analysis)

탐색적 데이터 분석, 데이터 이해

- **Pre-processing**

  Bounding box, crop

  Resize

- **Generalization (일반화)**

  - **Bias & Variance**

    Underfitting, Overfitting

  - **Train & Validation**

  - **Data Augmentation**

    torchvision.transforms

    Albumentations

---

## Data Generation

- Data Feeding

- **Dataset 구조**

  ```python
  from torch.utils.data import Dataset
  
  # MyDataset 클래스가 처음 선언 되었을 떄 호출
  class MyDataset(Dataset):
    def __init__(self):
      pass
    
    # MyDataset 아이템 전체 길이
    def __len__(self):
      return None
    
    # MyDataset의 데이터 중 idx 위치의 아이템 return
    def __getitem__(self, idx):
      return None
  ```

  - num_workers

- Dataset, DataLoader 분리되는 게 좋음

  - Dataset: vanila 데이터를 원하는 형태로 출력해주는 class
  - Dataset을 더 효율적으로 사용하기 위함

---

## Model

- **Pytorch**

  low-level, Pythonic, Flexibility

- Pytorch 모델의 모든 레이어, **nn.Module** 클래스 따름

```python
import torch.nn as nn
import torch.nn.functional as F

class MyModel(nn.Module):
  # 정의
  def __init__(self):
    super(MyModel, self).__init__
    self.conv1 = nn.Conv2d(1,20,5)
    
  # 호출 시, 실행되는 함수
	def forward(self, x):
    return F.relu(self.conv1(x))
```

- 모든 nn.Module: forward() 함수 가짐

- Parameters: data, grad, requires_grad 변수 등 가지고 있음

- **ImageNet**

  14 miliion images, 20 thousands categories

```python
import torchvision.models as models

resnet18 = models.resnet18(pretrained=True)
# alexnet, squeezenet, vgg16, densenet, inception, googlenet, shufflenet, mobilenet, resnext50_32x4d, wide_resnet50_2, mnasnet
```

- Transfer Learning

- CNN base 모델 구조

  input + CNN Backbone + Classifier => Output

- fc == fully connected layer == classifier

---

## Trainig & Inference

- Error Backpropagation (오차 역전파

- **Loss**

  - Loss 함수 = Cost 함수 = Error 함수

  - loss.backward()

    실행 시, 모델의 파라미터의 grad 값 업데이트

  - **Focal Loss**

    Class Imbalance 문제가 있는 경우, 맞춘 확률이 높은 Class는 조금의 loss를, 맞춘 확률이 class는 loss 훨씬 높게 부여

  - **Label Smoothing Loss**

    class target label을 Onehot 표현으로 사용하기 보다,

    soft하게 표현해서 일반화 성능 높이기 위함

    > [0, 1, 0, 0...] => [0.025, 0.9, 0.025, 0.025, ...]

- **Optimizer, LR sheduler**

  - **StepLR**

    특정 step마다 lr 감소

    ```python
    scheduler = torch.optim.lr_scheduler.StepLR(optimizer, step_size=2, gamma=0.1)
    ```

  - **CosineAnnealingLR**

    코사인 함수 형태처럼 lr 급격히 변경

    ```python
    scheduler = torch.optim.lr_scheduler.CosineAnnealingLR(optimizer, T_max=10, eta_min=0)
    ```

  - **ReduceLROnPlateau**

    더 이상 성능 향상이 없을 때 lr 감소

    ```python
    scheduler = torch.optim.lr_scheduler.ReduceLROnPlateau(optimizer, 'min')
    ```

- **Metric**

  - 모델 평가, 객관적 평가

    - Classification: Accuracy, F1-score, precision, recall, ROC & AUC

    - Regression: MAE, MSE

    - Ranking: MRR, NDCG, MAP

  - Accuracy

    Class 별, 밸런스 적절히 분포

  - F1-Score

    Class 별 밸런스 좋지 않음

    => 각 클래스 별, 성능을 잘 낼 수 있는지 확인 필요

- **Trainig Process**

  1. model.train()

  2. optimzer.zero_grad()

  3. loss = criterion(outputs, labels)

  4. loss 마지막으로 chain 생성

  5. loss의 grad_fn chain

     => loss.backward()

  6. optimizer.step()

- **Inference Process**

  1. model.eval()
  2. with torch.no_grad():

- Validation (검증)

  추론 과정에 validation set 들어감

- Checkpoint

- 최종 Output, submission 형태 변환

- Pytorch Lightning

---

## Ensemble (앙상블)

싱글 모델보다 더 나은 성능을 위해 서로 다른 여러 학습 모델을 사용하는 것

- High Bias: **Underfitting**

- **Best**

- Hish Variance: **Overfitting**

  => Low Bias, High Variance

- **Model Averaging (Voting)**

  - Hard Voting, Soft Voting

- **Cross Validation (교차 검증)**

- **Stratified K-Fold Cross Validation**

  가능한 경우 모두 고려

  Split시에 Class 분포까지 고려

- **TTA (Test Time Augmentation)**

  테스트 이미지를 Augmentation => 모델 추론, 출력된 여러가지 결과 앙상블

- **성능과 효율의 Trade-off**

  앙상블 효과는 확실히 존재

  But, 그 만큼 학습, 추론 시간이 배로 소모

- **Hyperparameter Optimization, Optuna**

  파라미터 범위를 주고 그 범위 안에서 trials 만큼 시행

---

## Experiment Toolkits & Tips

- **Tensorboard**

  ```python
  tensorboard -logdir PATH --host ADDR --port PORT
  log가 저장된 경로 / 원격 서버에서 사용 시 0.0.0.0 (default: localhost) / 포트 번호
  ```

- **Weight and Bias (wandb)**

  깃허브 느낌

- **Machine Learning Project**

  - **Jupyter Notebook**

    코드를 빠르게 Cell 단위로 실행 가능

    보통 EDA 시, 매우 편리

    학습 진행 도중 노트북 창이 꺼지면 못 돌아감

  - **Python IDLE**

    구현은 한번만, 사용은 언제든, 간편한 코드 재사용

    디버깅, 자유로운 실험 핸들링



[↩️ Go Back](https://github.com/lisy0123/Boostcamp_AI_Tech)

