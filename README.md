# [jwilder/nginx-proxy](https://github.com/jwilder/nginx-proxy) as service

## Getting started

In first you must create Docker network for nginx-proxy

```bash
docker network create nginx-proxy
```

after it you can run this service

```bash
docker-compose up -d
```

and use it in your Docker Compose files

```yaml
version: "3.4"
services:

  myapp:
    environment:
      VIRTUAL_HOST: ${APP_HOST}
      VIRTUAL_PORT: ${APP_PORT}
    expose:
     - ${APP_PORT}

networks:
  default:
    external:
      name: nginx-proxy
```

Note: The above configuration uses environment variables. You can use `.env` for this or set it directly.
