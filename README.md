You can use this template to start a project with traefik proxy.
[how to use github templates](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-repository-on-github/creating-a-repository-from-a-template)

Proxy
=====

- for self signed certificates, you have to install [mkcert](https://github.com/FiloSottile/mkcert")
- you can change the `DOMAIN` name at `.env` file, default is fuf.me, its always point to 127.0.0.0 
- you can access traefik proxy at `http://proxy.<DOMAIN>` or `https://proxy.<DOMAIN>`
- `./run`: start the project (and generates self-signed certificates)
- `./down`: stop the project
- `./logs`: show the logs
- `./run <container> <command>`: execute <command> inside <container> 

**enable proxy for a container**

add the following code to the labels section of the container

```
        labels:
            - "traefik.enable=true"

            # enable https
            - "traefik.http.routers.proxy.rule=Host(`subdomain.${DOMAIN}`)"
            - "traefik.http.routers.proxy.entrypoints=websecure"
            - "traefik.http.routers.proxy.service=api@internal"
            - "traefik.http.routers.proxy.tls=true"

            # enable http
            - "traefik.http.routers.proxy-http.rule=Host(`subdomain.${DOMAIN}`)"
            - "traefik.http.routers.proxy-http.entrypoints=web"
            - "traefik.http.routers.proxy-http.service=api@internal"
```