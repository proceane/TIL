# Content Security Policy

## Definition
콘텐츠 보안 정책(Content Security Policy, CSP)은 신뢰된 웹 페이지 콘텍스트에서 악의적인 콘텐츠를 실행하게 하는 사이트 간 스크립팅(XSS), 클릭재킹, 그리고 기타 코드 인젝션 공격을 예방하기 위해 도입된 컴퓨터 보안 표준이다.([출처](https://ko.wikipedia.org/wiki/%EC%BD%98%ED%85%90%EC%B8%A0_%EB%B3%B4%EC%95%88_%EC%A0%95%EC%B1%85))  

주로 XSS나 Data Injection, Click Hacking 등 웹 페이지에 악성 스크립트를 삽입하는 공격기법들을 막기 위해 사용된다.
([출처](https://velog.io/@taylorkwon92/%EC%98%A4%EB%8A%98%EC%9D%98-TIL))

## Properties
[속성과 값 출처](https://velog.io/@taylorkwon92/%EC%98%A4%EB%8A%98%EC%9D%98-TIL)  

속성
- default-src : 모든 리소스에 대한 정책
- script-src : javascript 등 스크립트에 대한 정책
- object-src : 플러그인, 오브젝트에 대한 정책
- style-src : css에 대한 정책
- img-src : 이미지
- media-src : video, audio
- frame-src : iframe
- font-src : font
- connect-src : script src로 불러올 수 있는 url에 대한 정책
- form-action : form의 action에 대한 정책
- sandbox : html 샌드박스
- script-none : 위에 script, 아래쪽에 none이 포함되는 정책
- plugin-types : 로드할 수 있는 플러그인 타입. object-src와 접점
- reflected-xss : X-XSS-Protection header와 동일한 효과
- report-uri : 정책위반 케이스가 나타났을 때 내용을 전달할 url

값
- * : 모든 것을 허용
- none : 아무것도 일치하지 않음
- self : 현재 출처와 일치하지만 하위 도메인은 일치하지 않음
- unsafe-inline : 인라인 자바스크립트 및 css를 허용
- unsafe-eval : eval같은 텍스트-자바스크립트 매커니즘을 허용