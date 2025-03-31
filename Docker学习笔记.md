# Docker 学习笔记

> **容器**（Container）是指一种技术，Docker 只是一种容器技术的实现，所以 `Docker !== Container`

1. ##### Docker Desktop 版本与 Docker Linux 版本有什么区别？

   Docker 是 CS（Client Server）架构，Docker Desktop 版本除了安装 Client 的命令行接口 + Server 的 Engine（包含 Docker 的 daemon）以外，还会安装一些其他东西；而 Docker Server 版本只会安装 Client 的命令行接口 + Server 的 Engine（包含 Docker 的 daemon）

2. ##### 在 Linux 上安装 Docker

   - 通过 curl 命令下载安装脚本

     ```bash
     curl -fsSL get.docker.com -o get-docker.sh
     ```

     执行上面命令会下载保存一个 `get-docker.sh` 文件

   - 执行下载的 shell 脚本安装 Docker

     ```bash
     sh get-docker.sh
     ```

3. ##### 查看 Docker 版本的命令

   ```bash
   docker version
   ```

4. ##### 启动 Docker 守护进程的命令

   ```bash
   systemctl start docker
   ```

5. ##### 重新加载 Docker 服务配置

   ```bash
   systemctl daemon-reload
   ```

   具体过程:

   - 检查和解析所有 `systemd` 单元文件的更改
   - 将更改的单元文件加载到内存中
   - 不会停止或启动任何服务，仅仅是让 `systemd` 更新它的配置状态

   > `systemd` 是一个现代的初始化系统（init system）和服务管理器，专为 Linux 系统设计

6. ##### 重启 Docker 服务

   ```bash
   systemctl restart docker
   ```

   具体过程:

   - 停止 Docker 服务
   - 启动 Docker 服务
   - 加载最新的服务配置（如果先执行了 `systemctl daemon-reload`）

7. ##### 查看 Docker 状态的基本信息

   ```bash
   docker info
   ```

8. **查看 Docker 所有命令**

   - 执行上面命令会输出所有 Docker 命令；Docker 的基本命令结构是：`docker [操作对象] [操作行为]`

     ```bash
     docker
     ```

   - 执行下面命令会输出所有可对 Docker container 操作的行为

     ```bash
     docker container --help
     ```

   - 执行下面命令会输出所有可对 Docker image 操作的行为

     ```bash
     docker image --help
     ```

9. ##### 查看 Docker 容器命令

   - 查看正在运行的容器

     ```bash
     docker container ls
     
     docker container ps // 旧版本
     ```

   - 查看所有容器，包括正在运行和已经停止的容器

     ```bash
     docker container ls -a
     
     docker container ps -a  // 旧版本
     ```

10. ##### 查看 Docker 镜像命令

    ```bash
    docker image ls
    ```

11. ##### 删除 Docker 镜像命令

    ```bash
    docker image rm <IMAGE>
    ```

12. ##### Docker 镜像与 Docker 容器

    - Docker 镜像
      1. Docker 镜像是一个 `read-only` 文件
      2. Docker 镜像包含文件系统、源码、库文件、依赖、工具等一些运行 application 所需要的文件
      3. Docker 镜像可以理解成一个模板
      4. Docker 镜像具有分层的概念

    - Docker 容器

      1. Docker 容器是 Docker 镜像的实例

      2. Docker 容器实质是复制 Docker 镜像并在镜像最上层加一层 read-write 的层，称之为 **容器层**（container layer）

         ![container](F:\Typora保存的文件\NoteLibrary\images\container.svg)

13. ##### 创建一个 Docker 容器

    ```bash
    docker container run <IMAGE>
    ```

14. ##### 创建一个拥有名字的 Docker 容器

    ```bash
    docker container run --name <NAME> <IMAGE>
    ```

15. ##### 创建一个具有端口映射的 Docker 容器

    ```bash
    docker container run -p <宿主机端口>:<容器内端口> <IMAGE>
    
    docker container run --publish <宿主机端口>:<容器内端口> <IMAGE>
    ```

16. ##### 停止一个 Docker 容器

    ```bash
    docker container stop <CONTAINER>
    ```

17. ##### 删除一个 Docker 容器

    ```bash
    docker container rm <CONTAINER>
    ```

18. ##### 强制删除一个 Docker 容器

    ```bash
    docker container rm <CONTAINER> -f
    ```

19. ##### 查询所有 Docker 容器的 id（包括正在运行和已经停止的容器）

    ```bash
    docker container ls -aq
    ```

20. ##### 批量停止 Docker 容器

    ```bash
    docker container stop <CONTAINER1> <CONTAINER2> <CONTAINER3>...
    ```

21. ##### 停止所有 Docker 容器

    ```bash
    docker container stop $(docker container ls -aq)
    ```

22. ##### 批量删除 Docker 容器

    ```bash
    docker container rm <CONTAINER1> <CONTAINER2> <CONTAINER3>...
    ```

23. ##### 删除所有 Docker 容器

    ```bash
    docker container rm $(docker container ls -aq)
    ```

24. ##### Docker 容器的 attached 和 detached 模式

    创建 Docker 容器默认是 attached 模式，attached 模式的容器是在前台运行，会把容器的输入输出结果反映到到本地的，比如访问服务会在命令行打印出访问日志；本地的输入输出也会反映到容器中去，比如输入 ctrl + c 会停止容器。但 detached 模式并不会有输入输出的反映

    - 创建一个 detached 模式的容器

      ```bash
      docker container run -d <IMAGE>
      
      docker container run --detach <IMAGE>
      ```

    - 将 detached 模式的容器切换为 attached 模式

      ```bash
      docker container attach <CONTAINER>
      ```

25. ##### 查看 Docker 容器日志

    - 查看 Docker 容器日志

      ```bash
      docker container logs <CONTAINER>
      ```

    - 实时跟踪查看 Docker 容器日志

      ```
      docker container logs -f <CONTAINER>
      ```

26. ##### 创建一个交互式运行的 Docker 容器

    ```bash
    docker container run -it <IMAGE> <COMMAND>
    ```

    当执行 `exit` 命令，这个交互式运行的容器会退出

27. ##### 要可交互式的进入正在运行的 Docker 容器

    ```bash
    docker container exec -it <CONTAINER> <COMMAND>
    ```

    当执行 `exit` 命令，这个运行的容器**不会**退出

28. ##### 查询 Docker 容器启动了哪些进程

    ```bash
    docker container top <CONTAINER>
    ```

    这些进程运行在 Docker engine 的宿主机上

29. ##### 创建 Docker 容器时背后发生了什么？

    比如执行下面命令

    ```bash
    docker container run -d -p 80:80 --name customName nginx
    ```

    ![](F:\Typora保存的文件\NoteLibrary\images\container-back.svg)

30. 
