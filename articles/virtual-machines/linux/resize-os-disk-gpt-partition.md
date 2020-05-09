---
title: GPT bölümü ile bir işletim sistemi diskini yeniden boyutlandırma | Microsoft Docs
description: Bu makalede, GPT bölümüyle bir işletim sistemi diskini yeniden boyutlandırmayla ilgili yönergeler sağlanır.
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
ms.openlocfilehash: f863233f0a34271841cc8e973f9aa3ca9416ceeb
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858996"
---
# <a name="resize-an-os-disk-with-a-gpt-partition"></a>GPT bölümü ile bir işletim sistemi diskini yeniden boyutlandırma

> [!NOTE]
> Bu senaryo yalnızca GPT bölümü olan işletim sistemi diski için geçerlidir.

Bu makalede, Linux 'ta GPT bölümü ile işletim sistemi diskinin boyutunun nasıl yükseltileceği açıklanır.

## <a name="identify-whether-the-os-disk-has-an-mbr-or-gpt-partition"></a>İşletim sistemi diskinde MBR veya GPT bölümü olup olmadığını belirler

Disk bölümünün ana önyükleme kaydı (MBR) bölümüyle veya bir GUID bölümleme tablosu (GPT) bölümüyle oluşturulup oluşturulmediğinin belirlemek için, **ayrıştıralınmış** komutunu kullanın.

### <a name="mbr-partition"></a>MBR bölümü

Aşağıdaki çıktıda, **bölüm tablosu** bir, bir **MBR** bölümünü tanımlayan bir **Msdos**değeri gösterir.

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

Aşağıdaki çıktıda, **bölüm tablosu** GPT bir bölümü tanımlayarak bir **GPT**değeri gösterir.

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

### <a name="ubuntu-16x-and-18x"></a>Ubuntu 16. x ve 18. x

Ubuntu 16. x ve 18. x içindeki işletim sistemi diskinin boyutunu artırmak için:

1. VM'yi durdurun.
1. Portaldan OSDisk boyutunu artırın.
1. VM 'yi yeniden başlatın ve ardından VM 'de bir **kök** Kullanıcı olarak oturum açın.
1. OSDisk artık daha fazla dosya sistemi boyutunu görüntüleyecek.

Aşağıdaki örnekte gösterildiği gibi, **/dev/sda1** dosya sistemi **/** artık 97 GB görüntülüyorsa, işletim sistemi DISKI portaldan 100 GB 'a yeniden boyutlandırılır.

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

### <a name="suse-12-sp4suse-sles-12-for-sap-suse-sles-15-and-suse-sles-15-for-sap"></a>SUSE 12 SP4, SUSE SLES 12 for SAP, SUSE SLES 15 ve SAP için SUSE SLES 15

SUSE 12 SP4, SUSE SLES 15, ve SAP için SUSE SLES 15 ' te işletim sistemi diskinin boyutunu artırmak için:

1. VM'yi durdurun.
1. Portaldan OSDisk boyutunu artırın.
1. VM’yi yeniden başlatın.

VM yeniden başlatıldığında, aşağıdaki adımları gerçekleştirin:

   1. Aşağıdaki komutu kullanarak sanal makinenize **kök Kullanıcı** olarak erişin:
   
      `#sudo su`

   1. İşletim sistemi diskinin boyutunu artırmak için gerekli olan **gptfdisk** paketini yüklemek için aşağıdaki komutu kullanın:

      `#zypper install gptfdisk -y`

   1. Diskte bulunan en büyük kesimi görüntülemek için aşağıdaki komutu çalıştırın:

      `#sgdisk -e /dev/sda`

   1. Aşağıdaki komutu kullanarak bölümü silmeden bölümü yeniden boyutlandırın. **Genişletilmiş** komutun, Bölümü silmeden yeniden boyutlandırmak için **resizepart** adlı bir seçeneği vardır. Resizepart öğesinden sonraki 4 sayısı dördüncü (4) bölümün yeniden boyutlandırdığını gösterir.

      `#parted -s /dev/sda "resizepart 4 -1" quit`

   1. Bölümün arttırılmadığını denetlemek için `#lsblk` komutunu çalıştırın.

      Aşağıdaki çıktı, **/dev/sda4** bölümünün 98,5 GB olarak yeniden boyutlandırıldığını gösterir.

      ```
      user@myvm:~ # lsblk
      NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
      sda      8:0    0  100G  0 disk
      ├─sda1   8:1    0    2M  0 part
      ├─sda2   8:2    0  512M  0 part /boot/efi
      └─sda4   8:4    0 98.5G  0 part /
      sdb      8:16   0   20G  0 disk
      └─sdb1   8:17   0   20G  0 part /mnt/resource
      ```
      
   1. Aşağıdaki komutu kullanarak OSDisk üzerindeki dosya sistemi türünü belirler:

      `blkid`

      Örnek çıktı:

      ```
      #blkid

      user@myvm:~ # blkid
      /dev/sda1: PARTLABEL="p.legacy" PARTUUID="0122fd4c-0069-4a45-bfd4-98b97ccb6e8c"
      /dev/sda2: SEC_TYPE="msdos" LABEL_FATBOOT="EFI" LABEL="EFI" UUID="00A9-D170" TYPE="vfat" PARTLABEL="p.UEFI" PARTUUID="abac3cd8-949b-4e83-81b1-9636493388c7"
      /dev/sda3: LABEL="BOOT" UUID="aa2492db-f9ed-4f5a-822a-1233c06d57cc" TYPE="xfs" PARTLABEL="p.lxboot" PARTUUID="dfb36c61-b15f-4505-8e06-552cf1589cf7"
      /dev/sda4: LABEL="ROOT" UUID="26104965-251c-4e8d-b069-5f5323d2a9ba" TYPE="xfs" PARTLABEL="p.lxroot" PARTUUID="50fecee0-f22b-4406-94c3-622507e2dbce"
      /dev/sdb1: UUID="95239fce-ca97-4f03-a077-4e291588afc9" TYPE="ext4" PARTUUID="953afef3-01"
      ```

   1. Dosya sistemi türüne göre, dosya sistemini yeniden boyutlandırmak için uygun komutları kullanın.

      **XFS**için aşağıdaki komutu kullanın:

      ` #xfs_growfs /`

      Örnek çıktı:

      ```
      user@myvm:~ # xfs_growfs /
      meta-data=/dev/sda4              isize=512    agcount=4, agsize=1867583 blks
               =                       sectsz=512   attr=2, projid32bit=1
               =                       crc=1        finobt=1 spinodes=0 rmapbt=0
               =                       reflink=0
      data     =                       bsize=4096   blocks=7470331, imaxpct=25
               =                       sunit=0      swidth=0 blks
      naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
      log      =internal               bsize=4096   blocks=3647, version=2
               =                       sectsz=512   sunit=0 blks, lazy-count=1
      realtime =none                   extsz=4096   blocks=0, rtextents=0
      data blocks changed from 7470331 to 25820172
      ```

      **Ext4**için aşağıdaki komutu kullanın:

      ```#resize2fs /dev/sda4```

   1. Aşağıdaki komutu kullanarak **df**için daha önce dosya sistemi boyutunu doğrulayın:

      `#df -Th`

      Örnek çıktı:

      ```
      user@myvm:~ # df -Th
      Filesystem     Type      Size  Used Avail Use% Mounted on
      devtmpfs       devtmpfs  306M  4.0K  306M   1% /dev
      tmpfs          tmpfs     320M     0  320M   0% /dev/shm
      tmpfs          tmpfs     320M  8.8M  311M   3% /run
      tmpfs          tmpfs     320M     0  320M   0% /sys/fs/cgroup
      /dev/sda4      xfs        99G  1.8G   97G   2% /
      /dev/sda3      xfs      1014M   88M  927M   9% /boot
      /dev/sda2      vfat      512M  1.1M  511M   1% /boot/efi
      /dev/sdb1      ext4       20G   45M   19G   1% /mnt/resource
      tmpfs          tmpfs      64M     0   64M   0% /run/user/1000
      user@myvm:~ #
      ```

