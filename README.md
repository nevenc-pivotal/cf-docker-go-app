# cf-docker-go-app

## Description
This is a simple go app that is "dockerized".

## Create a Docker image
* Make sure you have `go` installed.
  * On MacOS, I like to use `brew install go` to install `go`.
* Create a local Docker image
  * Use `make` to compile go image and create a Docker image
```
neven@nc.local:~/cf-docker-go-app$ make

GOARCH=amd64 GOOS=linux go build -o dockerapp dockerapp.go
docker build -t nevenc/cf-docker-go-app:latest .
Sending build context to Docker daemon 7.041 MB
Step 1 : FROM busybox
latest: Pulling from library/busybox
5506dda26018: Pull complete 
65e4158d9625: Pull complete 
Digest: sha256:97473e34e311e6c1b3f61f2a721d038d1e5eef17d98d1353a513007cf46ca6bd
Status: Downloaded newer image for busybox:latest
 ---> 65e4158d9625
Step 2 : MAINTAINER https://github.com/cloudfoundry-incubator/diego-dockerfiles
 ---> Running in 5913dd4c804e
 ---> 1534e899e632
Removing intermediate container 5913dd4c804e
Step 3 : ENV VCAP_APPLICATION {}
 ---> Running in 6ae736a08d16
 ---> 05638d50fa9c
Removing intermediate container 6ae736a08d16
Step 4 : ENV BAD_QUOTE \'
 ---> Running in c8c65977c53b
 ---> ecdf4ab11ff2
Removing intermediate container c8c65977c53b
Step 5 : ENV BAD_SHELL \$1
 ---> Running in 35a92005dbd7
 ---> 8d1cf3a612cd
Removing intermediate container 35a92005dbd7
Step 6 : ENV HOME /home/some_docker_user
 ---> Running in 41be824a2f3e
 ---> e0f2b5a8ff91
Removing intermediate container 41be824a2f3e
Step 7 : ENV SOME_VAR some_docker_value
 ---> Running in 92e84f9d618c
 ---> a8dfd0b4857e
Removing intermediate container 92e84f9d618c
Step 8 : ENV PATH /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/myapp/bin
 ---> Running in b18f1c4a0dac
 ---> 63e94fc07b51
Removing intermediate container b18f1c4a0dac
Step 9 : EXPOSE 8080
 ---> Running in 0a90d5919e9f
 ---> d69f4c71c9d8
Removing intermediate container 0a90d5919e9f
Step 10 : COPY dockerapp /myapp/bin/dockerapp
 ---> c4e2db9eea86
Removing intermediate container 667747c7c5d1
Step 11 : WORKDIR /myapp
 ---> Running in 9bdcbb3fb7bd
 ---> ae4452e95e20
Removing intermediate container 9bdcbb3fb7bd
Step 12 : RUN adduser -D vcap
 ---> Running in a5d70b0d1752
 ---> d0013f06419f
Removing intermediate container a5d70b0d1752
Step 13 : CMD dockerapp
 ---> Running in 5a132b9d4d08
 ---> 77d6a79b8749
Removing intermediate container 5a132b9d4d08
Successfully built 77d6a79b8749
rm dockerapp
```

## Push a Docker image to Repository
* I like to use public Docker Hub repository.
* Push the created image to repository
  * Use `docker push` to push an image
```
neven@nc.local:~/cf-docker-go-app$ docker push nevenc/cf-docker-go-app:latest
The push refers to a repository [docker.io/nevenc/cf-docker-go-app] (len: 1)
e640a82aa746: Pushed 
f51b703a7e02: Pushed 
60c14ca05102: Pushed 
4f1f2973e6f6: Pushed 
147fb79a8173: Pushed 
983b6e12ef1b: Pushed 
ec0353926431: Pushed 
c3af1dabaf8c: Pushed 
9d0798ba89f2: Pushed 
5300f1e78e1f: Pushed 
9b34e89e6002: Pushed 
c238f95f9269: Pushed 
5506dda26018: Pushed 
latest: digest: sha256:49aac4541873642bf97ee4c986a80a56e1df60b5e8804029088d815aeba2bbad size: 24425
```

## Run Docker image on PCF
* Make sure that Docker feature is enabled on your Diego cell, e.g. `cf feature-flag diego_docker`
* Push the public Docker image to your PCF
  * e.g. `cf push cf-docker-go-app -o nevenc/cf-docker-go-app:latest -m 20M`

## Test Docker Application on PCF
* Make sure you record the URL for your pushed application, e.g. https://cf-docker-go-app.your.pcf.domain.here
* Test in your browser
  * https://cf-docker-go-app.your.pcf.domain.here
  * https://cf-docker-go-app.your.pcf.domain.here/env
* Test using curl, e.g.
  * `curl https://cf-docker-go-app.your.pcf.domain.here`
  * `curl https://cf-docker-go-app.your.pcf.domain.here/env`


## Notes
See original example at https://github.com/cloudfoundry-incubator/diego-dockerfiles




