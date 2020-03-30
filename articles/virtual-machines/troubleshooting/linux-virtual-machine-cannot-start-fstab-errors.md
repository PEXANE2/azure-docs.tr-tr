---
title: Fstab hataları nedeniyle Linux VM başlangıç sorunları | Microsoft Dokümanlar
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
ms.openlocfilehash: 7e16eabc4f9572591eabd37b93258fcd783cce7e
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80351145"
---
# <a name="troubleshoot-linux-vm-starting-issues-due-to-fstab-errors"></a>Fstab hataları nedeniyle Linux VM başlangıç sorunları gidermek

Güvenli Kabuk (SSH) bağlantısını kullanarak Bir Azure Linux Sanal Makinesine (VM) bağlanamazsınız. [Azure portalında](https://portal.azure.com/) [Önyükleme Tanılama](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) özelliğini çalıştırdığınızda, aşağıdaki örneklere benzeyen günlük girişleri görürsünüz:

## <a name="examples"></a>Örnekler

Aşağıda olası hatalara örnekler verilmiştir.

### <a name="example-1-a-disk-is-mounted-by-the-scsi-id-instead-of-the-universally-unique-identifier-uuid"></a>Örnek 1: Bir disk, evrensel olarak benzersiz tanımlayıcı (UUID) yerine SCSI Kimliği ile monte edilir

```
[K[[1;31m TIME [0m] Timed out waiting for device dev-incorrect.device.
[[1;33mDEPEND[0m] Dependency failed for /data.
[[1;33mDEPEND[0m] Dependency failed for Local File Systems.
…
Welcome to emergency mode! After logging in, type "journalctl -xb" to viewsystem logs, "systemctl reboot" to reboot, "systemctl default" to try again to boot into default mode.
Give root password for maintenance
(or type Control-D to continue)
```

### <a name="example-2-an-unattached-device-is-missing-on-centos"></a>Örnek 2: CentOS'ta ilişmemiş bir aygıt eksik

```
Checking file systems…
fsck from util-linux 2.19.1
Checking all file systems.
/dev/sdc1: nonexistent device ("nofail" fstab option may be used to skip this device)
/dev/sdd1: nonexistent device ("nofail" fstab option may be used to skip this device)
/dev/sde1: nonexistent device ("nofail" fstab option may be used to skip this device)

[/sbin/fsck.ext3 (1) — /CODE] sck.ext3 -a /dev/sdc1
fsck.ext3: No such file or directory while trying to open /dev/sdc1

/dev/sdc1:
The superblock could not be read or does not describe a correct ext2
filesystem. If the device is valid and it really contains an ext2
filesystem (and not swap or ufs or something else), then the superblock
is corrupt, and you might try running e2fsck with an alternate superblock:

e2fsck -b 8193 <device>

[/sbin/fsck.xfs (1) — /GLUSTERDISK] fsck.xfs -a /dev/sdd1
/sbin/fsck.xfs: /dev/sdd1 does not exist
[/sbin/fsck.ext3 (1) — /DATATEMP] fsck.ext3 -a /dev/sde1 fsck.ext3: No such file or directory while trying to open /dev/sde1
```

### <a name="example-3-a-vm-cannot-start-because-of-an-fstab-misconfiguration-or-because-the-disk-is-no-longer-attached"></a>Örnek 3: Bir VM fstab yanlış yapılandırması nedeniyle veya disk artık bağlı olmadığından başlatılamaz

```
The disk drive for /var/lib/mysql is not ready yet or not present.
Continue to wait, or Press S to skip mounting or M for manual recovery
```

### <a name="example-4-a-serial-log-entry-shows-an-incorrect-uuid"></a>Örnek 4: Seri günlük girişi yanlış bir UUID gösterir

```
Checking filesystems
Checking all file systems.
[/sbin/fsck.ext4 (1) — /] fsck.ext4 -a /dev/sda1
/dev/sda1: clean, 70442/1905008 files, 800094/7608064 blocks
[/sbin/fsck.ext4 (1) — /datadrive] fsck.ext4 -a UUID="<UUID>"
fsck.ext4: Unable to resolve UUID="<UUID>"
[FAILED

*** An error occurred during the file system check.
*** Dropping you to a shell; the system will reboot
*** when you leave the shell.
*** Warning — SELinux is active
*** Disabling security enforcement for system recovery.
*** Run 'setenforce 1' to reenable.
type=1404 audit(1428047455.949:4): enforcing=0 old_enforcing=1 auid=<AUID> ses=4294967295
Give root password for maintenance
(or type Control-D to continue)
```

Bu sorun, dosya sistemleri tablosu (fstab) sözdizimi yanlışsa veya "/etc/fstab" dosyasındaki bir girişe eşlenen gerekli bir veri diski VM'ye eklenmemişse oluşabilir.

## <a name="resolution"></a>Çözüm

Bu sorunu gidermek için, Azure Sanal Makineler için seri konsolu kullanarak VM'yi acil durum modunda başlatın. Ardından dosya sistemini onarmak için aracı kullanın. VM'nizde seri konsol etkinleştirilemiyorsa, [VM çevrimdışı](#repair-the-vm-offline) onarım bölümüne gidin.

## <a name="use-the-serial-console"></a>Seri konsolu kullanma

### <a name="using-single-user-mode"></a>Tek Kullanıcı Modunu Kullanma

1. Seri [konsola bağlanın.](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-linux)
2. Tek kullanıcı modu tek [kullanıcı modu](https://docs.microsoft.com/azure/virtual-machines/linux/serial-console-grub-single-user-mode) almak için seri konsolu kullanın
3. Bir kez vm tek kullanıcı moduna önyükleme vardır. Fstab dosyasını açmak için en sevdiğiniz metin düzenleyicisini kullanın. 

   ```
   # nano /etc/fstab
   ```

4. Listelenen dosya sistemlerini gözden geçirin. fstab dosyasındaki her satır, VM başlatıldığında monte edilen bir dosya sistemini gösterir. fstab dosyasının sözdizimi hakkında daha fazla bilgi için, adam fstab komutunu çalıştırın. Bir başlangıç hatasını gidermek için, hem yapıda hem de içerikte doğru olduğundan emin olmak için her satırı gözden geçirin.

   > [!Note]
   > * Her satırdaki alanlar sekmelerle veya boşluklarla ayrılır. Boş satırlar yoksayılır. İlk karakter olarak sayı işareti (#) olan satırlar yorumdur. Yorumlanan satırlar fstab dosyasında kalabilir, ancak bunlar işlenmez. Satırları kaldırmak yerine emin olmadığınız fstab satırlarına yorum yapmanızı öneririz.
   > * VM kurtarmak ve başlatmak için, dosya sistemi bölümleri gerekli tek bölümler olmalıdır. VM, yorumlanan ek bölümlerle ilgili uygulama hatalarıyla karşılaşabilir. Ancak, VM ek bölümler olmadan başlamalıdır. Daha sonra yorumlanan satırların yorumlarını uncomment edebilirsiniz.
   > * Dosya sistemi bölümü UUID'sini kullanarak Azure VM'lerine veri diskleri takmanızı öneririz. Örneğin, aşağıdaki komutu çalıştırın:``/dev/sdc1: LABEL="cloudimg-rootfs" UUID="<UUID>" TYPE="ext4" PARTUUID="<PartUUID>"``
   > * Dosya sisteminin UUID'sini belirlemek için blkid komutunu çalıştırın. Sözdizimi hakkında daha fazla bilgi için, adam blkid komutunu çalıştırın.
   > * Nofail seçeneği, dosya sistemi bozuk olsa veya dosya sistemi başlangıçta yok olsa bile VM'nin başlatılmasını sağlamaya yardımcı olur. VM'nin başlaması için gerekli olmayan bölümlerde hatalar oluştuktan sonra başlatmanın devam etmesini sağlamak için fstab dosyasındaki nofail seçeneğini kullanmanızı öneririz.

5. VM'nin doğru şekilde başlatılmasını sağlamak için fstab dosyasındaki yanlış veya gereksiz satırları değiştirin veya yorum yapın.

6. Değişiklikleri fstab dosyasına kaydedin.

7. Aşağıdaki komutu kullanarak vm yeniden başlatın.
   
   ```
   # reboot -f
   ```
> [!Note]
   > VM'yi yeniden başlatacak "ctrl+x" komutunu da kullanabilirsiniz.


8. Girişler yorum veya düzeltme başarılı olduysa, sistem portalda bir bash istemi ne ulaşmalıdır. VM'ye bağlanıp bağlanamayacağınızı kontrol edin.

### <a name="using-root-password"></a>Kök Parola Kullanma

1. Seri [konsola bağlanın.](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-linux)
2. Yerel bir kullanıcı ve parola kullanarak sisteme oturum açın.

   > [!Note]
   > Seri konsoldaki sistemde oturum açabilmek için SSH tuşu kullanamazsınız.

3. Diskin monte olmadığını gösteren hatayı arayın. Aşağıdaki örnekte, sistem artık mevcut olmayan bir disk eklemeye çalışıyordu:

   ```
   [DEPEND] Dependency failed for /datadisk1.
   [DEPEND] Dependency failed for Local File Systems.
   [DEPEND] Dependency failed for Relabel all filesystems, if necessary.
   [DEPEND] Dependency failed for Migrate local... structure to the new structure.
   Welcome to emergency mode! After logging in, type "journalctl -xb" to view
   system logs, "systemctl reboot" to reboot, "systemctl default" or ^D to try again to boot into default mode.
   Give root password for maintenance
   (or type Control-D to continue):
   ```

4. Kök parolayı (Red Hat tabanlı VM) kullanarak VM'ye bağlanın.

5. Fstab dosyasını açmak için en sevdiğiniz metin düzenleyicisini kullanın. Disk monte ediletıldıktan sonra Nano için aşağıdaki komutu çalıştırın:

   ```
   $ nano /mnt/troubleshootingdisk/etc/fstab
   ```

6. Listelenen dosya sistemlerini gözden geçirin. fstab dosyasındaki her satır, VM başlatıldığında monte edilen bir dosya sistemini gösterir. fstab dosyasının sözdizimi hakkında daha fazla bilgi için, adam fstab komutunu çalıştırın. Bir başlangıç hatasını gidermek için, hem yapıda hem de içerikte doğru olduğundan emin olmak için her satırı gözden geçirin.

   > [!Note]
   > * Her satırdaki alanlar sekmelerle veya boşluklarla ayrılır. Boş satırlar yoksayılır. İlk karakter olarak sayı işareti (#) olan satırlar yorumdur. Yorumlanan satırlar fstab dosyasında kalabilir, ancak bunlar işlenmez. Satırları kaldırmak yerine emin olmadığınız fstab satırlarına yorum yapmanızı öneririz.
   > * VM kurtarmak ve başlatmak için, dosya sistemi bölümleri gerekli tek bölümler olmalıdır. VM, yorumlanan ek bölümlerle ilgili uygulama hatalarıyla karşılaşabilir. Ancak, VM ek bölümler olmadan başlamalıdır. Daha sonra yorumlanan satırların yorumlarını uncomment edebilirsiniz.
   > * Dosya sistemi bölümü UUID'sini kullanarak Azure VM'lerine veri diskleri takmanızı öneririz. Örneğin, aşağıdaki komutu çalıştırın:``/dev/sdc1: LABEL="cloudimg-rootfs" UUID="<UUID>" TYPE="ext4" PARTUUID="<PartUUID>"``
   > * Dosya sisteminin UUID'sini belirlemek için blkid komutunu çalıştırın. Sözdizimi hakkında daha fazla bilgi için, adam blkid komutunu çalıştırın.
   > * Nofail seçeneği, dosya sistemi bozuk olsa veya dosya sistemi başlangıçta yok olsa bile VM'nin başlatılmasını sağlamaya yardımcı olur. VM'nin başlaması için gerekli olmayan bölümlerde hatalar oluştuktan sonra başlatmanın devam etmesini sağlamak için fstab dosyasındaki nofail seçeneğini kullanmanızı öneririz.

7. VM'nin doğru şekilde başlatılmasını sağlamak için fstab dosyasındaki yanlış veya gereksiz satırları değiştirin veya yorum yapın.

8. Değişiklikleri fstab dosyasına kaydedin.

9. Sanal makineyi yeniden başlatın.

10. Girişler yorum veya düzeltme başarılı olduysa, sistem portalda bir bash istemi ne ulaşmalıdır. VM'ye bağlanıp bağlanamayacağınızı kontrol edin.

11. Montaj -bir komut çalıştırarak herhangi bir fstab değişikliği test ederken montaj noktaları kontrol edin. Herhangi bir hata yoksa, montaj noktaları iyi olmalıdır.

## <a name="repair-the-vm-offline"></a>VM'yi çevrimdışı onar

1. VM'nin sistem diskini bir kurtarma VM'ine (çalışan herhangi bir Linux VM) veri diski olarak takın. Bunu yapmak için [CLI komutlarını](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-recovery-disks-linux) kullanabilir veya [VM onarım komutlarını](repair-linux-vm-using-azure-virtual-machine-repair-commands.md)kullanarak kurtarma VM'ini ayarlamayı otomatikleştirebilirsiniz.

2. Sistem diskini kurtarma VM'ine veri diski olarak monte ettikten sonra, değişiklik yapmadan önce fstab dosyasını yedekleyip fstab dosyasını düzeltmek için sonraki adımları izleyin.

3.    Diskin monte edilmemiş olduğunu gösteren hatayı arayın. Aşağıdaki örnekte, sistem artık mevcut olmayan bir disk eklemeye çalışıyordu:

    ```
    [DEPEND] Dependency failed for /datadisk1.
    [DEPEND] Dependency failed for Local File Systems.
    [DEPEND] Dependency failed for Relabel all filesystems, if necessary.
    [DEPEND] Dependency failed for Migrate local... structure to the new structure.
    Welcome to emergency mode! After logging in, type "journalctl -xb" to view system logs, "systemctl reboot" to reboot, "systemctl default" or ^D to try again to boot into default mode.
    Give root password for maintenance (or type Control-D to continue):
    ```

4. Kök parolayı (Red Hat tabanlı VM) kullanarak VM'ye bağlanın.

5. Fstab dosyasını açmak için en sevdiğiniz metin düzenleyicisini kullanın. Disk monte ediletıldıktan sonra Nano için aşağıdaki komutu çalıştırın. Kurtarma VM'indeki fstab dosyası nda değil, monte edilmiş diskte bulunan fstab dosyası üzerinde çalıştığınızdan emin olun.

   ```
   $ nano /mnt/troubleshootingdisk/etc/fstab
   ```

6. Listelenen dosya sistemlerini gözden geçirin. fstab dosyasındaki her satır, VM başlatıldığında monte edilen bir dosya sistemini gösterir. fstab dosyasının sözdizimi hakkında daha fazla bilgi için, adam fstab komutunu çalıştırın. Bir başlangıç hatasını gidermek için, hem yapıda hem de içerikte doğru olduğundan emin olmak için her satırı gözden geçirin.

   > [!Note]
   > * Her satırdaki alanlar sekmelerle veya boşluklarla ayrılır. Boş satırlar yoksayılır. İlk karakter olarak sayı işareti (#) olan satırlar yorumdur. Yorumlanan satırlar fstab dosyasında kalabilir, ancak bunlar işlenmez. Satırları kaldırmak yerine emin olmadığınız fstab satırlarına yorum yapmanızı öneririz.
   > * VM kurtarmak ve başlatmak için, dosya sistemi bölümleri gerekli tek bölümler olmalıdır. VM, yorumlanan ek bölümlerle ilgili uygulama hatalarıyla karşılaşabilir. Ancak, VM ek bölümler olmadan başlamalıdır. Daha sonra yorumlanan satırların yorumlarını uncomment edebilirsiniz.
   > * Dosya sistemi bölümü UUID'sini kullanarak Azure VM'lerine veri diskleri takmanızı öneririz. Örneğin, aşağıdaki komutu çalıştırın:``/dev/sdc1: LABEL="cloudimg-rootfs" UUID="<UUID>" TYPE="ext4" PARTUUID="<PartUUID>"``
   > * Dosya sisteminin UUID'sini belirlemek için blkid komutunu çalıştırın. Sözdizimi hakkında daha fazla bilgi için, adam blkid komutunu çalıştırın. Kurtarmak istediğiniz diskin artık yeni bir VM'ye monte edilmiş olduğuna dikkat edin. UUIEds tutarlı olması gerekir, ancak aygıt bölüm disme (örneğin, "/ dev / sda1") bu VM farklıdır. Sistem dışı bir VHD üzerinde bulunan orijinal başarısız VM dosya sistemi bölümleri [CLI komutları kullanarak](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-recovery-disks-linux)kurtarma VM için kullanılamaz.
   > * Nofail seçeneği, dosya sistemi bozuk olsa veya dosya sistemi başlangıçta yok olsa bile VM'nin başlatılmasını sağlamaya yardımcı olur. VM'nin başlaması için gerekli olmayan bölümlerde hatalar oluştuktan sonra başlatmanın devam etmesini sağlamak için fstab dosyasındaki nofail seçeneğini kullanmanızı öneririz.

7. VM'nin doğru şekilde başlatılmasını sağlamak için fstab dosyasındaki yanlış veya gereksiz satırları değiştirin veya yorum yapın.

8. Değişiklikleri fstab dosyasına kaydedin.

9. Sanal makineyi yeniden başlatın veya orijinal VM'yi yeniden yeniden başlatın.

10. Girişler yorum veya düzeltme başarılı olduysa, sistem portalda bir bash istemi ne ulaşmalıdır. VM'ye bağlanıp bağlanamayacağınızı kontrol edin.

11. Montaj -bir komut çalıştırarak herhangi bir fstab değişikliği test ederken montaj noktaları kontrol edin. Herhangi bir hata yoksa, montaj noktaları iyi olmalıdır.

12. Özgün sanal sabit diski söküp ayırın ve sonra orijinal sistem diskinden bir VM oluşturun. Bunu yapmak için, kurtarma VM oluşturmak için bunları kullandıysanız [CLI komutları](troubleshoot-recovery-disks-linux.md) veya [VM onarım komutları](repair-linux-vm-using-azure-virtual-machine-repair-commands.md) kullanabilirsiniz.

13. VM'yi yeniden oluşturduktan ve SSH üzerinden bağlanabildiğinizden sonra aşağıdaki işlemleri yapabilirsiniz:
    * Kurtarma sırasında değiştirilen veya yorumlanan fstab satırlarından herhangi birini gözden geçirin.
    * UUID ve nofail seçeneğini uygun şekilde kullandığınızdan emin olun.
    * VM'yi yeniden başlatmadan önce fstab değişikliklerini test edin. Bunu yapmak için aşağıdaki komutu kullanın:``$ sudo mount -a``
    * Gelecekteki kurtarma senaryolarında kullanılmak üzere düzeltilen fstab dosyasının ek bir kopyasını oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure CLI 2.0 ile işletim sistemi diskini kurtarma VM'ine takarak bir Linux VM sorun giderme](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-troubleshoot-recovery-disks)
* [Azure portalını kullanarak işletim sistemi diskini kurtarma VM'ine takarak bir Linux VM sorun giderme](https://docs.microsoft.com/azure/virtual-machines/linux/troubleshoot-recovery-disks-portal)

