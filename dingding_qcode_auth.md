# @直接上代码

钉钉扫码登录文档有够烂的, 啃文档啃了2个小时不明道理;

```html
<!DOCTYPE html>
<html lang="en" dir="ltr">
    <head>
        <meta charset="utf-8">
        <title>扫码登录</title>
    </head>
    <body>
        <div class="" id="js_qcode"></div>
        <script src="//g.alicdn.com/dingding/dinglogin/0.0.2/ddLogin.js"></script>
        <script type="text/javascript">
            var appid = 'yours appid';
            var redirect_uri = 'https://www.example.com';
            var uri = 'https://oapi.dingtalk.com/connect/oauth2/sns_authorize?appid='+ 
                       appid+'&response_type=code&scope=snsapi_login&state=STATE&redirect_uri=' + redirect_uri;
            var obj = DDLogin({
                id:"js_qcode",// 这里需要你在自己的页面定义一个HTML标签并设置id，
                              // 例如<div id="login_container"></div>或<span id="login_container"></span>
                goto: encodeURIComponent(uri),
                style: '',
                href: "",
                width : "300px",
                height: "300px"
            });
            var hanndleMessage = function (event) {
                var loginTmpCode = event.data; //拿到loginTmpCode后就可以在这里构造跳转链接进行跳转了
                var origin = event.origin;
                console.log("origin", event.origin);
                if( origin == "https://login.dingtalk.com" ) { //判断是否来自ddLogin扫码事件。
                     var redirect_uri_check = 'https://oapi.dingtalk.com/connect/oauth2/sns_authorize?appid='+
                                yours appid +' &response_type=code&scope=snsapi_login&state='+
                                new Date()+'&redirect_uri=http://&loginTmpCode=';  // 这里划重点 !!! 
                                               
                     window.location.href= redirect_uri_check + loginTmpCode; // 这里划重点 !!! 
                                                                              // loginTmpCode 调试的时候这个地方拼接错误
                                                                              // 提示的也太有歧义了, 竟然提示redirect没有权限
                                                                              // 相当难理解
                }

            };

            if (typeof window.addEventListener != 'undefined') {
                window.addEventListener('message', hanndleMessage, false);
            } else if (typeof window.attachEvent != 'undefined') {
                window.attachEvent('onmessage', hanndleMessage);
            }
        </script>
    </body>
</html>

```

# @参考

网站应用钉钉扫码登录开发指南 (https://g.alicdn.com/dingding/opendoc/docs/_identityverify/tab5.html)
