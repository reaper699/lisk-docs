
# Preparing your Operating System

This tutorial describes how to prepare your operating system for Lisk.

## 1. Confirm your operating system is supported.

The following operating systems and architectures are supported:

- [Linux (x86_64)](#linux-x86_64-)
- [Linux (i686)](#linux-i686-)
- [Linux (armv6l)](#linux-armv6l-)
- [Linux (armv7l)](#linux-armv7l-)
- [Darwin (x86_64)](#darwin-x86_64-)
- [FreeBSD (amd64)](#freebsd-amd64-)

If you are unsure which platform to choose from, open a terminal and run the following command:

```text
uname -sm
```

The resulting output, should tell you if your machine is running on a supported operating system and architecture.

## 2. Download and Install packages

Follow these instructions to load required software to your system.

Ensure you are logged in as root.

```text
whoami
```


### Linux

#### 1. Install `curl, wget, tar, sudo`

*  If you are running Ubuntu
  
  ```text
  apt-get install curl wget tar sudo
  ```
  
*  If you are running RHEL or CentOS

  ```text
  yum install curl wget tar sudo
  ```

#### 2. Create a user to run lisk, create sudo group and give the lisk user sudo permissions

  ```text
  useradd lisk
  groupadd sudo
  usermod -a -G sudo lisk
  ```

#### 3. Set a password for the lisk user, make this password strong.

  ```text
  passwd lisk
  ```

#### 4. Setup the sudoers file

  ```text
  visudo
  ```
  
  Paste this line in at the bottom of the file
  ```text
  %sudo   ALL=(ALL:ALL) ALL
  ```
  
  Exit with `:wq!`

  
