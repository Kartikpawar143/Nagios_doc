# Nagios Core 4.5.13 Installation and Configuration Guide
## Red Hat Enterprise Linux 9.8 (Plow)

**Version:** 1.0

**Nagios Core Version:** 4.5.13

**Nagios Plugins Version:** 2.5

**Operating System:** Red Hat Enterprise Linux 9.8 (Plow)

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
<img width="630" height="711" alt="image" src="https://github.com/user-attachments/assets/31d9e2e9-de70-42d6-8593-705f0381a4e5" />

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
<img width="522" height="355" alt="image" src="https://github.com/user-attachments/assets/a69e4e3e-913a-4e9c-b9a7-a0e40c397af2" />

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
<img width="447" height="86" alt="image" src="https://github.com/user-attachments/assets/b1a426c6-1648-4113-8471-225b3633bf13" />

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

# Part 2B-1 – Extracting, Understanding and Configuring Nagios Core

---

# 16. Extracting the Source Code

## Objective

After downloading the Nagios Core source code archive, the next step is to extract it into a working directory.

The downloaded archive is a compressed **tar.gz** file containing the complete Nagios Core source code.

---

## What is a tar.gz File?

A **tar.gz** file is a compressed archive commonly used in Linux.

It consists of two stages:

```
Source Files
      │
      ▼
 TAR Archive (.tar)
      │
      ▼
 GZIP Compression (.gz)
      │
      ▼
 file.tar.gz
```

The `.tar` package groups files together, while `.gz` compresses the package to reduce download size.

---

## Verify the Download

Navigate to the download directory.

```bash
cd /usr/src
```

List the downloaded file.

```bash
ls -lh
```

Example Output

```
-rw-r--r-- 1 root root 2.5M nagios.tar.gz
```

---

## Check File Type

```bash
file nagios.tar.gz
```

Expected Output

```
nagios.tar.gz: gzip compressed data
```

---

## Extract the Archive

Run:

```bash
tar -xzf nagios.tar.gz
```

### Command Explanation

| Option | Description |
|---------|-------------|
| x | Extract archive |
| z | Use gzip decompression |
| f | Read from file |

---

## Verify Extraction

```bash
ls
```

Example

```
nagios.tar.gz

nagioscore-nagios-4.5.13
```

---

## Change Directory

```bash
cd nagioscore-nagios-4.5.13
```

Verify

```bash
pwd
```

Example

```
/usr/src/nagioscore-nagios-4.5.13
```

---

## Why Extract in /usr/src?

Linux follows the Filesystem Hierarchy Standard (FHS).

```
/
├── bin
├── boot
├── etc
├── home
├── opt
├── usr
│   └── src
│       └── nagioscore-nagios-4.5.13
└── var
```

Advantages

- Standard build location
- Easy cleanup
- Does not interfere with installed applications
- Used by most source installations

---

## Common Errors

### Error

```
tar: Cannot open: No such file
```

Cause

Wrong filename.

Solution

```
ls
```

Verify the exact filename.

---

### Error

```
gzip: stdin: not in gzip format
```

Cause

Corrupted download.

Solution

Download the archive again.

---

## Best Practice

Always verify the downloaded archive before extracting it.

---

# 17. Understanding the Source Tree

## Objective

Nagios is distributed as source code.

Understanding the source directory helps administrators troubleshoot compilation problems and customize installations.

---

## View Directory Structure

Run

```bash
ls
```

You will see folders similar to:

```
base/
cgi/
common/
contrib/
docs/
html/
include/
lib/
module/
sample-config/
startup/
worker/
```

---

## Complete Directory Layout

```
nagioscore-nagios-4.5.13

├── base
├── cgi
├── common
├── contrib
├── docs
├── html
├── include
├── lib
├── module
├── sample-config
├── startup
├── worker
├── configure
├── Makefile.in
└── README
```

---

## Directory Explanation

### base/

Contains the Nagios monitoring engine.

Responsible for

- Scheduling
- Host checks
- Service checks
- Notifications
- Logging

Important files

```
nagios.c
checks.c
events.c
notifications.c
```

---

### cgi/

Contains the web interface programs.

These generate pages like

```
Current Status

Hosts

Services

Reports
```

Example

```
status.cgi

history.cgi

config.cgi
```

---

### common/

Shared code used throughout Nagios.

Includes

- Objects
- Commands
- Logging
- Comments

---

### html/

Contains

```
CSS

Images

JavaScript

PHP
```

Everything displayed inside the browser comes from this directory.

---

### include/

Contains

```
Header Files
```

Examples

```
config.h

common.h

objects.h
```

---

### lib/

Libraries used by Nagios.

Provides

```
Networking

Queues

Memory

Utilities
```

---

### module/

Contains Event Broker Modules.

Allows third-party integrations.

Examples

```
Livestatus

NDOUtils

Broker Modules
```

---

### startup/

Contains

```
systemd Service

Startup Scripts
```

This directory installs

```
nagios.service
```

---

### sample-config/

Contains example configuration files.

Examples

```
nagios.cfg

cgi.cfg

localhost.cfg

contacts.cfg

commands.cfg
```

These become the default configuration after installation.

---

### worker/

Contains worker processes.

Used for

- Ping checks
- Worker scheduling
- Parallel execution

---

## Important Files

### configure

```
./configure
```

Checks your Linux system.

Generates

```
Makefile
```

---

### Makefile

Contains instructions for

```
Compilation

Installation

Cleaning
```

---

### README

General information.

Always read before compiling software.

---

## Relationship Between Directories

```
configure
     │
     ▼
Makefile
     │
     ▼
Compilation
     │
     ▼
base/
cgi/
html/
worker/
```

---

## Source Code Flow

```
Source Code
      │
      ▼
configure
      │
      ▼
Makefile
      │
      ▼
make all
      │
      ▼
Executable Files
      │
      ▼
Installation
```

---

# 18. Configuring Nagios

