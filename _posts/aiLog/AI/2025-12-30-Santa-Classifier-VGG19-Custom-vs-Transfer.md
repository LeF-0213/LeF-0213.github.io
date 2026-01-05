---
layout: post
related_posts:
  - /aiLog/ai
title:  "Santa vs 일반인 이미지 분류 (VGG19 직접 구현 & 전이학습)"
date:   2025-12-24
categories:
  - ai
description: >
  VGG19 모델을 직접 구현하고, 사전 학습된 모델을 전이 학습과 성능 비교
---
* toc
{:toc .large-only}

## VGG-19 Architecture

VGG-19는 **16개의 합성공 계층(Convolutional layer)** 과 **3개의 완전 연결 층(Fully Connected layer)** 으로 구성된 **총 19개** 의 가중치 레이어를 가진 심층 컨블루션 신경망(DCNN)이다.

### 핵심 구성 요소
- **Convolution Layers**:
    - 3x3 크기의 작은 필터를 사용하며,
    - stride 1과 padding 1 설정을 통해 연산 후에도 이미지의 가로·세로 크기(공간 해상도)가 줄어들지 않고 유지되도록 설계
- **Activation Function (활성화 함수: ReLU)**:
    - 모든 합성곱층과 연결층 다음에 **ReLU(Rectified Linear Unit)** 함수 적용
- **Pooling Layers (Max Pooling)**:
    - 2x2 필터와 stride 2를 사용하는 Max Pooling 방식을 사용하여 공간 차원을 축소 
- **Fully Connected Layers (완전 연결 층: FC Layers)**:
    - 분류를 위해 네트워크 마지막 부분에 위치한 3개의 층으로,
    - 앞선 과정에서 추출된 2차원 특징 맵들을 1차원 벡터로 펼친(Flatten) 후 연결한다.
- **Softmax Layer**:
    - 네트워크의 가장 마지막에 위치하는 출력 층으로 완전 연결 층에서 나온 수치들을 0과 1 사이의 확률 값으로 변환한다.
    - 확률 값의 합은 1이며, 이를 통해 특정 클래스로 분류할 최종 확률을 출력한다.

### VGG-19의 설계 원칙
- **Uniform Convolution Filters (균일한 합성곱 필터)** :
    - 모델 전체에서 오직 **3x3 크기의 필터만 사용**
    - 필터를 두 개 쌓으면 5x5 필터 한 개와 같고, 세 개 쌓으면 7x7과 같아진다.
    - 때문에 필터 크기를 작게 고정하면 파라미터 수는 줄이면서 층을 더 깊게 쌓을 수 있어 모델의 학습 능력이 극대화 된다. 
- **Deep Architectrue (깊은 아키텍처)** :
    - 층의 개수를 19개까지 늘려 네트워크 깊이를 깊게 가져간다.
    - 네트워크가 깊어질수록 AI는 단순한 선이나 색상 단계를 넘어, 사물의 복잡한 형태나 추상적인 개념(예: 동물의 귀, 자동차 휠 등)을 단계별로 나누어 정교하게 학습할 수 있게 된다. 
- **ReLU Activation (ReLU 활성화 함수)** :
    - 모든 연산 층 뒤에 비선형 함수인 ReLU를 배치한다.
    - 활성화 함수는 신경망에 '비성형성'을 부여하여, 모델은 단순한 선형 결합으로 해결할 수 없는 복잡하고 꼬여 있는 데이터 패턴을 유연하게 학습할 수 있다. 
- **Max Pooling** :
    - 중요한 정보만 남기고 이미지 크기를 줄여나가는 것.
    - 이는 계산 속도를 높일 뿐만 아니라, 물체가 이미지 내에서 조금 이동하거나 왜곡되어도 동일하게 인식할 수 있도록 '공간적 불변성'을 제공한다.
- **Fully Connected Layers** :
    - 추출된 모든 특징을 하나로 모아 분류한다.
    - 앞선 층들이 이미지의 '특징'을 찾는 역할이었다면, FC Layers는 찾아낸 모든 조각 정보(예: 털, 꼬리, 눈 모양)를 종합하여 "이 사진은 고양이입니다." 라고 최종 결론을 내리는 판단 장치 역할을 한다. 

### 특징
- **Model Simplicity and Effectiveness (모델의 단순성과 효과성)**:
    - VGG-19의 구조가 매우 직관적이다. 모든 블록엣 일관되게 **3x3 필터** 를 사용하고, 동일한 패턴의 블록을 반복적으로 쌓아올렸다.
    - 아키텍처가 복잡하지 않아 설계와 구현이 매우 쉽고, 다양한 컴퓨터 비전 작업(이미지 분류, 객체 탐지 등)에서 기본 모델로 사용하기에 안정적이다.
- **Computational Requirements (컴퓨팅 자원 요구 사항)**:
    - VGG-19의 가장 큰 단점은 **매우 무겁다** 는 것이다.
    - 19개의 깊은 층과 수많은 필터 때문에 학습해야 할 파라미터가 약 1억 4천만개에 해당한다.
- **Robust Feature Extraction (강력한 특징 추출 능력)**:
    - 층이 매우 깊기 때문에 이미지 속의 아주 미세하고 복잡한 특징(텍스처, 사물의 부위 등)을 잡아내는 능력이 탁월하다.
    -  **(전이 학습, Transfer Learning) 활용**:
        - 이미 거대 데이터(ImageNet)로 학습된 VGG-19의 **'가중치(Weights)'** 를 가지여와서, 내가 가진 적은 양의 데이터로도 고성능 AI를 만드는 **전이 학습** 의 기초 모델로 자주 사용된다.
