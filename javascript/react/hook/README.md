# Hook

## 리액트 Hook이란?

React 16.8부터 새로 추가된 기능으로 class를 작성하지 않고도 state나 props와 같은 리액트의 기능들을 함수 컴포넌트에서도 사용할 수 있게 해주는 기능입니다.

리액트 팀에서 Hook을 만들게 된 이유는 아래와 같다고합니다.

1. 컴포넌트 사이에서 상태와 관련된 로직을 재사용하기 어렵다.
2. 복잡한 컴포넌트들을 이해하기 어렵다.
3. class는 사람과 기계를 혼동시킨다.

이러한 이유들로 인해서 리액트 팀에서는 Hook을 만들게 되었고 Hook 에는 아래의 API를 가지고 있습니다.

기본 Hook  
  - useState  
  - useEffect  
  - useContext  
  
추가 Hooks  
  - useReducer  
  - useCallback  
  - useMemo  
  - useRef  
  - useImperativeHandle  
  - useLayoutEffect  
  - useDebugValue

다음 게시글부터는 기본 Hook부터 순서대로 보겠습니다.

