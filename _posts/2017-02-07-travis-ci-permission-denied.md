---
title: Travis CI运行构建脚本时报Permission Error的解决方法
---

把这个博客集成到Travis CI的时候出现了这样的错误：

```
/home/travis/build.sh: line 59: script/bootstrap: Permission denied
The command "script/bootstrap" failed and exited with 126 during .
```

因为这个博客的Jekyll主题是从[别的repo](https://github.com/pages-themes/cayman)上clone下来的，原来的构建脚本并没有动过，而且看了下原repo的travis状态是没问题的，觉得有点奇怪。遂去StackOverflow找到了[这个答案](https://stackoverflow.com/questions/33820638/travis-yml-gradlew-permission-denied)

结论是权限问题，没有加可执行位。

首先查看当前的文件权限：

```
$ git ls-tree HEAD
100644 blob 492e5535f187441911852349a3659e4ce90cf2cf    bootstrap
100644 blob f62753003679cb2929314a2a08178fb9c96f87c7    cibuild
100644 blob fb400aab392d9e1b0364a391b9dbdd623846862d    release
100644 blob d8c3e15694b8d80bbca0dbc2ba11a70dd951861b    server

```

可以看到是644，没有执行权限。

```
$git update-index --chmod=+x bootstrap
(还有3个文件省略)
$git commit -m "Fix permission"
[master c04bba0] Fix permission
 4 files changed, 0 insertions(+), 0 deletions(-)
 mode change 100644 => 100755 script/bootstrap
 mode change 100644 => 100755 script/cibuild
 mode change 100644 => 100755 script/release
 mode change 100644 => 100755 script/server

```

最后push到Github上，问题解决。

(｢･ω･)｢<sup>[![Build Status](https://travis-ci.org/sdygt/sdygt.github.io.svg?branch=master)](https://travis-ci.org/sdygt/sdygt.github.io)</sup>