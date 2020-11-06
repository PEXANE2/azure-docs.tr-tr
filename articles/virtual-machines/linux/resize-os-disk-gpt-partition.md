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
ms.openlocfilehash: 99b723322ce7636edce3ae5b59a69b96e288ca24
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93392699"
---
# <a name="resize-an-os-disk-that-has-a-gpt-partition"></a>GPT bölümü olan bir işletim sistemi diskini yeniden boyutlandırma

> [!NOTE]
> Bu senaryo yalnızca GUID bölümleme tablosu (GPT) bölümü olan işletim sistemi diskleri için geçerlidir.

Bu makalede, Linux 'ta GPT bölümü olan bir işletim sistemi diskinin boyutunu artırma işlemi açıklanır. 

## <a name="identify-whether-the-os-disk-has-an-mbr-or-gpt-partition"></a>İşletim sistemi diskinde MBR veya GPT bölümü olup olmadığını belirler

`parted`Disk bölümünün ana önyükleme kaydı (MBR) bölümüyle veya BIR GPT bölümüyle oluşturulmuş olup olmadığını belirlemek için komutunu kullanın.

### <a name="mbr-partition"></a>MBR bölümü

Aşağıdaki çıktıda, **bölüm tablosunda** **Msdos** değeri gösterilmektedir. Bu değer bir MBR bölümünü tanımlar.

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

Aşağıdaki çıktıda, **bölüm tablosu** **GPT** değerini gösterir. Bu değer bir GPT bölümünü tanımlar.

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
   
   **XFS** için aşağıdaki komutu kullanın:
   
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
   
   **Ext4** için aşağıdaki komutu kullanın:
   
   ```
   #resize2fs /dev/sda4
   ```
   
1. Aşağıdaki komutu kullanarak **df** için daha fazla dosya sistemi boyutunu doğrulayın:
   
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

### <a name="rhel-with-lvm"></a>LVM ile RHEL

1. Aşağıdaki komutu kullanarak sanal makinenize **kök** Kullanıcı olarak erişin:

   ```bash
   [root@dd-rhel7vm ~]# sudo -i
   ```

1. `lsblk`FileSystem ("/") köküne hangi mantıksal birimin (LV) bağlı olduğunu bulmak için komutunu kullanın. Bu durumda, **_rootvg-rootlv_*_ ' nin _* / üzerine takıldığını** görüyoruz.  Başka bir dosya sistemi isteniyorsa, bu belge aracılığıyla LV ve bağlama noktasını değiştirin.

   ```shell
   [root@dd-rhel7vm ~]# lsblk -f
   NAME                  FSTYPE      LABEL   UUID                                   MOUNTPOINT
   fd0
   sda
   ├─sda1                vfat                C13D-C339                              /boot/efi
   ├─sda2                xfs                 8cc4c23c-fa7b-4a4d-bba8-4108b7ac0135   /boot
   ├─sda3
   └─sda4                LVM2_member         zx0Lio-2YsN-ukmz-BvAY-LCKb-kRU0-ReRBzh
      ├─rootvg-tmplv      xfs                 174c3c3a-9e65-409a-af59-5204a5c00550   /tmp
      ├─rootvg-usrlv      xfs                 a48dbaac-75d4-4cf6-a5e6-dcd3ffed9af1   /usr
      ├─rootvg-optlv      xfs                 85fe8660-9acb-48b8-98aa-bf16f14b9587   /opt
      ├─rootvg-homelv     xfs                 b22432b1-c905-492b-a27f-199c1a6497e7   /home
      ├─rootvg-varlv      xfs                 24ad0b4e-1b6b-45e7-9605-8aca02d20d22   /var
      └─rootvg-rootlv     xfs                 4f3e6f40-61bf-4866-a7ae-5c6a94675193   /
   ```

