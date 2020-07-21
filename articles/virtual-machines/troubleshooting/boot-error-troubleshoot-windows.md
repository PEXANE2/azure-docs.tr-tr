---
title: Azure sanal makineleri kapatma, yeniden başlatma, kapatma veya hizmetleri durdurma sırasında takıldı | Microsoft Docs
description: Bu makale, Azure Windows Sanal Makineleri 'de hizmet hatalarını gidermenize yardımcı olur.
services: virtual-machines-windows
documentationCenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/19/2019
ms.author: tibasham
ms.openlocfilehash: e5ab1262def78da4971ea6e5535f3ac915a38ec8
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86526767"
---
# <a name="azure-windows-vm-shutdown-is-stuck-on-restarting-shutting-down-or-stopping-services"></a>Azure Windows VM kapatma, "yeniden başlatılıyor", "kapatılıyor" veya "Hizmetleri durduruluyor" üzerine takılmış

Bu makalede, Microsoft Azure ' de bir Windows sanal makinesini (VM) yeniden başlattığınızda karşılaşabileceğiniz "yeniden başlatma", "kapatma" veya "hizmetleri durdurma" iletilerinin sorunları çözümlenme adımları sağlanmaktadır.

## <a name="symptoms"></a>Belirtiler

VM 'nin ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](./boot-diagnostics.md) kullandığınızda, ekran görüntüsünde "yeniden başlatılıyor", "kapatılıyor" veya "Hizmetleri durduruluyor" iletisini görüntülüyor olabilirsiniz.

![Hizmet ekranlarını yeniden başlatma, kapatma ve durdurma](./media/boot-error-troubleshooting-windows/restart-shut-down-stop-service.png)
 
## <a name="cause"></a>Nedeni

Windows, sistem bakım işlemlerini gerçekleştirmek ve güncelleştirmeler, roller ve özellikler gibi değişiklikleri işlemek için kapalı işlemini kullanır. Tamamlanana kadar bu kritik işlemi kesmeniz önerilmez. Güncelleştirme/değişiklik sayısına ve VM boyutuna bağlı olarak, işlem uzun sürebilir. İşlem durdurulmuşsa, işletim sisteminin bozuk olması mümkündür. Yalnızca aşırı uzun sürüyorsa işlemi keser.

## <a name="solution"></a>Çözüm

### <a name="collect-a-process-memory-dump"></a>Işlem belleği dökümünü topla

