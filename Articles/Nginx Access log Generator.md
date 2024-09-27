# Nginx Access.log Generator

Created: September 12, 2024 1:48 PM
Labels & Language: Python, http, nginx

## 引言

Nginx是一种流行的开源Web服务器和反向代理服务器技术。它旨在处理高流量的网站并快速高效地提供Web内容。Nginx还可用作负载均衡器、HTTP缓存和电子邮件协议（IMAP、POP3和SMTP）的代理服务器等。本篇笔记记录的技术，旨在通过模拟多个IP登录访问nginx的`index.html`页面，达到生成数量级达到至多每日千万体量的访问日志的效果。这些日志文件数据将作为后续分析的基础。

## Nginx生成访问日志

### 程序设计概述

考虑项目的性质和成本，将采用nginx架构的django项目部署在本地虚拟机中，模拟公网中建立的网站（实际在内网进行操作），该虚拟机服务器采用Ubuntu Linux操作系统。搭载Windows系统的本地主机作为发起访问请求的实体，通过`python requests`库编写的自动脚本，实现千万数量级的访问操作。在这个自动脚本中，除了本机访问外，为了模拟多个虚拟`IP`，使用基于`squid`的`proxy`代理技术，通过另外3台配置有`squid`代理的由虚拟机模拟的服务器（分别名为`hadoop102`, `hadoop103`, `hadoop104`）转发访问请求，达到`access.log`中多个不同`IP`访问的效果。使用`fake_useragent`库实现对访问请求信息的随机生成，将`header`伪装成多个不同来源，如不同的浏览器、操作系统等。

### 为代理服务器配置squid转发

- 在代理服务器上以root用户身份执行安装`squid`：

```bash
yum install squid # hadoop102-104服务器使用的是CentOS系统
```

- 修改 Squid 配置文件`/etc/squid/squid.conf`，进行相应更改并完成配置：

```bash
http_port 3128 # 默认监听地址和端口
http_access allow all # 允许所有客户端连接
-------------------------------------------------------
systemctl restart squid # 保存配置文件并重新启动squid服务
```

- 使用集群分发脚本`xsync`同步所有`hadoop`服务器的代理配置信息：

```bash
xsync /etc/squid/squid.conf # 见[Hadoop3.x Basics](http://124.222.120.214/media/notion_files/070523/hadoop_basics_notes.html)
```

### 部署基于Nginx架构的网站

