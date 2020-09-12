---
title: GPT bölümü olan bir işletim sistemi diskini yeniden boyutlandırma | Microsoft Docs
description: Bu makale, GPT bölümü olan bir işletim sistemi diskini yeniden boyutlandırmayla ilgili yönergeler sağlar.
services: virtual-machines-linux
documentationcenter: ''
author: kailashmsft
manager: dcscontentpm
editor: ''
tags: ''
ms.service: security
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.devlang: azurecli
ms.date: 05/03/2020
ms.author: kaib
ms.custom: seodec18
ms.openlocfilehash: 30a960c3ed76788158b15022947fec49a95ae299
ms.sourcegitcommit: 3246e278d094f0ae435c2393ebf278914ec7b97b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89375219"
---
# <a name="resize-an-os-disk-that-has-a-gpt-partition"></a>GPT bölümü olan bir işletim sistemi diskini yeniden boyutlandırma

> [!NOTE]
> Bu senaryo yalnızca GUID bölümleme tablosu (GPT) bölümü olan işletim sistemi diskleri için geçerlidir.

Bu makalede, Linux 'ta GPT bölümü olan bir işletim sistemi diskinin boyutunu artırma işlemi açıklanır. 

## <a name="identify-whether-the-os-disk-has-an-mbr-or-gpt-partition"></a>İşletim sistemi diskinde MBR veya GPT bölümü olup olmadığını belirler

`parted`Disk bölümünün ana önyükleme kaydı (MBR) bölümüyle veya BIR GPT bölümüyle oluşturulmuş olup olmadığını belirlemek için komutunu kullanın.

### <a name="mbr-partition"></a>MBR bölümü

Aşağıdaki çıktıda, **bölüm tablosunda** **Msdos**değeri gösterilmektedir. Bu değer bir MBR bölümünü tanımlar.

```
[user@myvm ~]# parted -l /dev/sda
Model: Msft Virtual Disk (scsi)
Disk /dev/sda: 107GB
Sector size (logical/physical): 512B/512B
Partition Table: msdos
Number  Start   End     Size    Type     File system  Flags
1       1049kB  525MB   524MB   primary  ext4         boot
2       525MB   34.4GB  33.8GB  primary  ext4
[user@myvm ~]#
```

### <a name="gpt-partition"></a>GPT bölümü

Aşağıdaki çıktıda, **bölüm tablosu** **GPT**değerini gösterir. Bu değer bir GPT bölümünü tanımlar.

```
[user@myvm ~]# parted -l /dev/sda
Model: Msft Virtual Disk (scsi)
Disk /dev/sda: 68.7GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags:

Number  Start   End     Size    File system  Name                  Flags
1       1049kB  525MB   524MB   fat16        EFI System Partition  boot
2       525MB   1050MB  524MB   xfs
3       1050MB  1052MB  2097kB                                     bios_grub
4       1052MB  68.7GB  67.7GB                                     lvm
```

Sanal makinenizde (VM) işletim sistemi diskinizde bir GPT bölümü varsa, işletim sistemi diskinin boyutunu artırın.

## <a name="increase-the-size-of-the-os-disk"></a>İşletim sistemi diskinin boyutunu artırın

Aşağıdaki yönergeler, Linux onaylı dağıtımlar için geçerlidir.

> [!NOTE]
> Devam etmeden önce, VM 'nizin yedek bir kopyasını oluşturun veya işletim sistemi diskinizin bir anlık görüntüsünü alın.

### <a name="ubuntu"></a>Ubuntu

Ubuntu 16. x ve 18. x içindeki işletim sistemi diskinin boyutunu artırmak için:

1. VM'yi durdurun.
1. Portaldan işletim sistemi diskinin boyutunu artırın.
1. VM 'yi yeniden başlatın ve ardından VM 'de bir **kök** Kullanıcı olarak oturum açın.
1. İşletim sistemi diskinin artık daha fazla dosya sistemi boyutunu görüntülediğini doğrulayın.

