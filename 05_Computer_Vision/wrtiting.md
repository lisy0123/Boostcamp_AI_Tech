K Nearest Neighbors (k-NN)

분류기

backbone network: CNN

fully connected reural net

locally connected reural net

image-level classification

classification + regression

pixel-level classification



CNN architectures

AlexNet 2012

VGGNet 2014

GoogLeNet 2015

ResNet 2016

Beyond ResNet 



LeNet-5 1998

Conv - Pool - Conv - Pool - FC - FC

Conv 2개 FC 2개



AlexNet

Bigger, ImageNet

ReLU, dropout



hard label

soft label



distillation loss: KLdiv 주로 사용

student loss: crossentropy

둘의 wieghted sum 사용



semi-supervised learning

pseudo-labeled dataset



self-training

nosiy student trainig



GoogLeNet

inception module

1 X 1 convolutions

steam network

stacked inception modules

auxiliary classifiers: vanishing 문제 해결

- fc 2Ro, 1 X 1 Conv
- 학습 도중에만 사용
- 최종 결과만 사용

classifier ouput (a single FC layer)



ResNet

layer deeps가 중요

depradation problem

- Not caused by overfitting
- reseon is optimization

shortcut connection

residual block



DenseNet

concatenate 신호 보존, 채널 늘어남, 피쳐 내용 그대로 보존

dense blocks

channel asix



SENet

Squeeze 각 채널축의 분포

Excitation 채널값의 연관성, attention score

rescaling



EfficientNet

width scaling, depth scaling, resolution scaling, compound scaling



Deformable convolution



주로, VGGNet, ResNet 사용



잔차



semantic segmentation

픽셀 단위로 구분

영상 속 마스크 생성

영상 콘텐츠 이해하는데 이용

유저 인터페이스 제공



FCN (Fully Convolutional Networks)

모두 미분 가능한 뉴렬 네트워크

학습을 통해 문제 풀이 가능

임의의 사이즈 영상도 가능, 호환성 높음



fully connected layer

fully convolutional layer : 1 by 1 convolutions



FCN

알렉스넷 : flattening, full;y-conected

하나의 백터로 섞음

각 위치마다 flaettening

flattening 위치마다 백터 한개씩 나옴

1 by 1 convolution 

FCN 레이어의 한 컬럼

내적

웨이트 공유

1 by 1 conv

어떤 이미지에도 대응 가능

작은 스코어맵을 얻음

넓은 콘텍트를 고려, 더 정답에 가까운 결론

저해상도 회피 => upsampling

리셉티브 필드 작음, 전반적인 영상 파악 못함

리셉티브 최대한 키워놓음

Transposed convolution: 

upsampling : 학습 가능한 업샘플링릏 한 번에 

인터폴리이션들

nearest-neighbor(nn), bilinear

일괄 적용

해상도 줄어듬, 장버 살리기 어려움

국지적, 디테일, 민감

해상도 낮아짐, 글로벌, 의미론적



높은 레이어, 업샘플링, 해상도 올림

다른 모델



u-net

입력, pooling, 해상도 낮춤, 채널 늘림

decoding upsampling

해상도, 처널 늘림

대칭되는 특징들을

expanding path

채널 사이즈 줄음, 해상도 늘음, 컴퓨터네이션 이용

해상도, 채널 호환

공간적으로 높은 해상도, 민감한 

바로 전달 중요한 역할

주의 사이즈 홀수를 가질 경우

downsample, updampling

원래랑 차이, 홀수는 쓰지 말기

테스트, 레이어 확인, 에러 미연에 확인

conv 두번, double conv

채널 늘리는 방향

expending path

ConvTransposed2d



 DeepLab

CRFs 컨셉

픽셀 사이의 관계 이어줌, 경계 잘 찾도록



dilated convolution

파라미터 안 늘림

receptive field

연산 줄이기, 

depthwise 채널별로 conv 값을 뽑음, 하나의 값에 출력되게 합쳐줌

