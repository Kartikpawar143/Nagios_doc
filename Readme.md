# Nagios Core 4.5.13 Installation and Configuration Guide
## Red Hat Enterprise Linux 9.8 (Plow)

**Version:** 1.0

**Nagios Core Version:** 4.5.13

**Nagios Plugins Version:** 2.5

**Operating System:** Red Hat Enterprise Linux 9.8 (Plow)

---

# Table of Contents

1. Introduction
2. About Nagios Core
3. Nagios Architecture
4. Components of Nagios
5. Lab Architecture
6. Hardware Requirements
7. Software Requirements
8. Network Requirements
9. RHEL 9.8 Preparation
10. Registering the System

---

# 1. Introduction

## What is Nagios?

Nagios is one of the most popular open-source infrastructure monitoring solutions used by system administrators and DevOps engineers. It continuously monitors servers, applications, services, databases, and network devices and immediately notifies administrators whenever a problem occurs.

Nagios helps organizations minimize downtime by detecting issues before they become critical. It supports monitoring of Linux servers, Windows servers, cloud infrastructure, virtual machines, routers, switches, printers, storage systems, and many other devices.

Nagios Core is the free, open-source monitoring engine upon which several commercial Nagios products are built.

---

## Why Use Nagios?

Modern IT infrastructures consist of many interconnected systems. Monitoring these systems manually is almost impossible.

Nagios automates monitoring by performing periodic health checks and generating alerts whenever a monitored resource becomes unavailable.

Benefits include:

- Continuous infrastructure monitoring
- Immediate alert notifications
- Reduced downtime
- Centralized monitoring dashboard
- Historical performance reporting
- Plugin-based architecture
- Highly customizable
- Open-source and free

---

## Features of Nagios

- Host monitoring
- Service monitoring
- Network monitoring
- Application monitoring
- Email notifications
- SMS notifications
- Event handlers
- Performance data collection
- Web-based dashboard
- Plugin architecture
- Scalability
- Distributed monitoring

---

## Common Use Cases

Nagios is commonly used to monitor:

- Linux Servers
- Windows Servers
- VMware
- Hyper-V
- Docker
- Kubernetes
- AWS Instances
- Azure Virtual Machines
- Routers
- Switches
- Firewalls
- Printers
- Databases
- Web Servers
- DNS Servers
- Mail Servers
- Storage Devices
- Ceph Clusters

---

# 2. About Nagios Core

Nagios Core is the monitoring engine responsible for executing host and service checks.

It continuously performs health checks by executing plugins at configured intervals.

The results are processed and displayed through the web interface.

Whenever a service changes state (for example, from OK to CRITICAL), Nagios generates notifications based on the configured contact policies.

---

## Nagios Core Responsibilities

- Execute monitoring plugins
- Schedule host checks
- Schedule service checks
- Store monitoring status
- Generate alerts
- Execute event handlers
- Display status through CGI
- Process external commands

---

## Advantages

- Free and Open Source
- Lightweight
- Plugin-based
- Flexible
- Large community
- Enterprise-ready
- Supports thousands of hosts

---

## Limitations

- Initial configuration requires manual setup
- Web interface is basic
- Graphing requires additional tools
- Complex configurations for large environments

---

# 3. Nagios Architecture

Nagios follows a modular architecture.

```
                    +---------------------+
                    |   Administrator     |
                    +----------+----------+
                               |
                               |
                      Web Browser (HTTP)
                               |
                               |
                   +-----------+-----------+
                   |        Apache         |
                   +-----------+-----------+
                               |
                               |
                     Nagios CGI Interface
                               |
                               |
                  +------------+------------+
                  |      Nagios Core        |
                  +------------+------------+
                               |
        ----------------------------------------------------
        |            |            |             |           |
     Plugins      NRPE         SNMP         SSH Checks   Event Handler
        |            |            |             |
        |            |            |             |
   Linux Host   Remote Linux   Routers      Applications
                             Switches
                             Firewalls
```

---

## Workflow

1. Nagios schedules checks.
2. Plugin executes.
3. Plugin returns status.
4. Nagios stores the result.
5. Dashboard updates.
6. Alert generated if required.

---

## Monitoring Cycle

```
Scheduler
    ↓
Plugin Execution
    ↓
Plugin Result
    ↓
Status Processing
    ↓
Notifications
    ↓
Dashboard Update
```

---

# 4. Components of Nagios

Nagios consists of several components working together.

---

## 4.1 Nagios Core

The monitoring engine responsible for scheduling and processing checks.

---

## 4.2 Plugins

Plugins are executable programs that perform monitoring tasks.

Examples:

```
check_ping
check_http
check_disk
check_load
check_users
check_tcp
check_dns
check_ssh
```

---

## 4.3 Web Interface