Aşağıdaki örnekte gösterildiği gibi, işletim sistemi diski portaldan 100 GB 'a yeniden boyutlandırıldı. **/Dev/sda1** dosya sistemi **/** artık 97 GB görüntülüyor.

```
user@myvm:~# df -Th
Filesystem     Type      Size  Used Avail Use% Mounted on
udev           devtmpfs  314M     0  314M   0% /dev
tmpfs          tmpfs      65M  2.3M   63M   4% /run
/dev/sda1      ext4       97G  1.8G   95G   2% /
tmpfs          tmpfs     324M     0  324M   0% /dev/shm
tmpfs          tmpfs     5.0M     0  5.0M   0% /run/lock
tmpfs          tmpfs     324M     0  324M   0% /sys/fs/cgroup
/dev/sda15     vfat      105M  3.6M  101M   4% /boot/efi
/dev/sdb1      ext4       20G   44M   19G   1% /mnt
tmpfs          tmpfs      65M     0   65M   0% /run/user/1000
user@myvm:~#
```

### <a name="suse"></a>SUSE

SUSE 12 SP4'TE işletim sistemi diskinin boyutunu artırmak için, SAP için SUSE SLES 12, SUSE SLES 15 ve SAP için SUSE SLES 15:

1. VM'yi durdurun.
1. Portaldan işletim sistemi diskinin boyutunu artırın.
1. VM’yi yeniden başlatın.

VM yeniden başlatıldığında, aşağıdaki adımları gerçekleştirin:

1. Aşağıdaki komutu kullanarak sanal makinenize **kök** Kullanıcı olarak erişin:

   ```
   # sudo -i
   ```

1. Bölümü yeniden boyutlandırmak için kullanılacak **growpart** paketini yüklemek için aşağıdaki komutu kullanın:

   ```
   # zypper install growpart
   ```

1. `lsblk`FileSystem ("/") köküne takılmış bölümü bulmak için komutunu kullanın. Bu örnekte, SDA cihazının 4. bölümüne bağlı olduğunu görüyoruz.

   ```
   # lsblk
   NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   sda      8:0    0   48G  0 disk
   ├─sda1   8:1    0    2M  0 part
   ├─sda2   8:2    0  512M  0 part /boot/efi
   ├─sda3   8:3    0    1G  0 part /boot
   └─sda4   8:4    0 28.5G  0 part /
   sdb      8:16   0    4G  0 disk
   └─sdb1   8:17   0    4G  0 part /mnt/resource
   ```

1. `growpart`Önceki adımda bulunan bölüm numarasını kullanarak komutu kullanarak gerekli bölümü yeniden boyutlandırın.

   ```
   # growpart /dev/sda 4
   CHANGED: partition=4 start=3151872 old: size=59762655 end=62914527 new: size=97511391 end=100663263
   ```

1. `lsblk`Bölümün arttırılmadığını denetlemek için komutu yeniden çalıştırın.

   Aşağıdaki çıktıda **/dev/sda4** bölümünün 46,5 GB olarak yeniden boyutlandırıldığı gösterilmektedir:
   
   ```
   linux:~ # lsblk
   NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   sda      8:0    0   48G  0 disk
   ├─sda1   8:1    0    2M  0 part
   ├─sda2   8:2    0  512M  0 part /boot/efi
   ├─sda3   8:3    0    1G  0 part /boot
   └─sda4   8:4    0 46.5G  0 part /
   sdb      8:16   0    4G  0 disk
   └─sdb1   8:17   0    4G  0 part /mnt/resource
   ```

1. Komutunu bayrağıyla kullanarak işletim sistemi diskindeki dosya sisteminin türünü belirler `lsblk` `-f` :

   ```
   linux:~ # lsblk -f
   NAME   FSTYPE LABEL UUID                                 MOUNTPOINT
   sda
   ├─sda1
   ├─sda2 vfat   EFI   AC67-D22D                            /boot/efi
   ├─sda3 xfs    BOOT  5731a128-db36-4899-b3d2-eb5ae8126188 /boot
   └─sda4 xfs    ROOT  70f83359-c7f2-4409-bba5-37b07534af96 /
   sdb
   └─sdb1 ext4         8c4ca904-cd93-4939-b240-fb45401e2ec6 /mnt/resource
   ```