- **Data Augmentation (데이터 증강)**:
    - 모델이 너무 깊으면 훈련 데이터에만 과하게 익숙해지는 **'과적합'** 이 발생하기 쉽다.
    - 이를 막기 위해 데이터를 인위적으로 변형하는데 다양한 데이터 증강 기법이 사용된다.
        -  **Random Cropping**: 사진을 무작위로 자르는 기법
        -  **Horizontal Flipping**: 좌우를 반전시키는 기법
        -  **Color Jittering**: 색상을 살짝 바꾸는 기법
- **Influence on Network Design (네트워크 설계에 미친 영향)**:
    - '큰 필터 하나보다 작은 필터 여러개가 낫다'
    - '망이 깊어질수록 성능이 좋아진다'
    - 이 두 원칙은 이후 등작한 ResNet(잔차 학습)이나 Inception(병렬 구조)같은 현대적 AI 모델들이 탄생하는 밑거름이 되었으며, 현재까지도 컴퓨터 비전 연구의 중요한 기준점으로 활용되고 있다. 

<img src='https://media.geeksforgeeks.org/wp-content/uploads/20240607123924/VGG--19-Architecture-.webp' width=650px/>


```python
!pip install torchvision
```

    Requirement already satisfied: torchvision in /Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12/site-packages (0.24.1)
    Requirement already satisfied: numpy in /Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12/site-packages (from torchvision) (2.2.6)
    Requirement already satisfied: torch==2.9.1 in /Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12/site-packages (from torchvision) (2.9.1)
    Requirement already satisfied: pillow!=8.3.*,>=5.3.0 in /Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12/site-packages (from torchvision) (12.0.0)
    Requirement already satisfied: filelock in /Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12/site-packages (from torch==2.9.1->torchvision) (3.20.1)
    Requirement already satisfied: typing-extensions>=4.10.0 in /Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12/site-packages (from torch==2.9.1->torchvision) (4.15.0)
    Requirement already satisfied: setuptools in /Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12/site-packages (from torch==2.9.1->torchvision) (80.9.0)
    Requirement already satisfied: sympy>=1.13.3 in /Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12/site-packages (from torch==2.9.1->torchvision) (1.14.0)
    Requirement already satisfied: networkx>=2.5.1 in /Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12/site-packages (from torch==2.9.1->torchvision) (3.6.1)
    Requirement already satisfied: jinja2 in /Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12/site-packages (from torch==2.9.1->torchvision) (3.1.6)
    Requirement already satisfied: fsspec>=0.8.5 in /Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12/site-packages (from torch==2.9.1->torchvision) (2025.12.0)
    Requirement already satisfied: mpmath<1.4,>=1.1.0 in /Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12/site-packages (from sympy>=1.13.3->torch==2.9.1->torchvision) (1.3.0)
    Requirement already satisfied: MarkupSafe>=2.0 in /Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12/site-packages (from jinja2->torch==2.9.1->torchvision) (3.0.3)



```python
import torch
# nn: Neutral Network, 인공신경망 층(Layer)을 쌓을 때 사용
import torch.nn as nn
# optimizer: 최적화 알고리즘
import torch.optim as optim
# 모델에게 batch 사이즈만큼의 데이터를 전달
from torch.utils.data import DataLoader
# models: 이미 만들어진 모델 사용하기 위한 도구
# transforms: Resize, ToTensor, Normalize 등 데이터를 변형하기 위한 도구
from torchvision import datasets, transforms, models
import matplotlib.pyplot as plt
import numpy as np
import time
import os
os.environ['MallocStackLogging'] = '0'
```


```python
# GPU 확인
device = torch.device("mps" if torch.backends.mps.is_available() else "cpu")
print(device)
```

    mps


## VGG 직접 구현
Block 1
- Conv1_1: 64 filters, 3x3 kernel, ReLU activation
- Conv1_2: 64 filters, 3x3 kernel, ReLU activation
- Max Pooling: 2x2 filter, stride 2
  
Block 2
- Conv2_1: 128 filters, 3x3 kernel, ReLU activation
- Conv2_2: 128 filters, 3x3 kernel, ReLU activation
- Max Pooling: 2x2 filter, stride 2
  
Block 3
- Conv3_1: 256 filters, 3x3 kernel, ReLU activation
- Conv3_2: 256 filters, 3x3 kernel, ReLU activation
- Conv3_3: 256 filters, 3x3 kernel, ReLU activation
- Conv3_4: 256 filters, 3x3 kernel, ReLU activation
- Max Pooling: 2x2 filter, stride 2
  
Block 4
- Conv4_1: 512 filters, 3x3 kernel, ReLU activation
- Conv4_2: 512 filters, 3x3 kernel, ReLU activation
- Conv4_3: 512 filters, 3x3 kernel, ReLU activation
- Conv4_4: 512 filters, 3x3 kernel, ReLU activation
- Max Pooling: 2x2 filter, stride 2
  
Block 5
- Conv5_1: 512 filters, 3x3 kernel, ReLU activation
- Conv5_2: 512 filters, 3x3 kernel, ReLU activation
- Conv5_3: 512 filters, 3x3 kernel, ReLU activation
- Conv5_4: 512 filters, 3x3 kernel, ReLU activation
- Max Pooling: 2x2 filter, stride 2
  
Fully Connected Layers
- FC1: 4096 neurons, ReLU activation
- FC2: 4096 neurons, ReLU activation
- FC3: 1000 neurons, softmax activation (for 1000-class classification)


