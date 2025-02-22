---
title: 购买域名并使用Cloudflare&DDNS-GO开启DDNS
date: 2025-02-22
category: 网络
tags: [网络, DDNS, Cloudflare]
cover: https://s2.loli.net/2025/02/22/E39fJANOGgbVc5T.jpg
---

TP-LINK的免费DDNS要在6/30停服...只能去买个六位数xyz域名当做DDNS的药引子

##### Step.1 买一个域名

www.spaceship.com 六位数xyz域名十年，花费50元。要先买完才能在**Launchpad**→**域名管理**中再续费9年，等于10年的。

![image-20250222195343590](https://s2.loli.net/2025/02/22/1t9nxfujviVkaGC.png)

![image-20250222195742564](https://s2.loli.net/2025/02/22/wBhZl3qyIfMvX6H.png)

![](https://s2.loli.net/2025/02/22/ipKn9M71l2IDWVs.png)

##### Step.2 更改DNS名称服务器（注册商自带也可跳过）

在 **高级DNS** 中，将DNS名称服务器改为Cloudflare的地址，地址要在Cloudflare的**添加域**页面的提示中找到

![image-20250222200449321](https://s2.loli.net/2025/02/22/C7VHcGIN5OTtjhB.png)

![image-20250222200639293](https://s2.loli.net/2025/02/22/Xb5iAVefvkWHKLO.png)

保存后等待Cloudflare检测到活动即可。

![image-20250222200132976](https://s2.loli.net/2025/02/22/qAmpn1Y5s3tKfBM.png)

##### Step.3 设置DDNS-GO

①先获取**令牌**，在https://dash.cloudflare.com/profile/api-tokens

![image-20250222201825858](https://s2.loli.net/2025/02/22/5ryHY9XQj8n1kz7.png)

红框内选择刚才添加的域名，下一步即可获得令牌

![image-20250222201921371](https://s2.loli.net/2025/02/22/iPnCIvzjYQNT3oB.png)

②在DDNS-GO中选择服务商，添加刚才的**令牌**，按需勾选，保存后即可在日志中查看运行情况

![image-20250222202401482](https://s2.loli.net/2025/02/22/qU2GPdBowDiYE9K.png)

> 注意Cloudflare不再支持.cf .ga .ml .tk 顶级域名（都是freenom提供的免费域名）,通过调用api的方式进行配置了。只能登录Cloudflare 控制面板手动配置。
