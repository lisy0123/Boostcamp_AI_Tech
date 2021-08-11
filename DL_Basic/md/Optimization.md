# Optimization(최적화)

- **Gradient Descent**

  First-order(일차미분값만 사용) iterative optimization algorithm for finding a local minimum of a differentiable function

- **Generalization**

  How well the learned model will behave on unseen data

  <img width="865" alt="image1" src="https://user-images.githubusercontent.com/60209937/128796090-a0d9a371-7500-4389-82cf-96f3e57294d1.png" style="zoom: 50%;" >

  갭이 작을 수록 well generalized

  학습데이터 성능이 안 좋으면 generalization performance가 높아도, 데티스트 데이터에서의 성능 보장 X

  => generalization performance가 높다는 게 네트워크 성능 좋은 것 X

- **Under-fitting <=> over-fitting**

  <img width="961" alt="image2" src="https://user-images.githubusercontent.com/60209937/128796094-4faf3683-3223-4e6b-a520-2f99d37f9067.png" style="zoom:55%;" >

  가장 이상적인 방향: **balanced-fitting과 동시에 generalization performance 높이기**

  - **Under-fitting**

    학습을 마친 후, 모델의 성능이 기대치보다 떨어지는 현상

    이상적인 결정경계(Decision Boundary)에 비해 지나치게 단순한 경우, 학습데이터조차 제대로 학습 못한 상황

    - 발생 이유

      작은 학습 반복 횟수

      학습 데이터 부족

      데이터의 feature에 비해 간단한 모델

  - **Over-fitting**

    학습을 마친 후, 새로운 데이터에 대해 그 결과가 매우 다르게 나오는 현상

    이상적인 결정경계(Decision Boundary)보다 지나치게 복잡한 경우, 학습데이터에 과하게 학습된 상황

    - 발생 이유

      편중된 학습데이터

      무분별한 noise 수용

      너무 많은 데이터 feature로 모델 복잡

  - 충분한 학습데이터 확보, 모델 및 하이퍼파라미터 조정 => under-fitting 해결

  - 다양한 학습데이터 확보, regularization => over-fitting 해결

  - trade-off 관계를 가지므로 적절한 파라미터 설정이 중요

