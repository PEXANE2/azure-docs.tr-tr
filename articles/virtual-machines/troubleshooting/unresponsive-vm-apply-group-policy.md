---
title: İlke uygulanırken Azure sanal makinesi yanıt vermiyor
description: Bu makalede, bir Azure VM 'de başlangıç sırasında bir ilke uygulanırken yükleme ekranının yanıt vermediği sorunları gidermek için adımlar sağlanmaktadır.
services: virtual-machines-windows
documentationcenter: ''
author: TobyTu
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: a97393c3-351d-4324-867d-9329e31b5628
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 05/07/2020
ms.author: v-mibufo
ms.openlocfilehash: 187098f557cb691e023abb282a265b11e975c544
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84629257"
---
# <a name="vm-is-unresponsive-when-applying-group-policy-local-users-and-groups-policy"></a>Yerel Kullanıcılar ve Gruplar ilkesi grup ilkesi uygulanırken VM yanıt vermiyor

Bu makalede, bir Azure sanal makinesi (VM) başlangıç sırasında bir ilke uygularken yükleme ekranının yanıt vermediği sorunları gidermeye yönelik adımlar sağlanmaktadır.

## <a name="symptoms"></a>Belirtiler

VM 'nin bir ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) kullandığınızda ekran, "yerel kullanıcılar ve gruplar Ilkesi Grup ilkesi uygulanıyor" iletisiyle takılmış olur.

:::image type="content" source="media//unresponsive-vm-apply-group-policy/applying-group-policy-1.png" alt-text="Grup ilkesi yerel kullanıcılar ve Gruplar ilkesi yüklemeyi uygulama ekran görüntüsü (Windows Server 2012 R2).":::

:::image type="content" source="media/unresponsive-vm-apply-group-policy/applying-group-policy-2.png" alt-text="Grup ilkesi yerel kullanıcılar ve Gruplar ilkesi yüklemeyi uygulama ekran görüntüsü (Windows Server 2012).":::

## <a name="cause"></a>Nedeni

İlke eski Kullanıcı profillerini temizlemeye çalıştığında çakışan kilitler vardır.

> [!NOTE]
> Bu yalnızca Windows Server 2012 ve Windows Server 2012 R2 için geçerlidir.

Sorunlu ilke aşağıda verilmiştir:

`Computer Configuration\Policies\Administrative Templates\System/User Profiles\Delete user profiles older than a specified number of days on system restart`

## <a name="resolution"></a>Çözüm

### <a name="process-overview"></a>İşleme genel bakış

