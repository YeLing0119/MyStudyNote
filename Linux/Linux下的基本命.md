### Linux 下的基本命令 

####  1. ls 命令

​        格式   ：  ls [OPTION]... [FILE]...

​        用途   ：  显示目录下的内容

​        [OPTION] : 

​                -l  ： 列出详细信息

​                -d ： 显示目录本身，而不是目录的内容

​                -a ： 显示所有文件，包括隐藏文件

​                         . 开头的文件称为隐藏文件

####  2. pwd 命令

​        格式  ：  pwd [OPTION]...

​        用途  ：  查看当前路径

####  3.  cd 命令

​        格式  ：  cd [OPTION]...

​        用途  ：  改变当前路径

​        [OPTION]  :

​                不带参数  ：  返回当前用户的家目录

​                ~  ：  返回当前用户的家目录

​                          ~ 表示当前用户的家目录

​                \-  ：  进入上次进入的目录

####  4.   touch 命令

​        格式  ：  touch [OPTION]... FILE...

​        用途  ：  若文件不存在创建文件，若存在更新文件最后一次更改的时间

####  5.   rm 命令

​        格式  ：  rm [OPTION]... FILE...

​        用途  ：  删除文件

​        [OPTION]  ：

​                -r  :  带有提示的递归删除

​                -rf  :  不带提示的递归删除（慎用）

####  6.  mkdir 命令

​        格式  ：  mkdir [OPTION]... DIRECTORY...

​        用途  ：  创建目录

​        [OPTION]  ：

​                -p  :  若父目录不存在，则创建

####  7.   cat 命令

​        格式  ：  cat [OPTION]... [FILE]...

​        用途  ：  查看文件全部内容

####  8.   more 命令

​        格式  ：  more [OPTION]... [FILE]...

​        用途  ：  分屏看文件

​        [OPTION]  ：

​                空格  ：  翻到下一屏

​                b  :  向上翻一屏

####  9.   less 命令

​        格式  ：  less [OPTION]... [FILE]...

​        用途  ：  分屏看文件，还可以查找

​        [OPTION]  ：

​                空格  ：  翻到下一屏

​                b  ：  向上翻一屏

​                /*  ：  查找字符串*

​                q  ：  退出

####  10.   head 命令

​        格式  ：  head [OPTION]... [FILE]...

​        用途  ：  缺省显示文件前10行

​        [OPTION]  ：

​                -n num  ：  显示文件的前num行

####  11.   tail 命令

​        格式  ：  tail [OPTION]... [FILE]...

​        用途  ：  缺省显示文件的尾10行

​        [OPTION]  ：

​                -n num  ：  显示文件的尾num行

​                -f  :  追加输出

####  12.   echo 命令

​        格式  ：  echo [SHORT-OPTION]... [STRING]...

​                       echo LONG-OPTION

​        用途  ：  在屏幕上显示内容

####  13.  seq 命令

​        格式  ：  seq [OPTION]... LAST

​                       seq [OPTION]... FIRST LAST

​                       seq [OPTION]... FIRST INCREMENT LAST

​        用途  ：  生成一系列的数字

####  14.  cut 命令

​        格式  ：  cut [OPTION]... [FILE]...

​        用途  ：  分列显示

​        [OPTION]  ：  

​                -d:  :  告诉程序分列符为冒号

​                -f  :  取那些列

​                               ，离散的列   

​                               start - end 连续的列

####  15.   sort 命令

​        格式  ：  sort [OPTION]... [FILE]...

​                       sort [OPTION]... --files0-from=F

​        用途  ：  排序文本内容，缺省按ASCII码排序

​        [OPTION]  ：

​                -n  :  按数值来排序

####  16.   uniq 命令

​        格式  ：  uniq [OPTION]... [INPUT [OUTPUT]]

​        用途  ：  去除相邻重复行

####  17.   wc 命令

​        格式  ：  wc [OPTION]... [FILE]...

​        用途  ：  统计文件的行数、 单词数、字符数

​        [OPTION]  ：

​                -l  ： 行数

​                -w  ： 单词数

​                -c  ： 字符数

####  18.  date 命令

​        格式  ：  date [OPTION]... [+FORMAT]

​        用途  ：  显示当前时间

​        [OPTION]  ：

​                -s  ：  设置当前时间，只有root权限才可以设置

​                -d  ：  把时间戳转换成时间

​        [+FORMAT]  ：

​                \+ 进行格式化输出

​                %Y  年份  %m  月份  %d  天数

​                %F  等于  %Y-%d-%m

​                %H 小时  %M  分钟  %S  秒

​                %T  等于 %M：%H：%S

​                +"%F  %T"  有空格要加引号

​                %s  把当前时间转换成时间戳

####  19.   find 命令

​        格式  ：  find [-H][-L] [-P][-D debugopts] [-Olevel][path...] [expression]

​        用途  ：  缺省在当前文件目录寻找文件

​        [OPTION]  ：

​                -name  :  按照文件名进行查找

​                               \* 代表通配符，0到多个

​                -*time  day :  按时间进行查找

​                -size  ：  按文件大小查找

####  20.   whereis 命令

​        格式  ：  whereis [options][-BMS directory... -f] name...

​        用途  ：  查找命令的存放目录

####  21.   grep 命令

​        格式  ：  grep [OPTIONS] PATTERN [FILE...]

​        用途  ：  在文件中查找内容

​        [OPTIONS]  ：

​                -n  ：  显示行号

####  22.   tar 命令

​        格式  ：  tar [OPTION...][FILE]...

​        用法  ：  压缩 解压 

​        [OPTION...]  ：

​                -cvf  :  打包

​                -xvf  :  解包

​                -j  ：  压缩成bzip2

​                -z  :  压缩成gzip

​                -C  ：  指定解包的位置



#### linux 关机流程

shutdown   --> 广播在多少时间后关机

init 0   -->  关闭所有服务 

halt  -->  关机