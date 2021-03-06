# CSV生成
csv就是Excel能打开的一种格式。  
实现方式：  
- 构造方法：
```
public function outputCsv($head, $body) {
        $csv = "\xEF\xBB\xBF";

        foreach ($head as $k => $v) {
            $csv .= "{$v}\t,";
        }
        $csv .= "\n";

        foreach ($body as $key => $value) {
            foreach ($value as $k => $v) {
                $csv .= "{$v}\t,";
            }
            $csv .= "\n";
        }

        return $csv;
    }
```
这里的`head`即表格最顶部的字段，`body`为内容。看代码可以明白csv其实就是用 `,` 分隔和用 `\n` 来换行，这里的例子是公司一个例子，所以循环看起来有些复杂。  
**其中 `\t` 是横向制表(HT)（即跳到下一个TAB位置），用这个符号后生成的表格会规范很多。**

- 输出到浏览器端下载
```
$head = ['字段1', '字段2', '字段3'];
$body[1]['字段1'] = 11111;
$body[2]['字段2'] = 22222;
$result = outputCsv($head, $body);
header("Content-type:text/csv");
header("Content-Disposition:attachment; filename=文件名{$time}.csv");
echo $result;exit();
```
这里为了跟上面的构造方法一样，body使用了二维数组。

<br /><br /><br /><br />

> github: https://github.com/Hikiy  
> 作者：Hiki

<center>(<font color=red size=2>转载文章请注明作者和出处 </font><a href="https://github.com/Hikiy">Hiki)</a></center>  