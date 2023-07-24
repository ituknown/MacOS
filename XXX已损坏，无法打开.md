安装新软件或更新系统之后运行 App 如果提示类似如下的错误基本上都是因为 macOS 启用了新的安全机制的问题。

- xxx已损坏，无法打开，你应该将它移到废纸篓解决办法
- 打不开xxx，因为它来自身份不明的开发者
- 打不开xxx，因为 Apple 无法检查其是否包含恶意软件
- 在安装的时候提示加载失败！

**苹果默认是只允许安装自家【App Store】来源的应用，如果你想安装第三方的应用，那么需要在【系统偏 好设置 -> 安全性与隐私 -> 通用】中勾选【App Store 和被认可的开发者】选项。**

**而被认可的开发者是需要购买苹果的企业证书对应用进行签名，然后再提交给苹果审核才可以，这对破解应用来说很不现实，因为破解应用必定会修改应用的文件从而导致签名失效而运行显示【已损坏】。**

**解决方法就是去开启【任何来源】选项了，但是 macOS 默认是隐藏了这个设置的，需要用户手动通过终端执行命令行代码来开启。**

**解决方式主要有如下几种： **

# 开启任何来源方式

**先打开 系统偏好设置 -> 安全与隐私 -> 通用 选项卡，检查是否已经启用了 任何来源 选项。**