```python
class VGG19FromScratch(nn.Module):
    # num_classes는 AI가 최종적으로 분류할 정답의 개수를 의미
    # 산타인지 아닌지 이진 분류이므로 num_classes=2
    def __init__(self, num_classes=2):
        super().__init__()
        
        # Block 1
        self.conv1_1 = nn.Conv2d(3, 64, kernel_size=3, padding=1)
        self.conv1_2 = nn.Conv2d(64, 64, kernel_size=3, padding=1)
        self.pool1 = nn.MaxPool2d(kernel_size=2, stride=2)

        # Block 2
        self.conv2_1 = nn.Conv2d(64, 128, kernel_size=3, padding=1)
        self.conv2_2 = nn.Conv2d(128, 128, kernel_size=3, padding=1)
        self.pool2 = nn.MaxPool2d(kernel_size=2, stride=2)

        # Block 3
        self.conv3_1 = nn.Conv2d(128, 256, kernel_size=3, padding=1)
        self.conv3_2 = nn.Conv2d(256, 256, kernel_size=3, padding=1)
        self.conv3_3 = nn.Conv2d(256, 256, kernel_size=3, padding=1)
        self.conv3_4 = nn.Conv2d(256, 256, kernel_size=3, padding=1)
        self.pool3 = nn.MaxPool2d(kernel_size=2, stride=2)

        # Block 4
        self.conv4_1 = nn.Conv2d(256, 512, kernel_size=3, padding=1)
        self.conv4_2 = nn.Conv2d(512, 512, kernel_size=3, padding=1)
        self.conv4_3 = nn.Conv2d(512, 512, kernel_size=3, padding=1)
        self.conv4_4 = nn.Conv2d(512, 512, kernel_size=3, padding=1)
        self.pool4 = nn.MaxPool2d(kernel_size=2, stride=2)

        # Block 5
        self.conv5_1 = nn.Conv2d(512, 512, kernel_size=3, padding=1)
        self.conv5_2 = nn.Conv2d(512, 512, kernel_size=3, padding=1)
        self.conv5_3 = nn.Conv2d(512, 512, kernel_size=3, padding=1)
        self.conv5_4 = nn.Conv2d(512, 512, kernel_size=3, padding=1)
        self.pool5 = nn.MaxPool2d(kernel_size=2, stride=2)

        #ReLU
        self.relu = nn.ReLU(inplace=True)

        # FC Layers
        self.fc1 = nn.Linear(512 * 7 * 7, 4096)
        self.fc2 = nn.Linear(4096, 4096)
        self.fc3 = nn.Linear(4096, num_classes)
        self.dropout = nn.Dropout(0.5)

        self._initialize_weights()

    def forward(self, x):
        # Block 1
        x = self.relu(self.conv1_1(x))
        x = self.relu(self.conv1_2(x))
        x = self.pool1(x)
        
        # Block 2
        x = self.relu(self.conv2_1(x))
        x = self.relu(self.conv2_2(x))
        x = self.pool2(x)

        # Block 3
        x = self.relu(self.conv3_1(x))
        x = self.relu(self.conv3_2(x))
        x = self.relu(self.conv3_3(x))
        x = self.relu(self.conv3_4(x))
        x = self.pool3(x)

        # Block 4
        x = self.relu(self.conv4_1(x))
        x = self.relu(self.conv4_2(x))
        x = self.relu(self.conv4_3(x))
        x = self.relu(self.conv4_4(x))
        x = self.pool4(x)

        # Block 5
        x = self.relu(self.conv5_1(x))
        x = self.relu(self.conv5_2(x))
        x = self.relu(self.conv5_3(x))
        x = self.relu(self.conv5_4(x))
        x = self.pool5(x)

        # Flatten
        # x.shape(): [batch size, channel, height, width]
        x = x.flatten(start_dim=1)

        # FC
        x = self.relu(self.fc1(x))
        x = self.dropout(x)
        x = self.relu(self.fc2(x))
        x = self.dropout(x)
        x = self.fc3(x)

        return x

    # 가중치 초기화를 하기 위함
    def _initialize_weights(self):
        # 모델 안에 들어있는 모든 층(Conv, ReLU, Linear 등)을 하나하나 꺼내주는 역할
        for m in self.modules():
            # 꺼낸 m이 Conv2d 층인지 확인
            if isinstance(m, nn.Conv2d):
                # nn.init.kaimin_normal_(): He 초기화, 
                # 층이 깊어지면 숫자들이 너무 작아지거나 커져서 학습이 안 되는 문제(기울기 소실/ 폭발)가 생긴다.
                # 이를 방지하기 위해 수학적으로 가장 적절한 범위의 난수를 채워주는 역할을 한다.
                # mode=''fan_out': 입력 데이터가 아니라 출력 채널의 개수를 기준으로 가중치 크기를 조절하겠다라는 의미
                # nonlinearity=relu': ReLU를 쓸 테니 거기에 맞춰 계산해달라는 요청
                nn.init.kaiming_normal_(m.weight, mode='fan_out', nonlinearity='relu')
                if m.bias is not None:
                    nn.init.constant_(m.bias, 0)
            elif isinstance(m, nn.Linear):
                # 가중치를 '평균 0, 표준편차 0.01인 정규분포'에서 무작위로 뽑아 채운다.
                # 아주 작은 값들로 채워서, 모델이 처음부터 너무 강한 편견을 갖지 않고 학습을 시작하게 된다.
                nn.init.normal_(m.weight, 0, 0.01)
                nn.init.constant_(m.bias, 0)
```