## Objective

Before compiling Nagios, the system must be checked for required libraries, compilers and dependencies.

This process is performed using the **configure** script.

---

## What is configure?

The configure script automatically checks

- Compiler
- Libraries
- Header files
- Operating System
- SSL
- Apache
- GD Library
- User Accounts

After successful verification, it creates a customized **Makefile**.

---

## Run Configure

Execute

```bash
./configure \
--with-command-group=nagcmd
```

---

## Command Breakdown

```
./configure
```

Runs the configuration script.

---

```
--with-command-group=nagcmd
```

Specifies the group allowed to execute external Nagios commands.

---

## What Happens Internally?

The configure script checks

```
Compiler

Header Files

Libraries

SSL

GD

System Type

Apache

Perl

Systemd

Socket Support

Shared Libraries
```

---

Example Flow

```
Start

↓

Check Compiler

↓

Check Libraries

↓

Check Apache

↓

Check SSL

↓

Check GD

↓

Generate Makefile

↓

Configuration Complete
```

---

## Successful Output

Near the end you should see

```
*** Configuration summary for Nagios ***
```

Example

```
Nagios executable

Nagios user

Command group

Install directory

Apache directory

HTML URL

CGI URL
```

---

## Verify Generated Files

Run

```bash
ls
```

You should now see

```
Makefile

config.status

config.log
```

---

## Understanding Generated Files

### Makefile

Used by

```
make
```

Contains every compilation instruction.

---

### config.log

Contains detailed information.

Useful when configure fails.

---

### config.status

Records configuration settings.

Used to regenerate files.

---

## Common Warnings

### Warning

```
Kerberos include files not found
```

Usually safe to ignore.

---

### Warning

```
mail not found
```

Install

```
mailx
```

if email notifications are required.

---

### GD Library Warning

```
checking for GD library...
```

Must end with

```
yes
```

Otherwise install

```
gd-devel
```

---

### OpenSSL

Should display

```
checking whether compiling and linking against SSL works...

yes
```

---

## Common Errors

### Error

```
C compiler cannot create executables
```

Cause

```
gcc missing
```

Fix

```bash
sudo dnf install gcc
```

---

### Error

```
GD library not found
```

Fix

```bash
sudo dnf install gd-devel
```

---

### Error

```
OpenSSL headers missing
```

Fix

```bash
sudo dnf install openssl-devel
```

---

## Verification Checklist

Before moving to compilation, verify:

- ✅ configure completed successfully
- ✅ Makefile exists
- ✅ config.log exists
- ✅ config.status exists
- ✅ Configuration summary displayed
- ✅ No fatal errors reported

---

## End of Part 2B-1

# Part 2B-2 – Compiling and Installing Nagios Core

---

# 19. Compiling Nagios Core

## Objective

After successfully running the `configure` script, the next step is to compile the Nagios source code.

Compilation converts the C source files into executable binaries that Linux can execute.

During compilation, the GNU Compiler Collection (GCC) translates thousands of lines of C source code into machine language.

---

# What is Compilation?

Compilation is the process of converting human-readable source code into executable binary files.

```
Source Code (.c)
        │
        ▼
      GCC Compiler
        │
        ▼
 Object Files (.o)
        │
        ▼
     Linker (ld)
        │
        ▼
 Executable Binary
```

Without compilation, the Linux operating system cannot execute the Nagios program.

---

# Compilation Workflow

```
configure
      │
      ▼
 Generate Makefile
      │
      ▼
 make all
      │
      ▼
 GCC Compiler
      │
      ▼
 Object Files
      │
      ▼
 Executables
```

---

# Verify Current Directory

Before compiling, ensure you are inside the Nagios source directory.

```bash
pwd
```

Expected Output

```
/usr/src/nagioscore-nagios-4.5.13
```

---

# Verify Makefile Exists

```bash
ls Makefile
```

Expected

```
Makefile
```

If the Makefile is missing, the configure step did not complete successfully.

---

# Start Compilation

Execute

```bash
make all
```

---

# What Does "make all" Do?

The `make` utility reads instructions from the Makefile generated during the configure step.

It automatically compiles every required source file in the correct order.

Internally, the following components are compiled:

```
base/
cgi/
html/
module/
worker/
lib/
```

---

# Compilation Process

```
make all

↓

Compile base/

↓

Compile cgi/

↓

Compile html/

↓

Compile module/

↓

Compile worker/

↓

Link Libraries

↓

Create Executable

↓

Compilation Complete
```

---

# Example Output

```
cd ./base && make

gcc -o nagios

cd ./cgi && make

gcc -o status.cgi

gcc -o history.cgi

cd ./html && make

cd ./worker && make

*** Compile finished ***
```

---

# What Gets Created?

Several executable programs are produced.

Examples

```
nagios

nagiostats

status.cgi

history.cgi

config.cgi

cmd.cgi

summary.cgi
```

---

# Understanding Compilation Messages

During compilation, many messages are displayed.

Example

```
gcc -Wall -g -O2
```

Explanation

| Option | Description |
|----------|-------------|
| gcc | GNU Compiler |
| -Wall | Enable compiler warnings |
| -g | Include debugging symbols |
| -O2 | Optimization level |

---

# Compiler Warnings

Example

```
warning:
```

Warnings do not stop compilation.

Compilation is considered successful if it reaches

```
*** Compile finished ***
```

---

# Compiler Errors

Errors begin with

```
error:
```

Compilation immediately stops.

Example

```
fatal error:

openssl/ssl.h

No such file
```

Solution

```
sudo dnf install openssl-devel
```

---

# Another Common Error

```
gd.h

No such file
```

Solution

```
sudo dnf install gd-devel
```

---

# Verify Compilation

After compilation completes successfully, verify that the executable exists.

```bash
ls base/nagios
```

Expected

```
base/nagios
```

---

# Verify Binary Type

```bash
file base/nagios
```

Expected

