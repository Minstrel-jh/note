[akka]: /note/akka/README.md
[url:cookie_session_token]: https://www.cnblogs.com/moyand/p/9047978.html
[url:akka-http]: https://doc.akka.io/docs/akka-http/current/

# akka-http-session

[返回][akka]

> [Akka HTTP 官方文档][url:akka-http]

akka-http-session使用cookies或自定义headers + local storage提供客户端会话(Session)管理的指令，并且具有可选的Json Web Tokens格式支持。

## 什么是session

> [彻底理解cookie, session, token][url:cookie_session_token]

- session数据至少需要包括已登录用户的`id`或者`username`。需要确保`id`的安全，以免会话被窃取或伪造。
- session可以保存在服务端的内存或数据库中，把`session id`发送给客户端；或者以序列化格式完整地保存在客户端。前者可提供粘性会话和共享存储。后者(正是akka-http-session支持的)可以在任何服务器上轻松的反序列化session
- 会话是一个字符串的令牌，它被发送到客户端，并且需要在每次请求中返回给服务端
- 为了防止伪造，会使用服务器的秘钥对序列化的会话数据进行签名

## `akka-http-session`特性
