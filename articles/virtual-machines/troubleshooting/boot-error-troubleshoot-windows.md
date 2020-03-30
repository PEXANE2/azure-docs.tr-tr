---
title: Azure Sanal Makineler kapatma, Hizmetleri Yeniden Başlatma, Kapatma veya Durdurma | Microsoft Dokümanlar
description: Bu makale, Azure Windows Sanal Makineler'deki hizmet hatalarını gidermenize yardımcı olur.
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
ms.openlocfilehash: 5d6396efc9ab25baa0d32e7c33c7715863516249
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77371362"
---
# <a name="azure-windows-vm-shutdown-is-stuck-on-restarting-shutting-down-or-stopping-services"></a>Azure Windows VM kapatması "Yeniden Başlatma", "Kapatma" veya "Hizmetleri Durdurma" üzerinde takılıp kaldı

Bu makalede, Microsoft Azure'da bir Windows sanal makinesini (VM) yeniden başlatırken karşılaşabileceğiniz "Yeniden Başlatma", "Kapatma" veya "Hizmetleri durdurma" iletileri sorunlarını gidermek için adımlar sağlanmaktadır.

## <a name="symptoms"></a>Belirtiler

VM'nin ekran görüntüsünü görüntülemek için [Önyükleme tanılama](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) yöntemini kullandığınızda, ekran görüntüsünün "Yeniden Başlatma", "Kapatma" veya "Hizmetleri durdurma" iletisini görüntülediğini görebilirsiniz.

![Hizmetleri Yeniden Başlatma, Kapatma ve Durdurma Hizmetleri Ekranları](./media/boot-error-troubleshooting-windows/restart-shut-down-stop-service.png)
 
## <a name="cause"></a>Nedeni

Windows, sistem bakım işlemlerini gerçekleştirmek ve güncelleştirmeler, roller ve özellikler gibi değişiklikleri işlemek için kapatma işlemini kullanır. Tamamlanana kadar bu kritik işlemi kesintiye uğratmanız önerilmez. Güncelleştirmelerin/değişikliklerin sayısına ve VM boyutuna bağlı olarak işlem uzun sürebilir. İşlem durdurulursa, işletim sistemi bozulması mümkündür. Sadece aşırı uzun sürüyorsa işlemi yarıda kes.

## <a name="solution"></a>Çözüm

### <a name="collect-a-process-memory-dump"></a>İşlem bellek dökümü toplama

