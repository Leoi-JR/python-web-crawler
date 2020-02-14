<!-- TOC -->

- [requests](#requests)
    - [方法](#方法)
    - [参数](#参数)
- [requests.Session()](#requestssession)

<!-- /TOC -->
# requests
## 方法
+ requests.get
+ requests.post 
+ requests.put 
+ requests.delete   
......  
+ 以上方法都可以通过以下代码指定method进行指定：requests.request(method='POST')
## 参数
+ url(string，请求的网址)
+ headers(dict，请求头)
+ cookies(dict)
+ params(dict)
    ```python
    requests.post(
        url='https://www.baidu.com',
        headers={...},
        cookies={...},
        params={'k1':'v1', 'k2':'v2'}, # ==> https://www.baidu.com?k1=v1&k2=v2
        data={...}
    )
    ```
+ data，传请求体
    ```python
    requests.post(
        ...,
        data={'user':'alex','pwd':'123'} # ==> GET /index http1.1\r\nhost:c1.com\r\n\r\nuser=alex&pwd=123
    )
    ```
+ json，传请求体
    ```python
    requests.post(
        ...,
        json={'user':'alex','pwd':'123'} # ==> GET /index http1.1\r\nhost:c1.com\r\nContent-Type:application/json\r\n\r\n{"user":"alex","pwd":123}
    )
    ```
+ proxies，代理
    - 无验证
    ```python
    proxie_dict = {
        "http": "61.172.249.96:80", # 如果是http协议就用该代理ip
        "https": "http://61.185.219.126:3128", # 如果是https协议就用该代理ip
        # 也可以指定某个网址用某个ip
        "https://www.proxy360.cn/Proxy": "http://61.185.219.126:3128"
    }
    ret = requests.get("https://www.proxy360.cn/Proxy", proxies=proxie_dict)
    ```
    - 验证代理
    ```python
    from requests.auth import HTTPProxyAuth
    
    proxyDict = {
        'http': '77.75.105.165',
        'https': '77.75.106.165'
    }
    auth = HTTPProxyAuth('用户名', '密码')
    
    r = requests.get("http://www.google.com",data={'xxx':'ffff'}, proxies=proxyDict, auth=auth) # auth传入代理的用户名和密码，正确则可以使用该代理ip
    print(r.text)    
    ```
+ files，上传文件
```python
file_dict = {
    'f1': open('xxxx.log', 'rb')
}
requests.request(
    method='POST',
    url='http://127.0.0.1:8000/test/',
    files=file_dict # key值为上传后的文件名
)
```
+ auth，认证
    - 浏览器内部：用户名和密码，用户和密码加密，放在请求头中传给（路由器）后台。
        - "用户:密码"
        - base64("用户:密码")
        - "Basic base64("用户|密码")"
        - 请求头：
            Authorization： "basic base64("用户|密码")"
    ```python
    from requests.auth import HTTPBasicAuth, HTTPDigestAuth

    ret = requests.get('https://api.github.com/user', auth=HTTPBasicAuth('wupeiqi', 'sdfasdfasdf'))
    print(ret.text)
    ```
+ timeout，超时
```python
ret = requests.get('http://google.com/', timeout=1) # 一个参数代表连接上限时间
print(ret)

ret = requests.get('http://google.com/', timeout=(5, 1)) # 两个参数，第一个代表连接，第二个代表返回
print(ret)
```
+ allow_redirects，允许重定向
```python   
ret = requests.get('http://127.0.0.1:8000/test/', allow_redirects=False) # false不允许重定向
print(ret.text)
```
+ stream，大文件下载
```python
from contextlib import closing
with closing(requests.get('http://httpbin.org/get', stream=True)) as r1:
# 在此处理响应。
for i in r1.iter_content():
    print(i)
```
+ cert，证书 
    - 百度、腾讯 => 不用携带证书（系统帮你做了）
    - 自定义证书
    ```python
    requests.get('http://127.0.0.1:8000/test/', cert="xxxx/xxx/xxx.pem")
    requests.get('http://127.0.0.1:8000/test/', cert=("xxxx/xxx/xxx.pem","xxx.xxx.xx.key"))
    ```
+ 确认 verify =False 
# requests.Session()
```python
session = requests.Session()
session.get(
    url="xxx",
    ...
    ) # 用session就不需要带cookie，内部会将所有cookie收集起来发送
```