---
title: Önyükleme hatası – "Bu önyüklenebilir bir disk değil"
description: Bu makalede, bir Azure Sanal Makine'de diskin önyüklenmediği sorunları gidermek için adımlar
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
ms.openlocfilehash: 9f0c6350b89dcfecefcadcc166f7af35abc4b128
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80300985"
---
# <a name="boot-error--this-is-not-a-bootable-disk"></a>Önyükleme Hatası – Bu Önyükleme Diski değildir

Bu makalede, diskin Azure Sanal Makine'de (VM) önyüklenmediği sorunları gidermek için adımlar bulunur.

## <a name="symptoms"></a>Belirtiler

VM ekran görüntüsünü görüntülemek için [Önyükleme tanılama](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) kullandığınızda, ekran görüntüsü 'Bu başlatılabilir bir disk değildir' iletisi ile bir istemi görüntüler görürsünüz. Lütfen önyüklenebilir bir disket takın ve yeniden denemek için herhangi bir tuşa basın...'.

   Şekil 1

   ![Şekil 1 iletiyi gösterir *"Bu önyüklenebilir bir disk değildir. Lütfen önyüklenebilir bir disket takın ve yeniden denemek için herhangi bir tuşa basın..."*](media/troubleshoot-guide-not-bootable-disk/1.jpg)

## <a name="cause"></a>Nedeni

Bu hata iletisi, işletim sistemi önyükleme işleminin etkin bir sistem bölümü bulamadığı anlamına gelir. Bu hata, Önyükleme Yapılandırma Verileri (BCD) deposunda eksik bir başvuru olduğu ve Windows bölümü bulunmasını engellediği anlamına da gelebilir.

## <a name="solution"></a>Çözüm

### <a name="process-overview"></a>İşleme Genel Bakış

1. Onarım VM'si oluşturma ve erişme.
2. Bölüm Durumunu Etkin olarak ayarlayın.
3. Disk Bölümlerini düzeltin.
4. **Önerilen**: VM'yi yeniden oluşturmadan önce seri konsol ve bellek dökümü koleksiyonunu etkinleştirin.
5. Orijinal VM'yi yeniden oluştur.

   > [!NOTE]
   > Bu önyükleme hatasıyla karşılaştığında, Konuk Işletim Sistemi çalışmıyor. Bu sorunu gidermek için çevrimdışı modda sorun giderme olacak.

### <a name="create-and-access-a-repair-vm"></a>Onarım VM Oluşturma ve Eriş

1. Onarım VM'i hazırlamak için [VM Onarım Komutları'nın](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands) 1-3 adımlarını kullanın.
2. Uzak Masaüstü Bağlantısını Kullanarak Onarım VM'sine bağlanın.

### <a name="set-partition-status-to-active"></a>Bölüm Durumunu Etkin olarak ayarlama

