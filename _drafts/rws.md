###安全模型
WebSocket协议使用浏览器中用来限制从web页面向服务器发起连接请求的源模型。
但是这种限制仅仅是在浏览器中，对于特殊的客户端（不是通过web浏览器），这种源模型就失效了。

####连接失败
和已经存在其他协议（例如SMTP或HTTP）的服务器建立连接将会导致连接失败，
这种机制是由设计严秘的“握手”和握手完成之前建立连接时的一些“限制数据”实现的。

另一种情况，假如数据是从其他协议（特别是HTTP）发送过来的，同样会导致连接建立失败。
例如这种情况：当web页面中的表单数据被提交到一个WS-Server
这一机制主要是由接受到数据的服务器证明了它已经读取了握手信息从而实现的。并且只有从客户端发送的握手帧中包含合适的信息才会让服务器触发这个动作。

###和TCP、HTTP协议的关系
基于TCP的独立的协议。
和HTTP的唯一关联就是HTTP服务器需要把ws发出的握手动作升级为“UPgrade”请求并进行解释。

默认情况下，ws协议使用80端口进行普通连接，加密的TLS连接默认使用443端口。

###ws的子协议
客户端向服务器发起握手请求的header中可能带有“Sec-WebSocket-Protocol”字段，用来指定一个特定的子协议，一旦这个字段有设置，那么服务器需要在建立连接的响应头中包含同样的字段，内容就是选择的子协议之一。

子协议的命名应该是注册过的（有一套）
为了避免潜在的冲突，建议子协议的源（发起者）使用ASCII编码的域名。
例子：
一个注册过的子协议叫“chat.xxx.com”，另一个叫“chat.xxx.org”。这两个子协议都会被server同时实现，server会动态的选择使用哪个子协议（取决于客户端发送过来的值）。

###Extensions
扩展是用来增加ws协议一些新特性的

