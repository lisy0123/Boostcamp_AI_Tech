# Generative Models 2

실용저그로 많이 사용되는(practical) 생성모델

- **VAE (Variational Auto-Encoder)**

  - **Variational inference**(VI, 변분추론)

    - 목적

      posterior distribution(사후분포)과 근사하는 variational distribution을 최적화하는 것

      observation 주어짐 => 관심 있는 random variables의 확률 분포, 즉 posterior dist.를 찾는 것

      - posterior distribution: $p_{\theta}(z|x)$

        관측값 $x$가 주어졌을 때, 특정확률변수 $z$의 분포

        - latent(잠재백터): $z$
        - likelihood(가능도): $p_{\theta}(x|z)$

      - variational distribution: $q_{\theta}(z|x)$

        일반적, variational inference 정확히 구하는 것 거의 불가능

        => variational distribution 구해 사후분포에 근사하게 최적화

        실제 사후분포와의 KL Divergence(쿨백 라이블러 발산) 최소화되는 varitional distribution 찾기

      <img width="634" src="https://user-images.githubusercontent.com/60209937/129579805-0b7401d4-5847-4039-a128-0b1abfbe200c.png" style="zoom:60%;" >

  - **ELBO (Evidence Lower BOund)**

    <img width="1049" src="https://user-images.githubusercontent.com/60209937/129580016-692434c9-9875-4187-afba-4b151255e2cf.png" style="zoom:60%;" >

    $p_{\theta}(D)=\text{ELBO}+D_{KL}$

    variational inference는 ELBO항과 KL Divergence 항의 합으로 유도

    **ELBO법**: KL Divergence를 최소화하기 위해 반대로 ELBO항을 최대화시키는 것

    => ELBO항은 tractable(구할 수 있음) 위 방법 가능

    => Sandwitch method라도 부름

    <img width="884"  src="https://user-images.githubusercontent.com/60209937/129580180-bbe5f1ff-5073-4969-a559-71ecdd570253.png" style="zoom:60%;" >

    - **Reconstruction Term**

      오토인코더의 reconstruction loss 최소화

      - reconstructio loss

        입력값 $x$가 인코더를 통해 latent space(잠재공간)으로 이동, 다시 디코더 돌아오는 과정에서 일어나는 loss

        => 이상적인 샘플링 함수로부터 **얼마나 잘 복원했는지 나타냄**

    - **Prior Fitting Term**

      입력값 $x$를 latent space에 점으로 표현 시

      => 점들이 이루는 분포, latent distribution(잠재분포)로 하여금 prior distribution 비슷해지도록 강제함

      이상적인 샘플링 함수의 값이 **최대한 prior(사전분포)와 유사한 값을 만들어내도록 condition 부여**

- **VAE 정리**

  - 목적

    가지고 있는 이미지 입력값(관측 데이터) $x$를 잘 표현할 수 있는 latent space 찾는 것

  - posterior distribution 정확히 예측 못함

    => latent space를 posterior distribution에 근사시키기 위해 variational inference 테크닉 사용

    => 오토인코더 모델: **Variational Auto-Encoder(VAE)**

  **VAE: prior disstribution와 비슷한 latent space 이루도록 새로운 이미지 $x_{new}$ 만듬**

  => Genarative model 될 수 있음

  AutoEncoder: 단순히 입력값 인코딩, latent space 보냈다가 다시 디코딩, 어떤 출력값 얻어내는 모델

  => Genarative model X

- **VAE 한계**

  - **intractable model**

    어떤 입력 주어짐 => likelihood 측정 판별 X

    => explicit 모델 X

  - **prior fitting term 미분가능**

    KL Divergence는 그 자체로 적분 들어있음, 적분이 intractable 경우 많음

    => Gaussian prior distribution 제외하고, closed form 잘 안 나옴

    - closed form: 수학적으로 유한한 term, 표현할 수 있는 항(식)

    But, SGD/Adam 같은 Optimizer 사용하려면 KL Divergence 포함한 prior fitting term 미분가능

    => 대부분 VAE: closed form 나오는 분포, Gaussian prior distribution 사용

  - **일반적으로 isotropic Gaussian 사용**
  
    모든 output dimension이 independent한 Guassian Distribution
  
    Gaussian priot distribution일 경우, 
  
    $D_{KL}(q_{\phi}(Z|x)||N(0,1))=\frac12\sum^D_{i=1}(\sigma^2_{z_i}+\mu^2_{z_i}-ln(\sigma^2_{z_i})-1)$
  
    loss function에 집어넣어 학습, 원하는 결과 나옴
  
- **Adversarial auto-encoder(AAE)**

  Gaussian Distribution이 아닌 다른 prior distribution 활용

  VAE의 prior fitting term을 GAN objective으로 바꾼 것

  <img width="819" alt="image1" src="https://user-images.githubusercontent.com/60209937/129591026-6e5c19db-b165-40f9-b8ba-da34dd11d58a.png" style="zoom:50%;" >

  - GAN을 사용해 latent 분포들 사이의 분포 맞춰줌
  - VAE보다 대부분 성능 잘 나옴