Nesil 1 VM'ler öncelikle BCD deposunu tutan işletim sistemi bölümü *etkin*olarak işaretlendiğini doğrulamalıdır. Bir Generation 2 VM'iniz varsa, *Durum* bayrağı sonraki nesilde amortismana götürüldüği için [Disk Bölüm'ü düzelt'e](#fix-the-disk-partition)doğru atlayın.

1. Yükseltilmiş komut *istemini açın (cmd.exe)*.
2. DISKPART aracını başlatmak için *diskbölümü* girin.
3. Sistemdeki diskleri listelemek ve bağlı OS VHD'yi tanımlamak için *liste diskini* girin.
4. Bağlı OS VHD bulunduğunda, diski seçmek için *sel disk #* girin.  Bkz. Disk 1'in bağlı OS VHD olduğu Şekil 2'ye bakın.

   Şekil 2

   ![Şekil 2 tabloda görüntülenen liste disk komutu, Disk 0 ve Disk 1 çıktısını gösteren *DISKPART* penceresini gösterir.  Ayrıca sel disk 1 komutunun çıktısını gösterir, Disk 1 seçili disktir](media/troubleshoot-guide-not-bootable-disk/2.jpg)

5. Disk seçildikten sonra, seçili diskin bölümlerini listelemek için *liste bölümüne* girin.
6. Önyükleme bölümü tanımlandıktan sonra, bölümü seçmek için *sel partition #* girin.  Genellikle önyükleme bölümü boyutu 350 MB civarında olacaktır.  Bkz. Bölüm 1'in önyükleme bölümü olduğu Şekil 3'e bakın.

   Şekil 3

   ![Şekil 3 *list partition* komutunun çıktısı ile *DISKPART* penceresini gösterir. Bölüm 1 ve Bölüm 2 tabloda görüntülenir. Ayrıca* sel bölüm 1* komutunun çıktısını, Bölüm 1 seçili disk olduğunda gösterir.](media/troubleshoot-guide-not-bootable-disk/3.jpg)

7. Bölümün durumunu denetlemek için 'ayrıntı bölümü' girin. Bkz. Şekil 4, bölümün Etkin olduğu *yer: Hayır*veya Şekil 5, bölümün 'Etkin: Evet' olduğu yer.

   Şekil 4

   ![Şekil 4*DETAIL partition* komutunun çıktısıyla *DISKPART* penceresini gösterir, Bölüm 1 *Active olarak ayarlandığında: Hayır*](media/troubleshoot-guide-not-bootable-disk/4.jpg)

   Şekil 5

   ![Şekil 5, *detay bölümü* komutunun çıktısıyla *DISKPART* penceresini gösterir, Bölüm 1 *Active: Yes* olarak ayarlandığında.](media/troubleshoot-guide-not-bootable-disk/5.jpg)

8. Bölüm Etkin **değilse,** *Etkin* bayrağı değiştirmek için *etkin* girin.
9. Durum değişikliğinin *ayrıntı bölümü*yazarak düzgün şekilde yapılabileceğini denetleyin.

   Şekil 6

   ![Şekil 6, Bölüm 1 *Active olarak ayarlandığında *detay bölümü* komutunun çıktısıyla diskbölümü penceresini gösterir: Evet*](media/troubleshoot-guide-not-bootable-disk/6.jpg)

10. DISKPART aracını kapatmak ve yapılandırma değişikliklerinizi kaydetmek için *çıkışgirin.*

### <a name="fix-the-disk-partition"></a>Disk Bölmesini Düzeltme

1. Yükseltilmiş bir komut istemi (cmd.exe) açın.
2. *CHKDSK'yi* diskte çalıştırmak ve hataları düzeltmek için aşağıdaki komutu kullanın:

   `chkdsk <DRIVE LETTER>: /f`

   '/f' komutu seçeneğinin eklenmesi diskteki hataları düzeltecektir. Ekli OS <DRIVE LETTER> VHD harfi ile değiştirmek için emin olun.

### <a name="recommended-before-you-rebuild-the-vm-enable-serial-console-and-memory-dump-collection"></a>Önerilen: VM'yi yeniden oluşturmadan önce seri konsol ve bellek dökümü koleksiyonunu etkinleştirin

Bellek dökümü toplama ve Seri Konsol'u etkinleştirmek için aşağıdaki komut dosyasını çalıştırın:

1. Yükseltilmiş bir komut istemi oturumu açın (Yönetici olarak çalıştırın).
2. Aşağıdaki komutları çalıştırın:

   Seri Konsolu Etkinleştir

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON`

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

3. Os diskindeki boş alanın VM'deki bellek boyutu (RAM) kadar olduğunu doğrulayın.

   Os diskinde yeterli alan yoksa, bellek dökümü dosyasının oluşturulacağı konumu değiştirmeli ve bunu Yeterli boş alana sahip VM'ye bağlı herhangi bir veri diskine yönlendirmelisiniz. Konumu değiştirmek için, aşağıdaki komutlarda veri diskinin sürücü harfiyle "%SystemRoot%" (örneğin, "F:") değiştirin.

#### <a name="suggested-configuration-to-enable-os-dump"></a>İşletim Sistemi Döküm'ü etkinleştirmek için önerilen yapılandırma

**Yük Kırık İşletim Sistemi Diski**:

`REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM`

**ControlSet001'de etkinleştirin:**

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

**ControlSet002'de etkinleştirin:**

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

**Bozuk Işletim Sistemi Diskini Boşaltın:**

`REG UNLOAD HKLM\BROKENSYSTEM`

### <a name="rebuild-the-original-vm"></a>Orijinal VM'yi yeniden oluşturma

VM'yi yeniden birleştirmek için [VM Onarım Komutlarının adım](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) 5'ini kullanın.
