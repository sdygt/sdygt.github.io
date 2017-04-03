---
title: Linux下使用find寻找不符合条件的文件
---

通常来说，使用find是用来寻找符合条件的文件的，例如(递归)寻找当前目录下后缀名为mp3的文件：

```
find . -name *.mp3 -type f
```

不过有的时候我们需要查找不符合指定条件的文件，例如现在我需要找到所有后缀名不是mp3的文件。（实际情况是我需要提取夹杂在我音乐文件夹中的专辑封面图片）

在这种情况下，只要在条件表达式前面加上`!`就行了。

先来看看`man find`怎么说：


**!** <u>expr</u> True if <u>expr</u> is false.  This character will also usually need protection from interpretation by the shell.<br>

**-not** <u>expr</u><br>
       Same as ! <u>expr</u>, but not POSIX compliant.<br>

例如：

```
find . ! -name *.mp3 ! -name *.flac  ! -name *.MP3  ! -name Thumbs.db -type f
```

我查到的有些资料里说这个`!`感叹号最好要转义一下使用，不过好像我没转义也没什么问题……

除此以外，还可以加上`-exec`执行一些组合技能：

```
find . ! -name *.mp3 ! -name *.flac  ! -name *.MP3  ! -name Thumbs.db -type f -exec cp --parents '{}' ../cover  \;
```

把当前目录下满足（显而易见的）这些条件的**文件** (`-type f`) ，复制到`../cover`文件夹下，同时保持原有目录结构 (`--parents`) 。这样就达到了抽取所有指定文件的目的。

顺便一提，这么做的根本原因是因为iTunes把我精心整理的音乐文件夹的目录结构翻了个底朝天……所以只能自己重新整理了。

而且拜Ubuntu on Windows的福，在Windows上也可以愉快地使用原来Linux下的工具了。岂不美哉！