---
title: 利用github + picgo + jsdeliv搭建免费图床
date: 2024-06-29 00:00:00
updated: 2024-06-29 00:00:00
tags: [图床搭建]
categories: 开发笔记
description: 利用github + picgo + jsdeliv搭建免费图床
top_img: https://cdn.jsdelivr.net/gh/kayee0212/blog-img-1@main/6%E6%9C%8829%E6%97%A5.png
cover: https://cdn.jsdelivr.net/gh/kayee0212/blog-img-1@main/6%E6%9C%8829%E6%97%A5.png
main_color: "#F6F2F9"
---

# 前期准备

1.**GitHub**账号（没有的话可以先去注册一个）

​	注册地址：[https://github.com/](https://github.com/)

2.**PicGo**下载（`官方` 或 `github` 访问不了 就去 `山东大学镜像站` 下载）

​	官方网站：[https://picgo.github.io/PicGo-Doc/zh/](https://picgo.github.io/PicGo-Doc/zh/)

​	下载地址（github）：[https://github.com/Molunerfinn/PicGo/releases](https://github.com/Molunerfinn/PicGo/releases)

​	下载地址（山东大学镜像站）：[https://mirrors.sdu.edu.cn/github-release/Molunerfinn_PicGo](https://mirrors.sdu.edu.cn/github-release/Molunerfinn_PicGo)

3.**jsdelivCDN**配置

```markdown
https://cdn.jsdelivr.net/gh/[github用户名]/[仓库名称]@main
```

如果`github`访问不了的话可以下载一个 **Watt Toolkit**（原steam++）

下载地址：[https://steampp.net/](https://steampp.net/)

下载好之后直接按下图步骤点击加速即可

![Watt Toolkit](https://img.bolong.eu.org/file/9d4f968459579bae65810.png)

# 🍜开始搭建

### 1.创建github仓库

登录自己的`github`账号 点击 **new** 创建一个新的仓库

![image-20240629112526799](https://cdn.jsdelivr.net/gh/kayee0212/blog-img-1@main/image-20240629112526799.png)

跳入创建页配置仓库信息

![image-20240629113320569](https://cdn.jsdelivr.net/gh/kayee0212/blog-img-1@main/image-20240629113320569.png)

### 2.配置并获取GitHub的token值

点击`右上角头像`找到 **Settings** 选项 点击进入

![image-20240629114025312](https://cdn.jsdelivr.net/gh/kayee0212/blog-img-1@main/image-20240629114025312.png)

![image-20240629114156537](https://cdn.jsdelivr.net/gh/kayee0212/blog-img-1@main/image-20240629114156537.png)

进入`Settings`页面 下拉找到 **Developer Settings** 选项 点击进入

![image-20240629120847752](https://cdn.jsdelivr.net/gh/kayee0212/blog-img-1@main/image-20240629120847752.png)

进入`Developer Settings`页面 下拉 **Personal access tokens** 选项 点击 **Tokens (classic)** 选项

![image-20240629121356711](https://cdn.jsdelivr.net/gh/kayee0212/blog-img-1@main/image-20240629121356711.png)

点击右侧 **Generate new token** 下拉选项 选择第二项 **Generate new token (classic)** 进入创建token页面

![image-20240629122000597](https://cdn.jsdelivr.net/gh/kayee0212/blog-img-1@main/image-20240629122000597.png)

进入到创建token页面 配置以下选项 配置完毕后往下拉点击创建 **Generate new token** 按钮

![image-20240629123015016](https://cdn.jsdelivr.net/gh/kayee0212/blog-img-1@main/image-20240629123015016.png)

![image-20240629123239212](https://cdn.jsdelivr.net/gh/kayee0212/blog-img-1@main/image-20240629123239212.png)

创建完成之后会出现token值 这个 **token** 值`只会出现这一次`一定要 {% span red, 保存%} 下来

![image-20240629123741177](https://cdn.jsdelivr.net/gh/kayee0212/blog-img-1@main/image-20240629123741177.png)

### 3.在PicGo中添加GitHub配置

安装运行PicGo可以先先到设置中把这个勾选掉（当然这个不是必须的哈 只是不会让自己看到这么多的选项而已🐶）

![image-20240629124844794](https://cdn.jsdelivr.net/gh/kayee0212/blog-img-1@main/image-20240629124844794.png)



上边的选项可以跳过 直接从这里看

选择`图床设置`下拉菜单 选择 **GitHub** 选项

![image-20240629125616170](https://cdn.jsdelivr.net/gh/kayee0212/blog-img-1@main/image-20240629125616170.png)

进入到 **GitHub** 配置页 点击`添加新配置`

![image-20240629125900581](https://cdn.jsdelivr.net/gh/kayee0212/blog-img-1@main/image-20240629125900581.png)

根据下图配置 配置完成之后就可以点击 **确定** 按钮进行添加了

![image-20240629131701041](https://cdn.jsdelivr.net/gh/kayee0212/blog-img-1@main/image-20240629131701041.png)

各配置项解释

​	图床配置名：给图床起一个名字

​	设定仓库名：你的 **GitHub** 名称/第一步新建的图床仓库名称 例如：`kayee0212/blog-img-2`

​	设定分支名：默认填写main 如果有特殊需求可以填写其他分支

​	设定Token：这是第二步不配置保存下来的 **GitHub token值**

​	设定存储路径：这个是存储在图床仓库那个文件夹下 默认不填写 如果有特殊需求可以填写 例如：`img/`

​	设定自定义域名：这个是 **jsdelivCDN** 域 必须配置 如果不配置国内可能访问不到 链接为：`https://cdn.jsdelivr.net/gh/[github用户名]/[仓库名称]@main` 将 **[github用户名]** 和 **[仓库名称]** 替换为自己的 github用户名 和 仓库名称 例如：`https://cdn.jsdelivr.net/gh/kayee/blog-img-2@main`

**至此 所有配置就完成了 之后就可以愉快的使用起来了**🦧🦧🦧

### 4.使用PicGo上传图片

选择 **上传区** 右侧 **下拉选项** 找到 **GitHub** 选择 **你创建的图床配置** 

![image-20240629133756572](https://cdn.jsdelivr.net/gh/kayee0212/blog-img-1@main/image-20240629133756572.png)

选择完之后就可在上传处上传你的图片了

![image-20240629134139214](https://cdn.jsdelivr.net/gh/kayee0212/blog-img-1@main/image-20240629134139214.png)

上传完成之后的图片会自动添加到 **剪贴板**内 直接在 **文体处粘贴** 即可使用！！！

# 结语

OK 结束👌

有什么不明白的可以在下方评论区留言！！！

也可以在网页 页脚通过邮箱告知我🤞🤞🤞

再不行QQ找我（2937589293 添加时说明来意）🧓