1. Dosya sistemi türüne göre, dosya sistemini yeniden boyutlandırmak için uygun komutları kullanın.
   
   **XFS**için aşağıdaki komutu kullanın:
   
   ```
   #xfs_growfs /
   ```
   
   Örnek çıktı:
   
   ```
   linux:~ # xfs_growfs /
   meta-data=/dev/sda4              isize=512    agcount=4, agsize=1867583 blks
            =                       sectsz=512   attr=2, projid32bit=1
            =                       crc=1        finobt=0 spinodes=0 rmapbt=0
            =                       reflink=0
   data     =                       bsize=4096   blocks=7470331, imaxpct=25
            =                       sunit=0      swidth=0 blks
   naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
   log      =internal               bsize=4096   blocks=3647, version=2
            =                       sectsz=512   sunit=0 blks, lazy-count=1
   realtime =none                   extsz=4096   blocks=0, rtextents=0
   data blocks changed from 7470331 to 12188923
   ```
   
   **Ext4**için aşağıdaki komutu kullanın:
   
   ```
   #resize2fs /dev/sda4
   ```
   
1. Aşağıdaki komutu kullanarak **df**için daha fazla dosya sistemi boyutunu doğrulayın:
   
   ```
   #df -Thl
   ```
   
   Örnek çıktı:
   
   ```
   linux:~ # df -Thl
   Filesystem     Type      Size  Used Avail Use% Mounted on
   devtmpfs       devtmpfs  445M  4.0K  445M   1% /dev
   tmpfs          tmpfs     458M     0  458M   0% /dev/shm
   tmpfs          tmpfs     458M   14M  445M   3% /run
   tmpfs          tmpfs     458M     0  458M   0% /sys/fs/cgroup
   /dev/sda4      xfs        47G  2.2G   45G   5% /
   /dev/sda3      xfs      1014M   86M  929M   9% /boot
   /dev/sda2      vfat      512M  1.1M  511M   1% /boot/efi
   /dev/sdb1      ext4      3.9G   16M  3.7G   1% /mnt/resource
   tmpfs          tmpfs      92M     0   92M   0% /run/user/1000
   tmpfs          tmpfs      92M     0   92M   0% /run/user/490
   ```
   
   Yukarıdaki örnekte, işletim sistemi diski için dosya sistemi boyutunun arttığını görebiliriz.

### <a name="rhel"></a>RHEL

RHEL 7. x içindeki işletim sistemi diskinin boyutunu LVM ile artırmak için:

1. VM'yi durdurun.
1. Portaldan işletim sistemi diskinin boyutunu artırın.
1. VM’yi başlatın.

VM yeniden başlatıldığında, aşağıdaki adımları gerçekleştirin:

1. Aşağıdaki komutu kullanarak sanal makinenize **kök** Kullanıcı olarak erişin:
 
   ```
   #sudo su
   ```

1. İşletim sistemi diskinin boyutunu artırmak için gereken **gptfdisk** paketini yükler.

   ```
   #yum install gdisk -y
   ```

1. Diskte bulunan en büyük kesimi görmek için şu komutu çalıştırın:

   ```
   #sgdisk -e /dev/sda
   ```

1. Aşağıdaki komutu kullanarak silmeden bölümü yeniden boyutlandırın. **Genişletilmiş** komutun, Bölümü silmeden yeniden boyutlandırmak için **resizepart** adlı bir seçeneği vardır. **Resizepart** öğesinden sonraki 4 sayısı dördüncü bölümün yeniden boyutlandırdığını gösterir.

   ```
   #parted -s /dev/sda "resizepart 4 -1" quit
   ```
    