```
ELF 64-bit executable
```

---

# Testing Before Installation

Nagios provides a built-in test suite.

Execute

```bash
make test
```

If everything passes, proceed with installation.

---

# Best Practices

✔ Never ignore compiler errors

✔ Warnings are acceptable

✔ Errors must be fixed before installation

---

# 20. Installing Nagios Core

## Objective

Compilation creates executable files.

Installation copies these compiled files into their proper locations on the operating system.

Until installation is performed, Nagios cannot be used.

---

# Installation Workflow

```
Compiled Files

↓

Copy Files

↓

Create Directories

↓

Install Configuration

↓

Install Service

↓

Install Apache Configuration

↓

Ready to Run
```

---

# Install Main Program

Execute

```bash
make install
```

---

## What Does It Install?

Copies

```
Nagios Binary

CGI Programs

HTML Files

Images

Libraries
```

into

```
/usr/local/nagios
```

---

# Directory Structure After Installation

```
/usr/local/nagios

├── bin
├── etc
├── libexec
├── sbin
├── share
├── var
└── include
```

---

# Install Systemd Service

```bash
make install-init
```

Purpose

Creates

```
/lib/systemd/system/nagios.service
```

---

# Verify

```bash
ls /lib/systemd/system/nagios.service
```

Expected

```
nagios.service
```

---

# Install Configuration Files

```bash
make install-config
```

Copies sample configuration files into

```
/usr/local/nagios/etc
```

---

# Installed Files

```
nagios.cfg

cgi.cfg

resource.cfg

objects/
```

---

# Verify

```bash
ls /usr/local/nagios/etc
```

---

# Install Command Mode

```bash
make install-commandmode
```

Purpose

Creates

```
/usr/local/nagios/var/rw
```

This directory allows the web interface to send commands to the Nagios daemon.

Examples

```
Schedule Downtime

Acknowledge Problem

Disable Notifications

Enable Notifications
```

---

# Verify

```bash
ls -ld /usr/local/nagios/var/rw
```

Expected

```
nagios nagcmd
```

---

# Install Apache Configuration

```bash
make install-webconf
```

Purpose

Creates

```
/etc/httpd/conf.d/nagios.conf
```

---

# Verify

```bash
ls /etc/httpd/conf.d/nagios.conf
```

---

# Install Sequence Summary

```
make install

↓

make install-init

↓

make install-config

↓

make install-commandmode

↓

make install-webconf
```

---

# Understanding Each Command

| Command | Purpose |
|----------|----------|
| make install | Install binaries and HTML files |
| make install-init | Install systemd service |
| make install-config | Install configuration files |
| make install-commandmode | Configure command pipe |
| make install-webconf | Configure Apache |

---

# Verify Installation Directories

```bash
tree -L 2 /usr/local/nagios
```

Expected

```
bin/

etc/

libexec/

sbin/

share/

var/
```

---

# Verify Ownership

```bash
ls -ld /usr/local/nagios
```

Expected

```
nagios nagios
```

---

# Verify Installed Binary

```bash
/usr/local/nagios/bin/nagios --help
```

Expected

Nagios command-line help should be displayed.

---

# Verify Version

```bash
/usr/local/nagios/bin/nagios -V
```

Expected

```
Nagios Core 4.5.13
```

---

# Common Installation Errors

## Permission Denied

```
Permission denied
```

Cause

Not running as root.

Solution

```
sudo
```

or login as

```
root
```

---

## Missing Directory

```
No such file or directory
```

Cause

Compilation did not complete.

Solution

Run

```
make all
```

again.

---

## Apache Configuration Missing

```
/etc/httpd/conf.d/nagios.conf
```

Solution

```
make install-webconf
```

---

## Nagios Service Missing

```
Unit nagios.service not found
```

Solution

```
make install-init
systemctl daemon-reload
```

---

# Installation Verification Checklist

Verify the following:

✅ Nagios binary installed

```bash
ls /usr/local/nagios/bin
```

---

✅ Configuration files installed

```bash
ls /usr/local/nagios/etc
```

---

✅ Apache configuration installed

```bash
ls /etc/httpd/conf.d/nagios.conf
```

---

✅ Service file installed

```bash
ls /lib/systemd/system/nagios.service
```

---

✅ Version check

```bash
/usr/local/nagios/bin/nagios -V
```

---

# Installation Flow Diagram

```
Download Source

↓

Extract Archive

↓

Configure

↓

Compile

↓

Install Binary

↓

Install Service

↓

Install Config

↓

Install Apache

↓

Ready for Plugins
```

---

# End of Part 2B-2

# Part 3A – Installing Nagios Plugins

---

# 21. Installing Nagios Plugins

## Objective

Nagios Core itself is only a monitoring engine.

It **does not directly monitor anything**.

The actual monitoring is performed by **Nagios Plugins**.

Without plugins, Nagios can schedule checks, but it cannot determine whether a host or service is healthy.

Therefore, immediately after installing Nagios Core, the next step is to install the official Nagios Plugins package.

---

# What are Nagios Plugins?

Nagios Plugins are small executable programs that perform monitoring tasks.

Each plugin checks a particular resource and returns one of four standard status codes.

```
                Nagios Core
                     │
                     │ Executes
                     ▼
            Nagios Plugin
                     │
                     │ Checks
                     ▼
          Host / Service Status
                     │
                     ▼
           Status Returned to Core
```

---

## Plugin Return Codes

Every plugin returns one of the following values.

| Return Code | Status | Description |
|-------------|--------|-------------|
| 0 | OK | Everything is working normally |
| 1 | WARNING | Resource is above warning threshold |
| 2 | CRITICAL | Serious issue detected |
| 3 | UNKNOWN | Plugin could not determine status |

Example

```
check_ping

↓

PING OK

↓

Exit Code = 0
```

---

# Common Plugins

Nagios Plugins package installs many useful monitoring commands.

Examples include:

```
check_ping

check_http

check_disk

check_load

check_users

check_tcp

check_dns

check_procs

check_swap

check_ssh

check_snmp

check_ntp

check_icmp

check_ftp

check_smtp

check_pop

check_imap

check_tcp

check_udp
```

---

## Where are Plugins Installed?

Default installation directory

```
/usr/local/nagios/libexec
```

Example

```
/usr/local/nagios/libexec/check_ping

/usr/local/nagios/libexec/check_http

/usr/local/nagios/libexec/check_disk
```

---

## Plugin Workflow

```
Nagios Scheduler

↓

Execute Plugin

↓

Plugin Checks Resource

↓

Plugin Returns Status

↓

Nagios Updates Dashboard
```

---

# Why Install Plugins Separately?

Nagios follows a modular architecture.

Advantages

- Easy maintenance
- Easy upgrades
- Custom plugins supported
- Lightweight core
- Flexible monitoring

---

# Official Plugin Package

Current Version

```
Nagios Plugins 2.5
```

Official GitHub Repository

```
https://github.com/nagios-plugins/nagios-plugins
```

---

# Change to Source Directory

```bash
cd /usr/src
```

Verify

```bash
pwd
```

Expected

```
/usr/src
```

---

# Download Plugins

```bash
wget -O plugins.tar.gz \
https://github.com/nagios-plugins/nagios-plugins/releases/download/release-2.5/nagios-plugins-2.5.tar.gz
```

---

# Verify Download

```bash
ls -lh plugins.tar.gz
```

Expected

```
2.6M
```

---

# Verify File Type

```bash
file plugins.tar.gz
```

Expected

```
gzip compressed data
```

---

## Download Directory

```
/usr/src

├── nagios.tar.gz

├── plugins.tar.gz
```

---

# Best Practices

Always download plugins from the official GitHub repository.

Avoid third-party mirrors.

---

# 22. Understanding Nagios Plugins

## Plugin Architecture

```
             Nagios Core
                  │
                  ▼
            check_ping
                  │
                  ▼
          ICMP Echo Request
                  │
                  ▼
              Remote Host
                  │
                  ▼
           ICMP Echo Reply
                  │
                  ▼
              Plugin Result
                  │
                  ▼
             Nagios Dashboard
```

---

## Plugin Categories

### Host Plugins

Examples

```
check_ping

check_icmp

check_ssh

check_tcp
```

---

### Service Plugins

Examples

```
check_http

check_dns

check_mysql

check_ldap

check_ftp
```

---

### Resource Plugins

Examples

```
check_disk

check_load

check_users

check_swap

check_procs
```

---

### Network Plugins

Examples

```
check_snmp

check_tcp

check_udp

check_ntp
```

---

## Plugin Naming Convention

```
check_*
```

Examples

```
check_http

check_dns

check_disk

check_swap
```

---

## Plugin Execution

Nagios executes plugins exactly like Linux commands.

Example

```bash
/usr/local/nagios/libexec/check_ping \
-H 8.8.8.8
```

Possible Output

```
PING OK

Packet Loss = 0%
```

---

## Plugin Exit Status

```
Exit Code

↓

0

↓

OK
```

---

Example

```
check_disk

↓

Disk Usage = 95%

↓

Exit Code = 2

↓

CRITICAL
```

---

## Custom Plugins

Administrators can also write their own plugins.

Supported Languages

```
Bash

Python

Perl

C

Go

Ruby
```

---

# 23. Compiling Nagios Plugins

## Objective

After downloading the source code, plugins must be compiled.

Compilation converts the plugin source code into executable binaries.

---

# Extract Plugins

```bash
tar -xzf plugins.tar.gz
```

---

Verify

```bash
ls
```

Expected

```
nagios-plugins-2.5
```

---

Enter Directory

```bash
cd nagios-plugins-2.5
```

---

Verify

```bash
pwd
```

Expected

```
/usr/src/nagios-plugins-2.5
```

---

## Configure Plugins

Execute

```bash
./configure \
--with-nagios-user=nagios \
--with-nagios-group=nagios
```

---

### Command Explanation

| Option | Description |
|----------|-------------|
| --with-nagios-user | Plugin ownership |
| --with-nagios-group | Plugin group |

---

## Why No ./tools/setup?

Older documentation sometimes includes:

```bash
./tools/setup
```

This step is **obsolete** for **Nagios Plugins 2.5** and should **not** be used.

The `configure` script performs the required setup.

---

## Verify Configure

Expected Output

```
Configuration completed.
```

Near the end

```
config.status

Makefile
```

---

## Compile Plugins

```bash
make
```

Compilation begins.

```
gcc

↓

Object Files

↓

Executable Plugins
```

---

## Example Output

```
Making all in plugins

gcc

check_ping

check_http

check_disk

Compile finished
```

---

## Common Errors

### Error

```
No acceptable C compiler found
```

Solution

```bash
sudo dnf install gcc
```

---

### Error

```
openssl headers missing
```

Solution

```bash
sudo dnf install openssl-devel
```

---

### Error

```
configure: command not found
```

Ensure you are inside

```
nagios-plugins-2.5
```

---

# 24. Installing Nagios Plugins

## Install Plugins

```bash
make install
```

---

## Installation Process

```
Compiled Plugins

↓

Copy to

↓

/usr/local/nagios/libexec
```

---

Verify

```bash
ls /usr/local/nagios/libexec
```

You should see many plugin executables.

Examples

```
check_ping

check_http

check_disk

check_load

check_swap

check_users

check_snmp

check_tcp

check_ssh
```

---

## Installation Layout

```
/usr/local/nagios

├── bin

├── etc

├── libexec

│     ├── check_ping

│     ├── check_http

│     ├── check_disk

│     ├── check_load

│     ├── check_tcp

│     └── ...

├── share

├── var
```

---

# 25. Verifying Plugin Installation

## Objective

After installation, verify that plugins were correctly installed.

---

