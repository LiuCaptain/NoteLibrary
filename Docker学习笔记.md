# Docker 学习笔记

> **容器**（Container）是指一种技术，Docker 只是一种容器技术的实现，所以 `Docker !== Container`

- #### Docker Desktop 版本与 Docker Linux 版本有什么区别？

  Docker 是 CS（Client Server）架构，Docker Desktop 版本除了安装 Client 的命令行接口 + Server 的 Engine（包含 Docker 的 daemon）以外，还会安装一些其他东西；而 Docker Server 版本只会安装 Client 的命令行接口 + Server 的 Engine（包含 Docker 的 daemon）

- #### 在 Linux 上安装 Docker

  1. 通过 curl 命令下载安装脚本

     ```bash
     curl -fsSL get.docker.com -o get-docker.sh
     ```

     执行上面命令会下载保存一个 `get-docker.sh` 文件

  2. 执行下载的 shell 脚本安装 Docker

     ```bash
     sh get-docker.sh
     ```

- #### 查看 Docker 版本的命令

  ```bash
  docker version
  ```

- #### 启动 Docker 守护进程的命令

  ```bash
  systemctl start docker
  ```

- 查看 Docker 状态的基本信息

  ```bash
  docker info
  ```

  

