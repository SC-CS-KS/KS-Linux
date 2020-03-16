UltiSnips?- ultimate snippets?

UltiSnips - ultimate snippets 

这是什么？
相信大家经常在写代码时需要在文件开头加一个版权声明之类的注释，又或者在头文件中要需要：
#ifndef... #def... #endif这样的宏，亦或者写一个for、switch等很固定的代码片段，这是一个非常机械的重复过程，但又十分频繁。
我十分厌倦这种重复，为什么不能有一种快速输入这种代码片段的方法呢？
于是，各种snippets插件出现了，而它们之中，UltiSnips是最好的一个。
比如上面的一长串#ifndef... #def... #endif，你只需要输入ifn<TAB>，怎么样，方便吧。
更为重要的一点是它支持扩展，你可以随心所欲的编辑你自己的snippets。

现在它可以和上面介绍的YouCompleteMe插件一块使用，比如在敲完ifn时，YouCompleteMe会将这个snippet也放在下拉框中让你选择，
这样你就不用去记何时按<TAB>来展开snippets，YouCompleteMe已经帮你完成。

去它的网站看看，有几个视频，绝对亮瞎你的双眼(需要翻墙)。
--help: 
它和YouCompleteMe一块使用时会有一定的冲突，因为两者都默认绑定了<TAB>键，
可以参考各自的help文档，将其中一个绑定到其它的快捷键，或者借助其它的插件让它们兼容。
