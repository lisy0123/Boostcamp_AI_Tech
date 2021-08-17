# RNN

- **RNN: Recurrent Neural Network, 순환신경망**

  반복적이고 순차적인 데이터(Sequential data)학습에 특화된 인공신경망의 한 종류

  내부의 순환구조가 들어있음

  순환구조를 이용하여 과거의 학습을 Weight를 통해 현재 학습 반영

  기존의 지속적이고 반복적이며 순차적인 데이터학습의 한계를 해결하는 알고리즘

  현재의 학습과 과거의 학습의 연결을 가능하게 하고 시간에 종속됨

  음성 웨이브폼을 파악, 텍스트의 앞 뒤 성분을 파악할 때 주로 사용

- **시퀀스 데이터**: 소리, 문자열, 주가 등의 데이터를 스퀀스(sequence) 데이터로 분류

  (시계열(time-series) 데이터는 시간 순서에 따라 나열된 데이터로 시퀀스 데이터에 속함)

  시퀀스 데이터는 독립동등분포(i.i.d.) 가정을 잘 위배함 => 순서를 바꾸거나 과거 정보에 손실이 발생하면 데이터의 확률분포도 바뀜

- 과거 정보 또는 앞뒤 맥락 없이 미래를 예측하거나 문장을 완성하는 건 불가능하다.

- 이전 시퀀스의 정보를 가지고 앞으로 발생할 데이터의 확률분포를 다루기 위해 조건부확률 이용 가능

  $\begin{aligned}
  P\left(X_{1}, \ldots, X_{t}\right) &=P\left(X_{t} \mid X_{1}, \ldots, X_{t-1}\right) P\left(X_{1}, \ldots, X_{t-1}\right) \\
  &=P\left(X_{t} \mid X_{1}, \ldots, X_{t-1}\right) P\left(X_{t-1} \mid X_{1}, \ldots, X_{t-2}\right) \times P\left(X_{1}, \ldots, X_{t-2}\right)\\
  &=\prod_{s=1}^{t} P\left(X_{s} \mid X_{s-1}, \ldots, X_{1}\right)
  \end{aligned}$

  $X_{t} \sim P\left(X_{t} \mid X_{t-1}, \ldots, X_{1}\right)$

  - 과거의 모든 정보를 사용하지만 스퀸스 데이터를 분석할 때 모든 과거 정보들이 필요한 것 X

- 시퀀스 데이터를 다루기 위해 길이가 **가변적**인 데이터를 다룰 수 있는 모델 필요

  <img src="https://user-images.githubusercontent.com/60209937/128503349-d843862a-0408-4c4b-b9d6-d30508819617.png" style="zoom:45%;" />

- 조건부에 들어가는 데이터 길이 가변적

  <img src="https://user-images.githubusercontent.com/60209937/128503357-4bdd2a18-dfd0-4f63-b9a9-f70c9c27fa95.png" alt="Screen Shot 2021-08-06 at 7 57 41 PM" style="zoom:45%;" />

- 고정된 길이 $\tau$ 만큼의 시퀀스만 사용하는 경우 $AR(\tau)$​(Autoregressive Model) 자가회귀모델이라고 부름

  <img src="https://user-images.githubusercontent.com/60209937/128503577-fc5c2b8f-ba7f-460f-82db-7bda37839c99.png" alt="132" style="zoom:35%;" />

- 다른 방법: 바로 이전 정보를 제외한 나머지 정보들을 $H_t$​​라는 잠재변수로 인코딩해서 활용하는 잠재 AR 모델

  <img src="https://user-images.githubusercontent.com/60209937/128503360-922fcb95-1124-4e1f-88ac-011e7d0772c9.png" alt="Screen Shot 2021-08-06 at 8 09 15 PM" style="zoom:50%;" />

  => 어떻게 인코딩하지 => RNN(순환 신경망)

- RNN: 잠재변수 $H_t$​를 신경망을 통해 반복해서 사용하여 시퀀스 데이터의 패턴을 학습하는 모델

- 가장 기본적인 RNN 모형: MLP와 유사한 모양

  <img src="https://user-images.githubusercontent.com/60209937/128503919-3da834b2-7453-4067-8b15-375e7165db67.png" alt="5" style="zoom:60%;" /><img src="https://user-images.githubusercontent.com/60209937/128503366-d493d641-a0d9-4627-987c-7929d3c570d4.png" alt="Screen Shot 2021-08-06 at 8 10 28 PM" style="zoom:70%;" />

  $W^{(1)}, W^{(2)}$: 시퀀스와 상관없이 불변인 행렬

- RNN은 이전 순서의 잠재변수와 현재의 입력을 활용하여 모델링

  $\begin{aligned}
  \mathbf{O}_{t} &=\mathbf{H}_{t} \mathbf{W}^{(2)}+\mathbf{b}^{(2)} \\
  \mathbf{H}_{t} &=\sigma\left(\mathbf{X}_{t} \mathbf{W}_{X}^{(1)}+\mathbf{H}_{t-1} \mathbf{W}_{H}^{(1)}+\mathbf{b}^{(1)}\right)
  \end{aligned}$<img src="https://user-images.githubusercontent.com/60209937/128503369-ddc1abaa-f287-4adc-9aae-8b44a0349d0b.png" alt="Screen Shot 2021-08-06 at 8 12 00 PM" style="zoom:70%;" />

  잠재변수인 $H_t$를 복제해서 다음 순서의 잠재변수를 인코딩하데 사용

  - t에 따라 변하는 것은 잠재변수, 입력데이터

- **RNN의 역전파**

  <img src="https://user-images.githubusercontent.com/60209937/128503371-ea4ae870-de31-4953-8b59-6f256abab80f.png" alt="Screen Shot 2021-08-06 at 8 14 19 PM" style="zoom:70%;" />

  잠재변수의 연결그래프에 따라 순차적으로 계산

  이를 **Backpropagation Through Time(BPTT)**이라 하고, RNN의 역전파 방법

- BPTT를 통해 RNN의 가중치행렬의 미분을 계산해보면 아래처럼 미분의 곱으로 이루어진 항 계산됨

  <img src="https://user-images.githubusercontent.com/60209937/128503375-a10de35f-ba51-4dc1-858d-8f6349103fef.png" alt="Screen Shot 2021-08-06 at 8 15 34 PM" style="zoom:90%;" />

  <img src="https://user-images.githubusercontent.com/60209937/128503376-b5331865-5a41-469b-9bb7-3108ae0a5381.png" alt="Screen Shot 2021-08-06 at 8 16 00 PM" style="zoom:90%;" />

- 기울기 소실(Gradient Vanishing)의 해결책

  - **Truncated BPTT**

    <img src="https://user-images.githubusercontent.com/60209937/128503378-11ec4e66-7407-47e2-8646-25d1268f95e2.png" alt="Screen Shot 2021-08-06 at 8 19 33 PM" style="zoom:80%;" />

    스퀀스 길이가 길어지는 경우, BPTT를 통한 역전파 알고리즘의 계산이 불안정해지므로 길이를 끊는 것 필요

  => Vanilla RNN은 길이가 긴 시퀀스를 처리하는데 문제가 있음

  => 이를 해결하기 위해 등장한 RNN 네트워크가 LSTM과 GUE

  <img src="https://user-images.githubusercontent.com/60209937/128503379-36b73d0d-efb7-414a-9bc1-cc3ae9487df6.png" alt="Screen Shot 2021-08-06 at 8 20 44 PM" style="zoom:70%;" />

  

