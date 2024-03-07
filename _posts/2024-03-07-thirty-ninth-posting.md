---
title: "[Jekyll] Liquid Exception: Liquid syntax error: Variable '{{a1}' was not properly terminated with regexp"
categories: [jekyll]
tags: [jekyll, error]
author-profile: true
sidebar_main: true
main: true
toc: true
toc_sticky: true
---

# 서른 아홉 번째 포스팅

안녕하세요! 서른 아홉 번째 포스팅으로 찾아뵙게 되어 반갑습니다!♥

오늘의 포스팅 내용은 **Jekyll Liquid 문법 오류**에 관한 내용입니다. <br/>
자세한 내용을 알아보러 갑시다❗️

**[Boongranii] Here We Go 🔥**

---

# 💥 오류 상황

[**[Lv.2-튜플]**](https://bbjbc.github.io/algorithm/thirty-eighth-posting/)

위 문제에서 지킬 서버를 로컬 환경에서 실행하다가 생긴 오류이다.

현재는 오류를 해결하고 push하여 문제가 없는 상황이며, 오류에 관한 포스팅을 지금 작성하는 것이다.

---

# ⛔️ 오류 메시지

![error](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/5b060a03-438f-47a7-95ec-52ead0295fac) <br>

이러한 오류가 떠서 당황했다.

```md
Liquid Exception: Liquid syntax error (line 30): Variable {% raw %} '{{a1}' was not properly terminated with regexp: /\}\}/ {% endraw %}
```

핵심 오류는 위와 같았다.

---

# ⚠️ 오류 발생 원인

오류 발생한 원인은 `Liquid syntax error`가 발생했기 때문이다. 이는 `Jekyll`의 `Liquid` 템플릿 엔진에서 발생한 오류이며, 보통 `Liquid 문법 오류`로 인해 발생한다.

오류의 원인을 마크다운에서 찾고 있었는데 Liquid 템플릿 엔진이 원인이었다. {% raw %} `{{a1`이라는 변수가 제대로 종료되지 않았다는 것이다. {% endraw %}

결국 마크다운 본문에 있는 `culry brace`가 연속으로 2개 들어가면 Jekyll은 텍스트가 아닌 변수로 인식해서 발생하는 오류인 것이다.

> **템플릿 언어 (Template Language)**

jekyll은 [**[Liquid]**](https://shopify.github.io/liquid/) 라는 템플릿 언어를 사용한다.

템플릿 언어는 중괄호(curly brace)와 퍼센트 기호(percentage sign)를 사용해서 변수나 제어문을 표현한다.

![liquid3](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/2ed2ef22-88c5-4ca4-8f3f-664f7c156243) <br>

위는 Liquid를 통한 변수 사용 예시이다. (원래 코드로 썼었는데 왜인지 모르게 변수 부분이 안보이게 되어 사진을 첨부한다.)

![liquid4](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/26939dbe-2bff-4615-a4ac-8d8e17f4ae71) <br>

위는 Liquid를 통한 조건문과 반복문을 사용하는 방법이다. `if`, `else`, `endif`, `for`, `endfor` 과 같은 키워드를 사용한다.

---

# 🔍 오류 분석

문제와 풀이 방법에 널려 있던 {% raw %} `{{a1}, {a1, a2} ...`에서 {% endraw %} 중괄호 2개로 시작해서 변수를 정상적인 변수의 형태로 인식하지 못하기 때문이다. 이런 식으로 변수명을 사용하는 형식은 Liquid 문법에서 제대로 된 문법이 아니다.

그러므로 오류가 발생한 것이다.

---

# 💡 오류 해결 방법

![liquid2](https://github.com/bbjbc/bbjbc.github.io/assets/102457140/0a78cc0a-4070-4063-9576-feb514feac0a) <br>

해결책이다. 설명을 해주겠다.

Liquid 문법에서 `{퍼센트_기호 raw 퍼센트_기호}` 와 `{퍼센트_기호 endraw 퍼센트_기호}`는 태그 처리를 임시로 비활성화하는 기능을 제공한다. 이는 일부 특정 콘텐츠를 생성할 때 유용하며 다른 템플릿 엔진과 충돌하는 구문을 사용해야 할 때 사용 가능하다.

```liquid
{퍼센트_기호 raw 퍼센트_기호}
    In Handlebars,{% raw %} {{ this }} will be HTML-escaped, but {{{ that }}} will not. {% endraw %}
{퍼센트_기호 endraw 퍼센트_기호}
```

이것을 보면 태그가 없었다면 무조건 에러가 발생했을 것이다. `raw-endraw`를 통해 내부의 모든 내용이 Liquid 문법으로 해석되지 않고 그대로 출력된다.

```md
In Handlebars,{% raw %} {{ this }} will be HTML-escaped, but {{{ that }}} will not. {% endraw %}
```

이렇게 이상없이 출력되는 결과를 볼 수 있다.

`퍼센트_기호`라고 사용한 것은 `%`를 사용하면 안보이게 된다.

---

# 👅 느낀점

다행히 좋은 분의 블로그를 찾아서 오류를 해결할 수 있었고 Liquid 템플릿 언어에 대해 알아볼 수 있었다. 자료 제공 감사합니다.

사실, 이 게시글을 쓸 때도 계속되는 에러로 애를 먹었다. 어딘가에 이중괄호 때문이다.. ㅡ3ㅡ

**그럼 안녕핑🐌**

## 레퍼런스

- [**[Dev Joon님의 블로그]**](https://han-joon-hyeok.github.io/posts/jekyll-liquid-syntax-error-curly-braces/)
- [**[Liquid 공식 문서]**](https://shopify.github.io/liquid/)
- [**[Liquid 공식 문서 - raw]**](https://shopify.github.io/liquid/tags/template/)