Yukarıdaki örnekte gösterildiği gibi, OSDisk için dosya sistemi boyutunu daha fazla görebiliriz.

### <a name="rhel-7x-with-lvm"></a>LVM ile RHEL 7. x

1. VM'yi durdurun.
1. Portaldan OSDisk boyutunu artırın.
1. VM’yi başlatın.

VM yeniden başlatıldığında, aşağıdaki adımları gerçekleştirin:

   1. Aşağıdaki komutu kullanarak sanal makinenize **kök Kullanıcı** olarak erişin:
   
      `#sudo su`

   1. İşletim sistemi diskinin boyutunu artırmak için gereken **gptfdisk** paketini yükler.

      `#yum install gdisk -y`

   1. Diskte bulunan en büyük kesimi görmek için şu komutu çalıştırın:

      `#sgdisk -e /dev/sda`

   1. Aşağıdaki komutu kullanarak bölümü silmeden bölümü yeniden boyutlandırın. **Genişletilmiş** komutun, Bölümü silmeden yeniden boyutlandırmak için **resizepart** adlı bir seçeneği vardır. Resizepart öğesinden sonraki 4 sayısı dördüncü (4) bölümün yeniden boyutlandırdığını gösterir.

      `#parted -s /dev/sda "resizepart 4 -1" quit`
    
   1. Bölümün arttığını doğrulamak için aşağıdaki komutu çalıştırın:

      `#lsblk`

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

   1. **Fiziksel birimi (BD)** yeniden boyutlandırmak için aşağıdaki komutu kullanın:

      `#pvresize /dev/sda4`

      Aşağıdaki çıktı BD 'in 99,02 GB olarak yeniden boyutlandırıldığını gösterir.

      ```
      [user@myvm ~]# pvresize /dev/sda4
      Physical volume "/dev/sda4" changed
      1 physical volume(s) resized or updated / 0 physical volume(s) not resized

      [user@myvm ~]# pvs
      PV         VG     Fmt  Attr PSize   PFree
      /dev/sda4  rootvg lvm2 a--  <99.02g <74.02g
      ```

   1. Aşağıdaki örnekte, `/dev/mapper/rootvg-rootlv` aşağıdaki komutu kullanarak 2 GB 'den 12 GB 'A (10 GB 'lik bir artış) yeniden boyutlandırılıyor ve bu da dosya sistemini yeniden boyutlandıracaktır:

      `#lvresize -r -L +10G /dev/mapper/rootvg-rootlv`

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
         
   1. Dosya sistemi `/dev/mapper/rootvg-rootlv` boyutunu arttırıp yükselmediğini veya aşağıdaki komutu kullanıp kullanmadığını doğrulayın:

      `#df -Th /`

      Örnek çıktı:

      ```
      [user@myvm ~]# df -Th /
      Filesystem                Type  Size  Used Avail Use% Mounted on
      /dev/mapper/rootvg-rootlv xfs    12G   71M   12G   1% /
      [user@myvm ~]#
      ```

      > [!NOTE]
      > Diğer mantıksal birimleri yeniden boyutlandırmak için aynı yordamı kullanmak için 7. adımda **LV** adını değiştirin

## <a name="next-steps"></a>Sonraki Adımlar

- [Diski yeniden boyutlandır](expand-disks.md)