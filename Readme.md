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
# Part 2A – Nagios Core Installation on RHEL 9.8

---

# 11. Updating the System

## Objective

Before installing any software, always update the operating system to ensure all installed packages are current and any known bugs or security vulnerabilities are fixed.

Updating the system also ensures that the package manager has the latest metadata and dependency information.

---

## Why Update the System?

Updating the operating system provides several benefits:

- Installs latest security patches
- Fixes bugs in existing packages
- Updates dependency information
- Improves package compatibility
- Reduces installation failures
- Ensures newer compiler versions are available

---

## Verify Current OS Version

Before updating, verify the operating system version.

```bash
cat /etc/redhat-release
```

Example Output

```text
Red Hat Enterprise Linux release 9.8 (Plow)
```

---

## Verify Kernel Version

```bash
uname -r
```

Example

```text
5.14.0-570.23.1.el9_8.x86_64
```

---

## Check System Architecture

```bash
uname -m
```

Expected Output

```text
x86_64
```

---

## Refresh Repository Metadata

```bash
sudo dnf clean all
sudo dnf makecache
```

### Explanation

| Command | Description |
|----------|-------------|
| dnf clean all | Removes cached package information |
| dnf makecache | Downloads latest repository metadata |

---

## Update Installed Packages

```bash
sudo dnf update -y
```

### Command Breakdown

| Option | Meaning |
|---------|----------|
| dnf | Package manager |
| update | Update installed packages |
| -y | Automatically answer "Yes" |

---

Example Output

```text
Updating Subscription Management repositories.

Dependencies resolved.

Complete!
```

---

## Verify Updates

```bash
rpm -qa | wc -l
```

Displays the number of installed packages.

---

## Check for Reboot Requirement

```bash
needs-restarting -r
```

Possible outputs

```
No core libraries or services have been updated.
```

or

```
Reboot is required.
```

---

## Reboot the Server

If required

```bash
sudo reboot
```

---

## Verify After Reboot

```bash
uptime
```

```bash
hostnamectl
```

---

## Best Practice

Always reboot after kernel updates before installing production software.

---

# 12. Installing Dependencies

## Objective

Nagios is distributed as source code.

Before compiling Nagios, several development libraries and tools must be installed.

These packages provide:

- Compiler
- Build tools
- Graphics libraries
- SSL libraries
- Apache
- PHP
- SNMP libraries

---

## Install All Required Packages

```bash
sudo dnf install -y \
gcc \
gcc-c++ \
glibc \
glibc-common \
glibc-devel \
make \
gettext \
automake \
autoconf \
wget \
curl \
unzip \
tar \
git \
which \
passwd \
httpd \
httpd-tools \
php \
php-cli \
php-gd \
gd \
gd-devel \
libpng \
libpng-devel \
libjpeg-turbo \
libjpeg-turbo-devel \
openssl \
openssl-devel \
perl \
net-snmp \
net-snmp-utils \
net-snmp-perl \
policycoreutils-python-utils
```

---

## Verify Installation

Compiler

```bash
gcc --version
```

Expected

```
gcc (GCC) 11.x
```

---

Make

```bash
make --version
```

---

Apache

```bash
httpd -v
```

---

PHP

```bash
php -v
```

---

OpenSSL

```bash
openssl version
```

---

SNMP

```bash
snmpwalk --version
```

---

## Verify Important Libraries

```bash
rpm -q \
gd-devel \
libpng-devel \
libjpeg-turbo-devel \
openssl-devel
```

---

Expected

```
gd-devel
libpng-devel
libjpeg-turbo-devel
openssl-devel
```

---

## Common Issues

### Package not found

Example

```
libdbi-devel
```

RHEL 9 does not provide this package.

It is **not required** for Nagios Core.

---

### perl-Net-SNMP

Older documentation mentions

```
perl-Net-SNMP
```

RHEL 9 package name is

```
net-snmp-perl
```

---

## Best Practice

Always install dependencies before downloading Nagios.

---

# 13. Explanation of Every Package

This section explains every package installed during dependency installation.

---

## GCC

```
gcc
```

GNU Compiler Collection.

Purpose:

Compiles Nagios source code into executable binaries.

Without GCC:

```
Compilation will fail.
```

---

## GCC-C++

```
gcc-c++
```

C++ compiler.

Some plugins require C++ support.

---

## glibc

GNU C Standard Library.

Provides:

- printf()
- malloc()
- free()
- system()

Almost every Linux program depends on glibc.

---

## make

```
make
```

Reads the Makefile and compiles software automatically.

Example

```
make all
```

---

