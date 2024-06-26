---
title: Linux 基本配置
description: 执行基本的系统管理任务、配置环境设置、以及配置网络访问和系统安全。管理用户、组和文件权限。
keywords:
  - 系统配置
  - 用户管理
tags:
  - Linux/系统管理
  - 技术/操作系统
author: 仲平
date: 2024-03-26
---

## 1. 网络配置

网络配置是 Linux 系统管理中的一个基础且关键的环节，它确保系统能够正确地连接到网络并进行通信。以下是网络配置的一些基本步骤和建议实践：

### 1.1 配置网络接口

确保正确配置网络接口，以便系统能够连接到互联网或内部网络。这通常涉及到设置静态 IP 地址或配置 DHCP 客户端。

- **静态 IP 地址配置**：编辑网络配置文件，如 `/etc/network/interfaces`（对于基于 Debian 的系统）或 `/etc/sysconfig/network-scripts/ifcfg-<interface>`（对于基于 RedHat 的系统），来设置静态 IP 地址、子网掩码、默认网关和 DNS 服务器。
- **DHCP 客户端设置**：大多数现代 Linux 发行版使用 NetworkManager 或 NetworkConfig 工具来管理 DHCP。可以通过图形界面或相应的命令行工具来启用 DHCP 并配置网络接口。

### 1.2 配置 DNS

DNS 配置对于确保域名能够正确解析至关重要。以下是配置 DNS 的一些建议实践：

- **使用 NetworkManager 管理 DNS**：NetworkManager 可以自动配置 DNS 设置，包括使用网络接口的 DHCP 配置或手动设置 DNS 服务器。
- **手动配置 DNS**：如果需要手动配置 DNS，可以编辑 `/etc/resolv.conf` 文件来添加 DNS 服务器。此外，`/etc/hosts` 文件也可以用来添加静态主机名到 IP 地址的映射，这对于本地网络中的名称解析非常有用。

## 2. 主机配置

主机配置涉及到设置系统的基本信息，如主机名、语言和键盘布局、时区等。以下是主机配置的一些基本步骤和建议实践：

### 2.1 配置主机名称

主机名称是系统在网络上的身份标识，正确配置主机名称对于网络管理和故障排查非常重要。

- **使用 hostnamectl 配置主机名**：`hostnamectl` 是一个强大的工具，可以用来查看和修改系统的主机名。使用 `sudo hostnamectl set-hostname <新主机名>` 命令可以直接设置新的主机名。
- **手动编辑配置文件**：也可以直接编辑 `/etc/hostname` 文件来更改主机名，并更新 `/etc/hosts` 文件以确保内部名称解析的一致性。

### 2.2 配置语言和键盘布局

正确的语言和键盘布局设置可以提高用户工作效率，并确保系统输出的一致性。

- **使用 localectl 配置语言和键盘布局**：`localectl` 命令可以用来设置系统语言和键盘布局。例如，使用 `sudo localectl set-locale LANG=en_US.UTF-8` 设置系统语言为英语（美国），使用 `sudo localectl set-keymap us` 设置键盘布局为美国标准布局。
- **手动编辑配置文件**：语言和键盘布局的配置通常存储在 `/etc/locale.conf` 和 `/etc/vconsole.conf` 文件中。可以直接编辑这些文件来设置系统语言和键盘布局。

### 2.3 配置时区

正确的时区设置对于系统日志、定时任务和用户活动的记录非常重要。

- **使用 timedatectl 配置时区**：`timedatectl` 是一个用于管理日期和时间设置的工具。使用 `sudo timedatectl set-timezone <时区>` 命令可以设置系统时区，例如 `sudo timedatectl set-timezone Asia/Shanghai` 将时区设置为上海。
- **手动链接时区文件**：时区信息可以通过链接 `/etc/localtime` 文件到 `/usr/share/zoneinfo/` 目录下的相应时区文件来设置。`timedatectl` 命令会自动处理这个链接的更新。

### 2.4 配置 NTP

网络时间协议（NTP）用于同步系统时钟与互联网时间服务器，确保系统时间的准确性。

