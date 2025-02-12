데이터 감사 로그를 기록하기 위해 테이블에 created_at, created_by, updated_at, updated_by, deleted_at, deleted_by 필드를 추가하라는 요구 사항이 있었다. 이러한 Audit 필드만 관리하는 테이블을 따로 만들 경우 조회 시 모든 테이블에 대한 데이터를 스캔해야 하기 때문에 비효율적이다.
