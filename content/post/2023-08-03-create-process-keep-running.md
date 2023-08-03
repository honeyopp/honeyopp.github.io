---
showonlyimage: true
title: "linux设置开启重启执行某个shell脚本"
date: 2023-08-03T14:50:20+08:00
published: true
subtitle:   ""
excerpt: "介绍怎么让一个进程可以进驻到linux系统中并开机自启"
description: "介绍怎么让一个进程可以进驻到linux系统中并开机自启"
author: "冷轩"
image: "/img/2018-06-04-introducing-the-istio-v1alpha3-routing-api/background.jpg"
tags:
    - Linux
categories: [学习]
URL: "/2023/08/03/create-process-keep-running/"
---

## linux设置开启重启执行某个shell脚本


要在Linux中设置开机启动后重启时执行某个shell脚本，您可以使用`/etc/rc.local`或Systemd服务来实现。在这里，我将向您展示使用Systemd服务的方法，因为`/etc/rc.local`已经在一些Linux发行版中`被弃用`

## 以下是使用Systemd服务的步骤

创建脚本文件:
1. 首先，创建您要在系统启动时运行的shell脚本文件。假设您的脚本文件名为myscript.sh，可以使用任何文本编辑器来创建它。例如，使用vi编辑器：
    ```shell
       sudo vi /path/to/myscript.sh
    ```

2. 在编辑器中，添加您想要在启动时运行的命令和操作。保存并关闭文件 赋予脚本可执行权限：

   确保您的脚本文件具有可执行权限。使用以下命令赋予执行权限：
    
    ```shell
        sudo chmod +x /path/to/myscript.sh
    ```
    
3. 创建Systemd服务单元,在`/etc/systemd/system`目录下创建一个新的Systemd服务单元文件，例如`myscript.service`：
    ```shell
        sudo nano /etc/systemd/system/myscript.service
    ```

4. 在编辑器中，添加以下内容：

    ```shell
        [Unit]
        Description=My Script Service
        After=network.target
        
        [Service]
        ExecStart=/path/to/myscript.sh
        
        [Install]
        WantedBy=multi-user.target
     ```
    
    ps:请将/path/to/myscript.sh替换为您实际脚本的路径。


5. 启用并启动Systemd服务,现在，启用并启动新创建的Systemd服务：

    ```shell
        sudo systemctl enable myscript.service
        sudo systemctl start myscript.service
    ```


6. 验证：您可以使用以下命令检查服务的状态和日志：
    ```shell
        sudo systemctl status myscript.service
    ```

<!--more-->
确保服务处于"active (running)"状态，并查看相关日志以确保脚本正常运行。

现在，无论是开机启动还是重启，您的脚本都会自动执行。Systemd服务提供了更强大和灵活的方式来管理和监控服务。



