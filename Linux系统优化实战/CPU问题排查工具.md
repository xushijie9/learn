#### CPU问题排查工具

* top
    - top -Hp PID 查看指定进程使用CPU情况, H 查看线程
* pidstat
    - pidstat     查看系统CPU使用情况
    - pidstat -w  查看系统CPU上下文切换情况
    - pidstat -wt 查看系统进程的线程CPU上下文切换情况
* mpstat
    - mpstat -P ALL 查看系统所有CPU的运行情况
* vmstat
*   - vmstat -w   查看系统内存, CPU, CPU上下文切换的运行情况
* perf