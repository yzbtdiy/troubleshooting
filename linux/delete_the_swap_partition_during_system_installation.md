# Linux 删除系统安装时自动创建的 swap 分区

## 查看 swap 分区

```shell
swapon -s
```

## 卸载 swap 分区

```shell
swapoff /dev/SWAP_PARTITION
```

## 修改 `/etc/fstab`

注释或删除 swap 分区的一行

## 重点:gurb修改

删除 swap 分区并修改 fstab 后重启系统提示 swap 挂载失败, 导致开启长时间停留在文件系统挂载, 需要通过以下操作解决.

### Redhat 系列

1. 修改 `/etc/default/grub`, 删除 swap 分区相关内容
2. 更新 grub.cfg `grub2-mkconfig -o /boot/grub/grub.cfg`

### Debian 系列

1. 修改 `/etc/initramfs-tools/conf.d/resume`, 删除 swap 分区相关内容
2. 更新 grub
3. 更新 initramfs

方式一

```shell
sed -i 'SWAP_PARTITION' /etc/initramfs-tools/conf.d/resume
update-grub
update-initramfs -u
```

方式二

```shell
echo 'UUID=none' > /etc/initramfs-tools/conf.d/resume
update-initramfs -u -k all
```

参考: [lvm - Failed to find logical volume "group/swap" - Server Fault](https://serverfault.com/questions/1014031/failed-to-find-logical-volume-group-swap)

