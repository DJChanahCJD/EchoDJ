---
title: 【个人无限免费图床】Telegraph+PicGo+Typora（完整教程）
date: 2024-06-12 12:59:01
tags:
  - 教程
categories:
  - 有趣の东西
comments: true
---
![https://telegraph-image-177.pages.dev/file/0bfc14ccfe425244f65f3.png](https://telegraph-image-177.pages.dev/file/0bfc14ccfe425244f65f3.png)

<!--StartFragment-->

## 概要

本文介绍了一种无需服务器即可搭建无限空间且完全免费的图床的方法，适用于个人博客、社交媒体平台以及网页设计师。该方法主要利用了Cloudflare Pages和Telegraph，实现了图片的上传、存储与管理功能。以下是详细的搭建步骤和注意事项。

## Telegraph搭建步骤

1. **前提准备**：

   * 拥有**GitHub**和**Cloudflare**账号（可能需要魔法🧙‍♀️）。
2. **Fork项目**：

   * 访问[Telegraph-Image](https://github.com/cf-pages/Telegraph-Image)项目地址，并Fork到自己的GitHub账户。
3. **Cloudflare Pages创建项目**：

   * 登录[Cloudflare](https://www.cloudflare-cn.com/)，选择创建项目，链接到刚刚Fork的GitHub项目。![image-20240611165154730](https://telegraph-image-177.pages.dev/file/e85830eee8240b214a04c.png)
   * 选择对应的仓库（默认为`Telegraph-Image-`），**其他默认**配置进行部署，完成后会生成一个免费的域名。![image-20240611165942268](https://telegraph-image-177.pages.dev/file/331bc4c040f3d90f584a0.png)
4. **访问部署页面**：

   * 访问生成的域名![image-20240611172822691](https://telegraph-image-177.pages.dev/file/08c8e4a004932b65d87cf.png)
   * 进行图片上传，正常情况下系统会返回**图片链接**。![image-20240611172616544](https://telegraph-image-177.pages.dev/file/0bfc14ccfe425244f65f3.png)
   * 访问图片链接，若能成功加载，则表明图床功能正常˶╹ꇴ╹˶！![image-20240611172731934](https://telegraph-image-177.pages.dev/file/b662414033d7a8bb520c0.png)

   > 仅仅是上传和访问图片还不能满足我们的需求，如何管理我们的图床呢？
5. **后台管理配置**：

   * 在Cloudflare Pages的项目设置中，绑定KV命名空间并创建新的命名空间`img_url`。![image-20240611170742037](https://telegraph-image-177.pages.dev/file/354b6d5244e7e45bd848a.png)
   * 设置**环境变量**，添加管理用户和密码。

     注意**变量名称需完全相同**，且为**大写英文字母**。

     | 变量名称       | 值              |
     | ---------- | -------------- |
     | BASIC_USER | <后台管理页面登录用户名称> |
     | BASIC_PASS | <后台管理页面登录用户密码> |

     ![image-20240611173439257](https://telegraph-image-177.pages.dev/file/980838385788ed55a1328.png)
   * 重新部署程序![image-20240611173836428](https://telegraph-image-177.pages.dev/file/7edfd214b3e90411f12b8.png)
   * 在图床网址后面加上`/admin`, 访问图床后台。若弹出登录窗口说明`BASIC_USER`和`BASIC_PASS`配置成功，登录即可看到自己的图床🎉![image-20240611172208503](https://telegraph-image-177.pages.dev/file/f4dbfaced0948e947df28.png)
6. **图片管理**：

   * 在后台可查看**上传的图片数量**，进行**复制地址、白名单、黑名单和删除**操作。
   * 如果遇到上传图片后在后台不可见的情况，需**通过URL访问图片一次**以解决。

### 注意事项

1. **每日限制**：

   * 使用Cloudflare的免费资源，**每日读取次数**有限制（**100,000次**），删除操作有限制（1,000次）。
2. **图片大小限制**：

   * Telegraph限制上传的图片大小最大为**5MB**。
3. **更新程序**：

   * 当项目作者更新代码后，通过Sync fork->Update branch进行同步更新，Cloudflare会自动重新部署。
4. **其他**：

   * 遇到问题请查看 [Telegraph-Image官方文档-Github](https://github.com/cf-pages/Telegraph-Image?tab=readme-ov-file#telegraph-image)

## 在Typora使用图床

为了在Typora中方便地使用我们搭建的图床，可以将PicGo与Typora结合使用，实现自动上传图片至Telegraph，并生成图片链接。

### 配置PicGo

1. 下载并安装[PicGo](https://github.com/Molunerfinn/PicGo/releases)。
2. 打开PicGo，选择“插件设置”，搜索并安装插件`telegraph-image-uploader`

   ![image-20240611175020211](https://telegraph-image-177.pages.dev/file/b6e3a530eee45e13d22b9.png)
3. 点击“图床设置”，选择刚刚下载的插件`telegraph-image`，将URL设为你的**图床网址**（如"https:// xxx.pages.dev"，无需后缀），点击确定并**设为默认图床**![image-20240611175254230](https://telegraph-image-177.pages.dev/file/5e438b7514ae3bc66cc8b.png)

### 配置Typora

1. 打开Typora，选择`文件->偏好设置->图像`
2. 勾选“**上传图片**”，选择“**PicGo(app)**”，并填入PicGo的**安装路径**（即以PicGo.exe的文件路径），完成后重启Typora![image-20240611180050842](https://telegraph-image-177.pages.dev/file/d3cf27b70cd2ba61ae389.png)
3. 现在，您在Typora中插入图片时，PicGo会自动将图片上传到Telegraph，并返回图片链接🎉

> 参考文章：
>
> 1. [无需服务器，无限空间，完全免费的图床搭建 （图文教程）-腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/2312437)
> 2. [Telegraph-Image官方文档-Github](https://github.com/cf-pages/Telegraph-Image?tab=readme-ov-file#telegraph-image)

<!--EndFragment-->