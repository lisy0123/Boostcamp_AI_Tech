# Intro

<img width="1165" alt="image1" src="https://user-images.githubusercontent.com/60209937/129653988-5a05f654-ba7f-4103-b850-52245b2cb004.png" style="zoom:75%;" >

- **Computational Graph**

  $g=(x+y)*z$

  <img width="634" alt="image2" src="https://user-images.githubusercontent.com/60209937/129654002-edd7fd50-4080-4e67-82c7-1381e5f467ae.png" style="zoom:50%;" >

  - **Define and Run - TensorFlow**

    그래프 먼저 정의 => 실행시점에 데이터 feed

  - **Define by Run (Dynamic Computational Graph, DCG) - PyTorch**

    실행을 하면서 그래프를 생성하는 방식

<img width="1218" alt="image3" src="https://user-images.githubusercontent.com/60209937/129654004-cceb76ed-7709-4e5e-908f-9425ab303c23.png" style="zoom:75%;" >

- **Degine by Run 장점**

  즉시 확인 가능 => pythonic code

  사용 편함

  GPU 잘 지원, good API and community

- TensorFlow: production, scalability 장점

- **PyTorch: Numpy + AutoGrad + Function**

  Numpy 구조를 가지는 Tensor 객체로 array 표현

  자동미분 지원, DL 연산 지원

  다양한 형태의 DL 지원하는 함수와 모델 지원