You can use this template to start developing a symfony application. 
[how to use github templates](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-repository-on-github/creating-a-repository-from-a-template)

It's configured for local development mostly with default settings, but with some changes 
you can use it for production (Dockerfile is multipart, so you can use the "base" target, then extend it with your changes).

Install
=======
- you have to install [mkcert](https://github.com/FiloSottile/mkcert"), to enable self signed cert generation and
  to add it to your local cert store
- copy .env.example to .env and make the required changes
  - you have to change `USER_ID` and `GROUP_ID` if yours is different to prevent permission errors
- execute `./up`, wait till it completes
- if it's a new project, you have to install the symfony framework
  - you can do it by execute `./run symfony new --no-git .` or `./run symfony new --no-git . --full` (symfony is the official symfony installer) 
- you can access the new project at at `http://app.fuf.me` or `https://app.fuf.me`
  
How to use
==========

- execute composer and any php scripts (symfony commands) inside the container
- project directory is excluded from the git, you should make a new git repository and commit only your symfony project
- this docker-compose template should live in its own repository, separated from you symfony project 
- execute git outside the container (so, you don't have to share your private keys with the container)   
  
Helper scripts
==============

- `./up`: start the project (and generates self-signed certificates)
- `./down`: stop the project
- `./logs`: show the logs
- `./run <command>`: execute <command> inside symfony container (e.g composer)
- `./sf <command>`: execute bin/console <command> inside symfony container


Proxy
=====

- for self signed certificates, you have to install [mkcert](https://github.com/FiloSottile/mkcert")
- you can change the `DOMAIN` name at `.env` file, default is fuf.me, its always point to 127.0.0.0 
- you can access traefik proxy at `http://proxy.<DOMAIN>` or `https://proxy.<DOMAIN>` 

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