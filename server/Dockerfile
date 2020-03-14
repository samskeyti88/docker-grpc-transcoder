FROM alpine:3.8

RUN apk add --no-cache curl git go unzip musl-dev libc6-compat

ENV PROTOBUF_VERSION 3.11.4

RUN curl -sL https://github.com/google/protobuf/releases/download/v${PROTOBUF_VERSION}/protoc-${PROTOBUF_VERSION}-linux-x86_64.zip -o /tmp/protoc.zip && \
    unzip /tmp/protoc.zip -d /tmp && \
    cp -r /tmp/bin /tmp/include /usr/local/ && \
    rm -rf /tmp/*

ENV GOPATH /go
ENV PATH $PATH:$GOPATH/bin:/usr/local/go/bin

RUN mkdir /go && \
    go get -u google.golang.org/grpc && \
    go get -u github.com/golang/protobuf/protoc-gen-go && \
    go get -u github.com/grpc-ecosystem/grpc-gateway/protoc-gen-grpc-gateway && \
    go get -u github.com/grpc-ecosystem/grpc-gateway/protoc-gen-swagger