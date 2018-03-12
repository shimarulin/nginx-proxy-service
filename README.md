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

## Setup your local development environment

To locally resolve your service by name, you must also configure the DNS service. Probably the most common way 
to do this is to use *dnsmask*. You can setup dnsmask with or without NetworkManager.

### Setup dnsmask without NetworkManager

1. Install dnsmasq

    ```bash
    sudo apt-get -y install dnsmasq
    ```
    
2. Add your dnsmasq config:

    ```bash
    cat <<EOT | sudo tee -a /etc/dnsmasq.d/local
    ### Docker config
    local=/docker/
    listen-address=127.0.0.1,172.17.0.1
    address=/docker/172.17.0.1
    strict-order
    
    EOT
    ```

3. Restart dnsmasq service:

    ```bash
    sudo service dnsmasq restart
    ```
    
### About local domain zone

In [article](https://ma.ttias.be/chrome-force-dev-domains-https-via-preloaded-hsts/)
Mattias Geniar wrote:

> Chrome & Firefox now force .dev domains to HTTPS via preloaded HSTS...
  A lot of (web) developers use a local .dev TLD for their own development...
  With .dev being an official gTLD, we're most likely better of changing our preferred local development suffix from 
  .dev to something else.

Earlier, I also used the `.dev` domain zone for my development environment, but in the changed conditions 
I prefer to specify zone `.docker` as a reference to use Docker.
