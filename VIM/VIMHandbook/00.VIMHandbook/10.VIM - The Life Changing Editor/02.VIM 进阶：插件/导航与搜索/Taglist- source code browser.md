Taglist?- source code browser?

Taglist - source code browser 

想必使用过Visual Studio和Source Insight的人都非常喜爱这样一个功能：
左边有一个Symbol窗口，它列出了当前文件中的宏、全局变量、函数、类等信息，鼠标点击时就会跳到相应的源代码所在的位置，非常便捷。
Taglist就是实现这个功能的插件。

可以说symbol窗口是程序员不可缺少的功能，当年有很多人热衷于借助taglist、ctags和cscope，
将VIM打造成一个非常强大的Linux下的IDE，所以一直以来，taglist在VIM官方网站的scripts排列榜中一直高居榜首，成为VIM使用者的必备插件。

--help: 最常见的做法也是将它绑定到一个快捷键上，比如：map <silent> <F9> :TlistToggle<CR>
