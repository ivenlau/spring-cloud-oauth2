## 密码模式
密码模式用户相对于授权码模式，就是用户将用户名和密码提供给应用服务器，由应用服务器直接向授权服务器申请访问token，不需要用户自己登录，减少操作步骤。

1. 带上客户端账号密码与用户账户密码获取token
```
curl -X POST --user client:secret http://localhost:2048/oauth/token -H "accept: application/json" -H "content-type: application/x-www-form-urlencoded" -d "grant_type=password&username=admin&password=123456&scope=read"
```
返回token
```json
{
    "access_token": "33f4377c-1b27-4c3b-9426-48d394f1788e",
    "token_type": "bearer",
    "expires_in": 43199,
    "scope": "read"
}
```
2. 使用token访问资源
```
curl -X GET \
  http://localhost:8081/test/hello \
  -H 'Authorization: Bearer 58d25f8c-ea0e-40e0-9b55-9d7ef25c8145' \
```
返回响应
```
Hello word
```

3.  使用token获取用户信息
另外，我这边尝试了一下校验token的接口，之前使用授权码等校验时，只需要传token、client_id、client_secret 
就可以正确校验token，

如果是password模式，校验接口时，需要使用client_id、client_secret登录，传参时只需要传token

```
curl -X POST --user client:secret  http://localhost:2049/oauth/check_token \
 -d 'token=b462bd1f-a50c-4209-ac06-5a668143ed9e'
```
返回结果
```json
{
    "active": true,
    "exp": 1594331803,
    "user_name": "admin",
    "authorities": [
        "admin"
    ],
    "client_id": "client",
    "scope": [
        "read"
    ]
}
```