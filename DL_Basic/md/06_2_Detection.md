# Detection

classification, localization 문제

이미지 내 관심 있는 객체의 위치(Region Of Interest, ROI)를 Bounding Box를 그려 localization

다수의 Bounding Box를 다양한 object로 classification

- **R-CNN**

  <img src="https://user-images.githubusercontent.com/60209937/129054284-7e99ec0a-38ed-4c48-bc79-fc303b8a9426.png"  style="zoom:75%;" />

  Multi-stage Detector

  object 위치 찾는 Region Proposal & object 분류하는 Region Classfication

  - **Region Proposal**

    object 위치 찾는 단계, Selective Search로 약 2천개 후보 영역 추출

    추출된 후보영역의 사이즈, 해당 사이즈의 이미지로 warping

  - **CNN**

    약 2천개의 warped image, 각각 CNN에 넣어 feature 추출

    fixed-length feature vector(4096-dimensional feature vector) 추출

  - **SVM(Support Vector Machine) & Bounding Box Regression**

    추출된 fixed-length feature vector 활용해 object 분류

    - CNN fine-tunning을 위한 학습데이터가 시기상 많지 않음, Softmax 적용 => 오히려 성능 낮아짐 => SVM 사용

    BBox Regressor로 정확하지 않은 bounding box 위치 조정

  - 성능 뛰어남
  - But, 시간 오래 걸림, 복잡, End-to-end learning 불가능

- **SPPNet (Spatial Pyramid Pooling Network)**

  <img src="https://user-images.githubusercontent.com/60209937/129048454-ab4f1e86-c3ba-48cd-ad1c-a728be33c502.png" alt="image2" style="zoom:85%;" />

  <img src="https://user-images.githubusercontent.com/60209937/129055399-db08afd4-f84f-4e20-beed-af8f6e80fc26.png" style="zoom:75%;" />

  - CNN 문제점

    - 2천번의 연산

    - 입력 사이즈 $227\times227$​ 고정 => warped image 넣음 => image distortion 발생

  R-CNN과 같지만, bounding box에 해당하는 텐서만 떼와서 학습

  (CNN을 2천번이 아닌 1번 돌림)

  - **Multi-stage Detector**

    구조 R_CNN와 다름

    But, 3가지 pipeline(CNN, SVM, BBox Regressor)으로 이루어진 Multi-stage detector은 같음

  - **Reduce CNN Operation**

    입력 이미지에 대해 CNN 연산을 먼저 적용, feature map에 기반한 region propoal 과정 거침

    => **CNN 연산 1번**

  - **Spatial Pyramid Pooling**

    Max pooling 연산

    ROI feature size 에 따라 kernel size 와 stride 만 설정 시, 다양한 resolution을 설정할 수 있음

    => warping 작업이 필요가 없음, 다양한 resolution 가짐

    => Spatial Pyramid Pooling

  - R-CNN보다 소요시간 더 빠름

  - But, 복잡, end-to-end learning 불가능

- **Fast R-CNN**

  <img src="https://user-images.githubusercontent.com/60209937/129048460-f367f98f-23fc-4555-80b6-a51f4ab1cf03.png" alt="image3" style="zoom:67%;" />

  SPPNet과 거의 비슷하지만, 뒷단에 neural net (ROI feature vector 단)이 쓰임

  - **ROI Pooling Layer**

    $7\times7$​ single level spatial bin을 사용하여, over-fitting 문제 피함

  - **Softmax Classifier**

    SVM classifier가 아닌 Softmax classifier 이용

  - **Multi-task loss**

    하나의 loss fucntion을 학습하여 End-to-End learning 가능

  - **Truncated SVD**

    SVD(Single Value Decomposition)로 compression하여 파라미터 수 줄여, 소요시간 줄임

- **Faster R-CNN**

  <img src="https://user-images.githubusercontent.com/60209937/129056799-12c8b39b-6104-41d9-96bb-c898dcdcae9d.png" alt="6" style="zoom:75%;" />

  - region proposal, 즉 bounding box를 뽑는 것도 학습으로 해결

  - R-CNN + RPN

  - **RPN(Region proposal network)**

    <img src="https://user-images.githubusercontent.com/60209937/129057634-e3b1adf6-7e06-4a31-8efb-43ba4ad6b1bc.png" alt="Screen Shot 2021-08-12 at 12 25 25 AM" style="zoom:70%;" />

    ZFNet 기준의 그림이어서 256-d 으로 표현되었지만 VGG-16의 경우 512-d의 feature map 출력

  - **anchor box**

    미리 정해 둔 bounding box의 크기 (템플릿)

    detection box with predefined sizes

  - **fully conv**

    <img width="689" src="https://user-images.githubusercontent.com/60209937/129057225-f701ca6e-6675-4841-9a5f-cb214cba8ac1.png" style="zoom:55%;" >

    (3 region sizes * 3 ratios) * (4 bounding box params + 2 box classification(yes/no)) = 54

  <img src="https://user-images.githubusercontent.com/60209937/129057124-a61132c0-441d-4944-b6f9-c70a1b02ad3b.png" alt="Screen Shot 2021-08-12 at 12 22 21 AM" style="zoom:80%;" />

- **YOLO** (v1)

  <img src="https://user-images.githubusercontent.com/60209937/129048472-99552b73-02a1-4b71-99d0-f2f51d50017e.png" alt="image6" style="zoom:80%;" />

  <img src="https://user-images.githubusercontent.com/60209937/129048474-7f46db6d-149c-46ac-9942-abf0d063de9c.png" alt="image7" style="zoom:85%;" />

  - You Only Look Once: 이미지 한 장에서 바로 output 출력

  - **Faster R-CNN보다 훨씬 빠름** 

    localization(바운딩 박스 찾는 것), classfication(클래스 찾는 것) 동시에 진행

  - no explicit bounding box sampling

  - output tensor

    **(그리드의 셀 개수) * (바운딩박스 offset 5개 + 클래스 개수)**