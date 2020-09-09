# Docker 高级

## Docker Compose

> * DockerFile build run 手动操作，单个容器
> * 轻松高效的管理容器，定义运行多个容器
> * 是一个定义并运行多容器的应用
> * 用Yaml配置
> * 所有的环境都可以使用
> * 批量容器编排
> * 可以理解为一个运行docker run 的脚本？

### 三步骤：

1. 用Dockerfile定义，保证我们的项目可以在任何地方运行
2. 用`docker-compose.yaml`来配置
3. `docker-compose up`在任何地方运行

> YML怎么写？
>
> ```yaml
> version: '2.0'
> services:
>   web:
>     build: .
>     ports:
>     - "5000:5000"
>     volumes:
>     - .:/code
>     - logvolume01:/var/log
>     links:
>     - redis
>   redis:
>     image: redis
> volumes:
>   logvolume01: {}
> ```

### 写一个Compose应用

1. 下载docker-compose[Docker Compose](https://docs.docker.com/compose/install/)
2. `docker-compose version`测试是否安装成功
3. 开始写yaml
4. 默认的服务名`文件名_服务名_num`
5. 只要通过compose启动，就会自动生成一个网络，所有小项目都在同一个网络，然后再统一暴露一个端口。在这个网络中，会自动做好域名映射，容器之间通过域名访问。

### Compose编写规则

[详细的官方文档](https://docs.docker.com/compose/compose-file/)

```yaml
version: '2.0' #版本
services: #服务
  web: #服务1
  #服务1的配置
    build: .
    ports:
    - "5000:5000"
    depends_on: #以此定义依赖顺序，启动顺序
    	-db
    volumes:
    - .:/code
    - logvolume01:/var/log
    links:
    - redis
  redis: #服务2
    image: redis
volumes: 
  logvolume01: {}
```



