---
title: Dosya sistemi hataları nedeniyle Linux VM başlangıç sorunları | Microsoft Dokümanlar
description: Linux VM'nin neden başlayamadığını ve sorunu nasıl çözeceğini açıklar.
services: virtual-machines-linux
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: ''
ms.service: virtual-machines-linux
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 10/09/2019
ms.author: v-six
ms.openlocfilehash: 455cb1e0067217be6edcf665e8c07e8fcd684ab5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76842410"
---
# <a name="troubleshoot-linux-vm-starting-issues-due-to-file-system-errors"></a>Dosya sistemi hataları nedeniyle Linux VM başlangıç sorunlarını giderme

Secure Shell (SSH) kullanarak bir Azure Linux sanal makinesine (VM) bağlanamazsınız. [Azure portalında](https://portal.azure.com/)Önyükleme Tanılama özelliğini çalıştırdığınızda, aşağıdaki örneklere benzeyen günlük girişleri görürsünüz.

## <a name="examples"></a>Örnekler

Aşağıda olası hatalara örnekler verilmiştir.

### <a name="example-1"></a>Örnek 1 

```
Checking all file systems.
[/sbin/fsck.ext4 (1) — /] fsck.ext4 -a /dev/sda1
/dev/sda1 contains a file system with errors, check forced.
/dev/sda1: Inodes that were part of a corrupted orphan linked list found.
/dev/sda1: UNEXPECTED INCONSISTENCY; RUN fsck MANUALLY
```

### <a name="example-2"></a>Örnek 2

```
EXT4-fs (sda1): INFO: recovery required on readonly filesystem
EXT4-fs (sda1): write access will be enabled during recovery
EXT4-fs warning (device sda1): ext4_clear_journal_err:4531: Filesystem error recorded from previous mount: IO failure
EXT4-fs warning (device sda1): ext4_clear_journal_err:4532: Making fs in need of filesystem check.
```

### <a name="example-3"></a>Örnek 3

```
[  14.252404] EXT4-fs (sda1): Couldn’t remount RDWR because of unprocessed orphan inode list.  Please unmount/remount instead
An error occurred while mounting /.
```

### <a name="example-4"></a>Örnek 4 

Bu örnek temiz bir fsck neden olur. Bu durumda, VM (/dev/sdc1 ve /dev/sde1) ek veri diskleri de vardır.

```
Checking all file systems. 
[/sbin/fsck.ext4 (1) — /] fsck.ext4 -a /dev/sda1
/dev/sda1: clean, 65405/1905008 files, 732749/7608064 blocks
[/sbin/fsck.ext4 (1) — /tmp] fsck.ext4 -a /dev/sdc1
[/sbin/fsck.ext4 (2) — /backup] fsck.ext4 -a /dev/sde1
/dev/sdc1: clean, 12/1048576 files, 109842/4192957 blocks
/dev/sde1 : clean, 51/67043328 files, 4259482/268173037 blocks
```

Dosya sistemi temiz olarak kapatılmazsa veya depolamayla ilgili sorunlar varsa, bu sorun oluşabilir. Sorunlar arasında donanım veya yazılım hataları, sürücülerveya programlar ile ilgili sorunlar, yazma hataları, vb. yer alır. Kritik verilerin yedeğine sahip olmak her zaman önemlidir. Bu makalede açıklanan araçlar dosya sistemlerini kurtarmaya yardımcı olabilir, ancak veri kaybı hala oluşabilir.

Linux'ta birkaç dosya sistemi denetleyicisi mevcuttur. Azure'daki dağıtımlar için en yaygın olanlardır: [FSCK](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/storage_administration_guide/fsck-fs-specific), [E2FSCK](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/storage_administration_guide/fsck-fs-specific)ve [Xfs_repair](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/storage_administration_guide/xfsrepair).

## <a name="resolution"></a>Çözüm

Bu sorunu gidermek için, [seri konsolu](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-linux) kullanarak VM'yi acil durum moduna önyükleme yapın ve dosya sistemini onarmak için bu aracı kullanın. Seri konsol VM'nizde etkinleştirili değilse veya çalışmıyorsa, bu [makalenin VM çevrimdışı bölümünü onarmaya](#repair-the-vm-offline) bakın.

## <a name="use-the-serial-console"></a>Seri konsolu kullanma

1. Seri konsoluna bağlanın.

   > [!Note]
   > Linux için seri konsol kullanma hakkında daha fazla bilgi için bkz:
   > * [GRUB'a ve tek kullanıcı moduna erişmek için seri konsolu kullanma](https://docs.microsoft.com/azure/virtual-machines/linux/serial-console-grub-single-user-mode)
   > * [SysRq ve NMI aramaları için seri konsol u kullanma](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-nmi-sysrq)

2. Güç simgesi düğmesini seçin ve ardından VM'yi Yeniden Başlat'ı seçin. (Seri konsol etkin değilse veya başarıyla bağlanamazsa, düğmeyi görmezsiniz.)

   ![GÖRÜNTÜ](./media/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck/restart-vm.png)

3. VM'yi acil durum moduna önyükleme.

4. Acil durum modunda oturum açabilmek için kök hesabınızın parolasını girin.

5. Dosya sistemindeki hataları algılamak için -n seçeneğine sahip xfs_repair kullanın. Aşağıdaki örnekte, sistem bölünmesinin /dev/sda1 olduğunu varsayıyoruz. VM'iniz için uygun değerle değiştirin:

   ```
   xfs_repair -n /dev/sda1
   ```

6. Dosya sistemini onarmak için aşağıdaki komutu çalıştırın:

   ```
   xfs_repair /dev/sda1
   ```

7. "HATA: Dosya sisteminde yeniden oynan değerli meta veri değişiklikleri var" hata iletisi alırsanız, geçici bir dizin oluşturun ve dosya sistemini monte edin:

   ```
   mkdir /temp
   mount /dev/sda1 /temp
   ```

8. Disk monte edilemezse, -L seçeneği (kuvvet günlüğü sıfırlama) ile xfs_repair komutu çalıştırın:

   ```
   xfs_repair /dev/sda1 -L
   ```

9. Ardından, dosya sistemini takmayı deneyin. Disk başarıyla monte edilirse, aşağıdaki çıktıyı alırsınız:
 
   ```
   XFS (sda1): Mounting V1 Filesystem
   XFS (sda1): Ending clean mount
   ```

10. VM'yi yeniden başlatın ve sorunun çözülüp çözülmeyeceğini denetleyin.

    ```
    Reboot -f
    ```

## <a name="repair-the-vm-offline"></a>VM'yi çevrimdışı onar

1. VM'nin sistem diskini bir kurtarma VM'ine (çalışan herhangi bir Linux VM) veri diski olarak takın. Bunu yapmak için [CLI komutlarını](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-recovery-disks-linux) kullanabilir veya [VM onarım komutlarını](repair-linux-vm-using-azure-virtual-machine-repair-commands.md)kullanarak kurtarma VM'ini ayarlamayı otomatikleştirebilirsiniz.

2. Eklediğiniz sistem diskinin sürücü etiketini bulun. Bu durumda, iliştirdiğiniz sistem diskinin etiketinin /dev/sdc1 olduğunu varsayıyoruz. VM'iniz için uygun değerle değiştirin.

3. Dosya sistemindeki hataları algılamak için -n seçeneğine sahip xfs_repair kullanın.

   ```
   xfs_repair -n /dev/sdc1
   ```

4. Dosya sistemini onarmak için aşağıdaki komutu çalıştırın:

   ```
   xfs_repair /dev/sdc1
   ```

5. "HATA: Dosya sisteminde yeniden oynan değerli meta veri değişiklikleri var" hata iletisi alırsanız, geçici bir dizin oluşturun ve dosya sistemini monte edin:

   ```
   mkdir /temp

   mount /dev/sdc1 /temp
   ```

   Disk monte edilemezse, -L seçeneği (kuvvet günlüğü sıfırlama) ile xfs_repair komutu çalıştırın:

   ```
   xfs_repair /dev/sdc1 -L
   ```

6. Ardından, dosya sistemini takmayı deneyin. Disk başarıyla monte edilirse, aşağıdaki çıktıyı alırsınız:

   ```
   XFS (sdc1): Mounting V1 Filesystem
   
   XFS (sdc1): Ending clean mount
   ```

7. Özgün sanal sabit diski söküp ayırın ve sonra orijinal sistem diskinden bir VM oluşturun. Bunu yapmak için, kurtarma VM oluşturmak için bunları kullandıysanız [CLI komutları](troubleshoot-recovery-disks-linux.md) veya [VM onarım komutları](repair-linux-vm-using-azure-virtual-machine-repair-commands.md) kullanabilirsiniz.

8. Sorunun çözülüp çözülmeyeceğini denetleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure CLI 2.0 ile işletim sistemi diskini kurtarma VM'ine takarak bir Linux VM sorun giderme](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-troubleshoot-recovery-disks)
* [Linux VM'ye veri diski eklemek için portalı kullanma](https://docs.microsoft.com/azure/virtual-machines/linux/attach-disk-portal)

