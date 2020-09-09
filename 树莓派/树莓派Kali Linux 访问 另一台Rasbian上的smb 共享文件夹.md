# Prerequisites:
* 两台设备在同一局域网下LAN
* 服务器为Rasbian设备，访问客户端为Kali linux
* 两台主机的ip要知道
* 本教程针对的是客户端如何连接共享文件夹，默认的认为服务端的共享文件夹已经配置完成

## Getting start:
1. 先在客户端下载smbclient客户端
```
sudo apt-get install smbclient -y
```
2. 通过输入如下指令查询共享文件夹
```
smbclient -L 192.168.3.9
```
其中，上面的ip为服务端的ip。若出现如下：
```
	Sharename       Type      Comment
	---------       ----      -------
	print$          Disk      Printer Drivers
	pi              Disk      
	IPC$            IPC       IPC Service (Samba 4.9.5-Debian)
SMB1 disabled -- no workgroup available
```
说明能够访问共享文件夹
3. 输入如下指令将共享文件夹挂载到kali下:
```
sudo mount -t cifs -o user=pi,vers=3.0 //192.168.3.9/pi/ /mnt/Myshare
```
其中：
* `//192.168.3.9/pi/`  为服务端ip及共享文件夹地址(这里的地址一定要输入正确才行，多试几次)
* `user1=pi` 为在服务端上自己设置好的用户名（树莓派配置smb,默认都是pi)
* `vers=3.0` 为samba对应的版本，可以试着输入1.0、 2.0、 2.1、3.0，哪个能挂载起来用哪个 
### Trap:
```
mount error(2): No such file or directory
Refer to the mount.cifs(8) manual page (e.g. man mount.cifs) and kernel log messages (dmesg)
```
在上述第三步进行挂载的时候遇到了这样的问题，在stackoverflow学到了，可以通过更改`vers=`的版本号解决