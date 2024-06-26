---
title: Docker 使用
description: Docker 使用
keywords:
  - Docker
  - 使用
tags:
  - Docker/进阶
  - 技术/操作系统
author: 仲平
date: 2024-02-28
---

Docker 是一个开源平台，用于自动化开发、部署和运行应用程序的过程，通过使用容器化技术，Docker 允许开发者将应用及其依赖打包成一个轻量级、可移植的容器，然后这个容器可以在任何地方运行。这样，它解决了“在我的机器上可以运行”的问题，提高了软件交付的效率和可靠性。

## 容器

容器化是一种虚拟化技术，它允许你在隔离的环境中运行和部署应用。每个容器独立运行一个或多个应用程序，包括它们所需的所有依赖。这就像将你的应用及其所有依赖打包在一个集装箱中，无论在什么环境下运输（运行），都能确保其内容不受影响。

### 生命周期

容器的生命周期管理是 Docker 使用的核心概念之一，涉及容器从创建到销毁的全过程。容器的生命周期可以通过一系列的 Docker 命令来管理，这些命令包括创建、启动、停止、重启、删除等操作。

![生命周期](https://static.7wate.com/2024/02/20/3dff18d6e840374447cb5b2a1bfab3e4-docker-workflow.gif)

### 创建容器

创建容器是容器生命周期的第一步，这一过程依赖于 Docker 镜像。Docker 镜像是一个包含了应用及其依赖的轻量级、可执行的软件包，确保了应用在任何环境下的一致性和可移植性。

```shell
docker run hello-world
```

### 容器管理

容器一旦创建，就可以通过各种命令进行管理。这些命令允许用户控制容器的生命周期，包括启动、停止、重启和删除等操作。

- **启动容器**：`docker start my_container`，此命令用于启动一个或多个已经创建但停止运行的容器。
- **停止容器**：`docker stop my_container`，该命令用于停止一个或多个正在运行的容器。停止容器会向容器内的主进程发送 SIGTERM 信号，之后发送 SIGKILL 信号，以确保容器停止运行。
- **查看运行中的容器**：`docker ps`，此命令展示了所有当前正在运行的容器。使用 `-a` 选项可以查看包括停止的在内的所有容器。
- **进入运行中的容器**：`docker exec -it my_container /bin/bash`，该命令允许用户进入一个正在运行的容器内部，并以交互模式启动一个新的终端会话。这对于调试应用或管理容器内的服务非常有用。
- **删除容器**：`docker rm my_container`，使用这个命令可以删除一个或多个已停止的容器。如果要删除运行中的容器，需要加上 `-f` 或 `--force` 参数来强制删除。

### 高级容器管理

除了基本的生命周期管理命令，Docker 还提供了一系列高级功能，以支持更复杂的容器操作和管理需求。

- **查看容器日志**：`docker logs my_container`，这个命令允许用户检查和跟踪容器内的标准输出和错误输出。对于调试应用和监控容器运行状态非常有用。
- **查看容器内部进程**：`docker top my_container`，此命令显示运行在容器内部的进程列表，有助于了解容器内部的活动。
- **暂停容器**：`docker pause my_container`，该命令用于暂停运行中的容器，所有进程都会被挂起。这可以用于资源管理或在特定时刻“冻结”容器的状态。
- **恢复容器**：`docker unpause my_container`，与 `docker pause` 相对，此命令用于恢复被暂停的容器的执行。
- **容器资源限制**：在创建或运行容器时，可以通过 `--memory`、`--cpu-shares` 等参数来限制容器可以使用的资源，从而避免单个容器占用过多的系统资源。

## 数据卷

数据卷是 Docker 实现数据持久化和共享的关键机制之一。通过使用数据卷，用户可以在不同容器之间共享数据，同时保证数据的持久化存储，即使容器被删除，卷中的数据也不会丢失。

- **数据持久化**：数据卷提供了一种机制，可以将数据存储在容器之外，确保重要数据不会因容器的删除而丢失。
- **数据共享与重用**：数据卷可以被多个容器挂载和访问，实现数据的共享。
- **容器解耦**：通过数据卷，应用程序的运行状态可以与数据保持独立，便于应用的迁移和备份。

### 使用数据卷

创建和使用数据卷的基本命令如下：

```shell
# 创建一个新的数据卷
docker volume create my_volume

# 将数据卷挂载到容器
docker run -d -v my_volume:/path/in/container --name my_container my_image
```

此命令会启动一个新的容器，将之前创建的数据卷 `my_volume` 挂载到容器的指定路径 `/path/in/container` 下。容器内应用对该路径的任何写操作都会直接反映到数据卷上，同样，对数据卷的任何更改也会立即在挂载它的所有容器中可见。

### 数据卷容器

数据卷容器是一种使用数据卷共享数据的模式。通过创建一个专门的容器来持有数据卷，其他容器可以通过 --volumes-from 标志来挂载这个容器中的数据卷。

```shell
# 创建一个带数据卷的容器
docker run -d --name data_container -v my_volume:/path/in/container my_image

# 使用 --volumes-from 从其他容器挂载数据卷
docker run -d --name app_container --volumes-from data_container my_app_image
```

这种方法使得数据的管理和共享变得更加集中和高效。

## Docker 网络

Docker 网络功能允许容器相互通信，并与外部世界交互。Docker 提供了多种网络模式，以支持不同的使用场景。

### 网络模式

- **bridge**：默认网络模式。当容器运行在桥接网络中时，Docker 会自动使用私有子网内的 IP 地址来分配给每个容器，并通过 NAT 实现与外部网络的通信。
- **host**：在这种模式下，容器共享宿主机的网络命名空间，不进行网络隔离。容器的网络性能更好，但是安全性降低。
- **none**：在这种模式下，容器具有自己的网络命名空间，但不配置任何网络接口，通常用于需要手动管理网络的高级场景。

### 自定义网络

创建自定义网络可以提供更灵活的网络配置，使得容器间的通信更加方便和安全。

```shell
# 创建自定义桥接网络
docker network create --driver bridge my_custom_bridge

# 运行容器时指定网络
docker run -d --name my_container --network my_custom_bridge my_image
```

在自定义网络中，容器可以通过容器名相互访问，而不需要使用 IP 地址，简化了容器间通信的配置。

### 网络连接和断开

Docker 允许在运行时将容器连接到网络或从网络断开。

```shell
# 将运行中的容器连接到网络
docker network connect my_custom_bridge my_container

# 将容器从网络断开
docker network disconnect my_custom_bridge my_container
```

这提供了动态管理容器网络连接的灵活性，允许根据需要调整容器的网络配置。
