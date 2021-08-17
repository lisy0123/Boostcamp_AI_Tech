# RNN(Recurrent Neural Networks)

- **Sequential Model**

  소리, 문자열, 주가 등 이벤트의 발생 순서가 중요한 데이터

  시계열(time-series) 데이터

  i.i.d(독립동등분포) 가정 자주 위배 => 순서를 바꾸거나 과거 정보 손실 발생 시, 데이터 확률분포 바뀜

  이전 시퀀스 정보 가지고 앞으로 발생할 데이터 확률분포 다룸

  => 조건부확률 이용

  $\begin{aligned}P(X_1, ...X_t)&=P(X_t|X_1, ..., X_{t-1})P(X_1, ...,X_{t-1})\\&=\prod_{s=1}^tP(X_s|X_1,...,X_{s-1})\end{aligned}$​​

  => $X\sim P(X_t|T_{t-1},...,X_1)$​​로 과거의 정보를 담은 확률분포 표현

  과거의 모든 정보 필요한 것 X, 맥락에 따라 잘 truncate 하는 것이 중요

  - **Naive sequence model**

    $X_t\sim P(X_t|X_{t-1},...,X_1)$

    $X_{t+1} \sim P(X_{t+1}|X_t,...,X_1)$​

    이전의 모든 정보 담는 방법, 현 시점에서 과거의 모든 정보 담는 모델

    길이가 가변적인 데이터를 다룰 수 있는 모델

    $X_t$는 t-1개, $X_t$​는 t개의 정보 이용

  - **Autoregressive model** (자기회귀 모델, AR 모델)

    $X_t\sim P(X_t|X_{t-1},...,X_{t-\tau})$​​

    $X_{t+1} \sim P(X_{t+1}|X_t,...,X_{t-\tau+1})$​

    각 시점마다 가변적인 길이 데이터 다룸, 고정된 길이의 데이터를 다루는 문제로 재정의

    => 고정된 길이 $\tau$​​​만큼 정보를 담는 모델

    - **Markov model (first-rder autoregressive model)**

      AR 모델에서 가장 간단한 모델: 현재의 정보는 바로 이전 단계의 과거의 정보에만 영향을 받는다는 가정에 정의

      $P(X_1,...,X_t)=P(X_t|X_{t-1})P(X_{t-1}|X_{t-2})\dots P(X_2|X_1)P(X_1)=\prod^t_{s=1}P(X_s|X_{s-1})$​​고정된 길이 $\tau=1$ 만큼의 정보를 담는 모델

      **현 시점의 정보가 단순히 바로 단계의 과거 정보만 가지고 결정**

      => 많은 과거의 정보 이용 X

  - **Latent AR Model** (잠재자기회귀 모델)

    고정된 길이의 데이터를 다루는 동시에, 유의미한 과거의 정보를 담는 모델

    $X_t\sim P(X_t|X_{t-1},H_t)$

    $X_{t+1}\sim P(X_{t+1}|X_t,H_{t+1})$

    $H_t=Net_\theta(H_{t-1},X_{t-1})$

    **잠재변수(Latent Variable) $H_t$​** 이용하여 정의

    $H_t$를 신경망을 통해 반복해서 사용, 시퀀스 데이터의 패턴을 학습하는 모델 => **RNN(Recurrent Neural Networks)**

- **Recurrent Neural Neetwork**

  MLP와 달리, 자기 자신을 돌아오는 구조 있음

  <img width="784" alt="1" src="https://user-images.githubusercontent.com/60209937/129129965-2ddf441b-f76e-41e0-b3e1-3389e7042ef7.png" style="zoom:67%;" >

  - **short-term dependencies (단점)**

    RNN: 하나의 fixed rule로 정보들을 계속 취합

    => 먼 과거에 있던 정보가 미래까지 살아남기 어려움

    **현재에서 몇 개의 전 과거의 데이터는 잘 고려되지만, 멀리 있는 정보는 고려하기 힘듬**

  <img width="872" alt="Screen Shot 2021-08-12 at 10 42 33 AM" src="https://user-images.githubusercontent.com/60209937/129130841-6c170680-f652-46ae-a88c-0e662fe190cd.png" style="zoom:60%;" >

