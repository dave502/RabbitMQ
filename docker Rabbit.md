https://registry.hub.docker.com/_/rabbitmq/


```
docker run -d --hostname host-rabbit --name rabbit --rm -p 15672:15672 rabbitmq:3-management
```

Или

```console
$ docker run -d --hostname my-rabbit --name some-rabbit -e RABBITMQ_DEFAULT_USER=user -e RABBITMQ_DEFAULT_PASS=password rabbitmq:3-managemen
```

Docker-compose:
```
version: "3.8"

services:
  rabbit:
    hostname: 'rabbit'
    image: rabbitmq:3-management
    restart: always
    expose:
      - 5672:5672 #amqp
      - 15672:15672 #http
      - 15692:15692  #prometheus
    volumes:
      - ./rabbit-vol:/var/lib/rabbitmq
    environment:
      RABBITMQ_ERLANG_COOKIE: ${RABBITMQ_ERLANG_COOKIE:-secret_cookie}
      RABBITMQ_DEFAULT_USER: ${RABBITMQ_DEFAULT_USER:-admin}
      RABBITMQ_DEFAULT_PASS: ${RABBITMQ_DEFAULT_PASS:-admin}
      # для прода желательно задать пороговое значение 2 гигабита
      #RABBITMQ_SERVER_ADDITIONAL_ERL_ARGS=-rabbit log_levels [{connection,error},{default,error}] disk_free_limit 2147483648
    healthcheck:
      test: rabbitmq-diagnostics -q ping
      interval: 30s
      timeout: 30s
      retries: 3
```