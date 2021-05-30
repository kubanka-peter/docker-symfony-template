You can use this template to start developing a symfony application. 
[how to use github templates](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-repository-on-github/creating-a-repository-from-a-template)

It's configured for local development, but with some changes you can use it for production (Dockerfile is multipart, you can use the "base" target, then extend it with your changes),
but production environments are different for project to project, so you have to configure it for yourself.

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

Add the following code to the labels section of the container.
Dont forget to replace <server> with the unique name of the server, or it will not work.

```
        labels:
            - "traefik.enable=true"

            # enable https
            - "traefik.http.routers.<server>.rule=Host(`subdomain.${DOMAIN}`)"
            - "traefik.http.routers.<server>.entrypoints=websecure"
            - "traefik.http.routers.<server>.tls=true"

            # enable http
            - "traefik.http.routers.<server>-http.rule=Host(`subdomain.${DOMAIN}`)"
            - "traefik.http.routers.<server>-http.entrypoints=web"
```