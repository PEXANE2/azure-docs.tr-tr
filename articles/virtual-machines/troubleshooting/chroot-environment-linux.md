---
title: Linux Kurtarma sanal makinesinde chroot ortamı.
description: Bu makalede, Linux 'ta kurtarma sanal makinesinde (VM) Chroot ortamında nasıl sorun giderileceği açıklanır.
mservices: virtual-machines-linux
documentationcenter: ''
author: kailashmsft
manager: dcscontentpm
editor: ''
tags: ''
ms.service: virtual-machines-linux
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.date: 05/05/2020
ms.author: kaib
ms.openlocfilehash: d6605aa77cfa746f60fc3b23e120267a2614f7b1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82864688"
---
# <a name="chroot-environment-in-a-linux-rescue-vm"></a>Linux Kurtarma sanal makinesinde chroot ortamı

Bu makalede, Linux 'ta kurtarma sanal makinesinde (VM) Chroot ortamında nasıl sorun giderileceği açıklanır.

## <a name="ubuntu-16x--ubuntu-18x"></a>Ubuntu 16. x && Ubuntu 18. x

1. Etkilenen VM 'yi durdurun veya serbest bırakma.
1. Aynı işletim sistemi sürümünün aynı kaynak grubunda (RSG) ve yönetilen disk kullanarak konumda bir kurtarma VM görüntüsü oluşturun.
1. Etkilenen sanal makinenin işletim sistemi diskinin anlık görüntüsünü almak için Azure portal kullanın.
1. İşletim sistemi diskinin anlık görüntüsünden bir disk oluşturun ve bunu kurtarma VM 'sine bağlayın.
1. Disk oluşturulduktan sonra, kurtarma sanal makinesinde Chroot ortamında sorun giderin.

   1. Aşağıdaki komutu kullanarak VM 'nize kök kullanıcı olarak erişin:

      `#sudo su -`

   1. Kullanarak diski bulun `dmesg` (yeni diskinizi bulmak için kullandığınız yöntem farklılık gösterebilir). Aşağıdaki örnek, **SCSI** disklerinde filtrelemek için **dmesg** kullanır:

      `dmesg | grep SCSI`

      Çıktılarınız aşağıdaki örneğe benzer olacaktır. Bu örnekte, **SDC** diskini istiyoruz:

      ```
      [    0.294784] SCSI subsystem initialized
      [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
      [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
      [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
      [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
      ```

   1. Chroot ortamına erişmek için aşağıdaki komutları kullanın:

      ```
      #mkdir /rescue
      #mount /dev/sdc1 /rescue
      #mount /dev/sdc15 /rescue/boot/efi
      #cd /rescue

      #mount -t proc proc proc
      #mount -t sysfs sys sys/
      #mount -o bind /dev dev/
      #mount -o bind /dev/pts dev/pts/
      #mount -o bind /run run/
      #chroot /rescue
      ```

   1. Chroot ortamında sorun giderin.

   1. Chroot ortamından çıkmak için aşağıdaki komutları kullanın:

      ```
      #exit

      #umount /rescue/proc/
      #umount /rescue/sys/
      #umount /rescue/dev/pts
      #umount /rescue/dev/
      #umount /rescue/run
      #cd /
      #umount /rescue/boot/efi
      #umount /rescue
      ```

      > [!NOTE]
      > Hatayı alırsanız `unable to unmount /rescue` , umorekıt komutuna bir-l seçeneğini ekleyin.
      >
      > Örnek: `umount -l /rescue`

1. Diski Kurtarma VM 'sinden ayırın ve özgün VM ile bir disk değiştirme işlemi gerçekleştirin.
1. Özgün VM 'yi başlatın ve bağlantısını denetleyin.

## <a name="rhelcentosoracle-6x--oracle-8x--rhelcentos-7x-with-raw-partitions"></a>RHEL/CentOS/Oracle 6. x && Oracle 8. x && RHEL/CentOS 7. x ham bölümlerle

