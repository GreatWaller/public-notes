# Docker

### 1 多阶段构建

```dockerfile
FROM golang AS build-env
ADD . /go/src/app
WORKDIR /go/src/app
RUN go get -u -v github.com/kardianos/govendor
RUN govendor sync
RUN GOOS=linux GOARCH=386 go build -v -o /go/src/app/app-server

FROM alpine
RUN apk add -U tzdata
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime
COPY --from=build-env /go/src/app/app-server /usr/local/bin/app-server
EXPOSE 8080
CMD [ "app-server" ]
```

build

```shell
$ docker build -t cnych/docker-multi-stage-demo:latest .
```

可以用`AS`指令为阶段命令，比如我们这里的将第一阶段命名为`build-env`，然后在其他阶段需要引用的时候使用`--from=build-env`参数即可

run

```shell
$ docker run --rm -p 8080:8080 cnych/docker-multi-stage-demo:latest
```

