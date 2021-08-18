# AutoGrad & Optimizer

- **torch.nn.Module**

  딥러닝 구성하는 layer의 base class

  Input, Output, Forward, Backward 정의

  학습 대상이 되는 parameter(tensor) 정의

  <img width="1008" alt="Screen Shot 2021-08-18 at 2 04 21 PM" src="https://user-images.githubusercontent.com/60209937/129840240-5afee5af-6204-4beb-bf42-2c9e77b2178e.png" style="zoom:40%;" >

- **nn.Parameter**

  Tensor 객체의 상속 객체

  nn.Module 내에 atrribute가 될 때, required_grad=True 지정되어 학습 대상이 되는 Tensor

  우리가 직접 지정할 일은 잘 없음

  => 대부분은 layer, weights 값들이 지정되어 있음

  ```python
  class MyLiner(nn.Module):
    def __init__(self, in_features, out_features, bias=True):
      super().__init__()
      self.in_features = in_features
      self.out_features = out_features
  		self.weights = nn.Parameter( torch.randn(in_features, out_features))
      self.bias = nn.Parameter(torch.randn(out_features))
    
    def forward(self, x : Tensor):
    	return x @ self.weights + self.bias
  ```

- **Backward**

  layer에 있는 parameter들의 미분 수행

  forward의 결과값 (model의 output=예측치) & 실제값 간의 차이(loss)에 대해 미분 수행

  해당 값으로 parameter 업데이트

  ```python
  for epoch in range(epochs):
    ......
    optimizer.zero_grad() # 이전 grad 영향 안 주게
    outputs = model(inputs) # y^
    loss = criterion(outputs, labels) print(loss) # get loss for the predicted output
  	loss.backward() # get gradients w.r.t to parameters
  	optimizer.step() # update parameters
  	.........
  ```

- **Backward form the scratch**

  실제 backward, module 단계에서 직접 지정 가능

  Module에서 backward, optimize 오버라이딩

  직접 미분 수식 작성 필요 => 쓸 일 없음, 순서 이해 필요

  <img width="979" alt="Screen Shot 2021-08-18 at 2 59 21 PM" src="https://user-images.githubusercontent.com/60209937/129845607-bffeed1c-3502-4644-b4f8-2b6140963edc.png" style="zoom:80%;" >

