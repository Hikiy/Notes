mongodb的操作在order中不能这样写：
```
->order('created desc')
```
只能这样写：
```
->order('created','desc')
```

<br /><br /><br /><br />
> github: https://github.com/Hikiy  
> 作者：Hiki  
> 创建日期：2019.05.14  
> 更新日期：2019.05.14

<center>(<font color=red size=2>转载文章请注明作者和出处 </font><a href="https://github.com/Hikiy">Hiki)</a></center>  