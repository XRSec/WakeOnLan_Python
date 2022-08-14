## Write a network line feed tool in Python

## Preview

![image-20210425235954481](https://dogefs.s3.ladydaily.com/ZYGG/storage/20210425235956573396.png?w=1280&fmt=png)
![image-20210426014122257](https://dogefs.s3.ladydaily.com/ZYGG/storage/20210417050652008075.png?w=1280&fmt=jpeg)

## Attention

```bash
chmod 777 wol
cp wol /usr/bin/wol
vi /usr/bin/wol
wol / wol 1.1.1.1 / wol google.com
```



```bash
    DestIp = "localhost"
    DestNatIp = "127.0.0.1"
    DestMacAddress = "B43E98F386A4"
    DestPort = 9
```

```python
#!/usr/bin/python3
# _*_ coding: utf-8 _*_

import socket, binascii, os, sys, requests, re


def banaer():
    os.system("clear")
    print("\n\033[24;37;34m __  __  ____                      \033[0m")
    print("\033[24;37;32m \ \/ / |  _ \   ___    ___    ___ \033[0m")
    print("\033[24;37;36m  \ \/  | |_) | / __|  / _ \  / __| \033[0m")
    print("\033[24;37;31m  /  \  |  _ <  \__ \ |  __/ | (__ \033[0m")
    print("\033[24;37;35m /_/\_\ |_| \_\ |___/  \___|  \___| \n\033[0m")
    print("\033[24;37;35m [ Help ] \n\033[0m")
    print("\033[24;37;31m wol / wol 1.1.1.1 / wol google.com \033[0m")
    print("\033[24;37;32m Edit wol「 DestIp DestPort DestMacAddress 」 \n\033[0m")


def DpmainToIp(dest_ip):
    web_status = requests.get("https://myssl.com/api/v1/tools/dns_query?qtype=1&host=%s&qmode=1" % dest_ip, timeout=5)
    try:
        dest_ip = (web_status.json()["data"])["86"][0]["answer"]["records"][0]["value"]
    except:
        try:
            if (web_status.json()["data"])["86"][0]["answer"]["records"] == "null":
                dest_ip = (web_status.json()["data"])["86"][0]["answer"]["records"][0]["value"]
        except:
            print("\033[24;37;31m Error : DpmainToIp > except > except \n\033[0m")
            exit()
    send_msg(dest_ip)


def IpOrDomain(dest_ip):
    check_ip = re.compile(
        '^(1\d{2}|2[0-4]\d|25[0-5]|[1-9]\d|[1-9])\.(1\d{2}|2[0-4]\d|25[0-5]|[1-9]\d|\d)\.(1\d{2}|2[0-4]\d|25[0-5]|['
        '1-9]\d|\d)\.(1\d{2}|2[0-4]\d|25[0-5]|[1-9]\d|\d)$')
    if check_ip.match(dest_ip):
        send_msg(dest_ip)
    else:
        DpmainToIp(dest_ip)


def ip_init(dest_ip):
    if len(sys.argv) == 2:
        dest_ip = sys.argv[1]
        IpOrDomain(dest_ip)
    elif len(sys.argv) == 1:
        IpOrDomain(dest_ip)
    else:
        print("Domain error")


def send_msg(DestIp):
    udp_socket = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
    send_data = binascii.unhexlify('FF' * 6 + DestMacAddress * 16)
    udp_socket.sendto(send_data, (DestIp, DestPort))
    udp_socket.sendto(send_data, (DestNatIp, DestPort))
    print("\033[24;37;35m Python Wol " + DestIp + " Successful! \033[0m")
    print("\033[24;37;34m")


if __name__ == "__main__":
    # TODO
    DestIp = "localhost"
    DestNatIp = "127.0.0.1"
    DestMacAddress = "B43E98F386A4"
    DestPort = 9

    banaer()
    ip_init(DestIp)

```



> 转载请声明来源：[Blog](https://xrsec.vercel.app/WakeOnLan_Python.html) [Github](https://github.com/zygds/WakeOnLan_Python)
