---
title: Önyükleme hatası – "Bu önyüklenebilir bir disk değil"
description: Bu makalede, bir Azure sanal makinesinde diskin önyüklenebilir olmadığı sorunları çözümlemek için adımlar sağlanmaktadır
services: virtual-machines-windows
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 5d6db4e3-c2f5-40c7-afea-54edf745f5eb
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 03/25/2020
ms.author: v-mibufo
ms.openlocfilehash: 16f6919577955bda5b04db26deb9fe78a467e364
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86509044"
---
# <a name="boot-error--this-is-not-a-bootable-disk"></a>Önyükleme hatası – bu önyüklenebilir bir disk değil

Bu makalede, diskin bir Azure sanal makinesinde (VM) önyüklenebilir olmadığı sorunları gidermek için gereken adımlar sağlanmaktadır.

## <a name="symptoms"></a>Belirtiler

VM 'nin ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](./boot-diagnostics.md) kullandığınızda, ekran görüntüsünde, ' this önyüklenebilir disk değil ' iletisi ile bir istem görüntülediğini görürsünüz. Lütfen bir önyüklenebilir disket yerleştirip yeniden denemek için herhangi bir tuşa basın... '.

   Şekil 1

   ![Şekil 1, "Bu önyüklenebilir bir disk değil" iletisini gösterir. Lütfen bir önyüklenebilir disket yerleştirip yeniden denemek için herhangi bir tuşa basın... "*](media/troubleshoot-guide-not-bootable-disk/1.jpg)

## <a name="cause"></a>Nedeni

Bu hata iletisi, IŞLETIM sistemi önyükleme işleminin etkin bir sistem bölümü bulamadı anlamına gelir. Bu hata, Önyükleme Yapılandırma Verileri (BCD) deposunda eksik bir başvurunun, Windows bölümünün yerini bulmasına karşı bir başvuru olduğunu da ifade ediyor.

## <a name="solution"></a>Çözüm

### <a name="process-overview"></a>İşleme genel bakış

1. Bir onarım VM 'si oluşturun ve erişin.
2. Bölüm durumunu etkin olarak ayarlayın.
3. Disk bölümünü onarın.
4. **Önerilir**: VM 'yi yeniden oluşturmadan önce, seri konsolu ve bellek dökümü toplamayı etkinleştirin.
5. Özgün VM 'yi yeniden derleyin.

   > [!NOTE]
   > Bu önyükleme hatasıyla karşılaşdığınızda, Konuk işletim sistemi çalışır durumda değildir. Bu sorunu çözmek için çevrimdışı modda sorun gidermeye başlayacaksınız.

### <a name="create-and-access-a-repair-vm"></a>Bir onarım VM 'si oluşturma ve erişme

1. Bir onarım VM 'si hazırlamak için [VM onarım komutlarının](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) 1-3 adımlarını kullanın.
2. Uzak Masaüstü Bağlantısı kullanarak, onarım VM 'ye bağlanın.

### <a name="set-partition-status-to-active"></a>Bölüm durumunu etkin olarak ayarla

1. nesil sanal makineler önce, BCD deposunu tutan işletim sistemi bölümünün *etkin*olarak işaretlendiğinden emin olmalıdır. 2. nesil bir sanal makinenize sahipseniz, sonraki nesil *durum* bayrağı kullanım dışı olduğundan, [disk bölümünü düzelten](#fix-the-disk-partition)önce atlayın.

1. Yükseltilmiş bir komut istemi açın *(cmd.exe)*.
2. DISKPART aracını başlatmak için *DiskPart* girin.
3. Sistemdeki diskleri listelemek ve bağlı işletim sistemi VHD 'sini tanımlamak için *liste diski* girin.
4. Bağlı işletim sistemi VHD 'SI konumlandırıldıktan sonra, diski seçmek için *sel disk #* girin.  Bkz. Şekil 2, disk 1, bağlı işletim sistemi VHD 'si.

   Şekil 2

   ![Şekil 2 ' de, tabloda gösterilen List disk komutu, disk 0 ve disk 1 çıktısını gösteren * DISKPART * penceresi gösterilmektedir.  Ayrıca, sel disk 1 komutunun çıkışını gösterir, disk 1 seçilen disktir](media/troubleshoot-guide-not-bootable-disk/2.jpg)

5. Disk seçildikten sonra Seçili diskin bölümlerini listelemek için *liste bölümü* girin.
6. Önyükleme bölümü tanımlandıktan sonra, bölümü seçmek için *sel bölüm #* girin.  Genellikle önyükleme bölümü 350 MB boyutunda olacaktır.  Bölüm 1 ' in önyükleme bölümü olduğu Şekil 3 ' ü inceleyin.

   Şekil 3

   ![Şekil 3 * liste bölümü * komutunun çıkışıyla * DISKPART * penceresini gösterir. Bölüm 1 ve Bölüm 2 tabloda görüntülenir. Ayrıca Bölüm 1 seçili diskse, * sel Bölüm 1 * komutunun çıkışını gösterir.](media/troubleshoot-guide-not-bootable-disk/3.jpg)

7. Bölümün durumunu denetlemek için ' ayrıntı bölümü ' girin. Bkz. Şekil 4, bölüm *etkin: Hayır*veya şekil 5, burada Bölüm ' aktif: Yes '.

   Şekil 4

   ![Şekil 4 ' te * ayrıntı bölümü * komutunun çıkışıyla *, Bölüm 1 * etkin: Hayır * olarak ayarlandığında * DISKPART * penceresi gösterilmektedir.](media/troubleshoot-guide-not-bootable-disk/4.jpg)

   Şekil 5

   ![Şekil 5, Bölüm 1 * etkin: Evet * olarak ayarlandığında * ayrıntı bölümü * komutunun çıkışıyla * DISKPART * penceresini gösterir.](media/troubleshoot-guide-not-bootable-disk/5.jpg)

8. Bölüm **etkin değilse** *, etkin bayrağını değiştirmek* için *etkin* ' i girin.
9. *Ayrıntı bölümü*yazarak durum değişikliğinin düzgün şekilde yapıldığından emin olun.

   Şekil 6

   ![Şekil 6, Bölüm 1 * etkin: Evet * olarak ayarlandığında, * ayrıntı bölümü * komutunun çıkışıyla DiskPart penceresini gösterir.](media/troubleshoot-guide-not-bootable-disk/6.jpg)

10. DISKPART aracını kapatmak ve yapılandırma değişikliklerinizi kaydetmek için *Çıkış* girin.

### <a name="fix-the-disk-partition"></a>Disk bölümünü çözme

1. Yükseltilmiş bir komut istemi açın (cmd.exe).
2. Disk (ler) de *Chkdsk* çalıştırmak ve hataları onarmak için aşağıdaki komutu kullanın:

   `chkdsk <DRIVE LETTER>: /f`

   '/F ' komut seçeneğinin eklenmesi diskteki tüm hataları düzeltir. <DRIVE LETTER>Bağlı işletim SISTEMI VHD 'sinin harfiyle değiştirdiğinizden emin olun.

### <a name="recommended-before-you-rebuild-the-vm-enable-serial-console-and-memory-dump-collection"></a>Önerilir: VM 'yi yeniden oluşturmadan önce, seri konsolu ve bellek dökümü toplamayı etkinleştirin

Bellek dökümü toplamayı ve seri konsolunu etkinleştirmek için aşağıdaki betiği çalıştırın:

1. Yükseltilmiş bir komut istemi oturumu açın (yönetici olarak çalıştır).
2. Aşağıdaki komutları çalıştırın:

   Seri konsolunu etkinleştir

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON`

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

3. İşletim sistemi diskindeki boş alanın, VM 'deki bellek boyutu (RAM) kadar olduğundan emin olun.

   İşletim sistemi diskinde yeterli alan yoksa, bellek dökümü dosyasının oluşturulacağı konumu değiştirmeniz ve yeterli boş alana sahip olan VM 'ye bağlı tüm veri diskine başvurmanız gerekir. Konumu değiştirmek için, "% SystemRoot%" değerini aşağıdaki komutlarda bulunan veri diskinin sürücü harfiyle (örneğin, "F:") değiştirin.

#### <a name="suggested-configuration-to-enable-os-dump"></a>İşletim sistemi dökümünü etkinleştirmek için önerilen yapılandırma

**Bozuk işletim sistemi diski yükle**:

`REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM`

**ControlSet001 üzerinde etkinleştir:**

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

**ControlSet002 üzerinde etkinleştir:**

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

**Bozuk işletim sistemi diskini kaldır:**

`REG UNLOAD HKLM\BROKENSYSTEM`

### <a name="rebuild-the-original-vm"></a>Özgün VM 'yi yeniden oluşturma

VM 'yi yeniden birleştirmek için [VM onarım komutlarının 5. adımını](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) kullanın.
