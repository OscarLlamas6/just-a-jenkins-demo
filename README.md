# Just a Jenkins demo


## Instalar Jenkins con Docker paso a paso con linea de comandos

```bash

# Build the Jenkins BlueOcean Docker Image
> docker build -t myjenkins-blueocean:2.332.3-1 .

# Create the network 'jenkins'
> docker network create jenkins

## MacOS / Linux

> docker run --name jenkins-blueocean --restart=on-failure --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:2.332.3-1

# Windows
> docker run --name jenkins-blueocean --restart=on-failure --detach `
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 `
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 `
  --volume jenkins-data:/var/jenkins_home `
  --volume jenkins-docker-certs:/certs/client:ro `
  --publish 8080:8080 --publish 50000:50000 myjenkins-blueocean:2.332.3-1


## Get the Password
> docker exec jenkins-server cat /var/jenkins_home/secrets/initialAdminPassword

# Connect to the Jenkins
> https://localhost:8080/
```

## Instalar Jenkins con Docker Compose

```bash

# Crear imagen docker
> docker-compose -f ./docker-compose.yaml build

# Revisar sintaxis ficker .yaml
> docker-compose -f ./docker-compose.yaml config

# Levantar servicio Jenkins
> docker-compose -f ./docker-compose.yaml up -d

## Obtener password para Jenkins
> docker exec jenkins-server cat /var/jenkins_home/secrets/initialAdminPassword

# Abrir Jenkins UI
> http://localhost:8080/

# Accediendo al sistema de archivos de Jenkins
> docker exec -it jenkins-server bash

## alpine/socat container to forward traffic from Jenkins to Docker Desktop on Host Machine
> docker run -d --restart=always -p 127.0.0.1:2376:2375 --network jenkins -v /var/run/docker.sock:/var/run/> docker.sock alpine/socat tcp-listen:2375,fork,reuseaddr unix-connect:/var/run/docker.sock
> docker inspect <container_id> | grep IPAddress

> docker run -d --restart=always -p 127.0.0.1:2376:2375 --network just-a-jenkins-demo_jenkins_network -v /var/run/docker.sock:/var/run/docker.sock alpine/socat tcp-listen:2375,fork,reuseaddr unix-connect:/var/run/docker.sock

```