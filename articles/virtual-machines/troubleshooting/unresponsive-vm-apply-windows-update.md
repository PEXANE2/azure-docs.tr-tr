---
title: Windows Update uygulanırken Azure VM, C01A001D hatasıyla yanıt vermiyor
description: Bu makalede, Windows Update 'in bir hata oluşturduğu ve bir Azure VM 'de yanıt vermeyen sorunları gidermeye yönelik adımlar sağlanmaktadır.
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
ms.openlocfilehash: 76c3f729a8520c7bff7b49a1d2200d7950f8a9f4
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87074297"
---
# <a name="vm-is-unresponsive-with-c01a001d-error-when-applying-windows-update"></a>Windows Update uygulanırken VM "C01A001D" hatası ile yanıt vermiyor

Bu makalede, Windows Update (KB) hata oluşturan ve bir Azure VM 'de yanıt vermeyen sorunları çözmek için adımlar sağlanmaktadır.

## <a name="symptoms"></a>Belirtiler

VM 'nin ekran görüntüsünü görüntülemek için [önyükleme tanılaması](./boot-diagnostics.md) kullanılırken, devam eden WINDOWS Update (KB) görüntülenir, ancak şu hata koduyla başarısız olur: ' C01A001D '.

![Yanıt vermeyen Windows Update](./media/unresponsive-vm-apply-windows-update/unresponsive-windows-update.png)

## <a name="cause"></a>Nedeni

Dosya sisteminde bir çekirdek dosya oluşturulamıyor. İşletim sistemi diske dosya yazamıyor.

## <a name="resolution"></a>Çözüm

### <a name="process-overview"></a>İşleme genel bakış

1. [Bir onarım VM 'Si oluşturun ve erişin](#create-and-access-a-repair-vm).
2. [Sabit diskte boş alan açın](#free-up-space-on-the-hard-disk).
3. [Önerilir: VM 'yi yeniden oluşturmadan önce, seri konsolu ve bellek dökümü toplamayı etkinleştirin](#recommended-before-rebuilding-the-vm-enable-serial-console-and-memory-dump-collection).
4. [VM 'Yi yeniden derleyin](#rebuild-the-vm).

> [!NOTE]
> Bu hata oluştuğunda, Konuk işletim sistemi çalışır durumda değildir. Bu sorunu çözmek için çevrimdışı modda sorun gidermeniz gerekir.

### <a name="create-and-access-a-repair-vm"></a>Bir onarım VM 'si oluşturma ve erişme

1. Bir onarım VM 'si hazırlamak için [VM onarım komutlarının 1-3 adımlarını](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) izleyin.
2. Uzak Masaüstü Bağlantısı kullanarak onarım sanal makinesine bağlanın.

### <a name="free-up-space-on-the-hard-disk"></a>Sabit diskte boş alan açın

Disk zaten 1 TB değilse, yeniden boyutlandırmalısınız. Disk 1 TB olduktan sonra Disk Temizleme ve sürücü birleştirmesi gerçekleştirin.

1. Diskin dolu olup olmadığını denetleyin. Disk 1 TB 'nin altındaysa, [PowerShell kullanarak en fazla 1 TB 'a genişletin](../windows/expand-os-disk.md?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json).
2. Disk 1 TB olduktan sonra bir disk temizleme işlemi gerçekleştirin.
    - [Veri diskini bozuk VM 'Den ayırın](../windows/detach-disk.md).
    - [Veri diskini çalışan BIR VM 'ye bağlayın](../windows/attach-disk-ps.md#attach-an-existing-data-disk-to-a-vm).
    - Boş alan boşaltmak için [Disk Temizleme aracını](https://support.microsoft.com/help/4026616/windows-10-disk-cleanup) kullanın.
3. Yeniden boyutlandırdıktan ve temizledikten sonra sürücüyü birleştirin:

    ```
    defrag <LETTER ASSIGN TO THE OS DISK>: /u /x /g
    ```
    Parçalama düzeyine bağlı olarak bu saat sürebilir.

### <a name="recommended-before-rebuilding-the-vm-enable-serial-console-and-memory-dump-collection"></a>Önerilir: VM 'yi yeniden oluşturmadan önce, seri konsolu ve bellek dökümü toplamayı etkinleştirin

1. Yükseltilmiş bir komut istemi oturumu açın (yönetici olarak çalıştır).
2. Aşağıdaki komutları çalıştırın:

    Seri konsolunu etkinleştir:

    ```
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200
    ```
3. İşletim sistemi diskindeki boş alanın en az VM bellek (RAM) boyutuna eşit olduğundan emin olun.

    İşletim sistemi diskinde yeterli alan yoksa, bellek dökümü dosyasının oluşturulacağı konumu değiştirin ve sanal makineye bağlı bir veri diskine ve yeterli boş alana başvurursunuz. Konumu değiştirmek için, `%SystemRoot%` aşağıdaki komutlarda veri diskinin sürücü harfiyle değiştirin (örneğin "F:"):

    **İşletim sistemi dökümünü etkinleştir önerilen yapılandırma:**

    Bozuk işletim sistemi diski yükle:

    ```
    REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM
    ```

    ControlSet001 üzerinde etkinleştir:

    ```
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f
    ```

    ControlSet002 üzerinde etkinleştir:

    ```
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f 
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f
    ```

    Bozuk işletim sistemi diskini kaldır:

    ```
    REG UNLOAD HKLM\BROKENSYSTEM
    ```

### <a name="rebuild-the-vm"></a>VM 'yi yeniden oluşturma

VM 'yi yeniden birleştirmek için [VM onarım komutlarının 5. adımını](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) kullanın.
