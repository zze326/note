## 方法一：扩展已有逻辑卷
> 该种方法通常在需要扩展文件系统根分区使用。

添加磁盘设备后，不重启扫描出磁盘设备：
```bash
for dirname in `ls /sys/class/scsi_host/`;do echo "- - -" > /sys/class/scsi_host/$dirname/scan; done
```
查看块设备的信息：
```bash
lsblk
```
将物理卷 /dev/sdb 添加到卷组 ubuntu-vg 中（通过这一步，扩展了卷组的可用空间）：
```bash
vgextend ubuntu-vg /dev/sdb
```
扩展逻辑卷 `/dev/mapper/ubuntu--vg-ubuntu--lv`（参数 `-l +100%FREE` 表示使用卷组中的所有可用空间进行扩展）：
```bash
lvextend -l +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv
```

刷新文件系统大小：
```
resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv
```

## 方法二：新加逻辑卷
> 该种方法通常在新加硬盘创建为 LVM 逻辑卷单独挂载到文件系统时使用。

将物理卷（Physical Volume）创建在设备 `/dev/sdb` 上：
```bash
pvcreate /dev/sdb
```
创建一个卷组（Volume Group）并将物理卷 `/dev/sdb` 添加到这个卷组中：
```bash
vgcreate data-vg /dev/sdb
```
在卷组 data-vg 上创建一个逻辑卷，命名为 data-lv（参数 `-l 100%FREE` 表示使用卷组中的所有可用空间来创建这个逻辑卷）：
```bash
lvcreate --name data-lv -l 100%FREE data-vg
```
显示逻辑卷的详细信息，包括逻辑卷的大小、卷组、物理卷等信息：
```bash
lvdisplay
```
在逻辑卷 `/dev/data-vg/data-lv` 上创建XFS文件系统：
```bash
mkfs.xfs /dev/data-vg/data-lv
```