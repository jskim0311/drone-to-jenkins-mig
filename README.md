# drone-to-jenkins-mig
CI Migration from drone.io to Jenkins

Jenkins

Install) Docker https://www.jenkins.io/doc/book/installing/docker/
Plugin) Docker Compose Build Step https://plugins.jenkins.io/docker-compose-build-step/



### Jenkins Run by docker

```
$ docker network create jenkins

$ docker run --name jenkins-docker --rm --detach \
    --privileged --network jenkins --network-alias docker \
    --env DOCKER_TLS_CERTDIR=/certs \
    --volume jenkins-docker-certs:/certs/client \
    --volume jenkins-data:/var/jenkins_home \
    --publish 2376:2376 \
    docker:dind --storage-driver overlay2


$ docker run --name myjenkins-2 \
    --restart=on-failure --detach \
    --network jenkins \
    --env DOCKER_HOST=tcp://docker:2376 \
    --env DOCKER_CERT_PATH=/certs/client \
    --env DOCKER_TLS_VERIFY=1 \
    --publish 8080:8080 \
    --publish 50000:50000 \
    --volume jenkins-data:/var/jenkins_home \
    --volume jenkins-docker-certs:/certs/client:ro \
    myjenkins:0.2
```



clear docker-compose
1.Stop the container(s) using the following command:
```
$ docker-compose down
```
2.Delete all containers using the following command:
```
$ docker rm -f $(docker ps -a -q)
```
Delete all volumes using the following command:
```
$ docker volume rm $(docker volume ls -q)
```
4.Restart the containers using the following command:
```
$ docker-compose up -d
```

