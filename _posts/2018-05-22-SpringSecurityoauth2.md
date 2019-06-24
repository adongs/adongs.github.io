---
layout: post
title: "Spring Security oauth2"
date: 2019-06-24 14:18:43
image: 'https://adongs.github.io/assets/img/resources/springsecurity.jpg'
description: 学习Spring Security oauth2
category: 'Spring Security oauth2'
tags:
- Spring boot
- Spring
- Spring Cloud
- Spring Security oauth2
- Spring Security
introduction: Spring Security oauth2 搭建和理解
---


### 应用场景

```
等待编写
```

### OAuth 2.0 专有名词


 - Third-party application：第三方应用程序，本文中又称"客户端"（client

 - HTTP service：HTTP服务提供商，本文中简称"服务提供商"

 - Resource Owner：资源所有者，本文中又称"用户"（user）。

 - User Agent：用户代理，本文中就是指浏览器。

 - Authorization server：认证服务器，即服务提供商专门用来处理认证的服务器。

 - Resource server：资源服务器，即服务提供商存放用户生成的资源的服务器。它与认证服务器，可以是同一台服务器，也可以是不同的服务器。


### OAuth 2.0 运行流程


- （A）用户打开客户端以后，客户端要求用户给予授权。

- （B）用户同意给予客户端授权。

- （C）客户端使用上一步获得的授权，向认证服务器申请令牌。

- （D）认证服务器对客户端进行认证以后，确认无误，同意发放令牌。

- （E）客户端使用令牌，向资源服务器申请获取资源。

- （F）资源服务器确认令牌无误，同意向客户端开放资源。


### OAuth 2.0 授权模式

####  授权码模式（authorization code）最复杂

> 1.用户访问客户端，后者将前者导向认证服务器。

```
response_type：表示授权类型，必选项，此处的值固定为"code"
client_id：表示客户端的ID，必选项
redirect_uri：表示重定向URI，可选项
scope：表示申请的权限范围，可选项
state：表示客户端的当前状态，可以指定任意值，认证服务器会原封不动地返回这个值

(客户端请求认证服务器)请求样例:
GET /authorize?response_type=code&client_id=s6BhdRkqt3&state=xyzredirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fcb HTTP/1.1
Host: server.example.com

```

> 2.用户选择是否给予客户端授权。

```
显示界面,用户点击授权按钮
```

> 3.假设用户给予授权，认证服务器将用户导向客户端事先指定的"重定向URI"（redirection URI），同时附上一个授权码。

```
code：表示授权码，必选项。该码的有效期应该很短，通常设为10分钟，客户端只能使用该码一次，否则会被授权服务器拒绝。该码与客户端ID和重定向URI，是一一对应关系
state：如果客户端的请求中包含这个参数，认证服务器的回应也必须一模一样包含这个参数

(认证服务器重定向到客户端)跳转如下:
HTTP/1.1 302 Found
Location: https://client.example.com/cb?code=SplxlOBeZQQYbYS6WxSbIA
          &state=xyz
```

> 4.客户端收到授权码，附上早先的"重定向URI"，向认证服务器申请令牌。这一步是在客户端的后台的服务器上完成的，对用户不可见。

```
grant_type：表示使用的授权模式，必选项，此处的值固定为"authorization_code"。
code：表示上一步获得的授权码，必选项。
redirect_uri：表示重定向URI，必选项，且必须与A步骤中的该参数值保持一致。
client_id：表示客户端ID，必选项。

(客户端请求认证服务器) 请求如下:
POST /token HTTP/1.1
Host: server.example.com
Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&code=SplxlOBeZQQYbYS6WxSbIA
&redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fcb

```



> 5.认证服务器核对了授权码和重定向URI，确认无误后，向客户端发送访问令牌（access token）和更新令牌（refresh token）。

```
access_token：表示访问令牌，必选项。
token_type：表示令牌类型，该值大小写不敏感，必选项，可以是bearer类型或mac类型。
expires_in：表示过期时间，单位为秒。如果省略该参数，必须其他方式设置过期时间。
refresh_token：表示更新令牌，用来获取下一次的访问令牌，可选项。
scope：表示权限范围，如果与客户端申请的范围一致，此项可省略。

(认证服务器返回) 响应如下:
     HTTP/1.1 200 OK
     Content-Type: application/json;charset=UTF-8
     Cache-Control: no-store
     Pragma: no-cache

     {
       "access_token":"2YotnFZFEjr1zCsicMWpAA",
       "token_type":"example",
       "expires_in":3600,
       "refresh_token":"tGzv3JOkF0XG5Qx2TlKWIA",
       "example_parameter":"example_value"
     }
```



