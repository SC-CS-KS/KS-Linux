文本三巨头：zsh、tmux 和 vim

罗马三巨头
公元前62年，凯撒 组建了一个包含了他自己， 政治家克拉苏，以及军事领袖庞培三人的政治联盟。   
这三个人一起组成了一个秘密政治小组，称为 Triumvirate（三巨头），来统治罗马共和国。   
而文本三巨头则是 zsh、vim 和 tmux。 

这三个令人尊敬的工具本身已经非常强大，然而它们的组合却更加所向披靡，把其他文本编辑组合甩开了 N 条街。
本文旨在向刚接触各类工具的新手们简述如何建立一个既强大又容易配置的文本三巨头。
我想把主要的篇幅放在如何将 zsh、vim 和 tmux 整合起来，并主要讲述了我如何解决两个常见的问题——复制/粘贴功能和颜色配置。

我的愚见
跟Rands一样，我对工具非常痴狂。我认为文本三巨头是最强大的文本编辑的工具链。如果你不使用这个工具链，那么我会建议你先干了这杯酒，然后尝试使用文本三巨头。如果你每天花费大量的时间在文本中纠缠，那么你更应该接受我的建议。一开始换工具或许会有些不习惯，但是你的努力会得到回报的。使用 zsh、vim 和 tmux 的好处就在于免费使用，速度快，可任意定制，在任何操作系统上都能使用，可在远程环境中使用，还在于可以实现远程结对编程，以及互相之间，和与 Unix 之间深度的整合。最终纯文本编辑的效率和组织性将会得到很大提升。该工具链可以完全由 git 管理，并且可以再几秒钟的时间内克隆到一台远程服务器或是一台新的机器上。总的来说，它们的这些优点让使我在写作和编程上变得更快，更有效率。
文本三巨头的一个巨大的优势在于对用于管理工作环境的分屏模型的普遍使用。分屏模型管理允许tmux像粘合剂一样组织工作流。通常在一天的结尾，我会发现我留下了一些shell窗口和一大堆的临时文件，数据文件，源代码文件，文档文件，还有打开的数据库。把这些窗口一个个关掉然后第二天再把它们打开是非常痛苦的一件事。tmux和vim支持对一个特定的项目打开大量的窗格和窗口，如果你希望转换到另一个完全不同的项目，你可以从这些窗口分离出来转向另外一个项目，然后再按原样返回这些窗口。在一时间段内，我通常同时在多个工作和个人的项目上进行工作。在多个工作环境中来回切换的能力对我来说非常重要。（Thoughtbot blog 中有对 tmux 中窗口和窗格的使用的讲解）
下面是——包装在tmux中的zsh和vim：

该tmux会话中有三个分别命名为demo、docs和scatch的窗口，然而在截图中只有最上面的窗口是可见的。
在这个窗口中有四个分区。左上角的分区是一个zsh窗口，左下角的分区是一个交互的python会话，
右上角的窗格是用vim打开的python代码，然后右下角是包含markdown文档的窗格。

外观设置
我建议给文本三巨头设置两种颜色主题——一个主题给工作上的项目而另外一个给个人项目。
我是情景依赖记忆的重度使用者，因此使用两个主题在认识和区分工作项目和个人项目上给予我很大的帮助。
如图，下面是我的个人主题（左），以及工作主题（右）。两个主题都是Ethan Schoonover 的solarized 项目中的版本。
我在玩的时候使用暗调主题，是因为我通常在清晨或傍晚天空还处在黑暗中时搞自己的项目。暗
调主题可以在这些时候让我的眼睛得到舒缓。关于字体，我用的是 14 point 的 Inconsolata。


安装
首先要做的事将大写锁定键（Caps Lock）重映射到Control 键上。
大写锁定键是个历史遗留问题，这个在键盘上的黄金位置的键需要被更好的利用。
在tmux中对Control键的使用非常频繁，因此将Control键重映射到一个符合人体工程学的位置对我们很有帮助。

