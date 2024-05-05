https://arch.icekylin.online/guide/advanced/system-ctl.html
```
sudo timeshift --list # 获取快照列表
sudo timeshift --restore --snapshot '20XX-XX-XX_XX-XX-XX' --skip-grub # 选择一个快照进行还原，并跳过 GRUB 安装，一般来说 GRUB 不需要重新安装
```

无法进入系统
```
sudo timeshift --restore --snapshot-device /dev/sdbx
```

无法挂载boot
```
# 手动挂载子卷 / 和 /boot（nvmexnxpx 填写实际分区号）
mount -t btrfs -o subvol=/@,compress=zstd /dev/nvmexnxpx /mnt
mount /dev/nvmexnxpx /mnt/boot
# chroot
arch-chroot /mnt
# 更新内核与软件包
pacman -Syu linux
# 重启
reboot
```

命令生效
```
sudo systemctl enable --now cronie.service
sudo systemctl start --now cronie.service
```
