# Dockerfile指令
### COPY
复制文件，格式：
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
<br /><br /><br /><br />
> github: https://github.com/Hikiy  
> 作者：Hiki  
> 创建日期：2019.04.25  
> 更新日期：2019.04.25

<center>(<font color=red size=2>转载文章请注明作者和出处 </font><a href="https://github.com/Hikiy">Hiki)</a></center>  