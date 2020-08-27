---
title: Windows Dur hatası-0x00000074 hatalı sistem yapılandırma bilgileri
description: Bu makalede, bir Azure sanal makinesindeki (VM) bozuk sistem yapılandırma bilgileri nedeniyle Windows 'un önyükleme yapması gerekmediği ve yeniden başlatılması gereken sorunları gidermeye yönelik adımlar sağlanmaktadır.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: a1e47f6a-c7d5-4327-a7b0-ad48ed543641
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 08/24/2020
ms.author: v-miegge
ms.openlocfilehash: 071b5786127af31a2ad3266c128dbfb7cacad656
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88942190"
---
# <a name="windows-stop-error---0x00000074-bad-system-config-info"></a>Windows Dur hatası-0x00000074 hatalı sistem yapılandırma bilgileri

Bu makalede, bir Azure sanal makinesindeki (VM) bozuk sistem yapılandırma bilgileri nedeniyle Windows 'un önyükleme yapması gerekmediği ve yeniden başlatılması gereken sorunları gidermeye yönelik adımlar sağlanmaktadır.

## <a name="symptom"></a>Belirti

VM 'nin ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) kullandığınızda, ekran görüntüsünde Windows dur kodu **#0x00000074** veya **BAD_SYSTEM_CONFIG_INFO**görüntülenir.

*Bilgisayarınız bir sorunla karşılaştı ve yeniden başlatılması gerekiyor. Yeniden başlatabilirsiniz.* 
 *Bu sorun ve olası düzeltmeler hakkında daha fazla bilgi için şu http://windows.com/stopcode adresi ziyaret edin* 
 *Bir destek kişisi çağırırsanız, bu bilgileri verin:* 
 *Kodu durdur: BAD_SYSTEM_CONFIG_INFO*

  ![Windows, "BAD_SYSTEM_CONFIG_INFO" olarak da gösterilen 0x00000074 kodunu durdurur. Windows, kullanıcıya BILGISAYARıN bir sorunla karşılaşdığını ve yeniden başlatılması gerektiğini bildirir.](./media/windows-stop-error-bad-system-config-info/1.png)

## <a name="cause"></a>Nedeni

**BAD_SYSTEM_CONFIG_INFO** dur kodu, **sistem** kayıt defteri kovanı bozuk görünüyorsa oluşur. Bu hatanın nedeni aşağıdakilerden biri olabilir:

- Kayıt defteri kovanı düzgün şekilde kapatılmadı.
- Kayıt defteri kovanı bozuk.
- Eksik kayıt defteri anahtarları veya değerleri var.

## <a name="solution"></a>Çözüm

### <a name="process-overview"></a>İşleme genel bakış:

1. Bir onarım VM 'si oluşturun ve erişin.
1. Hive bozulma olup olmadığını denetleyin.
1. Seri konsolu ve bellek dökümü toplamayı etkinleştirin.
1. VM 'yi yeniden derleyin.

> [!NOTE]
> Bu hatayla karşılaşdığınızda, Konuk işletim sistemi (OS) işlemsel değildir. Bu sorunu çözmek için çevrimdışı modda sorun gidermeye başlayacaksınız.

### <a name="create-and-access-a-repair-vm"></a>Bir onarım VM 'si oluşturma ve erişme

1. Bir onarım VM 'si hazırlamak için [VM onarım komutlarının](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands) 1-3 adımlarını kullanın.
1. Hive bozulma olup olmadığını denetleyin.
1. Onarım sanal makinesine bağlanmak için Uzak Masaüstü Bağlantısı kullanın.
1. Klasörü kopyalayın `\windows\system32\config` ve iyi disk bölümünüze ya da başka bir güvenli konuma kaydedin. Kritik kayıt defteri dosyalarını düzenleyebileceğinizden bu klasörü bir önlem olarak yedekleyin.

> [!NOTE]
> `\windows\system32\config`Kayıt defterinde yaptığınız değişiklikleri geri almanız gerektiğinde klasörün bir kopyasını yedekleme olarak oluşturun.

### <a name="check-for-hive-corruption"></a>Hive bozulmasını denetle

Aşağıdaki yönergeler, nedenin Hive bozulması olup olmadığını veya Hive doğru şekilde kapatılmadığını belirlemenize yardımcı olur. Hive doğru şekilde kapanmamışsa, dosyanın kilidini açabilir ve VM 'nizi düzeltireceksiniz.

1. Onarım sanal makinenizde, **kayıt defteri Düzenleyicisi** uygulamasını açın. Bulmak için Windows Search çubuğuna "REGEDIT" yazın.
1. Kayıt Defteri Düzenleyicisi 'nde, vurgulamak için **HKEY_LOCAL_MACHINE** seçin ve ardından **Dosya > Hive yükle...** seçeneğini belirleyin. menüsünde.
1. Öğesine gidin `\windows\system32\config\SYSTEM` ve **Aç**' ı seçin.
1. Ad girmeniz istendiğinde, **brokensystem**girin.

   1. Hive açılamazsa veya boşsa, Hive bozulur. Hive bozuksa [bir destek bileti açın](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

     ![Kayıt Defteri Düzenleyicisi 'nin Hive yükleyemediğini belirten bir hata oluşur.](./media/windows-stop-error-bad-system-config-info/2.png)

   1. Hive normal şekilde açılırsa Hive düzgün şekilde kapatılmadı. 5. adımdan devam edin.

1. Düzgün kapatılmamış bir Hive 'yi onarmak için, **Brokensistemi** vurgulayın ve **Hive > dosyayı kaldır ' ı seçin...** dosyasını açın.

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
