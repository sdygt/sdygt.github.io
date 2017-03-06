---
title: 用PowerShell计算文件哈希值
---

事情的起源是这样的，在学校宿舍想在同学电脑上装JDK，于是熟练地摸到Oracle那边点了下载，然而……

![](/images/2017-03-06/1.png)

……反正不是学校的缓存服务器就是电脑上某个代理把这个请求给截了。

![](/images/2017-03-06/2.jpg)

那么接下来的问题就是，为了验证安装包有没有被篡改，我们需要计算文件的哈希值。在Linux上我们可以使用`md5sum`、`sha1sum`、`sha256sum`一类的命令进行哈希值的计算。不过以往在Windows下面，通常要安装一些小工具才能达到目的。

不过好消息是现在我们有PowerShell了╮(￣▽￣)╭

```
Get-FileHash filename
```

默认情况下会使用SHA256算法，也可以手动指定其他算法

```
Get-FileHash -A sha1 filename
```

默认结果是按列显示的（见下方），也可以通过管道符传给`Format-List`改成按行显示

```
PS D:\Downloads> Get-FileHash .\python-3.6.0-amd64.exe

Algorithm       Hash                                                                   Path
---------       ----                                                                   ----
SHA256          F35D6FBA8C62FC7A03CEAE8B1B6EDC77249BCE8D30D0A16D140AFB2EE8F44194       D:\Downloads\python-3.6.0-amd64.exe


PS D:\Downloads> Get-FileHash -A md5 .\python-3.6.0-amd64.exe

Algorithm       Hash                                                                   Path
---------       ----                                                                   ----
MD5             71C9D30C1110ABF7F80A428970AB8EC2                                       D:\Downloads\python-3.6.0-amd64.exe


PS D:\Downloads> Get-FileHash -A sha1 .\python-3.6.0-amd64.exe

Algorithm       Hash                                                                   Path
---------       ----                                                                   ----
SHA1            2E0C3FE6DC63CC869435055DAA86CB4AAE68D4F4                               D:\Downloads\python-3.6.0-amd64.exe


PS D:\Downloads> Get-FileHash  .\python-3.6.0-amd64.exe | Format-List


Algorithm : SHA256
Hash      : F35D6FBA8C62FC7A03CEAE8B1B6EDC77249BCE8D30D0A16D140AFB2EE8F44194
Path      : D:\Downloads\python-3.6.0-amd64.exe

```

应该来说用起来还是比较轻松愉快的ww