## Count Installed Plugins

```bash
ls /usr/local/nagios/libexec | wc -l
```

Typically

```
60+
```

plugins.

---

## Verify Individual Plugin

Example

```bash
ls -l /usr/local/nagios/libexec/check_ping
```

Expected

```
Executable
```

---

## Check Permissions

```bash
ls -l /usr/local/nagios/libexec
```

Owner

```
nagios

nagios
```

---

## Run Plugin Manually

Example

```bash
/usr/local/nagios/libexec/check_ping \
-H 8.8.8.8
```

Expected

```
PING OK
```

---

## Check HTTP Plugin

```bash
/usr/local/nagios/libexec/check_http \
-H google.com
```

Expected

```
HTTP OK
```

---

## Check Disk Plugin

```bash
/usr/local/nagios/libexec/check_disk \
-w 20% \
-c 10% \
-p /
```

Expected

```
DISK OK
```

---

## Plugin Verification Checklist

Verify

- ✅ Plugins downloaded
- ✅ Plugins extracted
- ✅ Configure completed
- ✅ Compilation successful
- ✅ Installation successful
- ✅ Plugins copied to libexec
- ✅ Plugins executable
- ✅ Manual execution successful

---

## Common Troubleshooting

### Permission denied

```bash
chmod +x plugin_name
```

---

### Plugin not found

Verify installation directory.

```
/usr/local/nagios/libexec
```

---

### Plugin returns UNKNOWN

Run plugin manually to view the exact error message.

---

## End of Part 3A

# Part 3B-1 – Web Authentication, Apache Configuration and SELinux

---

# 26. Creating the Web Authentication User

## Objective

Nagios uses **Apache Basic Authentication** to protect the web interface.

Without authentication, anyone who knows the server IP address could access your monitoring dashboard.

For this reason, Nagios requires a valid username and password before allowing access.

---

# Authentication Workflow

```
                User Browser
                     │
                     ▼
            http://server/nagios
                     │
                     ▼
              Apache Web Server
                     │
        Is User Authenticated?
              │             │
             No             Yes
              │              │
              ▼              ▼
 Authentication Popup   Nagios Dashboard
              │
              ▼
      Verify Username
              │
              ▼
      Verify Password
              │
              ▼
       htpasswd.users File
```

---

## What is Basic Authentication?

Basic Authentication is an HTTP authentication mechanism provided by Apache.

When a user visits the Nagios URL:

```
http://<server-ip>/nagios
```

Apache immediately prompts for a username and password.

Only authenticated users can access the Nagios dashboard.

---

## Authentication File

Nagios stores usernames and encrypted passwords in:

```
/usr/local/nagios/etc/htpasswd.users
```

Passwords are **never stored in plain text**.

Example

```
nagiosadmin:$apr1$g2u4...
```

---

# Why Use htpasswd?

Apache provides the `htpasswd` utility to securely create and manage authentication users.

It automatically hashes passwords using secure algorithms.

---

## Verify htpasswd

```bash
which htpasswd
```

Expected

```
/usr/bin/htpasswd
```

If not found

```bash
sudo dnf install httpd-tools
```

---

# Create Web User

Execute

```bash
htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
```

---

## Command Explanation

| Option | Meaning |
|----------|---------|
| -c | Create new password file |
| htpasswd.users | Password database |
| nagiosadmin | Username |

---

Example

```
New password:

Re-type new password:

Adding password for user nagiosadmin
```

---

# Verify User

```bash
cat /usr/local/nagios/etc/htpasswd.users
```

Example

```
nagiosadmin:$apr1$.....
```

---

## Add Another User

Do **NOT** use `-c`.

Instead

```bash
htpasswd /usr/local/nagios/etc/htpasswd.users operator
```

Using `-c` again deletes existing users.

---

## Change Password

```bash
htpasswd /usr/local/nagios/etc/htpasswd.users nagiosadmin
```

---

## Delete User

```bash
htpasswd -D \
/usr/local/nagios/etc/htpasswd.users \
nagiosadmin
```

---

## Authentication File Permissions

Verify

```bash
ls -l /usr/local/nagios/etc/htpasswd.users
```

Expected

```
-rw-r-----
```

---

# Best Practice

Create individual accounts for administrators instead of sharing one account.

Example

```
admin

operator

network

storage

linux
```

---

# 27. Configuring Apache

## Objective

Apache provides the web interface for Nagios.

Without Apache, the dashboard cannot be accessed through a web browser.

---

# Apache Architecture

```
Browser

↓

Apache HTTP Server

↓

Nagios CGI

↓

Nagios Core

↓

Status Information
```

---

## Apache Configuration File

Installed during

```
make install-webconf
```

Location

```
/etc/httpd/conf.d/nagios.conf
```

---

## Verify Configuration File

```bash
ls /etc/httpd/conf.d/nagios.conf
```

---

## View Configuration

```bash
less /etc/httpd/conf.d/nagios.conf
```

---

## Important Directives

### ScriptAlias

```
ScriptAlias /nagios/cgi-bin
```

Defines CGI directory.

---

### Alias

```
Alias /nagios
```

Defines web interface directory.

---

### AuthUserFile

```
AuthUserFile /usr/local/nagios/etc/htpasswd.users
```

Specifies the authentication database.

---

### AuthType

```
AuthType Basic
```

Enables HTTP Basic Authentication.

---

### Require

```
Require valid-user
```

Only authenticated users may access Nagios.

---

# Verify Apache Configuration

Execute

```bash
apachectl configtest
```

Expected

```
Syntax OK
```

---

# If Syntax Fails

Example

```
Syntax error

Line 45
```

Correct the configuration before restarting Apache.

---

# Reload Apache Configuration

```bash
systemctl reload httpd
```

---

# Restart Apache

```bash
systemctl restart httpd
```

---

# Enable Apache at Boot

```bash
systemctl enable httpd
```

---

# Verify Apache Status

