# NextJS - 기본구조

NextJS의 기본 구조는 아래와 같습니다.

> pages/  
>     \_document.js  
>      \_app.js  
>      \_error.js  
>     index.js  
> static/  
> next.config.js

먼저 위의 내용을 하나씩 설명해보자면 \_\_document.js는 기존의 index.html과 동일하다고 보면 됩니다.  
그리고 \_\_app.js는 CRA의 APP.js와 동일한 역할로서 어플리케이션의 공통적인 레이아웃을 작성하는 파일입니다.  
\_\_error.js는 Error처리를 할때 사용하는 파일입니다.  
생략된경우 NextJS의 기본 값을 사용합니다.  
next.config.js는 webpack plugin들과 Next.js의 라우팅 설정을 작성하는 곳입니다.



참조: [https://salgum1114.github.io/nextjs/2019-05-06-nextjs-static-website-1/](https://salgum1114.github.io/nextjs/2019-05-06-nextjs-static-website-1/)



