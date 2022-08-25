#  检查环境

- 是否支持虚拟化：`egrep -c '(vmx|svm)' /proc/cpuinfo`
- 安装 cpu-checker 使用KVM加速：`sudo apt install cpu-checker`
  - `sudo kvm-ok`

# 安装KVM

-  1.  安装程序包

```
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils
sudo systemctl  enable libvirtd --now
````

- 2.  授权用户运行虚拟机

```
~$ sudo usermod -aG libvirt dengyouf
~$ sudo usermod -aG kvm dengyouf 
```

- 3.  安装虚拟机管理器

```
sudo apt install virt-manager  virtinst
```

- 4. 禁用ipv6

```
~$ cat /etc/sysctl.d/bridge.conf
net.bridge.bridge-nf-call-ip6tables=0
net.bridge.bridge-nf-call-iptables=0
net.bridge.bridge-nf-call-arptables=0

sudo vim /etc/udev/rules.d/99-bridge.rules
ACTION=="add", SUBSYSTEM=="module", KERNEL=="br_netfilter", \           RUN+="/sbin/sysctl -p /etc/sysctl.d/bridge.conf"
```

- 5. 配置网桥

```
# ubuntu 20.04
cat  /etc/netplan/01-network-manager-all.yaml
# Let NetworkManager manage all devices on this system
network:
  version: 2
  ethernets:
    eno1:
      dhcp4: false
  bridges:
    br0:
      interfaces: [eno1]
      addresses:
        - 192.168.1.250/24
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses: [114.114.114.114]
      parameters:
        stp:  true
        forward-delay: 4
      dhcp4: false
```
```
# ubuntu 18.04
 cat /etc/netplan/01-network-manager-all.yaml
# Let NetworkManager manage all devices on this system
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: no
      dhcp6: no
  bridges:
    br0:
      interfaces: [eth0]
      addresses:
      - 192.168.229.240/24
      gateway4: 192.168.229.2
      mtu: 1500
      nameservers:
        addresses: [223.5.5.5, 114.114.114.114]
      parameters:
        stp: true
        forward-delay: 4
      dhcp4: no
      dhcp6: no
```

- 6. 安装虚拟机

```
sudo virt-install --name=ubuntu1804 \
  --ram=4096 \
  --vcpus=2 \
  --virt-type=kvm \
  --os-type=linux \
  --os-variant=ubuntu18.04 \
  --network default,model=virtio \
  --graphics=vnc,password=123123,port=5911,listen=0.0.0.0 \
  --noautoconsole \
  --accelerate \  
  --cdrom=/opt/ios/ubuntu-18.04.5-server-amd64.iso \
  --disk path=/var/lib/libvirt/images/ubuntu1804.qcow2,device=disk,format=qcow2,bus=virtio,cache=writeback,size=40
```

> 查询执行的os类型--os-varian：`sudo apt install libosinfo-bin`