```bash
systemctl status httpd
```

Expected

```
Active: active (running)
```

---

# Verify Listening Port

```bash
ss -tlnp | grep :80
```

Expected

```
LISTEN

0.0.0.0:80
```

---

# Test Web Server

Open browser

```
http://server-ip
```

Apache test page should appear.

---

# Common Apache Problems

## Port Already In Use

```
Address already in use
```

Find process

```bash
ss -tlnp | grep :80
```

---

## Syntax Error

```
apachectl configtest
```

Always returns

```
Syntax OK
```

before restarting Apache.

---

## Authentication Loop

Browser repeatedly asks for password.

Check

```
AuthUserFile

htpasswd.users

Permissions
```

---

# Apache Workflow

```
Client Request

↓

Apache

↓

Authenticate User

↓

Nagios CGI

↓

Nagios Core

↓

Dashboard
```

---

# 28. Configuring SELinux

## Objective

SELinux (Security-Enhanced Linux) provides mandatory access control for Linux systems.

RHEL enables SELinux by default.

Improper SELinux configuration can prevent Apache and Nagios from functioning correctly.

---

# Verify SELinux Mode

```bash
getenforce
```

Possible Results

```
Enforcing

Permissive

Disabled
```

Recommended

```
Enforcing
```

---

# Check Detailed Status

```bash
sestatus
```

Example

```
SELinux status: enabled

Current mode: enforcing
```

---

# Why Configure SELinux?

Nagios requires Apache to communicate with several directories.

Without proper SELinux policies

```
Permission denied
```

errors may occur.

---

# Allow Apache Network Connections

```bash
setsebool -P httpd_can_network_connect 1
```

---

## Command Explanation

| Option | Meaning |
|----------|----------|
| setsebool | Change SELinux Boolean |
| -P | Permanent |
| httpd_can_network_connect | Allow Apache outbound connections |

---

# Verify Boolean

```bash
getsebool httpd_can_network_connect
```

Expected

```
on
```

---

# Restore Security Contexts

```bash
restorecon -Rv /usr/local/nagios
```

---

Restore Apache

```bash
restorecon -Rv /etc/httpd
```

---

# Verify Context

```bash
ls -Zd /usr/local/nagios
```

Displays

```
SELinux Context
```

---

# Common SELinux Commands

View Booleans

```bash
getsebool -a
```

---

Temporarily Disable (Testing Only)

```bash
setenforce 0
```

---

Enable Again

```bash
setenforce 1
```

---

⚠ **Never disable SELinux permanently on production servers.**

Always resolve policy issues instead.

---

# Troubleshooting SELinux

## Apache Forbidden

If browser displays

```
403 Forbidden
```

Verify

```
getenforce

restorecon

httpd_can_network_connect
```

---

## Audit Logs

```bash
ausearch -m avc -ts recent
```

Displays recent SELinux denials.

---

## Best Practices

✔ Keep SELinux enabled

✔ Use SELinux Booleans instead of disabling SELinux

✔ Restore contexts after installation

✔ Review audit logs if access is denied

---

# Verification Checklist

Verify

✅ Web user created

```bash
cat /usr/local/nagios/etc/htpasswd.users
```

---

✅ Apache configuration exists

```bash
ls /etc/httpd/conf.d/nagios.conf
```

---

✅ Apache syntax valid

```bash
apachectl configtest
```

---

✅ Apache running

```bash
systemctl status httpd
```

---

✅ SELinux enabled

```bash
getenforce
```

---

✅ httpd_can_network_connect enabled

```bash
getsebool httpd_can_network_connect
```

---

## End of Part 3B-1

# Part 3B-2 – Firewalld Configuration, Starting Nagios Services, Verification and First Login

---

# 29. Configuring Firewalld

## Objective

Firewalld is the default firewall management service in Red Hat Enterprise Linux 9.

Even if Nagios and Apache are installed correctly, users cannot access the web interface unless HTTP or HTTPS traffic is allowed through the firewall.

This section explains how to configure Firewalld securely for Nagios.

---

# What is Firewalld?

Firewalld is a dynamic firewall manager that uses zones to define trust levels for network connections.

Unlike traditional iptables, Firewalld allows firewall rules to be added or removed without restarting the firewall service.

---

## Firewalld Architecture

```
                Internet
                    │
                    ▼
            Firewalld Service
                    │
        -------------------------
        │                       │
     Allow                 Block
        │                       │
        ▼                       ▼
   Apache (80/443)      Unauthorized Traffic
```

---

# Check Firewalld Status

```bash
systemctl status firewalld
```

Expected Output

```
Active: active (running)
```

---

# Start Firewalld

If Firewalld is not running:

```bash
systemctl start firewalld
```

---

# Enable Firewalld at Boot

```bash
systemctl enable firewalld
```

---

# Check Current Firewall Rules

```bash
firewall-cmd --list-all
```

Example Output

```
services:
ssh dhcpv6-client
```

---

# Allow HTTP

```bash
firewall-cmd --permanent --add-service=http
```

---

## Command Explanation

| Option | Meaning |
|----------|----------|
| --permanent | Save rule permanently |
| --add-service | Add predefined service |
| http | Open TCP Port 80 |

---

# Allow HTTPS

```bash
firewall-cmd --permanent --add-service=https
```

---

# Reload Firewall

```bash
firewall-cmd --reload
```

Expected Output

```
success
```

---

# Verify Configuration

```bash
firewall-cmd --list-services
```

Expected

```
cockpit dhcpv6-client http https ssh
```

---

# Open NRPE Port (Optional)

If monitoring remote Linux servers using NRPE:

```bash
firewall-cmd --permanent --add-port=5666/tcp
```

Reload

```bash
firewall-cmd --reload
```

---

# Open SNMP (Optional)

```bash
firewall-cmd --permanent --add-service=snmp
```

Reload

```bash
firewall-cmd --reload
```

---

# Remove a Rule

