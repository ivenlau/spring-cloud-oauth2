## 客户端模式

**关键点在于：这里没有用户登录与授权的部分，直接由应用服务器向授权服务器申请token**

1. 发起请求，登录授权服务器获取token
    ```
    curl -X POST \
        --user client:secret http://localhost:2048/oauth/token \
        -d "grant_type=client_credentials&scope=read"
    ```

    返回响应
    ```json
    {
        "access_token": "3a2b350f-a2c8-4b7f-b4e4-070b95aa4bc7",
        "token_type": "bearer",
        "expires_in": 43199,
        "scope": "read"
    }
    ```
2. 调用资源接口
   ```
   curl -X GET --user client:secret http://localhost:2049/test/hello
        -H 'Authorization: Bearer fc4721f4-c4a3-44f8-8232-a31cf59964ca'
   ```
   返回响应
   ```
   Hello world
   ```
   
## 校验token
另外，我这边尝试了一下校验token的接口，之前使用授权码等校验时，只需要传token、client_id、client_secret 
就可以正确校验token，

如果是client_credentials模式，校验接口时，需要使用client_id、client_secret登录，传参时只需要传token

```
curl -X POST --user client:secret  http://localhost:2048/oauth/check_token \
 -d 'token=b462bd1f-a50c-4209-ac06-5a668143ed9e'
```
返回结果
```json
{
    "scope": [
        "read"
    ],
    "active": true,
    "exp": 1594328913,
    "client_id": "client"
}
```