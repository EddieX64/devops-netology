1. Разрежённый файл (англ. sparse file) — файл, в котором последовательности нулевых байтов заменены на информацию об этих последовательностях (список дыр).

2. Файлы, являющиеся жёсткой ссылкой на объект - не могут иметь разные права и владельца, т.к. жесткая ссылка и файл, для которой она создавалась имеют одинаковые inode. Поэтому жесткая ссылка имеет те же права доступа, владельца и время последней модификации, что и целевой файл. Различаются только имена файлов.

4. Вводим `fdisk -l` и видим что наши свежесозданные диски называются `/dev/sdb` и `/dev/sdc`

   Вводим команду `fdisk /dev/sdb` для разметки первого диска
   вводим m для отображения помощи по командам
   далее вводим n, создаем раздел sdb1 +2G (размеров 2 ГБ)
   снова вводим n, создаем еще один раздел с оставшимся пространством (511 Мб) sdb2
   вводим w, сохраняем изменения

5. Переносим таблицу разделов на второй диск `sfdisk -d /dev/sdb | sfdisk /dev/sdc`

6. ` mdadm --create --verbose /dev/md0 -l 1 -n 2 /dev/sd{b,c}1`

   Вводим `lsblk` и видим, что в разделах `sdb1` и `sdc1` создался массив raid1 под названием `md0`

7. `mdadm --create --verbose /dev/md1 -l 0 -n 2 /dev/sd{b,c}2`
проверяем `lsblk`
видим, что в разделах `sdb2` и `sdc2` создался массив raid0 под названием `md1`

8. Создаем physical volumes
   ```
   pvcreate /dev/md0
   pvcreate /dev/md1
   ```

9.  Создем общую volume group `vgcreate vgtest /dev/md0 /dev/md1`
 
10. `lvcreate -n lv_test -L 100M vgtest /dev/md1`

11. `mkfs.ext4 /dev/vgtest/lv_test`

12. `mount /dev/vgtest/lv_test /tmp/new`

13. `wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz`

14. 
```
root@vagrant:/tmp/new# lsblk
NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
sda                    8:0    0   64G  0 disk
├─sda1                 8:1    0  512M  0 part  /boot/efi
├─sda2                 8:2    0    1K  0 part
└─sda5                 8:5    0 63.5G  0 part
  ├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
  └─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
sdb                    8:16   0  2.5G  0 disk
├─sdb1                 8:17   0    2G  0 part
│ └─md0                9:0    0    2G  0 raid1
└─sdb2                 8:18   0  511M  0 part
  └─md1                9:1    0 1018M  0 raid0
    └─vgtest-lv_test 253:2    0  100M  0 lvm   /tmp/new
sdc                    8:32   0  2.5G  0 disk
├─sdc1                 8:33   0    2G  0 part
│ └─md0                9:0    0    2G  0 raid1
└─sdc2                 8:34   0  511M  0 part
  └─md1                9:1    0 1018M  0 raid0
    └─vgtest-lv_test 253:2    0  100M  0 lvm   /tmp/new
```

15. 
```
gzip -t /tmp/new/test.gz
echo $?
0
```

16. `pvmove /dev/md1 /dev/md0`

17. `mdadm --fail /dev/md0 /dev/sdb1`

18.  
```
[13868.496662] md/raid1:md0: Disk failure on sdb1, disabling device.
               md/raid1:md0: Operation continuing on 1 devices.
```
   
19. Файл остается доступен, даже с учетом сбоя одного диска в raid-массиве
```
gzip -t /tmp/new/test.gz
echo $?
0
```