```bash
firewall-cmd --permanent --remove-service=http
```

Reload

```bash
firewall-cmd --reload
```

---

# Verify Listening Ports

```bash
ss -tlnp
```

Expected

```
80

443
```

---

# Common Firewalld Commands

View Zones

```bash
firewall-cmd --get-active-zones
```

---

View Default Zone

```bash
firewall-cmd --get-default-zone
```

---

List All Rules

```bash
firewall-cmd --list-all
```

---

# Common Problems

## Browser Cannot Connect

Check

```
firewalld running

HTTP allowed

Apache running
```

---

## Firewall Not Running

Start

```bash
systemctl start firewalld
```

---

## Rule Missing After Reboot

Always use

```
--permanent
```

followed by

```
firewall-cmd --reload
```

---

# Best Practices

✔ Keep Firewalld enabled

✔ Allow only required services

✔ Remove unused ports

✔ Use predefined services whenever possible

---

# 30. Starting Nagios Services

## Objective

After installation, Nagios and Apache must be started and enabled so they automatically start after every system reboot.

---

# Reload Systemd

Whenever a new service file is installed:

```bash
systemctl daemon-reload
```

---

# Enable Nagios

```bash
systemctl enable nagios
```

Expected

```
Created symlink...
```

---

# Enable Apache

```bash
systemctl enable httpd
```

---

# Start Apache

```bash
systemctl start httpd
```

---

# Start Nagios

```bash
systemctl start nagios
```

---

# Restart Both Services

```bash
systemctl restart httpd
systemctl restart nagios
```

---

# Check Apache Status

```bash
systemctl status httpd
```

Expected

```
Active: active (running)
```

---

# Check Nagios Status

```bash
systemctl status nagios
```

Expected

```
Active: active (running)
```

---

# Verify Service Startup

```bash
systemctl is-enabled httpd
```

Expected

```
enabled
```

---

```bash
systemctl is-enabled nagios
```

Expected

```
enabled
```

---

# Verify Nagios Configuration

Before starting Nagios, always validate the configuration.

```bash
/usr/local/nagios/bin/nagios \
-v \
/usr/local/nagios/etc/nagios.cfg
```

---

## Expected Output

```
Things look okay.

No serious problems were detected.
```

If errors are reported, fix them before starting the service.

---

# Understanding the Verification Process

Nagios checks:

- Main configuration file
- Object definitions
- Host definitions
- Service definitions
- Contact definitions
- Command definitions
- Time periods
- File permissions

---

# Verify Running Process

Nagios

```bash
ps -ef | grep nagios
```

Apache

```bash
ps -ef | grep httpd
```

---

# Verify Listening Port

```bash
ss -tlnp | grep :80
```

Expected

```
LISTEN
0.0.0.0:80
```

---

# Verify Nagios Binary

```bash
/usr/local/nagios/bin/nagios -V
```

Expected

```
Nagios Core 4.5.13
```

---

# First Login

Open a browser.

```
http://SERVER-IP/nagios
```

Example

```
http://192.168.199.128/nagios
```

---

# Authentication Window

The browser displays an authentication popup.

Enter:

```
Username

nagiosadmin
```

Password

```
Your Password
```

---

# Dashboard

After successful authentication, the Nagios home page appears.

You should see:

```
Nagios Core 4.5.13

Process Information

Current Status

Service Status

Host Status
```

---

# Verify Localhost Monitoring

Navigate to

```
Current Status

↓

Hosts
```

Expected

```
localhost

UP
```

---

Navigate to

```
Current Status

↓

Services
```

Example

```
Current Load

Current Users

Disk Usage

HTTP

PING

Swap Usage
```

---

# Understanding Default Checks

Nagios installs sample monitoring for localhost.

These checks include:

| Service | Description |
|----------|-------------|
| Current Load | CPU load |
| Current Users | Logged-in users |
| Disk Usage | Root filesystem |
| HTTP | Apache availability |
| PING | ICMP reachability |
| Total Processes | Running processes |
| Swap Usage | Swap utilization |

---

# Common Startup Problems

## Nagios Service Fails

Run

```bash
journalctl -u nagios
```

---

## Apache Fails

```bash
journalctl -u httpd
```

---

## Browser Shows 403 Forbidden

Verify

```
SELinux

Apache Configuration

Permissions
```

---

## Authentication Keeps Appearing

Verify

```
htpasswd.users

AuthUserFile

Apache Restart
```

---

## Browser Cannot Connect

Check

```
Apache running

Firewall

Correct IP Address
```

---

## Configuration Error

Always verify before restarting.

```bash
/usr/local/nagios/bin/nagios \
-v \
/usr/local/nagios/etc/nagios.cfg
```

---

# Installation Verification Checklist

Verify:

✅ Apache installed

✅ Nagios installed

✅ Plugins installed

✅ Firewall configured

✅ SELinux configured

✅ Services running

✅ Configuration valid

✅ Browser accessible

✅ Login successful

✅ Localhost monitored

---

# End of Part 3B-2

---

# Part 4 – Understanding Nagios Configuration Files

---

# 31. Understanding Nagios Configuration Files

## Objective

Nagios stores all of its monitoring settings in **configuration files (`.cfg`)**. These files define what Nagios monitors, how often checks are performed, which plugins are executed, and who receives notifications.

Without these configuration files, Nagios has no information about the infrastructure it should monitor.

The primary configuration directory is:

```text
/usr/local/nagios/etc/
```

Main configuration files include:

```text
nagios.cfg
cgi.cfg
resource.cfg
objects/
```

---

## Configuration File Hierarchy

```text
/usr/local/nagios/etc
│
├── nagios.cfg          # Main configuration file
├── cgi.cfg             # Web interface configuration
├── resource.cfg        # Resource variables
└── objects/            # Monitoring object definitions
```

---

# 32. Directory Structure Explained

After installation, Nagios is installed under:

```text
/usr/local/nagios/
```

Directory layout:

