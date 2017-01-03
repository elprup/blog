+++
date = "2009-06-18T10:19:00Z"
title = "GAE使用urllib2.urlopen出现DownloadError的问题"
slug = "gae_download_error"

+++

# GAE使用urllib2.urlopen出现DownloadError的问题
实验目的： 
用flex在GAE上运行python代码 
实验器材： 
pyamf，FLEX3,Python25,IE，GAE SDK 
实验准备： 
GAE代码一份，flex代码一份 
见下： 
python部分 
main.py 
```
    # -*- coding:utf-8 -*-   
    import wsgiref.handlers  
    import sys
    import urllib2  
    from google.appengine.ext import webapp  
    from pyamf.remoting.gateway.google import WebAppGateway  
      
    class MainPage(webapp.RequestHandler):  
    def get(self):  
    self.response.headers['Content-Type'] = 'text/plain'  
    self.response.out.write('Hello, webapp World!')  
      
    def echo(data):
    p=urllib2.urlopen("http://www.sina.com.cn")
    return data
      
    services = {  
    'myservice.echo': echo,  
    }  
      
    def main():  
    application_paths = [('/', WebAppGateway(services)), ('/helloworld', MainPage)]  
    application = webapp.WSGIApplication(application_paths, debug=True)  
    wsgiref.handlers.CGIHandler().run(application)  
```

flex部分 
```
    <?xml version="1.0" encoding="utf-8"?>
    <mx:Application xmlns:mx="http://www.adobe.com/2006/mxml" layout="absolute" creationComplete="init();">
    <mx:Script> 
    <![CDATA[ 
    import flash.net.*;
    import mx.controls.Alert;
    
    public function init():void{
    var netConnection:NetConnection = new NetConnection();
    netConnection.connect("http://localhost:8080/");
    var responder:Responder = new Responder(onComplete, onFail);
    netConnection.call("myservice.echo", responder, "Flash talked to PyAMF.  They both say hello.");
    }
    
    function onComplete(results):void {
    //var my:String = String("我就是中文");
    //var aTemp:Array = results as Array;
    //Alert.show(aTemp.toString());
    trace(results.toString());
    
    //trace(sTemp);
    
    /*for each(var thisResult in results){
    trace(thisResult.toString());
    }*/
    }
    
    function onFail(results):void {
    for each (var thisResult in results){
    trace( thisResult);
    }
    }
    ]]> 
    </mx:Script>
    </mx:Application>
```

实验步骤： 
1.打开CMD，用dev_appserver开启GAE服务器 
2.在Flex中运行debug命令 

实验问题： 
```
    'Traceback (most recent call last):', '  File "c:\\gaedemos\\cake\\pyamf\\remoting\\amf0.py", line 106, in __call__    *args, **kwargs)', 
    '  File "c:\\gaedemos\\cake\\pyamf\\remoting\\amf0.py", line 59, in _getBody    **kwargs)', '
    File "c:\\gaedemos\\cake\\pyamf\\remoting\\gateway\\__init__.py", line 494, in callServiceRequest    return service_request(*args)',
    '  File "c:\\gaedemos\\cake\\pyamf\\remoting\\gateway\\__init__.py", line 224, in __call__    return self.service(self.method, args)',
    '  File "c:\\gaedemos\\cake\\pyamf\\remoting\\gateway\\__init__.py", line 125, in __call__    return func(*params)', ' 
    File "c:\\gaedemos\\cake\\main.py", line 18, in echo    w.login(url)',
    '  File "c:\\gaedemos\\cake\\webspy\\webspy.py", line 42, in login    myHome = urllib2.urlopen(request)',
    '  File "C:\\Python25\\lib\\urllib2.py", line 124, in urlopen    return _opener.open(url, data)', '  
    File "C:\\Python25\\lib\\urllib2.py", line 381, in open    response = self._open(req, data)', ' 
    File "C:\\Python25\\lib\\urllib2.py", line 399, in _open    \'_open\', req)', '  File "C:\\Python25\\lib\\urllib2.py", line 360, in _call_chain    result = func(*args)', '
    File "C:\\Python25\\lib\\urllib2.py", line 1107, in http_open    return self.do_open(httplib.HTTPConnection, req)',
    '  File "C:\\Python25\\lib\\urllib2.py", line 1080, in do_open    r = h.getresponse()', '
    File "C:\\Program Files\\Google\\google_appengine\\google\\appengine\\dist\\httplib.py", line 200, in getresponse    self._allow_truncated, self._follow_redirects)', '
    File "C:\\Program Files\\Google\\google_appengine\\google\\appengine\\api\\urlfetch.py", line 241, in fetch    return rpc.get_result(allow_truncated)'
    , '  File "C:\\Program Files\\Google\\google_appengine\\google\\appengine\\api\\urlfetch.py", line 388, in get_result    self.check_success(allow_truncated)',
    '  File "C:\\Program Files\\Google\\google_appengine\\google\\appengine\\api\\urlfetch.py", line 356, in check_success    raise DownloadError(str(e))'
    , "DownloadError: ApplicationError: 2 nonnumeric port: ''"
    DownloadError
    ApplicationError: 2 nonnumeric port: ''
```  

问题初步分析： 
最后给出的结果是非数字端口，于是我想是不是没写端口号啊。加上：80后依旧。python代码在Eclipse+pydev的环境下跑的好好的，怎么到了gae就不行了呢？在网上查了好久，看到如下链接：`http://groups.google.co.jp/group/google-appengine/browse_thread/thread/07f418a36888c937`
其中提到
This is a known incompatibility in App Engine. The GData APIs send an 
absolute URL to the httplib library where it expects a relative one 
(since the host and port was specified in the constructor); the Python 
httplib handles this properly, but the App Engine replacement 
currently does not. 
奇怪的是python和GAE不是都用的一样的内核吗？我把python25的httplib文件复制过来以后还是这个问题。


实验小结： 
问题期待继续调查。
发现一个解决的方法参见
[http://nodex.iteye.com/blog/360733](#)
参考了CPYUG组织，发现这是GAE的一个限制socket的行为，所以不管怎么改都没有用。可以用urlfetch代替，不过还困惑于cookie的处理。
可以参考：
`https://groups.google.com/group/python-cn/browse_thread/thread/4b94c4ae8eb8d41c/54f2628668af75a0?lnk=gst&q=urlfetch#54f2628668af75a0`

以后要多关注关注CPyUG了，真的好强啊。
