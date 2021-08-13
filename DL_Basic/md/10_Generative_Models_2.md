# Generative Models 2

- **VAE (Variational Auto-Encoder)**

  - **Variational inference**

    - 목적: observation 주어짐 => 관심 있는 random variables의 확률 분포, 즉 posterior dist.를 찾는 것
    - 방법: optimize(최적화, 근사) the **variational dist.** that best matches the posterior dist.
    - loss function: KL divergence
    - **ELBO (Evidence Lower BOund)**
      - Objective는 알 수 없음 => tractable한 ELBO 값을 증가하는 방향으로 
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

:label: Updating...
