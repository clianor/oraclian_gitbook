# 웹팩 이해하기

### 모듈의 정의

모듈은 프로그램을 구성하는 내부의 코드가 기능별로 나누어져 있는 것을 뜻함.

### 번들링이란

웹팩의 모든 것은 모듈로서 이를 참조관계가 있는 파일들을 모아 하나의 파일로 만드는 것을 번들링이라고 함.

### Bundle이 중요한 이유

1. 모든 모듈을 로드하기 위해 검색하는 시간이 단축됨
2. 사용되지 않는 코드를 제거해줌. \( 참조하지 않는 코드는 제거 \)
3. 파일의 크기를 줄여준다. \( 파일들을 번들링한 뒤 압축 \)
