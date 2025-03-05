외부 서비스 호출 시 장애로 인해 응답이 지연될 경우 응답을 받기 위해 대기하는 자원들이 운영 서버 내부에 쌓이면서 성능 저하를 발생시킬 수 있을 텐데, 이를 어떻게 해결할 수 있을까?

#### 외부 서비스를 호출할 때 네트워크 장애 또는 외부 서비스 장애 발생 시 조치 방법
1. Timeout
2. Bulkhead pattern
3. Circuit Breaker

#### Timeout
Connection Timeout, HTTP Connection Pool Timeout, Read Timeout 설정 적용
