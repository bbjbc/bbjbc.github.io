---
title: "[패스트캠퍼스] 김민태의 프론트엔드 데브캠프 2기 토이프로젝트1(2)"
categories: [dev-camp, react]
tags: [FE, 부트캠프, 패스트캠퍼스, 패스트캠퍼스데브캠프, 프론트엔드, 김민태]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 아흔 네 번째 포스팅

안녕하세요! 아흔 네 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥ 벌써 아흔💫

오늘의 포스팅 내용은 **[패스트캠퍼스] 김민태의 프론트엔드 데브캠프 2기 토이프로젝트1(2)**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 개발 시작

드디어 개발을 시작했다.

설렘반 두려움반으로 시작을 했다. 왜냐? Vanilla JS와 Vanilla CSS로 개발을 한다는 것은 정말 어려울 것으로 예측이 되니깐.

내 불안한 징조는 정답이었다. 항상 React를 사용해왔던 나로서는 정말 어색했다. 또한, CSS 파일을 import해서 사용해야 한다는 게 정말 말도 안되는 일이었다.

본격적인 개발에 앞서, 헤더와 사이드바의 레이아웃과 라우터 구성을 해야했다.

React였다면 뚝딱뚝딱 했을텐데 정말 험난했다. . . 코드가 도대체 왜 이러죠 . . ?

