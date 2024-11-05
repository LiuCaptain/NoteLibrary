# 安装nodejs步骤

1. wget 命令下载Node.js安装包。

选择自己需要安装的版本

```
复制代码 https://nodejs.org/dist
```

选择带`linux-x64.tar.xz`的安装包，该安装包是编译好的文件，解压之后，在bin文件夹中就已存在node和npm，无需重复编译。

```
复制代码wget https://nodejs.org/dist/latest-v10.x/node-v10.17.0-linux-x64.tar.xz  
```

> 如果提示找不到`wget`命令,可以先安装`weget`,执行`yum -y install wget`后再安装。

1. 解压文件。

```
复制代码tar xvf node-v10.17.0-linux-x64.tar.xz 
```

1. 创建软链接，使node和npm命令全局有效。

通过创建软链接的方法，使得在任意目录下都可以直接使用node和npm命令：

```
复制代码ln -s /root/node-v10.17.0-linux-x64/bin/node /usr/local/bin/node
ln -s /root/node-v10.17.0-linux-x64/bin/npm /usr/local/bin/npm
```



