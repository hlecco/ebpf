# Introduction to eBPF

Here is a list of some resources that are probably much more useful
than my own notes, because they are what I used to prepare these.

 - [Brendan Gregg's Learn eBPF Tracing](https://www.brendangregg.com/blog/2019-01-01/learn-ebpf-tracing.html)
 - [bcc tutorial](https://github.com/iovisor/bcc/blob/master/docs/tutorial.md)

## Linux performance analysis
Based on [Netflix's Medium post](https://netflixtechblog.com/linux-performance-analysis-in-60-000-milliseconds-accc10403c55)

TLDR: run this:
```bash
uptime
dmesg | tail
vmstat 1
mpstat -P ALL 1
pidstat 1
iostat -xz 1
free -h
sar -n DEV 1
sar -n TCP,ETCP 1
top
```

Let's run these commands one by one.

### uptime
From the manual:
The uptime utility displays the current time, the length of time the system has been up,
the number of users, and the load average of the system over the last 1, 5, and 15 minutess.

### dmesg
A command that prints kernel logs.
I actually find more useful to look at `journalctl` first, on systems that use systemd.
Most Linux distributions use it, so it can be quite useful.

`dmesg` may be useful, for example, in finding OOM-killer activity.

### vmstat
Provides an overview on workload and stress, loosely speaking.
The argument is the time interval to consider.
 - `r`: number of CPU processes waiting, excluding I/O;
 - `free`: free memory in kb;
 - `si`: swap-ins;
 - `so`: swap-outs;
 - `us`: CPU time on user mode;
 - `sy`: CPU time on kernel mode;
 - `wa`: I/O wait
 - `st`: stolen time by other guests

### mpstat
Report processors related statistics. Probably means "multiprocessor stat" or something like that.
Allows checking workload by processor.
This could be used, for example, to evaluate "single-threadness".

Flag `-P` is used to specify which processors to list (`ALL`), and `1` is the interval in seconds.


### pidstat
Just like `top`, but does not clear the screen.
Can be used to identify resource hungry processes.

### iostat
From the manual:
The `iostat` command is used for monitoring system input/output device loading by observing
the time the devices are active in relation to their average transfer rates.

`-x` displays extended statistics and `-z` omits activity for idle devices.

The command provides information on CPU, device and network utilization.
Can be used to detect saturation.

### free
Explains memory usage.
Useful to understand exactly how much memory is used for cache,
and also shows how much memory is _free_ in the sense of
free or not cached.

`-h` is a display option, to show as human-readable


### sar
`iostat` specialized for network I/O.
Can be used to check if maximum network usage has been reached, for example.

Also measures number of local and remotely-initiated TCP connections.
Retransmissions may indicate a faulty network interface.
