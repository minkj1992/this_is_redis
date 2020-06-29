# 기본 명령어

## String 명령

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

string을 정수로 파싱하고, 이를 atomic하게 증감하는 커맨드

```bash
> set counter 100
OK
> incr counter
(integer) 101
> incr counter
(integer) 102
> incrby counter 50
(integer) 152
```

키를 새 값으로 변경하고 이전 값을 반환하는 커맨드

```bash
> INCR mycounter
(integer) 1
> GETSET mycounter "0"
"1"
redis> GET mycounter
"0"
```

키가 이미 존재하거나, 존재하지 않을 때에만 데이터를 저장하게 하는 옵션

```bash
> set mykey newval nx
(nil)
> set mykey newval xx
OK
```

## List 명령어

- `lpush`
  - `O(1)`
  - 신기하게도 left push인데도 `O(1)`
- `lrange`
  - `O(S+N)`: s는 시작인덱스, 범위(worst case: O(N))에 속하는 요소의 갯수
  - `lrange <키> <시작인덱스> <종료인덱스>`
  - 파이썬처럼 `음수 표현식` 지원

```bash
redis:6379> lpush list:recommand java
(integer) 1
redis:6379> lpush list:recommand javascript
(integer) 2
redis:6379> lpush list:recommand python
(integer) 3
redis:6379> lpush list:recommand eclipse redis "eclipse plugin"
(integer) 6

redis:6379> lrange list:recommand
(error) ERR wrong number of arguments for 'lrange' command
redis:6379> lrange list:recommand 0 -1
1) "eclipse plugin"
2) "redis"
3) "eclipse"
4) "python"
5) "javascript"
6) "java"
```

## Set 명령어
- `sadd`
  - set에 멤버 추가
  - `O(n)`
- `smembers`
  - set 멤버 확인
  - `O(n)`

```bash
redis:6379> sadd my:test:set my
(integer) 1
redis:6379> sadd my:test:set mi
(integer) 1
redis:6379> sadd my:test:set me mine
(integer) 2

redis:6379> smembers my:test:set
1) "me"
2) "mine"
3) "my"
4) "mi"
```

## Ordered Set
> Set 데이터와 동일한 특징을 가지며 부가적으로 저장된 요소에 가중치를 부여하여 오름차순으로 정렬

> 같은 가중치에 대해서는 Unstable sort(정렬 이후 요소의 입력 순서가 바뀜)

> 내부적으로(2) zip-list, skip-list 자료구조로 구현되어있다.
> [Ordered Set 자료구조](http://redisgate.kr/redis/configuration/internal_skiplist.php)

- `zadd`
  - `O(log(N))`
- `zrange`
  - `O(log(N)+M)` -> `O(max(log(n),m))`
    - N: 입력된 값의 갯수
    - M: 조회된 값의 갯수 (범위)

```bash
redis:6379> zadd user:ranking 1 kris
(integer) 1
redis:6379> zadd user:ranking 2 leo
(integer) 1
redis:6379> zadd user:ranking 3 kakao
(integer) 1
redis:6379> zadd user:ranking 4 naver
(integer) 1
redis:6379> zadd user:ranking 5 semes
(integer) 1
redis:6379> zrange user:ranking 0 -1
1) "kris"
2) "leo"
3) "kakao"
4) "naver"
5) "semes"
redis:6379> zadd user:ranking 5 samsung
(integer) 1
redis:6379> zrange user:ranking 0 -1
1) "kris"
2) "leo"
3) "kakao"
4) "naver"
5) "samsung" #unstable sort
6) "semes"
redis:6379> zrange user:ranking 0 -1 withscores
 1) "kris"
 2) "1"
 3) "leo"
 4) "2"
 5) "kakao"
 6) "3"
 7) "naver"
 8) "4"
 9) "samsung"
10) "5"
11) "semes"
12) "5"
```

## Hash 명령어

- `hset`
  - add 또는 update(기 존재시)
- `hget`
  - 주어진 키에 대한 값 return
- `hgetall`
  - key와 값을 번갈아가며 표시해준다.

```bash
redis:6379> hset user:1 name minwook
(integer) 1
redis:6379> hset user:1 lastname jae
(integer) 1
redis:6379> hset user:1 company KakaoTalk
(integer) 1
redis:6379> hget user:1 company
"KakaoTalk"
redis:6379> hgetall user:1
1) "name"
2) "minwook"
3) "lastname"
4) "jae"
5) "company"
6) "KakaoTalk"
```