1.替换插件地址

- http://mirror.xmission.com/jenkins/updates/update-center.json 
- https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json

2. 使用httpd代理

- host 添加 解析  127.0.0.1 updates.jenkins-ci.org
- 本地 httpd 服务器 反向代理
```
listen 80
<VirtualHost *:80>
 SSLProxyEngine on 
  
ProxyPass "/download/plugins"  "https://mirrors.tuna.tsinghua.edu.cn/jenkins/plugins/"

</VirtualHost>
```
