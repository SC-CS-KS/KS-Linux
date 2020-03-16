Tabularize?- align everything?

这个插件的作用是用于按等号、冒号、表格等来对齐文本，参考下面这个初始化变量的例子：
int var1 = 10;
 float var2 = 10.0;
 char *var_ptr = "hello";

运行Tabularize /=可得：
 int var1      = 10;
 float var2    = 10.0;
 char *var_ptr = "hello";

另一个常见的用法是格式化文件头：
 file: main.cpp 
 author: feihu 
 date: 2013-12-17
 description: this is the introduction to vim 
 license:
 TODO:

运行Tabularize /:/r0可得：
 file        : main.cpp 
 author      : feihu 
 date        : 2013-12-17
 description : this is the introduction to vim 
 license     :
 TODO        :

另一种对齐方式，运行Tabularize /:/r1c1l0：
        file : main.cpp 
      author : feihu 
        date : 2013-12-17
 description : this is the introduction to vim 
     license :
        TODO :

对于写代码的人来说，还是非常有用的。
因为没有找到对应的图，所以这里就用另外一个插件的动画来代替了，Tabular的功能比它更为强大。
--help: 通常会绑定这样一些快捷键：
 nmap <Leader>a& :Tabularize /&<CR>
 vmap <Leader>a& :Tabularize /&<CR>
 nmap <Leader>a= :Tabularize /=<CR>
 vmap <Leader>a= :Tabularize /=<CR>
 nmap <Leader>a: :Tabularize /:<CR>
 vmap <Leader>a: :Tabularize /:<CR>
 nmap <Leader>a:: :Tabularize /:\zs<CR>
 vmap <Leader>a:: :Tabularize /:\zs<CR>
 nmap <Leader>a, :Tabularize /,<CR>
 vmap <Leader>a, :Tabularize /,<CR>
 nmap <Leader>a,, :Tabularize /,\zs<CR>
 vmap <Leader>a,, :Tabularize /,\zs<CR>
 nmap <Leader>a<Bar> :Tabularize /<Bar><CR>
 vmap <Leader>a<Bar> :Tabularize /<Bar><CR>