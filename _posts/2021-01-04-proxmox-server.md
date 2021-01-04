---
layout: post
title: Using Proxmox, Containers, and GPU Pass Through for Machine Learning
tags: proxmox, GPU, machine-learning, container, pyenv, virtualenv, home-lab
---

Setting up a home lab with GPU support can be helpful in learning machine learning and other technologies. While we could use virtual machines on Proxmox, we will focus on using containers with GPU pass through. Aside from making a few updates, [this post](https://medium.com/@MARatsimbazafy/journey-to-deep-learning-nvidia-gpu-passthrough-to-lxc-container-97d0bc474957) has most of the details necessary to setup the home lab properly. I am going to detail my experience in setting up a lab with Nvidia 1070 Ti and Core i7 setup. 

## Installing Proxmox

To install Proxomox, you will need to create a USB key with the installation software. 

1. Download Proxmox 
    You can download the latest version of Proxmox directly [at this page](https://www.proxmox.com/en/downloads/category/iso-images-pve).
2. Creating the USB key
    I created the key on macOS. For other OSes, you can find the instructions at the [Proxmox wiki](https://pve.proxmox.com/wiki/Prepare_Installation_Media).

    ``` bash
    hdiutil convert -format UDRW -o proxmox-ve_*.dmg proxmox-ve_*.iso # convert the proxmox image into proper format
    ```
    After converting the image file, plug the USB key into the Mac and looking for the disk.
    ``` bash
    # look for external disk
    diskutil list # list all the disk attached to the computer
    ```
    From the list of disks, there should be an external disk. We need to unmount it prior to writing the installation media.
    ``` bash
    diskutil unmountDisk /dev/diskX # replace X with the number corresponding to the external disk
    ```
    Now, you can create the install media using [dd](https://en.wikipedia.org/wiki/Dd_(Unix))
    ``` bash
    sudo dd if=proxmox-ve_*.dmg of=/dev/rdiskX bs=1m
    ```