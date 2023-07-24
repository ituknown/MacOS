# 前言

从 El Capitan（OS X 10.11）引入 System Integrity Protection（SIP）开始，mac 就已经开始逐步加强对系统文件的写限制，到 Catalina（macOS 10.15）时完全限制了在根目录下进行写操作。

从Catalina开始，官方提供了 /etc/synthetic.conf 配置文件以支持在根目录下创建软链。具体通过 `man synthetic.conf`命令查看文档：

```bash
NAME
     synthetic.conf – synthetic symbolic link and directory manifest

DESCRIPTION
     synthetic.conf describes virtual symbolic links and empty directories to
     be created at the root mount point.  Because the root mount point is
     read-only as of macOS 10.15, physical files may not be created at this
     location.  All writeable paths must reside on the data volume, which is
     mounted at /System/Volumes/Data.

     synthetic.conf provides a mechanism for some limited, user-controlled
     file-creation at /.  The synthetic entities described in this file are
     synthesized by the kernel during early system boot.  They are not
     physically present on the disk, but when the system is booted, they
     behave as if they were within certain parameters.

     synthetic.conf is intended to be used for creating mount points at /
     (e.g. for use as NFS mount points in enterprise deployments) and symbolic
     links (e.g. for creating a package manager root without modifying the
     system volume).  synthetic.conf is read by apfs.util(8) during early
     system boot.

FILES
     /etc/synthetic.conf
```



# synthetic.conf 配置示例

使用任何命令编辑 /etc/synthetic.conf 文件（以 vim 为例，若文件不存在会自动创建）：

```bash
sudo vim /etc/synthetic.conf
```

之后输入如下示例内容进行创建软连接：

```bash
data	Users/$me/data
data1	Users/$me/data1
```

**注意：**

**1、每行的两项配置不是以/开头（可以理解系统会帮我们加入前缀 `/`）。**

**2、data 与 Users/username/log 之间一定要使用tab进行分隔，否则重启后无效。如果指定目录（如 Users/$me/data）不存在记得创建。**

**3、示例中的 `$me` 是用户名的意思，别忘记替换为自己用户名，如 foo。**

**4、记得重启Mac。**

重启之后，使用 `ls -l /`命令就会发现在根目录下就有了两个软连接目录：

```bash
$ ls -l /

...
lrwxr-xr-x   1 root  wheel    46  7 24 13:34 data -> Users/ituknown/data
lrwxr-xr-x   1 root  wheel    46  7 24 13:34 data1 -> Users/ituknown/data1
```



# synthetic.conf 配置不生效？

如果发现 /data 目录没有被创建，那么一定要仔细检查下你的 /etc/synthetic.conf 文件里的 Tab 分隔符是否被正确配置了。

**有的机器的 vim 配置了 `set expandtab`，导致Tab被自动转换成了多个空格。**

这个时候可以在编辑模式下，先按 CTRL+V 再按 tab 键，就可以输入Tab了。可以用xxd查看，Tab是ASCII码是09，而空格的是20。

以下面的配置为例：

```bash
$ cat synthetic.conf
data    Users/ituknown/Downloads/data
```

vim 配置了 **set expandtab**，导致 TAB 键被替换为空格：

```bash
$ xxd /etc/synthetic.conf
00000000: 6461 7461 2020 2020 5573 6572 732f 6974  data    Users/it
00000010: 756b 6e6f 776e 2f44 6f77 6e6c 6f61 6473  uknown/Downloads
00000020: 2f64 6174 610a                           /data.
```

注意看第一行的 `2020 2020`，表示 TAB 被替换为 4 个空格！！！！

现在重新使用 vim 编辑配置文件：

```bash
sudo vim /etc/synthetic.conf
```

进入编辑模式之后将原空格删除，然后按下 CTRL+V，然后在按 TAB 就好了。通常情况下，当按下 CTRL+V 组合键之后会出现一个 ^ 符号（即使不出现也没关系）：

```bash
data^sers/ituknown/Downloads/data
```

保存之后再使用 xxd 命令查看下：

```bash
$ xxd /etc/synthetic.conf
00000000: 6461 7461 0955 7365 7273 2f69 7475 6b6e  data.Users/itukn
00000010: 6f77 6e2f 446f 776e 6c6f 6164 732f 6461  own/Downloads/da
00000020: 7461 0a                                  ta.
```

注意看第一行出现了 `0955`，其中 `09` 表示的就是 TAB 键！
