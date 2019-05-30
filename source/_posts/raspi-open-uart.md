---
title: 树莓pi开启串口
date: 2019-05-30 16:35:26
tags:
  - 树莓PI
  - 串口
---

1. 修改/boot/config.txt，增加如下 2 行

```
dtoverlay=pi3-miniuart-bt
enable_uart=1
```

2. 重启服务

```shell
sudo systemctl mask serial-getty@ttyAMA0.service
```
