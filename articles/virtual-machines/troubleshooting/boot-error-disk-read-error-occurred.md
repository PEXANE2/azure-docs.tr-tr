---
title: Önyükleme hatası sorunlarını giderme-disk okuma hatası oluştu
description: Bu makalede, diskin bir Azure VM 'de okuyabildiği sorunları gidermeye yönelik adımlar sağlanmaktadır.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 817c9c5c-6a81-4b42-a6ad-0a0a11858b84
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 06/01/2020
ms.author: v-miegge
ms.openlocfilehash: dea09b1ac29db99e1c52a31a605007fa4129e8ea
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84300529"
---
# <a name="troubleshoot-boot-error---disk-read-error-occurred"></a>Önyükleme hatası sorunlarını giderme-disk okuma hatası oluştu

Bu makalede, diskin bir Azure sanal makinesinde (VM) okungeçirebileceği sorunları gidermeye yönelik adımlar sağlanmaktadır.

## <a name="symptoms"></a>Belirtiler

VM 'nin ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) kullandığınızda, ekran görüntüsünde "disk okuma hatası oluştu" iletisiyle bir istem görüntülediğini görürsünüz. Yeniden başlatmak için Ctrl+Alt+Del tuşlarına basın”.

   ![Hata iletisi: bir disk okuma hatası oluştu. Yeniden başlatmak için Ctrl + Alt + Del tuşlarına basın.](./media/disk-read-error-occurred/1.png)

## <a name="cause"></a>Nedeni

Bu hata iletisi, disk yapısının bozuk ve okunamaz olduğunu gösterir. 1. nesil bir VM kullanıyorsanız, önyükleme yapılandırma verilerini içeren disk bölümünün **etkin**olarak ayarlanmamış olması da mümkündür.

## <a name="solution"></a>Çözüm

### <a name="process-overview"></a>İşleme genel bakış

