---
title: 【CloudFlare Workers搭建免费vless节点系列二】 利用勇哥yonggekkk脚本进行部署，稳定高效，解锁ChatGPT和奈飞流媒体
date: 2024-06-24 20:58:00
updated: 2024-06-24 20:58:00
tags: [节点搭建, CloudFlare]
categories: 科学上网
description: 利用CloudFlare Workers搭建免费vless节点系列二
top_img: https://lovetoshare.top/images/202406201540.png
cover: https://lovetoshare.top/images/202406201540.png
main_color: "#348CFF"
---

# 一、前言

本期主要来简单介绍如何利用**cf worker**搭建免费的**vless**节点，部署过程应该比较简单，一方面帮助新手快速上手，另一方面分享自己的使用心得，
主要是解决目前面临的两大问题，一是按照以往的代码搭建可能会出现**1101**的错误，另一个是很多人搭建的vless节点不能100%使用**ChatGPT**以及观看**奈飞**。

# 二、实现原理

简单说明一下实现原理，因为**cloudflare**作为一家良心云，他家的worker可以运行**JavaScript**代码，因此就有一些”别有用心“的人（大神们）编写代码部署在worker上，为数据包添加vlessheader信息，达到vless代理转发的目的。
由于cloudflare在国内某种程度能访问，而CF Worker又能访问GFW以外的信息，所以通过CF Worker来转发数据就能**实现翻墙**的效果。

# 三、补充说明

免费的东西总不可能很完美，CF Worker限制每天**10万个10ms CPU time**的请求，但对于个人使用应该足够了。另外由于cf bug，现在cf worker不能直接访问cf托管的网站，比如**speedtest、ChatGPT**等，所以需要配置一个**反代ip**，用来转发cf的流量。
因此我们在实际cf worker搭建中，不仅要**优选ip**，最好还要搭配一个**反代ip**来使用。至于能不能解锁ChatGPT，关键在于你的**反代ip**或**反代域名**。目前我收集整理了一些反代域名，实际使用下来，因为ip在变化，有时候的ip能解锁ChatGPT，因此你只需要将反代域名此时对应的ip提取出来，就能使用ChatGPT了，当然需要你自行多次尝试了！毕竟免费的就要**付出时间和精力**来折腾。

# 开始搭建

1.注册cloudflare账号并登录 (这个就不多赘述了和平时注册其他网站一样按要求填写即可)(什么你这都不会！？那你不太适合本教程)

**cloudflare**官网 : 🌐**网址**：https://dash.cloudflare.com/