1. [Procdump aracını,](http://download.sysinternals.com/files/Procdump.zip) aynı bölgeden çalışan bir VM'ye bağlı olan yeni veya varolan bir veri diskine indirin.

2. Çalışan VM'den gerekli dosyaları içeren diski ayırın ve diski bozuk VM'nize takın. Bu diske **Yardımcı Disk**diyoruz.

Aşağıdaki adımları tamamlamak için [Seri Konsol'u](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-windows) kullanın:

1. İdari bir Powershell açın ve durdurma üzerine asılı olan hizmeti kontrol edin.

   ``
   Get-Service | Where-Object {$_.Status -eq "STOP_PENDING"}
   ``

2. İdari bir CMD'de, askıda olan hizmetin PID'sini alın.

   ``
   tasklist /svc | findstr /i <STOPING SERVICE>
   ``

3. Asılı işlemden <STOPPING SERVICE>bir bellek dökümü örneği alın.

   ``
   procdump.exe -s 5 -n 3 -ma <PID>
   ``

4. Şimdi kapatma işlemini kilidini açmak için asılı işlemi öldürmek.

   ``
   taskkill /PID <PID> /t /f
   ``

Bir kez işletim sistemi yeniden başlar, normal önyükleme, o zaman sadece işletim sistemi tutarlılık ok olduğundan emin olun. Bozulma bildirilirse, disk bozulmadan gelene kadar aşağıdaki komutu çalıştırın:

``
dism /online /cleanup-image /restorehealth
``

Bir işlem bellek dökümü toplamak mümkün değilse veya bu sorun özyinelemeli ve bir kök neden çözümleme gerekiyorsa, aşağıda bir işletim sistemi bellek dökümü toplamaya devam, bir destek isteği açmak için devam edin.

### <a name="collect-an-os-memory-dump"></a>İşletim sistemi bellek dökümü toplama

Değişikliklerin işlemesini bekledikten sonra sorun çözülmezse, bir bellek döküm dosyası ve iletişim desteği toplamanız gerekir. Dökümü dosyasını toplamak için aşağıdaki adımları izleyin:

**Os diskini kurtarma VM'ine takın**

1. Yedek olarak etkilenen VM'nin işletim sistemi diskinin anlık görüntüsünü alın. Daha fazla bilgi için [bir diskanlık anlık görüntüsüne](https://docs.microsoft.com/azure/virtual-machines/windows/snapshot-copy-managed-disk)bakın.

2. [Os diskini kurtarma VM'sine takın.](https://docs.microsoft.com/azure/virtual-machines/windows/troubleshoot-recovery-disks-portal)

3. Kurtarma VM uzak masaüstü.

4. İşletim sistemi diski şifrelenmişse, bir sonraki adıma geçmeden önce şifrelemeyi kapatmanız gerekir. Daha fazla bilgi için [VM'de önyükleme yapamayan şifreli işletim sistemi diskinin şifresini çözeme bölümüne](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-bitlocker-boot-error#solution)bakın.

**Döküm dosyanı bulma ve destek bileti gönderme**

1. Kurtarma VM'inde, bağlı işletim sistemi diskindeki windows klasörüne gidin. Bağlı işletim sistemi diskine atanan sürücü harfi F ise, F:\Windows'a gitmeniz gerekir.

2. Memory.dmp dosyasını bulun ve ardından döküm dosyasıyla [birlikte bir destek bileti gönderin.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)

Döküm dosyasını bulamıyorsanız, döküm günlüğü ve Seri Konsol'u etkinleştirmek için bir sonraki adımı taşıyın.

**Döküm günlük ve Seri Konsol etkinleştirme**

Döküm günlüğü ve Seri Konsol'u etkinleştirmek için aşağıdaki komut dosyasını çalıştırın.

1. Yükseltilmiş komut Istem oturumunu açın (Yönetici olarak çalıştırın).

2. Şu betiği çalıştırın:

   Bu komut dosyasında, ekli işletim sistemi diskine atanan sürücü harfinin F olduğunu varsayıyoruz.

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

3. Diskte, bu VM için seçtiğiniz boyuta bağlı olarak RAM kadar bellek ayırmak için yeterli alan olduğunu doğrulayın.

4. Yeterli alan yoksa veya VM büyükse (G, GS veya E serisi), bu dosyanın oluşturulacağı konumu değiştirebilir ve bunu VM'ye bağlı başka bir veri diskine yönlendirebilirsiniz. Konumu değiştirmek için aşağıdaki anahtarı değiştirmeniz gerekir:

   ```
   reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

   REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "<DRIVE LETTER OF YOUR DATA DISK>:\MEMORY.DMP" /f
   REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "<DRIVE LETTER OF YOUR DATA DISK>:\MEMORY.DMP" /f
   
   reg unload HKLM\BROKENSYSTEM
   ```

5. [Os diskini ayırın ve işletim sistemi diskini etkilenen VM'ye yeniden takın.](https://docs.microsoft.com/azure/virtual-machines/windows/troubleshoot-recovery-disks-portal)

6. VM'yi başlatın ve Seri Konsol'a erişin.

7. Bellek dökümünü tetiklemek için Maskelemeyen Kesme Gönder'i (NMI) seçin.

   ![Maskelenemeyen Kesme Gönder](./media/boot-error-troubleshooting-windows/send-nonmaskable-interrupt.png)

8. Os diskini kurtarma VM'ine yeniden takın, döküm dosyasını toplayın.

## <a name="contact-microsoft-support"></a>Microsoft desteğine başvurma

Döküm dosyasını topladıktan sonra, temel nedenini belirlemek için Microsoft desteğine başvurun.