1. Bölümün arttığını doğrulamak için aşağıdaki komutu çalıştırın:

   ```
   #lsblk
   ```

   Aşağıdaki çıktı, **/dev/sda4** bölümünün 99 GB olarak yeniden boyutlandırıldığını gösterir.

   ```
   [user@myvm ~]# lsblk
   NAME              MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
   fd0                 2:0    1    4K  0 disk
   sda                 8:0    0  100G  0 disk
   ├─sda1              8:1    0  500M  0 part /boot/efi
   ├─sda2              8:2    0  500M  0 part /boot
   ├─sda3              8:3    0    2M  0 part
   └─sda4              8:4    0   99G  0 part
   ├─rootvg-tmplv    253:0    0    2G  0 lvm  /tmp
   ├─rootvg-usrlv    253:1    0   10G  0 lvm  /usr
   ├─rootvg-optlv    253:2    0    2G  0 lvm  /opt
   ├─rootvg-homelv   253:3    0    1G  0 lvm  /home
   ├─rootvg-varlv    253:4    0    8G  0 lvm  /var
   └─rootvg-rootlv   253:5    0    2G  0 lvm  /
   sdb                 8:16   0   50G  0 disk
   └─sdb1              8:17   0   50G  0 part /mnt/resource
   ```

1. Fiziksel birimi (BD) yeniden boyutlandırmak için aşağıdaki komutu kullanın:

   ```
   #pvresize /dev/sda4
   ```

   Aşağıdaki çıktı BD 'in 99,02 GB olarak yeniden boyutlandırıldığını gösterir.

   ```
   [user@myvm ~]# pvresize /dev/sda4
   Physical volume "/dev/sda4" changed
   1 physical volume(s) resized or updated / 0 physical volume(s) not resized

   [user@myvm ~]# pvs
   PV         VG     Fmt  Attr PSize   PFree
   /dev/sda4  rootvg lvm2 a--  <99.02g <74.02g
   ```

1. Aşağıdaki örnekte **/dev/mapper/rootvg-rootlv** , aşağıdaki KOMUTLA 2 GB Ila 12 GB (10 GB 'lık artış) olarak yeniden boyutlandırılıyor. Bu komut ayrıca dosya sistemini yeniden boyutlandıracaktır.

   ```
   #lvresize -r -L +10G /dev/mapper/rootvg-rootlv
   ```

   Örnek çıktı:

   ```
   [user@myvm ~]# lvresize -r -L +10G /dev/mapper/rootvg-rootlv
   Size of logical volume rootvg/rootlv changed from 2.00 GiB (512 extents) to 12.00 GiB (3072 extents).
   Logical volume rootvg/rootlv successfully resized.
   meta-data=/dev/mapper/rootvg-rootlv isize=512    agcount=4, agsize=131072 blks
            =                       sectsz=4096  attr=2, projid32bit=1
            =                       crc=1        finobt=0 spinodes=0
   data     =                       bsize=4096   blocks=524288, imaxpct=25
            =                       sunit=0      swidth=0 blks
   naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
   log      =internal               bsize=4096   blocks=2560, version=2
            =                       sectsz=4096  sunit=1 blks, lazy-count=1
   realtime =none                   extsz=4096   blocks=0, rtextents=0
   data blocks changed from 524288 to 3145728
   ```
         
1. Aşağıdaki komutu kullanarak **/dev/mapper/rootvg-rootlv** 'nin artırılmış bir dosya sistemi boyutuna sahip olup olmadığını doğrulayın:

   ```
   #df -Th /
   ```

   Örnek çıktı:

   ```
   [user@myvm ~]# df -Th /
   Filesystem                Type  Size  Used Avail Use% Mounted on
   /dev/mapper/rootvg-rootlv xfs    12G   71M   12G   1% /
   [user@myvm ~]#
   ```

> [!NOTE]
> Diğer mantıksal birimleri yeniden boyutlandırmak için aynı yordamı kullanmak için 7. adımda **LV** adını değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

- [Diski yeniden boyutlandır](expand-disks.md)
