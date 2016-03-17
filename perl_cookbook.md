
## 提取子资源

### 简便提取js和css单行命令

提取js

```perl
perl -nle 'print $1 if /script src="(.*?)"/'
```

提取css

```perl
perl -nle 'print $1 if /link rel="stylesheet".*?href="(.*)"/'
```

存在问题：只是提取相对路径

## 获取并分解单条Hummer日志

```perl
perl -F\` -alne 'foreach $l(@F){@t=(split /=/, $l, 2);$req{$t[0]}=$t[1];} #code here#'
```

## Hummer日志日期范围定位

```perl
perl -lne '$d1=`date -d "2016/01/31 03:41:33" +%s`; $d2=`date -d "2016/02/02 13:40:29" +%s`; if(/t=(.*？)\| /){$dd = `date -d "$1" +%s`; if($dd > $d1 and $dd < $d2){print $_}; };'
```

## 获取本机tcp连接状态

```bash
netstat -n |perl -anle '$S{$F[5]}+=1 if /^tcp/; END{while( ($k,$v) = each %S){print $k." ".$v}}'
```

## 小tips

perl -a 可以直接分割空格，而且连续空格当做一个空格，不像cut是死板地每个空格一个位置
在perl里 ``$a =~ /#regex#/``  里面填上括号分组，即可用$1, $2来获得匹配内容


## 参考

[sed]: http://www.sed.com  '《SED 单行脚本快速参考》的 perl 实现 - marlonyao - ITeye技术网站 '

Linux date命令用法和使用技巧（获取今天、昨天、一分钟前等）

[google]: http://www.baidu.com


![回文截图](http://i12.tietuku.cn/3843ad1ca8dd5a62.png)
