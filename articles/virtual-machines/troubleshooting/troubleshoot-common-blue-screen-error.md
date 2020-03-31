---
title: Azure VM önyükleme yaparken mavi ekran hataları| Microsoft Dokümanlar
description: Önyükleme yaparken mavi ekran hatasının alındığı sorunu nasıl gidereceklerini öğrenin| Microsoft Dokümanlar
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 09/28/2018
ms.author: genli
ms.openlocfilehash: beb1562738699bbcede58d8214e69342abbb7c93
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79266942"
---
# <a name="windows-shows-blue-screen-error-when-booting-an-azure-vm"></a>Windows, Azure VM önyükleme yaparken mavi ekran hatası gösterir
Bu makalede, Microsoft Azure'da bir Windows Sanal Makine (VM) önyükleme yaptığınızda karşılaşabileceğiniz mavi ekran hataları açıklanmaktadır. Destek bileti için veri toplamanıza yardımcı olacak adımlar sağlar. 


## <a name="symptom"></a>Belirti 

Windows VM başlatılamıyor. [Önyükleme tanılamaönyükleme](./boot-diagnostics.md)ekran görüntüleri kontrol ettiğinizde, mavi ekranda aşağıdaki hata iletilerinden birini görürsünüz:

- bizim PC bir sorunla karşılaştı ve yeniden başlatması gerekiyor. Sadece bazı hata bilgileri topluyoruz, ve sonra yeniden başlatabilirsiniz.
- Bilgisayarınız bir sorunla karşılaştı ve yeniden başlatılması gerekiyor.

Bu bölümde, VM'leri yönetirken karşılaşabileceğiniz yaygın hata iletileri listelenebilir:

## <a name="cause"></a>Nedeni

Bir durdurma hatası almak neden olarak birden çok nedeni olabilir. En yaygın nedenleri şunlardır:

- Sürücüyle ilgili sorun
- Bozuk sistem dosyası veya bellek
- Bir uygulama belleğin yasak bir sektörüne erişiyor

## <a name="collect-memory-dump-file"></a>Bellek dökümü dosyasını toplama

Bu sorunu gidermek için, önce kilitlenme için döküm dosyasını toplamanız ve döküm dosyasıyla desteğe başvurmanız gerekir. Dökümü dosyasını toplamak için aşağıdaki adımları izleyin:

### <a name="attach-the-os-disk-to-a-recovery-vm"></a>Os diskini kurtarma VM'ine takın

1. Yedek olarak etkilenen VM'nin işletim sistemi diskinin anlık görüntüsünü alın. Daha fazla bilgi için [bir diskanlık anlık görüntüsüne](../windows/snapshot-copy-managed-disk.md)bakın.
2. [Os diskini kurtarma VM'sine takın.](../windows/troubleshoot-recovery-disks-portal.md) 
3. Kurtarma VM uzak masaüstü.

### <a name="locate-dump-file-and-submit-a-support-ticket"></a>Döküm dosyanı bulma ve destek bileti gönderme

1. Kurtarma VM'inde, bağlı işletim sistemi diskindeki windows klasörüne gidin. Bağlı işletim sistemi diskine atanan sürücü harfi F ise, F:\Windows'a gitmeniz gerekir.
2. Memory.dmp dosyasını bulun ve ardından döküm dosyasıyla [birlikte bir destek bileti gönderin.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) 

Döküm dosyasını bulamıyorsanız, döküm günlüğü ve Seri Konsol'u etkinleştirmek için bir sonraki adımı taşıyın.

### <a name="enable-dump-log-and-serial-console"></a>Döküm günlük ve Seri Konsol etkinleştirme

Döküm günlüğü ve Seri Konsol'u etkinleştirmek için aşağıdaki komut dosyasını çalıştırın.

1. Yükseltilmiş komut Istem oturumunu açın (Yönetici olarak çalıştırın).
2. Şu betiği çalıştırın:

    Bu komut dosyasında, ekli işletim sistemi diskine atanan sürücü harfinin F olduğunu varsayıyoruz.  VM'inizdeki uygun değerle değiştirin.

    ```powershell
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
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f

    reg unload HKLM\BROKENSYSTEM
    ```

    1. Diskte, bu VM için seçtiğiniz boyuta bağlı olarak RAM kadar bellek ayırmak için yeterli alan olduğundan emin olun.
    2. Yeterli alan yoksa veya bu büyük boyutlu bir VM (G, GS veya E serisi) ise, bu dosyanın oluşturulacağı konumu değiştirebilir ve bunu VM'ye bağlı başka bir veri diskine yönlendirebilirsiniz. Bunu yapmak için aşağıdaki anahtarı değiştirmeniz gerekir:

            reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv

            REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "<DRIVE LETTER OF YOUR DATA DISK>:\MEMORY.DMP" /f
            REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "<DRIVE LETTER OF YOUR DATA DISK>:\MEMORY.DMP" /f

            reg unload HKLM\BROKENSYSTEM

3. [Os diskini ayırın ve ardından işletim sistemi diskini etkilenen VM'ye yeniden takın.](../windows/troubleshoot-recovery-disks-portal.md)
4. Sorunu çoğaltmak için VM'yi başlatın, ardından bir döküm dosyası oluşturulur.
5. Os diskini kurtarma VM'sine takın, döküm dosyasını toplayın ve ardından döküm dosyasıyla [birlikte bir destek bileti gönderin.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)



