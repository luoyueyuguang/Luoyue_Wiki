***
- qemu flag
``` shell
qemu-system-x86_64 [machine opts] \
                [cpu opts] \
                [accelerator opts] \
                [device opts] \
                [backend opts] \
                [interface opts] \
                [boot opts]
```
- example
``` shell
qemu-system-x86_64 -device scsi-hd,help
$ qemu-system-x86_64 -M help
```

|Options| |
|---|---|
|Machine|Define the machine type, amount of memory etc|
|CPU|Type and number/topology of vCPUs. Most accelerators offer a `host` cpu option which simply passes through your host CPU configuration without filtering out any features.|
|Accelerator|This will depend on the hypervisor you run. Note that the default is TCG, which is purely emulated, so you must specify an accelerator type to take advantage of hardware virtualization.|
|Devices|Additional devices that are not defined by default with the machine type.|
|Backends|Backends are how QEMU deals with the guest’s data, for example how a block device is stored, how network devices see the network or how a serial device is directed to the outside world.|
|Interfaces|How the system is displayed, how it is managed and controlled or debugged.|
|Boot|How the system boots, via firmware or direct kernel boot.|
***
- 一次qemu启动解析
``` shell
qemu-system-aarch64 \
   -machine type=virt,virtualization=on,pflash0=rom,pflash1=efivars \
   -m 4096 \
```
定义一个aarch64主机,type为virt,开启virtualization来在模拟里使用kvm.给pflash命名来以后覆盖默认设置.分配4096MB内存。
``` bash
-cpu max,pauth-impdef=on \
-smp 4 \
-accel tcg \
```

定义了最高4个vcpu,使用tcg加速,`pauth-impdef=on`：启用 PAUTH(pointer authentication)功能,这是 ARM 架构的用于保护指针不被篡改的功能。
``` bash
-device virtio-net-pci,netdev=unet \
-device virtio-scsi-pci \
-device scsi-hd,drive=hd \
```
添加和配置虚拟机中的设备
``` shell
-netdev user,id=unet,hostfwd=tcp::2222-:22 \
```
可以使用 `-netdev` 和 `-device` 选项创建一个用户模式网络设备，ID 为 `unet`，并将主机的 2222 端口转发到虚拟机的 22 端口。
``` shell
-blockdev driver=raw,node-name=hd,file.driver=host_device,file.filename=/dev/lvm-disk/debian-bullseye-arm64 \
```
配置一个块设备，其驱动为 `raw`，节点名称为 `hd`，文件驱动为 `host_device`，文件名为 `/dev/lvm-disk/debian-bullseye-arm64`
``` shell
-serial mon:stdio \
-display none \
```
使用 `-serial mon:stdio` 将监视器和虚拟机的串行控制台重定向到标准输入/输出，同时使用 `-display none` 禁用图形显示
``` shell
-blockdev node-name=rom,driver=file,filename=(pwd)/pc-bios/edk2-aarch64-code.fd,read-only=true \
-blockdev node-name=efivars,driver=file,filename=$HOME/images/qemu-arm64-efivars
```
使用 `-blockdev` 选项指定固件和变量文件
***
**调用**
