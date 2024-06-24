---
title: 利用Cloudflare Pages搭建telegraph-Image图床
date: 2024-06-24 08:41:00
updated: 2024-06-24 08:41:00
tags: [图床搭建]
categories: 开发笔记
description: 利用Cloudflare Pages搭建图床
top_img: https://img.090227.xyz/file/57b81d8fc9d8d534cabef.jpg
cover: https://img.090227.xyz/file/57b81d8fc9d8d534cabef.jpg
main_color: "#83877F"
---

# [telegraph-Image：搭建你的专属开源图床](https://github.com/x-dr/telegraph-Image)

通过 **Telegraph** 与**赛博菩萨**提供的 Pages、D1，搭建一个专属于你自己的开源图床，如果你有更多需求还可通过优选加速图片载入时间，更有接入ModerateContent提供的审查图像内容的API key，过滤涩涩内容！

### 优点

1. **无限图片储存数量**，你可以上传不限数量的图片 **but！单张图片不能超过5MB**
2. **无需服务器**，托管于Cloudflare的网络上，当使用量不超过Cloudflare的免费额度时，完全免费
3. **无需域名**，可以使用Cloudflare Pages提供的*.pages.dev的免费二级域名，同时也支持绑定自定义域名
4. **支持图片审查API**，可根据需要开启，开启后不良图片将自动屏蔽，不再加载
5. **支持后台管理**，日志管理，查看访问前20的Referer、IP、img,可以对上传的图片进行在线预览，添加白名单，黑名单等操作

------

## 开始部署

### 1. Pages 部署 **telegraph-Image 项目**

- 打开[telegraph-Image仓库](https://github.com/x-dr/telegraph-Image)项目，先给作者点击`Star`后再点击`Fork`！可以增加成功率！！！**手动狗头**
- 回到 **Workers 和 Pages** > **概述** > **创建** > **Pages** > **连接到Git** > 选择`telegraph-Image`项目 > **保存并部署**即可

------

### 2. 绑定自定义域

- 这里推荐优先使用已经转入CF的域名，并开启**小黄云**。如果你没有域名，也可以退而求其次使用**CNAME方式**使用免费域名接入自定义域。
- 回到 **Workers 和 Pages /**`telegraph-Image`项目 > **设置** > **函数** > **放置** > **制作** > **智能** > **保存**

------

### 3. 创建管理后台

- 3.1 回到 **Workers 和 Pages** > **D1** > **创建数据库** > **仪表盘** > 数据库名称`img`*(名称可取任意值)* > **创建**

- 3.2 进入 `img`数据库 >控制台>`粘贴以下代码`后 >点击 **执行**\> 等待提示**此查询已成功执行**。

  ```sql
  DROP TABLE IF EXISTS tgimglog;
  CREATE TABLE IF NOT EXISTS tgimglog (
      `id` integer PRIMARY KEY NOT NULL,
      `url` text,
      `referer` text,
      `ip` varchar(255),
      `time` DATE
  );
  DROP TABLE IF EXISTS imginfo;
  CREATE TABLE IF NOT EXISTS imginfo (
      `id` integer PRIMARY KEY NOT NULL,
      `url` text,
      `referer` text,
      `ip` varchar(255),
      `rating` text,
      `total` integer,
      `time` DATE
  );
  ```

- 3.3 回到 **Workers 和 Pages /**`telegraph-Image`项目 > **设置** > **函数** > **D1 数据库绑定** > 变量名`IMG` > `img`数据库 > 点击**保存**

- 3.4 回到 **Workers 和 Pages /**`telegraph-Image`项目 > **设置** > **环境变量** \> **为生产环境定义变量** \> 变量内容如下:

  - 变量名`BASIC_USER`，值为你的**后台管理员用户名**
  - 变量名`BASIC_PASS`，值为你的**后台管理员密码**
  - **无需审查涩涩内容可跳过这一步**
    - 打开[ModerateContent](https://moderatecontent.com/signup) API申请页面，输入你的邮箱后点击**SUBMIT**
    - 前往你的邮箱将 **ModerateContent.com Sup** 邮件内的 **API Key** 复制出来
    - 变量名`ModerateContentApiKey`，值为你的 **API Key**
  - 点击**保存**

- 3.5 回到 **Workers 和 Pages /**`telegraph-Image`项目 > **部署** > 右下角**三个点** > **重试部署**即可

------

## 如何使用

例如：你的 **Pages自定义域** 为`img.131213.xyz`

- `https://img.131213.xyz` 为 **图床上传使用地址**
- `https://img.131213.xyz/admin` 为 **图床后台管理地址**
- `https://img.131213.xyz/list` 为 **图床访问日志**

------

## 变量说明

| 变量名                | 示例                             | 备注                                                |
| --------------------- | -------------------------------- | --------------------------------------------------- |
| BASIC_USER            | admin                            | 后台管理员用户名                                    |
| BASIC_PASS            | 123456                           | 后台管理员密码                                      |
| ModerateContentApiKey | 8ba353957d6c2bea538dca28a66a04cd | 审查图像内容的API key                               |
| RATINGAPI             | `https://xxx.xxx/rating`         | [自建的鉴黄api](https://github.com/x-dr/nsfwjs-api) |

注意优先级 `RATINGAPI` > `ModerateContentApiKey`

------

# 感谢

CM大佬