1. [ProcDump aracını](http://download.sysinternals.com/files/Procdump.zip) , aynı bölgedeki çalışan bir sanal makineye bağlı yeni veya mevcut bir veri diskine indirin.

2. Çalışan VM 'den gerekli olan dosyaları içeren diski ayırın ve diski bozuk sanal makinenize bağlayın. Bu diski **yardımcı program diskine**arıyoruz.

Aşağıdaki adımları gerçekleştirmek için [seri konsol](./serial-console-windows.md) kullanın:

1. Bir yönetim PowerShell açın ve durdurma sırasında yanıt vermeyi durduran hizmeti kontrol edin.

   ``
   Get-Service | Where-Object {$_.Status -eq "STOP_PENDING"}
   ``

2. Bir yönetim CMD 'de, yanıt vermeyen hizmetin PID 'sini alın.

   ``
   tasklist /svc | findstr /i <STOPING SERVICE>
   ``

3. Yanıt vermeyen işlemden bir bellek dökümü örneği alın <STOPPING SERVICE> .

   ``
   procdump.exe -s 5 -n 3 -ma <PID>
   ``

4. Şimdi kapatma işleminin kilidini açmak için yanıt vermeyen işlemi sonlandırın.

   ``
   taskkill /PID <PID> /t /f
   ``

İşletim sistemi yeniden başlatıldıktan sonra, normal olarak önyükleniyorsa, işletim sistemi tutarlılığının tamam olduğundan emin olun. Bozulma bildirilmezse, disk bozuluncaya kadar aşağıdaki komutu çalıştırın:

``
dism /online /cleanup-image /restorehealth
``

İşlem belleği dökümünü toplayadıysanız veya bu sorun özyinelemeli ise ve bir kök neden analizi istiyorsanız, aşağıda bir işletim sistemi bellek dökümü toplamaya devam edin, bir destek isteği açmaya devam edin.

### <a name="collect-an-os-memory-dump"></a>Bir işletim sistemi bellek dökümü toplayın

Değişikliklerin işlenmesi beklendikten sonra Sorun çözümlenmezse, bir bellek dökümü dosyası toplayıp desteğe başvurmanız gerekir. Döküm dosyasını toplamak için aşağıdaki adımları izleyin:

**İşletim sistemi diskini bir kurtarma VM 'sine iliştirme**

1. Etkilenen VM 'nin işletim sistemi diskinin anlık görüntüsünü bir yedekleme olarak alın. Daha fazla bilgi için bkz. [disk anlık görüntüsü](../windows/snapshot-copy-managed-disk.md).

2. [İşletim sistemi diskini bir kurtarma sanal makinesine ekleyin](./troubleshoot-recovery-disks-portal-windows.md).

3. Kurtarma sanal makinesine uzak masaüstü.

4. İşletim sistemi diski şifrelenirse, bir sonraki adıma geçmeden önce şifrelemeyi kapatmanız gerekir. Daha fazla bilgi için bkz. [önyükleme YAPıLABILEN VM 'de şifrelenmiş işletim sistemi diskinin şifresini çözme](./troubleshoot-bitlocker-boot-error.md#solution).

**Döküm dosyasını bul ve bir destek bileti gönder**

1. Kurtarma VM 'sinde, bağlı işletim sistemi diskinde Windows klasörü ' ne gidin. Bağlı işletim sistemi diskine atanan sürücü harfi F ise, F:\windowsadresine gitmeniz gerekir.

2. Memory. dmp dosyasını bulun ve ardından döküm dosyası ile [bir destek bileti gönderebilirsiniz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

Döküm dosyasını bulamıyorsanız, döküm günlüğünü ve seri konsolunu etkinleştirmek için sonraki adımı taşıyın.

**Döküm günlüğünü ve seri konsolunu etkinleştir**

Döküm günlüğünü ve seri konsolunu etkinleştirmek için aşağıdaki betiği çalıştırın.

1. Yükseltilmiş komut Istemi oturumunu açın (yönetici olarak çalıştır).

2. Şu betiği çalıştırın:

   Bu betikte, bağlı işletim sistemi diskine atanan sürücü harfinin F olduğunu varsaytık. bunu sanal makinenizde uygun bir değerle değiştirin.

   ```
   reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv
   
   REM Enable Serial Console
   bcdedit /store F:\boot\bcd /set {bootmgr} displaybootmenu yes
   bcdedit /store F:\boot\bcd /set {bootmgr} timeout 5
   bcdedit /store F:\boot\bcd /set {bootmgr} bootems yes
   bcdedit /store F:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
   bcdedit /store F:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200
   
   REM Suggested configuration to enable OS Dump
   REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
   REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
   REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

   REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
   REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
   REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump / t REG_DWORD /d 1 /f
   
   reg unload HKLM\BROKENSYSTEM
   ```

3. Bu VM için seçtiğiniz boyuta bağlı olarak RAM 'e kadar bellek ayırmak için diskte yeterli alan olduğunu doğrulayın.

4. Yeterli alan yoksa veya VM büyükse (G, GS veya E serisi), bu dosyanın oluşturulacağı konumu değiştirebilir ve sanal makineye bağlı olan diğer tüm veri diskine başvurabilirsiniz. Konumu değiştirmek için aşağıdaki anahtarı değiştirmeniz gerekir:

   ```
   reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

   REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "<DRIVE LETTER OF YOUR DATA DISK>:\MEMORY.DMP" /f
   REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "<DRIVE LETTER OF YOUR DATA DISK>:\MEMORY.DMP" /f
   
   reg unload HKLM\BROKENSYSTEM
   ```

5. [İşletim sistemi diskini ayırın ve ardından işletim sistemi diskini ETKILENEN VM 'ye yeniden bağlayın](./troubleshoot-recovery-disks-portal-windows.md).

6. VM 'yi başlatın ve seri konsoluna erişin.

7. Bellek dökümünü tetiklemek için, maskelenemeyen kesme (NMI) Gönder ' i seçin.

   ![Maskelenemeyen kesme gönder](./media/boot-error-troubleshooting-windows/send-nonmaskable-interrupt.png)

8. İşletim sistemi diskini bir kurtarma VM 'sine yeniden ekleyin, döküm dosyasını toplayın.

## <a name="contact-microsoft-support"></a>Microsoft desteğine başvurma

Döküm dosyasını topladıktan sonra, kök nedenini öğrenmek için Microsoft desteği 'ne başvurun.
