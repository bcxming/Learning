### 基本使用

**<font color='red'>Linux系统里面他的文件权限系统是怎么样的？怎么控制它的文件权限，哪些值分别代表什么意思？</font>**

在 Linux 系统中，文件权限系统使用了 **用户**（User）、**用户组**（Group）和 **其他人**（Others）这三种身份来控制对文件和目录的访问。每种身份下设置了 **读取**（Read）、**写入**（Write）和 **执行**（Execute）三种权限，文件权限控制通过这些身份和权限的组合实现。

==文件权限概述==

每个文件或目录的权限表示为一个 9 位字符，例如：`rwxr-xr--`，这些字符从左到右分为三组，每组三位：

- **用户权限**（User）：前 3 位，定义文件所有者对该文件的权限。
- **用户组权限**（Group）：中间 3 位，定义与文件所属组的其他用户对该文件的权限。
- **其他人权限**（Others）：最后 3 位，定义系统中其他用户对该文件的权限。

每种权限由三种字符表示：

- `r`：读取权限（Read），允许查看文件内容或列出目录内容。
- `w`：写入权限（Write），允许修改文件内容或添加、删除目录中的文件。
- `x`：执行权限（Execute），允许执行文件或进入目录。

==权限值的数值表示==

在 Linux 中，文件权限也可以用数字表示，方便快速设置权限。每个权限字符对应一个数值：

- **读取**（Read，`r`）对应值：`4`
- **写入**（Write，`w`）对应值：`2`
- **执行**（Execute，`x`）对应值：`1`
- **无权限**（`-`）对应值：`0`

每组三位权限的值相加可以表示该组的权限，例如：

- `rwx` = 4 + 2 + 1 = `7`
- `rw-` = 4 + 2 + 0 = `6`
- `r--` = 4 + 0 + 0 = `4`

这样，一个文件权限如 `rwxr-xr--` 可以表示为 `750`，即：

- 用户权限：`rwx` = `7`
- 用户组权限：`r-x` = `5`
- 其他人权限：`r--` = `4`

==修改权限：==可以使用 `chmod` 命令来更改文件和目录的权限，通过字符表示或数字表示来设置

**<font color='red'>一般655是什么权限？</font>**

权限 `655` 表示文件或目录的权限组合，分为用户（User）、用户组（Group）和其他人（Others）的权限。具体来说，`655` 代表的权限是：

- **用户（User）**：`6`，即 `rw-`（可读、可写，无执行权限）。
- **用户组（Group）**：`5`，即 `r-x`（可读、无写权限，可执行）。
- **其他人（Others）**：`5`，即 `r-x`（可读、无写权限，可执行）。

**<font color='red'>在Linux上，僵尸进程是怎么形成的？</font>**

主要是由于**父进程未及时回收子进程的退出状态**；这些进程占据着系统的进程表条目，但不占用系统资源

==形成原因：==

**父进程未调用 `wait()`**：有时父进程设计上未调用 `wait()` 系统调用，可能是因为编写时忽略了子进程的清理，或者父进程被设计成不关注子进程的状态。

**父进程未正确处理 SIGCHLD 信号**：父进程可以通过处理 `SIGCHLD` 信号来在子进程退出时自动回收资源，但如果父进程未正确处理这个信号，也可能导致僵尸进程的形成。

==危害：==僵尸进程本身不占用内存等系统资源，但它会占据一个进程 ID

**<font color='red'>top命令，它是怎么做到能看到整机资源的？</font>**

`top` 命令通过读取 `/proc` 虚拟文件系统中的各个文件，动态获取系统的 CPU、内存、进程、负载等信息。

`top` 结合这些文件的数据并进行处理、计算，最终展示出整个系统的资源使用情况。

这种方式使得 `top` 可以在不直接访问内核的情况下高效、稳定地监控系统资源。

**<font color='red'>操作系统下面有个proc目录，讲一下这个目录中管理的都是什么？</font>**

`/proc` 目录是一个**虚拟文件系统**，用于显示内核和系统运行时的信息

==1、进程信息：==`/proc` 目录包含了系统中所有正在运行的进程信息，每个进程都用其 PID 作为目录名称

==2、系统信息：==`/proc` 目录中还包含许多文件和子目录，用于显示整个系统的状态信息

==3、 内核和系统配置==

==4、硬件和设备==

**<font color='red'>Linux系统中的管道和重定向的区别</font>**

在 Linux 系统中，**管道**和**重定向**都是用于处理输入输出的方式

1、管道用于将**一个命令的输出作为另一个命令的输入**，从而将多个命令组合在一起，实现数据流的传递。管道通过符号 `|` 实现；如grep

