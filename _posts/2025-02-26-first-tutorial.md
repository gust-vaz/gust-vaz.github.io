---
title: Setting up a test environment for Linux Kernel
date: 2025-02-26 16:00:00 +/-0300
categories: [Open Source Software Development, Linux Kernel, MAC0470]
tags: [linux, kernel, kernel-linux, setup, qemu]
---

### Introduction to Open Source Development

To introduce myself to the Open Source Development, I decided to take the course MAC0470 at IME-USP. We will focus on contribute at the Industrial I/O (IIO) subsystem of the Linux Kernel, which we will have the help of three teaching assistants, the professor and, eventually, the maintainer of the subsystem.

### Setting the Workspace - Tutorial 1

The tutorial at [flusp.ime.usp.br](https://flusp.ime.usp.br/kernel/qemu-libvirt-setup/) explained how to set up a virtualized environment using QEMU and libvirt to test and develop Linux kernels. It walks through installing the necessary tools, configuring virtual machines, and preparing disk images. The goal is to create a safe and efficient workflow for compiling, installing, and testing custom Linux kernels without risking your main system.

Since I didn’t have a Linux OS installed on my machine initially, I followed the setup steps by watching my partner configure the workspace. Once I installed the OS, setting everything up was straightforward—especially because my system was clean with no prior configurations. I just needed to install the required packages and set up the virtual machine.