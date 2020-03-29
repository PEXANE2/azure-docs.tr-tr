---
title: VM güvenli modda önyükleme ler için Azure Sanal Makineleri'ne uzaktan bağlanamıyor | Microsoft Dokümanlar
description: VM Güvenli Mod'a girdiği için VM'ye RDP yapamayan bir sorunu nasıl gidermeyeceğinizi öğrenin.| Microsoft Dokümanlar
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
ms.openlocfilehash: 7bc2c0f472a03c3f069a889c360bea9017a780f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77918215"
---
#  <a name="cannot-rdp-to-a-vm-because-the-vm-boots-into-safe-mode"></a>VM Güvenli Mod'a önyükleme ler için VM'ye RDP yapılamaz

Bu makalede, VM Güvenli Mod'a önyükleme için yapılandırıldığından, Azure Windows Sanal Makineleri'ne (VM) bağlanamayacağınız bir sorunun nasıl çözüleceği gösterilmektedir.


## <a name="symptoms"></a>Belirtiler

VM Güvenli Mod'a önyükleme yapmak üzere yapılandırıldığından, Azure'daki bir VM'ye RDP bağlantısı veya başka bağlantılar (HTTP gibi) yapamazsınız. Azure portalındaki [Önyükleme tanılama](../troubleshooting/boot-diagnostics.md) bölümündeekran görüntüsünü kontrol ettiğinizde, VM önyüklemelerinin normal olduğunu görebilirsiniz, ancak ağ arabirimi kullanılamıyor:

![Güvenli Mod'da ağ çıkarı hakkında görüntü](./media/troubleshoot-rdp-safe-mode/network-safe-mode.png)

## <a name="cause"></a>Nedeni

RDP hizmeti Güvenli Mod'da kullanılamaz. VM Güvenli Mod'a girdiğinde yalnızca temel sistem programları ve hizmetleri yüklenir. Bu, Güvenli Mod'un "Güvenli Önyükleme minimal" ve "Bağlantılı Güvenli Önyükleme" olmak gibi iki farklı sürümü için geçerlidir.


## <a name="solution"></a>Çözüm

Bu adımları izlemeden önce, etkilenen VM'nin işletim sistemi diskinin bir anlık görüntüsünü yedek olarak alın. Daha fazla bilgi için [bir diskanlık anlık görüntüsüne](../windows/snapshot-copy-managed-disk.md)bakın.

Bu sorunu gidermek için, VM'yi normal moda önyükleme yapacak şekilde yapılandırmak veya kurtarma VM'i kullanarak [VM'yi çevrimdışı onarmak](#repair-the-vm-offline) için Seri denetimini kullanın.

### <a name="use-serial-control"></a>Seri denetimi kullanma

1. Seri [Konsola bağlanın ve CMD örneğini açın.](./serial-console-windows.md#use-cmd-or-powershell-in-serial-console
   ) VM'nizde Seri Konsol etkinleştirilemiyorsa, [VM'yi çevrimdışı onarmaya](#repair-the-vm-offline)bakın.
2. Önyükleme yapılandırma verilerini kontrol edin:

        bcdedit /enum

    VM Güvenli Mod'a önyükleme yapmak üzere yapılandırılırsa, **Windows Boot Loader** bölümünün altında **safeboot**adı verilen fazladan bir bayrak görürsünüz. **Güvenli önyükleme** bayrağını görmüyorsanız, VM Güvenli Mod'da değildir. Bu makale senaryonuz için geçerli değildir.

    **Safeboot** bayrağı aşağıdaki değerlerle görünebilir:
   - En az
   - Ağ

     Bu iki moddan herhangi birinde RDP başlatılacaktır. Bu nedenle, düzeltme aynı kalır.

     ![Güvenli Mod bayrağı hakkında görüntü](./media/troubleshoot-rdp-safe-mode/safe-mode-tag.png)

3. Güvenli **moade** bayrağını silin, böylece VM normal moda önyükleme yapacaktır:

        bcdedit /deletevalue {current} safeboot

4. **Emniyetönle** bayrağının kaldırıldığından emin olmak için önyükleme yapılandırma verilerini denetleyin:

        bcdedit /enum

5. VM'yi yeniden başlatın ve sorunun çözülüp çözülmediğini denetleyin.

### <a name="repair-the-vm-offline"></a>VM'yi çevrimdışı onar

#### <a name="attach-the-os-disk-to-a-recovery-vm"></a>Os diskini kurtarma VM'ine takın

1. [Os diskini kurtarma VM'sine takın.](../windows/troubleshoot-recovery-disks-portal.md)
2. Kurtarma VM'sine Uzak Masaüstü bağlantısı başlatın.
3. Diskin Disk Yönetimi konsolunda **Çevrimiçi** olarak işaretlendiğini unutmayın. Ekli işletim sistemi diskine atanan sürücü mektubuna dikkat edin.

#### <a name="enable-dump-log-and-serial-console-optional"></a>Döküm günlük ve Seri Konsol (isteğe bağlı) etkinleştirme

Bu makaledeki çözüm tarafından çözülmezse, döküm günlüğü ve Seri Konsol daha fazla sorun giderme yapmamıza yardımcı olur.

Döküm günlüğü ve Seri Konsol'u etkinleştirmek için aşağıdaki komut dosyasını çalıştırın.

1. Yükseltilmiş bir komut istemi oturumu açın (**Yönetici olarak çalıştırın).**
2. Şu betiği çalıştırın:

    Bu komut dosyasında, ekli işletim sistemi diskine atanan sürücü harfinin F olduğunu varsayıyoruz.

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

#### <a name="configure-the-windows-to-boot-into-normal-mode"></a>Windows'u normal moda önyükleme yapacak şekilde yapılandırma

1. Yükseltilmiş bir komut istemi oturumu açın (**Yönetici olarak çalıştırın).**
2. Önyükleme yapılandırma verilerini kontrol edin. Aşağıdaki komutlarda, ekli işletim sistemi diskine atanan sürücü harfinin F olduğunu varsayıyoruz.

        bcdedit /store F:\boot\bcd /enum
    **\windows** klasörüne sahip bölümün Tanımlayıcı adını not alın. Varsayılan olarak, Tanımlayıcı adı "Varsayılan" dır.

    VM Güvenli Mod'a önyükleme yapmak üzere yapılandırılırsa, **Windows Boot Loader** bölümünün altında **safeboot**adı verilen fazladan bir bayrak görürsünüz. **Güvenli önyükleme** bayrağını görmüyorsanız, bu makale senaryonuz için geçerli değildir.

    ![Önyükleme Tanımlayıcısı ile ilgili görüntü](./media/troubleshoot-rdp-safe-mode/boot-id.png)

3. Güvenli **önyükleme** bayrağını kaldırın, böylece VM normal moda önyükleme yapacaktır:

        bcdedit /store F:\boot\bcd /deletevalue {Default} safeboot
4. **Emniyetönle** bayrağının kaldırıldığından emin olmak için önyükleme yapılandırma verilerini denetleyin:

        bcdedit /store F:\boot\bcd /enum
5. [İşletim sistemi diskini ayırın ve VM'yi yeniden oluşturun.](../windows/troubleshoot-recovery-disks-portal.md) Ardından sorunun çözülüp çözülmediğini kontrol edin.