## 전이학습 VGG19
[VGG19_Weights.IMAGENET1K_V1](https://docs.pytorch.org/vision/main/models/generated/torchvision.models.vgg19.html#torchvision.models.VGG19_Weights)

The inference transforms are available at VGG19_Weights.IMAGENET1K_V1.transforms and perform the following preprocessing operations: Accepts PIL.Image, batched (B, C, H, W) and single (C, H, W) image torch.Tensor objects. The images are resized to resize_size=[256] using interpolation=InterpolationMode.BILINEAR, followed by a central crop of crop_size=[224]. Finally the values are first rescaled to [0.0, 1.0] and then normalized using mean=[0.485, 0.456, 0.406] and std=[0.229, 0.224, 0.225].


```python
from torchvision.models import VGG19_Weights
```


```python
model = models.vgg19(weights=VGG19_Weights.IMAGENET1K_V1)
print(model)
```

    VGG(
      (features): Sequential(
        (0): Conv2d(3, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (1): ReLU(inplace=True)
        (2): Conv2d(64, 64, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (3): ReLU(inplace=True)
        (4): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
        (5): Conv2d(64, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (6): ReLU(inplace=True)
        (7): Conv2d(128, 128, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (8): ReLU(inplace=True)
        (9): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
        (10): Conv2d(128, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (11): ReLU(inplace=True)
        (12): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (13): ReLU(inplace=True)
        (14): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (15): ReLU(inplace=True)
        (16): Conv2d(256, 256, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (17): ReLU(inplace=True)
        (18): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
        (19): Conv2d(256, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (20): ReLU(inplace=True)
        (21): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (22): ReLU(inplace=True)
        (23): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (24): ReLU(inplace=True)
        (25): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (26): ReLU(inplace=True)
        (27): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
        (28): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (29): ReLU(inplace=True)
        (30): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (31): ReLU(inplace=True)
        (32): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (33): ReLU(inplace=True)
        (34): Conv2d(512, 512, kernel_size=(3, 3), stride=(1, 1), padding=(1, 1))
        (35): ReLU(inplace=True)
        (36): MaxPool2d(kernel_size=2, stride=2, padding=0, dilation=1, ceil_mode=False)
      )
      (avgpool): AdaptiveAvgPool2d(output_size=(7, 7))
      (classifier): Sequential(
        (0): Linear(in_features=25088, out_features=4096, bias=True)
        (1): ReLU(inplace=True)
        (2): Dropout(p=0.5, inplace=False)
        (3): Linear(in_features=4096, out_features=4096, bias=True)
        (4): ReLU(inplace=True)
        (5): Dropout(p=0.5, inplace=False)
        (6): Linear(in_features=4096, out_features=1000, bias=True)
      )
    )



```python
def VGG19Transfer(num_classes=2, freeze_features=True):
    model = models.vgg19(weights=VGG19_Weights.IMAGENET1K_V1)

    if freeze_features:
        for param in model.features.parameters():
            # requires_grad = True: 계산 결과를 누적시킴 (나중에 미분을 하기 위해서)
            # requires_grad = False: 더 이상 기울기를 업데이트 하지 않겠다. 기울기 고정
            param.requires_grad = False

    model.classifier = nn.Sequential(
        nn.Linear(512 * 7 * 7, 4096),
        nn.ReLU(inplace=True),
        nn.Dropout(0.5),
        nn.Linear(4096, 4096),
        nn.ReLU(inplace=True),
        nn.Dropout(0.5),
        nn.Linear(4096, num_classes) # 마지막만 이진 분류로 수정
    )

    return model
```

## 데이터 로딩


```python
data_transforms = {
    'train': transforms.Compose([
        transforms.Resize((224, 244)),
        transforms.RandomHorizontalFlip(),
        transforms.RandomRotation(10),
        transforms.ColorJitter(brightness=0.2, contrast=0.2),
        transforms.ToTensor(),
        transforms.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225])
    ]),
    'val': transforms.Compose([
        transforms.Resize((224, 244)),
        transforms.ToTensor(),
        transforms.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225])
    ]),
    'test': transforms.Compose([
        transforms.Resize((224, 244)),
        transforms.ToTensor(),
        transforms.Normalize(mean=[0.485, 0.456, 0.406], std=[0.229, 0.224, 0.225])
    ])
}
```


```python
data_dir = {
    'train': './data/santa/train',
    'val': './data/santa/val',
    'test': './data/santa/test'
}
```


```python
image_datasets = {
    # ImageFolder(): 폴더 안에 있는 경로를 레이블로 삼고 데이터를 가져오는 것, 폴더 순으로 레이블 0, 1 ... 지정
    # ImageFolder는 폴더 구조를 보고 자동으로 정답(Label)을 달아주는 도구
    x: datasets.ImageFolder(data_dir[x], transform=data_transforms[x])
    for x in ['train', 'val', 'test']
}
```


```python
dataloaders = {
    x: DataLoader(image_datasets[x],
                  batch_size=32,
                  shuffle=(x=='train'),
                  num_workers=2)
    for x in ['train', 'val', 'test']
}
```


```python
dataset_sizes = {x: len(image_datasets[x]) for x in ['train', 'val', 'test']}
class_names = image_datasets['train'].classes
print(f"dataset_sizes: {dataset_sizes}")
print(f"classe_names: {class_names}")
```

    dataset_sizes: {'train': 895, 'val': 267, 'test': 68}
    classe_names: ['normal', 'santa']


## 학습 함수


```python
def train_model(model, criterion, optimizer, num_epochs=20, model_name="Model"):
    model = model.to(device)
    history = {
        'train_loss': [], 'train_acc': [],
        'val_loss': [], 'val_acc': [],
        'epoch_time': []
    }

    best_val_acc = 0.0

    print(f"\n{'='*70}")
    print(f"{model_name} 학습시작")
    print(f"{'='*70}\n")

    for epoch in range(num_epochs):
        start_time = time.time()
        
        for phase in ['train', 'val']:
            if phase == 'train':
                model.train()
            else:
                model.eval()

            running_losses = 0.0
            running_accs = 0
            
            for x_batch, y_batch in dataloaders[phase]:
                x_batch, y_batch = x_batch.to(device), y_batch.to(device)

                if phase == 'train':
                    y_pred = model(x_batch)
                    _, preds = torch.max(y_pred, 1)
                    loss = criterion(y_pred, y_batch)

                    optimizer.zero_grad()
                    loss.backward()
                    optimizer.step()
                else:
                    with torch.no_grad():
                        y_pred = model(x_batch)
                        _, preds = torch.max(y_pred, 1)
                        loss = criterion(y_pred, y_batch)

                running_losses += loss.item() * x_batch.size(0)
                running_accs += torch.sum(preds == y_batch.data)

            epoch_loss = running_losses / dataset_sizes[phase]
            epoch_acc = running_accs.float() / dataset_sizes[phase] * 100

            if phase == 'train':
                history['train_loss'].append(epoch_loss)
                history['train_acc'].append(epoch_acc.item())
            else:
                history['val_loss'].append(epoch_loss)
                history['val_acc'].append(epoch_acc.item())

                # Best model 저장
                if epoch_acc > best_val_acc:
                    best_val_acc = epoch_acc
                    torch.save(model.state_dict(), f'{model_name}_best.pth')

        epoch_time = time.time() - start_time
        history['epoch_time'].append(epoch_time)

        print(f'Epoch [{epoch+1} / {num_epochs}]')
        print(f'Train Loss: {history['train_loss'][-1]:.4f} Acc: {history["train_acc"][-1]:.2f}%')
        print(f'Val Loss: {history["val_loss"][-1]:.4f} Acc: {history["val_acc"][-1]:.2f}%')
        print(f'Time: {epoch_time:.2f}s')

    print(f'{model_name} 학습 완료')
    print(f'Best Validaton Accuracy: {best_val_acc:.2f}%')

    return history
```

## 테스트 함수


```python
def test_model(model, criterion, model_name="Model"):
    model = model.to(device)
    model.eval()

    test_losses = 0.0
    test_accs = 0

    print(f"\n{'='*70}")
    print(f"{model_name} Test 시작")
    print(f"{'='*70}")

    with torch.no_grad():
        for x_batch, y_batch in dataloaders['test']:
            x_batch, y_batch = x_batch.to(device), y_batch.to(device)

            y_pred = model(x_batch)
            _, preds = torch.max(y_pred, 1)
            loss = criterion(y_pred, y_batch)

            test_losses += loss.item() * x_batch.size(0)
            test_accs += torch.sum(preds == y_batch.data)

    test_loss = test_losses / dataset_sizes['test']
    test_acc = test_accs.float() / dataset_sizes['test'] * 100

    print(f'Test Loss: {test_loss:.4f}')
    print(f'Test Accuracy: {test_acc:.2f}% ({test_accs}/{dataset_sizes['test']})')

    return test_acc.item()
```

## 학습 실행

### VGG19 직접 구현 학습


```python
model_scratch = VGG19FromScratch(num_classes=2)
# 처음부터 구현할 모델이라 학습률을 낮춰 안정적으로 기초를 쌓아가기 위함.
optimizer_scratch = optim.Adam(model_scratch.parameters(), lr=0.0001)
history_scratch = train_model(model_scratch, nn.CrossEntropyLoss(), optimizer_scratch, model_name="VGG19_Scratch")
```

    
    ======================================================================
    VGG19_Scratch 학습시작
    ======================================================================
    
    Epoch [1 / 20]
    Train Loss: 0.7066 Acc: 57.88%
    Val Loss: 0.6757 Acc: 49.81%
    Time: 85.46s
    Epoch [2 / 20]
    Train Loss: 0.5773 Acc: 67.60%
    Val Loss: 0.3747 Acc: 86.89%
    Time: 85.77s
    Epoch [3 / 20]
    Train Loss: 0.3899 Acc: 84.80%
    Val Loss: 0.3380 Acc: 87.27%
    Time: 86.19s
    Epoch [4 / 20]
    Train Loss: 0.3730 Acc: 85.81%
    Val Loss: 0.4252 Acc: 89.14%
    Time: 99.07s
    Epoch [5 / 20]
    Train Loss: 0.3408 Acc: 87.04%
    Val Loss: 0.2797 Acc: 87.27%
    Time: 116.92s
    Epoch [6 / 20]
    Train Loss: 0.2785 Acc: 87.82%
    Val Loss: 0.2017 Acc: 92.13%
    Time: 105.82s
    Epoch [7 / 20]
    Train Loss: 0.2941 Acc: 89.83%
    Val Loss: 0.4500 Acc: 77.15%
    Time: 111.79s
    Epoch [8 / 20]
    Train Loss: 0.3338 Acc: 86.26%
    Val Loss: 0.2879 Acc: 90.26%
    Time: 110.20s
    Epoch [9 / 20]
    Train Loss: 0.2910 Acc: 88.60%
    Val Loss: 0.1887 Acc: 92.51%
    Time: 87.49s
    Epoch [10 / 20]
    Train Loss: 0.2436 Acc: 89.61%
    Val Loss: 0.1992 Acc: 93.63%
    Time: 112.10s
    Epoch [11 / 20]
    Train Loss: 0.2242 Acc: 90.50%
    Val Loss: 0.2136 Acc: 92.51%
    Time: 98.13s
    Epoch [12 / 20]
    Train Loss: 0.2370 Acc: 90.17%
    Val Loss: 0.2271 Acc: 92.51%
    Time: 99.57s
    Epoch [13 / 20]
    Train Loss: 0.2081 Acc: 91.40%
    Val Loss: 0.2109 Acc: 91.76%
    Time: 97.42s
    Epoch [14 / 20]
    Train Loss: 0.2175 Acc: 91.51%
    Val Loss: 0.3358 Acc: 92.13%
    Time: 105.20s
    Epoch [15 / 20]
    Train Loss: 0.2077 Acc: 92.18%
    Val Loss: 0.1706 Acc: 93.26%
    Time: 102.27s
    Epoch [16 / 20]
    Train Loss: 0.1704 Acc: 93.63%
    Val Loss: 0.2005 Acc: 93.63%
    Time: 110.04s
    Epoch [17 / 20]
    Train Loss: 0.1776 Acc: 92.51%
    Val Loss: 0.2180 Acc: 91.01%
    Time: 115.92s
    Epoch [18 / 20]
    Train Loss: 0.3082 Acc: 86.70%
    Val Loss: 0.3792 Acc: 86.89%
    Time: 1002.46s
    Epoch [19 / 20]
    Train Loss: 0.3178 Acc: 87.26%
    Val Loss: 0.2600 Acc: 91.39%
    Time: 1065.16s
    Epoch [20 / 20]
    Train Loss: 0.2650 Acc: 89.61%
    Val Loss: 0.2313 Acc: 90.26%
    Time: 91.19s
    VGG19_Scratch 학습 완료
    Best Validaton Accuracy: 93.63%



```python
test_acc_scratch = test_model(model_scratch, nn.CrossEntropyLoss(), "VGG19_Scratch")
```

    
    ======================================================================
    VGG19_Scratch Test 시작
    ======================================================================
    Test Loss: 0.2395
    Test Accuracy: 89.71% (61/68)



```python
### VGG19 전이학습
```


```python
model_transfer = VGG19Transfer(num_classes=2, freeze_features=True)
# 이미 학습이 되어있기에 성능을 빠르게 올리기 위해 학습률을 높임.
optimizer_transfer = optim.Adam(model_transfer.parameters(), lr=0.001)
history_transfer = train_model(model_transfer, nn.CrossEntropyLoss(), optimizer_transfer, model_name="VGG19_Transfer")
```

    
    ======================================================================
    VGG19_Transfer 학습시작
    ======================================================================
    
    Epoch [1 / 20]
    Train Loss: 2.6082 Acc: 86.93%
    Val Loss: 0.3802 Acc: 84.64%
    Time: 47.07s
    Epoch [2 / 20]
    Train Loss: 0.3135 Acc: 94.08%
    Val Loss: 0.2165 Acc: 94.38%
    Time: 49.13s
    Epoch [3 / 20]
    Train Loss: 0.2009 Acc: 95.08%
    Val Loss: 0.1871 Acc: 94.01%
    Time: 48.81s
    Epoch [4 / 20]
    Train Loss: 0.0873 Acc: 97.88%
    Val Loss: 0.1394 Acc: 95.51%
    Time: 52.60s
    Epoch [5 / 20]
    Train Loss: 0.1610 Acc: 97.09%
    Val Loss: 0.2756 Acc: 96.63%
    Time: 48.50s
    Epoch [6 / 20]
    Train Loss: 0.0884 Acc: 98.66%
    Val Loss: 0.1893 Acc: 96.25%
    Time: 47.05s
    Epoch [7 / 20]
    Train Loss: 0.0531 Acc: 98.44%
    Val Loss: 0.2144 Acc: 94.76%
    Time: 47.92s
    Epoch [8 / 20]
    Train Loss: 0.0669 Acc: 98.55%
    Val Loss: 0.5348 Acc: 95.51%
    Time: 65.11s
    Epoch [9 / 20]
    Train Loss: 0.2980 Acc: 96.76%
    Val Loss: 1.8436 Acc: 87.64%
    Time: 47.88s
    Epoch [10 / 20]
    Train Loss: 0.3315 Acc: 97.54%
    Val Loss: 0.3845 Acc: 96.63%
    Time: 47.32s
    Epoch [11 / 20]
    Train Loss: 0.2270 Acc: 97.43%
    Val Loss: 0.5690 Acc: 95.13%
    Time: 47.52s
    Epoch [12 / 20]
    Train Loss: 0.1527 Acc: 98.32%
    Val Loss: 1.2531 Acc: 93.63%
    Time: 47.67s
    Epoch [13 / 20]
    Train Loss: 0.3151 Acc: 98.10%
    Val Loss: 0.6990 Acc: 96.63%
    Time: 48.29s
    Epoch [14 / 20]
    Train Loss: 0.2475 Acc: 98.55%
    Val Loss: 1.0154 Acc: 96.63%
    Time: 47.70s
    Epoch [15 / 20]
    Train Loss: 0.4922 Acc: 96.87%
    Val Loss: 1.5836 Acc: 96.63%
    Time: 47.49s
    Epoch [16 / 20]
    Train Loss: 0.0427 Acc: 99.33%
    Val Loss: 1.0250 Acc: 97.00%
    Time: 49.46s
    Epoch [17 / 20]
    Train Loss: 0.4482 Acc: 98.32%
    Val Loss: 1.9512 Acc: 95.88%
    Time: 47.10s
    Epoch [18 / 20]
    Train Loss: 0.5215 Acc: 97.99%
    Val Loss: 1.1036 Acc: 96.25%
    Time: 47.32s
    Epoch [19 / 20]
    Train Loss: 0.3625 Acc: 98.66%
    Val Loss: 1.2753 Acc: 96.25%
    Time: 47.03s
    Epoch [20 / 20]
    Train Loss: 0.1906 Acc: 98.77%
    Val Loss: 1.1849 Acc: 96.63%
    Time: 47.58s
    VGG19_Transfer 학습 완료
    Best Validaton Accuracy: 97.00%



```python
test_acc_transfer = test_model(model_transfer, nn.CrossEntropyLoss(), "VGG19_Transfer")
```

    
    ======================================================================
    VGG19_Transfer Test 시작
    ======================================================================
    Test Loss: 1.2791
    Test Accuracy: 97.06% (66/68)


## 결과 시각화


```python
fig, axes = plt.subplots(2, 2, figsize=(16, 12))

# Loss
axes[0, 0].plot(history_scratch['train_loss'], label='Scratch Train', linewidth=2)
axes[0, 0].plot(history_scratch['val_loss'], label='Scratch Val', linewidth=2)
axes[0, 0].plot(history_transfer['train_loss'], label='Transfer Train', linewidth=2, linestyle='--')
axes[0, 0].plot(history_transfer['val_loss'], label='Transfer Val', linewidth=2, linestyle='--')
axes[0, 0].set_title('Loss Comparison', fontsize=14, fontweight='bold')
axes[0, 0].set_xlabel('Epoch')
axes[0, 0].set_ylabel('Loss')
axes[0, 0].legend()
axes[0, 0].grid(True, alpha=0.3)

# Accuracy
axes[0, 1].plot(history_scratch['train_acc'], label='Scratch Train', linewidth=2)
axes[0, 1].plot(history_scratch['val_acc'], label='Scratch Val', linewidth=2)
axes[0, 1].plot(history_transfer['train_acc'], label='Transfer Train', linewidth=2, linestyle='--')
axes[0, 1].plot(history_transfer['val_acc'], label='Transfer Val', linewidth=2, linestyle='--')
axes[0, 1].set_title('Accuracy Comparison', fontsize=14, fontweight='bold')
axes[0, 1].set_xlabel('Epoch')
axes[0, 1].set_ylabel('Accuracy (%)')
axes[0, 1].legend()
axes[0, 1].grid(True, alpha=0.3)

# Training Time
epochs = range(1, len(history_scratch['epoch_time']) + 1)
axes[1, 0].bar([x - 0.2 for x in epochs], history_scratch['epoch_time'], 
               width=0.4, label='Scratch', alpha=0.8)
axes[1, 0].bar([x + 0.2 for x in epochs], history_transfer['epoch_time'], 
               width=0.4, label='Transfer', alpha=0.8)
axes[1, 0].set_title('Training Time per Epoch', fontsize=14, fontweight='bold')
axes[1, 0].set_xlabel('Epoch')
axes[1, 0].set_ylabel('Time (seconds)')
axes[1, 0].legend()
axes[1, 0].grid(True, alpha=0.3, axis='y')

# Final Performance
categories = ['Train Acc', 'Val Acc']
scratch_metrics = [history_scratch['train_acc'][-1], history_scratch['val_acc'][-1]]
transfer_metrics = [history_transfer['train_acc'][-1], history_transfer['val_acc'][-1]]

x = range(len(categories))
axes[1, 1].bar([i - 0.2 for i in x], scratch_metrics, width=0.4, label='Scratch', alpha=0.8)
axes[1, 1].bar([i + 0.2 for i in x], transfer_metrics, width=0.4, label='Transfer', alpha=0.8)
axes[1, 1].set_title('Final Performance', fontsize=14, fontweight='bold')
axes[1, 1].set_xticks(x)
axes[1, 1].set_xticklabels(categories)
axes[1, 1].legend()
axes[1, 1].grid(True, alpha=0.3, axis='y')

plt.tight_layout()
plt.savefig('vgg19_comparison.png', dpi=300, bbox_inches='tight')
plt.show()
```
 

<img width="1589" height="1190" alt="Image" src="https://github.com/user-attachments/assets/f0321fd8-7eae-47e9-aab7-add03c22e823" />
    

```python
print("\n" + "="*80)
print(" "*25 + "📊 최종 성능 비교 결과")
print("="*80)
print(f"{'Model':<20} {'Train Acc':<15} {'Val Acc':<15} {'Test Acc':<15}")
print("-"*80)
print(f"{'VGG19 직접 구현':<20} "
      f"{history_scratch['train_acc'][-1]:>12.2f}% "
      f"{history_scratch['val_acc'][-1]:>12.2f}% "
      f"{test_acc_scratch:>12.2f}%")
print(f"{'VGG19 전이학습':<20} "
      f"{history_transfer['train_acc'][-1]:>12.2f}% "
      f"{history_transfer['val_acc'][-1]:>12.2f}% "
      f"{test_acc_transfer:>12.2f}%")
print("-"*80)
print(f"{'정확도 향상':<20} "
      f"{history_transfer['train_acc'][-1] - history_scratch['train_acc'][-1]:>+11.2f}%p "
      f"{history_transfer['val_acc'][-1] - history_scratch['val_acc'][-1]:>+11.2f}%p "
      f"{test_acc_transfer - test_acc_scratch:>+11.2f}%p")
print(f"{'평균 Epoch 시간':<20} "
      f"{np.mean(history_scratch['epoch_time']):>11.2f}s "
      f"{np.mean(history_transfer['epoch_time']):>11.2f}s")
print("="*80)

```

    
    ================================================================================
                             📊 최종 성능 비교 결과
    ================================================================================
    Model                Train Acc       Val Acc         Test Acc       
    --------------------------------------------------------------------------------
    VGG19 직접 구현                 89.61%        90.26%        89.71%
    VGG19 전이학습                  98.77%        96.63%        97.06%
    --------------------------------------------------------------------------------
    정확도 향상                     +9.16%p       +6.37%p       +7.35%p
    평균 Epoch 시간               194.41s       48.93s
    ================================================================================


## 실제 예측 결과 시각화


```python
dataloaders['test'] = DataLoader(image_datasets['test'],
                  batch_size=32,
                  shuffle=True,
                  num_workers=2)
```


```python
def visualize_predictions(model, images, labels, model_name='Model', num_images=16):
    model = model.to(device)
    model.eval()

    # 예측
    images = images.to(device)
    with torch.no_grad():
        pred = model(images)
        _, preds = torch.max(pred, 1)

    # CPU로 이동 
    # (matplotlib, PIL, cv2 같은 라이브러리는 GPU 데이터를 직접 읽지 못하기 때문)
    images = images.cpu()
    preds = preds.cpu()
    labels = labels.cpu()

    # 시각화
    fig = plt.figure(figsize=(16, 8))
    fig.suptitle(f'{model_name} - Prediction Result', fontsize=20, fontweight='bold')

    num_images = min(num_images, len(images))

    for idx in range(num_images):
        ax = plt.subplot(4, 4, idx + 1)

        # 이미지 denormalize
        # matplotlib로 그림을 그리려면 NumPy 형태여야 한다.
        # transpose: 차원 순서 변경(C, H, W -> H, W, C)
        img = images[idx].numpy().transpose((1, 2, 0))
        mean = np.array([0.485, 0.456, 0.406])
        std = np.array([0.229, 0.224, 0.225])
        # 정규화 공식 역순 ((Raw - mean) / std)
        img = std * img + mean
        # clip(): 0~1 사이로 고정
        img = np.clip(img, 0, 1)

        ax.imshow(img)

        # 예측 결과
        pred_label = class_names[preds[idx]]
        true_label = class_names[labels[idx]]

        # 맞으면 초록색 틀리면 빨간색
        color = 'green' if preds[idx] == labels[idx] else 'red'

        ax.set_title(f'Predicted: {pred_label}\nActual: {true_label}',
                    color=color, fontsize=10, fontweight='bold')
        ax.axis('off')

    plt.tight_layout()
    plt.savefig(f'{model_name}_predictions.png', dpi=150, bbox_inches='tight')
    plt.show()
```


```python
print("\n" + "="*70)
print("실제 예측 결과 시각화")
print("="*70 + "\n")

# Test 데이터에서 랜덤으로 가져오기
dataiter = iter(dataloaders['test'])
images, labels = next(dataiter)

print("VGG19 직접 구현 모델의 예측")
visualize_predictions(model_scratch, images, labels, "VGG19_Scratch", num_images=16)
print("VGG19 전이학습 모델의 예측")
visualize_predictions(model_transfer, images, labels, "VGG19_Transfer", num_images=16)
```

    
    ======================================================================
    실제 예측 결과 시각화
    ======================================================================
    
    VGG19 직접 구현 모델의 예측
    
<img width="1262" height="788" alt="Image" src="https://github.com/user-attachments/assets/37a31d59-ad11-42f6-b85a-f645025bb707" />

    VGG19 전이학습 모델의 예측
    
<img width="1262" height="788" alt="Image" src="https://github.com/user-attachments/assets/0a6566b4-9540-4bcd-a5e4-a8ea042d7aaa" />
    
## 혼동행렬(Confusion Matrix) 시각화


```python
from sklearn.metrics import confusion_matrix, classification_report
import seaborn as sns

def plot_confusion_matrix(model, model_name="Model"):
    model = model.to(device)
    model.eval()

    all_preds = []
    all_labels = []

    with torch.no_grad():
        for inputs, labels in dataloaders['test']:
            inputs = inputs.to(device)
            outputs = model(inputs)
            _, preds = torch.max(outputs, 1)

            all_preds.extend(preds.cpu().numpy())
            all_labels.extend(labels.cpu().numpy())

    # Confusion Matrix
    cm = confusion_matrix(all_labels, all_preds)

    # 시각화
    plt.figure(figsize=(8, 6))
    sns.heatmap(cm, annot=True, # 각 셀에 숫자 표기
                fmt='d', # 숫자를 정수(decimal) 형식으로 표기
                cmap='Blues',
                xticklabels=class_names, 
                yticklabels=class_names,
                cbar_kws={'label': 'Count'} # 컬러바에 'Count' 라벨 추가
               )
    
    plt.title(f'{model_name} - Confusion Matrix', fontsize=16, fontweight='bold')
    plt.ylabel('Actual (True Label)', fontsize=12)
    plt.xlabel('Predicted (Predicted Label)', fontsize=12)
    plt.tight_layout()
    plt.savefig(f'{model_name}_confusion_matrix.png', dpi=150, bbox_inches='tight')
    plt.show()

    # Classification Report
    print(f"\n{model_name} - Classification Report:")
    print("="*70)
    print(classification_report(all_labels, all_preds, target_names=class_names))
    print("="*70 + "\n")
```


```python
print("\n" + "="*70)
print("혼동 행렬 (Confusion Matrix)")
print("="*70 + "\n")

print("VGG19 직접 구현")
plot_confusion_matrix(model_scratch, "VGG19_Scratch")

print("VGG19 전이학습")
plot_confusion_matrix(model_transfer, "VGG19_Transfer")
```

    
    ======================================================================
    혼동 행렬 (Confusion Matrix)
    ======================================================================
    
    VGG19 직접 구현

<img width="753" height="590" alt="Image" src="https://github.com/user-attachments/assets/4586c7eb-317a-4a15-b6c3-2a52907a3e0c" />
        
    VGG19_Scratch - Classification Report:
    ======================================================================
                  precision    recall  f1-score   support
    
          normal       0.86      0.94      0.90        32
           santa       0.94      0.86      0.90        36
    
        accuracy                           0.90        68
       macro avg       0.90      0.90      0.90        68
    weighted avg       0.90      0.90      0.90        68
    
    ======================================================================
    
    VGG19 전이학습

<img width="753" height="590" alt="Image" src="https://github.com/user-attachments/assets/1f231f9e-8008-4efb-84cd-185fb54177d4" />
        
    VGG19_Transfer - Classification Report:
    ======================================================================
                  precision    recall  f1-score   support
    
          normal       0.97      0.97      0.97        32
           santa       0.97      0.97      0.97        36
    
        accuracy                           0.97        68
       macro avg       0.97      0.97      0.97        68
    weighted avg       0.97      0.97      0.97        68
    
    ======================================================================
    
