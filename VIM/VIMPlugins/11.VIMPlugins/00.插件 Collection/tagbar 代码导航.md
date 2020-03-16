tagbar 代码导航

tagbar 代码导航
tagbar是一个taglist的替代品，比taglist更适合c++使用，函数能够按类区分，支持按类折叠显示等，显示结果清晰简洁

安装
   vim tagbar.vba 
    :so % 
    :q 

Quickstart
Put something like the following into your ~/.vimrc: 
nmap <F8> :TagbarToggle<CR>

.vimrc
配置文件中关于tagbar的配置，让tagbar可以在加载代码时自动打开
""""""""""""""""""""""""""""""""""""""""""""""""
" tagbar
""""""""""""""""""""""""""""""""""""""""""""""""
nmap <Leader>tb :TagbarToggle<CR>
let g:tagbar_ctags_bin='/usr/bin/ctags'
let g:tagbar_width=30
autocmd BufReadPost *.php,*.cpp,*.c,*.h,*.hpp,*.cc,*.cxx call tagbar#autoopen()