想要给三巨头创建一个强大的工作环境，我们可以下载 iTerm2 终端模拟器。
iTerm2 比普通的终端应用具有更强的性能，更多的特性和更灵活的定制化。当你开始使用iTerm2时，请回头阅读全部文档看看它能为你做什么。
其中一个特性是Command-?，显示出一个视窗帮助你快速地找到你当前的光标位置。大部分iTerm2非常酷炫的功能本文都没有提及。
请确保你了解了iTerm2的即时回放，正则查询，点击打开URL，以及标记跳转的功能。

当iTerm2安装完成，即可添加亮调和暗调主题。solarized 库中含有iTerm2调色板和 配置iTerm2主题的说明，所以它的安装简洁明了。另一项对使用iTerm2有用的配置是启用系统级别的绑定键，通过该键可以让iTerm2转为最前面的窗口。我发觉设置一个具体的绑定比使用应用切换器（Command-Tab）要快的多。该设置在Preferences > Keys中，而我使用的绑定键是Option-t。关于自定义，我还有一个建议，那就是在 Profiles > Terminal > Notifications中撤销选中iTerm2 的响铃声。
由于文本三巨头的操作高度集中在键盘上，因此，在你配置和形成自己的肌肉记忆之前，将iTerm, zsh, vim, tmux,和其他任何你之前使用的工具之间的快捷键冲突消除是非常明智的选择。做窗口移动时，我使用Option 键。Option-t将iTerm2移到屏幕前，而Option-i将Twitter移到屏幕前，等等。我还使用Moom 作为我在OS X上的平铺式窗口管理器，并将所有的快捷键配置为使用Option 键将窗口移至屏幕上特定的展示窗口或位置上。
接下来，安装Homebrew 然后使用它去安装git，MacVim，tmux和reattach-to-user-namespace（返回用户命名空间）。安装MacVim有两个原因。第一，默认的OS X自带的vim似乎对很多人来说很慢。我发现使用MacVim中的vim比OS X版本的vim要快很多。另外一个安装MacVim的好处是你的系统将得到一个更新版本的vim。第二个原因则是复制/粘贴的使用在OS X版本的vim中并没有得到优化。
安装完git，就可以新建一个存储库来放置文本三巨头的设置文件。我的存储库命名为dotfiles 并存储了我的所有zsh, vim, and tmux配置文件。如果你不知道怎么为你的文件设置版本控制，请阅读Pro Git 或者 Git Immersion。

ZSH
已经有很多文章写到了如何使用zsh以及为什么zsh比bash强大。基本上，bash有的功能zsh都有，而且zsh的一些特性bash是没有的。我使用zsh而不是bash是因为它有扩展的globbing（通配符），更好用的tab补全，内建的拼写纠正，一个更好的计算器（zcalc），以及一个内建的批重命名文件工具（zmv）。zsh的另外一个杀手级特色是oh-my-zsh——一个zsh的社区驱动的框架。oh-my-zsh预先打包好了很不错的主题，插件，以及让zsh极度强大的配置。如果你想学习本文的话，请安装iTerm2并将zsh作为你的默认shell。
我将我的 .zshrc、 .vimrc 和 .tmux.conf 配置文件保存在 dotfiles 目录中，并用 symlink 在 home 目录下创建链接。这样我就能只在一个目录里做zsh、vim 和tmux的配置的版本控制了。文本三巨头使用了vim，那么我们应该让zsh和tmux也使用vim以及它的绑定键并将vim设置为默认编辑器。将下面的文本加到.zshrc文件中，让zsh支持vim:
export EDITOR="vim"b
bindkey -v  
 #
# vi style incremental searchb
bindkey '^R' history-incremental-search-backwardb
bindkey '^S' history-incremental-search-forwardb
bindkey '^P' history-search-backwardb
bindkey '^N' history-search-forward
zsh不仅支持大多数bash命令，还支持更多的智能命令。比如，如果你想在bash中移动到一个目录里，你可能会输入cd foo。而在zsh中如果你将下面一行加入到.zshrc中，你只需要输入foo即可。
setopt AUTO_CD
为了设置一个好的命令行提示，我参考了Steve Losh’s excellent prompt然后做了一些小改动。只需要简单地在oh-my-zsh/themes/中创建一个新的主题文件并在你的zshrc文件中添加一行对应你的主题文件的文本（ZSH_THEME=bunsen）。这是我的修改后的Steve的命令行提示：
function virtualenv_info { 
    [ $VIRTUAL_ENV ] && echo '('`basename $VIRTUAL_ENV`') '}
} 
 f