1. Etkilenen VM 'yi durdurun veya serbest bırakma.
1. Aynı işletim sistemi sürümünün aynı kaynak grubunda (RSG) ve yönetilen disk kullanarak konumda bir kurtarma VM görüntüsü oluşturun.
1. Etkilenen sanal makinenin işletim sistemi diskinin anlık görüntüsünü almak için Azure portal kullanın.
1. İşletim sistemi diskinin anlık görüntüsünden bir disk oluşturun ve bunu kurtarma VM 'sine bağlayın.
1. Disk oluşturulduktan sonra, kurtarma sanal makinesinde Chroot ortamında sorun giderin.

   1. Aşağıdaki komutu kullanarak VM 'nize kök kullanıcı olarak erişin:

      `#sudo su -`

   1. Kullanarak diski bulun `dmesg` (yeni diskinizi bulmak için kullandığınız yöntem farklılık gösterebilir). Aşağıdaki örnek, **SCSI** disklerinde filtrelemek için **dmesg** kullanır:

      `dmesg | grep SCSI`

      Çıktılarınız aşağıdaki örneğe benzer olacaktır. Bu örnekte, **SDC** diskini istiyoruz:

      ```
      [    0.294784] SCSI subsystem initialized
      [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
      [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
      [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
      [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
      ```

   1. Chroot ortamına erişmek için aşağıdaki komutları kullanın:

      ```
      #mkdir /rescue
      #mount -o nouuid /dev/sdc2 /rescue
      #mount -o nouuid /dev/sdc1 /rescue/boot/
      #cd /rescue

      #mount -t proc proc proc
      #mount -t sysfs sys sys/
      #mount -o bind /dev dev/
      #mount -o bind /dev/pts dev/pts/
      #mount -o bind /run run/
      #chroot /rescue
      ```

   1. Chroot ortamında sorun giderin.

   1. Chroot ortamından çıkmak için aşağıdaki komutları kullanın:

      ```
      #exit

      #umount /rescue/proc/
      #umount /rescue/sys/
      #umount /rescue/dev/pts
      #umount /rescue/dev/
      #umount /rescue/run
      #cd /
      #umount /rescue/boot/efi
      #umount /rescue
      ```

      > [!NOTE]
      > Hatayı alırsanız `unable to unmount /rescue` , umorekıt komutuna bir-l seçeneğini ekleyin.
      >
      > Örnek: `umount -l /rescue`

1. Diski Kurtarma VM 'sinden ayırın ve özgün VM ile bir disk değiştirme işlemi gerçekleştirin.
1. Özgün VM 'yi başlatın ve bağlantısını denetleyin.

## <a name="rhelcentos-7x-with-lvm"></a>RHEL/CentOS 7. x LVM

   > [!NOTE]
   > Özgün VM 'niz işletim sistemi diskine mantıksal birim Yöneticisi (LVM) içeriyorsa, işletim sistemi diskinde ham bölümlerin bulunduğu görüntüyü kullanarak kurtarma VM 'sini oluşturun.

