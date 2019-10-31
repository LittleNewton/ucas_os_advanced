1、为什么要设计缓冲区，有什么好处？

2、操作系统如何利用buffer_head中的 b_data，b_blocknr，b_dev，
b_uptodate，b_dirt，b_count，b_lock，b_wait管理缓冲块的？

3、操作系统如何处理多个进程等待同一个正在与硬盘交互的缓冲块？

4、getblk函数中，申请空闲缓冲块的标准就是b_count为0，而申请到之后，为
什么在wait_on_buffer(bh)后又执行if（bh->b_count）来判断b_count是否为0
？

5、b_dirt已经被置为1的缓冲块，同步前能够被进程继续读、写？给出代码证
据。

6、分析panic函数的源代码，根据你学过的操作系统知识，完整、准确的判断
panic函数所起的作用。假如操作系统设计为支持内核进程（始终运行在0特权
级的进程），你将如何改进panic函数？

7、详细分析进程调度的全过程。考虑所有可能（signal、alarm除外）

8、wait_on_buffer函数中为什么不用if（）而是用while（）？

9、add_reques（）函数中有下列代码
    if (!(tmp = dev->current_request)) {
        dev->current_request = req;
        sti();
        (dev->request_fn)();
        return;
    }
其中的
    if (!(tmp = dev->current_request)) {
        dev->current_request = req;
是什么意思？
电梯算法完成后为什么没有执行do_hd_request函数？

10、getblk（）函数中，两次调用wait_on_buffer（）函数，两次的意思一样
吗？

11、getblk（）函数中
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
说明什么情况下执行continue、break。

12、make_request（）函数    
    if (req < request) {
        if (rw_ahead) {
            unlock_buffer(bh);
            return;
        }
        sleep_on(&wait_for_request);
        goto repeat;

其中的sleep_on(&wait_for_request)是谁在等？等什么？

13、bread（）函数代码中
    if (bh->b_uptodate)
        return bh;
    ll_rw_block(READ,bh);
    wait_on_buffer(bh);
    if (bh->b_uptodate)
        return bh;
为什么要做第二次if (bh->b_uptodate)判断？
