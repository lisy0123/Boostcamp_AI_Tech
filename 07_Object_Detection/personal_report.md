# Image Classification

## :one: EDA

- train 사진 수: 9754장
- 10 class : General trash, Paper, Paper pack, Metal, Glass, Plastic, Styrofoam, Plastic bag, Battery, Clothing

- images
  - id: 파일 안에서 image 고유 id, ex) 1
  - 이미지 크기: (1024, 1024)
  - filename: ex) train/002.jpg
- annotations
  - id: 파일 안에 annotation 고유 id
  - bbox: 객체가 존재하는 박스의 좌표 (xmin, ymin, w, h)
  - area: 객체가 존재하는 박스의 크기
  - category_id: 객체가 해당하는 class의 id
  - image_id: annotation이 표시된 이미지 고유 id

- Evaluation: mAP50

![EDA01](https://user-images.githubusercontent.com/60209937/135449907-d98f887c-3cb1-45da-8942-5b318bea6dcd.png)

- 데이터 불균형
- bbox가 1개인 imgs가 많다.
- 


## :two: Labeling



## :three: Data augmentation

Mosaic

- 이미지 4장을 각각 무작위로 잘라서 하나의 사진으로 만드는 augmentation.
- cutmix와의 차이점은 cutmix는 자른 사진이 다른 사진을 가리는 구조에 반해, 4장의 랜덤하게 자른 사진은 서로 겹치지 않는다는 점이다



## :four: Testing

| Architecture/backbone/Neck                              | Val_mAP50 | LB_mAP50  | optimizer                                              | class_loss, bbox_loss   | batch_size, epochs |      ETC       |
| ------------------------------------------------------- | :-------: | :-------: | :----------------------------------------------------- | :---------------------- | :----------------- | :------------: |
| Faster R-CNN / ResNet50 / FPN                           |           |   0.428   | SGD, StepLR, lr=2e-2                                   | CE, L1Loss              | 2, 12              |      [1]       |
| **Cascade R-CNN** / ResNet50 / FPN                      |   0.617   |   0.349   | SGD, StepLR, lr=1e-3                                   | CE, SmoothL1Loss        | 4, 37              |                |
| **Cascade R-CNN** / ResNet101 / FPN                     |   0.425   |   0.250   | SGD, StepLR, lr=1e-3                                   | CE, SmoothL1Loss        | 4, 36              |                |
| **Cascade R-CNN** / ResNet50 / FPN                      |   0.715   | **0.521** | SGD, **CosineAnnealing**, lr=1e-3 ~ 5e-6               | CE, SmoothL1Loss        | 4, 36              | [2], 이후 적용 |
| Cascade R-CNN / ResNet50 / **RFP+SAC**  **(DetectoRS)** |   0.810   | **0.559** | SGD, CosineAnnealing, lr=1e-3 ~ 5e-6                   | CE, SmoothL1Loss        | 4, 36              |                |
| Cascade R-CNN / **Swin base** / FPN                     |   0.620   |   0.540   | **AdamW, CosineAnnealingWarmRestarts, lr=1e-5 ~ 5e-6** | **FocalLoss, DIoULoss** | 4, 12*             |      [3]       |
| Cascade R-CNN / **Swin large** / FPN                    |           |           | AdamW, CosineAnnealingWarmRestarts, **lr=5e-5 ~ 5e-6** | FocalLoss, DIoULoss     | 4, 48              |      [4]       |
| Cascade R-CNN / Swin large / FPN                        |           |           | AdamW, CosineAnnealingWarmRestarts, **lr=3e-5 ~ 5e-6** | FocalLoss, DIoULoss     | 4, 48              |      [5]       |
|                                                         |           |           |                                                        |                         |                    |                |

**[1] Baseline**

- Data argument: Resize(1024, 1024), RandomFlip(p=0.5), Normalize

**[2] Data argument**

- RandomRotate90, GaussNoise(p=0.5), Resize(1024, 1024), RandomFlip(p=0.5), Normalize
- Blur, MedianBLur
- RandomBrightnessContrast, CLAHE, RandomGamma
- HueSatuationValue, RGBShift

**[3] overfitting 대폭 감소**

- soft_nms 적용

- 36epoch으로 설정했으나, OutOfMemory로 12 epoch 밖에 안 돔

**[4]**

- [Swin Transformer object detection](https://github.com/SwinTransformer/Swin-Transformer-Object-Detection)에 맞춰 mmdetection 2.11.0으로 버전 변경, 맞춰서 코드 수정
- score_thr=0.0
- soft_nms 적용
- anchor aspect_ratio=[0.5, 1.0, 2.0] => [0.33, 1.0, 2.0, 3.0]

**[5] Data argument 추가**

- Multi-size training (384, 384), (512, 512), (768, 768), (1024, 1024)

- TTA with Multi-size images

- 효율적인 학습을 위해 Mixed Precision (NVIDIA apex) 사용

  > Mosaic, MixUp은 mmdetection 2.11.0에서 제공 안 함

## :six: Furthermore...





[↩️ Go Back](https://github.com/lisy0123/Boostcamp_AI_Tech)