1. Etkilenen VM 'yi durdurun veya serbest bırakma.
1. Aynı işletim sistemi sürümünün aynı kaynak grubunda (RSG) ve yönetilen disk kullanarak konumda bir kurtarma VM görüntüsü oluşturun.
1. Etkilenen sanal makinenin işletim sistemi diskinin anlık görüntüsünü almak için Azure portal kullanın.
1. İşletim sistemi diskinin anlık görüntüsünden bir disk oluşturun ve bunu kurtarma VM 'sine bağlayın.
1. Disk oluşturulduktan sonra, kurtarma sanal makinesinde Chroot ortamında sorun giderin.

   1. Aşağıdaki komutu kullanarak VM 'nize kök kullanıcı olarak erişin:

      `#sudo su -`

   1. Kullanarak diski bulun `dmesg` (yeni diskinizi bulmak için kullandığınız yöntem farklılık gösterebilir). Aşağıdaki örnek, **SCSI** disklerinde filtrelemek için **dmesg** kullanır:

      `dmesg | grep SCSI`

      Çıktılarınız aşağıdaki örneğe benzer olacaktır. Bu örnekte, **SDC** diskini istiyoruz:

      ```
      [    0.294784] SCSI subsystem initialized
      [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
      [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
      [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
      [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
      ```

   1. Mantıksal birim grubunu etkinleştirmek için aşağıdaki komutu kullanın:

      ```
      #vgscan --mknodes
      #vgchange -ay
      #lvscan
      ```

   1. `lsblk`LVM adlarını almak için komutunu kullanın:

      ```
      [user@myvm ~]$ lsblk
      NAME              MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
      sda                 8:0    0   64G  0 disk
      ├─sda1              8:1    0  500M  0 part /boot
      ├─sda2              8:2    0   63G  0 part /
      sdb                 8:16   0    4G  0 disk
      └─sdb1              8:17   0    4G  0 part /mnt/resource
      sdc                 8:0    0   64G  0 disk
      ├─sdc1              8:1    0  500M  0 part
      ├─sdc2              8:2    0   63G  0 part
      ├─sdc3              8:3    0    2M  0 part
      ├─sdc4              8:4    0   63G  0 part
        ├─rootvg-tmplv  253:0    0    2G  0 lvm  
        ├─rootvg-usrlv  253:1    0   10G  0 lvm  
        ├─rootvg-optlv  253:2    0    2G  0 lvm  
        ├─rootvg-homelv 253:3    0    1G  0 lvm  
        ├─rootvg-varlv  253:4    0    8G  0 lvm  
        └─rootvg-rootlv 253:5    0    2G  0 lvm
      ```

   1. Chroot ortamına erişmek için aşağıdaki komutları kullanın:

      ```
      #mkdir /rescue
      #mount /dev/mapper/rootvg-rootlv /rescue
      #mount /dev/mapper/rootvg-varlv /rescue/var
      #mount /dev/mapper/rootvg-homelv /rescue/home
      #mount /dev/mapper/rootvg-usrlv /rescue/usr
      #mount /dev/mapper/rootvg-tmplv /rescue/tmp
      #mount /dev/mapper/rootvg-optlv /rescue/opt
      #mount /dev/sdc2 /rescue/boot/
      #mount /dev/sdc1 /rescue/boot/efi
      #cd /rescue

      #mount -t proc proc proc
      #mount -t sysfs sys sys/
      #mount -o bind /dev dev/
      #mount -o bind /dev/pts dev/pts/
      #mount -o bind /run run/
      #chroot /rescue
      ```

   1. Chroot ortamında sorun giderin.

   1. Chroot ortamından çıkmak için aşağıdaki komutları kullanın:

      ```
      #exit

      #umount /rescue/proc/
      #umount /rescue/sys/
      #umount /rescue/dev/pts
      #umount /rescue/dev/
      #umount /rescue/run
      #cd /
      #umount /rescue/boot/efi
      #umount /rescue/boot
      #umount /rescue/home
      #umount /rescue/var
      #umount /rescue/usr
      #umount /rescue/tmp
      #umount /rescue/opt
      #umount /rescue
      ```

      > [!NOTE]
      > Hatayı alırsanız `unable to unmount /rescue` , umorekıt komutuna bir-l seçeneğini ekleyin.
      >
      > Örnek: `umount -l /rescue`

1. Diski Kurtarma VM 'sinden ayırın ve özgün VM ile bir disk değiştirme işlemi gerçekleştirin.
1. Özgün VM 'yi başlatın ve bağlantısını denetleyin.

## <a name="rhel-8x-with-lvm"></a>LVM ile RHEL 8. x

   > [!NOTE]
   > Özgün VM 'niz işletim sistemi diskine mantıksal birim Yöneticisi (LVM) içeriyorsa, işletim sistemi diskinde ham bölümlerin bulunduğu görüntüyü kullanarak kurtarma VM 'sini oluşturun.

