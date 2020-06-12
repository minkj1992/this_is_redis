# redis 성능 측정
## `redis-benchmark`
> 전반적인 redis 서버 성능 측정
```shell
$ docker exec -it myredis /bin/bash
root@a4434086b48b:/data# redis-benchmark
```

## `redis-cli <option>`
> redis-cli 클라이언트 성능 측정

응답 시간 측정
```shell
root@a4434086b48b:/data# redis-cli --latency
min: 0, max: 1, avg: 0.32 (3569 samples)
```

주기적인 통계 정보 조회

메모리 상태, 저장된 키의 갯수와 같은 서버의 통계 정보를 주기적으로 확인해야 한다면 info를 사용하자

```shell
root@a4434086b48b:/data# redis-cli info cpu
root@a4434086b48b:/data# redis-cli info clients
root@a4434086b48b:/data# redis-cli info server
```