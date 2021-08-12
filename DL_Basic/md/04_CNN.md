# CNN (Convolutional Neural Network)

- Receptive Field & Feature Map

  - **Receptive Field**

    커널이 적용되는 영역으로 feature map의 한 노드

  - **Feature Map**

    커널의 합성곱연산을 적용하여 얻어낸 결과

  - **Receptive Field 크기** => 얼마나 많은 정보 수용하는지

    ex) $5\times5$ 입력 이미지, stride = 1, $3\times3$ 커널 연산 및 pooling 적용한 feature map의 한 노드

    => **$4\times4$​ receptive field 가짐**

  - Dialated convolution

    $D$​만큼 팽창된 커널의 convolution 연산

    <img src="https://user-images.githubusercontent.com/60209937/128983127-9a2d7a9c-82f6-42b6-985a-e49e673c9e63.png" alt="1" style="zoom:65%;" />

- **RGB Image Convolution**

  처널 3개, 즉 다수의 채널

  각 채널마다 커널이 합성곱연산

  3개의 커널이 아니라, **3개의 채널을 가진 1개의 커널**

  <img src="https://user-images.githubusercontent.com/60209937/128983426-a7b72a38-7939-4ec1-8b1d-6bd16e1b2ba2.png" alt="2" style="zoom:70%;" />

  <img src="https://user-images.githubusercontent.com/60209937/128983420-a8d62d89-98df-4977-97b1-1e1982743620.png" alt="3" style="zoom:50%;" />

- Continuous convolution

  $(f*g)(t)=\int f(\tau)g(t-\tau)d\tau = \int f(t-\tau)g(t)f\tau$​​

- Discrete convolution

  $(f*g)(t)=\sum^\infin_{i=-\infin}f(i)g(t-i)=\sum^\infin_{i=-\infin}f(t-i)g(i)$​

- 2D image convolution

  $(I*K)(i,j)=\sum_m\sum_nI(m,n)K(i-m,j-n)=\sum_m\sum_nI(i-m,i-n)K(m,n)$​​

- CNN

  - **Convolution layer**

    합성곱연산을 통해 feature 추출

  - **Pooling layer**

    down sampling 통해 feature 추출

  - **Fully connected layer**

    decision making (classification)

  <img width="655" src="https://user-images.githubusercontent.com/60209937/128986760-404d5fcc-1ccd-4d68-bcf2-fb5d9c1eb2c6.png" style="zoom:90%;" >

- **Stride**

  커널의 합성곱연산 적용할 때, 움직이는 간격

  <img src="https://user-images.githubusercontent.com/60209937/128983639-0a4fa70f-9f92-4c86-9e29-a97bce5531fe.png" alt="3" style="zoom:70%;" />

  <img src="https://user-images.githubusercontent.com/60209937/128959000-3cb21b54-ea29-47f6-87ef-f53f883427bb.png" alt="5" style="zoom:67%;" />

- **Padding**

  반복적으로 합성곱 연산 적용 시, feature map 너무 작아지거나 이미지의 모서리 부분의 정보 손실 막기 위한 방법

  입력데이터 주변을 0으로 채워넣는 방법

  <img src="https://user-images.githubusercontent.com/60209937/128983649-83fb12b4-3d35-48f4-97d9-3d06571e954c.png" alt="5" style="zoom:70%;" />

  <img src="https://user-images.githubusercontent.com/60209937/128959005-8ccae2dc-0e54-49f4-a30a-fe17ad8e96d9.png" alt="4" style="zoom:67%;" />

  <img src="https://user-images.githubusercontent.com/60209937/128959007-f2ad8177-44cc-4661-abb4-b68c23141c78.png" alt="3" style="zoom:90%;" />

- **Pooling**

  convolutional layer ekdmadp pooling layer 추가

  - pooling layer

    down sampling을 통해 feature map의 크기 줄여주는 연산, 커널, stride 개념 가짐

  - **max pooling**

    커널의 receptive field 최댓값

  - **average pooling**

    커널의 receptive field 평균값

  <img src="https://user-images.githubusercontent.com/60209937/128984652-0b95514f-8aa6-4280-a2a7-6cc5dd2aac70.png" style="zoom:45%;" />

- **Feature Map 크기 계산하기**

  - $I_h$​ : 입력 데이터 높이
  - $I_w$: 입력 데이터 너비
  - $K_h$: 커널 높이
  - $K_w$: 커널 너비
  - $S$: stride
  - $P$ : padding
  - $O_h$: feature map 높이
  - $O_w$: feature map 크기

  feature map => $(O_h, O_w)=(\lfloor\frac{I+h-K_h+2P}S+1\rfloor, \lfloor\frac{I_w-K_w+2P}S+1\rfloor)$​

  ex) $(O_h, O_w)=(\lfloor\frac{32-5+2*0}1+1\rfloor, \lfloor\frac{32-5+2*0}1+1\rfloor)=(28,28)$

- **Convolution Arithmetic**

  캐널의 채널 수 = 입력데이터의 채널 수

  하나의 커널 = $K_h*K_w*C$개의 파라미터

  커널 $C_n$개 존재 = 총 $K_h*K_w*C*C_n$개의 파라미터

  <img src="https://user-images.githubusercontent.com/60209937/128959010-f6c9cb4f-f9c6-47fe-b9fd-87a1dafba969.png" alt="2" style="zoom:67%;" />

  => $3\times3\times128\times64 = 73,728$​​

  <img src="https://user-images.githubusercontent.com/60209937/128959012-47cf8a3c-49bf-4853-bbec-f1c20684a3f3.png" alt="1" style="zoom: 90%;" />

  $11\times11\times3\times48*2\approx35k$​

  $3\times3\times128*2\times192*2\approx884k$​

  $3\times3\times192\times192*2\approx663k$

  $3\times3\times192\times1258*2\approx442k$

  $13*13*128*2\times2048*2\approx177M$

  $2048*2\times2048*2\approx16M$​

  $2048*2\times1000\approx4M$

- **$1\times1$​​ Convolution**

  $256\times256\times128 = CONV\ 1\times1\times128\times32 => 256\times256\times32$​

  - Dimension reduction
  - depth 증가, parametere 감소
  - bottleneck architecture