![image.png](https://cdn.nlark.com/yuque/0/2021/png/667060/1629348318213-9e2d4b7f-d640-45ac-a170-ebd3fedb2431.png#averageHue=%23343333&clientId=u335ad51e-35f5-4&from=paste&height=679&id=u897a2f51&originHeight=1358&originWidth=1336&originalType=binary&ratio=1&rotation=0&showTitle=false&size=396443&status=done&style=none&taskId=ue99a05a1-712d-492c-ad55-94325e3386b&title=&width=668)

![image.png](https://cdn.nlark.com/yuque/0/2021/png/667060/1629348356871-3ab99d04-f82c-47cb-9294-617d2cde4abe.png#averageHue=%23302f30&clientId=u335ad51e-35f5-4&from=paste&height=587&id=u2246b4c1&originHeight=1174&originWidth=1336&originalType=binary&ratio=1&rotation=0&showTitle=false&size=417076&status=done&style=none&taskId=u2a1ea075-83c2-4943-a0bc-dd2227411a5&title=&width=668)

**如果没有 【任何来源/Anywhere】 这个选项，复制下面的命令在终端中运行就就会出现这个选项了：**

```bash
sudo spctl --master-disable
```

# 绕过公证方式

**打开终端，输入以下命令：**

```bash
sudo xattr -rd com.apple.quarantine /Applications/xxxxxx.app
```

**将上面的 xxxxxx.app 换成你的App名称，比如 Sketch.app**

```bash
sudo xattr -rd com.apple.quarantine /Applications/Sketch.app
```

**或者复制以下命令粘贴到终端后**

```bash
sudo xattr -rd com.apple.quarantine 
```

**然后打开Finder（访达），点击左侧的 应用程序，将应用拖进终端中，然后按键盘的回车键（return），输入密码，再按回车键，完成。**

_**注意**** ****quarantine**** ****后面必须有个空格**_

![image.png](https://cdn.nlark.com/yuque/0/2021/png/667060/1629348857160-186e8d5c-a1b8-462e-a341-b2db8b0d4f9b.png#averageHue=%23201e1d&clientId=u335ad51e-35f5-4&from=paste&height=293&id=u42232e07&originHeight=585&originWidth=1400&originalType=binary&ratio=1&rotation=0&showTitle=false&size=446880&status=done&style=none&taskId=u731d2359-a1b2-44b4-accd-8b226c8bcb2&title=&width=700)

**好了再看一下是不是可以打开APP了！到这里一般情况下 90% 的应用都可以安装运行了。**

**如果还不行，那就需要对应用进行本地签名操作了！**



# 应用签名方式

1. **安装Command Line Tools 工具**

**打开终端工具输入如下命令：**

```bash
xcode-select --install
```

![image.png](https://cdn.nlark.com/yuque/0/2021/png/667060/1629349052896-142487c8-c2a8-412f-b4d7-cf6c58bb62c0.png#clientId=u335ad51e-35f5-4&from=paste&height=263&id=uc66b8a88&originHeight=526&originWidth=1230&originalType=binary&ratio=1&rotation=0&showTitle=false&size=52190&status=done&style=none&taskId=u9096c7a8-4978-497d-a4bb-e7a26b9af11&title=&width=615)

2. **弹出安装窗口后选择 继续安装，安装过程需要几分钟，请耐心等待。如果安装的时候提示“不能安装该软件，因为当前无法从软件更新服务器获得”，请按这篇教程操作：**[macOS “不能安装该软件，因为当前无法从软件更新服务器获得” 解决方法](https://www.macwk.com/article/macos-command-line-tools-cannot-be-installed)。

3. **打开终端工具输入并执行如下命令对应用签名：**

```bash
sudo codesign --force --deep --sign - (应用路径)
```

_**应用路径：打开访达（Finder），点击左侧导航栏的 **_**应用程序**_**，找到相关应用，将它拖进终端命令**_**-**_**的后面，然后按下回车即可，注意最后一个-后面有一个空格。**_

![image.png](https://cdn.nlark.com/yuque/0/2021/png/667060/1629349313341-c0685c27-afee-4a69-8f86-e16e9636b9b9.png#clientId=u335ad51e-35f5-4&from=paste&height=268&id=uff81b52b&originHeight=535&originWidth=1000&originalType=binary&ratio=1&rotation=0&showTitle=false&size=314649&status=done&style=none&taskId=u79db7424-4932-4333-b1de-7e683a4c5fe&title=&width=500)

![image.png](https://cdn.nlark.com/yuque/0/2021/png/667060/1629349322536-ab9b60d0-b266-40ac-829e-edd91e5dbb0c.png#clientId=u335ad51e-35f5-4&from=paste&height=295&id=u99625826&originHeight=589&originWidth=1440&originalType=binary&ratio=1&rotation=0&showTitle=false&size=624528&status=done&style=none&taskId=ub9c69ff3-a88e-4d11-b9e0-d5f0c8602cd&title=&width=720)

![image.png](https://cdn.nlark.com/yuque/0/2021/png/667060/1629349335556-28753f5b-3572-48ca-9d15-8d2b8c4e178e.png#clientId=u335ad51e-35f5-4&from=paste&height=284&id=u70933779&originHeight=567&originWidth=1000&originalType=binary&ratio=1&rotation=0&showTitle=false&size=244432&status=done&style=none&taskId=uf3788f1f-5bcc-4e07-94e9-edbdce1161f&title=&width=500)

![image.png](https://cdn.nlark.com/yuque/0/2021/png/667060/1629349359543-16e8dcdd-af25-4f65-8713-d5b0e8fa3a30.png#clientId=u335ad51e-35f5-4&from=paste&height=279&id=u69eb6b40&originHeight=557&originWidth=1000&originalType=binary&ratio=1&rotation=0&showTitle=false&size=302987&status=done&style=none&taskId=u83ca2869-eace-4506-8c98-6645c73d2a5&title=&width=500)

**正常情况下只有一行提示，即成功：**

```
/文件位置 : replacing existing signature
```

![image.png](https://cdn.nlark.com/yuque/0/2021/png/667060/1629349398821-59c702be-0cbd-4ab2-9907-f738ccd92f0e.png#clientId=u335ad51e-35f5-4&from=paste&height=285&id=u8fa6735f&originHeight=570&originWidth=1000&originalType=binary&ratio=1&rotation=0&showTitle=false&size=288516&status=done&style=none&taskId=uf19a0f60-55b2-4f93-9b08-f72eb6c09f4&title=&width=500)

## 遇到错误？

```
/文件位置 : replacing existing signature
/文件位置 : resource fork,Finder information,or similar detritus not allowed
```

1. **先在终端执行：**

```
xattr -cr /文件位置（直接将应用拖进去即可）
```

2. **然后再次执行如下指令即可：**

```
codesign --force --deep --sign - /文件位置（直接将应用拖进去即可）
```

**到这儿，百分之九十五的应用都可以正常运行了。**

--

**来源：** [MacWK  - xxx.app 已损坏，无法打开，你应该将它移到废纸篓/打不开 xxx，因为它来自身份不明的开发者解决方法](https://www.macwk.com/article/macos-file-damage#绕过公证（扩展）)
