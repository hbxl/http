本repository主要探讨php动态文件和外链js，图片等静态文件的各浏览器缓存机制，下述文字探讨的是php动态文件缓存，截图是外链js文件的缓存机制。

1.外链的静态图片是否缓存需要在服务器端nginx或者apache的配置文件中配，apache打开方法请看截图，其余截图是对外链图片的请求，非index.php文件。

2.返回304还是直接from cache还是去服务器端取返回200，跟你缓存设置有关，是设置了no-cache,还是max-age=1234，还跟浏览器操作有关，是
点击刷新按钮

还是后退再前进

还是重新载入

还是新开窗口填入url回车

还是地址栏直接回车，

不同操作结果也不同，下面是我做的测试，文件名为index.php,

测试浏览器：pc chrome， version：33.0.1750.117，括号里是ie10标准模式下的表现。

结果：

a.

header('Cache-Control:no-cache');时index.php请求，非内嵌的外链图片或js

刷新：重发请求（ie10标准模式下是重发请求）

后退再前进：from cache，从cache中取了，进一步证明no-cache不是不缓存,no-store才是真不缓存（ie10:304，服务器验证了）

新开窗口填url回车：重发请求（ie10:from cache）

地址栏回车：重发请求（ie10:304）

重载：重发请求（ie10没有重载）

b.

header（'cache-control:max-age=1234'）结果，仍然是index.php请求的：

刷新：重发请求（ie10标准模式下是重发请求）

后退再前进：from cache（ie10:304）

新开窗口填url回车：from cache（ie10:304）

地址栏回车：重发请求（ie10:304）

重载：重发请求（ie10：没有重载）





参考文档：http://www.haidx.com/http-304-and-200-from-cache.html

