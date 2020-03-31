---
title: VM başlatma "Windows'u hazırlamaya" takılıp kaldı. Azure'da bilgisayarınızı kapatmayın| Microsoft Dokümanlar
description: VM başlatmanın "Windows'u hazırlama"ya takılıp kaldığı sorunu gidermek için adımları tanıyın. Bilgisayarınızı kapatmayın.” iletisinde takılıyor
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 09/18/2018
ms.author: delhan
ms.openlocfilehash: da45e24898bc3b5aead250077af69a61bdb33bab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73749630"
---
# <a name="vm-startup-is-stuck-on-getting-windows-ready-dont-turn-off-your-computer-in-azure"></a>VM başlatma "Windows'u hazırlamaya" takılıp kaldı. Azure'da bilgisayarınızı kapatmayın"

Bu makalede, Microsoft Azure'da bir Windows sanal makineyi (VM) önyükleme yaptığınızda karşılaşabileceğiniz "Hazırlanma" ve "Windows'u hazırlama" ekranları açıklanmaktadır. Destek bileti için veri toplamanıza yardımcı olacak adımlar sağlar.

 

## <a name="symptoms"></a>Belirtiler

Windows VM önyükleme yapmaz. VM'nin ekran görüntüsünü almak için **Önyükleme tanılama** yöntemini kullandığınızda, VM'nin "Hazırlanıyor" veya "Windows'u hazırlama" iletisini görüntülediğini görebilirsiniz.

![Windows Server 2012 R2 için ileti örneği](./media/troubleshoot-vm-configure-update-boot/message1.png)

![İleti örneği](./media/troubleshoot-vm-configure-update-boot/message2.png)

## <a name="cause"></a>Nedeni

Genellikle bu sorun, sunucu yapılandırma değiştirildikten sonra son yeniden başlatmayı yaparken oluşur. Yapılandırma değişikliği, Windows güncelleştirmeleri veya sunucunun rollerindeki/özelliğindeki değişikliklertarafından başlatıldı. Windows Update için, güncelleştirmelerin boyutu büyükse, işletim sisteminin değişiklikleri yeniden yapılandırması için daha fazla zamana ihtiyacı vardır.

## <a name="collect-an-os-memory-dump"></a>İşletim sistemi bellek dökümü toplama

Değişikliklerin işlemesini bekledikten sonra sorun çözülmezse, bir bellek döküm dosyası ve iletişim desteği toplamanız gerekir. Dökümü dosyasını toplamak için aşağıdaki adımları izleyin:

### <a name="attach-the-os-disk-to-a-recovery-vm"></a>Os diskini kurtarma VM'ine takın

1. Yedek olarak etkilenen VM'nin işletim sistemi diskinin anlık görüntüsünü alın. Daha fazla bilgi için [bir diskanlık anlık görüntüsüne](../windows/snapshot-copy-managed-disk.md)bakın.
2. [Os diskini kurtarma VM'sine takın.](../windows/troubleshoot-recovery-disks-portal.md)
3. Kurtarma VM uzak masaüstü. 
4. İşletim sistemi diski şifrelenmişse, bir sonraki adıma geçmeden önce şifrelemeyi kapatmanız gerekir. Daha fazla bilgi için [VM'de önyükleme yapamayan şifreli işletim sistemi diskinin şifresini çözeme bölümüne](troubleshoot-bitlocker-boot-error.md#solution)bakın.

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
4. VM'yi başlatın ve Seri Konsol'a erişin.
5. Bellek dökümünü tetiklemek için **Maskelemeyen Kesme (NMI) Gönder'i** seçin.
    ![Maskelenebilir Olmayan Kesme nereye gönderilen hakkında görüntü](./media/troubleshoot-vm-configure-update-boot/run-nmi.png)
6. Os diskini kurtarma VM'ine yeniden takın, döküm dosyasını toplayın.

## <a name="contact-microsoft-support"></a>Microsoft desteğine başvurma

Döküm dosyasını topladıktan sonra, kök nedeni çözümlemek için [Microsoft desteğine](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) başvurun.