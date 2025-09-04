# KRATOS 项目模板

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

# 接口定义

参考文档：https://blog.csdn.net/m0_57836225/article/details/149691068

1. 对外接口使用PROTO进行一次定义（用于描述数据结构和服务）
2. 根据PROTO文件同时生成对应的GRPC代码（用于服务间高效通信）和HTTP代码（方便前端调用）
3. 生成HTTP代码需要借助GRPC-GATEWA（将GRPC服务转换为HTTP-RESTFUL-API的工具）

- 引入HTTP映射能力
```
import "google/api/annotations.proto";
```

- 用于每个RPC方法定义对应的HTTP方法和路径
```
option (google.api.http)
```

- 使用注解添加参数验证规则
```
protoc-gen-validate
```
甚至还能自动生成Internal层和Handler层


自动文档：同步生成OPENAPI文档，省去手写API文档的麻烦