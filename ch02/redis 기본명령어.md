# 기본 명령어

## 문자열 명령

- set
  - 주어진 키에 값을 저장
  - 키:값 = 1:1
- append
  - 주어진 키가 존재하면 이미 저장된 제일 마지막 값 뒤에 추가
  - 단, 문자열 값만 유효
  - key가 존재하지 않으면 set 명령과 동일
- get
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