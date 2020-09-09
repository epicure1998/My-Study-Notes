## 设置开机自动登录
1. 进入编辑`vim /etc/lightdm/lightdm.conf`
2. 打开lightdm.conf文件时，向下滚动，直到看到两行：
```
#autologin-user=
#autologin-user-timeout=0

取消注释并更改为root

autologin-user=kali
autologin-user-timeout=0
```
3. 保存退出，继续修改
```
vim /etc/pam.d/lightdm-autologin
将这行注释掉即可

# auth required pam_succeed_if.so user != root quiet_success
```
## 更换下载源
1. 打开编辑 `sudo vi /etc/apt/sources.list`
2. 将原有的源注释，然后添加新的源
```
deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
```
3. 执行`apt-get update && apt-get upgrade`