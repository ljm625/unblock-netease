# 解除网易云音乐海外限制

本程序主要用于解除网易云**等软件**的海外限制问题

**运行本程序的服务器并不需要在中国！**

## How to use ?

只需要找到一个服务器/本机 运行以下命令即可 （需要提前装好docker）*强烈建议部署到VPS，所有设备均可使用*

**更新**

支持dns 工作模式


运行
```dockerfile
docker run -p 80:9090 -p 53:53 -p 53:53/udp -p 9000:9000 -p 9090:9090 -d ljm625/unblock-netease
```

如果因为某些原因导致ip获取错误，（比如**本机没有公网ip**），则请手动指定ip（本机ip即可，大多数情况下127.0.0.1 works fine）

运行：
```dockerfile

docker run -p 80:9090 -p 53:53 -p 53:53/udp -p 9000:9000 -p 9090:9090 **-e "IPADDR=127.0.0.1"** -d ljm625/unblock-netease
```

之后将设备的dns服务器改为服务器IP或者本机（127.0.0.1），打开网易云稍等片刻，即可享受国区网易云！


**旧版PAC工作模式**


```dockerfile
docker run -p 9000:9000 -p 9090:9090 -d ljm625/unblock-netease
```
如果因为某些原因导致ip获取错误，（比如**本机没有公网ip**），则请手动指定ip（本机ip即可，大多数情况下127.0.0.1 works fine）

运行：
```dockerfile

docker run -p 9000:9000 -p 9090:9090 **-e "IPADDR=127.0.0.1"** -d ljm625/unblock-netease
```

然后设置你的设备pac为 http://你的ip:9000/proxy.pac

Or 将你的网易云设置**http代理**为 你的ip:9090


**此外，还可配合proxifier使用，这样可以不使用DNS以及PAC**


## How it works?
程序主要使用爬虫抓取http代理公布网站，并且对抓取结果进行**测试**，选出最佳的代理，并建立反向代理。

每隔设定的间隔，程序将再次检测代理的存活状态，如果有需要，则会自动替换代理，作为用户，只需要尽情使用即可

对于发给程序的http请求，程序会**判断目标域名是否处于白名单**中，如果处于白名单中，才会使用反向代理，否则会直接通过本地网络进行转发

## Configuration Parameters

所有的设置都在config.yaml文件中

```yaml
proxy_url: 采集代理的proxy网站
proxy_domain:
  - 代理的http请求域名列表

candidate_num: 抓取的最多代理数目
validate_url: 验证代理的URL
timeout: 验证超时时间（S）
checker_timeout: 二次验证超时时间（S）
check_interval: 检测时间（S）
# Socket params 一些socket参数，不推荐改动
buffer_size: 4096
delay_time: 0.001

# Proxy params
proxy_port: 代理端口设置

```


## Limitations and Improvements

- 目前仅限http代理，比较大的问题是他只能在http上work，如果是其他conn，比如https则会出现问题，下一步是准备实现一个socks5的反代

- 目前对各个代理网站规则内置，拟实现一个模版系统，存储不同代理网站的抓取规则

## Update History

Update 2017/11/02 : 更新了获取内网IP的方法，并将设定固定ip配置到了环境变量中

Update 2017/11/01 : Update Wiki details

Update 2017/11/01 : Update 了一下具体参数，代理效果更好

## Contributions

欢迎大家进行测试，提出issue，如果有任何问题，也可以发信到ljm625#gmail.com咨询


# English Version pending