---
title: kakaoLogin
date: 2024-02-02 18:00:00 +0900
categories: [stack, signIn]
tags: [stack]
---

<br>
<br>
<br>

# kakaoLogin

---

<br>

## 1. 소셜로그인

소셜로그인 기능 구현

#### 1.1. 로그인 페이지

CLIENT_ID와 REDIRECT_URI는 백엔드 로직을 맞춰보면서 설정 계획

```javascript
import React from "react";
import styled from "styled-components";

function KakaoSignIn() {
  const CLIENT_ID = "";
  const REDIRECT_URI = "http://localhost:3000/"; // 뒤 경로 백앤드와 설정
  const KAKAO_AUTH_URL = `https://kauth.kakao.com/oauth/authorize?client_id=${CLIENT_ID}&redirect_url=${REDIRECT_URI}&response_type=code`;

  const kakaoLoginBtn = () => {
    window.location.href = KAKAO_AUTH_URL;
  };

  return (
    <KakaoBtn onClick={kakaoLoginBtn}>
      <img src="" alt="kakaologo" />
      <span>카카오 로그인</span>
    </KakaoBtn>
  );
}

export default KakaoSignIn;
```

#### 1.2. 리다이렉트 페이지

- 위 로그인 페이지가 잘 띄워졌으면 완료 후 리다이렉트 페이지로 이동된다.
- 리다이렉트 주소에 맞춰 라우터 설정해서 띄워주는 페이지에서 안보이는 로직들 실행

```javascript
<Route path="/백엔드와 맞춘 리다이렉트 경로 넣기" element={<Redirection/>}/>
```

- 토큰값이 어떻게 들어오는지 보고 조정
- 로그인중 알 수 있게 스피너 넣어주기

```javascript
import React, { useEffect } from "react";
import api from "../../services/api";
import { useCookies } from "react-cookie";
import { useNavigate } from "react-router-dom";

function Redirection() {
  const navigate = useNavigate();
  const [Cookies, setCookie, RemoveCookie] = useCookies("oCookie");
  // window.location.href현재 페이지 url에서 searchParams params를 추적
  // .get('code') code를 빼온다.
  let code = new URL(window.location.href).searchParams.get("code");

  // 카카오 코드 주는 주소 백에서 알려달라하기 api수정할 수도..
  // 토큰 어떻게 들어오는지 보기
  const postKakao = async () => {
    try {
      const response = await api.post("", code);
      console.log(response.data);
      setCookie("tokenName", response.data, {
        path: "/경로",
        secure: true
      });
      navigate("/");
    } catch (error) {
      console.log("Login failed:", error);
    }
  };

  useEffect(() => {
    postKakao();
  }, []);

  return <div>로그인 중입니다.</div>;
}

export default Redirection;
```

#### 1.3. 생각
아직 서버와 연결을 안 해 봐서 잘 작동할지 모르겠다.  
세부적인건 연결하면서 하나씩 조정해봐야겠다.