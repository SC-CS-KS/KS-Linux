# SUSE zypper  


01.通过系统镜像来安装软件包以下以SUSE系统为例：  

1.1.加载系统镜像文件  

镜像文件为： SLES-11-SP1-DVD-x86_64.iso
# mkdir -p /mnt/cdrom
# mount -o loop -t ios9660 /home/dcache/SLES-11-SP1-DVD-x86_64.iso /mnt/cdrom/

1.2.设置/mnt/cdrom 为zypper的软件源  

# zypper ar /mnt/cdrom/  localmirr
Adding repository 'localmirr' [done]
Repository 'localmirr' successfully added
Enabled: Yes
Autorefresh: No
URI: dir:///mnt/cdrom

1.3.完成软件包安装  

# zypper install  gcc



zypper安装配置及使用方法(for SUSE)
zypper 是 OpenSUSE 命令行下管理软件的程序，功能十分强大。
------------------------------------------------------------------------------------------
添加软件源
zyppr ar URL alias
URL 就是软件源的地址
alias 就是你取另外一个名字
------------------------------------------------------------------------------------------
查找软件包
#zypper search  gcc*
Loading repository data...
Reading installed packages...

S | Name           | Summary                                       | Type      
--+----------------+-----------------------------------------------+-----------
i | gcc            | The system GNU C Compiler                     | package   
  | gcc            | The system GNU C Compiler                     | srcpackage
  | gcc-32bit      | The system GNU C Compiler                     | package   
  | gcc-c++        | The system GNU C++ Compiler                   | package   
  | gcc-info       | The system GNU Compiler documentation         | package   
  | gcc-locale     | The system GNU Compiler locale files          | package   
  | gcc33          | The GNU C Compiler Version 3.3(evaluation)    | srcpackage
i | gcc43          | The GNU C Compiler and Support Files          | package   
  | gcc43          | The GNU C Compiler and Support Files          | srcpackage
  | gcc43-32bit    | The GNU C Compiler and Support Files          | package   
  | gcc43-c++      | The GNU C++ Compiler                          | package   
  | gcc43-info     | Documentation for the GNU compiler collection | package   
  | gcc43-locale   | Locale Data for the GNU Compiler Collection   | package   
i | libgcc43       | C compiler runtime library                    | package   
i | libgcc43-32bit | C compiler runtime library                    | package 
------------------------------------------------------------------------------------------
刷新软件源
zypper refresh
------------------------------------------------------------------------------------------
升级软件
zypper update
------------------------------------------------------------------------------------------
安装软件
zypper install 软件名
------------------------------------------------------------------------------------------
下面是完整的介绍：
zypper [--全局选项] <命令> [--命令选项] [参数]

全局选项：
--help, -h 帮助。.
--version, -V 输出版本号。
--quiet, -q 减少普通输出，仅打印错误信息。
--verbose, -v 增加信息的详细程度
--no-abbrev, -A 表格中不出现缩写文本。
--table-style, -s 表格样式 (整数)。
--rug-compatible, -r 开启与 rug 的兼容。
--non-interactive, -n 不询问任何问题，自动使用默认的回复。
--xmlout, -x 切换到 XML 输出。
--reposd-dir, -D <dir> 使用其他的安装源定义文件目录。
--cache-dir, -C <dir> 使用其他的元数据缓存数据库目录。
--raw-cache-dir <dir> 使用其他的原始元数据缓存目录。

源选项:
--no-gpg-checks 忽略 GPG 检查失败并继续。
--plus-repo, -p <URI> 使用额外的安装源。
--disable-repositories 不从安装源读取元数据。
--no-refresh 不刷新安装源。
目标选项：
--root, -R <dir> 在不同的根目录下操作。
--disable-system-sources、-D 不读取系统安装的可解析项。
命令：
help, ? 打印帮助。
shell, sh 一次接受多个命令.

安装源操作：
repos, lr 列出所有定义的安装源。
addrepo, ar 添加一个新的安装源。
removerepo, rr 删除指定的安装源。
renamerepo, nr 重命名指定的安装源。
modifyrepo, mr 修改指定的安装源。
refresh, ref 刷新所有安装源。
clean 清除本地缓存。
------------------------------------------------------------------------------------------
软件管理：
install, in 安装软件包。
remove, rm 删除软件包。
verify, ve 检验软件包的依赖关系的完整性。
update, up 将已经安装的软件包更新到新的版本。
dist-upgrade, dup 执行整个系统的升级。
source-install, si 安装源代码软件包和它们的编译依赖。
------------------------------------------------------------------------------------------
查询：
search, se 查找符合一个模式的软件包。
info, if 显示指定软件包的完整信息。
patch-info 显示指定补丁的完整信息。
pattern-info 显示指定模式的完整信息。
product-info 显示指定产品的完整信息。
patch-check, pchk 检查补丁。
list-updates, lu 列出可用的更新。
patches, pch 列出所有可用的补丁。
packages, pa 列出所有可用的软件包。
patterns, pt 列出所有可用的模式。
products, pd 列出所有可用的产品。
what-provides, wp 列出能够提供指定功能的软件包。
------------------------------------------------------------------------------------------
软件包锁定：
addlock, al 添加一个软件包锁定。
removelock, rl 取消一个软件包锁定。
locks, ll 列出当前的软件包锁定。
------------------------------------------------------------------------------------------
其他：
versioncmp, vcmp 比较两个版本
targetos, tos 显示目标操作系统标识字符串
licenses  显示有关许可证、eulas的安装程序包
------------------------------------------------------------------------------------------
例：添加ISO本地源
iso路径是：/opt/SLES-11-SP1-DVD-x86_64-GM-DVD.iso
首先查询系统安装源列表，
执行命令一：zypper lr
结果如下：kingserver11sp1:~ # zypper lr
# | Alias                            | Name               | Enabled | Refresh
--+------------------------------------+-----------------------+---------+--------
1 | SUSE-Linux-Enterprise-Server  | SLES                | Yes     | No
根据查询得到的结果，1为系统默认的远程官方源；
删除远程源，执行命令二为：
zypper rs 1
 
执行命令三：zypper ref，刷新安装源
将iso添加到安装源，
执行命令四：zypper addrepo iso:/?iso=/opt/SLES-11-SP1-DVD-x86_64-GM-DVD.iso DVDISO，配置本地安装源
 
执行命令五：zypper ref，再次刷新安装源列表
 
执行命令六：zypper lr，查看安装源列表
server1:/opt # zypper lr
# | Alias   | Name    | Enabled | Refresh
--+---------+----------+----------+--------
1 | DVDISO | DVDISO | Yes     | No
------------------------------------------------------------------------------------------
