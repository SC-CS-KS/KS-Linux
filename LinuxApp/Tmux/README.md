# tmux  

## 窗格  

"   将当前面板平分为上下两块
%	将当前面板平分为左右两块
x	关闭当前面板
!	将当前面板置于新窗口；即新建一个窗口，其中仅包含当前面板
Ctrl+方向键	以1个单元格为单位移动边缘以调整当前面板大小
Alt+方向键	以5个单元格为单位移动边缘以调整当前面板大小
Space	在预置的面板布局中循环切换；依次包括even-horizontal、even-vertical、main-horizontal、main-vertical、tiled
q	显示面板编号
o	在当前窗口中选择下一面板
方向键	移动光标以选择面板
{	向前置换当前面板
}	向后置换当前面板
Alt+o	逆时针旋转当前窗口的面板
Ctrl+o	顺时针旋转当前窗口的面板

## 窗口  

窗口是个窗格容器，你可以将多个窗格放置在窗口中，
并根据你的实际需要在窗口中排列多个窗格，也是完全取决于你的需要。
c	创建新窗口
&	关闭当前窗口
数字键	切换至指定窗口
p	切换至上一窗口
n	切换至下一窗口
l	在前后两个窗口间互相切换
w	通过窗口列表切换窗口
,	重命名当前窗口；这样便于识别
.	修改当前窗口编号；相当于窗口重新排序
f	在所有窗口中查找指定文本

若要切换窗口，只需要先按下Ctrl-b，然后再按下想切换的窗口所对应的数字，该数字会紧挨着窗口的名字显示。

## 会话  

一个 Tmux 会话中可以包含多个窗口。会话功能非常简单易用，
例如可以为一个特定的项目创建一个专用的 Tmux 会话。若要创建一个新的会话，只需要在终端运行如下的命令：
tmux new -s <name-of-my-session>
或按下 Ctrl-b : ，然后输入如下的命令：
new -s <name-of-my-new-session>

若要获取现有会话的列表，可以按下Ctrl-b s
列表中的每个会话都有一个 ID，该 ID 是从 0 开始的。按下对应的 ID 就可以进入会话。

## 滚屏
Ctrl + a再按"["，此时进入所谓的copy-mode，可以用上下键或PageDn/PageUp浏览屏幕了。  
想退出copy-mode直接按"q"。
如果需要兼容vim的操作方式，那么在~/.tmux.conf加上一行:  

setw -g mode-keys vi

## Misc  

?	列出所有快捷键；按q返回  
d	脱离当前会话；这样可以暂时返回Shell界面，输入tmux attach能够重新进入之前的会话  
D	选择要脱离的会话；在同时开启了多个会话时使用  
Ctrl+z	挂起当前会话  
r	强制重绘未脱离的会话  
s	选择并切换会话；在同时开启了多个会话时使用  
:	进入命令行模式；此时可以输入支持的命令，例如kill-server可以关闭服务器  
[	进入复制模式；此时的操作与vi/emacs相同，按q/Esc退出  
~	列出提示信息缓存；其中包含了之前tmux返回的各种提示信息  

## 命令  

tmux里的session，window,pane
—-
session指的是按下tmux命令后 存在的连接便是session
创建session
tmux
创建并指定session名字
tmux new -s $session_name
删除session
Ctrl+b :kill-session
临时退出session
Ctrl+b d
列出session
tmux ls
进入已存在的session
tmux a -t $session_name
删除所有session
Ctrl+b :kill-server
删除指定session
tmux kill-session -t $session_name

## Tmux

https://forums.pragprog.com/forums/242  

http://blog.jobbole.com/87584/?utm_source=group.jobbole.com&utm_medium=relatedArticles  
