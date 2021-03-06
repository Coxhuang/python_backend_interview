[TOC]

# XSS攻击

## #1 什么是XSS攻击 

XSS（Cross Site Scripting）攻击全称跨站脚本攻击，为了不与层叠样式表CSS(Cascading Style Sheets)混淆，故将跨站脚本攻击缩写为XSS。


XSS攻击是指黑客通过`HTML注入`篡改网页,插入恶意脚本,从而在用户浏览网页时,控制浏览器的一种攻击行为


XSS攻击分为以下几种:

- 反射型XSS
- 存储型XSS
- DOM Based XSS

## #2 反射型XSS 

反射性XSS原理 : 反射性XSS一般指攻击者通过特定的方式来诱惑受害者去访问一个包含恶意代码的URL。这个URL前半部分是正常的访问某个站点(如:微博)的服务端地址,而URL的参数中加有恶意代码,当用户点击这个URL后,就会正常的访问微博服务器,如果服务器的这个接口正好会将URL的参数返回给用户,此时恶意代码就会在用户的浏览器上运行,如果恶意代码的内容是获取站点的Cookie,并发送到攻击者的服务器,那么攻击者就能获取用户的Cookie。

> 反射型XSS大概步骤 : 

1. 攻击者在正常的URL(微博某个接口)后面的参数中加入恶意攻击代码(代码内容为获取用户浏览器上微博的Cookie)
2. 当用户打开带有恶意代码的URL的时候，微博服务端将恶意代码从URL中取出，拼接在html中并且返回给浏览器端。
3. 用户浏览器接收到响应后执行解析，其中的恶意代码也会被执行到。
4. 攻击者通过恶意代码来窃取到用户数据并发送到攻击者的网站。攻击者会获取到比如cookie等信息，然后使用该信息来冒充合法用户的行为，调用目标网站接口执行攻击等操作。


## #3 存储型XSS 

存储型XSS原理 : 攻击者在某个站点的数据库注入恶意代码(如:在微博的评论区注入恶意代码),当有用户访问评论时,微博服务端就会将恶意代码返回给用户,此时恶意代码就会在用户的浏览器上执行,并盗取用户信息

> 存储型XSS大概步骤 : 

1. 攻击者将恶意代码提交到目标网站数据库中(可以通过评论区/留言板注入)。
2. 用户打开目标网站时，网站服务器将恶意代码从数据库中取出，然后拼接到html中返回给浏览器中。
3. 用户浏览器接收到响应后解析执行，那么其中的恶意代码也会被执行。
4. 那么恶意代码执行后，就能获取到用户数据，比如上面的cookie等信息，那么把该cookie发送到攻击者网站中，那么攻击者拿到该
cookie然后会冒充该用户的行为，调用目标网站接口等违法操作。


> 如何防范 : 

- 后端需要对提交的数据进行过滤。
- 前端也可以做一下处理方式，比如对script标签，将特殊字符替换成HTML编码这些等。


## #4 DOM Based XSS 


DOM Based XSS原理 : 客户端的js可以对页面dom节点进行动态的操作，比如插入、修改页面的内容。比如说客户端从URL中提取数据并且在本地执行、如果用户在客户端输入的数据包含了恶意的js脚本的话，但是这些脚本又没有做任何过滤处理的话，那么我们的应用程序就有可能受到DOM-based XSS的攻击。


> DOM Based XSS大概步骤 : 

1. 某个站点的客户端正好有一个功能,获取URL中的参数,并对DOM节点进行动态操作
2. 用户浏览器收到响应后解析执行。前端使用js取出url中的恶意代码并执行。
3. 执行时，恶意代码窃取用户数据并发送到攻击者的网站中，那么攻击者网站拿到这些数据去冒充用户的行为操作。调用目标网站接口
执行攻击者一些操作。



> 获取URl的参数,客户端将参数插入标签,如果参数是恶意代码,就会出现如下情况 : 



```html
<script>
    ...
    document.body.innerHTML = "<a href='"+url+"'>"+url+"</a>";
    ...
</script>
```


![](https://raw.githubusercontent.com/Coxhuang/yosoro/master/20200323061021.png)


## #5 防御 XSS 的几种策略



- 浏览器端主动进行XSS识别,Chrome浏览器会自动识别XSS攻击代码
- 服务器端对于用户输入的内容进行过滤

> 服务端如何处理 : 

- 将重要的cookie标记为http only, 这样的话Javascript 中的document.cookie语句就不能获取到cookie了
- 对数据进行html encode处理，过滤或移除特殊的Html标签
- 过滤JavaScript 事件的标签。例如 "οnclick=", "onfocus" 等等


## #5 XSS与CSRF区别

- CSRF攻击是在用户登录过某站点,并且在Cookie还没过期前,诱导用户点击恶意链接,这样就可以以用户的身份访问该站点服务端的一些接口(如:银行转账)
- XSS攻击是通过恶意链接或者往服务器注入恶意代码,达到获取用户cookie等信息