1. Kök bölümü içeren LVM birim grubunda boş alan olup olmadığını denetleyin.  Boş alan varsa, **12** . adıma atlayın

   ```bash
   [root@dd-rhel7vm ~]# vgdisplay rootvg
   --- Volume group ---
   VG Name               rootvg
   System ID
   Format                lvm2
   Metadata Areas        1
   Metadata Sequence No  7
   VG Access             read/write
   VG Status             resizable
   MAX LV                0
   Cur LV                6
   Open LV               6
   Max PV                0
   Cur PV                1
   Act PV                1
   VG Size               <63.02 GiB
   PE Size               4.00 MiB
   Total PE              16132
   Alloc PE / Size       6400 / 25.00 GiB
   Free  PE / Size       9732 / <38.02 GiB
   VG UUID               lPUfnV-3aYT-zDJJ-JaPX-L2d7-n8sL-A9AgJb
   ```

   Bu örnekte, satır **boş PE/boyut** , birim grubunda 38.02 GB boş olduğunu belirtir.  Birim grubuna boşluk eklenmeden önce disk yeniden boyutlandırma gerekmez

1. RHEL 7. x içindeki işletim sistemi diskinin boyutunu LVM ile artırmak için:

   1. VM'yi durdurun.
   1. Portaldan işletim sistemi diskinin boyutunu artırın.
   1. VM’yi başlatın.

1. VM yeniden başlatıldığında, aşağıdaki adımları gerçekleştirin:

   1. İşletim sistemi diskinin boyutunu artırmak için gerekli olan **growpart** komutunu sağlamak için **Cloud-utils-growpart** paketini yükler.

      Bu paket, Azure Marketi görüntülerinin çoğunda önceden yüklenir.

      ```bash
      [root@dd-rhel7vm ~]# yum install cloud-utils-growpart
      ```

1. Fiziksel VG adlı birim grubunda (VG), **pvscan** komutuyla hangi disk ve bölümün LVM fiziksel BIRIMLERINI (BD) bulundurduğunu belirleme.  **[]** Ayraçları arasında listelenen boyut ve boş alanı göz önünde atın.

   ```bash
   [root@dd-rhel7vm ~]# pvscan
     PV /dev/sda4   VG rootvg          lvm2 [<63.02 GiB / <38.02 GiB free]
   ```

1. **Lsblk** ile bölümün boyutunu doğrulayın.  Göz atın 

   ```bash
   [root@dd-rhel7vm ~]# lsblk /dev/sda4
   NAME            MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
   sda4              8:4    0  63G  0 part
   ├─rootvg-tmplv  253:1    0   2G  0 lvm  /tmp
   ├─rootvg-usrlv  253:2    0  10G  0 lvm  /usr
   ├─rootvg-optlv  253:3    0   2G  0 lvm  /opt
   ├─rootvg-homelv 253:4    0   1G  0 lvm  /home
   ├─rootvg-varlv  253:5    0   8G  0 lvm  /var
   └─rootvg-rootlv 253:6    0   2G  0 lvm  /
   ```

1. **Growpart** , cihaz adı ve bölüm numarası kullanarak bu BD içeren bölümü genişletin.  Bu işlem, cihazdaki tüm boş alanı kullanmak için belirtilen bölümü genişletir.

   ```bash
   [root@dd-rhel7vm ~]# growpart /dev/sda 4
   CHANGED: partition=4 start=2054144 old: size=132161536 end=134215680 new: size=199272414 end=201326558
   ```

1. Bölümün, **lsblk** komutuyla beklenen boyuta yeniden boyutlandırıldığından emin olun.  Örnekte sda4, 63G 'den 95G 'ye değiştirildiğine dikkat edin.

   ```bash
   [root@dd-rhel7vm ~]# lsblk /dev/sda4
   NAME            MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
   sda4              8:4    0  95G  0 part
   ├─rootvg-tmplv  253:1    0   2G  0 lvm  /tmp
   ├─rootvg-usrlv  253:2    0  10G  0 lvm  /usr
   ├─rootvg-optlv  253:3    0   2G  0 lvm  /opt
   ├─rootvg-homelv 253:4    0   1G  0 lvm  /home
   ├─rootvg-varlv  253:5    0   8G  0 lvm  /var
   └─rootvg-rootlv 253:6    0   2G  0 lvm  /
   ```

