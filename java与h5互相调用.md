����url�ľͲ���˵�ˡ����Ǳ��ص�h5�ļ�����÷���assets�У��������򵥷��㡣

gradle����� compile 'com.tamic:browse:1.0.0' ��һ���ܺõ�webview��
���а���������Ӧ��ע�᱾�ط�����js���õȹ��ܡ�

���ñ���h5��
webView.loadUrl("file:///android_asset/calendar_demo/sign.html"); // ��assets��
���ñ���h5�з�����������onPageFinished�е��ã���ҳ�������ϣ���
webView.loadUrl("javascript:setId('" + account.getUserId() + "','" + account.getToken() + "')"); // �����
webView.loadUrl("javascript:getContent('" + "im/integral/sign/record" + "')"); // ������

��h5�е��õ���https������Ҫ������´������ƹ���֤����
import android.net.http.SslError;
import android.webkit.SslErrorHandler;

mWebView.getSettings().setJavaScriptEnabled(true);
mWebView.requestFocus();
mWebView.setScrollBarStyle(WebView.SCROLLBARS_INSIDE_OVERLAY);

mWebView.setWebViewClient(new WebViewClient() {
@Override
   public void onReceivedSslError(WebView view, SslErrorHandler handler, SslError error) {
 
       // ��Ҫʹ��super��������Щ�ֻ����ʲ��ˣ���Ϊ������һ�� handler.cancel()
       // super.onReceivedSslError(view, handler, error);
     //handler.cancel(); Ĭ�ϵĴ���ʽ��WebView��ɿհ�ҳ
      //handler.process();����֤��
      //handleMessage(Message msg); ��������
       // ����������վ��֤�飬����SSL����ִ�з�����ҳ
       handler.proceed();
   }