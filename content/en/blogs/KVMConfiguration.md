---
title: "KVM Configuration Manual"
date: 2024-02-03T00:00:00+08:00
draft: false
author: "GeekerHWH"
category:
    - Technology
en_tags:
    - KVM
    - Virtualization
    - Debian
image: /images/blogs/KVM-1920x1080.jpg
description: "this page list commands that are commonly used when installing KVM with Virtual Machine Manager"
toc: true
---

## Intro
this page list commands that are commonly used when installing KVM, and **I promise this page is all you need**.

Kernel-based Virtual Machine (KVM) is a Linux kernel module that transforms the host operating system into a hypervisor,
enabling it to run multiple virtual machines (VMs). KVM leverages hardware virtualization extensions,
such as Intel VT-x and AMD-V, to provide efficient and secure virtualization capabilities.

Key features of KVM include:
1. Kernel Integration: KVM is tightly integrated into the Linux kernel, allowing it to take advantage of the kernel's stability, security features, and performance enhancements.
2. Hardware Virtualization Support: KVM utilizes hardware virtualization extensions present in modern CPUs to enable the efficient execution of virtualized workloads.
3. Performance: KVM provides high-performance virtualization by leveraging hardware capabilities, resulting in near-native performance for virtualized workloads.
4. Security: KVM benefits from the security features inherent in the Linux kernel. The isolation between VMs is enforced by the kernel, contributing to a secure virtualization environment.
5. Open Source: KVM is an open-source project, meaning its source code is freely available and can be modified and redistributed. This open nature promotes collaboration and widespread adoption.

KVM comes with various Powerful Management Tools: such as libvirt and virt-manager, support snapshots and simplify the creation, configuration, and monitoring of virtual machines.

## Simple Install Manual
> Note: This manual is based on Debian 12; different systems may have different installation commands, and I will inform you which command you should pay attention to.
> 
> All commands that require superuser privileges will begin with `sudo` on this page.
### Check if the computer support Virtualization Tech
```bash
egrep -c '(vmx|svm)' /proc/cpuinfo # 32 for i7-10870H
```
If the output is zero then **go to bios settings** and enable VT-x (Virtualization Technology Extension) for Intel processor and AMD-V for AMD processor.

<!-- ### Check virtualization support
To use VM drivers, verify that your system has virtualization support enabled:
```bash
grep -E -q 'vmx|svm' /proc/cpuinfo && echo yes || echo no
```
If the above command outputs “no”:
- If you are running within a VM, your hypervisor does not allow nested virtualization. You will need to use the None (bare-metal) driver
- If you are running on a physical machine, ensure that your BIOS has hardware virtualization enabled -->


### Install QEMU and Virtual Machine Manager
note: this command is based on Debian 12
```bash
sudo apt install qemu-kvm qemu-system qemu-utils python3 python3-pip libvirt-clients libvirt-daemon-system bridge-utils virtinst libvirt-daemon virt-manager -y
```
note:
- qemu-kvm: the emulator it self
- libvirt-daemon: runs virtualization in background
- bridge-utils: important networking dependencies
- virt-manager: the graphical program we'll use to work with VMs

then verify that Libvirtd service is started
```bash
systemctl status libvirtd.service
```

### Start Default Network for Networking
VIRSH is a command to directly interact with our VMs from terminal. We use it to list networks, vm-status and various other tools when we need to make tweaks. Here is how we start the default and make it auto-start after reboot.
```bash
sudo virsh net-start default
# if errors happened, try the following commands:
    # sudo virsh net-destroy default
    # sudo virsh net-undefine default
    # sudo systemctl restart libvirtd
```

Network default started
```bash
sudo virsh net-autostart default
```

Check status with
```bash
sudo virsh net-list --all
```

### Add User to libvirt to Allow Access to VMs
```bash
sudo usermod -aG libvirt $USER
sudo usermod -aG libvirt-qemu $USER
sudo usermod -aG kvm $USER
sudo usermod -aG input $USER
sudo usermod -aG disk $USER
```
Done! You can now rebbot and try to use virt-manager (if you had exprience working with vmware before)


## Manage VMs from CLI
Let's see how many tools that we can use.
```bash
geekerhwh@Debian ~ $ vir # Tab to prompt
virsh               virt-manager        virt-viewer
virt-admin          virt-pki-query-dn   virt-xml
virt-clone          virt-pki-validate   virt-xml-validate
virt-host-validate  virt-qemu-run       
virt-install        virt-ssh-helper
```


## To be continued...
- [ ] virt-manager manual
- [ ] KVM GPU Passthrough