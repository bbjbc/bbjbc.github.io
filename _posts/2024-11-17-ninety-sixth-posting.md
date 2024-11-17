---
title: "[React] 소셜 로그인은 선택이 아닌 필수: 구글/카카오 소셜 로그인을 (힘들게) 구현하며"
categories: [react]
tags: [OAuth, 인증, 소셜로그인, 카카오, 구글, 리액트, react]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 아흔 여섯 번째 포스팅

안녕하세요! 아흔 여섯 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **[React] 소셜 로그인은 선택이 아닌 필수: 구글/카카오 소셜 로그인을 (힘들게) 구현하며**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 1️⃣ 소셜 로그인의 이해

요즘 웹 서비스에 들어가보면 "이메일로 가입" 버튼 옆에 항상 보이는 것들이 있다. 바로 "구글로 계속하기", "카카오로 시작하기" 같은 소셜 로그인 버튼들이다.

새로운 서비스를 만들 때 소셜 로그인은 이제 **선택이 아닌 필수**가 되었다. 사용자 입장에서는 새로운 아이디와 비밀번호를 만들고 기억할 필요 없이, 평소에 사용하는 계정으로 바로 시작할 수 있으니 얼마나 편리한가❗️

저번에 OAuth에 대한 전반적인 내용을 다뤘었는데[(**이전 포스팅 보러가기**)](https://bbjbc.github.io/react/ninety-first-posting/) 이번에는 실제로 React에서 구글과 카카오 소셜 로그인을 구현하며 느낀점에 대해 다뤄보려고 한다.

![image](https://github.com/user-attachments/assets/a54dba60-a4da-41e2-91e2-e0ee8d6b13fe)

**소셜 로그인**은 **OAuth 2.0 프로토콜을 기반**으로 동작한다. 이는 사용자가 직접 계정 정보를 입력하지 않고도 신뢰할 수 있는 제3자(구글, 카카오 등)를 통해서 안전하게 인증할 수 있게 해주는 표준이다.

> 💥 **소셜 로그인의 흐름**

1. 사용자가 소셜 로그인 버튼 클릭
2. 해당 소셜 서비스의 로그인 페이지로 리다이렉션
3. 사용자가 소셜 서비스에서 인증
4. 인가 코드를 받아 서버로 전송
5. 서버가 인가 코드를 통해 액세스 토큰 발급
6. 클라이언트가 액세스 토큰을 저장하고 API 요청에 사용

위는 소셜 로그인의 대체적인 흐름이다.

더욱 자세한 내용이 궁금하다면 저번에 작성한
[**[OAuth 로그인 프로세스]**](https://bbjbc.github.io/react/ninety-first-posting/#-oauth-%EB%A1%9C%EA%B7%B8%EC%9D%B8-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4)글을 참고해보시면 좋을 것 같다. 다이어그램과 함께 자세히 설명해두었다.

---

# 2️⃣ 구현 방식 비교와 선택

소셜 로그인을 구현할 때 가장 먼저 고민해야 할 것은 프론트엔드에서 직접 로그인 화면을 처리할 것인지, 서버를 통해 처리할 것인지에 대한 결정이다. 처음에는 단순히 프론트엔드에서 처리하면 되겠지 싶었는데, 알아볼수록 그렇게 단순한 문제는 아니었다.

각 방식의 특징과 장단점이 존재하니 확인해보자!

## 💻 프론트엔드 중심의 처리 방식

프론트엔드에서 직접 OAuth URL을 구성하고 소셜 로그인 과정을 진행하는 방식이다.

```jsx
const GOOGLE_CLIENT_ID = process.env.REACT_APP_GOOGLE_CLIENT_ID;
const REDIRECT_URI = `${window.location.origin}/oauth/callback/google`;
const scope = "email profile";

const googleLoginUrl = `https://accounts.google.com/o/oauth2/v2/auth?client_id=${GOOGLE_CLIENT_ID}&redirect_uri=${REDIRECT_URI}&response_type=code&scope=${scope}`;
```

> 💡 **장점**

- 구현이 직관적이고 단순하다.
- 서버 의존성이 낮아 빠른 개발이 가능하다.
- 프론트엔드에서 전체 플로우를 제어할 수 있다.

> 💡 **단점**

- Client ID가 노출될 수 있어 보안에 취약하다.
- 인증 로직이 클라이언트에 노출된다.
- 클라이언트가 시크릿 키를 알 수 있기 때문에 공격자가 악용할 수 있다.

## 🔧 서버 중심의 처리 방식

인증 관련 로직을 서버에서 처리하고, 프론트엔드는 단순히 리다이렉션만 처리하는 방식이다.

```jsx
const GOOGLE_SERVER_REDIRECT_URI = import.meta.env
  .VITE_GOOGLE_SERVER_REDIRECT_URI;
const KAKAO_SERVER_REDIRECT_URI = import.meta.env
  .VITE_KAKAO_SERVER_REDIRECT_URI;

const handleLogin = () => {
  window.location.href = GOOGLE_SERVER_REDIRECT_URI;
};
```

나도 보안성을 고려하여 백엔드 중심 방식을 선택했다.

> 💡 **장점**

- 민감한 정보가 클라이언트에 노출되지 않아 보안성이 높다.
- 인증 로직을 서버에서 중앙 집중적으로 관리할 수 있다.
- Client ID와 Secret을 안전하게 관리할 수 있다.
- 정책 변경 시 서버 측 코드만 수정하면 된다.
- 토큰 관리가 서버에서 이루어진다.

> 💡 **단점**

- 초기 설정과 구현이 상대적으로 복잡할 수 있다.
- 서버 의존성이 높아 서버 리소스가 필요할 수 있다.

위와 같은 각 방법에 대한 장단점이 존재한다. 대부분의 개발자들이 서버 중심의 처리 방식을 택한다고 한다.

---

# 3️⃣ 개발 협업 관점에서의 고려사항

## 🔥 구현 방식 사전 협의의 중요성

개발을 하다 보면 처음 마주하는 기능들이 많다. 나에게 소셜 로그인 구현은 그런 도전 중 하나였다.

"구글 로그인 버튼 하나 만들고 인증 처리하면 되는거 아닌가?"라고 생각했던 것이 부끄러울 만큼, 실제 구현 과정에서는 많은 고민과 시행착오가 있었다😓

### 💪 첫 시도: 프론트엔드에서 직접 구현하기

처음에는 당연히 프론트엔드에서 모든 것을 처리하려 했다. 구글링을 React 관점으로 해서 그런지 온통 프론트엔드 측면에서 구현된 글이 대다수였다.

```jsx
const K_REST_API_KEY = import.meta.env.VITE_KAKAO_REST_API;
const K_REDIRECT_URI = import.meta.env.VITE_KAKAO_OAUTH_REDIRECT_URI;
const kakaoURL = `https://kauth.kakao.com/oauth/authorize?client_id=${K_REST_API_KEY}&redirect_uri=${K_REDIRECT_URI}&response_type=code`;

const handleKakaoLogin = () => {
  window.location.href = kakaoURL;
};
```

위는 카카오 로그인을 프론트엔드에서 처리하려 한 코드이다.

코드는 깔끔했고, 동작도 잘 됐다. 하지만 조금 위험해보였다. 저렇게 리다이렉트 되면 결국 Client ID를 프론트엔드에 노출해야 한다는 점, 토큰 관리는 어덯게 할 것인지, 보안은 괜찮은 건지 ... 고민이 쌓여갔다.

구글 소셜 로그인을 구현할 때는 `@react-oauth/google` 라이브러리를 찾아냈고, 이를 사용하면 쉽게 구현할 수 있을거라 생각했다.

```jsx
import { GoogleOAuthProvider } from "@react-oauth/google";

import GoogleCustomButton from "./GoogleCustomButton";

const GoogleLoginButton = () => {
  const GOOGLE_CLIENT_ID = import.meta.env.VITE_GOOGLE_CLIENT_ID;

  return (
    <GoogleOAuthProvider clientId={GOOGLE_CLIENT_ID}>
      <GoogleCustomButton />
    </GoogleOAuthProvider>
  );
};

export default GoogleLoginButton;
```

위는 `Provider`를 사용하기 위한 컴포넌트이다.

```jsx
import { useGoogleLogin } from "@react-oauth/google";

const GoogleCustomButton = () => {
  const REDIRECT_URI = import.meta.env.VITE_GOOGLE_OAUTH_REDIRECT_URI;

  const login = useGoogleLogin({
    flow: "auth-code",
    ux_mode: "redirect",
    redirect_uri: REDIRECT_URI,
  });

  return <Button>구글 계정으로 로그인</Button>;
};
export default GoogleCustomButton;
```

이런식으로 라이브러리를 사용해서 편리하게 구현하려고 했다.

하지만 이것마저 `CLIENT_ID`가 노출될 것만 같았다.

### 💪 백엔드 개발자와의 미팅

고민 끝에 백엔드 개발자와 이야기를 나누었다. 근데 허거덩이다. 백엔드에서는 이미 다른 방식으로 구현을 시작한 상태였다😖

백엔드 개발자는 구글링을 하면 백엔드 중심으로 검색을 해서 이미 백엔드 중심으로 처리하려고 개발중이었던 것이다. 그니까 OAuth URL을 생성하고 관리하는 방식으로 말이다.

두 가지의 다른 접근 방식에서 혼란스러웠다. 나는 분명 내가 찾은 방법이 이건데 새로운 방식으로 말씀하시니 조금 당황스러웠다.

어떤 것이 더 좋은 방식일까..? 이미 각자 작성한 코드는 어떻게 하지..?

![image](https://github.com/user-attachments/assets/0bcc6a1b-099b-4a1b-b463-b59dd91f0062)

### 💪 시행착오를 겪으며 느낀점

결국 프론트엔드 코드를 바꾸기로 했다. 서버 중심의 방식을 선택했고 알게된 점이 있다.

1. 보안이 중요한 기능은 서버에서 처리하는 것이 좋다. Client Secret 같은 민감한 정보는 절대로 프론트엔드단에 노출하면 안된다.
2. 토큰 관리도 서버에서 하는 것이 안전하다.
3. 무엇보다 중요한 것 **사전 협의**다. "각자 알아서 개발하고 나중에 합치면 되겠지"라는 생각은 버리자.

### 💪 그래서 지금은?

현재는 소셜 로그인이 안정적으로 동작하고 있다. 프론트엔드는 단순히 서버가 제공하는 URL로 리다이렉션만 처리하고, 복잡한 인증 로직은 모두 서버에서 안전하게 처리된다.

```jsx
const handleLogin = () => {
  window.location.href = GOOGLE_SERVER_REDIRECT_URI;
};
```

코드는 오히려 더 단순해졌지만, 보안성은 크게 향상됐다.

---

# 4️⃣ 상세 구현

소셜 로그인을 구현하면서 가장 많이 들었던 생각은 "이걸 이렇게 하는 게 맞나?"였다.

처음에는 정말 단순해 보였던 기능이 실제로 구현하다 보니 뭔가 잘 안되는 부분도 있고, 에러 생기는 부분도 있어 복잡했고, 고려해야 할 사항도 많았다. ~~내가 초보라 그럴수도~~

## ▶️ 로그인 버튼 구현

구글과 카카오는 각각의 브랜드 가이드라인이 있었다. 이를 준수하며 버튼을 제작했다. 버튼 아이콘은 보통 `svg`파일을 통해 사용한다. 하지만 `react-icons`에도 똑같은 아이콘이 있길래 사용했다.

> 🔘 **구글 버튼**

```jsx
import { FcGoogle } from "react-icons/fc";

import { Button } from "@/components/common/ui/button/button";

const GoogleCustomButton = () => {
  const GOOGLE_SERVER_REDIRECT_URI = import.meta.env
    .VITE_GOOGLE_SERVER_REDIRECT_URI;

  const handleGoogleLogin = () => {
    window.location.href = GOOGLE_SERVER_REDIRECT_URI;
  };

  return (
    <Button
      variant="danger"
      className="flex w-full items-center justify-center gap-2 rounded-md border border-gray-300 bg-white text-sm font-medium text-gray-700 transition-colors duration-300 hover:bg-gray-100"
      onClick={handleGoogleLogin}
    >
      <FcGoogle size={20} />
      구글 계정으로 로그인
    </Button>
  );
};

export default GoogleCustomButton;
```

> 🔘 **카카오 버튼**

```jsx
import { RiKakaoTalkFill } from "react-icons/ri";

import { Button } from "@/components/common/ui/button/button";

const KakaoLoginButton = () => {
  const KAKAO_SERVER_REDIRECT_URI = import.meta.env
    .VITE_KAKAO_SERVER_REDIRECT_URI;

  const handleKakaoLogin = () => {
    window.location.href = KAKAO_SERVER_REDIRECT_URI;
  };

  return (
    <Button
      variant="secondary"
      className="flex w-full items-center justify-center gap-2 rounded-md bg-[#FEE500] text-sm font-medium text-gray-700 transition-colors duration-300 hover:border hover:border-yellow-300 hover:bg-[#FFEB3B]"
      onClick={handleKakaoLogin}
    >
      <RiKakaoTalkFill size={20} />
      카카오 계정으로 로그인
    </Button>
  );
};

export default KakaoLoginButton;
```

![image](https://github.com/user-attachments/assets/7affad50-06a2-4f72-a549-7660e2c43342)

정말 깔끔하다.

## ▶️ OAuth 핸들러

버튼은 만드는 건 식은 죽 먹기였다. 하지만 진짜 고민은 OAuth 핸들러를 구현할 때부터 시작됐다.

사용자가 구글이나 카카오에서 로그인을 마치고 우리 서비스로 돌아왔을 때 이를 어떻게 처리할 것인가가 관건이었다.

```jsx
const useOAuthHandler = (provider: Provider) => {
  const navigate = useNavigate();
  const processedRef = useRef(false);
  const setAccessToken = useAuthStore((state) => state.setAccessToken);

  const { handleRequest: sendAuthRequest } =
    useAxios <
    OAuthResponse >
    {
      url: `/oauth/login/${provider}`,
      method: "POST",
      initialData: { access_token: "" },
    };

  const sendAuthCodeToServer = useCallback(
    async (code: string) => {
      try {
        const response = await sendAuthRequest({ body: { code } });

        if (response.access_token) {
          setAccessToken(response.access_token, provider);
          navigate("/");
        }
      } catch (error) {
        navigate("/login", { replace: true });
        console.error(`${provider} 로그인 에러:`, error);
      }
    },
    [navigate, provider, setAccessToken, sendAuthRequest]
  );

  useEffect(() => {
    if (processedRef.current) return;

    const searchParams = new URLSearchParams(window.location.search);
    const code = searchParams.get("code");

    if (code) {
      sendAuthCodeToServer(code);
    } else {
      navigate("/login", { replace: true });
    }

    processedRef.current = true;
  }, [navigate, sendAuthCodeToServer]);

  return null;
};

export { useOAuthHandler };
```

이 코드가 어떻게 실행되는지 한참 찾아보고 많은 것을 배웠다. 처음에는 단순하게 인가 코드를 서버로 요청하면 되겠지 했는데, 실제로는 고려해야 할 것이 많았다.

전체적인 흐름을 보면, 사용자가 소셜 로그인 화면에서 인증을 완료하면 미리 설정해둔 라우터로 리다이렉트된다.

```jsx
const router = createBrowserRouter([
  { path: GOOGLE_OAUTH, element: <GoogleOAuthHandler /> },
  { path: KAKAO_OAUTH, element: <KakaoOAuthHandler /> },
]);
```

그러면 각 소셜 로그인별 핸들러 컴포넌트가 실행되면서 인증 처리가 시작된다.

```jsx
import { useOAuthHandler } from "@/hooks/useOAuthHandler";

const KakaoOAuthHandler = () => {
  useOAuthHandler("kakao");

  return <h1>로그인 중입니다. 잠시만 기다려주세요.</h1>;
};

export default KakaoOAuthHandler;
```

여기서 어이없는 점은, 이 과정을 구현하면서 꽤나 고생했다는 거다. 특히 카카오 개발자 도구나 구글 클라우드 콘솔에서 리다이렉트 주소 설정할 때는 정말 머리가 아팠다.

프론트엔드에서 처리할 주소를 정확히 등록해야 하는데, 한 글자라도 다르면 동작하지 않았다. 처음에는 이 문제를 찾는다고 몇 시간을 헤맸던 기억이 난다 😅

여기에 더해 CORS 문제로 또 한번 곤욕을 치렀다.
백엔드 서버로 인증 코드를 보내는 과정에서 CORS 에러가 발생했는데, 이걸 해결하느라 무한 디버깅의 늪에 빠졌었다.

![image](https://github.com/user-attachments/assets/5138fc69-3308-4b66-852b-d3073181a7ee)

프론트엔드 개발자라면 한 번쯤은 겪어봤을 그 익숙한 빨간 글씨와의 씨름... 🥲

결국 백엔드 개발자와 함께 설정을 하나하나 점검하면서 해결했지만, 이런 문제들이 하나둘 쌓이다 보니 단순해 보였던 소셜 로그인 구현이 만만치 않은 과제였다는 걸 깨달았다.

```jsx
const processedRef = useRef(false);

useEffect(() => {
  if (processedRef.current) return;
  // ... 인증 처리 로직
  processedRef.current = true;
}, []);
```

또한, 이 부분은 실수로 알게 된 부분이다. React의 특성상 `useEffect`가 두 번 실행될 수 있어서 인증 처리가 중복으로 일어날 수 있다는 것을 발견했다.

`processdRef`를 사용해 한 번만 처리되도록 만든 것은 실제 에러를 겪고 나서야 추가한 부분이었다.

---

# 5️⃣ 토큰 관리

토큰 관리는 정말 많은 고민이 필요했던 부분이다. 처음에는 단순히 localStorage에 토큰을 저장하려 했다. 하지만, 보안 이슈를 찾아보니 이게 최선은 아니라는 것을 깨달았다.

## 👏 Persist 미들웨어

현재는 Zustand의 `persist` 미들웨어를 사용한 방식을 택했다.

```jsx
const useAuthStore = create<AuthStoreType>()(
  persist(
    (set) => ({
      accessToken: null,
      provider: null,
      setAccessToken: (token: string, provider: Provider) =>
        set({ accessToken: token, provider }),
      clearAccessToken: () => set({ accessToken: null, provider: null }),
    }),
    {
      name: 'auth-storage',
      storage: createJSONStorage(() => localStorage),
    },
  ),
);
```

Zustand의 `persist`를 선택한 이유는 크게 두 가지였다. 첫째로는 서버에서 아직 리프레시 토큰 로직이 구현되지 않았기 때문이고, 둘째로는 `persist`가 제공하는 몇 가지 장점 때문이었다. [(**Zustand-persist 알아보기**)](https://zustand.docs.pmnd.rs/integrations/persisting-store-data)

1. 상태 관리와 영속성을 한 곳에서 처리할 수 있다.
2. 스토리지 동기화가 자동으로 이루어진다.
3. 타입 안정성을 보장받을 수 있다.

## 👏 Refresh Token

하지만 이는 어디까지나 임시방편이다. 많은 개발자들은 리프레시 토큰을 활용한 방식이 더 안전하다고 한다. 결국 서버에서 리프레시 토큰 로직이 구현되면 다음과 같이 방식을 전환하고 싶다.

```jsx
type AuthStoreType = {
  accessToken: string | null;
  provider: Provider | null;
  setAccessToken: (token: string, provider: Provider) => void;
  clearAccessToken: () => void;
  refreshToken: () => Promise<string | null>;
};

const useAuthStore = create<AuthStoreType>()((set) => ({
  accessToken: null,
  provider: null,
  setAccessToken: (token: string, provider: Provider) =>
    set({ accessToken: token, provider }),
  clearAccessToken: () => set({ accessToken: null, provider: null }),
  refreshToken: async () => {
    try {
      const response = await axios.post('/api/auth/refresh');
      const newToken = response.data.access_token;
      set({ accessToken: newToken });
      return newToken;
    } catch (error) {
      set({ accessToken: null, provider: null });
      return null;
    }
  },
}));
```

1. Access Token은 메모리에만 저장해 XSS 공격으로부터 보호할 수 있다.
2. Refresh Token은 httpOnly 쿠키로 관리해 JavaScript로의 접근을 차단한다.
3. Access Token이 만료되면 자동으로 Refresh Token을 사용해 갱신한다.
4. Refresh Token도 만료되면 사용자를 로그인 페이지로 리다이렉트한다.

이렇게 하면 보안성도 높이고 사용자 경험도 않을 수 있다. 특히 httpOnly 쿠키를 사용하면 XSS 공격으로부터 안전하면서도, CSRF 공격은 별도의 토큰으로 방어할 수 있다고 한다.

물론 이 방식도 완벽하진 않다.

![image](https://github.com/user-attachments/assets/0cd238ce-ffba-448d-a8c1-0c4ae4e8d826)

Refresh Token이 탈취되면 새로운 Access Token을 계속 발급받을 수 있다는 위험이 있다. 하지만 이는 서버 측에서 Refresh Token Rotation이나 탈취 감지 메커니즘을 구현해 보완할 수 있다고 한다.

결국 보안이란 건 완벽한 해결책보다는 위험을 최소화하는 방향으로 가는 게 현실적인 것 같다.

지금은 임시방편으로 `persist`를 사용하고 있지만, 조만간 보안 강화 작업을 진행할 예정이다.

---

# 6️⃣ 마무리

소셜 로그인 구현을 찾아보았을 때는 정말 단순해서 금방 구현할 줄 알았다. 하지만, 실제로는 많은 고민과 시행착오가 필요한 작업이란 걸 몸소 느낀 작업이었다.

특히 보안과 사용자 경험 사이에서 균형을 맞추는 게 가장 어려웠다. 너무 보안에만 신경 쓰다 보면 사용자가 불편해하고, 편의성만 생각하면 보안이 걱정되고... 이런 고민들을 하나씩 해결해가는 과정이 힘들면서도 즐거웠다.

처음 이 글을 쓰려고 했을 때는 "내가 겪은 이런 시행착오들이 너무 기초적이고 부끄러운 거 아닐까?" 하는 걱정도 있었다. 하지만 누군가는 분명 나처럼 이 과정을 시작하고 있을 거란 생각이 들었다.

지금 이 글을 읽고 있는 당신도 소셜 로그인 구현으로 고민하고 있다면, 너무 걱정하지 말자. 우리 모두 처음은 있으니까! 이 글에서 다룬 경험들이 앞으로 소셜 로그인을 구현할 다른 개발자들에게 조금이나마 도움이 되었으면 좋겠다.

앞으로도 더 나은 방법을 찾아가면서, 배운 것들을 계속 공유해보려고 한다. 다음에는 리프레시 토큰 도입기도 들려드릴 수 있겠죠?😊

![image](https://github.com/user-attachments/assets/0937225f-c259-4516-a096-5e32b693f4d2)
