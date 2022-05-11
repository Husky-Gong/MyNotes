# 搭建笔记本系统 | Typora + Git + GitHub

使用过印象笔记和OneNote，但是功能都太臃肿，不适合自己。之前一直都听说Typora，加上自己对Markdown语法有了解，因此搭建了这套笔记系统。这篇文章将讲述如何结合Typora和GitHub来管理自己的笔记。系统如下：

![流程图](https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

- Typora：编辑器，上传的图片存储到云库中（阿里云OSS）
- 阿里云OSS：图片云库
- Git：同步文档到GitHub，云端存储

## 阿里云OSS

1. 登录阿里云

3. 打开侧边栏，选择对象存储OSS

4. 对象存储界面右部选择创建Bucket

5. 之后会弹出一个配置的侧边栏

![createBucket](https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/createBucket.png)

6. 因为一开始没有可用的“存储包”，因此点击购买链接

这里可以根据自己的实际情况来设定购买存储包的方案，如果在中国大陆，建议地域选择中国大陆通用。购买完成后，继续进行创建 Bucket工作，读写权限要设定为**公共读写**，其他的服务选项，如果自己有特殊需要，可以选择配置同城冗余存储，版本控制等服务，如果没有可以默认。

7. 完成后点击确定即可

> 注意：购买完存储包就可以了，至于流量包，默认的是按量计费，[阿里云的计费方法](https://help.aliyun.com/document_detail/59636.html?spm=5176.11451019.101.3.5b302135Qd1ak4)。如果是自己简单的写个博客或公众号，平时可能没有特别大的访问量，所以选择默认的按量计费，如果访问量较大的，可以考虑包年包月。

8. 添加用户

> **推荐**最好是单独创建一个用户来单独操作OSS（用户的定义就是需要访问云资源的人员或应用程序），这样给这个用户赋予单独的访问OSS的权限，更加安全方便。添加用户步骤为：点击自己头像，然后选择访问控制，如下图所示：

![addUser](https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/addUser.png)

点击用户，选择新建用户，如下图所示

![CreateUser](https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/CreateUser.png)

之后进行用户配置，注意要勾选编程访问，控制台密码登录可以不选择，登录名称，显示名称自己设定：

![UserConfig](https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/UserConfig.png)

确定后得到创建用户的信息，这里要记下**AccessKeyID** 和 **AccessKeySecret**，之后配置PICGO用到，因为这个界面关掉之后就不好找了，所以最好记在记事本里。设置用户权限，使得用户只能访问OSS的服务。点击添加权限，如下图所示：

![SetAccess](https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/SetAccess.png)

选择管理对象存储服务权限，点击确定，如下图所示：


![OSSAccess](https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/OSSAccess.png)

## PicGo

1. 下载：[PICGO下载](https://github.com/Molunerfinn/PicGo/releases) 里面有相应操作系统的安装包文件以及源码，点击下载安装文件即可。

2. 安装完成后，打开图床设置，点击阿里云OSS，得到如下界面

![PicGoOss](https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/PicGoOss.png)

3. 配置：

   - keyid：创建用户的AccessKey ID

   - KeySecret： AccessKeySecret，

   - 存储空间名：创建Bucket的名字
   - 存储路径：img/即可
   - 如果自己有已经备案的域名，可以填写设定自定义域名，如果没有不填即可。
   - 存储区域：创建时设定的，忘记的可以通过Bucket概览查看，如下图所示：

![zoneInfo](https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/zoneInfo.png)

4. 点击确定即完成，也可以设置为默认图床。

### 上传图片

可以看到PICGO能够文件上传，也支持剪贴板上传。上传过程为：

1. 拖拽文件或点击上传文件或点击剪贴板图片上传。
2. 上传完成后电脑剪贴板里就有了所选链接格式的图片链接。
3. 到相应的地方粘贴即可。
4. 开始可能有疑问，我的图片存到哪里了呢？很简单，点击文件管理，如下图所示：

![OSSImage](https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/OSSImage.png)

看到img文件加了吗？就在里面了，你可以在文件夹里对图片进行删除等操作。

## Typora

下载：[Typora下载链接](https://typora.io/)

设置偏好：将上传的图片地址设置为阿里云OSS

![Typora云图库配置](https://zexi-typora.oss-cn-beijing.aliyuncs.com/picgo/image-20220510211034393.png)

## 下载 Git

我们想要将本地写的文章或者笔记上传到 GitHub，手动逐个上传文件还是比较麻烦，也不够优雅，这里我们使用一个程序员群体经常会用到的工具——**Git**，来帮我们快速地上传文件。

Git 支持 Windows、macOS 和 Linux 系统，下载之后，像平常安装软件那样安装 Git 即可。

![Image](https://mmbiz.qpic.cn/mmbiz_png/WdqAMJxDC6oyYQeVDlurt7gSCejy3QqVPic8fArzrqj6EzNc4SopmGHuXKQczSWqonWUxCPNliahWicBMn1cNkYyw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

下载：[Git 官网下载](https://git-scm.com/downloads)

## 配置公钥

将我们电脑本地与远端的 GitHub 仓库连接起来，还需要用到**传输协议**，常用的传输协议如下图，这里我们选择使用 **ssh 协议**。

![Image](https://mmbiz.qpic.cn/mmbiz_png/WdqAMJxDC6oyYQeVDlurt7gSCejy3QqVuAnkaKxd4oy3M37icK5NMcEkNPxicjV0faXHOF2UTTiaLNk6bf4XiaABqQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

为了简化生成公钥的流程，这里我们可以使用另外一个工具——**Quicker**，Quicker 是一个 Windows 上的多功能工具箱，有点像是苹果生态下的「快捷指令」，通过使用别人写好的**快捷指令**或**动作**，可以让我们免去操作中间的繁琐流程。

安装好 Quicker 之后，我们需要给 Quicker 添加「**获得 ssh 公钥**」的动作，添加动作的方法，可以查看这篇文章：[**这款相见恨晚的效率神器，让我卸载了80%的软件**](https://mp.weixin.qq.com/s?__biz=MzAxMjY0NTY5OA==&mid=2649881749&idx=1&sn=52709287a4eb8c2b8c624755adb8edbb&scene=21#wechat_redirect) 

![Image](https://mmbiz.qpic.cn/mmbiz_png/WdqAMJxDC6oyYQeVDlurt7gSCejy3QqVR4rb0GCrIs6MO1zOhEKqu2TzfwpibYN9R2X3XLoakwsKN7cBmpNXD7A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

Quicker 动作「获得 ssh 公钥」安装地址：

*https://sourl.cn/vMbu46* 

添加动作之后，按下鼠标中键，呼出 Quicker 面板，点击运行「获得 ssh 公钥」。

![Image](https://mmbiz.qpic.cn/mmbiz_png/WdqAMJxDC6oyYQeVDlurt7gSCejy3QqVxOibjH9ibHicF3Yox4zAQsADWB38BOMnwibryXkd2fmV1pFLrLgicGynuoA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

运行后会打开一个 PowerShell 的窗口，窗口提示会生成一对公钥和私钥，按下 Enter 回车键，确认保存公钥和私钥的位置，后面还会提示让我们输入密码，简单起见，就不设置密码了，直接按下回车键即可。

![Image](https://mmbiz.qpic.cn/mmbiz_png/WdqAMJxDC6oyYQeVDlurt7gSCejy3QqVxZBDRljaRicKQLUmdMlVaicONb0GS4BwEGKSbAeZFwv9mpdsG0Jib10JQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

最后它会在窗口中显示生成的公钥，如下图所示的一大坨以 ssh-rsa 开头的内容，用鼠标选中**复制**这一大坨内容。

![Image](https://mmbiz.qpic.cn/mmbiz_png/WdqAMJxDC6oyYQeVDlurt7gSCejy3QqVQRhs1aobDetNV8LOdCW5LroFh241loRrPgvt5vmcg4S7u4Pohzamhw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

## **GitHub  配置公钥 & 创建仓库**

得到公钥之后，打开 GitHub 官网，点击右上角的个人头像，选择 Settings 打开设置页面。

![Image](https://mmbiz.qpic.cn/mmbiz_png/WdqAMJxDC6oyYQeVDlurt7gSCejy3QqVGr49bxZgaQlQHJFfcw8KZ8nX2BnKFXNrGSHkzSzPgXzILiaUvia3nOGg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

左侧栏切换到 **SSH and GPG keys** 选项卡，点击右上角的 **New SSH key**。

![Image](https://mmbiz.qpic.cn/mmbiz_png/WdqAMJxDC6oyYQeVDlurt7gSCejy3QqVSTSibDviciatZBAnhootIB2XYmN34198O07yZhWm7IPxW3BHcvL94VfPQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

在打开的 SSH 配置页面，**在 Key 里面粘贴从 PowerShell 中复制的公钥**，而上面的 Title 可以随意写。到这里，我们就完成了本地和 GitHub 仓库传输文件的配置。

![Image](https://mmbiz.qpic.cn/mmbiz_png/WdqAMJxDC6oyYQeVDlurt7gSCejy3QqVbk6UPDRo4ERfP2mOw1wv3UPMnSWlR1y0R4LRboW14t0YMZogXXZf2Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

接着我们在 GitHub 中**创建一个新的仓库**，用来存放我们等下要上传到 GitHub 的文章。以我自己为例，我创建一个名为「**Typora_articles**」的仓库，对于刚创建的新仓库，一般会有类似下图的引导。

点击下方的 **SSH** 按钮，获得仓库的 SSH 传输链接，再点击最右侧的像是「记事本」的按钮，将 SSH 链接复制到系统剪切板。

![Image](https://mmbiz.qpic.cn/mmbiz_png/WdqAMJxDC6oyYQeVDlurt7gSCejy3QqV0VHunMfTH98tYLia139mCuZzXFTL2mt5cE6B8a8FxO5z8beowHsomuw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

## **本地配置**

我们在本地写东西使用的工具是 Typora，它不像微软的 VS Code 代码编辑器那么强大——集成了 Git，可以通过图形化操作的方式，很方便地将本地文件上传到 GitHub。

为了简化 Typora 往 GitHub 上传文件的操作，我们要再次用到 Quicker，需要给 Quicker 安装另外一个动作「**Typora+github 云笔记初始化/远程同步**」。

![Image](https://mmbiz.qpic.cn/mmbiz_png/WdqAMJxDC6oyYQeVDlurt7gSCejy3QqVVNL4s3X1geD6U1COPyEy8mERvNQhEVVcgyqufLA97xgcV7ria9pQh0Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

Quicker 动作「Typora+github 云笔记初始化/远程同步」安装地址：

*https://sourl.cn/tLeFKN*

添加动作之后，按下鼠标中键呼出 Quicker 面板，将鼠标移动到刚安装的动作上方，按住 **Ctrl** 键，再单击鼠标左键，会弹出下图右侧的窗口，**粘贴前面创建的 GitHub 仓库的 SSH 链接**。

![Image](https://mmbiz.qpic.cn/mmbiz_png/WdqAMJxDC6oyYQeVDlurt7gSCejy3QqVuAC07pZqLlvQbFlhY5TEJRARS93QKZialRHM1sRBzKcIlIhoVfxessw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

粘贴 SSH 链接之后，会弹出下图的窗口，从本地选择要上传到 GitHub 的文件夹。

因为我把在 Typora 中写的文章都存放在本地的 `E:\OneDrive\Typora文章`路径下，因此选的是整个「Typora 文章」文件夹。

![Image](https://mmbiz.qpic.cn/mmbiz_png/WdqAMJxDC6oyYQeVDlurt7gSCejy3QqVylvxk0v7nPDnIUoWUYxEuqeX3rowKq5lZIib9gAyY4429qjp5aRnuqw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

到这里，我们就算是完成了所有写文章之前的前置操作，接下来就可以愉快地码字了。

## **将 Typora 写的文章上传到 GitHub**

就譬如你当前正在看的这篇文章，我就是在 Typora 中写的，我可以在写的过程中边写边上传到 GitHub，也可以等整篇文章写完再一并上传。

上传的方法也非常简单，按下鼠标中键呼出 Quicker 面板，点击运行前面安装的动作「Typora+github 云笔记初始化/远程同步」。

运行动作之后，会弹出一个 PowerShell 窗口，当本地的文件和图片都上传到 GitHub 上，它会最末尾的地方提示「**笔记本远程同步成功**」。

![Image](https://mmbiz.qpic.cn/mmbiz_png/WdqAMJxDC6oyYQeVDlurt7gSCejy3QqVwY6tGvIahjBsPEUFmfLT5b28DoawMwDk6dTyA3BDa0ZREaNPdB6MaQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

此时打开在浏览器中打开 GitHub 仓库，就可以看到我们刚刚上传的文章和图片啦。

![Image](https://mmbiz.qpic.cn/mmbiz_png/WdqAMJxDC6oyYQeVDlurt7gSCejy3QqV88GvXcydprZfj3gfNbOA9XnQgIH2oThyfibZuaalVQza6J9hP29O7MA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

只要有网络，你也能随时随地查看自己写过的东西，除了网页端，GitHub 也推出了**移动端**，不过稍微有点遗憾的是，使用 iPhone 的朋友，需要去到非国区的 App Store 才能下到 GitHub App。