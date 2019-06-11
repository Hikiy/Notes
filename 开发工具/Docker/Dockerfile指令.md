# Dockerfile指令
## COPY
**复制文件**

格式：
- `COPY [--chown=<user>:<group>] <源路径>... <目标路径>`
- `COPY [--chown=<user>:<group>] ["<源路径1>",... "<目标路径>"]`
```
COPY package.json /usr/src/app/
```
源路径还可以用通配符：
```
COPY hom* /mydir/
COPY hom?.txt /mydir/
```
目标路径不需要事先创建，如果目录不存在会在复制文件前先行创建缺失目录。

在使用该指令的时候还可以加上```--chown=<user>:<group>```选项来改变文件的所属用户及所属组:
```
COPY --chown=55:mygroup files* /mydir/
COPY --chown=bin files* /mydir/
COPY --chown=1 files* /mydir/
COPY --chown=10:11 files* /mydir/
```
## CMD 
**容器启动命令**

与`RUN`类似:
- shell 格式：`CMD <命令>`
- exec 格式：`CMD ["可执行文件", "参数1", "参数2"...]`
- 参数列表格式：`CMD ["参数1", "参数2"...]`。在指定了 `ENTRYPOINT` 指令后，用 `CMD` 指定具体的参数。

Docker 不是虚拟机，容器就是进程。么在启动容器的时候，需要指定所运行的程序及参数。`CMD`指令就是用于指定默认的容器主进程的启动命令的。

一般推荐使用`exec`格式，这类格式在解析时会被解析为`JSON`数组，因此一定要使用双引号，而不要使用单引号。

如果使用`shell`格式的话，实际的命令会被包装为`sh -c`的参数的形式进行执行。比如：
```
CMD echo $HOME
```
在实际执行中，会将其变更为：
```
CMD [ "sh", "-c", "echo $HOME" ]
```

Docker 不是虚拟机，容器中的应用都应该以前台执行，容器内**没有后台服务的概念**。对于容器而言，其启动程序就是容器应用进程，容器就是为了主进程而存在的，主进程退出，容器就失去了存在的意义，从而退出，其它辅助进程不是它需要关心的东西。  

~~错误写法~~：
```
CMD service nginx start
```
会发现容器执行后立即退出。

**正确写法**是直接执行 nginx 可执行文件，并且要求以前台形式运行。：
```
CMD ["nginx", "-g", "daemon off;"]
```
## EXPOSE
**声明端口**

格式：
```
EXPOSE <端口1> [<端口2>...]
```
例:
```
EXPOSE 8000
```
只是一个声明用于
- 帮助镜像使用者理解镜像服务的守护端口，方便配置映射
- 在运行时使用随机端口映射时会自动随机映射`EXPOSE`的端口

要将`EXPROSE`与运行时使用`-p <宿主端口>:<容器端口>`区分开
- `EXPOSE`只是声明容器打算用什么端口，不会自动在宿主进行端口映射
- `-p`映射宿主端口和容器端口，将容器的端口服务公开给外界访问吗

<br /><br /><br /><br />
> github: https://github.com/Hikiy  
> 作者：Hiki  
> 创建日期：2019.04.25  
> 更新日期：2019.06.11

<center>(<font color=red size=2>转载文章请注明作者和出处 </font><a href="https://github.com/Hikiy">Hiki)</a></center>  