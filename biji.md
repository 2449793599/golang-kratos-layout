# Kratos Project Template

## 安装KRATOS
```
go install github.com/go-kratos/kratos/cmd/kratos/v2@latest
```
## 创建服务
```
kratos new server

cd server

kratos proto add api/server/server.proto    # 添加PROTO文件
kratos proto client api/server/server.proto # 生成PROTO代码
kratos proto server api/server/server.proto -t internal/service # 生成服务代码

go generate ./...
go build -o ./bin/ ./...
./bin/server -conf ./configs
```
## Generate other auxiliary files by Makefile
```
# Download and update dependencies
make init
# Generate API files (include: pb.go, http, grpc, validate, swagger) by proto file
make api
# Generate all files
make all
```
## Automated Initialization (wire)
```
# install wire
go get github.com/google/wire/cmd/wire

# generate wire
cd cmd/server
wire
```

## Docker
```bash
# build
docker build -t <your-docker-image-name> .

# run
docker run --rm -p 8000:8000 -p 9000:9000 -v </path/to/your/configs>:/data/conf <your-docker-image-name>
```