1. Bir onarım VM 'si oluşturun ve erişin.
1. Bir çözüm seçin:
   - [Bölüm durumunu etkin olarak ayarla](#set-partition-status-to-active)
   - [Disk bölümünü çözme](#fix-the-disk-partition)
1. Seri konsolu ve bellek dökümü toplamayı etkinleştirin.
1. VM 'yi yeniden derleyin.

> [!NOTE]
> Bu önyükleme hatasıyla karşılaşdığınızda, Konuk işletim sistemi (OS) çalışır durumda değildir. Bu sorunu çözmek için çevrimdışı modda sorun gidermeye başlayacaksınız.

### <a name="create-and-access-a-repair-vm"></a>Bir onarım VM 'si oluşturma ve erişme

1. Bir onarım VM 'si hazırlamak için [VM onarım komutlarının](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands) 1-3 adımlarını kullanın.
1. Uzak Masaüstü Bağlantısı kullanarak, onarım sanal makinesine bağlanın.

### <a name="set-partition-status-to-active"></a>Bölüm durumunu etkin olarak ayarla

1. nesil sanal makineler önce, BCD deposunu tutan işletim sistemi bölümünün **etkin**olarak işaretlendiğinden emin olmalıdır. 2. nesil bir sanal makinenize sahipseniz, sonraki nesil durum bayrağı kullanım dışı olduğundan, [disk bölümünü düzelten](#fix-the-disk-partition)önce atlayın.

1. Yükseltilmiş bir komut istemi açın (cmd.exe).
1. **DiskPart** aracını başlatmak için **DiskPart** girin.
1. Sistemdeki diskleri listelemek ve bağlı işletim sistemi sanal sabit diskini (VHD) belirlemek için **liste diski** girin.
1. Bağlı işletim sistemi VHD 'SI konumlandırıldıktan sonra, diski seçmek için **sel disk #** girin. Disk 1 ' in bağlı işletim sistemi VHD 'sinin bir örneği için aşağıdaki resme bakın.

   ![Disk 0 ve disk 1 ' in tabloda görüntülendiği * * List disk * * komutunun çıkışıyla ilgili DiskPart penceresi. Pencere Ayrıca, * * sel disk 1 * * komutunun çıkışını gösterir; burada disk 1 seçilen disktir](./media/disk-read-error-occurred/2.png)

1. Disk seçildikten sonra Seçili diskin bölümlerini listelemek için **liste bölümü** girin.
1. Önyükleme bölümü tanımlandıktan sonra, bölümü seçmek için **sel bölüm #** girin. Önyükleme bölümü genellikle yaklaşık 350 MB boyutundaydı.  Aşağıdaki resme bakın; örneğin, Bölüm 1 ' in önyükleme bölümüdür.

   ![Bölüm 1 ve Bölüm 2 ' nin tabloda görüntülendiği * * liste bölümü * * komutunun çıkışıyla ilgili DiskPart penceresi. Pencere Ayrıca, Bölüm 1 ' in seçili disk olduğu * * sel Bölüm 1 * * komutunun çıkışını gösterir.](./media/disk-read-error-occurred/3.png)

1. Bölümün durumunu denetlemek için **ayrıntı bölümünü** girin. Geçerli olarak ayarlanan bölüm örnekleri için aşağıdaki ekran görüntülerine bakın **: Hayır** veya **etkin: Evet**.

   **Etkin: Hayır**

   ![Bölüm 1 ' in * * etkin: Hayır * * olarak ayarlandığı * * ayrıntı bölümü * * komutunun çıkışıyla DiskPart penceresi.](./media/disk-read-error-occurred/4.png)

   **Etkin: Evet**

   ![Bölüm 1 ' in * * etkin: Evet * * olarak ayarlandığı * * ayrıntı bölümü * * komutunun çıkışıyla DiskPart penceresi.](./media/disk-read-error-occurred/5.png)

1. Bölüm **etkin**olarak ayarlanmamışsa etkin bayrağını değiştirmek için **etkin** ' i girin.
1. Durum değişikliğinin düzgün şekilde tamamlandığını denetlemek için **ayrıntı bölümünü** girin ve çıktının etkin içerdiğini doğrulayın **: Evet**. 
1. DISKPART aracını kapatmak ve yapılandırma değişikliklerinizi kaydetmek için **Çıkış** girin.

### <a name="fix-the-disk-partition"></a>Disk bölümünü çözme

1. Yükseltilmiş bir komut istemi açın (cmd.exe).
1. Disklerde **Chkdsk** çalıştırmak ve hata düzeltmeleri yapmak için aşağıdaki komutu kullanın:

   `chkdsk <DRIVE LETTER>: /f`

   **/F** komut seçeneğinin eklenmesi diskteki tüm hataları düzeltir. **< sürücü harfi >** eklenmiş IŞLETIM sistemi VHD 'si ile değiştirdiğinizden emin olun.

### <a name="enable-the-serial-console-and-memory-dump-collection"></a>Seri konsol ve bellek dökümü toplamayı etkinleştirme

**Önerilir**: VM 'yi yeniden oluşturmadan önce, aşağıdaki betiği çalıştırarak seri konsol ve bellek dökümü toplamasını etkinleştirin:

1. Yükseltilmiş bir komut istemi oturumunu yönetici olarak açın.
1. Aşağıdaki komutları çalıştırın:

   **Seri konsolunu etkinleştirin**:
   
   ```
   bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON 
   bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200
   ```

1. İşletim sistemi diskindeki boş alanın, VM 'deki bellek boyutundan (RAM) daha büyük olduğunu doğrulayın.

   İşletim sistemi diskinde yeterli alan yoksa, bellek dökümü dosyasının oluşturulacağı konumu değiştirin ve bu konumu, yeterli boş alana sahip olan VM 'ye bağlı herhangi bir veri diskine başvurun. Konumu değiştirmek için, aşağıdaki komutlarda **% systemroot%** değerini **F:** gibi veri diskinin sürücü harfiyle değiştirin.

   İşletim sistemi dökümünü etkinleştirmek için önerilen yapılandırma:

   **Bozuk işletim sistemi diskinden kayıt defteri kovanını yükle:**

   ```
   REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM
   ```

   **ControlSet001 üzerinde etkinleştir:**

   ```
   REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f 
   REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f 
   REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f 
   ```

   **ControlSet002 üzerinde etkinleştir:**

   ```
   REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f 
   REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f 
   REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f 
   ```

   **Bozuk işletim sistemi diskini kaldır:**

   ```
   REG UNLOAD HKLM\BROKENSYSTEM
   ```
   
### <a name="rebuild-the-vm"></a>VM 'yi yeniden oluşturma

VM 'yi yeniden derlemek için [VM onarım komutlarının 5. adımını](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) kullanın.
