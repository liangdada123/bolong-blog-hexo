---
title: 【CloudFlare Workers搭建免费vless节点】 利用作者zizifn脚本进行部署，稳定高效，解锁ChatGPT和奈飞流媒体
date: 2024-06-24 20:14:00
updated: 2024-06-25 11:50:00
tags: [节点搭建, CloudFlare]
categories: 科学上网
description: 利用CloudFlare Workers搭建免费vless节点系列一
top_img: https://img.bolong.eu.org/file/426b0e0896eecc1710e11.png
cover: https://img.bolong.eu.org/file/426b0e0896eecc1710e11.png
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

# ☕️开始搭建

#### 1.注册cloudflare账号并登录 

(这个就不多赘述了和平时注册其他网站一样按要求填写即可)(什么？你这都不会！？那你不太适合本教程)🐶

**cloudflare**官网：🌐**网址**：https://dash.cloudflare.com/



#### 2.在cloudflare创建Workers

注册完成之后进入cloudflare管理页 点击打开 **Workers和Pages** 页点击 **创建**

![c76d348ae11eefc52e08c.png](https://img.bolong.eu.org/file/c76d348ae11eefc52e08c.png)

跳转到创建页 选择  **Workers** 点击 **创建 Workers**

![8b8e3f203f4dad65d40cd.png](https://img.bolong.eu.org/file/8b8e3f203f4dad65d40cd.png)

在这里填写一个你喜欢的**项目名称**(不一定要跟我一样) 然后点击 **保存** 后点击 **完成**

![734d76e8f0ebd7f952c65.png](https://img.bolong.eu.org/file/734d76e8f0ebd7f952c65.png)

![60cbe6841a2fa19e78ee0.png](https://img.bolong.eu.org/file/60cbe6841a2fa19e78ee0.png)

创建完成之后点击 **编辑代码** 会进入到**代码编辑页面**

![b6d4cd65e36634f2860bd.png](https://img.bolong.eu.org/file/b6d4cd65e36634f2860bd.png)

![89cb741acb57dec0a5a2b.png](https://img.bolong.eu.org/file/89cb741acb57dec0a5a2b.png)



#### 3.添加zizifn大佬的 worker-vless.js 脚本

(这里需要访问 **github** 如果有小伙伴访问不了的话 可以去下一个 **Watt Toolkit** 选中 **Github** 点击 **一键加速** 即可)

![9d4f968459579bae65810.png](https://img.bolong.eu.org/file/9d4f968459579bae65810.png)



**zizifn大佬** 脚本链接：[https://github.com/zizifn/edgetunnel/blob/main/src/worker-vless.js](https://github.com/zizifn/edgetunnel/blob/main/src/worker-vless.js)



来到大佬的脚本后点击 **复制** 按钮 即可复制脚本内容

![0f046e38ba877034bb7a3.png](https://img.bolong.eu.org/file/0f046e38ba877034bb7a3.png)

复制完脚本内容之后来到刚才创建的 **cloudflare Workers** 项目 **代码编辑页面** 直接**Ctrl+A**全选然后再**Ctrl+V**粘贴 大佬的脚本代码

![72145db21f461f7d02048.png](https://img.bolong.eu.org/file/72145db21f461f7d02048.png)

粘贴覆盖完成之后 修改 **userID** 的值和 **proxyIP** 的值(这两个值一般在代码的最顶部 直接把滑块来的顶就行)

![c54d4cb374e22459e4e85.png](https://img.bolong.eu.org/file/c54d4cb374e22459e4e85.png)

这里的 **userID** 的值是生成的 **UUID** 的值 `不要用代码里自带的` (如何生成？如果你不会的话可以到这个网站来生成 网址：[https://1024tools.com/uuid](https://1024tools.com/uuid))

![6d342c382182f6cbad539.png](https://img.bolong.eu.org/file/6d342c382182f6cbad539.png)

生成后随便复制一条 **UUID** 粘贴覆盖到 **userID** 的值(这里我已我在网站生成出来的第一条UUID值为例 你只需要复制出你生成出来的UUID就行 每个人生成出来的是不一样的)

![8b9c998d6e4f72978cbc6.png](https://img.bolong.eu.org/file/8b9c998d6e4f72978cbc6.png)

替换完成之后 接下来添加 **proxyIP** 的值 下面随便复制一条 粘贴到 **proxyIP** 的值中(这里的proxyIP是反代 cloudflare 的 CDN 服务器ip 是为了解锁ChatGPT和奈飞流媒体用的)

**146.70.175.98**
**146.70.175.99**
**146.70.175.100**
**146.70.175.101**
**146.70.175.102**
**146.70.175.103**
**146.70.175.104**
**146.70.175.116**

这里我还是以第一条为例

![ed4f96dd7dd68e39a8b25.png](https://img.bolong.eu.org/file/ed4f96dd7dd68e39a8b25.png)

配置完成这两个值之后 点击 **部署** 按钮 之后点击 **保存并部署** 按钮 出现 **版本已保存** 的绿框 就说明部署成功了

![21dc7a959999b31b2b1ec.png](https://img.bolong.eu.org/file/21dc7a959999b31b2b1ec.png)

![4f69598a22d5a859cc75c.png](https://img.bolong.eu.org/file/4f69598a22d5a859cc75c.png)

![bc02385a5a767c3a98aa1.png](https://img.bolong.eu.org/file/bc02385a5a767c3a98aa1.png)



#### 4.获取vless节点并实现科学上网

在代码编辑页面点击复制**访问网址** 并在 浏览器打开**新标签页**粘贴

![00641c5c2d47d51fbe708.png](https://img.bolong.eu.org/file/00641c5c2d47d51fbe708.png)

 在后面加上之前生成填写到代码 **userID** 中的 **UUID** 值进行访问(例如我复制出来的访问地址是 https://xxx.xxxx.workers.dev 那我就在后边加 **/UUID值**)

![5e4c244a3a0a1169dbb12.png](https://img.bolong.eu.org/file/5e4c244a3a0a1169dbb12.png)

这样我们就能看到 部署到**cloudflare workers**的 **vless** 节点以及节点信息 复制出 **vless节点** 添加到科学上网工具中(这里我已**v2rayN**为例)

![fc2ff2749c7ae36bb733b.png](https://img.bolong.eu.org/file/fc2ff2749c7ae36bb733b.png)

如果没有**v2rayN**可前往这个链接：[https://github.com/2dust/v2rayN/releases](https://github.com/2dust/v2rayN/releases)

不会使用这个软件的可以去网上搜索教程 这里就不多赘述

安装之后打开 **v2rayN** 粘贴刚才复制出的 **vless节点**

![99ad75bd6e24408ed27ff.png](https://img.bolong.eu.org/file/99ad75bd6e24408ed27ff.png)

然后选中这个节点右键 **编辑服务器** 并选取下面的任意一个 **优选域名** 填入到 **地址** 框中 并将 **端口** 改为**80**

**www.visa.com**
**time.cloudflare.com**
**shopify.com**
**time.is**
**icook.hk**
**icook.tw**
**ip.sb**
**japan.com**
**malaysia.com**
**russia.com**
**singapore.com**
**skk.moe**
**www.visa.com**
**www.visa.com.sg**
**www.visa.com.hk**
**www.visa.com.tw**
**www.visa.co.jp**
**www.visakorea.com**
**www.gco.gov.qa**
**www.gov.se**
**www.gov.ua**
**www.digitalocean.com**
**www.csgo.com**
**www.shopify.com**
**www.whoer.net**
**www.whatismyip.com**
**www.ipget.net**
**www.hugedomains.com**
**www.udacity.com**
**www.4chan.org**
**www.okcupid.com**
**www.glassdoor.com**
**www.udemy.com**
**www.baipiao.eu.org**
**cdn.anycast.eu.org**
**cdn-all.xn--b6gac.eu.org**
**cdn-b100.xn--b6gac.eu.org**
**xn--b6gac.eu.org**
**edgetunnel.anycast.eu.org**
**alejandracaiccedo.com**
**nc.gocada.co**
**log.bpminecraft.com**
**www.boba88slot.com**
**gur.gov.ua**
**www.zsu.gov.ua**
**www.iakeys.com**
**edtunnel-dgp.pages.dev**
**www.d-555.com**
**fbi.gov**

这里我还是以第一条为例

![7f176c77b887b53e87145.png](https://img.bolong.eu.org/file/7f176c77b887b53e87145.png)

![c535c853600dc2ebcc1ad.png](https://img.bolong.eu.org/file/c535c853600dc2ebcc1ad.png)

在更改 **传输层安全** 为空 点击 **确定** 按钮保存即可

![f71a2fa884edd65f52d46.png](https://img.bolong.eu.org/file/f71a2fa884edd65f52d46.png)

![0a4640370b3449ef7f7c8.png](https://img.bolong.eu.org/file/0a4640370b3449ef7f7c8.png)

至此我们搭建的 **vless节点** 就能用来科学上网了 只要将这个节点设为 **活动服务器** 然后 **开启系统代理** 就OK了

![fb7477dbee9d9b96c775a.png](https://img.bolong.eu.org/file/fb7477dbee9d9b96c775a.png)

![fa5cebaf2f8e193b6b5ce.png](https://img.bolong.eu.org/file/fa5cebaf2f8e193b6b5ce.png)

附访问YouTube速度测试

![2d6ea73cec60dd0f1581f.png](https://img.bolong.eu.org/file/2d6ea73cec60dd0f1581f.png)

# 结语

这就是全部教程了！！！

有什么不懂的 或者 工具不会使用的可以在下方评论区留言 我会收集反馈比较多的问题单独出一期文章 或者 来QQ(2937589293)找我 (添加时请说明来意)

OK结束
