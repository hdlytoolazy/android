访问url的就不多说了。若是本地的h5文件，最好放在assets中，用起来简单方便。

gradle中添加 compile 'com.tamic:browse:1.0.0' ，一个很好的webview。
其中包含了自适应、注册本地方法供js调用等功能。

调用本地h5：
webView.loadUrl("file:///android_asset/calendar_demo/sign.html"); // 在assets下
调用本地h5中方法（必须在onPageFinished中调用，即页面加载完毕）：
webView.loadUrl("javascript:setId('" + account.getUserId() + "','" + account.getToken() + "')"); // 多参数
webView.loadUrl("javascript:getContent('" + "im/integral/sign/record" + "')"); // 单参数

若h5中调用的是https，则需要添加如下代码来绕过认证处理：
import android.net.http.SslError;
import android.webkit.SslErrorHandler;

mWebView.getSettings().setJavaScriptEnabled(true);
mWebView.requestFocus();
mWebView.setScrollBarStyle(WebView.SCROLLBARS_INSIDE_OVERLAY);

mWebView.setWebViewClient(new WebViewClient() {
@Override
   public void onReceivedSslError(WebView view, SslErrorHandler handler, SslError error) {
 
       // 不要使用super，否则有些手机访问不了，因为包含了一条 handler.cancel()
       // super.onReceivedSslError(view, handler, error);
     //handler.cancel(); 默认的处理方式，WebView变成空白页
      //handler.process();接受证书
      //handleMessage(Message msg); 其他处理
       // 接受所有网站的证书，忽略SSL错误，执行访问网页
       handler.proceed();
   }