![image](https://github.com/user-attachments/assets/64bf8749-3cc8-46bd-90ae-9f220180f02f)

레이아웃 구성하는 데에만 이틀이나 소요된 것 같다. 팀원 한 명이 맡아 작업하다가 모르는 부분이 생기면 의논하다가 서로 풀 땡겨서 코드를 보면서 수정을 해나갔다.

결국은 당연히 잘 해결이 되어 레이아웃을 통해 그 안에 페이지만 구성하는 식으로 진행했다.

그리고나서 역할을 나누었다.

![image](https://github.com/user-attachments/assets/c2201d9e-9df8-4b0b-b5d9-f84158baa2ff)

위 사진은 개발 전에 프로토타입을 위해 피그마로 디자인을 한 것이다.

나는 관리자 페이지를 맡았다. 관리자 페이지는 한 번도 만들어본 경험이 없기도 했고 시도해보고 싶었다.

관리자 파트 2명, 사용자 파트 3명으로 이루어져 작업을 진행했다.

관리자 파트에서도 파트를 나누어 진행했다.

layout이 배치된 develop 브랜치에서 각자의 기능을 퍼블리싱하기 위해 브랜치를 만들어 작업을 진행했다.

작업을 진행하기 전에 각자가 현재 무얼 하고 있는지 파악하기 위해서 이슈를 생성하여 이슈사항대로 작업을 진행했다.

![image](https://github.com/user-attachments/assets/4e00779c-0ea6-42d0-9162-fc1031359e4b)

그리고 작업 사항이 완료되면 PR을 올려 2명 이상이 리뷰를 하거나 승인이 나면 머지하기로 결정했다.

![image](https://github.com/user-attachments/assets/e8a15793-66dd-4693-b86b-44a6fb403e56)

위와 같이 PR을 마구마구 올렸다.

![image](https://github.com/user-attachments/assets/1b9392b0-6640-48e0-9f3f-113a624a2106)

PR을 진행하며 코드리뷰를 통해 내가 알고 있던 지식을 통해 리뷰를 한 것도 있다.

코드리뷰를 진행하다 보니 나와 다르게 코드를 짜는 것을 보며 로직에 대해서도 배운 것 같다.

![trouble](https://github.com/user-attachments/assets/bef8df51-2381-4fd1-b359-81e558e76040)

또한, PR을 올리며 내가 코드를 짜면서 마주친 트러블슈팅 과정에 대해서도 작성했다.

이렇게 트러블슈팅 과정도 공유하니 지식이 쌓이는 느낌이 들었다😗

---

# 2️⃣ 트러블 슈팅

**그래서** PR에 작성했던 트러블슈팅 과정에 대해서 작성해보겠다.

## 👈 네비게이팅이 왜 안돼?

```js
import { ADMIN_PATH } from "../../../utils/constants";
import navigate from "../../../utils/navigation";

export const RenderAdminNoticeItem = ({
  post,
  itemContainerClassName,
  imageClassName,
  contentContainerClassName,
  contentTitleClassName,
  contentDescriptionClassName,
  contentDateClassName,
}) => {
  const handleNoticeClick = () => {
    navigate(`${ADMIN_PATH.NOTICE}/${post.post_id}`);
  };

  const uniqueId = `admin-notice-item-${post.post_id}`;

  const element = document.getElementById(uniqueId);
  if (element) {
    element.addEventListener("click", handleNoticeClick);
  }

  return `
        <div class="${itemContainerClassName}" id="${uniqueId}">
          <div class="${imageClassName}">
            <img src="${post.post_image}" alt="${post.post_title}" />
          </div>
          <div class="${contentContainerClassName}">
            <div class="${contentTitleClassName}">${post.post_title}</div>
            <div class="${contentDescriptionClassName}">${
    post.post_description
  }</div>
            <div class="${contentDateClassName}">
              <span class="material-symbols-rounded">
                  update
              </span>
              <p>${new Date(post.updated_at).toLocaleString()}</p>
            </div>
          </div>
        </div>
      `;
};
```

위 코드는 관리자 페이지에서 `메인페이지`와 `공지 관리`페이지에서 재사용하기 위해 만든 컴포넌트였다. 그래서 클릭 시 네비게이팅 처리는 여기서만 해주면 됐다. 그러나 마주한 문제가 있었다.

```js
const element = document.getElementById(uniqueId);
if (element) {
  element.addEventListener("click", handleNoticeClick);
}
```

위 코드와 같이 실행을 했더니 네비게이팅 되지 않는 문제를 마주했다. 왜 그런지 만들었던 파일을 다시 한 번 살펴보며 문제를 파악했다. 하지만 좀처럼 파악되지 않아 찾아보았다.

문제는 바로 위 코드였다.

현재 코드의 실행 순서를 보면

1. `RenderAdminNoticeItem` 함수가 HTML 문자열을 반환하고 있다.
2. 이 HTML 문자열이 나중에 DOM에 삽입된다.
3. `document.getElementById()`를 바로 실행하면, HTML이 아직 DOM에 삽입되기 전이라 요소를 찾지 못할 수 있다.

위와 같은 실행 순서 때문에 네비게이팅 처리가 되지 않았던 것이다.

이를 다음과 같이 바꾸면 해결 됐다.

```js
setTimeout(() => {
  const element = document.getElementById(uniqueId);
  if (element) {
    element.addEventListener("click", handleNoticeClick);
  }
}, 0);
```

변경된 코드를 보면 주석에도 적어놨듯 `setTimeout`을 사용하였다. 심지어 `0ms` 딜레이이다.
이를 사용하면

1. 현재 실행 중인 JavaScript 코드가 완료될 때까지 기다린다.
2. 브라우저가 DOM을 업데이트할 시간을 갖는다.
3. 그 후 이벤트 리스너를 추가하는 코드가 실행된다.

이는 JavaScript의 이벤트 루프와 관련이 있다고 한다. `setTimeout`은 콜백을 매크로태스크 큐에 넣어서 현재 실행 중인 코드 스택이 비워지고 DOM 업데이트가 이루어진 후에 실행되도록 보장하기 때문이다.

`setTimeout`이 없이 실행하면 바로 `getElementById`가 실행되어, DOM이 아직 업데이트 되기 전이라 `null`을 반환할 수 있는 가능성이 있으며 결과적으로 이벤트 리스너가 추가되지 않는 것이었다.

## 👈 setTimeout이 별로라고?

```js
setTimeout(() => {
  const logoContainer = document.querySelector(".navbar-top");
  logoContainer.addEventListener("click", () => {
    navigate(ADMIN_PATH.HOME);
  });
}, 0);
```

위에서 작성했듯이 처음에는 `setTimeout`을 사용해서 코드를 작성했다. `setTimeout`을 통해서 DOM 요소가 등록되기 전에 이벤트리스너가 실행되고자 하는 것을 방지했다.

하지만 이는 별로 좋지 않은 선택이라는 것을 알게 됐다. 이는 불필요한 지연이 발생할 수도 있고 DOM 요소가 실제로 준비됐는지 확실히 알기 어렵기 때문이다. 그래서 찾아본 것이 바로 `이벤트 위임(Event Delegation)`이다.

```js
navbar.addEventListener("click", (e) => {
  if (e.target.closest(".navbar-top")) {
    navigate(ADMIN_PATH.HOME);
  }
});
```

위 코드가 이벤트 위임을 사용한 코드이다. 즉시 이벤트 리스너를 설정해서 더 빠른 반응이 가능하다고 한다. `setTimeout`은 DOM 요소의 존재를 보장하지 않지만 `이벤트 위임`은 동적으로 추가되는 요소들도 자동으로 처리해 준다.

이 외에도 이점들이 더 있다.

**1. 메모리 효율성 향상**

- 각 요소마다 이벤트리스너를 추가할 필요가 없어서 메모리 사용이 효율적이다.

**2. 에러 방지**

- DOM이 로드되기 전에 이벤트를 등록하려는 시도를 방지할 수 있다.
- 요소가 없을 때 발생할 수 있는 `null` 참조 에러를 방지한다.

`setTimeout`에도 존재하는 이점일지라도 조금 더 나은 방법이라고 한다!

위에서 `setTimeout`으로 만들었던 코드를 이벤트 위임 방식으로 변경하려고 했는데 상위 컴포넌트로 두 컴포넌트를 거쳐 전달을 해줘야해서 그 부분은 수정하지 않았다. 좀 많이 복잡해지는 느낌이라서,, 지금으로서는 실용성을 택하도록 했다..😓

## 👈 알게된 점

**1. DOM 요소 접근 시점 확인하기**

- 무작정 `querySelector`를 사용했었는데 DOM에 요소가 추가되기 전인지 확인하는 것이 중요한 것 같다.
- DOM이 만들어지기 전에 접근하면 `null`을 반환할 수 있어 존재 여부를 체크하는 습관이 필요할 것 같다.

**2. 안전하게 DOM 조작하기**

- `querySelector`로 찾은 요소가 있을 때만 `appendChild`와 같은 메서드를 사용하도록 해야 한다.
- 조건문으로 분기 처리를 해서 안전하게 DOM을 다뤄야 할 것 같다.

```js
const element = document.querySelector(".any-element");
if (element) {
  element.appendChild(childElement);
}
```

## 👈 멘토님 헬프미!

> 😲 컴포넌트 내에서 `setTimeout`으로 처리하는 방식과, 부모 컴포넌트로 `이벤트 위임`을 하는 방식이 있는데 `setTimeout`은 한 파일에서 깔끔하게 처리되지만 찾아보니 성능 이슈가 있다고 하고, `이벤트 위임`은 성능적으로 이점이 있지만 여러 컴포넌트를 거쳐야 하는 상황이라 코드가 복잡해질 수 있는데 어떤 방식을 택하는 것이 좋을까요?

라고.

다가오는 멘토링 시간에 바로 멘토님께 질문을 드렸다. 돌아오는 멘토님 답변은,,

> 상황에 따라 선택하면 됩니다!
> `setTimeout`을 사용한다면 페이지 이탈 또는 화면 전환 시 `clearTimeout`으로 이벤트 제거 처리를 잘 진행 해주시면 됩니다. 적절한 타이밍을 `setTimeout`으로 설정하고 의도된 결과를 안전하게 낼 수 있다면 사용하지 않을 이유는 없습니다. <br>
> 만약 특정 컴포넌트 내부에서만 사용하는 동작이라면 `setTimeout`으로 구현하는 것이 간편하고 정확한 트리거 관리가 가능할 수 있습니다.
>
> 컴포넌트로 이벤트를 위임/props를 통한 데이터 전달하는 것은 반복된 이벤트 생성을 방지하고 로직관리 및 성능 측면에서 더 효율적일 수 있습니다. <br>
> 동일한 대상을 참고해야 한다면 더욱 그렇습니다. 그렇지만 그 데이터 전달을 위해 수많은 드릴링이 필요하고 컴포넌트 복잡성이 극대화된다면 좋지 않습니다.
>
> 부모 컴포넌트가 복잡도가 높아질수록 로직 관리도 어렵고, 유지보수 뿐만 아니라 기능 추가 시 오류 발생 가능성도 높아질 수 있습니다. <br>
> 또, `부모-이벤트 타겟 자식 컴포넌트` 사이에 다른 자식 요소가 추가될 때마다 수정 작업이 복잡해질 수 있습니다.
>
> `이벤트 위임`이 효율적인 관리의 범주를 벗어나게 된다면 구조적인 개선을 통해 적당한 뎁스를 유지하거나, 전역데이터 기반으로 동작할 수 있도록 이용해볼 수 있습니다.

라고 아주 명확한 답변을 해주셨다. 답변을 보니 정말 당연한 말 뿐이었다. 언제나 실용적이고 상황을 보면서 택하는 것이 가장 중요한 것 같다❗️

---

# 3️⃣ firebase 사용

우리는 서버를 `NodeJS`를 사용하지 않고 서버 역할을 하는 `firebase`를 사용하기로 했다.

팀원 중 한 분께서 사용해보셨다고 해서 자신이 있다고 하셨다.

페이지의 퍼블리싱을 마치고 기존의 로컬에서 `json` 파일을 통해 데이터를 불러오는 것들을 모두 `firebase`에서 가져와 CRUD를 할 계획이었다.

CRUD 작업을 발표 전 3일 전부터 시작을 해서 정말 빠듯하게 진행했다.

처음 코드를 사용해봐서 데이터를 다루는 과정에서 무엇인가가 삭제되고 잘 이해가 되지 않았지만 잘 아시는 팀원분께 계속 질문을 하며 진행했다. 다행히도 잘 답변해주셔서 잘 해결된 것 같다.

프로젝트는 모달을 통한 `수정`, `삭제`, `업로드`가 이루어졌는데 모달을 달고 API까지 만들어 사용하려 하니 정말 복잡해지고 코드도 꽤나 바꿔야해서 난감했다.

시간은 얼마 남지 않아 불안감은 몰려왔다....지만.. 어쩌겠어.ㅋ 해내야지 ㅋ

![image](https://github.com/user-attachments/assets/8ec93070-b0b7-40f4-9fd4-e08a49c9d2d8)

그래도 `firebase` 라이브러리에서 제공하는 함수들이 다양하고 잘 되어 있어서 그것만 잘 활용하고 데이터 구조만 잘 맞춰서 API를 금방 구현할 수 있었다.

결국 어찌저찌 완성(?)이 된 것 같다.

---

# 4️⃣ KPT 회고

KPT란 프로젝트나 활동을 돌아보는 회고 방식 중 하나이다. `Keep(유지할 점)`, `Problem(문제점)`, `Try(시도할 점)`의 관점에서 분석하는 방법이라고 한다.

다음은 나의 KPT 회고이다.

- **KEEP**
  - 모르는 것 있으면 팀 회의를 진행하며 바로바로 질문하기
  - 내 파트 진행하며 궁금했던 점 멘토님께 질문하기
  - 팀원들과 수시로 코드 리뷰를 진행하여 피드백 주고받기
  - PR을 통한 트러블슈팅 과정 공유하기
  - Git과 관련한 컨벤션을 준수하며 코드와 커밋 컨벤션 일관성 증대
  - 기능 구현 전 팀원들과 충분한 논의를 통해 설계 진행
- **PROBLEM**
  - admin과 user가 나뉘다 보니 공통되는 컴포넌트가 존재하는데 완성하기에 급급하여 중복되는 코드가 많은 점이 아쉬움
  - UI 컴포넌트의 재활용도를 조금 더 높이기
  - Vanilla JS를 처음 사용하다 보니 전체적으로 코드가 복잡하게 되어 관심사와 가독성이 떨어지는 것 같음
  - 사용자 경험이 다소 부족한 부분이 아쉬움
- **TRY**
  - 중복 코드를 줄이고 최대한 공통 컴포넌트를 통해 재사용하기
  - 컨벤션을 사전에 정의했지만 일관성이 떨어지는 부분 수정하기
  - 컴포넌트 설계 시 재사용성과 확장성 우선 고려하기
  - 성능 최적화를 위한 데이터 캐싱 전략 도입
  - 사용자 경험 향상을 위한 피드백 시스템 구축

Vanilla의 벽은 정말 높았다. 물론 이것을 기반으로 라이브러리나 프레임워크를 만든거긴 하지만 정말 힘들고 어려웠던 시간들이었다.

이번 프로젝트를 통해서 프레임워크의 소중함을 깨닫는 계기가 되었다. 강사님께서도 이번 프로젝트를 Vanilla JS로 제한하셨던 이유가 프레임워크의 소중함을 일깨워 주기 위해서라고 하셨다.

몸소 체감하고 나니 매우 얼얼하고 정신이 없었다. 얼렁뚱땅 만들어도 되니 최선을 다해서 완성을 해보라고 하셨던 강사님 감사합니다😆

3주도 안되는 기간동안 열심히 작업 해주시고 회의 해주셔서 감사했습니다 팀원분들😃

---

# 5️⃣ 깃허브

[**[프로젝트 깃허브]**](https://github.com/Dev-FE-2/toy-project1-team2-intranet-project)입니다❗️
