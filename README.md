1.外链的静态图片是否缓存需要在服务器端nginx或者apache的配置文件中配，apache打开方法请看截图

2.返回304还是直接from cache还是去服务器端取返回200，跟你缓存设置有关，是设置了no-cache,还是max-age=1234，还跟浏览器操作有关，是
点击刷新按钮

还是后退再前进

还是重新载入

还是新开窗口填入url回车

还是地址栏直接回车，

不同操作结果也不同，下面是我做的测试，文件名为index.php,测试浏览器：pc chrome， version：33.0.1750.117，括号里是ie10标准模式下的表现。

测试代码：

<?php

header('Cache-Control:no-cache');

?>

<!DOCTYPE html>

<html>

<body>

<div class="pic">

	<img src="test.jpg" />
	
	<script type='text/javascript' src='jquery.js'></script>
	
</div>

</body>

</html>

结果：index.php请求，非内嵌的外链图片或js

刷新：重发请求（ie10标准模式下是重发请求）

后退再前进：from cache，从cache中取了，进一步证明no-cache不是不缓存,no-store才是真不缓存（304，服务器验证了）

新开窗口填url回车：重发请求（from cache）

地址栏回车：重发请求（304）

重载：重发请求（ie10没有重载）

将php文件中header('Cache-Control:no-cache');改为header（'cache-control:max-age=1234'）结果，仍然是index.php请求的：

刷新：重发请求（ie10标准模式下是重发请求）

后退再前进：from cache（304）

新开窗口填url回车：from cache（304）

地址栏回车：重发请求（304）

重载：重发请求（没有重载）





参考文档：http://www.haidx.com/http-304-and-200-from-cache.html

