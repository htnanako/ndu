# NDU

![GitHub repo size](https://img.shields.io/github/repo-size/htnanako/ndu)
![Docker Image Size (tag)](https://img.shields.io/docker/image-size/htnanako/ndu/latest)
![Docker Pulls](https://img.shields.io/docker/pulls/htnanako/ndu)
[![GitHub Repo stars](https://img.shields.io/github/stars/htnanako/ndu?style=social)](https://github.com/htnanako/ndu/stargazers)

# About

### 本项目用于监测Dockerhub的容器更新，并推送通知提醒。
### 支持多通知渠道(bark,telegram,企业微信)
### 支持多镜像多tag同时监测

# Start

### Docker部署

- Docker Cli
```shell
docker run -itd --name=ndu -v /path/to/ndu:/data -p 5050:5050 htnanako/ndu:latest
```

- Docker Compose
```yaml
version: "3"
services:
  ndu:
    image: htnanako/ndu:latest
    container_name: ndu
    network_mode: bridge
    ports:
      - 5050:5050
    volumes:
      - ./:/data
```

### Configuration

> 容器启动后，先关闭容器。
> 
> 在映射路径下找到conf/base_config.yml文件
> 
> 自动生成的配置文件如下所示

```yaml
ndu:
  interval: 60 # 监测任务的间隔时间，单位为秒
  registry:
    username: '' # 暂时无用，可不填
    password: '' # 暂时无用，可不填
  notify: bark,telegram,qywx # 通知渠道，可选bark,telegram,qywx，多个渠道用逗号分隔
notify:
  public:
    notify_img: '' # 通知图片，有需要可自定义，也可不填
  qywx:
    qywx_base_url: https://qyapi.weixin.qq.com # 企业微信API地址，有需要可自定义
    corpid: '' # 企业微信corpid
    corpsecret: '' # 企业微信corpsecret
    agentid: '' # 企业微信agentid
    touser: '@all' # 企业微信通知对象，@all为所有人，也可指定成员，暂时不支持多个
  bark:
    bark_url: '' # bark接口地址及Key
    bark_sound: chime # bark通知铃声
    bark_group: NDU # bark通知分组
    bark_icon: '' # bark通知图标
  telegram:
    tg_base_url: https://api.telegram.org/bot # telegram的api服务地址，默认是官方，可以改为自建
    tgbot_token: '' # telegram bot的Token，BotFather获取到的
    tg_chat_id: '' # 接收消息的chat_id，可为频道、群组或个人
    proxy: '' # 留空则不使用代理。支持通过HTTP代理、SOCKS代理发送消息。示范：http://localhost:8030 或 socks5://user:pass@host:port
images:
- htnanako/ndu:latest # 填写需要监测的镜像名:tag，如不加tag默认为latest，多个镜像按相同格式一行一个
```


## 支持一下

你可以送我一杯咖啡，以表示对这个项目的支持😉

<img src="https://nanako-1253183981.cos.ap-guangzhou.myqcloud.com/public-IMG/bmc_qr.png" width="300" />