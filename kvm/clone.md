基于已经创建的虚拟机，克隆新的虚拟机
- scrvm: ubuntu1804
- dstvm: kubeadm-master-01

```
sudo  virt-clone --original  ubuntu1804  --name kubeadm-master-01 --auto-clone
sudo virt-sysprep -d kubeadm-master-01 --hostname kubeadm-master-01 --run-command \
        "sudo sed -i 's@192.168.1.9@192.168.1.200@' /etc/netplan/00-installer-config.yaml \
        && sudo netplan apply \
        && ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key  -N  '' "
```

