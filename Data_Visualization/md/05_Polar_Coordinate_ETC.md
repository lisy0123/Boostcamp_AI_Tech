# Polar Coordinate

- **Polar Plot**

  극 좌표계 사용하는 시각

  거리(R), 각($\theta$) 사용

  회전, 수기성 등 표현에 적합

  projection = polar 추가하여 사용

  line, bar 모두 가능

  직교 좌표계 X, Y에서 변환 가능

  - X = R cos $\theta$
  - Y = R sin $\theta$

- **Radar Plot**

  극 좌표계에서 가장 대표적인 차트

  별 모양, star plot으로도 불림

  중심점을 기준으로 N개의 변수 값 표현

  데이터 quality 표현하기 좋음, 비교 적합

  - 주의점

    - 각 feature 독립적, 척도 같아야 함

      순서형 변수/수치형 변수 함께 존재 시, 고려 필요

    - feature의 순서에 따라 다각형 면적 많이 달라짐

    - feature 많아짐 => 가독성 떨어짐

---

# Pie Charts

- 전체 백분위 나타낼 때 유용

- 많이 사용, but 지양

  - 비교 어려움
  - 유용성 떨어짐
  - bar plot가 더 유용

- **Donut Chart**

  중간이 빈 pie chart

  디자인적으로 선호

  인포그래픽에서 종종 사용

  plotly에서 쉽게 사용 가능

- **Sunburst Chart**

  햇살(sunburst) 닯은 chart

  계층적 데이터 시각화

  - 구현 난이도에 비해 화려
  - 이것보다 treemap 추천!
  - plotly로 쉽게 사용 가능

---

# Missingno

결측치(missing value) 시각화

결측치 체크하는 시각화 라이브러리

빠르게 결측치 부포 확인 시, 사용 가능

`pip install missingno`

# Treemap

계층적 데이터, 직사각형으로 포함 관계 표현한 시각화

사각형 분할, 타일링 알고리즘, 형태 다양ㅇ

**모자이크 플롯(Mosaic plot)**(큰 사각형을 분할하여 전체를 나타냄)과도 유사

`pip install squarify`

plotly의 treemap 사용

# Waffle Chart

와플 형태, discrete하게 값 나타냄

기본: 정사각형, 원하는 백터 이미지 사용 가능

`pip install pywaffle`

icon 활용한 waffle chart 가능

인포그래픽에서 유용

# Venn

집합(set) 등에서 사용하는 벤 다이어그램

- 출판, 프레젠테이션에 사용
- draw.io나 ppt에 비해 어려움

`pip install pyvenn`

`pip install matplotlib-venn`



