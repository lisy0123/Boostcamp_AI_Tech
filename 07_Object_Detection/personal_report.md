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

- Data argument: Resize(1024, 1024), RandomFlip(p=0.5), Normalize

| Architecture/backbone/Neck          | mAP50                 | optimizer             | loss (class, bbox) | batch_size, epochs | ETC                                    |
| ----------------------------------- | --------------------- | --------------------- | ------------------ | ------------------ | -------------------------------------- |
| Faster R-CNN / ResNet50 / FPN       | LB: 0.428             | SGD, StepLR, lr=0.02  | CE, L1Loss         | 2, 12              | baseline에 있는 data argument   그대로 |
| **Cascade R-CNN** / ResNet50 / FPN  | Val: 0.6--, LB: 0.349 | SGD, StepLR, lr=0.001 | CE, SmoothL1Loss   | 4, 37              |                                        |
| **Cascade R-CNN** / ResNet101 / FPN | Val: 0.4--, LB: 0.250 | SGD, StepLR, lr=0.001 | CE, SmoothL1Loss   | 4, 36              |                                        |

---

- Data argument
  - RandomRotate90, GaussNoise(p=0.5), Resize(1024, 1024), RandomFlip(p=0.5), Normalize
  - Blur, MedianBLur
  - RandomBrightnessContrast, CLAHE, RandomGamma
  - HueSatuationValue, RGBShift

| Architecture/backbone/Neck                                   | mAP50                 | optimizer                          | loss (class, bbox) | batch_size, epochs | ETC                                      |
| ------------------------------------------------------------ | --------------------- | ---------------------------------- | ------------------ | ------------------ | ---------------------------------------- |
| **Cascade R-CNN** / ResNet50 / FPN                           | Val: 0.715, LB: 0.521 | SGD, **CosineAnnealing**, lr=0.001 | CE, SmoothL1Loss   | 4, 36              | Data argument 추가                       |
| **Cascade R-CNN** / ResNet50 / **RFP+SAC**  **(DetectoRS)**  | Val: , LB:            | SGD, **CosineAnnealing**, lr=0.001 | CE, SmoothL1Loss   | 4, 36              |                                          |
| **Cascade R-CNN** / ResNet101 / **RFP+SAC**  **(DetectoRS)** + | Val: , LB:            | SGD, **CosineAnnealing**, lr=0.001 | CE, SmoothL1Loss   | 4, 36              |                                          |
|                                                              |                       |                                    |                    |                    |                                          |
|                                                              |                       |                                    |                    |                    |                                          |
|                                                              |                       |                                    |                    |                    | Stratified Group k-Fold Cross-Validation |



Data argument

- RandomRotate90, GaussNoise(p=0.5), Resize(1024, 1024), RandomFlip(p=0.5), Normalize

- Blur, MedianBLur
- RandomBrightnessContrast, CLAHE, RandomGamma
- HueSatuationValue, RGBShift

MMDetection 논문 참고

- **Hybrid Task Cascade** : 가장 높은 성능을 보인 모델

Swin Tran- + HTC 조합





## :six: Furthermore...





[↩️ Go Back](https://github.com/lisy0123/Boostcamp_AI_Tech)