1. Etkilenen VM 'yi durdurun veya serbest bırakma.
1. Aynı işletim sistemi sürümünün aynı kaynak grubunda (RSG) ve yönetilen disk kullanarak konumda bir kurtarma VM görüntüsü oluşturun.
1. Etkilenen sanal makinenin işletim sistemi diskinin anlık görüntüsünü almak için Azure portal kullanın.
1. İşletim sistemi diskinin anlık görüntüsünden bir disk oluşturun ve bunu kurtarma VM 'sine bağlayın.
1. Disk oluşturulduktan sonra, kurtarma sanal makinesinde Chroot ortamında sorun giderin.

   1. Aşağıdaki komutu kullanarak VM 'nize kök kullanıcı olarak erişin:

      `#sudo su -`

   1. Kullanarak diski bulun `dmesg` (yeni diskinizi bulmak için kullandığınız yöntem farklılık gösterebilir). Aşağıdaki örnek, **SCSI** disklerinde filtrelemek için **dmesg** kullanır:

      `dmesg | grep SCSI`

      Çıktılarınız aşağıdaki örneğe benzer olacaktır. Bu örnekte, **SDC** diskini istiyoruz:

      ```
      [    0.294784] SCSI subsystem initialized
      [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
      [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
      [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
      [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
      ```

   1. Mantıksal birim grubunu etkinleştirmek için aşağıdaki komutu kullanın:

      ```
      #vgscan --mknodes
      #vgchange -ay
      #lvscan
      ```

   1. `lsblk`LVM adlarını almak için komutunu kullanın:

      ```
      [user@myvm ~]$ lsblk
      NAME              MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
      sda                 8:0    0   64G  0 disk
      ├─sda1              8:1    0  500M  0 part /boot
      ├─sda2              8:2    0   63G  0 part /
      sdb                 8:16   0    4G  0 disk
      └─sdb1              8:17   0    4G  0 part /mnt/resource
      sdc                 8:0    0   64G  0 disk
      ├─sdc1              8:1    0  500M  0 part
      ├─sdc2              8:2    0   63G  0 part
      │ ├─rootvg-tmplv  253:0    0    2G  0 lvm  
      │ ├─rootvg-usrlv  253:1    0   10G  0 lvm  
      │ ├─rootvg-homelv 253:2    0    1G  0 lvm  
      │ ├─rootvg-varlv  253:3    0    8G  0 lvm  
      │ └─rootvg-rootlv 253:4    0    2G  0 lvm  
      ├─sdc14             8:14   0    4M  0 part
      └─sdc15             8:15   0  495M  0 part
      ```

   1. Chroot ortamına erişmek için aşağıdaki komutları kullanın:

      ```
      #mkdir /rescue
      #mount /dev/mapper/rootvg-rootlv /rescue
      #mount /dev/mapper/rootvg-varlv /rescue/var
      #mount /dev/mapper/rootvg-homelv /rescue/home
      #mount /dev/mapper/rootvg-usrlv /rescue/usr
      #mount /dev/mapper/rootvg-tmplv /rescue/tmp
      #mount /dev/sdc1 /rescue/boot/
      #mount /dev/sdc15 /rescue/boot/efi
      #cd /rescue

      #mount -t proc proc proc
      #mount -t sysfs sys sys/
      #mount -o bind /dev dev/
      #mount -o bind /dev/pts dev/pts/
      #mount -o bind /run run/
      #chroot /rescue
      ```

   1. Chroot ortamında sorun giderin.

   1. Chroot ortamından çıkmak için aşağıdaki komutları kullanın:

      ```
      #exit

      #umount /rescue/proc/
      #umount /rescue/sys/
      #umount /rescue/dev/pts
      #umount /rescue/dev/
      #umount /rescue/run
      #cd /
      #umount /rescue/boot/efi
      #umount /rescue/boot
      #umount /rescue/home
      #umount /rescue/var
      #umount /rescue/usr
      #umount /rescue/tmp
      #umount /rescue
      ```

      > [!NOTE]
      > Hatayı alırsanız `unable to unmount /rescue` , umorekıt komutuna bir-l seçeneğini ekleyin.
      >
      > Örnek: `umount -l /rescue`

1. Diski Kurtarma VM 'sinden ayırın ve özgün VM ile bir disk değiştirme işlemi gerçekleştirin.
1. Özgün VM 'yi başlatın ve bağlantısını denetleyin.

## <a name="oracle-7x"></a>Oracle 7. x

1. Etkilenen VM 'yi durdurun veya serbest bırakma.
1. Aynı işletim sistemi sürümünün aynı kaynak grubunda (RSG) ve yönetilen disk kullanarak konumda bir kurtarma VM görüntüsü oluşturun.
1. Etkilenen sanal makinenin işletim sistemi diskinin anlık görüntüsünü almak için Azure portal kullanın.
1. İşletim sistemi diskinin anlık görüntüsünden bir disk oluşturun ve bunu kurtarma VM 'sine bağlayın.
1. Disk oluşturulduktan sonra, kurtarma sanal makinesinde Chroot ortamında sorun giderin.

   1. Aşağıdaki komutu kullanarak VM 'nize kök kullanıcı olarak erişin:

      `#sudo su -`

   1. Kullanarak diski bulun `dmesg` (yeni diskinizi bulmak için kullandığınız yöntem farklılık gösterebilir). Aşağıdaki örnek, **SCSI** disklerinde filtrelemek için **dmesg** kullanır:

      `dmesg | grep SCSI`

      Çıktılarınız aşağıdaki örneğe benzer olacaktır. Bu örnekte, **SDC** diskini istiyoruz:

      ```
      [    0.294784] SCSI subsystem initialized
      [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
      [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
      [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
      [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
      ```

   1. Chroot ortamına erişmek için aşağıdaki komutları kullanın:

      ```
      #mkdir /rescue
      #mount -o nouuid /dev/sdc2 /rescue
      #mount -o nouuid /dev/sdc1 /rescue/boot/
      #mount /dev/sdc15 /rescue/boot/efi
      #cd /rescue

      #mount -t proc proc proc
      #mount -t sysfs sys sys/
      #mount -o bind /dev dev/
      #mount -o bind /dev/pts dev/pts/
      #mount -o bind /run run/
      ##chroot /rescue
      ```

   1. Chroot ortamında sorun giderin.

   1. Chroot ortamından çıkmak için aşağıdaki komutları kullanın:

      ```
      #exit

      #umount /rescue/proc/
      #umount /rescue/sys/
      #umount /rescue/dev/pts
      #umount /rescue/dev/
      #umount /rescue/run
      #cd /
      #umount /rescue/boot/efi
      #umount /rescue/boot
      #umount /rescue
      ```

      > [!NOTE]
      > Hatayı alırsanız `unable to unmount /rescue` , umorekıt komutuna bir-l seçeneğini ekleyin.
      >
      > Örnek: `umount -l /rescue`

