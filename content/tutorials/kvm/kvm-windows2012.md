---
title: "Instalando Windows 2012 sobre KVM"
menuTitle: "kVM - VM Win2012"
description: Instalando windows 2012 sobre KVM
weight: 10
comments: true
asciinema: true
tags:
  - windows
  - kvm
---

## Instalacão

Consiga uma imagem ISO do [windows 2012](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2012-r2) e copie ela na pasta do `/var/lib/libvirt/images`


```shell
sudo mv Downloads/9600.17050.WINBLUE_REFRESH.140317-1640_X64FRE_SERVER_EVAL_EN-US-IR3_SSS_X64FREE_EN-US_DV9.ISO /var/lib/libvirt/images
```

Logo execute `virt-manager`

![](https://i.imgur.com/8fQvJmE.png)




