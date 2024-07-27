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
定义一个aarch64主机,type为virt,开启i
