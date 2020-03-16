Sessionman?- session manager

Sessionman - session manager
这是VIM的Session Manager，作用很简单，管理VIM的会话，可以让你在重新打开VIM之后立刻进行之前的编辑状态，
就像Windows的休眠一样，相信它一定是你工作的好伴侣。

--help: 我的配置如下：
 set sessionoptions=blank,buffers,curdir,folds,tabpages,winsize 
 nmap <leader>sl :SessionList<CR>
 nmap <leader>ss :SessionSave<CR>
 nmap <leader>sc :SessionClose<CR>
