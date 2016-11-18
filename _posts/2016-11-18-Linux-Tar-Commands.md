---
layout: post
title: Mac如何用tar进行压缩分包，和分包解压缩
---
#tar 分包压缩与合并  

要将目录logs打包压缩并分割成多个1M的文件，可以用下面的命令：
 
```
tar cjf - logs/ |split -b 1m - logs.tar.bz2.
```

完成后会产生下列文件：

```
 logs.tar.bz2.aa, logs.tar.bz2.ab, logs.tar.bz2.ac
```

要解压的时候只要执行下面的命令就可以了：

```
cat logs.tar.bz2.a* | tar xj
```

另一个例子，使用Gzip压缩：

要将文件test.pdf分包压缩成500 bytes的文件：

```
tar czf - test.pdf | split -b 500 - test.tar.gz
```
最后要提醒但是那两个"-"不要漏了，那是tar的ouput和split的input的参数。

```
gzcat sxrt5.0.dvd1.tar.gza[a-c]|tar xvf -
```

1. 合并使用spilt分割的文件 
   `# cat sxrt5.0.dvd1.tar.gzaa  sxrt5.0.dvd1.tar.gzab sxrt5.0.dvd1.tar.gzac >>sxrt5.0.dvd1.tar.gz`
2. 解压gz文件
   `# gunzip sxrt5.0.dvd1.tar.gz`
3. 解tar包
   `# tar xvf sxrt5.0.dvd1.tar` 