- **Cross-validation**(교차검증)

  특정한 테스트 데이터셋에 과적합 문제, 테스트 데이터가 고정되어 있기 때문

  => 데이터 중 일부분을 테스트 데이터로 두지 앟ㄴ고 데이터의 모든 부분 사용, 교차 검증

  => 성능 검증

  Model validation technique for assessing how the model will generalize to an independent (test) data set

  - **K-fold Cross-validation**

    <img src="https://user-images.githubusercontent.com/60209937/129023051-317bd25f-45df-4066-88d3-d4aec5041fe4.png" alt="image1" style="zoom:75%;" />

    전체 데이터 셋 K등분의 부분집합으로 분할

    K - 1개의 부분집합은 학습 데이터셋, 나머지 1개의 부분집합은 테스트 데이터셋 할당

    교차 검증 총 K번만큼 반복

  - **Hold-out Calidation**

    <img src="https://user-images.githubusercontent.com/60209937/129023054-3d03b382-9719-4f5f-be9c-0868cce0f092.png" alt="image2" style="zoom:75%;" />

    전체 데이터셋을 학습 데이터셋과 테스트 데이터셋으로 나눔

    분리된 학습 데이터셋에서 다시 검증 데이터셋을 따로 떼어내어 교차 검증

    장점: 교차 검증을 한 번만 진행 => 계산 시간 적음

  - **Leave-one-out Cross-validation**

    <img src="https://user-images.githubusercontent.com/60209937/129023058-ad9938f3-9bf3-4fc7-9261-ddffd289287d.png" alt="image3" style="zoom:75%;" />

    전체 N개의 샘플 데이터셋을 N-1개의 학습 데이터셋과 1개의 테스트 데이터셋으로 나눔

    총 N번만큼 교차 검증 반복

    단점: 계산량이 많음

  - **Leave-p-out Cross-validation**

    <img src="https://user-images.githubusercontent.com/60209937/129023063-edafe120-f310-46f1-8074-e9b1edc46874.png" alt="image4" style="zoom:80%;" />

    전체 N개의 샘플 데이터셋을 N-p개의 학습 데이터셋과 p개의 테스트 데이터셋으로 나눔

    총 nCp번만큼 교차 검증 반복

    Leave-one-out 교차 검증 기법보다 더 계산량 많음 => 교차 검증 반복 횟수를 늘리고자 할 때 사용

  - **Stratified K-fold Cross-validation**

    <img src="https://user-images.githubusercontent.com/60209937/129023064-eafc86a5-5a70-4934-adfc-cd64a2464eef.png" alt="image5" style="zoom:85%;" />

    데이터 label 분포까지 고려해서 각 fold의 라벨분포가 전체 데이터의 라벨분포에 근사해서 훈련 및 검증

    label 분포, 불균형한 상태 검증 => 오류 일으킴

    => 주로 classification에서 사용

  - **Shuffle-split Cross-validation**

    <img src="https://user-images.githubusercontent.com/60209937/129023068-676620c5-3585-4cd4-90b6-19c03e61801e.png" alt="image6" style="zoom:80%;" />

    train dateset과 test dataset을 랜덤하게 나누고 반복

    랜덤 => 어떤 fold는 여러 번 선택, 한번도 선택되지 않은 fold 발생

  - **Nested Cross-validation**

    <img src="https://user-images.githubusercontent.com/60209937/129023072-a26f5ad1-49d0-427e-b677-2fc04a0776be.png" alt="image7" style="zoom:65%;" />

    기존 교차검증 중첩

    outer loop에서 기존 validation set 나누는 것 대신, testset을 다양한 fold로 나눠서 구성

    outer loop에서 생성된 train set에 inner loop 적용

    => train set + validation set으로 나눠서 검증, 파라미터 튜닝

    최적의 파라미터 통해 outerloop에서 생성된 test set으로 평가

  - **Time-series Cross-validation**

    <img src="https://user-images.githubusercontent.com/60209937/129023075-86e95e59-978f-4d3e-b92e-5112230b47c5.png" alt="image8" style="zoom:67%;" />

    시계열 데이터 경우, 시간정보 결과에 영향

    high variance 띔 => over-fitting 발생

    => validation / test set에 train set보다 미래 데이터 사용 => 합리적 검증

- **Bias and Variance**

  - **Bias**: aim값에서 평균적으로 얼마나 벗어났는지
  - **Variance**: 비슷한 입력에 대한 일관적인 출력 (분산)

  <img width="451" alt="image4" src="https://user-images.githubusercontent.com/60209937/128796101-ad12c1a8-58e2-45b6-ad56-adf2fde8058b.png" style="zoom:65%;" >

- **Bias and Variance Tradeoff**

  cost = bais^2 + bariance + noise

  <img width="778" alt="image5" src="https://user-images.githubusercontent.com/60209937/128796102-4034fd13-ec2f-404d-b78c-3581a4b94985.png" style="zoom:50%;" >

  모델 학습 => cost 최소화

  cost 를 최소화하는 것: bias 와 variance를 조절하는 문제

  Noise: 데이터가 가지는 본질적인 한계치 이기 때문에 irreducible error

  bias/variance : 모델에 따라 변하는 것이기에 reducible error

  **bias / variance => trade-off 관계를 가지므로 적절한 파라미터 설정이 주요문제**

