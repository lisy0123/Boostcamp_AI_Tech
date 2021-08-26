이미지를 crop해서 사용하시는 분들도 많으실 텐데요, facenet을 이용해 이미지를 crop하는 방법을 이용해봤습니다.

MTCNN으로 face detection해서 crop하고, 인식 못하는 부분은 (300, 300)으로 직접 crop했습니다.



## Code

```python
import torch
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from facenet_pytorch import MTCNN
import os, cv2
```



```python
mtcnn = MTCNN(keep_all=True, device=device)
new_img_dir = './input/data/train/new_imgs'
img_path = './input/data/train/images'


cnt = 0

for paths in os.listdir(img_path):
    if paths[0] == '.':continue
    
    sub_dir = os.path.join(img_path, paths)
    
    for imgs in os.listdir(sub_dir):
        if imgs[0] == '.':continue
        
        img_dir = os.path.join(sub_dir, imgs)
        img = cv2.imread(img_dir)
        img = cv2.cvtColor(img,cv2.COLOR_BGR2RGB)
        
        #mtcnn 적용
        boxes,probs = mtcnn.detect(img)
        
        # boxes 확인
        if len(probs) > 1: 
            print(boxes)
        if not isinstance(boxes, np.ndarray):
            print('Nope!')
            # 직접 crop
            img=img[100:400, 50:350, :]
        
        # boexes size 확인
        else:
            xmin = int(boxes[0,0])-30
            ymin = int(boxes[0,1])-30
            xmax = int(boxes[0,2])+30
            ymax = int(boxes[0,3])+30
            
            if xmin < 0: xmin = 0
            if ymin < 0: ymin = 0
            if xmax > 384: xmax = 384
            if ymax > 512: ymax = 512
            
            img = img[ymin:ymax, xmin:xmax, :]
            
        tmp = os.path.join(new_img_dir, paths)
        cnt += 1
        plt.imsave(os.path.join(tmp, imgs), img)
        
print(cnt)
```



이렇게 나온 이미지들을 Resize(224, 244)로 사이즈 통일시켜 사용했습니다.

eval의 이미지들도 동일하게 path만 바꿔서 적용해줬습니다.



## Result

저 같은 경우, EfficientNet, data augmentation, k-fold 베이스에 crop 안 한 이미지로 돌렸을 때, accuracy 75.905, f1 0.695 나왔고,

crop한 이미지로 돌렸을 때, accuracy 78.206, f1 0.726 나왔습니다.

이미지 crop하고 안 하고의 성능 차이가 꽤 있으니, 아직 crop 안해보신 분들은 꼭 해보시길 추천드립니다!





## References

**face detection**

- https://github.com/timesler/facenet-pytorch
- https://seongkyun.github.io/study/2019/03/25/face_detection/
- https://sefiks.com/2020/09/09/deep-face-detection-with-mtcnn-in-python/
- https://blog.naver.com/zxc1552916/221957018715
- https://yeomko.tistory.com/16

