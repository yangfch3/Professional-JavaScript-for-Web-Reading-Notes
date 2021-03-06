# 安全性
1. 服务器端必须要对客户端的请求进行读写权限的验（例如 `github api` 的 `oauth` 认证）；

2. 确保通过 `XHR` 访问的 `URL` 安全；

3. `CSRF` -- `Cross-Site Request Forgery`
    > 未被授权的系统有权访问某个资源的情况叫做：跨站点请求伪造
    >
    > 未被授权的系统会伪装自己，让处理请求的服务器以为它是合法的，从而实现数据的窃取或销毁

4. 以 `SSL` 连接来访问可以通过 `XHR` 请求的资源；

5. 每一次请求都要附带经过相应算法计算得到的验证码；

6. 以下措施多防范 `CSRF` 攻击起不了作用
    * 限定请求方式为 `POST`，不能为 `GET` -- 很容易改变
    * 检查来源 URL 以确定是否可信 -- 来源记录很容易伪造
    * 基于 `Cookie` 信息进行验证 -- 很容易伪造

7. `XHR` 对象还有两个可选参数：`username` `password`，请不要使用这两个参数。
