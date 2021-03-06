# JWT \(Json Web Token\)

> JWT이란 웹 표준\(RFC 7519\)로써 두 개체에서 JSON 객체를 사용하여 가볍고 자가수용적인\(self-contained\) 방식으로 정보를 안정성 있게 전달해줍니다

JWT 토큰의 자가수용적인이란 JWT는 기본적으로 필요한 정보를 자체적으로 지니고 있는 것을 의미하며, 또한 HTTP 헤더에 넣어서 전달할 수도 있고 URL의 파라미터로 전달할 수 있음을 말합니다.

JWT는 회원인증이나 정보 교류에 사용되며 정보를 서명하기 때문에 정보가 조작되진 않았는지 검증할 수 있습니다.

JWT는 aaaa.bbbb.cccc의 구조를 가지며 먼저 헤더\(header\), 내용\(payload\), 서명\(signautre\)으로 구성이됩니다.

먼저 헤더\(Header\)는 typ와 alg 두 정보만을 가지고 있으 typ는 토큰의 타입을 지정하는 것이고, alg는 사용된 해싱 알고리즘을 의미합니다.  
보통 해싱 알고리즘으로는 HMAC, SHA256 또는 RSA를 사용하며 이 알고리즘은 토큰을 검증할때 사용되는 서명부분에서 사용됩니다.  
`{  
  "typ: "JWT",  
  "alg": "HS256"  
}`     
이 예제는 JWT 토큰이면서 이 것은 HMAC SHA256이 해싱 알고리즘으로 사용된다는 뜻입니다.

내용\(Payload\)에는 토큰에 담을 정보들이 들어있습니다.  
정보는 키와 값으로 이루어져 있는데 이 한 쌍을 이를 클레임이라고 합니다.  
클레임에는 이미 이름이 정해진 것들이 존재하는데 이 내용은 선택적이며 선택에 따라 넣거나 빼거나하면됩니다.  
- `iss` : 토큰 발행자  
- `sub` : 토큰제목  
- `aud` : 토큰 대상  
- `exp` : 토큰의 만료시간  
- `nbf` : 이 날짜가 지나기 전까지는 토큰이 처리되지 않음  
- `iat` : 토큰이 발급된 시간  
- `jti` : JWT의 고유 식별자로서, 주로 중복적인 처리를 방지하기 위하여 사용 \( 주로 일회용 토큰에 사용 \)

서명\(Signature\)는 헤더의 인코딩값과 정보의 인코딩 값을 합친뒤 주어진 비밀키로 해쉬를 하여 생성합니다. 이렇게 많은 해쉬를 base64 형태로 나타냅니다.

