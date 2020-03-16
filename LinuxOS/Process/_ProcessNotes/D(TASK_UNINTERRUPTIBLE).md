绝大多数情况下，进程处在睡眠状态时，总是应该能够响应异步信号的。否则你将惊奇的发现，kill -9竟然杀不死一个正在睡眠的进程了（TASK_INTERRUPTIBLE状态）！

TASK_UNINTERRUPTIBLE状态存在的意义就在于，内核的某些处理流程是不能被打断的。如果响应异步信号，程序的执行流程中就会被插入一段用于处理异步信号的流程（这个插入的流程可能只存在于内核态，也可能延伸到用户态），于是原有的流程就被中断了（参见《linux异步信号handle浅析》）。

在进程对某些硬件进行操作时（比如进程调用read系统调用对某个设备文件进行读操作，而read系统调用最终执行到对应设备驱动的代码，并与对应的物理设备进行交互），可能需要使用TASK_UNINTERRUPTIBLE状态对进程进行保护，以避免进程与设备交互的过程被打断，造成设备陷入不可控的状态。（比如read系统调用触发了一次磁盘到用户空间的内存的DMA，如果DMA进行过程中，进程由于响应信号而退出了，那么DMA正在访问的内存可能就要被释放了。）这种情况下的TASK_UNINTERRUPTIBLE状态总是非常短暂的，通过ps命令基本上不可能捕捉到。

linux系统中也存在容易捕捉的TASK_UNINTERRUPTIBLE状态。执行vfork系统调用后，父进程将进入TASK_UNINTERRUPTIBLE状态，直到子进程调用exit或exec（参见《神奇的vfork》）。

通过下面的代码就能得到处于TASK_UNINTERRUPTIBLE状态的进程：
#include
void main() {
if (!vfork()) sleep(100);
}

编译运行，然后ps一下：
kouu@kouu-one:~/test$ ps -ax | grep a.out
4371 pts/0      D+       0:00 ./a.out
4372 pts/0      S+       0:00 ./a.out
4374 pts/1      S+       0:00 grep a.out

然后我们可以试验一下TASK_UNINTERRUPTIBLE状态的威力。不管kill还是kill -9，这个TASK_UNINTERRUPTIBLE状态的父进程依然屹立不倒。