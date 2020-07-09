## 密码模式
密码模式用户与其它几个模式相比，更加简单一些，用户访问资源时，客户端（浏览器）跳转自己的登录页面，
用户输入账号密码提交表单，表单提交时带上客户端的账号和密码+用户账户密码+scope+grant_type
授权服务器校验通过后直接返回token
> 这里要注意：表单提交的uri是`/oauth/token`

1. 带上客户端账号密码与用户账户密码获取token
```
curl -X POST --user client-a:client-a-secret http://localhost:8080/oauth/token -H "accept: application/json" -H "content-type: application/x-www-form-urlencoded" -d "grant_type=password&username=hellxz&password=xyz&scope=read_scope"
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
  http://localhost:8081/user/hellxz001 \
  -H 'Authorization: Bearer 58d25f8c-ea0e-40e0-9b55-9d7ef25c8145' \
```
返回响应
```json
{
    "username": "hellxz001",
    "email": "hellxz001@foxmail.com"
}
```

3.  使用token获取用户信息
另外，我这边尝试了一下校验token的接口，之前使用授权码等校验时，只需要传token、client_id、client_secret 
就可以正确校验token，

如果是password模式，校验接口时，需要使用client_id、client_secret登录，传参时只需要传token

```
curl -X POST --user user-center:12345  http://localhost:8080/oauth/check_token \
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