function box_name { 
    [ -f ~/.box-name ] && cat ~/.box-name || hostname -s}
} 
 P
PROMPT='%
%{$fg[magenta]%}%n%{$reset_color%} at %{$fg[yellow]%}$(box_name)%{$reset_color%} in %{$f
fg_bold[green]%}${PWD/#$HOME/~}%{$reset_color%}$(git_prompt_info)$
$(virtualenv_info)%(?,,%{${fg_bold[blue]}%}[%?]%{$reset_color%} )$ ' 
 Z
ZSH_THEME_GIT_PROMPT_PREFIX=" on %{$fg[magenta]%}"Z
ZSH_THEME_GIT_PROMPT_SUFFIX="%{$reset_color%}"Z
ZSH_THEME_GIT_PROMPT_DIRTY="%{$fg[green]%}!"Z
ZSH_THEME_GIT_PROMPT_UNTRACKED="%{$fg[green]%}?"Z
ZSH_THEME_GIT_PROMPT_CLEAN="" 
 l
local return_status="%{$fg[red]%}%(?..⤬)%{$reset_color%}"R
RPROMPT='${return_status}%{$reset_color%}'
VIM
下面我会将注意力放在vim与文本三巨头的整合而不是vim本身。为了将solarized整合到vim中，你需要安装vim solarized plugin然后将下面的内容放到你的vimrc里：
syntax enablel
let g:solarized_termtrans = 1c
colorscheme solarizedt
togglebg#map("<F5>")
终端中的颜色管理会比较复杂。在我的系统中，为了在终端的vim中得到合适的颜色渲染，我特地加了let g:solarized_termtrans = 1。Solarized 提供了内建后台函数，让你可以使用&lt;F5&gt;在亮调和暗调主题之间切换，因此如果你需要这个功能就需要加上上面的内容的最后一行。你还可以在vim里运行:set background=dark 或者 :set background=light去实现同样的功能。
vim对复制/粘贴的处理跟基于GUI的文本编辑器有些不同。vim有许多复制寄存器和一些粘贴模式，而不是一个单一的复制/粘贴机制。我向我的vimrc里添加了下列内容，使复制/粘贴机制更加直观。
" Yank text to the OS X clipboard" 将文本复制到OS X剪贴板中n
noremap <leader>y "*yn
noremap <leader>yy "*Y 
 "
" Preserve indentation while pasting text from the OS X clipboard 在粘贴OS X剪贴板中的文本时保留缩进n
noremap <leader>p :set paste<CR>:put *<CR>:set nopaste<CR>
上面的映射大幅提升了OS X系统剪贴板的可用性。前两个命令分别复制选中的文本或一行内容到系统剪贴板中。最后一行则使粘贴的文本的格式维持不变。在实践中我发现我并不需要进行很多对vim里外文本的粘贴。如果我需要分享代码，我通常会使用vim gist plugin，这比复制/粘贴要快多了。
TMUX
tmux像胶水一样将文本三巨头紧密联系在一起。我在上个月才开始使用tmux，但我惊讶地发现它现在对我的工作流来说是如此的不可或缺。下面是维基百科对tmux的描述：
	tmux是一个用于终端复用的软件，它允许一个用户在一个终端窗口或远程终端会话中使用多个不同的终端会话。在同一个命令行接口处理多个程序，以及将程序从已经开始运行另外的程序的Unix shell中分离出来，是非常有用的。
从本质上来说，tmux允许你创建会话，只要你愿意，你可以随时离开或返回该会话。tmux非常的宝贵，因为你可以根据上下文去安排你的工作。
就像vim一样，设置和使用tmux最难的部分就是颜色管理和用到系统剪贴板的复制/粘贴功能。通过确保tmux知道你使用的256色来创建合适的solarized颜色是非常简单直白的。将下面的内容添加到你的tmux.conf文件中：
set -g default-terminal "screen-256color"
关于复制/粘贴，tmux有一个特别的复制模式。tmux的复制模式命令以一个前缀键开头。默认的前缀键是Control-b。大多数人，包括我自己，都会重映射前缀键为Control-a，因为这样容易使用多了，而且这还是GNU screen的默认绑定键。当你看到我在下面提到prefix，我指的都是Control-a。因此&lt;prefix&gt; c的意思就是：点击Control-a再点击c。
tmux里的复制/粘贴在OS X中完全不起作用。幸运的是，Chris Johnsen创建了一个好用的，很容易通过 Homebrew 安装的补丁，名为 reattach-to-user-namespace（返回到用户命名空间）。Thoughtbot 里的人们有一些很实用的博文解释了如何使用tmux和如何使复制/粘贴功能运行起来（看这和这）。然而读完这些教程，我开始还是没搞懂如何在OS X剪切板中使用tmux，因此下面就是你安装完 reattach-to-user-namespace 后，你需要向你的tmux.conf文件中添加的内容：
set -g default-command "reattach-to-user-namespace -l zsh" 
 s
set -g mode-mouse ons
setw -g mouse-select-window ons
setw -g mouse-select-pane on 
 #
# Copy modes
setw -g mode-keys vib
bind ` copy-modeu
unbind [u
unbind pb
bind p paste-bufferb
bind -t vi-copy v begin-selectionb
bind -t vi-copy y copy-selectionb
bind -t vi-copy Escape cancelb
bind y run "tmux save-buffer - | reattach-to-user-namespace pbcopy"
第一行设置令tmux使用 wrapper 程序给每个新打开的tmux窗口去启动zsh。接下来的三行是我个人对tmux里鼠标操作的设置。你可以保留或删掉这三行，这取决于你自己的需求。真正的干货在接下来的十行，它们用于处理复制模式。
除了vim和OS X的复制/粘贴缓存外，tmux有它自己的复制/粘贴缓存。为了高效地使用tmux缓存，可以点击 ` 键来进入复制模式。我已经将默认的复制绑定重映射为跟vi类似的绑定。为了将文本放入tmux的复制/粘贴缓存中，可以点击v去做出文本的选定然后点击y复制选中项。此时，所选的文本就被放在tmux复制/粘贴缓存中。输入&lt;prefix&gt; p可以粘贴该文本。不过，如果你想将文本放入OS X的复制/粘贴缓存里，你需要输入&lt;prefix&gt; y。
插件
要是我没提及一些非常棒的特别与文本三巨头融合的很好的开源项目，就是我的不对了。
我就不深入地一个一个说这些工具了，下面是一些我最喜欢的项目的链接以及简介：
·	Ack—比grep要好
·	Autojump—目录导航
·	Command-t—用于模糊查询的vim插件；（点击链接了解如何在tmux中设置）
·	Pandoc—格式转换
·	Poweline-vim—定制vim状态栏
·	Pianobar—终端Pandora 音乐播放器
·	pdfgrep&mdash DF文件的grep
·	shelr—shell中的屏幕录制工具
·	vimux—用vim与tmux交互
·	virtualenv&mdash ython虚拟环境创建工具
·	wemux—多用户终端共享器
·	yadr—一套zsh，MacVim，和git 的配置文件

更新
一些朋友问我如何像上面的截屏里一样在tmux中设置漂亮的状态栏。我是从wemux project中学到的。
如果你已经安装了vim-powerline 并且正在使用补充的字体，你只需要向你的tmux.conf中加入下面的内容去得到我的状态栏样式。
感谢Matt Furden！
set -g status-left-length 52
set -g status-right-length 451
set -g status-fg white
set -g status-bg colour234
set -g window-status-activity-attr bold
set -g pane-border-fg colour245
set -g pane-active-border-fg colour39
set -g message-fg colour16
set -g message-bg colour221
set -g message-attr bold
set -g status-left '#[fg=colour235,bg=colour252,bold] ❐ #S
#[fg=colour252,bg=colour238,nobold]⮀#[fg=colour245,bg=colour238,bold] #(whoami)
#[fg=colour238,bg=colour234,nobold]⮀'
set -g window-status-format "#[fg=white,bg=colour234] #I #W "
set -g window-status-current-format
"#[fg=colour234,bg=colour39]⮀#[fg=colour25,bg=colour39,noreverse,bold] #I ⮁ #W
#[fg=colour39,bg=colour234,nobold]⮀
