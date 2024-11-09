---
title: Cold Turkey Blocker Pro 4.4 白嫖教程
date: 2024-11-09 15:49:03
comments: true
---

### 1. 介绍

[Cold Turkey Blocker](https://getcoldturkey.com)，是一款帮助用户管理时间的电脑软件，通过屏蔽或限制访问特定网站来帮助你专注于工作或学习。Pro 版本提供了更多强大的功能，比如设置定时屏蔽、允许时间段访问以及番茄钟模式的循环屏蔽等，非常适合想提升自制力的小伙伴。

### 2. 安装软件

**Cold Turkey Blocker Pro 4.4 版本安装包**：[百度网盘链接](https://pan.baidu.com/s/1JwnUSYIDCQxEAwVZYtPj3A?pwd=ptc6)  提取码：ptc6

> **注意：** 本方法仅适用于 Cold Turkey Blocker 4.4/4.3 版本，4.5 及更高版本已证实无效，请勿升级。安装时选择**默认安装路径 **`C:\Program Files\Cold Turkey`。

### 3. 解锁 Pro 版

#### 3.1 安装 Python 环境

在继续操作前，你需要安装 [Python](https://www.python.org/downloads/) 。

选择默认安装选项，将 Python 安装在默认路径，并确保勾选**“将 Python 添加到系统 PATH 环境变量”**。

安装完成后，可以通过在命令行中输入 `python --version` ，出现类似下图的信息即为安装成功。

![image-20241109153956149](C:/Users/DJCHAN/AppData/Roaming/Typora/typora-user-images/image-20241109153956149.png)

#### 3.2 创建激活脚本

下载并安装 Cold Turkey Blocker 后，按照以下步骤来解锁 Pro 版功能：

1. **创建激活脚本**

   - 在桌面新建一个文本文件，复制以下代码粘贴进去：

   ```python
   import json
   import sqlite3
   import os
   
   DB_PATH = "C:/Program Files/Cold Turkey/data-app.db"  # 更改为你的安装目录，默认无需修改
   
   def activate():
       try:
           conn = sqlite3.connect(DB_PATH)
           c = conn.cursor()
           s = c.execute("SELECT value FROM settings WHERE key = 'settings'").fetchone()[0]
           dat = json.loads(s)
           if dat["additional"]["proStatus"] != "pro":
               print("Your version of Cold Turkey Blocker is not activated.")
               dat["additional"]["proStatus"] = "pro"
               print("But now it is activated.\nPlease close Cold Turkey Blocker and run it again.")
               c.execute("""UPDATE settings SET value = ? WHERE "key" = 'settings'""", (json.dumps(dat),))
               conn.commit()
           else:
               print("Looks like your copy of Cold Turkey Blocker is already activated.")
       except sqlite3.Error as e:
           print("Failed to activate", e)
       finally:
           if conn:
               conn.close()
   
   def main():
       if os.path.exists(DB_PATH):
           print("Data file found.\nLet's activate your copy of Cold Turkey Blocker.")
           activate()
       else:
           print("Looks like Cold Turkey Blocker is not installed.\n If it is installed then run it at least once.")
   
   if __name__ == '__main__':
       main()
   ```

2. **保存文件**

   - 将该文件保存，并修改文件名为 `ColdTurkeyBlockerActivator.py`，务必将扩展名改为 `.py`。

3. **运行脚本**

   - 打开命令行，进入保存脚本的目录，运行以下命令：

     ```
     python ColdTurkeyBlockerActivator.py
     ```

   - 如果一切顺利，你会看到提示 "**But now it is activated.**"，这说明已经成功激活了 Pro 版本。

### 4. 其他

- **DB_PATH 路径**：无需查找 `C:\ProgramData` 目录，因为它是默认隐藏的，请直接使用提供的路径。
- **参考博客：**[Cold Turkey Blocker Pro 4.4版本免费使用 - 丘丘王 - 博客园](https://www.cnblogs.com/czy-blogs/p/18053394)，[Cold Turkey Blocker Pro 免费使用简易教程(最新版4.3) - 知乎](https://zhuanlan.zhihu.com/p/605890408)