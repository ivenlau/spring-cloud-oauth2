# spring-cloud-oauth2


此项目用于演示基于Spring Cloud基于OAuth2的实现模式。
请单独启动四种模式下的 authorization server 和 resource server。详情请参考子目录README.md。


## 基础演示demo功能说明
> 基于内存保存用户信息和token，仅用于测试。

- [x] authorization-code : 授权码模式

- [x] client-credentials : 客户端模式

- [x] implicit: 隐式模式（简化模式）

- [x] password: 密码模式