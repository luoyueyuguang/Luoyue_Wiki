- 下载win11
- https://www.debugpoint.com/install-windows-ubuntu-virt-manager/
***
- 报错
```
libvirt.libvirtError: Requested operation is not valid: network 'default' is not active
```
- 解决办法
```
sudo virsh net-list --all
sudo virsh net-start default
```
启用开机自启
```
sudo virsh net-autostart default
```
关闭开机自启
```
sudo virsh net-autostart default
```