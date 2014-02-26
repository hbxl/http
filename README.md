1.外链的静态图片是否缓存需要在服务器端nginx或者apache的配置文件中配，apache打开方法请看截图

2.返回304还是直接from cache跟请求发送方式有关，在先前至少有过一次有效访问后，在以后对同一URI资源的请求中，浏览器只进行两种动作：

(a)直接在缓存中去获取内容。
如果先前有效访问的响应头包含 Expires, max-age的话，“打开新窗口”、“输入URI回车”、“前一页”、“后一页”这些浏览器行为不会使浏览器在Expires, max-age设置的有效期时间内去访问服务器，而是在缓存中去获取内容，但是’”刷新’”或”重载”例外。

(b)访问服务器，根据服务器响应来获取内容。这种情况发生在设置no-cache等头标要求不缓存，或者是设置了 Expires,max-age但浏览器行为是“刷新”或“重载”时候。’Last-Modified’、’ETag’、’must-revalidate’ 等有些特殊，不直接受浏览器行为影响，它们必须访问服务器后，再由服务器判断是直接发送新的资源，还是发送一个304 Not Modfied让浏览器使用缓存中的资源。

3.所以可见截图中不同操作，点击 '刷新'按钮时会去服务器端验证，返回304,点击'后退'再'前进'直接从cache中取了，返回200

4.php文件中header（'cache-control:no-cache'）测试，php文件名index.php，下面说明均是对index.php的请求的，非内嵌的外链图片或js，测试浏览器：pc版chrome，version: 33.0.1750.117 ,括号中为ie10标准模式下表现

刷新：重发请求（重发请求）

后退再前进：from cache，从cache中取了，进一步证明no-cache不是不缓存,no-store才是真不缓存（304，服务器验证了）

新开窗口填url回车：重发请求（from cache）

地址栏回车：重发请求（304）

重载：重发请求（没有重载）

5.php文件中header（'cache-control:max-age=1234'）测试：

刷新：重发请求（重发请求）

后退再前进：from cache（304）

新开窗口填url回车：from cache（304）

地址栏回车：重发请求（304）

重载：重发请求（没有重载）





参考文档：http://www.haidx.com/http-304-and-200-from-cache.html