2、重定向用于将**命令的输入或输出重定向到文件或其他位置**。重定向符号包括 `>`（输出重定向）、`>>`（追加输出）、`<`（输入重定向）

**<font color='red'>打包文件夹</font>**

```shell
tar -czf chatserver.tar.gz chatserver-master
-c: 创建一个新的归档文件。
-J: 使用 xz 压缩。
-v: 显示压缩过程中的详细信息。
-f: 指定归档文件名。
tar -xJvf archive_name.tar.xz
-x: 解压文件。
-J: 使用 xz 解压。
-v: 显示解压过程中的详细信息。
-f: 指定归档文件名
```

**<font color='red'>查看文件大小</font>**

```shell
ls -lh
```

less、tail、cat

**<font color='red'>查看显卡</font>**

```shell
nvidia-smi
```

**<font color='red'>复制文件夹</font>**

```shell
cp -r 源 目标
```

**<font color='red'>linux中export的作用</font>**

在 Linux 中，`export` 命令用于将 shell 变量声明为环境变量，使其可以在当前 shell 会话中被子进程继承。

```shell
export PATH=$PATH:/usr/local/bin
```

### 进程

**<font color='red'>运行中的进程都有一个以其 PID 为名的子目录,我要查看这个进程的CPU占用怎么看？</font>**

在 Linux 系统中，可以通过 `/proc/[PID]/stat` 文件查看指定进程的 CPU 使用情况。这个文件包含了进程的各种状态信息，包括 CPU 使用时间

**<font color='red'>Linux如何查看一个进程的信息？</font>**

**基本信息**：使用 `ps`

**实时监控**：使用 `top` 或 `htop`

**详细信息**：使用 `/proc` 文件系统

**内存分部**：`pmap` 

**文件句柄**：使用 `lsof`

**系统调用**：使用 `strace`

**网络连接**：使用 `netstat`

**<font color='red'>哪个命令可以查看端口被哪个应用占用？</font>**

```shell
lsof -i :3000
```

**<font color='red'>linux怎么查看线程和进程运行状态</font>**

查看进程

```shell
ps -aux  # 显示所有进程的详细信息
ps -p <PID>   # 显示指定 PID 进程的详细信息
ps aux | grep <PID>   # 使用 grep 过滤指定 PID 的进程信息
top -p <PID>   # 实时显示指定 PID 进程的详细信息
top         # 显示实时的进程和线程状态信息
ps -eLf # 会列出所有进程及其线程
```

查看线程

```shell
ps -Lp <PID>   # 显示指定 PID 进程的所有线程的详细信息
```

**<font color='red'>怎么查看线程资源发生泄漏</font>**

**使用 Valgrind 工具**：

```
valgrind --leak-check=full ./your_program
```

**使用 gdb 调试器**：可以使用 gdb 调试器来检查程序中的线程资源泄漏问题

**使用 strace 工具**：strace 工具可以跟踪系统调用并记录程序的系统调用信息

**使用性能分析工具**：使用性能分析工具（如 perf、gprof 等）来分析程序的运行性能

**<font color='red'>怎么排除linux中句柄发生泄漏</font>**

**使用 lsof 命令**：lsof 命令可以列出系统中打开的文件和文件描述符信息

```
lsof -p <PID>
```

**使用 Valgrind 工具**：Valgrind 是一个强大的内存检测工具，可以用于检测程序中的内存泄漏问题

### 网络

**<font color='red'>linux看端口占用情况，都有什么命令？</font>**

`netstat -tuln`：查看所有端口的监听情况。

`ss -tuln`：查看监听端口，更快。

`lsof -i :<port>`：查看特定端口的进程。

`fuser <port>/tcp`：查看端口被哪个进程使用。

`nmap -p- localhost`：扫描本机所有开放端口。

**<font color='red'>linux上怎么查看路由表</font>**

```shell
# 查看路由表（现代方法）
ip route
# 查看路由表（传统方法）
route -n
# 查看路由表（使用 netstat）
netstat -r
```

**<font color='red'>指定IP端口建立TCP</font>**

```CPP
telnet 192.168.1.100 80
```

**<font color='red'>如何在 Linux 系统中查看 TCP 状态？</font>**

```shell
netstat -atn
netstat -atn | grep ':80'
netstat -atn | grep '192.168.1.100'
netstat -atn | grep 'ESTABLISHED'
netstat -atn | grep '192.168.1.100' | grep ':80'
```

- `-a` 显示所有连接，包括监听中的连接和已经建立的连接
- `-t` 仅显示 TCP 连接
- `-n` 显示数字地址和端口号，而不是尝试解析它们

**<font color='red'>查看网络流量</font>**

```shell
ss -s  #显示所有活动的套接字
netstat -i # 显示网络连接、路由表、接口统计等信息
```

