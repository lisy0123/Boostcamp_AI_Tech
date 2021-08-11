# MLP (Multi-Layer Perceptron)

- Neural Networks

  - Computing systems vaguely inspired by the biological neural networks that constitute animal brains

  - **Function approximators that stack affine transformations followed by nonlinear transformations.**

- Linear Neural Networks

  - Data: $\mathcal{D}=\left\{\left(x_{i}, y_{i}\right)\right\}_{i=1}^{N}$
  - Model: $\hat{u}=w x+b$
  - Loss: $\operatorname{loss}=\frac{1}{N} \sum_{i=1}^{N}\left(y_{i}-\hat{y}_{i}\right)^{2}$

  $\begin{aligned}
  \frac{\partial \operatorname{loss}}{\partial w} &=\frac{\partial}{\partial w} \frac{1}{N} \sum_{i=1}^{N}\left(y_{i}-\hat{y}_{i}\right)^{2} \\
  &=\frac{\partial}{\partial w} \frac{1}{N} \sum_{i=1}^{N}\left(y_{i}-w x_{i}-b\right)^{2} \\
  &=\frac{1}{N} \sum_{i=1}^{N}-2\left(y_{i}-w x_{i}-b\right) x_{i}
  \end{aligned}$​

  $\begin{aligned}
  \frac{\partial \operatorname{loss}}{\partial b} &=\frac{\partial}{\partial b} \frac{1}{N} \sum_{i=1}^{N}\left(y_{i}-\hat{y}_{i}\right)^{2} \\
  &=\frac{\partial}{\partial b} \frac{1}{N} \sum_{i=1}^{N}\left(y_{i}-w x_{i}-b\right)^{2} \\
  &=\frac{1}{N} \sum_{i=1}^{N}-2\left(y_{i}-w x_{i}-b\right)
  \end{aligned}$​

  $w \leftarrow w-\eta \frac{\partial \operatorname{loss}}{\partial w}$

  $b \leftarrow b-\eta \frac{\partial l o s s}{\partial b}$​

  - $\eta$ (Stepsize) 적절하게 잡는 것 중요

  <img src="https://user-images.githubusercontent.com/60209937/128656028-8146a913-75cc-4b6e-94e2-cb7ad1519bcb.png" alt="01" style="zoom:80%;" />

  $y=W_{3}^{T} h_{2}=W_{3}^{T} \rho\left(W_{2}^{T} X h_{1}\right)=W_{3}^{T} \rho\left(W_{2}^{T} \rho\left(W_{1}^{T} X\right)\right)$

  - Non-linear transform ($\rho$​​​), activation functions(활성함수)이 들어야 네트워크를 깊게 쌓았을 때 의미 있음

- Activation functions

  - ReLU (Rectified Linear Unit)
  - sigmoid
  - tanh (Hyperbolic Tangent)

- Universal approximation theory

  - 히든 레이어가 1개 있는 신경망의 표현력은 일반적인 continuous function들을 포함함
  - 존재성에 대한 증명 (어떻게 찾는지까지에 대한 이론은 아님)

- Loss functions

  - Regression Task

    $ \mathrm{MSE}=\frac{1}{N} \sum_{i=1}^{N} \sum_{d=1}^{D}\left(y_{i}^{(d)}-\hat{y}_{i}^{(d)}\right)^{2}$​

  - Classification Task

    $\mathrm{CE}=-\frac{1}{N} \sum_{i=1}^{N} \sum_{d=1}^{D} y_{i}^{(d)} \log \hat{y}_{i}^{(d)}$​

  - Probabilistic Task

    $\mathrm{MLE}=\frac{1}{N} \sum_{i=1}^{N} \sum_{d=1}^{D} \log \mathcal{N}\left(y_{i}^{(d)} ; \hat{y}_{i}^{(d)}, 1\right) \quad(=\mathrm{MSE})$​​