```text
/usr/local/nagios
│
├── bin/
├── etc/
├── include/
├── libexec/
├── sbin/
├── share/
└── var/
```

---

## Directory Explanation

| Directory | Purpose |
|-----------|---------|
| **bin** | Contains the Nagios executable (`nagios`) |
| **etc** | Stores all configuration files |
| **include** | Header files used during compilation |
| **libexec** | Stores all Nagios plugins |
| **sbin** | CGI programs used by the web interface |
| **share** | HTML, CSS, JavaScript, and images |
| **var** | Log files, status information, runtime data |

---

## Important Directories

### bin/

Contains the Nagios executable.

Example:

```bash
/usr/local/nagios/bin/nagios
```

---

### etc/

Stores configuration files.

Examples:

```text
nagios.cfg
cgi.cfg
resource.cfg
objects/
```

---

### libexec/

Contains monitoring plugins.

Examples:

```text
check_ping
check_http
check_disk
check_load
check_tcp
```

---

### share/

Contains web interface resources.

Examples:

- Images
- CSS
- HTML
- JavaScript

---

### var/

Stores runtime information.

Examples:

```text
nagios.log
status.dat
objects.cache
```

---

# 33. Understanding `nagios.cfg`

## What is nagios.cfg?

`nagios.cfg` is the **main configuration file** for Nagios Core.

It tells Nagios:

- Which configuration files to load
- Where logs are stored
- Where plugins are located
- Which object definitions should be read
- Runtime settings

Location:

```text
/usr/local/nagios/etc/nagios.cfg
```

---

## View the File

```bash
less /usr/local/nagios/etc/nagios.cfg
```

---

## Important Parameters

### Log File

```cfg
log_file=/usr/local/nagios/var/nagios.log
```

Specifies where Nagios writes its log messages.

---

### Object Directory

```cfg
cfg_dir=/usr/local/nagios/etc/objects
```

Tells Nagios to load every configuration file from the `objects` directory.

---

### Command File

```cfg
command_file=/usr/local/nagios/var/rw/nagios.cmd
```

Used by the web interface to send commands to the Nagios daemon.

Examples:

- Schedule downtime
- Acknowledge alerts
- Restart checks

---

### Status File

```cfg
status_file=/usr/local/nagios/var/status.dat
```

Stores the current status of all hosts and services.

---

### State Retention

```cfg
retain_state_information=1
```

Allows Nagios to remember the previous state after a restart.

---

## Why is nagios.cfg Important?

Think of `nagios.cfg` as the **master configuration file**.

It tells Nagios where every other configuration file is located.

---

# 34. Understanding `cgi.cfg`

## What is cgi.cfg?

`cgi.cfg` controls the Nagios **web interface**.

It defines:

- Authentication
- User permissions
- Administrative access
- Read-only access

Location:

```text
/usr/local/nagios/etc/cgi.cfg
```

---

## View File

```bash
less /usr/local/nagios/etc/cgi.cfg
```

---

## Important Parameters

### Main Administrator

```cfg
authorized_for_system_information=nagiosadmin
```

Allows the specified user to view system information.

---

### Configuration Access

```cfg
authorized_for_configuration_information=nagiosadmin
```

Allows viewing configuration through the web interface.

---

### Service Commands

```cfg
authorized_for_all_service_commands=nagiosadmin
```

Allows:

- Restart checks
- Disable notifications
- Schedule downtime

---

### Host Commands

```cfg
authorized_for_all_host_commands=nagiosadmin
```

Allows host-related administrative actions.

---

## How Authentication Works

```
Browser

↓

Apache Authentication

↓

htpasswd.users

↓

cgi.cfg Permissions

↓

Nagios Dashboard
```

---

# 35. Understanding `resource.cfg`

## What is resource.cfg?

`resource.cfg` stores reusable variables and sensitive information.

Instead of hardcoding passwords in plugins, Nagios references variables stored in this file.

Location:

```text
/usr/local/nagios/etc/resource.cfg
```

---

## View File

```bash
less /usr/local/nagios/etc/resource.cfg
```

---

## Common Variables

### USER1

```cfg
$USER1$=/usr/local/nagios/libexec
```

Points to the Nagios plugins directory.

Example:

```cfg
command_line $USER1$/check_ping
```

Instead of writing:

```cfg
/usr/local/nagios/libexec/check_ping
```

Nagios replaces `$USER1$` automatically.

---

### USER2

Example:

```cfg
$USER2$=public
```

Can store an SNMP community string.

---

### USER3

Example:

```cfg
$USER3$=database_password
```

Can store database credentials.

---

## Why Use Variables?

Benefits:

- Avoid hardcoding passwords
- Easier maintenance
- Reusable configuration
- Improved security

---

## Example

Instead of:

```cfg
command_line /usr/local/nagios/libexec/check_snmp -C public
```

Use:

```cfg
command_line $USER1$/check_snmp -C $USER2$
```

This makes future password changes much easier.

---

# Summary

| File | Purpose |
|------|---------|
| **nagios.cfg** | Main Nagios configuration |
| **cgi.cfg** | Web interface permissions |
| **resource.cfg** | Variables and credentials |
| **objects/** | Hosts, services, contacts, commands |

---

## Configuration Relationship

```text
                nagios.cfg
                     │
        ┌────────────┼────────────┐
        │            │            │
        ▼            ▼            ▼
   cgi.cfg     resource.cfg    objects/
                                    │
                    ┌───────────────┼────────────────┐
                    ▼               ▼                ▼
              commands.cfg    contacts.cfg    localhost.cfg
```

---

## Best Practices

- Keep configuration files backed up.
- Do not store plain-text passwords in command definitions.
- Use `resource.cfg` for sensitive variables.
- Validate changes before restarting Nagios:

```bash
/usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
```

A successful validation should end with:

```text
Things look okay - No serious problems were detected during the pre-flight check.
```

---

## End of Part 4A
