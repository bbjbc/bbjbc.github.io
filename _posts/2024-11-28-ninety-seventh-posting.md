---
title: "[김민태의 데브캠프] 애니메이션 라이브러리 없이 styled-component로 바텀시트 형태의 모달 구현하기"
categories: [dev-camp, react]
tags:
  [
    FE,
    부트캠프,
    패스트캠퍼스,
    패스트캠퍼스데브캠프,
    프론트엔드,
    김민태,
    2기,
    김민태의 데브캠프,
    데브캠프 후기,
    styled-component,
    모달,
  ]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 아흔 일곱 번째 포스팅

안녕하세요! 아흔 일곱 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **[김민태의 데브캠프] 애니메이션 라이브러리 없이 styled-component로 바텀시트 형태의 모달 구현하기**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 바텀시트 형태의 모달

모바일 앱이나 웹앱을 사용하다 보면 화면 하단에서 올라오는 인터페이스를 자주 마주치게 된다. 이것이 바로 **'바텀 시트(Bottom Sheet)'**이다.

바텀시트와 모달 형태의 바텀시트의 차이점을 구글맵과 인스타그램 사례를 통해 설명하려고 한다.

## 🍁 Standard Bottom Sheet

**구글맵**의 바텀시트는 스탠다드 바텀시트의 대표적인 예시이다.

