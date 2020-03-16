YouCompleteMe?- visual assist for vim?

YouCompleteMe - visual assist for vim 

这是迄今为止，我认为VIM历史上最好的插件，没有之一。
为什么这么说？因为作为一个程序员，这个功能必不可少，而它是迄今为止完成的最好的。
从名字可以推断出，它的作用是代码补全。不管是在Source Insight，还是安装了Visual Assist的Visual Studio中，
代码补全功能可以极大的提高生产力，增加编码的乐趣。
大学第一次遇到Visual Assist时带给我的震撼至今记忆犹新，那感觉就似百兽之王有了翅膀，如虎添翼，
从此只要安装有Visual Studio的地方我第一时间就会安装Visual Assist。

而作为编辑器的VIM，一直以来都没有一个能够达到Visual Assist哪怕一成功力的插件，不管是自带的补全，
omnicppcomplete，neocompletecache，完全和Visual Assist不在一个数量级上。
Visual Assist借助于Visual Studio，它的补全是语义层面的，它完全能够理解程序语言，而VIM的这些插件仅仅是基于文本匹配，
虽然最近的neocompletecache已经好了很多，但准确率非常低。
所以在写代码时，即使VIM用得再顺手，绝大部分情况下我还是倾向于Visual Studio + Visual Assist。

但是YouCompleteMe的出现彻底的改变了这一现状，它对代码的补全完全终于也达到了编译器级别，绝不弱于Visual Assist，
遇到它是我使用VIM之后最兴奋的一件事。为什么一个编辑器的插件可以做到如此的神奇，原因就在于它基于LLVM/clang，
一个Apple公司为了代替GNU/GCC而支持的编译器，正因为YouCompleteMe有了编译器的支持，
而不再像以往的插件一样基于文本来进行匹配，所以准确率才如此之高。
其次，由于它是C/S架构，会在本机创建一个服务器端，利用clang来解析代码，然后将结果返回给客户端，
所以也就解决了VIM是单线程而造成的各种补全插件速度奇慢的诟病，在使用时，几乎感觉不到任何的延时，体验达到了Visual Assist的级别。

YouCompleteMe也是所有的插件当中安装最为复杂的一个，这是因为需要用clang来编译相应的库。
因为clang在Linux和Mac平台上支持的非常好，所以在这两个平台上安装相对简单。
但是clang并没有官方支持Windows，所以YouCompleteMe插件也没有官方支持Windows。
可这么好的东西，活跃在Windows上聪明的Vimer们怎么可能容忍这种事情呢，有人就提供了Windows Installation Guide，
已经编译好了各种版本的YouCompleteMe插件，可以参考这个Guide来安装。
我并没有采用它，而是参考了这里，自己编译了YouCompleteMe，其实也不难，一步一步按照介绍的步骤，相信你也可以。

YouCompleteMe除了补全以外，还有一个非常重要的作用：
代码跳转，同样可以达到编译器级别的准确度，媲美Visual Assist与Source Insight。

有了YouCompleteMe之后，是时候抛弃昂贵的Visual Assist与Source Insight了。赶快安装尝试吧:-)
--help: 
只要设置好项目的.ycm_extra_conf.py，自动补全功能就可以完美的使用了。
通常一个全局的.ycm_extra_conf.py足矣。

代码跳转可以绑定一个快捷键：
nnoremap <leader>jd :YcmCompleter GoToDefinitionElseDeclaration<CR>，
很好理解，先跳到定义，如果没找到，则跳到声明处。