- **Bootstrapping**

  Any test or metric that uses random sampling with replacement

  고정된 데이터에 대해 random sampling with replacement 적용

  => 생성된 데이터 또는 이 데이터를 기반으로 만들어진 model/metric  생성

  - **Bagging** (**B**ooststrapping **agg**regat**ing**)

    Multiple models are being trained with bootstrapping

    주어진 데이터에서 여러 개의 boostrap 이용, 각각의 모델 생성 후 최종 모델을 결정하는 방법

    - 데이터로 부터 여러번의 복원 샘플링

      => 예측 모형의 분산을 줄여 줌

      => 예측력을 향상

    **High variance,Low bias(Over-fitting) 인 모델에 사용**

  - **Boosting**

    Focus on those specific training samples that are hard to classify

    weak learners들을 결합하여 하나의 strong learner 만듦

    - 순차적으로 오분류된 개체들에게는 높은 가중치,

      정확하게 분류된 개체들에게는 낮은 가중치 적용

      => 오분류된 객체들이 더 잘 분류하는 것

  <img width="736" alt="image6" src="https://user-images.githubusercontent.com/60209937/128796104-15ce544a-85f2-4d8c-a15a-2bee6e9bc904.png" style="zoom:60%;" >

- **Gadient Descent Methods**

  - BGD (Batch Gradient Descent) / 배치 전체
  - SGD (Stochastic Gradient Descent) / 배치 1 
  - MSGD (Mini-batch Gradient Descent) / 배치 사용자 지정

- **Batch-size Matters**

  <img width="887" alt="image7" src="https://user-images.githubusercontent.com/60209937/128796107-d32943ae-be6f-4ee9-80ac-a0867cf62bb4.png" style="zoom:50%;" >

  batch: large => minimizers: sharp

  batch: small => minimizers: flat

  => 작은 배치 사이즈가 더 학습이 잘됨, generalization performance 더 놓음

- **Gradient Descent Methods**

  - **Stochastic gradient descent**

    <img width="396" alt="image8" src="https://user-images.githubusercontent.com/60209937/128796108-61e46a0a-324f-4632-9158-ac9443c9849a.png" style="zoom:60%;" >

  - **Momentum (관성)**

    <img width="482" alt="image9" src="https://user-images.githubusercontent.com/60209937/128796109-0e584816-ec5b-417e-9d1e-3a975fbd8f7f.png" style="zoom:60%;" >

    관성 존재 => gradient 요동쳐도 어느 정도 꾸준히 학습 가능

    과거의 누적 gradient $v_t$ (Accumulation)에 momentum $μ$ 을 곱한 momentum step $μv_t$ 을 이용해, 현재 gradient $∇f(\theta_t)$ 조정

    보통 모멘텀 $μ=0.9$​​ 사용

    <img src="https://user-images.githubusercontent.com/60209937/128809286-afee016f-b151-4443-885e-b7705f72d757.png" alt="1" style="zoom:65%;" />

    - SGD가 oscilation(진동)을 겪을 때, 중앙의 최적점을 이동하는 힘을 줌

      => SGD에 비해 상대적으로 빠르게 이동 가능

    - 기존에 이동했던 방향에 모멘텀 있음

      => local minima 빠져나오는 효과 기대 가능

  - **Nesterov accelerated gradient(NAG)**

    <img width="441" alt="image10" src="https://user-images.githubusercontent.com/60209937/128796112-f31d690f-0789-4991-b97d-b879002834b5.png" style="zoom:60%;" >

    - $\nabla \mathcal{L}\left(W_{t}\right)=g_{t}$

    - $-\eta \beta a_{t}$: momentum에 대한 피드백, gradient 값이 -/+로 바뀔 때 급격하게 바뀌도록 함

    현재 위치의 gradient $∇f(θ_t)$​가 아닌 현재 위치에서 만$μv_t$​큼 이동한 gradient $∇f(θ+μv_t)$​​ 이용

    momentum step 을 적용한 위치에서의 gradient $∇f(θ+μv_t)$​ (lookahead gradient step) 구한 후 업데이트

    momentum이 local minimum에 converge하지 못하는, 느린 경우 예방

    lookahead gradient step을 통해 어떤 방식으로 이동할지 결정

    => 유동적 이동 가능 => 기존의 momentum 방식에 비해 효과적

    <img width="880" alt="image11" src="https://user-images.githubusercontent.com/60209937/128796114-ce9d8deb-d9ff-4b30-b787-b26c3abd6749.png" style="zoom:80%;" >

  - **Adagrad(Adaptive Gradient)**

    <img width="649" alt="image12" src="https://user-images.githubusercontent.com/60209937/128796116-d7a0b9f5-6c0a-420e-bb7d-b4024be45993.png" style="zoom:60%;" >

    - $G_t$: 변화량 제곱합을 역수로 넣음

    - $\epsilon$: zero division 예방

    많이 변화한 파라미터 일수록 적게 이동, 적게 변화한 파라미터 일수록 많이 이동하게 adapts

    학습을 진행하는 동안: 보통 step size=0.01 정도를 사용한 뒤, step size decay 등을 신경 쓰지 X

    - 학습이 긴 시간 진행될 경우 $G_t$는 계속 증가

      => $\frac η{\sqrt{G_t+ϵ}}$​이 작아지는 문제 발생

      => 결국 거의 움직이지 않게 되는 문제 존재

  - **Adadelta(Adaptive Delta)**

    <img width="788" alt="image13" src="https://user-images.githubusercontent.com/60209937/128796117-4ca6cba7-8eab-4db3-94a9-62b6b9e9640b.png" style="zoom:60%;" >

    No learning rate

    기존 Adagrad 보완

    - 학습이 길어짐에 따라 **learning rate decay 문제 해결**
    - **직접 global learning rate를 결정해야하는 문제 해결**

    **EMA(Exponential Moving Average)를 이용해 최신 gradient 반영**

    => learning rate decay 문제 해결, 메모리 문제 해결

    monotonically dexreasing property 예방

  - **RMSprop**

    An unpublished, adaptive learning rate method proposed by Geoff Hinton in his lecture.

    <img width="790" alt="image14" src="https://user-images.githubusercontent.com/60209937/128796118-1440b225-d0b5-4dd6-8d91-3e39c095d7ac.png" style="zoom:60%;" >

    기존 Adagrad 보완

    **EMA를 통해 최신의 gradient 반영**

  - **Adam**

    Adaptive Moment Estimation (Adam) leverages both past gradients and squared gradients.

    => **adaptive + momentum**

    <img width="858" alt="image15" src="https://user-images.githubusercontent.com/60209937/128796119-4c5ed4e1-17e6-4a8f-b64f-9d2358c36c77.png" style="zoom:60%;" >

    `Momentum + Adaptive` 방식으로 적절한 **step size & step 방향** 결정

    - 보통 $β_1=0.9$​​로는 $0.9, β_2=0.999, ϵ=10^{−8}$​​​ 정도의 값 사용

  <img src="https://user-images.githubusercontent.com/60209937/128810540-5715db77-8f02-4440-ba99-da3649098873.png" alt="9" style="zoom:80%;" />

