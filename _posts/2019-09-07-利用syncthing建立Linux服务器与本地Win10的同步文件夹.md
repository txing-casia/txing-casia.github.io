---
layout:     post
title:      "利用syncthing建立Linux服务器与本地Win10的同步文件夹"
subtitle:   ""
date:       2019-09-07
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Windows
    - Linux
---

# 利用syncthing建立Linux服务器与本地Win10的同步文件夹

## Background

由于最近把本地的试验程序搬到了服务器上（[Linux服务器上配置Anaconda3以Pytorch-RL环境](<https://txing.tk/2019/09/06/Linux%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E9%85%8D%E7%BD%AEAnaconda3%E4%BB%A5Pytorch-RL%E7%8E%AF%E5%A2%83/>)），现在的试验结果就自然地在运行后保存在了服务器上（图片、数据）。

由于没有配置服务器端的可视化界面，就没法查看结果了。如果利用pscp进行Linux$$\to$$windows的文件传输，等结果到了本地再查看，这样每跑一次实验就要单独传输一次，势必非常麻烦。

于是这里就迫切需求一种**及时**、**高效**、**自动**的linux$$\to$$windows10的文件传输方案。 

---

经过一番调研，目前有这样几种方案备选：

**1、在linux服务器端查看图片。**这个方案需要使用到类似SimpleHTTPServer的服务，简单地说就是在通过浏览器在ip:8000口查看图片。这个方案仍然需要在运行完成之后单独再操作一步，虽然实现了查看图片的功能，但如果需要保存图片到本地，还是绕不开pscp的操作。

**2、利用VNCserver将服务器端界面图形化。**这是一个非常有吸引力的方案，因为图形化服务器端的意义不仅仅在于查看图片，跟在于今后其它文件操作都可以变得更加具体明确。但初尝试的过程中发现提供VNC服务的第三方软件很多，具体操作和配置看得眼花缭乱，莫衷一是。

**3、把服务器的文件传回本地查看。**既然要在服务器上查看图片这么麻烦，我们何不直接把图片传回本地的windows环境呢，到了windows下那还不是为所欲为啊。这一方案主要同样有很多的备选子方案：

-  **我们的老朋友pscp**：每次使用都要输入一长串目录路径（本地和服务器的），让人身心俱疲。
- **rsync**：支持增量备份的优秀镜像备份工具，网上的具体实施方案比较繁琐，依然莫衷一是。
- **syncthing**：syncthing是个类似rsync的开运啊同步工具，支持windows, linux, Mac等不同的平台看来是个不错的选择。

___

## syncthing: a wise choice

接下来我们选择syncthing，并尝试安装并配置

### windows 10端：

- 直接在[syncthing官网](https://syncthing.net/)可以下载到windows版本的安装包，直接解压点开exe即可完成安装。

### Linux服务器端：

- 第一步：在windows上下载好最新的[安装包](<https://github.com/syncthing/syncthing/releases/tag/v1.2.2>)（我这里选择的是[syncthing-linux-amd64-v1.2.2.tar.gz](https://github.com/syncthing/syncthing/releases/download/v1.2.2/syncthing-linux-amd64-v1.2.2.tar.gz)）

- 第二步：将安装包上传至服务器，具体操作参考[Windows通过SSH向Linux服务器上传文件](<https://txing.tk/2019/09/06/Windows%E9%80%9A%E8%BF%87SSH%E5%90%91Linux%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E4%BC%A0%E6%96%87%E4%BB%B6/>)

- 第三步：解压文件并安装。

  在新建的安装包所在目录中执行解压命令，并打开文件夹

  ```
  tar zxvf syncthing-linux-amd64-v1.2.2.tar.gz
  cd syncthing-linux-amd64-v1.2.2
  ```

  这里通过ls命令可以看到有一个可执行文件，现在先打开它用来生成我们需要更改的配置文件

  ```
  ./syncthing
  ```

  等到看见类似如下字样的时候，表示启动完成，按Ctrl+c关闭

  ```
  [QQAFC] 16:59:18 INFO: Detected 1 NAT service
  ```

- 第四步：配置syncthing。由于syncthing默认是只允许本机访问，这里需要进行设置。大多数教程这里都会使用vim进行编辑，但是我的环境里没有vim，也有有sudo权限安装，因此参考这里的[某教程](<https://www.youcl.com/info/10588>)里的方法，通过nano进行编辑

  ```
  nano ~/.config/syncthing/config.xml
  ```

  这时会弹出一大片文本，但我们只需要找到下面这一段

  ```
  <gui enabled="true" tls="false">
      <address>127.0.0.1:46554</address>
  </gui>
  ```

  再把前4段数字全部变成0，即0.0.0.0:46554（冒号后面的数字不用变）。然后按Ctrl+x退出编辑，在按y保存更改。

- 第五步：再次运行syncthing

  ```
  ./syncthing
  ```

  在浏览器输入

  ```
  http://服务器IP:46554
  ```

  进行图形化的配置

之后的设置比较简单这里给出参考不过多介绍：[Syncthing同步设置教程](<https://blog.curlc.com/archives/333.html>) 