Provides:

- Dashboard
- Host status
- Service status
- Reports
- Logs
- Configuration status

---

## 4.4 Configuration Files

Stored under:

```
/usr/local/nagios/etc
```

Main files include:

```
nagios.cfg
cgi.cfg
resource.cfg
objects/
```

---

## 4.5 NRPE

NRPE allows Nagios to execute plugins on remote Linux systems.

Example:

```
Nagios Server
      |
      | NRPE
      |
Remote Linux Server
```

---

## 4.6 SNMP

Used for monitoring:

- Routers
- Switches
- Firewalls
- Storage Devices
- UPS
- Printers

---

## 4.7 Apache

Apache hosts the Nagios web interface.

URL:

```
http://server-ip/nagios
```

---

# 5. Lab Architecture

The following lab environment will be used throughout this guide.

```
               Administrator PC
                    Windows 11
                         |
                         |
                  192.168.199.x
                         |
               -------------------
               |                 |
         Nagios Server      Future Linux Hosts
          RHEL 9.8
       192.168.199.128
```

---

## Lab Details

| Component | Value |
|-----------|-------|
| OS | RHEL 9.8 |
| Monitoring Software | Nagios Core 4.5.13 |
| Plugins | Nagios Plugins 2.5 |
| Web Server | Apache |
| PHP | PHP 8 |
| Firewall | Firewalld |
| SELinux | Enforcing |

---

# 6. Hardware Requirements

Minimum requirements

| Component | Minimum |
|------------|----------|
| CPU | 2 Cores |
| RAM | 2 GB |
| Disk | 20 GB |
| Network | 1 Gbps |

---

Recommended

| Component | Recommended |
|------------|--------------|
| CPU | 4 Cores |
| RAM | 8 GB |
| Disk | 100 GB SSD |
| Network | 1 Gbps |

---

Large Enterprise

| Component | Value |
|------------|-------|
| CPU | 8+ Cores |
| RAM | 16 GB |
| Storage | SSD RAID |
| Backup | Daily |

---

# 7. Software Requirements

Operating System

```
Red Hat Enterprise Linux 9.8
```

Required Packages

```
gcc
gcc-c++
make
autoconf
automake
gettext
wget
curl
git
tar
unzip
httpd
php
php-cli
php-gd
gd
gd-devel
openssl
openssl-devel
net-snmp
net-snmp-utils
net-snmp-perl
```

Development Tools

```
GNU Compiler Collection
GNU Make
Autoconf
Automake
```

---

# 8. Network Requirements

Ports Required

| Port | Protocol | Purpose |
|------|----------|----------|
| 80 | TCP | HTTP |
| 443 | TCP | HTTPS |
| 5666 | TCP | NRPE |
| 161 | UDP | SNMP |
| 162 | UDP | SNMP Trap |
| 22 | TCP | SSH |

---

Firewall

```
HTTP
HTTPS
NRPE
SNMP
```

---

DNS

Optional but recommended.

Example

```
nagios.example.com
```

---

# 9. RHEL 9.8 Preparation

Before installing Nagios, verify the operating system.

Check version

```bash
cat /etc/redhat-release
```

Expected Output

```
Red Hat Enterprise Linux release 9.8 (Plow)
```

---

Check Kernel

```bash
uname -r
```

---

Check Architecture

```bash
uname -m
```

Expected

```
x86_64
```

---

Verify Subscription

```bash
subscription-manager status
```

---

Check Enabled Repositories

```bash
dnf repolist
```

Expected repositories

```
BaseOS
AppStream
CodeReady Builder
```

---

Update the System

```bash
sudo dnf update -y
```

---

Reboot if Required

```bash
sudo reboot
```

---

# 10. Registering the System

Before installing Nagios, the RHEL system must be registered with Red Hat Subscription Management.

Check Subscription Status

```bash
subscription-manager status
```

Register the System

```bash
subscription-manager register
```

Attach Subscription Automatically

```bash
subscription-manager attach --auto
```

Enable Required Repositories

```bash
subscription-manager repos \
--enable=rhel-9-for-x86_64-baseos-rpms

subscription-manager repos \
--enable=rhel-9-for-x86_64-appstream-rpms

subscription-manager repos \
--enable=codeready-builder-for-rhel-9-x86_64-rpms
```

Verify

```bash
dnf repolist
```

Expected Output

```
repo id
----------------------------
BaseOS
AppStream
CodeReady Builder
```

---

## End of Part 1

The next chapter begins with:

11. Updating the System
12. Installing Dependencies
13. Explanation of Every Package
14. Creating Nagios User and Groups
15. Downloading Nagios Core
16. Extracting the Source Code
17. Understanding the Source Tree
18. Configuring Nagios
19. Compiling Nagios
20. Installing Nagios