## autoconf

Creates the configure script.

Used for source code portability.

---

## automake

Generates Makefile.in.

Works together with autoconf.

---

## gettext

Provides localization support.

Allows software messages in multiple languages.

---

## wget

Downloads source code.

Example

```bash
wget https://example.com/file.tar.gz
```

---

## curl

Transfers data over HTTP, HTTPS, FTP and many protocols.

Useful for API testing and downloads.

---

## git

Version control software.

Not mandatory for Nagios but useful for downloading repositories.

---

## tar

Extracts compressed archives.

Example

```bash
tar -xzf nagios.tar.gz
```

---

## unzip

Extract ZIP archives.

---

## Apache

```
httpd
```

Hosts the Nagios web interface.

Without Apache

```
No Web UI
```

---

## httpd-tools

Contains

```
htpasswd
```

Used to create

```
nagiosadmin
```

web login.

---

## PHP

Runs Nagios web pages.

---

## PHP CLI

Allows PHP execution from terminal.

---

## php-gd

Provides graphics support.

Required for status maps.

---

## GD

Graphics library used for generating images.

---

## GD Development

Provides header files required during compilation.

---

## libpng

PNG image library.

---

## libjpeg

JPEG image support.

---

## OpenSSL

Provides encrypted communication.

---

## OpenSSL Development

Required during compilation.

Provides

```
openssl/ssl.h
```

---

## Perl

Several Nagios plugins are written in Perl.

---

## net-snmp

Provides SNMP utilities.

---

## net-snmp-utils

Commands like

```
snmpwalk
snmpget
snmptranslate
```

---

## net-snmp-perl

Provides Perl SNMP module.

---

## policycoreutils-python-utils

Contains

```
semanage
```

Used for SELinux configuration.

---

# 14. Creating Nagios User and Groups

## Objective

Nagios should never run as the root user.

A dedicated service account improves security and follows Linux best practices.

---

## Create Nagios User

```bash
sudo useradd nagios
```

Verify

```bash
id nagios
```

Example

```
uid=1001(nagios)
```

---

## Create Command Group

```bash
sudo groupadd nagcmd
```

---

## Add Nagios User

```bash
sudo usermod -aG nagcmd nagios
```

---

## Add Apache User

Apache must also access Nagios command files.

```bash
sudo usermod -aG nagcmd apache
```

---

## Verify Groups

```bash
groups nagios
```

Expected

```
nagios nagcmd
```

---

Apache

```bash
groups apache
```

Expected

```
apache nagcmd
```

---

## Why Separate Users?

Benefits

- Improved security
- Better auditing
- Principle of least privilege
- Prevent accidental system damage

---

## Important Files

Nagios files belong to

```
nagios:nagios
```

Command pipe belongs to

```
nagios:nagcmd
```

---

# 15. Downloading Nagios Core

## Objective

Nagios Core is distributed as source code.

We download the latest stable version directly from the official GitHub repository.

---

## Change Directory

```bash
cd /usr/src
```

---

## Why /usr/src?

Linux convention for source code.

Advantages

- Organized
- Writable by root
- Standard build location

---

## Download Nagios Core

```bash
wget -O nagios.tar.gz \
https://github.com/NagiosEnterprises/nagioscore/archive/refs/tags/nagios-4.5.13.tar.gz
```

---

## Verify Download

```bash
ls -lh nagios.tar.gz
```

Example

```
2.5M
```

---

## Verify File Type

```bash
file nagios.tar.gz
```

Expected

```
gzip compressed data
```

---

## Verify SHA (Optional)

```bash
sha256sum nagios.tar.gz
```

Compare with the checksum published by the Nagios project when available.

---

## Why Download from GitHub?

Advantages

- Official source
- Latest stable release
- Easy version management
- Secure HTTPS download

---

## Download Directory Structure

```
/usr/src
│
├── nagios.tar.gz
│
└── (later)
    nagioscore-nagios-4.5.13/
```

---

## Verification Checklist

At this stage verify:

- ✅ System updated
- ✅ Required repositories enabled
- ✅ All dependencies installed
- ✅ Compiler working
- ✅ Apache installed
- ✅ PHP installed
- ✅ Nagios user created
- ✅ nagcmd group created
- ✅ Source code downloaded

---

## End of Part 2A

The next part (2B) covers:

16. Extracting the Source Code  
17. Understanding the Source Tree  
18. Configuring Nagios (`./configure`)  
19. Compiling Nagios (`make all`)  
20. Installing Nagios (`make install`, `make install-init`, `make install-config`, `make install-commandmode`, `make install-webconf`)
