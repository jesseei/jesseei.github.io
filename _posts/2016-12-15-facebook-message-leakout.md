---
layout: post
title: 10亿FacebookMessager用户隐私泄露
category: TechnicalReports
---

原文：[Critical Issue Affects Privacy of 1-Billion Facebook Messenger](http://www.cynet.com/wp-content/uploads/2016/12/Blog-Post-BugSec-Cynet-Facebook-Originull.pdf)

## 严重缺陷影响10亿Facebook Messager用户的隐私；潜在影响数百万其他网站

预计有18亿月度活跃用户信任Facebook保留他们的帐户，用户详细信息和通信安全。一方面，这个社交网络存在是基于分享：用户每天发布约350万张照片，每分钟发布近30万的状态。另一方面，Messenger，作为Facebook最受欢迎的一个特性，拥有10亿月度活跃用户。不像专门设计用于分享的照片和状态特性，Messenger的能力在于能提供一种私密的交流方式。

在本篇文章中，我们将描述一个在Facebook上发现的严重安全漏洞，它也可能影响使用来源空限制检查的数百万其他网站，威胁到他们用户的隐私，也可能让打开他们网站的访客感染恶意实体。这个漏洞被称为“Originull”，使攻击者可以访问病查看一个用户所有的私人聊天，照片和其他通过Facebook Messager发送的附件。这个漏洞由Ysrael Gurt研究团队发现并报告给了Facebook。

### “非技术性”说明

这个被发现的漏洞是一个跨域迂回攻击，允许黑客利用一个外部网站来访问和阅读一个用户的私人Facebook信息。通常来说，浏览器只允许Facebook页面来获取这些信息，以保护Messager用户免受此类事件。然而，Facebook开放了一个“桥”，以便让Facebook.com的子网站来访问Messenger信息。在Facebook管理这些子网站认证过程中的一个漏洞使得恶意网站有可能访问到Messenger的私人聊天信息。

例如，如果用户打开一个被黑客接管的网站（通过恶意广告，安全漏洞或者就是黑客自己的网站），那黑客就可以看到这个用户所有的Facebook Messenger聊天记录，照片和其他发送/接收到的附件，哪怕这个用户是使用其他电脑或者使用个人移动终端！


![2016-12-15-1](/pic/2016-12-15-1.PNG)

图 1：BugSec网站上显示出的聊天记录。左边可以看到用户ID。


### 技术性说明

这是对上述问题的更深入的技术性解释。Facebook Messenger聊天信息通过位于以下地址的单独服务器管理：{number} -edge-chat.facebook.com。 聊天服务本身在www.facebook.com域上运行。

通信在JaaSccript和服务器之间通过XML HTTP请求（XHR）进行。为了处理从5-edge-chat.facebook.com发送来的由JavaScript构建的数据，Facebook必须在响应里增加值为请求域的“Access-Control-Allow-Origin”头，并且增加值为true的“Access-Control-Allow-Credentials”头，以便让数据在cookies已经被发送的情况下还可以访问到。

![2016-12-15-2](/pic/2016-12-15-2.PNG)

图2：原始请求格式

截止目前，这看上去就是一个普通的CORS过程。为了防止其它网站获取数据，Facebook会检查请求头来源域数据。如果请求来自一个没有得到认证的域，服务器会返回400，返回包的头里“xfb-chat-failure-reason.”的值被设置成 “badorigin”。

![2016-12-15-3](/pic/2016-12-15-3.PNG)

图3：从不同域发来的请求

现在精彩的来了：Facebook也允许来自聊天域的普通GET请求。但是普通GET请求不会带着一个来源域头（origin header）。只有浏览器发送的XHR类型的请求才会包含一个来源域头。

所以，当服务器接受到一个GET请求的时候，这个请求没有包含一个“origin”头。在许多开发语言里，不存在的头会以null值来表现。如果Facebook期望接收到一个值为“null”的“origin”头，它不会禁止来自这个“origin”的请求。

to be continued...

Most likely, the filtering mechanism is separated from the responder mechanism, and
the responder assumes that the value in the “origin” header is allowed, because if not,
the filter would already have dropped the request. This development design allows
Facebook to add authorized origins by changing code in one position only.
