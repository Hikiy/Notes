# 获取指定url的内容
get方法使用`file_get_contents`方法效率较高
```
function send_get($url) {
    return file_get_contents($url);
}
 
function send_post($url, $data) {
    $data = http_build_query($data);
    $options = array(
        'http' => array(
            'method' => 'POST',
            'header' => 'Content-type:application/x-www-form-urlencoded',
            'content' => $data,
            'timeout' => 10
        )
    );
    $context = stream_context_create($options);
    $result = file_get_contents($url, false, $context);
 
    return $result;
}
 
function send_post_raw($url, $str) {
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
    curl_setopt($ch, CURLOPT_HTTPHEADER, array(
        'X-AjaxPro-Method:ShowList',
        'Content-Type: application/json; charset=utf-8',
        'Content-Length: ' . strlen($str))
    );
    curl_setopt($ch, CURLOPT_POST, 1);
    curl_setopt($ch, CURLOPT_POSTFIELDS, $str);
    $data = curl_exec($ch);
    curl_close($ch);
 
    return $data;
}
```

<br /><br /><br /><br />

> github: https://github.com/Hikiy  
> 作者：Hiki

<center>(<font color=red size=2>转载文章请注明作者和出处 </font><a href="https://github.com/Hikiy">Hiki)</a></center>  