1. Diski Kurtarma VM 'sinden ayırın ve özgün VM ile bir disk değiştirme işlemi gerçekleştirin.
1. Özgün VM 'yi başlatın ve bağlantısını denetleyin.

## <a name="suse-sles-12-sp4-suse-sles-12-sp4-for-sap---suse-sles-15-sp1-suse-sles-15-sp1-for-sap"></a>SUSE-SLES 12 SP4, SUSE-SLES 12 SP4 for SAP &&  # # SUSE-SLES 15 SP1, SUSE-SLES 15 SP1 for SAP

1. Etkilenen VM 'yi durdurun veya serbest bırakma.
1. Aynı işletim sistemi sürümünün aynı kaynak grubunda (RSG) ve yönetilen disk kullanarak konumda bir kurtarma VM görüntüsü oluşturun.
1. Etkilenen sanal makinenin işletim sistemi diskinin anlık görüntüsünü almak için Azure portal kullanın.
1. İşletim sistemi diskinin anlık görüntüsünden bir disk oluşturun ve bunu kurtarma VM 'sine bağlayın.
1. Disk oluşturulduktan sonra, kurtarma sanal makinesinde Chroot ortamında sorun giderin.

   1. Aşağıdaki komutu kullanarak VM 'nize kök kullanıcı olarak erişin:

      `#sudo su -`

   1. Kullanarak diski bulun `dmesg` (yeni diskinizi bulmak için kullandığınız yöntem farklılık gösterebilir). Aşağıdaki örnek, **SCSI** disklerinde filtrelemek için **dmesg** kullanır:

      `dmesg | grep SCSI`

      Çıktılarınız aşağıdaki örneğe benzer olacaktır. Bu örnekte, **SDC** diskini istiyoruz:

      ```
      [    0.294784] SCSI subsystem initialized
      [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
      [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
      [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
      [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
      ```

   1. Chroot ortamına erişmek için aşağıdaki komutları kullanın:

      ```
      #mkdir /rescue
      #mount -o nouuid /dev/sdc4 /rescue
      #mount -o nouuid /dev/sdc3 /rescue/boot/
      #mount /dev/sdc2 /rescue/boot/efi
      #cd /rescue

      #mount -t proc proc proc
      #mount -t sysfs sys sys/
      #mount -o bind /dev dev/
      #mount -o bind /dev/pts dev/pts/
      #mount -o bind /run run/
      #chroot /rescue
      ```

   1. Chroot ortamında sorun giderin.

   1. Chroot ortamından çıkmak için aşağıdaki komutları kullanın:

      ```
      #exit

      #umount /rescue/proc/
      #umount /rescue/sys/
      #umount /rescue/dev/pts
      #umount /rescue/dev/
      #umount /rescue/run
      #cd /
      #umount /rescue/boot/efi
      #umount /rescue/boot
      #umount /rescue
      ```

      > [!NOTE]
      > Hatayı alırsanız `unable to unmount /rescue` , umorekıt komutuna bir-l seçeneğini ekleyin.
      >
      > Örnek: `umount -l /rescue`

1. Diski Kurtarma VM 'sinden ayırın ve özgün VM ile bir disk değiştirme işlemi gerçekleştirin.
1. Özgün VM 'yi başlatın ve bağlantısını denetleyin.

## <a name="next-steps"></a>Sonraki Adımlar

- [SSH bağlantısı sorunlarını giderme](troubleshoot-ssh-connection.md)