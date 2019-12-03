# 缓冲区

```md
应用缓冲区机制提高了磁盘的I/O效率；
优化了磁盘的写操作；需要及时的将缓冲数据写到磁盘。
根据应用程序对文件的访问方式，即是否存在缓冲区，对文件的访问可以分为带缓冲区的操作和非缓冲区的文件操作。
```
```md
非缓冲的文件操作访问方式，每次对文件进行一次读写操作时，都需要使用读写系统调用来处理此操作，
即需要执行一次系统调用，执行一次系统调用将涉及到CPU状态的切换，即从用户空间切换到内核空间，实现进程上下文的切换，
这将损耗一定的CPU时间，频繁的磁盘访问对程序的执行效率造成很大的影响。
```
```md
ANSI 标准C库函数 是建立在底层的系统调用之上，即 C函数库文件访问函数的实现中使用了低级文件 I/O系统调用，
ANSI 标准C库中的文件处理函数为了减少使用系统调用的次数，提高效率，采用缓冲机制，
这样，可以在磁盘文件进行操作时，可以一次从文件中读出大量的数据到缓冲区中，
以后对这部分的访问就不需要再使用系统调用了，即需要少量的CPU状态切换，提高了效率。
```
* IO缓冲区

* 内核缓冲区


## 缓冲类型
* 全缓冲区
```md
要求填满整个缓冲区后才进行I/O系统调用操作。
对于磁盘文件的操作通常使用全缓冲的方式访问。
第一次执行I/O操作时，ANSI标准的文件管理函数通过调用 malloc 函数获得需要使用的缓冲区，默认大小为8192。
```
```c
//come from /usr/include/stdio.h
        /* Default buffer size. */
        #ifndef BUFSIZ
        # define BUFSIZ _IO_BUFSIZ        //BUFSIZ 全局宏定义
        #endif
        //come from /usr/include/libio.h
        #define _IO_BUFSIZ _G_BUFSIZ
        //come from /usr/include/_g_config.h
        #define _G_BUFSIZ 8192        //真实大小
```
* 行缓冲区
```md
在这种情况下，当在输入和输出中遇到换行符时，标准I/O库函数将会执行系统调用操作。
当所操作的流涉及一个终端时（例如标准输入和标准输出），使用行缓冲方式。
因为标准I/O库每行的缓冲区长度是固定的，所以只要填满了缓冲区，
即使还没有遇到换行符，也会执行I/O系统调用操作，默认行缓冲区的大小为1024。
```
* 无缓冲区
```md
无缓冲区是指标准I/O库不对字符进行缓存，直接调用系统调用。
标准出错流stderr通常是不带缓冲区的，这使得出错信息能够尽快地显示出来。
```
```md
注：
①标准输入和标准输出设备：当且仅当不涉及交互作用设备时，标准输入流和标准输出流才是全缓冲的。
②标准错误输出设备：标准出错绝不会是全缓冲方式的。
```
* 更改缓冲区类型
```md
对于任何一个给定的流，可以调用setbuf()和setvbuf()函数更改其缓冲区类型。
```
## 测试
```c++
import java.io.*; 
public class Test2 {
	public static void main(String args[]) {
		FileWriter fw=null;
		try {
			fw=new FileWriter("test.txt");
			for(int i=0;i<8193;i++)		//写入超过8192个字节，即超过8k
			{
				fw.write('a');
			}
		}
		catch(Exception e) {
			e.printStackTrace();
		}
	}
}
```
```md
文件已经被写入了8k大小的内容。超出缓冲区大小，刷磁盘。
```
```c++

import java.io.*; 
public class Test2
{
	public static void main(String args[])
	{
		FileWriter fw=null;
		try
		{
			fw=new FileWriter("test.txt");
			for(int i=0;i<8192;i++)   //总共写入8192个字节，即8k
			{
				fw.write('a');
			}
		}
		catch(Exception e)
		{
                        e.printStackTrace();
		}
	}
}
```
```md
文件大小为0KB，证明没有写入内容。
默认的缓冲大小为8k，当没有超过这个值时，里面的内容就不会自动写入文件当中；而当超过这个值时，里面的内容就会自动写入文件当中。
```
