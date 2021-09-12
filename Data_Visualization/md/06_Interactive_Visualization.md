# Interactive Visualization

- **정적 시각화 단점**

  공간적 낭비 큼

  각 사용자가 원하는 인사이트 다를 수 있음

  원하는 메시지를 압축해서 담는 것 => 정적 시각화 장점

- 종류
  - **Select** : mark something as interesting
  - **Explore** : show me something else
  - **Reconfigure** : show me a different arrangement
  - **Encode** : show me a different representation
  - **Abstract** : show me more or less detail
  - **Filter** : show me something conditionally
  - **Connect** : show me related items

- 대표 라이브러리

  문법, 제공 방법 차이 있음

  **Plotly**

  **Bokeh**

  **Altair**

- Matplotlib도 인터랙티브 제공

  => 주피터 노트북 환경/local에서만 실행 가능

  - 다른 라이브러리: 웹에 deploy 가능
  - `mpld3` 사용, 웹에서 D3-based Viewer 제공

- **Plotly**

  인터랙티브 시각화에서 가장 많이 사용

  Python, R, JS에서도 제공

  예시, 문서화 잘되어 있음

  통계, 지리, 3D, 금융, 등 다양한 시각화 기능 제공

  JS 시각화 라이브러리 D3js 기반, 웹에서 사용 가능

  형광 color 인상적

  - **Plotly Express**

    Plotly를 seaborn과 유사, 쉬운 문법

    커스텀 부분 부족, but 다양한 함수 제공

- **Bokeh**

   문법은 Matplotlib과 더 유사

  기본 Theme: Plotly에 비해 깔끔

  비교적 부족한 문서화

- **Altair**

  Vega 라이브러리 사용해 만든 인터랙티브

  시각화를 + 연산 등으로 배치

  문법: Pythonic하지 않음, js스러움

  데이터 크기에 5000개 제한

  Bar, Line, Scatter, Histogram에 특화

