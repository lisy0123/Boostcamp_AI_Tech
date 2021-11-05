# Reference

mmdetection

https://mmdetection.readthedocs.io/en/latest/api.html

https://github.com/open-mmlab/mmcv

https://wordbe.tistory.com/entry/MMDetection-%EB%85%BC%EB%AC%B8-%EC%A0%95%EB%A6%AC-%EB%B0%8F-%EB%AA%A8%EB%8D%B8-%EA%B5%AC%ED%98%84

https://github.com/open-mmlab/mmdetection

save_best

https://github.com/open-mmlab/mmcv/blob/7540cf73ac7e5d1e14d0ffbd9b6759e83929ecfc/mmcv/runner/hooks/evaluation.py#L33

evaluation results

https://github.com/matterport/Mask_RCNN/issues/663

---

Cascade R-CNN

https://blog.lunit.io/2018/08/13/cascade-r-cnn-delving-into-high-quality-object-detection/

https://seol8118.github.io/object%20detection/od-cascadeRCNN/#

https://deep-learning-study.tistory.com/605

DetectoRS

https://chacha95.github.io/2020-02-14-Object-Detection2/

https://cdm98.tistory.com/49

https://hugrypiggykim.com/2021/03/26/detectors-detecting-objects-with-recursive-feature-pyramid-and-switchable-atrous-convolution/

https://www.notion.so/DetectoRS-Detecting-Objects-with-Recursive-Feature-Pyramid-and-Switchable-Atrous-Convolution-29969b0db009456e87941f27c71d7585

swin

https://deep-learning-study.tistory.com/728

https://byeongjo-kim.tistory.com/36

https://visionhong.tistory.com/31

https://github.com/microsoft/Swin-Transformer

https://github.com/SwinTransformer/Swin-Transformer-Object-Detection

---

Albumentation augmentation

https://github.com/open-mmlab/mmdetection/blob/master/mmdet/datasets/pipelines/transforms.py#L1320

https://github.com/open-mmlab/mmdetection/issues/3377

https://github.com/open-mmlab/mmdetection/pull/1354

---

Focal Loss

https://ropiens.tistory.com/83

https://deep-learning-study.tistory.com/504

https://herbwood.tistory.com/19

https://github.com/open-mmlab/mmdetection/blob/master/mmdet/models/losses/focal_loss.py#L107

IoU Loss

https://medium.com/analytics-vidhya/different-iou-losses-for-faster-and-accurate-object-detection-3345781e0bf

https://deep-learning-study.tistory.com/402

https://hongl.tistory.com/215

https://github.com/open-mmlab/mmdetection/blob/master/mmdet/models/losses/iou_loss.py#L241

GIoU Loss

https://gaussian37.github.io/vision-detection-giou/

https://melona94.tistory.com/2?category=845893

https://github.com/open-mmlab/mmdetection/blob/master/mmdet/models/losses/iou_loss.py#L358

CIoU Loss

https://github.com/open-mmlab/mmdetection/blob/master/mmdet/models/losses/iou_loss.py#L438

DIoU Loss

https://melona94.tistory.com/3

https://deep-learning-study.tistory.com/634

https://github.com/open-mmlab/mmdetection/blob/master/mmdet/models/losses/iou_loss.py#L398

Complete IoU 논문에 소개된 cluster nms를 적용

https://github.com/HikariTJU/LD/blob/main/mmdet/core/post_processing/bbox_nms.py

---

CosineAnnealing

https://sanghyu.tistory.com/113

https://ai4nlp.tistory.com/16

https://velog.io/@changdaeoh/learningrateschedule

https://github.com/open-mmlab/mmcv/blob/master/mmcv/runner/hooks/lr_updater.py#L258

CosineAnnealingWarmRestarts

https://pytorch.org/docs/stable/generated/torch.optim.lr_scheduler.CosineAnnealingWarmRestarts.html

https://gaussian37.github.io/dl-pytorch-lr_scheduler/

https://github.com/open-mmlab/mmcv/blob/master/mmcv/runner/hooks/lr_updater.py#L336

---

wandb

https://github.com/open-mmlab/mmdetection/issues/4575

https://github.com/open-mmlab/mmcv/blob/c47c9196d067a0900b7b8987a8e82768edab2fff/mmcv/runner/hooks/logger/wandb.py#L8

---

Mixed Precision

```python
git clone https://github.com/NVIDIA/apex
cd apex
pip install -v --disable-pip-version-check --no-cache-dir --global-option="--cpp_ext" --global-option="--cuda_ext" ./
```

https://hoya012.github.io/blog/Mixed-Precision-Training/

https://bo-10000.tistory.com/32

https://github.com/SwinTransformer/Swin-Transformer-Object-Detection/blob/master/mmdet/utils/optimizer.py#L9

anchor size, aspect ratio

https://www.kaggle.com/c/vinbigdata-chest-xray-abnormalities-detection/discussion/220295

---

Mosaic

https://blog.roboflow.com/yolov4-data-augmentation/

https://hoya012.github.io/blog/yolov4/

https://github.com/open-mmlab/mmdetection/blob/a1ab8d1a2278d752f875ca8e5b4791cf5b8a06b7/mmdet/datasets/pipelines/transforms.py#L1948

MixUp

https://everyday-image-processing.tistory.com/145

https://blog.airlab.re.kr/2019/11/mixup

https://github.com/open-mmlab/mmdetection/blob/a1ab8d1a2278d752f875ca8e5b4791cf5b8a06b7/mmdet/datasets/pipelines/transforms.py#L2197

MultiImageMixDataset

https://github.com/open-mmlab/mmdetection/issues/6155

https://github.com/open-mmlab/mmdetection/blob/8e89779837d65c413e8cdc67a021863e189d2010/configs/yolox/yolox_s_8x8_300e_coco.py#L29

---

Weighted boxes fusion

https://github.com/ZFTurbo/Weighted-Boxes-Fusion/issues/2

https://github.com/ZFTurbo/Weighted-Boxes-Fusion

- Non-maximum Suppression (NMS)

- Soft-NMS

  https://deep-learning-study.tistory.com/606

  https://eehoeskrap.tistory.com/407

  https://m.blog.naver.com/jws2218/222077974368

  https://github.com/open-mmlab/mmcv/blob/bf2c9fa8d21e4af903769b3d7aca20452e4b4669/mmcv/ops/nms.py#L181

- **Non-maximum weighted (NMW)**

- Weighted boxes fusion (WBF)

  https://lv99.tistory.com/74

  https://sseunghyuns.github.io/detection/2021/03/29/wheatdetection-wbf-inference/ 

---
