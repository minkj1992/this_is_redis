# Docker Redis 시작하기
> [redisgate](http://redisgate.kr/redis/education/docker_intro.php)



## redis 설치

도커 update

```bash
sudo apt-get update
sudo apt-get upgrade docker
```

redis image pull

```bash
docker pull redis
```

redis server container run
```bash
docker run --name myredis -d -p 6379:6379 redis
```

redis-cli run with generated redis container
```bash
docker run -it --link myredis:redis --rm redis redis-cli -h redis -p 6379
```

## -d 옵션으로 돌고있는 redis 서버에 접속하기

    docker exec -it myredis /bin/bash
