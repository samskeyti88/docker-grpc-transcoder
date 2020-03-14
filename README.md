## Setup gRPC Server

1. Run gRPC server to setup.

```
$ docker-compose run --rm server /bin/sh
```

2. Generate `.pb` from `.proto`.

```
# protoc \
   -I $GOPATH/src/github.com/grpc-ecosystem/grpc-gateway/third_party/googleapis \
   -I /go/src/pb/  \
   --include_imports  \
   --include_source_info \
   --descriptor_set_out=/go/src/pb/helloworld.pb \
   helloworld.proto
```

3. Generate `.pb.go` from `.proto`. gRPC server written by golang will use this file.

```
# protoc \
   -I $GOPATH/src/github.com/grpc-ecosystem/grpc-gateway/third_party/googleapis \
   --proto_path /go/src/pb \
   --go_out=plugins=grpc:/go/src/pb \
   helloworld.proto
```

4. Build golang file.

```
# cd /go/src/server &&  go build -i -v -o bin/server
```

5. Exit from gRPC server container. This container will be removed.

```
# exit
```

## Run Envoy and gRPC Server

```
$ docker-compose up -d
```

## Examples

### GET

```
$ curl  http://localhost:51051/hello  -v
*   Trying ::1...
* TCP_NODELAY set
* Connected to localhost (::1) port 51051 (#0)
> GET /hello HTTP/1.1
> Host: localhost:51051
> User-Agent: curl/7.54.0
> Accept: */*
> 
< HTTP/1.1 200 OK
< content-type: application/json
< x-envoy-upstream-service-time: 8
< grpc-status: 0
< grpc-message: 
< content-length: 30
< date: Sat, 14 Mar 2020 07:31:18 GMT
< server: envoy
< 
{
 "message": "Hello World"
}
* Connection #0 to host localhost left intact
```

### POST

```
$ curl -X POST http://localhost:51051/hello -d '{"name":"samskeyti88"}' -v
Note: Unnecessary use of -X or --request, POST is already inferred.
*   Trying ::1...
* TCP_NODELAY set
* Connected to localhost (::1) port 51051 (#0)
> POST /hello HTTP/1.1
> Host: localhost:51051
> User-Agent: curl/7.54.0
> Accept: */*
> Content-Length: 22
> Content-Type: application/x-www-form-urlencoded
> 
* upload completely sent off: 22 out of 22 bytes
< HTTP/1.1 200 OK
< content-type: application/json
< x-envoy-upstream-service-time: 0
< grpc-status: 0
< grpc-message: 
< content-length: 36
< date: Sat, 14 Mar 2020 07:50:14 GMT
< server: envoy
< 
{
 "message": "Hello samskeyti88"
}
* Connection #0 to host localhost left intact
```