- **安装并启动 NTP 服务**：安装 `chronyd` 或 `ntpd` 服务，并启动它们。对于 `chronyd`，使用 `sudo systemctl start chronyd` 和 `sudo systemctl enable chronyd` 命令；对于 `ntpd`，使用 `sudo systemctl start ntpd` 和 `sudo systemctl enable ntpd` 命令。
- **配置 NTP 服务器**：在 NTP 配置文件中指定要使用的 NTP 服务器。对于 `chronyd`，编辑 `/etc/chrony.conf` 文件；对于 `ntpd`，编辑 `/etc/ntp.conf` 文件。添加 `server` 指令来指定 NTP 服务器地址。

### 2.5 配置时间同步

确保系统时间自动与选定的 NTP 服务器保持同步，这通常通过 NTP 服务的配置来自动完成。

- **检查和手动触发同步**：对于 `chronyd`，使用 `chronyc sources` 命令查看时间源状态，使用 `chronyc makestep` 命令手动触发立即同步。对于 `ntpd`，使用 `ntpq -p` 命令查看时间源状态。

## 3. 更新系统

为了确保系统的安全性和稳定性，定期更新操作系统和应用程序是非常重要的。这不仅可以修复已知的安全漏洞，还可以带来新功能和性能改进。

- **软件源管理**：首先，需要确保软件源（repositories）是最新和最可靠的
  - 对于基于 Debian 的系统，编辑 `/etc/apt/sources.list` 文件。
  - 对于基于 RedHat 的系统，编辑 `/etc/yum.repos.d/` 目录下的文件。
- **更新软件包列表**：使用包管理器更新本地软件包数据库。
  - 对于 APT，可以使用 `sudo apt-get update` 命令；
  - 对于 YUM，可以使用 `sudo yum makecache` 或者 `sudo dnf makecache`（如果你的系统使用 DNF 作为包管理器）。
- **更新升级系统**：在更新了软件包列表之后，可以进行系统更新。
  - 对于 APT，使用 `sudo apt-get upgrade` 命令；
  - 对于 YUM 或 DNF，使用 `sudo yum update` 或 `sudo dnf upgrade`。这些命令会下载并安装所有可用的更新。

## 4. 安装基本软件和工具

- **安装文本编辑器**：选择适合的文本编辑器（如 vim、nano）以便于配置文件编辑。
- **安装网络工具**：安装基础网络诊断工具（如 curl、wget、net-tools、traceroute）。

## 5. 安全设置

安全性是系统管理中的一个关键方面。以下是一些基本的安全设置步骤。

- **设置防火墙**：`ufw` 和 `firewalld` 是两个流行的防火墙管理工具。它们可以轻松配置入站和出站规则，限制未授权访问。例如，使用 `sudo ufw allow 22` 来允许 SSH 连接。
- **安装和配置安全增强工具**：SELinux 和 AppArmor 提供了强制访问控制，可以增强系统安全。安装后，需要根据需要配置相应的策略。
- **定期扫描漏洞**：使用安全扫描工具定期检查系统漏洞。ClamAV 是一个开源的杀毒工具，可以扫描恶意软件。Lynis 是一个安全审计工具，可以帮助检查系统配置和潜在的安全风险。

## 6. OpenSSH 通讯

OpenSSH 是 Linux 系统中用于远程管理的标准工具。以下是一些重要的 SSH 配置和安全措施。

- **配置 SSH**：安装 OpenSSH 服务后，编辑 `/etc/ssh/sshd_config` 文件进行配置。可以更改默认端口，配置密钥认证，禁用 root 登录等。
- **定期更新密码**：定期更新用户和 root 账户的密码，遵循强密码策略，以增加账户安全性。
- **配置 SSH 密钥认证**：密钥认证比密码认证更安全。生成密钥对，并将其添加到服务器的 `~/.ssh/authorized_keys` 文件中。
- **禁用 SSH 空密码登录**：确保 SSH 配置文件中的 `PermitEmptyPasswords` 选项设置为 `no`，以防止空密码账户通过 SSH 登录。

## 7. 用户账户角色管理

用户账户管理是系统安全的关键部分。合理的用户和权限管理可以防止未授权的访问和潜在的安全风险。

- **添加用户**：使用 `sudo adduser` 或 `sudo useradd` 命令创建新用户。为每个任务创建专用的用户账户，避免使用 root 账户进行日常工作。
- **用户组管理**：合理地使用用户组来分配权限和管理用户权限。使用 `sudo groupadd` 创建新组，使用 `sudo usermod -aG` 将用户添加到组中。
- **root 用户管理**：限制 root 账户的远程访问，只通过 sudo 提升权限。确保 root 账户有一个强密码，并定期更新。
- **用户权限管理**：通过配置 `/etc/sudoers` 文件或使用 `visudo` 命令，为特定用户分配 sudo 权限。确保遵循最小权限原则，只授予必要的权限。
- **重置 root 密码**：如果忘记 root 密码，可以使用系统救援模式或 Live CD/USB 来重置密码。这通常涉及到挂载系统分区并修改 `/etc/shadow` 文件。

