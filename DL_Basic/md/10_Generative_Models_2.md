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
        - likelihood(가능도, 우도): $p_{\theta}(x|z)$

      - variational distribution: $q_{\theta}(z|x)$

        일반적, variational inference 정확히 구하는 것 거의 불가능

        => variational distribution 구해 사후분포에 근사하게 최적화

        실제 사후분포와의 KL Divergence(쿨백 라이블러 발산) 최소화되는 varitional distribution 찾기

      <img width="634" src="https://user-images.githubusercontent.com/60209937/129579805-0b7401d4-5847-4039-a128-0b1abfbe200c.png" style="zoom:60%;" >

    - **ELBO (Evidence Lower BOund)**

      <img width="1049" src="https://user-images.githubusercontent.com/60209937/129580016-692434c9-9875-4187-afba-4b151255e2cf.png" style="zoom:70%;" >

      $p_{\theta}(D)=\text{ELBO}+D_{KL}$

      variational inference는 ELBO항과 KL Divergence 항의 합으로 유도

      **ELBO법**: KL Divergence를 최소화하기 위해 반대로 ELBO항을 최대화시키는 것

      => ELBO항은 tractable(구할 수 있음) 위 방법 가능

      => Sandwitch method라도 부름

      <img width="884"  src="https://user-images.githubusercontent.com/60209937/129580180-bbe5f1ff-5073-4969-a559-71ecdd570253.png" style="zoom:65%;" >

      - Reconstruction Term

        오토인코더의 reconstruction loss 최소화

        - reconstructio loss: 입력값 $x$가 인코더를 통해 latent space(잠재공간)으로 이동, 다시 디코더 돌아오는 

      - Prior Fitting Term

      - **Decompose ELBO**

    - Reconstruction term: minimize the reconstruction loss of an auto-encoder

    - Prior fitting term: enforce the latent dist. to be similar to the prior dist.

  - **VAE 한계**

    - intractable model

      implicit, hard to evaluate likelihood

    - prior fitting term must be differential(미분가능)

      => 대부분 가우시안 prior를 사용

  - **Adversarial auto-encoder**

    - GAN을 사용해 latent 분포들 사이의 분포 맞춰줌
    - VAE보다 대부분 성능 잘 나옴

- **Generative Adversarial Network**

  - two player minimax game bw Generator and Discriminator

    two Jenson-Shannon Divergence 최소화

  - **DCGAN**

    - image domain으로 활용 (Deep Convolutional GAN)

  - **Info-GAN**

    - 단순히 True/False 뿐만 아니라, class를 랜덤하게 같이 넣어줘서 학습

  - **Text2Image**

  - **Puzzle-GAN**

  - **CycleGAN**

    - cycle-consistency loss
    - 이미지에서 두 도메인을 서로 바꿀 수 있음

  - **Star-GAN**

  - **Progressive-GAN**