1. Yeni genişletilen bölümün geri kalanını kullanmak için BD 'i genişletin

   ```bash
   [root@dd-rhel7vm ~]# pvresize /dev/sda4
   Physical volume "/dev/sda4" changed
   1 physical volume(s) resized or updated / 0 physical volume(s) not resized
   ```

1. BD 'in yeni boyutunu, orijinal **[boyut/boş]** değerleriyle karşılaştırarak beklenen boyutta olduğunu doğrulayın.

   ```bash
   [root@dd-rhel7vm ~]# pvscan
   PV /dev/sda4   VG rootvg          lvm2 [<95.02 GiB / <70.02 GiB free]
   ```

1. İstenen mantıksal birimi (LV) istenen miktarda genişleterek, birim grubundaki tüm boş alan olması gerekmez.  Aşağıdaki örnekte **/dev/mapper/rootvg-rootlv** , aşağıdaki KOMUTLA 2 GB Ila 12 GB (10 GB 'lık artış) olarak yeniden boyutlandırılıyor. Bu komut ayrıca dosya sistemini yeniden boyutlandıracaktır.

   ```bash
   [root@dd-rhel7vm ~]# lvresize -r -L +10G /dev/mapper/rootvg-rootlv
   ```

   Örnek çıktı:

   ```bash
   [root@dd-rhel7vm ~]# lvresize -r -L +10G /dev/mapper/rootvg-rootlv
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

1. Lvresize komutu, LV içindeki dosya sistemi için uygun yeniden boyutlandırma komutunu otomatik olarak çağırır. ' A bağlı olan **/dev/mapper/rootvg-rootlv** 'nin, **/** aşağıdaki komutu kullanarak daha fazla dosya sistemi boyutuna sahip olup olmadığını doğrulayın:

   ```shell
   [root@dd-rhel7vm ~]# df -Th /
   ```

   Örnek çıktı:

   ```shell
   [root@dd-rhel7vm ~]# df -Th /
   Filesystem                Type  Size  Used Avail Use% Mounted on
   /dev/mapper/rootvg-rootlv xfs    12G   71M   12G   1% /
   [root@dd-rhel7vm ~]#
   ```

> [!NOTE]
> Diğer herhangi bir mantıksal birimi yeniden boyutlandırmak için aynı yordamı kullanmak için, adım **12** ' de **LV** adı ' nı değiştirin.

### <a name="rhel-raw"></a>RHEL RAW
>[!NOTE]
>İşletim sistemi disk boyutunu arttırmadan önce her zaman sanal makinenin anlık görüntüsünü alın.

RHEL içindeki işletim sistemi diskinin boyutunu ham bölümle artırmak için:

VM'yi durdurun.
Portaldan işletim sistemi diskinin boyutunu artırın.
VM’yi başlatın.
VM yeniden başlatıldığında, aşağıdaki adımları gerçekleştirin:

1. Aşağıdaki komutu kullanarak sanal makinenize **kök** Kullanıcı olarak erişin:
 
   ```
   sudo su
   ```

1. İşletim sistemi diskinin boyutunu artırmak için gereken **gptfdisk** paketini yükler.

   ```
   yum install gdisk -y
   ```

1.  Diskte bulunan tüm kesimleri görmek için şu komutu çalıştırın:
    ```
    gdisk -l /dev/sda
    ```

1. Bölüm türünü bildiren ayrıntıları görürsünüz. GPT olduğundan emin olun. Kök bölümü belirler. Önyükleme bölümünü (BIOS önyükleme bölümü) ve sistem bölümünü değiştirme veya silme (' EFı sistem bölümü ')

1. Bölümlemeyi ilk kez başlatmak için aşağıdaki komutu kullanın. 
    ```
    gdisk /dev/sda
    ```

1. Şimdi bir sonraki komuta (' komut:?) soran bir ileti görürsünüz. yardım için '). 

   ```
   w
   ```

1. Şöyle bir uyarı alacaksınız "uyarı! İkincil üst bilgi diskte çok erken yerleştirilmiş! Bu sorunu düzeltmek istiyor musunuz? (E/H): ". ' Y ' tuşuna basmanız gerekir

   ```
   Y
   ```

1. Son denetimlerin tamamlandığını bildiren ve onay isteyen bir ileti görmeniz gerekir. ' Y ' tuşuna basın

   ```
   Y
   ```

1. Partaraştırması komutu kullanılarak her şeyin doğru şekilde gerçekleştiğini denetleyin

   ```
   partprobe
   ```

1. Yukarıdaki adımlarda ikincil GPT üstbilgisinin sonuna yerleştirilmiş olması gerekir. Sonraki adım, GDisk aracını tekrar kullanarak yeniden boyutlandırma sürecini başlatmakta. Aşağıdaki komutu kullanın.

   ```
   gdisk /dev/sda
   ```
1. Komut menüsünde, bölüm listesini görmek için ' p ' tuşuna basın. Kök bölümü (adımlarda, sda2 kök bölüm olarak kabul edilir) ve önyükleme bölümünü (adımlarda, sda3, önyükleme bölümü olarak kabul edilir) belirler 

   ```
   p
   ```
    ![Kök bölüm ve önyükleme bölümü](./media/resize-os-disk-rhelraw/resize-os-disk-rhelraw1.png)

1. Bölümü silmek için ' a basın ve önyükleme için atanan bölüm numarasını seçin (Bu örnekte, ' 3 ')
   ```
   d
   3
   ```
1. Bölümü silmek için ' a basın ve önyükleme için atanan bölüm numarasını seçin (Bu örnekte, ' 2 ' olur)
   ```
   d
   2
   ```
    ![Kök bölümü ve önyükleme bölümünü Sil](./media/resize-os-disk-rhelraw/resize-os-disk-rhelraw2.png)

1. Kök bölümü artan boyut ile yeniden oluşturmak için, ' n ' tuşuna basın, daha önce kök için sildiğiniz bölüm numarasını (Bu örnek için ' 2 ') girin ve Ilk kesimi ' varsayılan değer ', son sektör ' son sektör değeri-önyükleme boyutu kesimi ' (Bu durumda, 2MB önyüklemesine karşılık gelen ' 4096 ') ve onaltılık kod ' 8300 ' olarak seçin
   ```
   n
   2
   (Enter default)
   (Calculateed value of Last sector value - 4096)
   8300
   ```
1. Önyükleme bölümünü yeniden oluşturmak için ' n ' tuşuna basın, daha önce önyükleme için sildiğiniz bölüm numarasını (Bu örnek için ' 3 ') girin ve Ilk kesimi ' varsayılan değer ', son kesim ' varsayılan değer ' ve onaltılık kod ' EF02 ' olarak seçin
   ```
   n
   3
   (Enter default)
   (Enter default)
   EF02
   ```

1. Değişiklikleri ' w ' komutuyla yazın ve ' Y ' tuşuna basın
   ```
   w
   Y
   ```
1. Disk kararlılığını denetlemek için ' partaraştırması ' komutunu çalıştırın
   ```
   partprobe
   ```
1. VM 'yi yeniden başlatın ve kök bölüm boyutu artmıştır
   ```
   reboot
   ```

   ![Yeni kök bölüm ve önyükleme bölümü](./media/resize-os-disk-rhelraw/resize-os-disk-rhelraw3.png)

1. Yeniden boyutlandırmak için bölümdeki xfs_growfs komutunu çalıştırın
   ```
   xfs_growfs /dev/sda2
   ```

   ![XFS büyüme FS](./media/resize-os-disk-rhelraw/resize-os-disk-rhelraw4.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Diski yeniden boyutlandır](expand-disks.md)
