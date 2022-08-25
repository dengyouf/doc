```
~$ sudo apt install xrdp 
sudo adduser xrdp ssl-cert 

~$ sudo sed -i 's/allowed_users=console/allowed_users=anybody/g' /etc/X11/Xwrapper.config
~$ sudo systemctl  restart xrdp

```

- 避免黑屏

```
sudo cat /etc/xrdp/startwm.sh
 # 追加文件
unset DBUS_SESSION_BUS_ADDRESS 
unset XDG_RUNTIME_DIR
 systemctl restart xrdp
```