运行网站的服务器端采用基于Django的web架构，并通过nginx进行部署。有关该网站架构的技术细节，可以参照我另一篇笔记：[**`Django Basics`**](http://124.222.120.214/media/notion_files/070923/Django%20Basics%20668d2388c67b47869a192459163ec675.html)

关于该网站的nginx配置信息，可以参照这份配置时留存的操作备份：[`deploy_nginx_on_server`](http://124.222.120.214/media/notion_files/070923/deploy_nginx_on_server.txt)

- 使用`uwsgi`运行`Django`工程，并启动`nginx`服务使之在内网上线：

```bash
nohup uwsgi --ini uwsgi.ini &
service nginx start
```

- 在Windows主机访问`nginx`网站`homepage.html`页面，页面返回正常。
- 检查`access.log`中是否正确生成了日志：

```bash
# 相比原文件，增加了新访问时生成的3条日志，分别获取了主页的html,css,和js文件：
192.168.10.1 - - [09/Jul/2023:20:51:23 +0800] "GET /homepage/ HTTP/1.1" 200 5266 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36 Edg/114.0.1823.67"
192.168.10.1 - - [09/Jul/2023:20:51:23 +0800] "GET /static/js/scripts.js HTTP/1.1" 404 1035 "http://192.168.10.128/homepage/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36 Edg/114.0.1823.67"
192.168.10.1 - - [09/Jul/2023:20:51:23 +0800] "GET /static/css/styles.css HTTP/1.1" 404 1029 "http://192.168.10.128/homepage/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/114.0.0.0 Safari/537.36 Edg/114.0.1823.67"
```

### 通过代理转发、header伪装、自动化脚本生成大量日志数据

- 如程序设计概述中所述，使用`requests`库实现批量访问：

```python
import requests
from fake_useragent import UserAgent

def send_proxy_requests():
    proxy_list = ['hadoop102:3128', 'hadoop103:3128', 'hadoop104:3128']  # 代理服务器列表
    nginx_url = 'http://192.168.10.128/homepage/'  # Nginx的URL

    for proxy in proxy_list:
        proxies = {
            'http': 'http://' + proxy,
            'https': 'https://' + proxy
        }

        user_agent = UserAgent().random
        headers = {'User-Agent': user_agent}

        try:
            response = requests.get(nginx_url, proxies=proxies, headers=headers)
            print('IP {} 访问结果: {}'.format(proxy, response.status_code))
        except requests.exceptions.RequestException as e:
            print('IP {} 访问异常: {}'.format(proxy, str(e)))

def send_local_request():
    nginx_url = 'http://192.168.10.128/homepage/'
    try:
        response = requests.get(nginx_url)
        print('IP {} 访问结果: {}'.format("localhost:80", response.status_code))
    except requests.exceptions.RequestException as e:
        print('IP {} 访问异常: {}'.format("localhost:80", str(e)))

# 测试：生成400条数据：
for request_index in range(100):
    send_proxy_requests()
    send_local_request()
59-458
```

- 查看access.log，成功生成相应的日志文件。该日志文件的下载链接：[`access.log`](http://124.222.120.214/download/access_log.txt)
- 其中第59行~458行为运行上述代码生成的日志，前58行为此前其他测试生成。

```python
# 部分日志一览（240~249）
192.168.10.103 - - [09/Jul/2023:21:20:48 +0800] "GET /homepage/ HTTP/1.1" 200 21552 "-" "Mozilla/4.0 (compatible; MSIE 6.0; Linux i686 ; en) Opera 9.70"
192.168.10.104 - - [09/Jul/2023:21:20:48 +0800] "GET /homepage/ HTTP/1.1" 200 21552 "-" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.0) Opera 7.10  [en]"
192.168.10.1 - - [09/Jul/2023:21:20:48 +0800] "GET /homepage/ HTTP/1.1" 200 5266 "-" "python-requests/2.31.0"
192.168.10.102 - - [09/Jul/2023:21:20:48 +0800] "GET /homepage/ HTTP/1.1" 200 21552 "-" "Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_5_2; pt-br) AppleWebKit/525.13 (KHTML, like Gecko) Version/3.1 Safari/525.13"
192.168.10.103 - - [09/Jul/2023:21:20:48 +0800] "GET /homepage/ HTTP/1.1" 200 21552 "-" "Mozilla/5.0 (Macintosh; U; PPC Mac OS X; nl-nl) AppleWebKit/418.8 (KHTML, like Gecko) Safari/419.3"
192.168.10.104 - - [09/Jul/2023:21:20:48 +0800] "GET /homepage/ HTTP/1.1" 200 21552 "-" "Mozilla/5.0 (Windows; U; Windows NT 6.0; en-US) AppleWebKit/525.19 (KHTML, like Gecko) Chrome/1.0.154.59 Safari/525.19"
192.168.10.1 - - [09/Jul/2023:21:20:48 +0800] "GET /homepage/ HTTP/1.1" 200 5266 "-" "python-requests/2.31.0"
192.168.10.102 - - [09/Jul/2023:21:20:48 +0800] "GET /homepage/ HTTP/1.1" 200 21552 "-" "Mozilla/5.0 (X11; U; Linux x86_64; de; rv:1.8.1.12) Gecko/20080208 Fedora/2.0.0.12-1.fc8 Firefox/2.0.0.12"
192.168.10.103 - - [09/Jul/2023:21:20:48 +0800] "GET /homepage/ HTTP/1.1" 200 21552 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML like Gecko) Chrome/51.0.2704.79 Safari/537.36 Edge/14.14931"
192.168.10.104 - - [09/Jul/2023:21:20:48 +0800] "GET /homepage/ HTTP/1.1" 200 21552 "-" "Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 6.0; en) Opera 11.00"
```

## Reference

- 在操作过程中采用了的ChatGPT提供的建议的对话链接：

[ChatGPT](https://chat.openai.com/share/16f97992-a96a-4c43-9970-7c963f9e1be0)