**<font color='red'>Linux系统中查询指定端口</font>**

```shell
root@iZ0jlfgmvv2c1wis0pe11zZ:~# netstat -tuln|grep 3333
tcp        0      0 0.0.0.0:3333            0.0.0.0:*               LISTEN     
root@iZ0jlfgmvv2c1wis0pe11zZ:~# netstat -tapt|grep 3333
tcp        0      0 0.0.0.0:3333            0.0.0.0:*               LISTEN      33447/node   
```

### 内存

**<font color='red'>怎么判断操作系统有没有在内存替换？或者说怎么统计内存替换的频率？</font>**

1、sar - B 1查看直接和后台内存回收指标变化情况

2、查看 `/proc/vmstat` 文件

```shell
cat /proc/vmstat | grep pg
```

3、使用 `vmstat` 查看页面调度信息

```shell
vmstat 1
```

**<font color='red'>top 命令和 free 命令都可以查看内存，有什么区别？</font>**

free -h显示系统内存情况
top命令除了内存，还会显示系统任务，CPU使用情况和各个进程状态信息

**<font color='red'>怎么判断服务器内存是否够用？如何查看服务器性能瓶颈是否是内存？</font>**

```shell
free -h #它可以显示系统中内存的使用情况
# 观看是否有swap，dmesg查看系统日志OOM，vmstat观察内存使用情况
top
vmstat 1
```

**查看磁盘 I/O 使用情况**

```shell
sar -d 1 5
```

### 系统调优

**<font color='red'>现在写一些C++项目，部署到服务器上，他现在出现了OM，它被cue了，那这个时候需要排查一下这个问题怎么排查？</font>**

使用 `top` 和 `/proc` 文件查看内存占用情况。

检查系统日志中的 OOM 日志以获取具体信息。/var/log/messages

使用 `valgrind` 或其他工具进行内存泄漏分析。

分析代码中的内存管理方式，避免内存泄漏和过度分配。

**<font color='red'>如何写脚本查看linux上的cpu核心数量</font>**

常见方法是使用 `nproc` 或通过读取系统文件 `/proc/cpuinfo`

```shell
#!/bin/bash
# 获取 CPU 核心数量
cpu_cores=$(nproc)
echo "CPU 核心数量: $cpu_cores"

#!/bin/bash
# 获取 CPU 核心数量
cpu_cores=$(grep -c ^processor /proc/cpuinfo)
echo "CPU 核心数量: $cpu_cores"
```

**<font color='red'>查看中断信息？</font>**

```shell
cat /proc/softirqs # 软中断 
cat /proc/interrupts # 硬中断
watch -d cat /proc/softirqs # 命令查看中断次数的变化速率
```

**<font color='red'>Linux 服务器当中如何查看负载情况？通过什么指标进行查看？</font>**

1、top

2、uptime可以查看1，5，15分钟的平均负载

3、cpu使用率和平均负载不一定完全对应，mpstat查看CPU性能（IO密集，计算密集）

4、pidstat查看哪个进程

**<font color='red'>Linux 服务器当中如何查看负载情况？通过什么指标进行查看？</font>**

`top` 命令提供了实时的系统监控信息，包括 CPU 使用率、内存使用情况、进程列表等

```shell
top
```

uptime可以查看1，5，15分钟的平均负载

```shell
[root@iZ2zeg7ccxt693shmqcw6oZ ~]# uptime
 10:17:41 up 300 days, 13:35,  1 user,  load average: 0.01, 0.02, 0.05
```

cpu使用率和平均负载不一定完全对应，mpstat查看CPU性能（IO密集，计算密集）

```shell
[root@iZ2zeg7ccxt693shmqcw6oZ ~]# mpstat
Linux 3.10.0-1160.95.1.el7.x86_64 (iZ2zeg7ccxt693shmqcw6oZ) 	09/08/2024 	_x86_64_	(2 CPU)

10:19:48 AM  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
10:19:48 AM  all    0.73    0.00    0.57    0.05    0.00    0.00    0.00    0.00    0.00   98.65
```

### Git

**<font color='red'>Git 冲突怎么解决</font>**

**查看冲突文件：**你可以使用 `git status` 命令来查看冲突文件的列表

**编辑冲突文件**：打开冲突文件，你会看到 Git 在文件中标记出了冲突的部分

**标记冲突已解决**：使用 `git add` 命令将修改后的文件标记为已解决冲突状态

**完成合并**：当所有的冲突文件都解决了，git merge --continue

**<font color='red'>git怎么保证代码之间不冲突，保证可以最终合并？</font>**

**<font color='red'>线上代码部署的是master分支，本地开发一个新的分支，分支代码你还不想提交，但是你现在要去修改线上的bug，怎么操作git？</font>**