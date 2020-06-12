# 기본 명령어

## 문자열 명령

- `set`
  - 주어진 키에 값을 저장
  - 키:값 = 1:1
- `append`
  - 주어진 키가 존재하면 이미 저장된 제일 마지막 값 뒤에 추가
  - 단, 문자열 값만 유효
  - key가 존재하지 않으면 set 명령과 동일
- `get`
  - 키를 주어 값을 받는다.

```bash
redis:6379> set user:name "leo"
OK
redis:6379> set user:name "minkj1992"
OK
redis:6379> append user:name "jejulover"
(integer) 18
redis:6379> get user:name
"minkj1992jejulover"
```

- 참고사항
  - redis의 `키이름`에는 `:`과 같은 내용도 들어갈 수 있다.
  - 심지어 바이트 배열도 키로 사용할 수 있다.
  - 다만, 관례상 `:`은 키에 의미를 부여하는 구분자로 사용된다.

- `incr`(증가) / `decr`(감소)
  - 키에 저장된 값을 1씩 증가시키고 그 결과 값을 반환한다.
  - 단 저장된 값이 숫자일 때만 수행된다.
  - 참고로 `문자열의 숫자 증감처리`는 레디스가 문자열을 다루는 특별한 방법의 하나다. ( 문자열의 값이 숫자일때, 증감 명령을 허용한다.)
  - 이외에도 문자열 비트연산 제공

```bash
redis:6379> set login:counter "0"
OK
redis:6379> incr login:counter
(integer) 1
redis:6379> incr login:counter
(integer) 2
redis:6379> incr login:counter
(integer) 3
redis:6379> get login:counter
"3"
```