#### 简化模式（implicit)

> 1.客户端将用户导向认证服务器。

```
response_type：表示授权类型，此处的值固定为"token"，必选项。
client_id：表示客户端的ID，必选项。
redirect_uri：表示重定向的URI，可选项。
scope：表示权限范围，可选项。
state：表示客户端的当前状态，可以指定任意值，认证服务器会原封不动地返回这个值。

(客户端请求认证服务武器) 请求如下:
GET /authorize?response_type=token&client_id=s6BhdRkqt3&state=xyz&redirect_uri=https%3A%2F%2Fclient%2Eexample%2Ecom%2Fcb HTTP/1.1
Host: server.example.com

```

> 2.用户决定是否给于客户端授权。

```
显示界面,用户点击授权按钮
```

> 3.假设用户给予授权，认证服务器将用户导向客户端指定的"重定向URI"，并在URI的Hash部分包含了访问令牌。


```
access_token：表示访问令牌，必选项。
token_type：表示令牌类型，该值大小写不敏感，必选项。
expires_in：表示过期时间，单位为秒。如果省略该参数，必须其他方式设置过期时间。
scope：表示权限范围，如果与客户端申请的范围一致，此项可省略。
state：如果客户端的请求中包含这个参数，认证服务器的回应也必须一模一样包含这个参数。

(认证服务器重定向到客户端) 请求如下:
HTTP/1.1 302 Found
Location: http://example.com/cb#access_token=2YotnFZFEjr1zCsicMWpAA&state=xyz&token_type=example&expires_in=3600

```

> 4.浏览器向资源服务器发出请求，其中不包括上一步收到的Hash值。

```
(浏览器请求资源服务器) 请求如下:
http://example.com/cb#state=xyz&token_type=example&expires_in=3600

```

> 5.资源服务器返回一个网页，其中包含的代码可以获取Hash值中的令牌。

```
资源服务返回一个网页,提取hash值中的令牌
```

> 6.浏览器执行上一步获得的脚本，提取出令牌。

```
执行脚本,获取令牌
```

> 7.浏览器将令牌发给客户端。

```
浏览器将获取到的令牌上传给客户端
```

#### 密码模式（resource owner password credentials）

> 用户向客户端提供用户名和密码。

```
用户提供给客户端账号和密码
```

> 客户端将用户名和密码发给认证服务器，向后者请求令牌。

```
grant_type：表示授权类型，此处的值固定为"password"，必选项。
username：表示用户名，必选项。
password：表示用户的密码，必选项。
scope：表示权限范围，可选项。

(客户端请求认证服务器)请求如下:
  POST /token HTTP/1.1
  Host: server.example.com
  Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
  Content-Type: application/x-www-form-urlencoded
  grant_type=password&username=johndoe&password=A3ddj3w

```

> 认证服务器确认无误后，向客户端提供访问令牌。

```
(认证服务器响应客户端) 响应如下
HTTP/1.1 200 OK
     Content-Type: application/json;charset=UTF-8
     Cache-Control: no-store
     Pragma: no-cache

     {
       "access_token":"2YotnFZFEjr1zCsicMWpAA",
       "token_type":"example",
       "expires_in":3600,
       "refresh_token":"tGzv3JOkF0XG5Qx2TlKWIA",
       "example_parameter":"example_value"
     }

```


#### 客户端模式（client credentials）

> 客户端向认证服务器进行身份认证，并要求一个访问令牌。

```
granttype：表示授权类型，此处的值固定为"clientcredentials"，必选项。
scope：表示权限范围，可选项
(客户端请求授权服务器) 请求如下:
    POST /token HTTP/1.1
     Host: server.example.com
     Authorization: Basic czZCaGRSa3F0MzpnWDFmQmF0M2JW
     Content-Type: application/x-www-form-urlencoded

     grant_type=client_credentials
```

> 认证服务器确认无误后，向客户端提供访问令牌。

```
(认证服务器响应) 响应如下:
     HTTP/1.1 200 OK
     Content-Type: application/json;charset=UTF-8
     Cache-Control: no-store
     Pragma: no-cache

     {
       "access_token":"2YotnFZFEjr1zCsicMWpAA",
       "token_type":"example",
       "expires_in":3600,
       "example_parameter":"example_value"
     }
```