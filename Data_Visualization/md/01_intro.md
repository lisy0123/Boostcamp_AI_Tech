# Intro

- 데이터 시각화

  데이터를 그래픽 요소를 매핑하여 시각적으로 표현하는 것

  - 목적, 독자, 데이터, 스토리, 방법, 디자인

- 데이터

  - 정형 데이터

    테이블 형태로 제공되는 데이터, csv, tsv

    통계적 특성, feature 사이 관계, 데이터 간 관계 및 비교

  - 시계열 데이터

    시간 흐름에 따른 데이터

    기온, 주가, 정형데이터 & 음성, 비디오, 비정형 데이터 존재

    추세(Trend), 계절성(Seasonality), 주기성(Cycle) 살핌

  - 지리/지도 데이터

    정보 간의 조화 중요, 지도 정보 단순화

    거리, 경로, 분포 등 다양한 실사용

  - 관계 데이터

    객체와 객체 간의 관계 시각화

    객체 - Node, 관계 - Link / 크기, 색, 수 등 객체와 관계의 가중치 표현

    휴리스틱하게 노드 배치 구성

  - 계층적 데이터

    포함관계가 분명한 데이터, 네트워크 시각화로도 표현 가능

    Tree, Treemap, Sunburst

  - **수치형(numerical)**

    - **연속형(continuous)**: 길이, 무게, 온도
    - **이산형(discrete)**: 주사위 눈금, 횟수, 사람 수

  - **범주형**

    - **명목형(nominal)**: 혈액형, 종교
    - **순서형(ordinal)**: 학년, 등급

- 마크

  A mask is a basic graphical element in an inage

  점선면으로 이루어진 데이터 시각화

- 채널

  A visual channel is a way to control the appearance of marks, independent of the dimensionality of the geometric primitive.

  각 마크를 변경할 수 있는 요소들

- 전주의적 속성(Pre-attentive Attribute)

  <img src="https://user-images.githubusercontent.com/60209937/128663551-bdc752f6-49be-4cde-86d0-37ba20dde116.png" alt="1" style="zoom:80%;" />

  주의를 주지 않아도 인지하게 되는 요소

  - 시각적으로 다양한 전주의적 속성 존재

  동시에 사용하면 인지하기 어려움

  - 적절하게 사용할 때, 시각적 분리(**visual pop-out**)(colour)

- **Matplotlib**

```python
fig = plt.figure()
ax = fig.add_subplot(111) 
ax.plot([1, 1, 1], label='1') 
ax.plot([2, 2, 2], label='2') 
ax.plot([3, 3, 3], label='3')

# fig.suptitle('fig')
ax.set_title('Basic Plot')
ax.set_xticks([0, 1, 2])
ax.set_xticklabels(['zero', 'one', 'two'])

ax.annotate(s='This is Annotate', xy=(1, 2),
           xytext=(1.2, 2.2), 
            arrowprops=dict(facecolor='black'),
           )

ax.legend()
plt.show()
```

<img src="https://user-images.githubusercontent.com/60209937/128665710-d28fdb46-e188-4769-ad64-0eabba632d36.png" alt="1" style="zoom:80%;" />