## 8. 配置服务和程序

服务和程序的配置对于系统的稳定运行和性能至关重要。

- **管理 systemd 系统服务**：使用 `systemctl` 命令管理服务。了解不同 `systemd` 单元文件的存放位置和优先级，以便正确管理服务。
- **服务自启动管理**：使用 `systemctl enable` 或 `disable` 命令来管理服务是否在系统启动时自动运行。这有助于控制系统启动时的资源消耗。
- **不必要服务的移除**：定期审查系统服务，卸载或禁用不必要的服务和程序，以减少系统的潜在攻击面和提高系统性能。
- **引导至救援模式**：了解如何使用系统引导菜单或添加启动参数进入救援模式。这对于系统修复和故障排除非常有用。

## 9. 日志管理和监控

日志管理和监控对于维护系统安全和稳定运行至关重要。

- **配置系统日志**：使用 `rsyslog` 或 `syslog-ng` 配置日志记录规则。确保重要信息，如安全警告和系统错误，被妥善记录。
- **安装监控工具**：安装 `htop` 和 `iftop` 等工具来监控系统资源使用情况。还可以考虑安装 `nmon`、`glances` 等综合系统监控工具，以获得更全面的系统状态视图。
- **日志分析工具**：安装日志分析工具如 Logwatch 或 GoAccess，这些工具可以帮助更容易地分析和理解日志文件，及时发现潜在的安全威胁或系统问题。

## 10. 备份和恢复计划

备份和恢复计划是防止数据丢失和系统故障的关键。

- **配置定期备份**：使用 `rsync`、`tar` 或定制脚本实现重要数据和配置的定期备份。考虑使用定时任务（cron jobs）来自动执行备份。
- **远程备份**：使用远程备份解决方案，如 `rsync` 配合 SSH 将备份数据同步到远程服务器，提高数据安全性。
- **灾难恢复计划**：制定并测试灾难恢复计划，确保在数据丢失或系统崩溃时能够迅速恢复。这可能包括系统镜像备份、关键数据的恢复流程等。

## 11. 自定义用户环境

为了提高用户的工作效率和满意度，可以对用户环境进行一些自定义设置。

- **磁盘配额**：使用 `quota` 工具为用户和用户组设置磁盘配额，防止单个用户占用过多磁盘空间。
- **使用 `ulimit` 设置资源限制**：使用 `ulimit` 命令为用户和进程设置资源使用限制，如打开文件的数量，以避免单个进程耗尽系统资源。
- **Shell 环境定制**：根据个人偏好定制 Shell 环境，配置别名、环境变量和命令提示符。这可以通过编辑用户的 `.bashrc`、`.bash_profile` 或 `.zshrc` 文件来实现。
- **安装和配置图形界面**（可选）：如果系统需要图形界面，安装桌面环境（如 GNOME、KDE）并进行相应配置。确保图形界面的安全性，例如通过配置 `.xinitrc` 文件来启动图形会话。

## 12. 系统性能优化

系统性能优化可以提高系统响应速度和处理能力，特别是在资源受限的环境中。

- **内核参数调整**：根据系统用途调整 `/etc/sysctl.conf` 中的内核参数。例如，可以增加 `net.core.somaxconn` 来提高网络性能，或者调整 `fs.file-max` 来增加文件系统的最大句柄数。

## 13. 故障转移

故障转移和灾难恢复策略对于确保业务连续性至关重要。

- **故障转移和灾难恢复策略**：制定并实施故障转移和灾难恢复策略。这可能包括设置冗余服务器、配置数据同步、以及制定在发生故障时切换到备份系统的流程。

## 14. 文档和记录

良好的文档和记录习惯对于系统管理和故障排除非常重要。

- **系统配置文档化**：详细记录系统配置和定制过程。这包括网络配置、安全设置、服务管理等，以便于故障排除和系统迁移。
- **变更管理**：记录所有系统变更，包括软件安装、配置修改等。这有助于追踪可能的问题来源，并在必要时回滚更改。
