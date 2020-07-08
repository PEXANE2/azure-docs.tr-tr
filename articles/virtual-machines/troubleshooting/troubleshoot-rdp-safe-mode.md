---
title: VM güvenli modda önyüklendiği için Azure sanal makinelerine uzaktan bağlanılamıyor | Microsoft Docs
description: VM, güvenli modda önyüklendiği için bir VM 'de RDP ile ilgili sorun giderme hakkında bilgi edinin. | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/13/2018
ms.author: genli
ms.openlocfilehash: f1ffd26a243d15f7ee6e06d6c52406a16327b4a0
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86086781"
---
#  <a name="cannot-rdp-to-a-vm-because-the-vm-boots-into-safe-mode"></a>VM, güvenli modda önyüklendiği için bir VM 'ye RDP uygulanamıyor

Bu makalede, sanal makine güvenli modda önyüklenecek şekilde yapılandırıldığından Azure Windows Sanal Makineleri (VM 'Ler) ile bağlanamadaki bir sorunu çözme işlemi gösterilmektedir.


## <a name="symptoms"></a>Belirtiler

VM güvenli modda önyüklenecek şekilde yapılandırıldığından, RDP bağlantısı veya diğer bağlantıları (HTTP gibi) Azure 'daki bir VM 'ye yapamazsınız. Azure portal [önyükleme tanılamasında](../troubleshooting/boot-diagnostics.md) ekran görüntüsünü denetlediğinizde, sanal makinenin normal olarak önyüklenebileceğini görebilirsiniz, ancak ağ arabirimi kullanılabilir değildir:

![Güvenli modda ağ Inse ile ilgili görüntü](./media/troubleshoot-rdp-safe-mode/network-safe-mode.png)

## <a name="cause"></a>Nedeni

RDP hizmeti güvenli modda kullanılamıyor. VM güvenli modda önyüklendiğinde yalnızca gerekli sistem programları ve Hizmetleri yüklenir. Bu, "güvenli önyükleme en düşük" ve "bağlantı ile güvenli önyükleme" olan iki farklı güvenli mod sürümü için geçerlidir.


## <a name="solution"></a>Çözüm

Bu adımları izlemeden önce, etkilenen VM 'nin işletim sistemi diskinin bir yedek olarak anlık görüntüsünü alın. Daha fazla bilgi için bkz. [disk anlık görüntüsü](../windows/snapshot-copy-managed-disk.md).

Bu sorunu çözmek için, VM 'yi normal modda önyüklenecek şekilde yapılandırmak veya bir kurtarma VM kullanarak [VM 'yi çevrimdışı olarak onarmak](#repair-the-vm-offline) üzere seri denetim kullanın.

### <a name="use-serial-control"></a>Seri denetim kullan

1. [Seri konsoluna bağlanın ve cmd örneğini açın](./serial-console-windows.md#use-cmd-or-powershell-in-serial-console
   ). VM 'niz üzerinde seri konsol etkinleştirilmemişse, bkz. [VM 'yi çevrimdışı olarak onarma](#repair-the-vm-offline).
2. Önyükleme yapılandırma verilerini denetleyin:

    ```console
    bcdedit /enum
    ```

    VM güvenli modda önyüklenecek şekilde yapılandırıldıysa, **Windows önyükleme yükleyicisi** bölümünde **safeboot**adlı bir ek bayrak görürsünüz. **Safeboot** bayrağını görmüyorsanız, sanal makine güvenli modda değildir. Bu makale senaryonuz için geçerlidir.

    **Safeboot** bayrağı aşağıdaki değerlerle görüntülenebilir:
   - En az
   - Ağ

     Bu iki moddan birinde, RDP başlatılmayacak. Bu nedenle, bu çözüm aynı kalır.

     ![Güvenli mod bayrağıyla ilgili resim](./media/troubleshoot-rdp-safe-mode/safe-mode-tag.png)

3. **Safemoade** BAYRAĞıNı silerek VM normal modda önyüklenir:

    ```console
    bcdedit /deletevalue {current} safeboot
    ```

4. **Güvenli önyükleme** bayrağının kaldırıldığından emin olmak için önyükleme yapılandırma verilerini denetleyin:

    ```console
    bcdedit /enum
    ```

5. VM 'yi yeniden başlatın ve sorunun çözümlenip çözümlenmediğini denetleyin.

### <a name="repair-the-vm-offline"></a>VM 'yi çevrimdışı onarma

#### <a name="attach-the-os-disk-to-a-recovery-vm"></a>İşletim sistemi diskini bir kurtarma VM 'sine iliştirme

1. [İşletim sistemi diskini bir kurtarma sanal makinesine ekleyin](../windows/troubleshoot-recovery-disks-portal.md).
2. Kurtarma VM 'sine bir Uzak Masaüstü bağlantısı başlatın.
3. Diskin Disk Yönetimi konsolunda **çevrimiçi** olarak işaretlendiğinden emin olun. Bağlı işletim sistemi diskine atanan sürücü harfini unutmayın.

#### <a name="enable-dump-log-and-serial-console-optional"></a>Döküm günlüğünü ve seri konsolunu etkinleştir (isteğe bağlı)

Döküm günlüğü ve seri konsol, bu makaledeki çözüm tarafından sorunu çözülemezse daha fazla sorun giderme yapmamıza yardımcı olur.

Döküm günlüğünü ve seri konsolunu etkinleştirmek için aşağıdaki betiği çalıştırın.

1. Yükseltilmiş bir komut istemi oturumu açın (**yönetici olarak çalıştır**).
2. Şu betiği çalıştırın:

    Bu betikte, bağlı işletim sistemi diskine atanan sürücü harfinin F olduğunu varsaytık. Bu sürücü harfini VM 'niz için uygun değerle değiştirin.

    ```powershell
    reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM

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

#### <a name="configure-the-windows-to-boot-into-normal-mode"></a>Windows 'ı normal modda önyüklenecek şekilde yapılandırma

1. Yükseltilmiş bir komut istemi oturumu açın (**yönetici olarak çalıştır**).
2. Önyükleme yapılandırma verilerini denetleyin. Aşağıdaki komutlarda, bağlı işletim sistemi diskine atanan sürücü harfinin F olduğunu varsaytık. Bu sürücü harfini VM 'niz için uygun değerle değiştirin.

    ```console
    bcdedit /store F:\boot\bcd /enum
    ```

    **\Windows** klasörünün bulunduğu bölümün tanımlayıcı adını unutmayın. Varsayılan olarak, tanımlayıcı adı "varsayılan" ' dır.

    VM güvenli modda önyüklenecek şekilde yapılandırıldıysa, **Windows önyükleme yükleyicisi** bölümünde **safeboot**adlı bir ek bayrak görürsünüz. **Safeboot** bayrağını görmüyorsanız, bu makale senaryonuz için geçerlidir.

    ![Önyükleme tanımlayıcısı ile ilgili görüntü](./media/troubleshoot-rdp-safe-mode/boot-id.png)

3. **Safeboot** bayrağını kaldırın, bu nedenle VM normal modda önyüklenir:

    ```console
    bcdedit /store F:\boot\bcd /deletevalue {Default} safeboot
    ```

4. **Güvenli önyükleme** bayrağının kaldırıldığından emin olmak için önyükleme yapılandırma verilerini denetleyin:

    ```console
    bcdedit /store F:\boot\bcd /enum
    ```

5. [İşletim sistemi diskini ayırın ve VM 'yi yeniden oluşturun](../windows/troubleshoot-recovery-disks-portal.md). Sonra sorunun çözümlenip çözümlenmediğini denetleyin.