- **GAN(Generative Adversarial Network)**

  Generator 성능 높이는 것

  Generator 학습시키는 Discriminator 점점 좋아짐

  $\underset{G}{min}\underset{D}{max}V(D,G)=\mathbb{E}_{x\sim p_{data}(x)}[logD(x)]+\mathbb{E}_{z\sim p_z(z)}[log(1-D(G(z)))]$

  - 우변: loss, 생성값과 실제값의 오차

  Generator: loss 최소화 / Discriminator: loss 최대화

  <img width="1068" alt="image2" src="https://user-images.githubusercontent.com/60209937/129591035-bdb2320c-8db0-4c2d-bdd2-bd4396188c83.png" style="zoom:45%;" >

  - VAE

    - 학습과정

      입력값 $x$를 인코더를 통해 latent vector $z$로 만듬

      => $z$를 다시 디코딩, 원래 $x$로 복원시키도록 인토더와 디코더의 파라미터 학습

    - 생성과정

      latent distribution $p_{\theta}(z)$ sampling한 뒤, 디코딩하여 $x_{new}$ 출력

  - GAN

    - 학습과정

      latent distribution $z$에서 출발, generator 통해 Fake 만듬

      discriminator 기준의 레이블과 Fake 이미지 비교 판독, 분류기 학습

      Generator는 discrimator가 Fake 이미지에 대해 True 나오도록 위조기 update하여 학습

  - **GAN 목표**

    discriminator의 알고리즘 수식

    $\underset{D}{max}V(G,D)=\mathbb{E}_{x\sim p_{data}[logD(x)]+\mathbb{E}_{x\sim p_G}[log(1-D(c)]}$

    Generator가 고정, Disciminator를 optimalization(최적화)시키는 form

    $D^*_G(x)=\frac{p_{data}(x)}{p_{data}(x)+p_G(x)}$

    Generator 알고리즘: 동일한 loss에 대해, dicriminator 최대화, generator 최소화

    $\underset{G}{min}V(G,D)=\mathbb{E}_{x\sim p_{data}[logD(x)]+\mathbb{E}_{x\sim p_G}}[log(1-D(x))]$

    optimal discriminator를 generator 수식에 적용 시, 

  <img width="861" alt="image3" src="https://user-images.githubusercontent.com/60209937/129591043-52bced6d-c621-4c5b-b6af-686fb0ebc3d7.png" style="zoom:60%;" >

  => GAN 목적: **실제 데이터의 분포와 학습한 데이터의 분포 사이에 jenson-Shannon Divergene(JSD) 최소화**

  => **Disriminator가 optimal하다는 가정**에 generator 알고리즘 대입

  - discriminator가 optimal discriminator에 수렴 보장 어려움, generator 위 식처럼 전개 못함

    => 이론적 타당, 현실적 JSD 줄이는 방식 사용 어려움

  - **DCGAN(Deep Convolutional GAN)**

    GAN 모델을 이미지 도메인으로 개량

    <img width="1191" alt="image4" src="https://user-images.githubusercontent.com/60209937/129591050-98e6a9ed-5b4b-448b-80f0-eb07da188de1.png" style="zoom:45%;" >

    generator에 deconvolution 활용, discriminator에 convolution 연산 수행

    => 벡터 늘림

    알고리즘 개량 X, 하이퍼파라미터/테크닉에 대한 실험적 결과 수록

  - **Info-GAN**

    <img width="627" alt="image5" src="https://user-images.githubusercontent.com/60209937/129591054-15ebbfd7-43f3-4c63-9446-3d83e64832ab.png" style="zoom:50%;" >

    단순히 True/False 뿐만 아니라, class를 랜덤하게 같이 넣어줘서 학습

    단순하게 $z$를 이용해 이미지뿐만 아니라, $c$라는 클래스 집어넣은 방식

    - $c$라는 원-핫 벡터 제공, generator의 multi-model distribution 학습 도움

  - **Text2Image**

    <img width="985" alt="image6" src="https://user-images.githubusercontent.com/60209937/129595003-dab67242-9892-4c35-908d-8d40103bb5df.png" style="zoom:55%;" >

    문장 => 이미지 만드는 모델

    문장 입력으로 받아, Conditional GAN 통해 이미지 출력

    OpenAI에서 제공하는 DALL-E의 원조 격

  - **Puzzle-GAN**

    <img width="1131" alt="image7" src="https://user-images.githubusercontent.com/60209937/129591061-cc5a732d-8da2-40c5-9dd1-a232a5ade3f5.png" style="zoom:60%;" >

  - **CycleGAN**

    <img width="941" alt="image8" src="https://user-images.githubusercontent.com/60209937/129591066-098b2249-3856-4868-aa46-c3421d02d18b.png" style="zoom:50%;" >

    <img width="1182"  src="https://user-images.githubusercontent.com/60209937/129595681-c4d7ab85-3820-4023-adb4-4d8e6569567d.png" style="zoom:60%;" >

    이미지 사이의 도메인 바꿀 수 있는 모델

    이미지 주어지면, 이미지를 주어진 조건에 알맞은 형태로 변형

    **Cycle-consistency loss 알고리즘 활용**

    - 임의의 말 이미지들과 임의의 얼룩말 이미지 잔뜩 모아둠 => 이를 이용해 하나의 이미지를 다른 도메인 이미지로 그냥 변형시켜줌

      => **GAN 모델 2개 사용**

  - **Star-GAN**

    <img width="922" alt="image10" src="https://user-images.githubusercontent.com/60209937/129591073-330bae70-3c1c-4bbb-a98a-715fec20d669.png" style="zoom:50%;" >

    단순한 이미지를 원하는 형태로 control할 수 있게 만들어주는 모델

  - **Progressive-GAN**

    <img width="746" alt="image11" src="https://user-images.githubusercontent.com/60209937/129591078-6d78e0bc-42da-4145-88a6-569d4a41ec09.png" style="zoom:60%;" >

    고해상도 이미지 잘 만들 수 있는 GAN

    $4\times4$ 같이 작은 사이즈에서부터 시작, HD size까지 픽셀을 키워가며 해상도 점진적으로 상승(progressive)

<img width="636" alt="image12" src="https://user-images.githubusercontent.com/60209937/129591080-c3ce2fe7-320a-464f-bfac-3f8a7e7062f7.png" style="zoom: 65%;" >

