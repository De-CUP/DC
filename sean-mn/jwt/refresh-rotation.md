# 인증/인가를 안전하게 처리하기! (Refresh Token Rotation)

## Access Token과 그 한계

Access Token은 사용자가 인증된 상태임을 증명하는 토큰이다.  
만료 시간이 길면 편리하지만, 탈취당했을 때 공격자가 오랫동안 악용할 수 있다는 문제가 있다.  
그렇기 때문에 Access Token의 만료 시간은 짧게 유지하는 것이 일반적이다.

## Refresh Token이란?

Access Token의 만료 시간을 짧게 유지하면, 사용자는 자주 로그인을 다시 해야 하는 불편함이 생긴다.  
이를 해결하기 위해 Refresh Token을 사용한다.

Refresh Token은 Access Token을 재발급받기 위한 토큰으로, 상대적으로 긴 만료 시간을 가진다.  
Access Token이 만료되면 클라이언트는 Refresh Token을 서버에 전달하고,
서버는 이를 검증한 뒤 새로운 Access Token을 발급해준다.  
덕분에 사용자는 로그아웃 없이 로그인 상태를 유지할 수 있다.

## Refresh Token의 취약점

하지만 Refresh Token도 탈취 가능성이 있다.  
만료 시간이 길기 때문에, 탈취당하면 공격자는 오랫동안 Access Token을 계속 발급받을 수 있다.

## Refresh Token Rotation (RTR)

Refresh Token Rotation은 Access Token을 재발급할 때 Refresh Token도 함께 교체하는 방식이다.  
즉, Refresh Token을 일회용으로 만들어 재사용을 불가능하게 함으로써 보안을 강화한다.

동작 흐름은 다음과 같다.

1. 클라이언트가 Refresh Token을 서버에 전달한다.
2. 서버는 해당 토큰을 검증하고, 새로운 Access Token과 새로운 Refresh Token을 함께 발급한다.
3. 기존 Refresh Token은 즉시 무효화된다.

## Reuse Detection - 탈취 감지

RTR의 진짜 강점은 탈취된 토큰을 감지할 수 있다는 점이다.

공격자가 탈취한 Refresh Token으로 재발급을 시도하면, 서버는 이미 사용된 토큰임을 감지한다.  
이 시점에서 서버는 해당 사용자의 모든 세션을 강제 종료시킨다.  
정상 사용자도 로그아웃되지만, 공격자의 접근을 차단할 수 있다.

반대로, 정상 사용자가 먼저 토큰을 교체했다면 공격자의 구 토큰은 이미 무효화된 상태이므로  
재발급 시도 자체가 차단된다.