1. [Bir onarım VM 'si oluşturma ve erişme](#step-1-create-and-access-a-repair-vm)
1. [İlkeyi devre dışı bırak](#step-2-disable-the-policy)
1. [Seri konsolu ve bellek dökümü toplamayı etkinleştir](#step-3-enable-serial-console-and-memory-dump-collection)
1. [VM 'yi yeniden oluşturma](#step-4-rebuild-the-vm)

> [!NOTE]
> Bu önyükleme hatasıyla karşılaşırsanız, Konuk işletim sistemi çalışır durumda değildir. Çevrimdışı modda sorun gidermeniz gerekir.

### <a name="step-1-create-and-access-a-repair-vm"></a>1. Adım: bir onarım VM 'si oluşturma ve erişme

1. Bir onarım VM 'si hazırlamak için [VM onarım komutlarının 1-3 adımlarını](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) kullanın.
2. Onarım sanal makinesine bağlanmak için Uzak Masaüstü Bağlantısı kullanın.

### <a name="step-2-disable-the-policy"></a>2. Adım: ilkeyi devre dışı bırakma

1. VM 'yi Onar sayfasında, kayıt defteri düzenleyicisini açın.
1. **HKEY_LOCAL_MACHINE** anahtarı bulun ve menüden **Dosya**  >  **yükleme Hive** ' yi seçin.

    :::image type="content" source="media/unresponsive-vm-apply-group-policy/registry.png" alt-text="Ekran görüntüsü vurgulanan HKEY_LOCAL_MACHINE ve Load Hive içeren menüyü gösterir.":::

    - Çevrimdışı bir sistemden kayıt defteri anahtarlarını yüklemek için Hive Yükle ' ye yararlanabilirsiniz. Bu durumda, sistem, onarım sanal makinesine bağlı bozuk disktir.
    - Sistem genelinde ayarlar üzerinde depolanır `HKEY_LOCAL_MACHINE` ve "HKLM" olarak kısaltılabilir.
1. Ekli diskte, `\windows\system32\config\SOFTWARE` dosyaya gidin ve açın.

    1. Bir ad girmeniz istendiğinde, BROKENSOFTWARE ' i girin.
    1. BROKENSOFTWARE 'in yüklendiğini doğrulamak için **HKEY_LOCAL_MACHINE** genişletin ve eklenen bromi yazılım anahtarını arayın.
1. BROVKENSOFTWARE ' e gidin ve yüklenen Hive içinde CleanupProfile anahtarının var olup olmadığını denetleyin.

    1. Anahtar varsa, CleanupProfile ilkesi ayarlanır. Değeri, gün cinsinden ölçülen bekletme ilkesini temsil eder. Anahtarı silmeye devam edin.
    1. Anahtar yoksa, CleanupProfile ilkesi ayarlı değildir. Bağlı işletim sistemi diskinin Windows dizininde bulunan Memory. dmp dosyası dahil olmak üzere [bir destek bileti gönderme](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

1. CleanupProfiles anahtarını şu komutu kullanarak silin:

    ```
    reg delete "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows\System" /v CleanupProfiles /f
    ```
1.  Şu komutu kullanarak BROKENSOFTWARE kovanını kaldırın:

    ```
    reg unload HKLM\BROKENSOFTWARE
    ```

### <a name="step-3-enable-serial-console-and-memory-dump-collection"></a>3. Adım: seri konsolu ve bellek dökümü toplamayı etkinleştirme

Bellek dökümü toplamayı ve seri konsolunu etkinleştirmek için şu betiği çalıştırın:

1. Yükseltilmiş bir komut istemi oturumu açın. (Yönetici olarak çalıştır.)
1. Seri konsolunu etkinleştirmek için şu komutları çalıştırın:
    
    ```
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON
    ```

    ```
    bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200
    ```
1. İşletim sistemi diskindeki boş alanın en az VM 'nin bellek boyutuna (RAM) eşit olduğunu doğrulayın.

    İşletim sistemi diskinde yeterli alan yoksa, bellek dökümü konumunu değiştirin ve yeterli boş alana sahip bir bağlı veri diskine başvurun. Konumu değiştirmek için, "% SystemRoot%" değerini aşağıdaki komutlarda bulunan veri diskinin sürücü harfiyle (örneğin, "F:") değiştirin.

    **İşletim sistemi dökümünü etkinleştirmek için önerilen yapılandırma**

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

### <a name="step-4-rebuild-the-vm"></a>4. Adım: VM 'yi yeniden oluşturma

VM 'yi yeniden birleştirmek için [VM onarım komutlarının 5. adımını](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) kullanın.

Sorun giderilirse, ilke artık yerel olarak devre dışıdır. Kalıcı bir çözüm için VM 'lerde CleanupProfiles ilkesini kullanmayın. Profil temiztaları gerçekleştirmek için farklı bir yöntem kullanın.

Bu ilkeyi kullanmayın:

`Machine\Admin Templates\System\User Profiles\Delete user profiles older than a specified number of days on system restart`

## <a name="next-steps"></a>Sonraki adımlar

Windows Update uyguladığınızda sorunlarla karşılaşırsanız, [Windows Update uygulanırken VM 'nin "C01A001D" hatası ile yanıt vermemeye başladı](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/unresponsive-vm-apply-windows-update).
