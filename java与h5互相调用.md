访问url的就不多说了。若是本地的h5文件，最好放在assets中，用起来简单方便。

gradle中添加 compile 'com.tamic:browse:1.0.0' ，一个很好的webview。
其中包含了自适应、注册本地方法供js调用等功能。

调用本地h5：
webView.loadUrl("file:///android_asset/calendar_demo/sign.html"); // 在assets下
调用本地h5中方法（必须在onPageFinished中调用，即页面加载完毕）：
webView.loadUrl("javascript:setId('" + account.getUserId() + "','" + account.getToken() + "')"); // 多参数
webView.loadUrl("javascript:getContent('" + "im/integral/sign/record" + "')"); // 单参数