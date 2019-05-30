---
title: Python 获取 IP 地址
date: 2019-05-30 16:32:26
tags:
  - Python3
  - IP
  - Socket
  - 网络
---

创建一个 UDP 包，不发送这个包。
然后从这个包中获取当前的 IP 地址。

```python
import socket
try:
    s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    s.connect(('8.8.8.8', 80))
    ip = s.getsockname()[0]
finally:
    s.close()
```
