文件包含数据，而目录是文件的列表。不同的目录相互连接构成树状的结构。

ls -R  列出指定目录及其子目录的所有内容
chmod -R 修改文件的许可权限位
du   disk usage 给出指定目录及其子目录下所有文件占用硬盘中数据块的总数
find  在一个目录及其所有子目录中检索符合要求的文件和目录

文件系统可以存储文件内容，文件属性（文件所有者，日期等），目录。这些不同类型的数据是如何存储在被编号的磁盘块上的？
Unix将磁盘块分成了3部分：
1. 数据区      
    存放文件内容
2. i-节点表 (inode table)  
    存放文件属性。所有的i-节点都有相同的大小，并且i-节点表是这些结构的一个列表。
    文件系统中的每个文件在该表中都有一个i-节点。如果你有root权限，就可以像操作文件一样
    将分区打开，阅读并显示i-节点表。
3. 超级块 (superblock)
    文件系统中的第一个块被称为超级块。用来存放文件系统本身的结构信息。

目录是一种包含了i-节点号和文件名列表的特殊文件
ls -1ia

在open一个没有读或写权限的文件时将发生什么情况 ？
    内核首先根据文件名找到i-节点号，然后根据i-节点号找到i-节点。在i-节点中，内核找到文件的权限位和拥有者的用户ID。
    如果权限位设置你的用户ID对文件没有访问权限，则open返回-1并且将全局变量errno的值设为EPERM。

在文件系统内部，目录是一个包含文件名与i-节点对的列表的文件。


相同的i-节点号可能以不通的名字在若干个目录中出现。每个入口被称为指向文件的硬链接。符号链接是通过文件名引用文件，而不是i-节点号。
ln 创建硬链接（hard links）是将目录连接到树的指针，硬链接同时也是将文件名和文件本身链接起来的指针。
ln -s 创建符号链接 通过名字引用文件，而不是i-节点号。



47536022 -rw-r--r--  2 jiawei  staff   160B Nov 25 09:17 ulist
47536085 lrwxr-xr-x  1 jiawei  staff     6B Nov 25 09:18 users -> whoson
47536022 -rw-r--r--  2 jiawei  staff   160B Nov 25 09:17 whoson


wc -l whoson users 统计行数
diff whoson users  比较内容