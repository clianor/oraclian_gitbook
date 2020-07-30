# HTTP 상태 코드

HTTP 상태 코드는 10x, 2xx, 30x, 4xx, 5xx 이렇게 1~50x 까지 존재합니다.  
각각의 숫자는 의미를 가지고 있는데 우리가 웹 페이지를 보다보면 가장 많이 볼 수 있는 상태코드가 바로 404 Not Found입니다.

각각의 숫자에 대한 의미는 아래와 같습니다.

1.  10x는 조건부 응답
2.  20x는 통신 성공
3.  30x는 리다이렉트
4.  40x는 클라이언트 오류
5.  50x는 서버오

자주 쓰이는 HTTP 상태 코드는 아래와 같으며 10x는 직접 본적이 없으므로 넘어가겠습니다.

* 2xx

| 상태코드 | 이름 | 의 |
| :--- | :--- | :--- |
| 200 | 성공 | 요청 성공\(GET\) |
| 201 | 작성됨 | 서버가 새 리소스를 작성함\(POST\) |
| 202 | 허용됨 | 서버가 요청은 접수했으나 아직 처리하지  |
| 203 | 신뢰할 수 없는 정보 | 서버가 요청을 성공적으로 처리했지만 다른 소스에서 수신된 정보를 제공하고 있다. \(캐싱된 데이터\) |
| 204 | 콘텐츠 없음 | 서버가 요청을 성공적으로 처리했지만 콘텐츠를 제공하지 않음 |
| 205 | 콘텐츠 재설정 | 204와 다른점은 Client의 뷰를 새로고침하라는 응답 |

* 30x

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xC0C1;&#xD0DC;&#xCF54;&#xB4DC;</th>
      <th style="text-align:left">&#xC774;&#xB984;</th>
      <th style="text-align:left">&#xC758;&#xBBF8;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">301</td>
      <td style="text-align:left">&#xC601;&#xAD6C; &#xC774;&#xB3D9;</td>
      <td style="text-align:left">
        <p>Location &#xD5E4;&#xB354;&#xAC00; &#xC9C0;&#xC815;&#xD55C; URL&#xB85C;
          &#xC601;&#xAD6C; &#xC774;&#xB3D9;&#xB418;&#xC5C8;&#xC74C;&#xC744; &#xB098;&#xD0C0;&#xB0C4;</p>
        <p>HTTP Method&#xC758; &#xBCC0;&#xACBD; &#xBD88;&#xAC00;&#xB2A5; (&#xBCC0;&#xACBD;
          &#xD544;&#xC694;&#xC2DC; &#xB610;&#xB294; POST &#xC694;&#xCCAD;&#xC2DC;
          308 &#xC0AC;&#xC6A9; &#xAD8C;&#xC7A5;)</p>
        <p>&#xAC80;&#xC0C9;&#xC5D4;&#xC9C4;&#xC5D0; &#xC774;&#xB3D9;&#xB41C; URL&#xB85C;
          &#xC5C5;&#xB370;&#xC774;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">302</td>
      <td style="text-align:left">&#xC784;&#xC2DC; &#xC774;&#xB3D9;</td>
      <td style="text-align:left">
        <p>Location &#xD5E4;&#xB354;&#xAC00; &#xC9C0;&#xC815;&#xD55C; URL&#xB85C;
          &#xC784;&#xC2DC; &#xC774;&#xB3D9;&#xB418;&#xC5C8;&#xC74C;&#xC744; &#xB098;&#xD0C0;&#xB0C4;
          <br
          />HTTP Method&#xC758; &#xBCC0;&#xACBD; &#xBD88;&#xAC00; (&#xBCC0;&#xACBD;
          &#xD544;&#xC694;&#xC2DC; 307 &#xC0AC;&#xC6A9; &#xAD8C;&#xC7A5;)</p>
        <p>&#xAC80;&#xC0C9;&#xC5D4;&#xC9C4;&#xC5D0; &#xC774;&#xB3D9;&#xB41C; URL&#xB85C;
          &#xC5C5;&#xB370;&#xC774;&#xD2B8; X</p>
      </td>
    </tr>
  </tbody>
</table>

* 4xx

| 상태코드 | 이름 | 의 |
| :--- | :--- | :--- |
| 400 | 잘못된 요청 | 클라이언트에서 잘못된 요청이 날아옴 |
| 401 | 권한 없음 | 인증 실패 |
| 403 | 금지됨 | 사용자가 리소스에 대한 필요 권한을 갖고 있지 않 |
| 404 | 찾을 수 없음 | 서버가 요청한 페이지를 찾을 수 없 |
| 405 | 허용되지 않는 방법 | POST 방식으로 요청을 받는 서버에 GET 요청을 보내는 경우, 또는 읽기 전용 리소스에 PUT 요청을 보내는 경우에 이 코드를 제공 |
| 414 | 요청 URI가 너무 긺 | 서버가 처리할 수 한계를 넘은 길이의 요청 URL이 포함된 요청을 클라가 보냈을 때 사용 |
| 429 | 너무 많은 요청 | 사용자가 일정 시간 동안 너무 많은 요청을 보냈 |

* 5xx

| 상태코드 | 이름 | 의미 |
| :--- | :--- | :--- |
| 500 | 내부 서버 오류 | 서버가 요청을 처리할 수 없게 만드는 에러를 만났을 때 사용 |
| 501 | 구현되지 않음 | 클라가 서버의 능력을 넘은 요청을 했을 때 사용한다 |
| 502 | 불량 게이트웨이 | 게이트웨이 오류 |
| 504 | 시간초과 | 다른 서버에게 요청을 보내고 응답을 기다리다 타임아웃이 발생 |

