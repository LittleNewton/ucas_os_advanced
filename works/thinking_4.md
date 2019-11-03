1、为什么要设计缓冲区，有什么好处？
> 所有的文件交互都在缓冲区完成，当进程准备读取某个块设备中的文件时，系统并不是直接让该进程获得块设备的访问权，而是让其在缓冲区与块设备交互。一来是避免了块设备被独立的进程访问造成安全威胁，另一方面是为了提升效率。若块设备中的某文件被频繁地调用，那么将其放入缓冲区即可重复利用，节省了块设备寻址、取数据等过程花费的时间，大大提高了系统的运行效率。

2、操作系统如何利用 `buffer_head` 中的 `b_data`，`b_blocknr`，`b_dev`，`b_uptodate`，`b_dirt`，`b_count`，`b_lock`，`b_wait`管理缓冲块的？

3、操作系统如何处理多个进程等待同一个正在与硬盘交互的缓冲块？
> 

4、`getblk` 函数中，申请空闲缓冲块的标准就是 `b_count` 为0，而申请到之后，为什么在 `wait_on_buffer(bh)` 后又执行 `if(bh->b_count)` 来判断 `b_count` 是否为0？
> 

5、`b_dirt` 已经被置为1的缓冲块，同步前能够被进程继续读、写？给出代码证据。

6、分析 `panic` 函数的源代码，根据你学过的操作系统知识，完整、准确的判断 `panic` 函数所起的作用。假如操作系统设计为支持内核进程（始终运行在0特权级的进程），你将如何改进 `panic` 函数？
> 

7、详细分析进程调度的全过程。考虑所有可能（`signal`、`alarm`除外）
>

8、`wait_on_buffer` 函数中为什么不用 `if` 而是用 `while`？
>

9、`add_reques()` 函数中有下列代码
``` C 
if (!(tmp = dev->current_request)) {
    dev->current_request = req;
    sti();
    (dev->request_fn)();
    return;
}
```
其中的

``` C
if (!(tmp = dev->current_request)) {
    dev->current_request = req;
```
是什么意思？
电梯算法完成后为什么没有执行 `do_hd_request` 函数？

10、`getblk` 函数中，两次调用 `wait_on_buffer` 函数，两次的意思一样吗？

11、`getblk` 函数中

``` C
do {
    if (tmp->b_count)
        continue;
    if (!bh || BADNESS(tmp)<BADNESS(bh)) {
        bh = tmp;
        if (!BADNESS(tmp))
            break;
    }
/* and repeat until we find something good */
    } while ((tmp = tmp->b_next_free) != free_list);
```
说明什么情况下执行 `continue`、`break`。

12、`make_request` 函数

``` C
if (req < request) {
    if (rw_ahead) {
        unlock_buffer(bh);
        return;
    }
    sleep_on(&wait_for_request);
    goto repeat;
```
其中的 `sleep_on(&wait_for_request)` 是谁在等？等什么？

13、`bread` 函数代码中
``` C
if ( bh->b_uptodate )
    return bh;
ll_rw_block( READ, bh );
wait_on_buffer( bh );
if ( bh->b_uptodate )
    return bh;
```
为什么要做第二次 `if (bh->b_uptodate)` 判断？
