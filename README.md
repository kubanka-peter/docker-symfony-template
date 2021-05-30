You can use this template to start a project with traefik proxy.
[how to use github templates](https://docs.github.com/en/github/creating-cloning-and-archiving-repositories/creating-a-repository-on-github/creating-a-repository-from-a-template)

- for self signed certificates, you have to install [mkcert](https://github.com/FiloSottile/mkcert")
- `./run`: start the project (and generates self-signed certificates)
- `./down`: stop the project
- `./logs`: show the logs
- `./run <container> <command>`: execute <command> inside <container> 