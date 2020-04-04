---
title: Azure VM, Windows Update uygularken C01A001D hatasıyla yanıt vermiyor
description: Bu makalede, Windows güncelleştirmesi bir hata oluşturur ve bir Azure VM yanıt vermez olur sorunları gidermek için adımlar sağlar.
services: virtual-machines-windows
documentationcenter: ''
author: TobyTu
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: a97393c3-351d-4324-867d-9329e31b3528
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 03/31/2020
ms.author: v-mibufo
ms.openlocfilehash: 16c8eed3377c2191b4345ec59ec1eba8be01369d
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633963"
---
# <a name="vm-is-unresponsive-with-c01a001d-error-when-applying-windows-update"></a>VM, Windows Update uygularken "C01A001D" hatasıyla yanıt vermiyor

Bu makalede, Windows Update 'in (KB) hata oluşturduğu ve Azure VM'de yanıt vermediği sorunları gidermek için adımlar sağlanmaktadır.

## <a name="symptoms"></a>Belirtiler

VM ekran görüntüsünü görüntülemek için [Boot tanılama](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) kullanırken, devam eden Windows Update (KB) görüntülenir, ancak hata kodu ile başarısız olur: 'C01A001D'.

![yanıt vermeyen Windows Update](./media/unresponsive-vm-apply-windows-update/unresponsive-windows-update.png)

## <a name="cause"></a>Nedeni

Dosya sisteminde çekirdek dosya oluşturulamıyor. İşletim sistemi diske dosya yazamıyor.

## <a name="resolution"></a>Çözüm

### <a name="process-overview"></a>İşleme genel bakış

1. [Bir onarım VM oluşturun ve erişin.](#create-and-access-a-repair-vm)
2. [Sabit diskte yer aç.](#free-up-space-on-the-hard-disk)
3. [Önerilen: VM'yi yeniden oluşturmadan önce seri konsol ve bellek dökümü koleksiyonunu etkinleştirin.](#recommended-before-rebuilding-the-vm-enable-serial-console-and-memory-dump-collection)
4. [VM'yi yeniden oluştur.](#rebuild-the-vm)

> [!NOTE]
> Bu hata oluştuğunda, Konuk İşletim Sistemi çalışmaz. Bu sorunu gidermek için çevrimdışı modda sorun gidermeniz gerekir.

### <a name="create-and-access-a-repair-vm"></a>Onarım VM oluşturma ve bu vm'ye erişme

1. Onarım VM'i hazırlamak için [VM Onarım Komutları'nın 1-3 adımlarını](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands) izleyin.
2. Uzak Masaüstü Bağlantısını kullanarak Onarım VM'sine bağlanın.

### <a name="free-up-space-on-the-hard-disk"></a>Sabit diskte yer aç

Disk zaten 1 Tb değilse, yeniden boyutlandırmanız gerekir. Disk 1 TB olduğunda, bir disk temizleme ve sürücünün parçalanması gerçekleştirin.

1. Diskin dolu olup olmadığını kontrol edin. Disk 1 Tb'ın altındaysa, [PowerShell kullanarak en fazla 1 Tb'a genişletin.](https://docs.microsoft.com/azure/virtual-machines/windows/expand-os-disk?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json)
2. Disk 1 Tb olduğunda, bir disk temizleme gerçekleştirin.
    - [Veri diskini bozuk VM'den ayırın.](https://docs.microsoft.com/azure/virtual-machines/windows/detach-disk)
    - [Veri diskini işleyen bir VM'ye takın.](https://docs.microsoft.com/azure/virtual-machines/windows/attach-disk-ps#attach-an-existing-data-disk-to-a-vm)
    - Yer açmak için [Disk Temizleme aracını](https://support.microsoft.com/help/4026616/windows-10-disk-cleanup) kullanın.
3. Yeniden boyutlandırma ve temizlemeden sonra, sürücüyü parçala:

    ```
    defrag <LETTER ASSIGN TO THE OS DISK>: /u /x /g
    ```
    Parçalanma seviyesine bağlı olarak, bu saatler sürebilir.

### <a name="recommended-before-rebuilding-the-vm-enable-serial-console-and-memory-dump-collection"></a>Önerilen: VM'yi yeniden oluşturmadan önce seri konsol ve bellek dökümü koleksiyonunu etkinleştirin

1. Yükseltilmiş bir komut istemi oturumu açın (Yönetici olarak çalıştırın).
2. Aşağıdaki komutları çalıştırın:

    Seri Konsolu Etkinleştir:

    ```
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200
    ```
3. Os diskindeki boş alanın en azından VM bellek (RAM) boyutuna eşit olduğundan emin olun.

    İşletim sistemi diskinde yeterli alan yoksa, bellek dökümü dosyasının oluşturulacağı konumu değiştirin ve VM'ye bağlı ve yeterli boş alana sahip bir veri diskine yönlendirin. Konumu değiştirmek için, `%SystemRoot%` aşağıdaki komutlarda veri diskinin sürücü harfini (örneğin "F:") ile değiştirin:

    **İşletim sistemi dökümü önerilen yapılandırmayı etkinleştirin:**

    Yük Kırık İşletim Sistemi Diski:

    ```
    REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM
    ```

    ControlSet001'de etkinleştirin:

    ```
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f
    ```

    ControlSet002'de etkinleştirin:

    ```
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f 
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f
    ```

    Bozuk işletim sistemi diskini boşaltın:

    ```
    REG UNLOAD HKLM\BROKENSYSTEM
    ```

### <a name="rebuild-the-vm"></a>VM'yi yeniden oluşturma

VM'yi yeniden birleştirmek için [VM onarım komutlarının adım](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) 5'ini kullanın.
