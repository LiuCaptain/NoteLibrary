- `lscpu `；`lscpu` 命令用于显示有关 CPU 架构和信息的详细信息，它提供了关于系统中逻辑处理器、物理处理器、CPU 系列、架构、CPU 核心数等方面的信息。

  

- `cat /etc/os-release`；`cat /etc/os-release` 用于查看 Linux 系统的发行版信息；其中 `cat` 是一个用于连接文件并打印到标准输出的命令，`/etc/os-release` 这是一个包含有关操作系统信息的文件的路径。在大多数基于 systemd 的 Linux 发行版中，这个文件包含了关于发行版、版本、ID、名称等信息。

  

- `mkdir` 是用于创建目录的命令；`mikdir -p` 其中携带的参数 `-p` 允许递归创建目录，即使上级目录不存在也会一并创建

- `ps aux | grep nginx` 是一个 Linux 命令，用于查看当前运行的与 `nginx` 相关的进程

- `pstree <PSID>` 查看该进程的树状结构