![googlemap](https://github.com/user-attachments/assets/6fdd1bb2-32fd-430d-b9c1-8840047442ef)

장소를 검색하면 하단에서 정보가 올라오지만, 사용자는 여전히 지도와 상호작용할 수 있다. 이는 몇 가지 특징을 가진다.

1. 멀티태스킹 가능: 바텀시트를 열어둔 채로 지도를 확대/축소하거나 움직일 수 있다.
2. 유동적인 높이: 사용자가 드래그해서 높이 조절이 가능하며 조금 보임/중간/전체의 형태로 조절된다.
3. 지속적인 정보 제공: 사용자가 다른 작업을 하면서도 계속해서 정보를 참고할 수 있다.

## 🍁 Modal Bottom Sheet

**인스타그램의 댓글창**이 모달 바텀시트의 대표적인 예시이다.

![instagram](https://github.com/user-attachments/assets/44cd096a-6732-40e4-97e5-7b1e2616245c)

댓글 아이콘을 탭하면 아래에서 올라오는 모달 형태가 구현이 된다. 이도 몇 가지 특징을 가진다.

1. 집중된 상호작용: 모달이 열리면 배경이 어두워지고, 메인 컨텐츠와의 상호작용이 차단된다.
2. 명확한 계층구조: 모달이 최상위 레이어로 표시되어 사용자의 주의를 집중시킨다.
3. 강제적 선택: 모달을 직접 내리거나, 닫거나 종료해야만 다른 작업이 가능하다.

## 🍁 사용 시나리오?

> 🍃 **스탠다드 바텀시트는 이런 경우에 적합하다?**

- 지속적인 정보 표시가 필요한 경우 (지도 앱의 장소 정보)
- 백그라운드 컨텐츠와의 상호작용이 필요한 경우 (음악 플레이어)

> 🍃 **모달 형태의 바텀시트는 이런 경우에 적합하다?**

- 사용자의 입력이 필요한 경우(댓글 작성, 일정 관리)
- 작업의 완료가 필요한 경우
- 중요한 선택이 필요한 경우

위와 같은 경우에 따라 사용하는 방식이 다를 것이다.

---

# 2️⃣ 프로젝트에 적용해야 적합한 것

이번 프로젝트의 주제는 **근무 일정 관리 및 급여 정보 시스템**이다.

![image](https://github.com/user-attachments/assets/1bc980d8-5f45-48a9-89f5-338669b6ffb1)

우리 팀은 모바일 퍼스트의 웹애플리케이션을 제작할 예정이었고, 캘린더 내부에 근무 일정과 급여 관련 정보를 효과적으로 표시해야 했다.

캘린더의 셀을 터치하면 일정을 추가할 수 있는 모달창이 필요했는데, 모바일 사용성을 고려하여 하단에서 올라오는 형태의 모달을 구현하고자 했다.

바텀시트의 두 가지 유형인 스탠다드 바텀시트와 모달 바텀시트 중에서 선택해야 했는데, 일정 추가라는 작업의 특성상 **모달 바텀시트가 더 적합**했다. 일정 추가 시에는 **사용자의 집중이 필요**하고, **정확한 정보 입력**이 중요하기 때문이다.

인스타그램의 댓글창이 모바일 환경에서 모달 바텀시트를 효과적으로 활용하는 좋은 사례였기에, 이를 참고하여 구현하기로 결정했다.

---

# 3️⃣ 모달 형태의 바텀시트 컴포넌트 제작기

우선 나는 [**[motion 애니메이션 라이브러리]**](https://motion.dev/)를 사용하려고 했다. 하지만, 규모가 작은 프로젝트이며 애니메이션 사용할 곳이 많이 없지 않냐는 의견에 팀원들의 말에 수긍을 하며 직접 제작하기로 했다.

바텀 시트 모달을 React와 styled-components를 통해 구현했다. 드래그 기능과 스크롤 관리까지 신경 써서 제작했다.

## ❄️ 작업 목표

먼저 **작업 목표**는 아래와 같았다.

- 아래에서 위로 올라오는 애니메이션을 구현한다.
- 드래그로 모달을 내릴 수 있도록 한다.
- 오버레이 클릭 시 닫혀야 한다.
- 모달이 열려있을 때 배경 스크롤 방지한다.
- React Portal을 사용하여 모달을 body에 직접 마운트한다.

## ❄️ 모달 컴포넌트 구조

모달은 크게 다섯 가지 주요 컴포넌트 구성되어 있다.

![image](https://github.com/user-attachments/assets/0aac8772-e3e2-48fb-8138-e0e58da4391e)

1. **ModalOverlay**

   - 전체 화면을 덮는 반투명한 배경 레이어
   - 모달의 최상위 컨테이너 역할
   - 드래그 거리에 따라 배경 투명도가 동적으로 변화

2. **ModalContainer**

   - 실제 모달 창을 감싸는 컨테이너
   - 드래그 동작과 슬라이드 애니메이션을 처리
   - 하단에서 위로 올라오는 슬라이드 모션 구현

3. **ModalHeader**

   - 모달 상단의 헤더 영역
   - 둥근 모서리 처리로 바텀 시트임을 시각적으로 표현
   - 하단에 border로 구분선 추가

4. **SwipeIndicator (ModalIndicator)**

   - 상단 중앙의 가로 막대 형태의 스와이프 인디케이터
   - 사용자에게 드래그 가능함을 직관적으로 알림
   - 모달을 드래그할 수 있는 핸들 역할

5. **ModalContent**

   - 실제 컨텐츠가 들어가는 영역
   - overflow-y: auto로 스크롤 가능하도록 구현
   - 컨텐츠의 높이에 따라 유동적으로 조절

## ❄️ 주요 기능 구현

### ⛄️ 바텀시트 모달의 기본 구현

먼저 바텀시트 모달의 가장 기본적인 동작부터 시작했다. 모달은 하단에서 올라오는 형태로 구현했으며, 이를 위해 styled-components의 keyframes를 활용했다.

모달이 열릴 때는 하단에서 위로 올라오는 슬라이드 애니메이션이 적용되고, 닫힐 때는 다시 아래로 내려가는 애니메이션이 적용된다.

기본적으로 모달은 ReactDOM의 `createPortal`을 사용해 구현했다. Portal을 사용함으로써 모달을 DOM 트리의 최상단에 렌더링할 수 있어, `z-index`나 이벤트 버블링 제어를 용이하게 할 수 있다.

```jsx
return ReactDOM.createPortal(
  <S.ModalOverlay>
    <S.ModalContainer>
      {/* 모달 내용 */}
    </S.ModalContainer>
  </S.ModalOverlay>,
  modalRoot as HTMLElement
);
```

### ⛄️ 애니메이션 처리

모달의 열림/닫힘 애니메이션은 keyframes와 transition을 조합해 구현했고, 닫히는 애니메이션은 `isClosing` 상태를 활용해 처리했다.

```jsx
const closeAnimation = useCallback(() => {
  setIsClosing(true);
  setTranslateY(window.innerHeight); // 화면 높이만큼 아래로 이동
  setTimeout(onClose, 200); // 애니메이션 완료 후 실제 모달 닫기
}, [onClose]);
```

닫기 애니메이션이 실행될 때는 `translateY` 값을 화면 높이만큼 설정해 모달이 아래로 사라지도록 만들고, 200ms 후 모달을 닫도록 했다.

이 시간은 transition 시간과 동일하게 설정되어 애니메이션이 완료된 후 모달이 사라진다.

### ⛄️ 드래그 기능 구현

모달의 드래그 기능은 `useDrag`라는 커스텀 훅으로 분리해 구현했다. 드래그 기능은 **드래그 시작, 이동 종료**의 3가지의 단계로 나뉜다.

> ⚡️ **상태 관리**

```jsx
const [translateY, setTranslateY] = useState(0); // 모달의 현재 y축 위치
const [isClosing, setIsClosing] = useState(false); // 모달 닫힘 상태
const startPosition = useRef(0); // 드래그 시작 위치
const isDragging = useRef(false); // 현재 드래그 중인지 여부
```

위와 같이 관리하였고 `startPosition`과 `isDragging`은 `useRef`를 사용하는데 이는 드래그 중 발생하는 상태 변경으로 인한 불필요한 리렌더링을 방지하기 위함이다.

> ⚡️ **드래그 시작**

```jsx
const handleDragStart = useCallback((clientY: number) => {
  startPosition.current = clientY;
  isDragging.current = true;
}, []);
```

드래그가 시작될 때는 현재 터치/마우스 포인터의 y좌표를 저장한다.

> ⚡️ **드래그 중 처리**

```jsx
const handleDragMove = useCallback((clientY: number) => {
  if (!isDragging.current) return;

  const diff = clientY - startPosition.current;
  // 아래 방향으로만 드래그 가능
  if (diff > 0) {
    setTranslateY(diff);
  }
}, []);
```

드래그 중에는 시작 위치와 현재 위치의 차이를 계산해 모달의 위치를 업데이트한다.

여기서 `diff > 0`을 통해 아래 방향으로만 드래그가 가능하도록 제한했다.

> ⚡️ **드래그 종료 처리**

```jsx
const handleDragEnd = useCallback(() => {
  if (!isDragging.current) return;

  isDragging.current = false;
  if (translateY > DRAG_CLOSE_THRESHOLD) {
    closeAnimation(); // 임계값을 넘으면 모달 닫기
  } else {
    setTranslateY(0); // 그렇지 않으면 원위치로
  }
}, [closeAnimation, translateY]);
```

여기서 `DRAG_CLOSE_THRESHOLD`는 모달을 닫기 위한 최소 드래그 거리를 뜻한다. 이 값을 넘어서면 모달이 닫히고, 아니라면 다시 원위치로 돌아간다.

> ⚡️ **터치/마우스 이벤트 통합**

```jsx
const handlers: DragHandlers = {
  onTouchStart: (e) => handleDragStart(e.touches[0].clientY),
  onTouchMove: (e) => handleDragMove(e.touches[0].clientY),
  onTouchEnd: handleDragEnd,
  onMouseDown: (e) => handleDragStart(e.clientY),
  onMouseMove: (e) => handleDragMove(e.clientY),
  onMouseUp: handleDragEnd,
};
```

모바일 퍼스트 웹애플리케이션으로 터치와 마우스 이벤트를 모두 통합해 처리한다.

> ⚡️ **브라우저 이탈 처리**

```jsx
useEffect(() => {
  const handleMouseLeave = handlers.onMouseUp;
  document.addEventListener("mouseleave", handleMouseLeave);
  return () => document.removeEventListener("mouseleave", handleMouseLeave);
}, [handlers.onMouseUp]);
```

드래그 중 마우스를 브라우저 밖으로 이동시킨다면 마우스를 뗀 것으로 간주되어 모달이 작동하지 않을 것이다.

## ❄️ 모달 컴포넌트 사용 예시

```jsx
const App = () => {
  const [isModalOpen, setIsModalOpen] = useState(false);

  const toggleModal = () => {
    setIsModalOpen(!isModalOpen);
  };

  return (
    <div>
      <button onClick={toggleModal}>모달 열기</button>
      <Modal isOpen={isModalOpen} onClose={toggleModal}>
        <p>모달 내용입니다.</p>
      </Modal>
    </div>
  );
};
```

위와 같이 사용하고자 하는 페이지에서 사용하면 된다.

## ❄️ 전체 코드

> ⚡️ **Modal/index.tsx**

```jsx
type ModalProps = {
  isOpen: boolean;
  onClose: () => void;
  children: React.ReactNode;
};

const Modal = ({ isOpen, onClose, children }: ModalProps) => {
  const { translateY, isClosing, handlers, closeAnimation } = useDrag({
    onClose,
  });
  const modalRoot = document.getElementById('modal-overlay');

  // 모달이 열릴 때 스크롤 방지
  const preventScroll = () => {
    const currentScrollY = window.scrollY;
    document.body.style.position = 'fixed';
    document.body.style.width = '100%';
    document.body.style.top = `-${currentScrollY}px`;
    document.body.style.overflow = 'scroll';
    return currentScrollY;
  };

  // 모달이 닫힐 때 스크롤 허용
  const allowScroll = (scrollY: number) => {
    document.body.style.position = '';
    document.body.style.width = '';
    document.body.style.top = '';
    document.body.style.overflow = '';
    window.scrollTo(0, scrollY);
  };

  useEffect(() => {
    if (isOpen) {
      const scrollY = preventScroll();
      return () => allowScroll(scrollY);
    }
  }, [isOpen]);

  useEffect(() => {
    const handleMouseLeave = handlers.onMouseUp;
    document.addEventListener('mouseleave', handleMouseLeave);
    return () => document.removeEventListener('mouseleave', handleMouseLeave);
  }, [handlers.onMouseUp]);

  if (!isOpen) return null;

  return ReactDOM.createPortal(
    <S.ModalOverlay
      $closing={isClosing}
      $translateY={translateY}
      onClick={closeAnimation}
    >
      <S.ModalContainer
        id="modal-container"
        onClick={e => e.stopPropagation()}
        $translateY={translateY}
        $closing={isClosing}
        {...handlers}
      >
        <S.ModalHeader>
          <S.SwipeIndicator />
        </S.ModalHeader>
        <S.ModalContent>{children}</S.ModalContent>
      </S.ModalContainer>
    </S.ModalOverlay>,
    modalRoot as HTMLElement,
  );
};

export default Modal;
```

> ⚡️ **hooks/useDrag.ts**

```jsx
type UseDragProps = {
  onClose: () => void,
};

const useDrag = ({ onClose }: UseDragProps) => {
  const [translateY, setTranslateY] = useState(0); // 모달의 y축 위치값
  const [isClosing, setIsClosing] = useState(false); // 모달이 닫히는 중인지 여부
  const startPosition = useRef(0); // 드래그 시작 지점
  const isDragging = useRef(false); // 드래그 중인지 여부

  const closeAnimation = useCallback(() => {
    setIsClosing(true);
    setTranslateY(window.innerHeight); // 화면 아래로 내림
    setTimeout(onClose, 200); // 애니메이션이 끝나면 모달을 닫음
  }, [onClose]);

  const handleDragStart = useCallback((clientY: number) => {
    startPosition.current = clientY;
    isDragging.current = true;
  }, []);

  const handleDragMove = useCallback((clientY: number) => {
    if (!isDragging.current) return;

    const diff = clientY - startPosition.current;
    if (diff > 0) {
      setTranslateY(diff);
    }
  }, []);

  const handleDragEnd = useCallback(() => {
    if (!isDragging.current) return;

    isDragging.current = false;
    if (translateY > DRAG_CLOSE_THRESHOLD) {
      closeAnimation();
    } else {
      setTranslateY(0); // 취소 시에만 원위치로
    }
  }, [closeAnimation, translateY]);

  useEffect(() => {
    setTranslateY(0);
    setIsClosing(false);
  }, [onClose]);

  const handlers: DragHandlers = {
    onTouchStart: (e) => handleDragStart(e.touches[0].clientY),
    onTouchMove: (e) => handleDragMove(e.touches[0].clientY),
    onTouchEnd: handleDragEnd,
    onMouseDown: (e) => handleDragStart(e.clientY),
    onMouseMove: (e) => handleDragMove(e.clientY),
    onMouseUp: handleDragEnd,
  };

  return { translateY, isClosing, handlers, closeAnimation };
};

export { useDrag };
```

![modalmodal](https://github.com/user-attachments/assets/ae4f26f7-4a2a-44ac-991f-5dadf0935d80)

위는 구현 화면이다. 드래그와 애니메이션 모두 잘 작동하는 것을 볼 수 있다.

---

# 4️⃣ 트러블 슈팅1- 드래그에 따른 무한 리렌더링

## 🌊 문제 발견

처음 바텀시트 모달을 구현하고 나서, 드래그 동작을 테스트하던 중 개발자 도구에서 수많은 `warning` 메시지를 마주하게 됐다. 왜 그러지?

![image](https://github.com/user-attachments/assets/d08c9f35-4325-4ae0-b5ec-cc05aabffae4)

warning 파티였다.....

문제는 `styled-components`에서 props를 통한 동적 스타일링 방식에 있었다.

```jsx
export const ModalContainer = styled.div<{
  $translateY: number;
  $closing: boolean;
}>`
  transform: translateY(${(props) => props.$translateY}px);
  // ... 기타 스타일
`;
```

이 방식에서 드래그할 때마다 `$translateY` 값이 변경되면서 새로운 클래스가 생성되고 있었다.

![not-attrs](https://github.com/user-attachments/assets/fdda5691-e279-4c49-b5d7-d35685866493)

개발자 도구를 통해 확인해보니 드래그 동작마다 클래스 이름이 계속해서 변경되는 것을 발견할 수 있다.

## 🌊 attrs를 활용한 해결

styled-components의 `attrs`를 사용하면 이 문제를 깔끔하게 해결할 수 있었다.

```jsx
export const ModalContainer = styled.div.attrs<{
  $translateY: number;
  $closing: boolean;
}>(props => ({
  style: {
    transform: `translateY(${props.$translateY}px)`,
  },
}))`
  // ... 기타 스타일
`;
```

![use-attrs](https://github.com/user-attachments/assets/495d79a2-9ecf-4207-9ac7-9c3152a3ddcc)

`attrs`를 사용한 후 개발자 도구를 확인해보니, 더 이상 클래스 이름이 변경되지 않고 대신 `style` 속성값만 업데이트되는 것을 확인할 수 있었다. 이는 성능 측면에서 큰 차이를 만들어낸다.

## 🌊 왜 이런 차이가 발생하지?

styled-components에서 일반적으로 props를 사용해 스타일을 동적으로 사용하면 props가 변경될 때마다 새로운 스타일이 생성되고 컴포넌트가 리렌더링된다고 한다.

하지만 `attrs`를 사용하면 style 객체를 직접 조작하기 때문에, React의 Virtual DOM 단계를 거치지 않고 실제 DOM의 style 속성만 업데이트된다고 한다.

특히 드래그나 뭐 빈번하게 값이 변하는 경우에는 `attrs`를 사용하는 것이 효율적이다❗️

---

# 5️⃣ 트러블 슈팅2- 모달에서 스크롤 막기

## ☀️ 기존 구현의 문제점

처음에는 단순히 모달이 열릴 때 스크롤을 막는 방식을 사용했다.

```jsx
useEffect(() => {
  if (isOpen) {
    document.body.style.overflow = "hidden";
  }
  return () => {
    document.body.style.overflow = "unset";
  };
}, [isOpen]);
```

하지만 위 방식은 모달이 열리고 닫힐 때 스크롤바가 사라지며 화면이 흔들리는 현상이 발생했다. 이는 UX상 매우 안좋다고 생각하여 변경하고자 했다.

## ☀️ 개선된 스크롤 처리

> ⚡️ **스크롤 방지 (preventScroll)**

```jsx
const preventScroll = () => {
  const currentScrollY = window.scrollY; // 현재 스크롤 위치 저장
  document.body.style.position = "fixed"; // body 고정
  document.body.style.width = "100%"; // 전체 너비 설정
  document.body.style.top = `-${currentScrollY}px`; // 현재 위치만큼 위로 올림
  document.body.style.overflow = "scroll"; // 스크롤바 유지
  return currentScrollY; // 저장된 위치 반환
};
```

위처럼 body를 고정해 스크롤을 방지하고 스크롤바가 사라지며 발생하는 화면 떨림을 방지했다.

> ⚡️ **스크롤 복원 (allowScroll)**

```jsx
const allowScroll = (scrollY: number) => {
  document.body.style.position = ""; // position 초기화
  document.body.style.width = ""; // width 초기화
  document.body.style.top = ""; // top 초기화
  document.body.style.overflow = ""; // overflow 초기화
  window.scrollTo(0, scrollY); // 저장된 위치로 스크롤
};
```

모달이 닫히면 모든 스타일을 초기화해서 본래 상태로 복원하고 이전 스크롤 위치로 복귀할 수 있게 바꾸었다.

## ☀️ 사용

```jsx
useEffect(() => {
  if (isOpen) {
    const scrollY = preventScroll();
    return () => allowScroll(scrollY); // cleanup 함수에서 스크롤 복원
  }
}, [isOpen]);
```

이처럼 구현하니 모달을 열고 닫아도 스크롤 바가 사라지지 않고 유지되며 모달이 열렸을 때 스크롤도 되지 않도록 하였다.

단순히 `overflow: hidden`을 설정하는 것보다 훨씬 더 섬세한 처리가 필요했다. 특히 스크롤 위치 보존과 쉬프트 방지는 UX에 직접적인 영향을 미칠 수 있는 요소로 더욱 완성도 높은 바텀시트 모달을 만들 수 있었다.

# 6️⃣ 결론

처음에는 단순히 하단에서 올라오는 모달을 구현하는 것이 정말 어려울 것이라고 생각했다. 실제로도 구현하며 많은 고려사항이 있다는 것을 깨달았다.

특히 애니메이션 라이브러리 없이 순수하게 구현하려다 보니 생각보다 더 어려움이 있었다. 드래그 기능을 구현하면서 발생한 리렌더링 문제는 `attrs`를 활용해 해결할 수 있었고, 이 과정에서 성능 최적화에 대해 깊이 생각해볼 수 있었다.

또한 모달이 열리고 닫힐 때의 스크롤 처리도 단순히 생각했던 것보다 훨씬 복잡했다. 처음에는 `overflow: hidden`만으로 충분하다고 생각했지만, 실제로는 스크롤 위치 보존 등 고려해야 할 부분이 많았다. 이러한 세세한 부분들을 신경 쓰면서 UX의 중요성을 다시 한번 실감했다.

결과적으로는 라이브러리의 도움 없이도 네이티브 앱과 비슷한 수준의 바텀시트를 구현할 수 있었다. 이 과정에서 성능 최적화, 사용자 경험 등 다양한 측면을 고려하며 개발해야 한다는 것을 배웠다.

앞으로도 단순히 기능 구현에만 그치지 않고, 사용자 경험과 성능까지 고려한 개발을 해야겠다는 생각이 들었다. 특히 모바일 환경에서의 사용자 경험은 디테일한 부분 하나하나가 매우 중요하다는 것을 이번 기회를 통해 깊이 이해하게 되었다.

![image](https://github.com/user-attachments/assets/8f3f133a-a6dc-4e6b-b0f7-0c768f00d9e9)