- **LSTM(Long Sort Term Memory)**

  RNN의 멀리 있는 과거 정보 gradient vanshing 문제로 학습 기여 못하는 문제 해결을 위해 고안

  RNN의 hidden statedp cell state을 추가한 구조

  <img width="887" alt="image2" src="https://user-images.githubusercontent.com/60209937/129130986-32a3a13b-a25e-49f9-b568-c33a57f6eb40.png" style="zoom:70%;" >

  - **Cell state**

    <img src="https://user-images.githubusercontent.com/60209937/129131621-e96228f6-fc62-4be7-a362-3deef1bb2c43.png" alt="image1" style="zoom:67%;" />

    same as 컨테이너 벨트

    작은 linear interaction만을 적용시키면서 전체 체인 계속 구동

    정보가 전혀 바뀌지 않고 흐르게만 하는 것을 매우 쉽게 함

  - **gate**

    <img width="146" alt="image2" src="https://user-images.githubusercontent.com/60209937/129131622-fd2f5187-b1da-4daf-b5c5-8bf5da173ac0.png" style="zoom:50%;" >

    정보 전달될 수 있는 추가적인 방법

    sigmoid layer와 pointwise 곱셈으로 이루어짐

    sigmoid layer 0,1 숫자 내보냄 => 각 컴포넌트가 얼마나 정보 전달해야하는지 척도

    - 0: 아무 것도 넘기지 말라 // 1: 모든 것을 넘겨라

  - **Forget Gate**

    어떤 정보를 버릴지 결정, 과거 정보 얼마나 반영하지 결정

    (0,1) 값을 가지는 sigmoid가 과거의 정보를 얼마나 반영할지 결정

    - 1에 가까울수록 과거 정보 많이 반영 // 0에 가까울수록 과거 정보 적게 반영

    <img width="321" alt="image3" src="https://user-images.githubusercontent.com/60209937/129131623-35ab3f68-3e47-456b-8541-eb37de05fa9d.png" style="zoom:77%;" > $f_t=σ(W_f⋅[h_{t−1},x_t]+b_f)$​​

  - **Input Gate**

    어떤 정보를 cell state에 저장할지 결정

    <img width="329" alt="image4" src="https://user-images.githubusercontent.com/60209937/129131627-d13c4b29-c6fd-40a1-83df-1ad34241f96b.png" style="zoom:77%;" >

    $i_t=σ(W_i⋅[h_{t−1},x_t]+b_i)$​​

    $\tilde C_t=tanh(W_C⋅[h_{t−1},x_t]+b_C)$​​​

    - $i_t$: 현시점의 정보를 셀에 얼마나 반영할지 결정

    - $\tilde C_t$: 현 시점의 정보 계산

      => 현 시점이 실제 가지고 있는 정보가 얼마나 중요한지 반영

  - **Update Cell**

    forget gate, input gate의 값을 잘 취합해 cell state를 update
    과거 정보와 현시점의 정보의 중요도 반영해서 update

    <img width="321" alt="image5" src="https://user-images.githubusercontent.com/60209937/129131631-76c1b8dd-29ec-4c95-8c0b-3d2aea6bed61.png" style="zoom:77%;" >

    $i_t=σ(W_i⋅[h_{t−1},x_t]+b_i)$​​

    $C_t=f_t∗C_{t−1}+i_t∗\tilde C_t$

  - **Output Gate**

    update된 cell stat(과거 중요한 모든 정보)를 hidden state(다음 time step에 당장 필요한 정보)로 출력할 양 결정

    

    <img src="https://user-images.githubusercontent.com/60209937/129131633-e0aeed8b-de0c-4694-9656-0927d34b180f.png" alt="image6" style="zoom:77%;" />

    $o_t=σ(W_o⋅[h_{t−1},x_t]+b_o)$​

    $h_t=o_t∗tanh(C_t)$

  <img width="832" alt="image3" src="https://user-images.githubusercontent.com/60209937/129130987-ca4519ad-6395-411c-9f00-361c7faf300f.png" style="zoom: 67%;" >

- **GRU(Gated Recurrent Unit)**

  LSTM와 유사, but cell state 없이 더 간략한 구조

  <img width="796" alt="5" src="https://user-images.githubusercontent.com/60209937/129133038-aef95dc0-0aa7-4bc0-a70c-864b8bc20684.png" style="zoom:67%;" >

  LSTM와의 차이점: cell state $C_t$의 역할을 $H_t$가 같이 수행

  **$C_t$의 forget gate $f_t$와 input gate $i_t$의 역할을 $z_t$​의 가중평균으로 대체**

  두 개의 독립적인 gate => 1개의 gate, cell state 없이 hidden state만 있음 => **계산량, 메모리 이점**

