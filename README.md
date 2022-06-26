# github self hosted runner

- diferentes runners segun el entorno: https://jonico.github.io/awesome-runners/
- aspectos de seguridad: https://github.com/dduzgun-security/github-self-hosted-runners

Link directo a los runners:
https://github.com/sosan/self-runner-on-vps-to-do/settings/actions/runners

Esta en Settings -> Actions/Runners

click boton New self hosted runner
copiamos el token del self runner AAPYBGPKQ3UMKEO5QECBGC3CXCH4I(ejemplo)


En el vps, tenemos montado un docker sin rootless (TODO: rootless)
> NOTA: si seguimos las instrucciones del github, se ejecutara un runner, y cuando queramos borrarlo podemos ir al github y click boton borrar y se borraran los servicios del vps referentes al github runner

usando imagen https://hub.docker.com/r/myoung34/github-runner

Config basica runner:

```
docker run -d --restart always --name github-runner \
  -e REPO_URL="https://github.com/sosan/self-runner-on-vps-to-do" \
  -e RUNNER_NAME="linux-terraform" \
  -e RUNNER_TOKEN="AAPYBGPKQ3UMKEO5QECBGC3CXCH4I" \
  -e RUNNER_WORKDIR="/tmp/github-runner-self-runner-on-vps-to-do" \
  -v /tmp/github-runner-self-runner-on-vps-to-do:/tmp/github-runner-self-runner-on-vps-to-do \
  -v /var/run/docker.sock:/var/run/docker.sock \
  myoung34/github-runner:latest 
```
```
docker logs github-runner
```

muestra:
```
# Authentication
√ Connected to GitHub
# Runner Registration
√ Runner successfully added
√ Runner connection is good
# Runner settings
√ Settings Saved.
√ Connected to GitHub
Current runner version: '2.294.0'
2022-06-26 15:58:21Z: Listening for Jobs
```
en el github seccion actions/runners tendria que poner idle

mas info:
https://docs.github.com/en/actions/hosting-your-own-runners/adding-self-hosted-runners#adding-a-self-hosted-runner-to-a-repository


Desde github añadimos un token llamado: `DIGITALOCEAN_ACCESS_TOKEN` con el PAT de digitalocean

DIGITALOCEAN_ACCESS_TOKEN=dop_v1_d8eeed703665XXXXXXXXXXX18d0021319XXXXXXXXX(ejemplo)

PETA: al crear un nginx con helm 