- **Reqularization**

  Over-fitting을 막기 위한 regularization 종류

  - **Early stopping**

    training 중 validation error가 증가하는 부분에서 early stop

    <img width="882" alt="image16" src="https://user-images.githubusercontent.com/60209937/128796121-8ce0f03d-20e9-4c45-9013-88a1468d741f.png" style="zoom:50%;" >

    - 주의: loss 값이 상하로 움직이는 경우 생기므로, 직전 학습의 loss 만을 비교해서 종료하면 X

      => 일정 epoch 동안 계속해서 loss 가 증가 시, 학습 중단

  - **Parameter norm penalty(Weight Decay)**

    weight 숫자 작게 규제

    => add sommthness to the function space

    <img width="550" alt="image17" src="https://user-images.githubusercontent.com/60209937/128796123-fec458ab-6d6d-430c-904e-3800f9620ec4.png" style="zoom:67%;" >

    - **L1-norm penalty** => **Ridge model**

      $J(\theta)=Loss+\frac\lambda{2m}\sum_\theta\theta^2$

      $\begin{aligned}\theta:&=\theta-\alpha\frac{\part J(\theta)}{\part\theta}\\&=\theta-\alpha\frac{\part Loss}{\part\theta}+\frac \lambda m \theta \\&=(1-\frac{\alpha \lambda}m\theta-\alpha\frac{\part Loss}{\part \theta})\end{aligned}$

      $θ$ 가 작아지는 방향으로 진행

      => weight decay 를 통해 `outlier` 의 영향을 적게 받도록 만들어 generalization performance를 높이는 것

    - **L2-norm penalty** => **Lasso model**

      $J(\theta)=Loss+\frac\lambda{m}\sum_\theta|\theta|$​

      $\begin{aligned}\theta:&=\theta-\alpha\frac{\part J(\theta)}{\part\theta}\\&=\theta-\frac{\lambda\alpha}m sgn(\theta)-\alpha\frac{\part Loss}{\part\theta} \end{aligned}$​

      $\theta$ 값 자체를 줄이는 것이 아닌 $\theta$ 부호에 따른 상수값을 빼주는 방향으로 진행

      => 상수값을 빼줘 작은 $\theta$ 들은 거의 0으로 수렴하게 되어 특정 중요한 $\theta$​들만 남음

  - **Data augmentation**

    label preserving augmentation

    **class 에 영향을 끼치지 않는 선에서 Data Augmentation은 필수적**

    <img width="970" alt="image18" src="https://user-images.githubusercontent.com/60209937/128796125-9c29b4fc-6e06-40b0-92da-ea6c94366f4f.png" style="zoom:80%;" >

    <img width="479" alt="image19" src="https://user-images.githubusercontent.com/60209937/128796126-e51d5a2b-5b12-4c39-88ff-f8c6b1094fee.png" style="zoom:50%;" >

  - **Noise robustness**

    Add random noise inputs or weights

    <img width="653" alt="image20" src="https://user-images.githubusercontent.com/60209937/128796129-c7fdbe68-379e-490a-8177-1a51c08c47bd.png" style="zoom:60%;" >

  - **Label smoothing**

    <img width="645" alt="image22" src="https://user-images.githubusercontent.com/60209937/128796133-07aee170-67cf-4d08-b7e9-00b8ab25392e.png" style="zoom:70%;" >

    Hard target(label) 을 sotf target(label) 로 바꾸는 것

    label smoothing을 통해 mislabeling 데이터 다룸 + generalization performance를 올리는 방법

    사진 합성, 알맞은 레이블링 => 성능 향상

  - **Dropout**

    <img width="706" alt="image23" src="https://user-images.githubusercontent.com/60209937/128796135-fd562c46-28c8-4b43-889f-63d2494cbb4b.png" style="zoom:50%;" >

    Radomly set some neurons to zero (무작위로 일부 노드들을 생략하여 학습을 진행)

    드롭아웃 $p=0.5$ => 50%의 뉴런 비활성화

    각각 뉴런들이 좀 더 robust한 feature를 잡을 수 있음

    => 모델의 generalization performance 올라감

    **over-fitting을 막음**
  
    매번 다른 형태의 노드로 학습 => 여러 형태의 네트워크를 생성하는 **앙상블 효과**
  
  - **Batch normalization**

    $\begin{aligned}
    \mu_{B} &=\frac{1}{m} \sum_{i=1}^{m} x_{i} \ \ // \text{mini-batch mean}\\
    \sigma_{B}^{2} &=\frac{1}{m} \sum_{i=1}^{m}\left(x_{i}-\mu_{B}\right)^{2} \ \ // \text{mini-batch variance}\\
    \hat{x}_{i} &=\frac{x_{i}-\mu_{B}}{\sqrt{\sigma_{B}^{2}+\epsilon}} \ \ // \text{normalize}\\ y_i&=\gamma\hat{x}_i+\beta\equiv BN _{\gamma,\beta}(X_i) \ \ // \text{scale and shift}
    \end{aligned}$​​​​​​​​ 

    internal covariate shift(네트워크의 각 층이나 Activation마다 input 의 distribution이 달라지는 현상)가 줄어듬

    Learning rate 을 너무 높게 잡은 경우, gradient vanishing/exploding 가 발생

    => Batch Normalization을 통해 parameter scale 영향 제거, learning rate를 크게 잡을수 있음, 빠른 학습을 가능
  
    regularization 효과, Dropout와 같은 효과(Dropout의 경우 효과는 좋지만 학습속도가 다소 느려짐)
  
    => Dropout을 제거, 학습속도 향상
  
    