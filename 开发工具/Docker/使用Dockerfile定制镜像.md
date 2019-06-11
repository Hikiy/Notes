# 使用Dockerfile定制镜像
Dockerfile 是一个文本文件，其内包含了一条条的指令(Instruction)，每一条指令构建一层，因此每一条指令的内容，就是描述该层应当如何构建。  
在空目录建立文本文件命名为```Dockerfile```:
```
FROM nginx
RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
```
这里涉及到两个指令```FROM```和```RUN```。
### FROM
用于指定基础镜像，是```Dockerfile```中必备的指令，而且必须是第一条指令。 


在 Docker Hub 上有非常多的高质量的官方镜像，有可以直接拿来使用的服务类的镜像，如 nginx、redis、mongo、mysql、httpd、php、tomcat等；也有一些方便开发、构建、运行各种语言应用的镜像，如 node、openjdk、python、ruby、golang等。可以在其中寻找一个最符合我们最终目标的镜像为基础镜像进行定制。

如果没有找到对应服务的镜像，官方镜像中还提供了一些更为基础的操作系统镜像，如ubuntu、debian、centos、fedora、alpine等，这些操作系统的软件库为我们提供了更广阔的扩展空间。

除了选择现有镜像为基础镜像外，Docker还存在一个特殊的镜像，名为scratch。这个镜像是虚拟的概念，并不实际存在，它表示一个空白的镜像。  
### RUN
用来执行命令行命令，有两种格式：  
- shell格式：```RUN <命令>```。和在命令行中输入命令一样
```
RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
```
- exec格式：```RUN["可执行文件","参数1","参数2"]```，比较像函数调用。  

```RUN``` 像Shell脚本一样执行命令，但是不要每一个命令对应一个```RUN```，因为每一个```RUN```的行为，就像建立镜像一样新建一层，然后在上面执行命令，执行完毕后，```commit```这一层修改，构成新的镜像。如果每一个命令对应一个```RUN```则会创建很多层，正确写法：  
```
FROM debian:stretch

RUN buildDeps='gcc libc6-dev make wget' \
    && apt-get update \
    && apt-get install -y $buildDeps \
    && wget -O redis.tar.gz "http://download.redis.io/releases/redis-5.0.3.tar.gz" \
    && mkdir -p /usr/src/redis \
    && tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1 \
    && make -C /usr/src/redis \
    && make -C /usr/src/redis install \
    && rm -rf /var/lib/apt/lists/* \
    && rm redis.tar.gz \
    && rm -r /usr/src/redis \
    && apt-get purge -y --auto-remove $buildDeps
```

```/``` 为命令换行，```#```为注释，```&&```将各个指令串联  

### 构建镜像  
在```Dockerfile```文件所在目录执行：
```
$ docker build -t nginx:v3 .
Sending build context to Docker daemon 2.048 kB
Step 1 : FROM nginx
 ---> e43d811ce2f4
Step 2 : RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
 ---> Running in 9cdc27646c7b
 ---> 44aa4490ce2c
Removing intermediate container 9cdc27646c7b
Successfully built 44aa4490ce2c
```
从命令的输出结果中，我们可以清晰的看到镜像的构建过程。在`Step2`中，如同我们之前所说的那样，`RUN` 指令启动了一个容器`9cdc27646c7b`，执行了所要求的命令，并最后提交了这一层 `44aa4490ce2c`，随后删除了所用到的这个容器 `9cdc27646c7b`。

这里我们使用了 docker build 命令进行镜像构建。其格式为：
```
docker build [选项] <上下文路径/URL/->
```
在这里我们指定了最终镜像的名称 `-t nginx:v3`，构建成功后，我们可以像之前运行 nginx:v2 那样来运行这个镜像，其结果会和 `nginx:v2` 一样.

<br /><br /><br /><br />
> github: https://github.com/Hikiy  
> 作者：Hiki

<center>(<font color=red size=2>转载文章请注明作者和出处 </font><a href="https://github.com/Hikiy">Hiki)</a></center>  