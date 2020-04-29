---
title: Windows sanal makinesi Windows Önyükleme Yöneticisi nedeniyle önyükleme yapılamıyor
description: Bu makalede, Windows Önyükleme Yöneticisi 'nin bir Azure sanal makinesinin önyüklemesini önlediği sorunları gidermeye yönelik adımlar sağlanmaktadır.
services: virtual-machines-windows
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: a97393c3-351d-4324-867d-9329e31b3598
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 03/26/2020
ms.author: v-mibufo
ms.openlocfilehash: 5d2fb62870e2c41af635627f5d692f08c67f8394
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80373355"
---
# <a name="windows-vm-cannot-boot-due-to-windows-boot-manager"></a>Windows Önyükleme Yöneticisi nedeniyle Windows sanal makinesi önyükleme yapılamıyor

Bu makalede, Windows Önyükleme Yöneticisi 'nin bir Azure sanal makinesinin (VM) önyüklemesini önleyen sorunları gidermeye yönelik adımlar sağlanmaktadır.

## <a name="symptom"></a>Belirti

VM, bir Kullanıcı isteminde bekleyen bir şekilde takılırsa ve el ile talimat verilmedikçe önyükleme yapmaz.

VM 'nin ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) kullandığınızda, ekran görüntüsünde başlatılacak bir Işletim sistemi seçin Iletisi Ile Windows Önyükleme Yöneticisi görüntülenir *veya bir araç seçmek için Tab tuşuna basın:*.

Şekil 1
 
![Windows Önyükleme Yöneticisi "başlatılacak bir işletim sistemi seçin veya bir araç seçmek için TAB tuşuna basın:"](media/troubleshoot-guide-windows-boot-manager-menu/1.jpg)

## <a name="cause"></a>Nedeni

Hata, Windows Önyükleme Yöneticisi 'ndeki bir BCD bayrağının *displaybootmenu* olmasından kaynaklanır. Bayrak etkinleştirildiğinde, Windows önyükleme yöneticisi kullanıcıya önyükleme işlemi sırasında, hangi yükleyiciyi çalıştırmak istediğinizi seçerek önyükleme gecikmesine neden olur. Azure 'da, bu özellik bir VM 'yi önyüklemek için gereken süreye eklenebilir.

## <a name="solution"></a>Çözüm

İşleme genel bakış:

1. Seri konsol kullanarak daha hızlı önyükleme süresi için yapılandırın.
2. Bir onarım VM 'si oluşturun ve erişin.
3. Bir onarım VM 'sinde daha hızlı önyükleme süresi için yapılandırma.
4. **Önerilir**: VM 'yi yeniden oluşturmadan önce, seri konsolu ve bellek dökümü toplamayı etkinleştirin.
5. VM 'yi yeniden derleyin.

### <a name="configure-for-faster-boot-time-using-serial-console"></a>Seri konsol kullanarak daha hızlı önyükleme süresi için yapılandırma

Seri konsoluna erişiminiz varsa, daha hızlı önyükleme süreleri elde etmenin iki yolu vardır. *Displaybootmenu* bekleme süresini azaltın ya da bayrağı tamamen kaldırın.

1. Metin tabanlı konsola erişim kazanmak için [Windows Için Azure seri konsoluna](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-windows) erişmek için yönergeleri izleyin.

   > [!NOTE]
   > Seri konsoluna erişemiyorsanız, [bir onarım VM 'Si oluşturma ve erişme](#create-and-access-a-repair-vm)konusuna atlayın.

2. **Seçenek A**: bekleme süresini azaltma

   a. Bekleme süresi varsayılan olarak 30 saniye olarak ayarlanır ancak daha hızlı bir zamana (ör. 5 saniye) değiştirilebilir.

   b. Zaman aşımı değerini ayarlamak için seri konsolunda aşağıdaki komutu kullanın:

      `bcdedit /set {bootmgr} timeout 5`

3. **Seçenek B**: BCD bayrağını kaldırma

   a. Ön yükleme menüsünü görüntülemeyi tamamen engellemek için aşağıdaki komutu girin:

      `bcdedit /deletevalue {bootmgr} displaybootmenu`

      > [!NOTE]
      > Yukarıdaki adımlarda daha hızlı bir önyükleme süresi yapılandırmak için seri konsol kullandıysanız, bunun yerine aşağıdaki adımlara devam edebilirsiniz. Bu sorunu çözmek için artık çevrimdışı modda sorun gidermeye hazırsınız.

### <a name="create-and-access-a-repair-vm"></a>Bir onarım VM 'si oluşturma ve erişme

1. Bir onarım VM 'si hazırlamak için [VM onarım komutlarının 1-3 adımlarını](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands) kullanın.
2. Onarım VM 'sine bağlanmak Uzak Masaüstü Bağlantısı kullanın.

### <a name="configure-for-faster-boot-time-on-a-repair-vm"></a>Bir onarım VM 'sinde daha hızlı önyükleme süresi için yapılandırma

1. Yükseltilmiş bir komut istemi açın.
2. DisplayBootMenu ' i etkinleştirmek için aşağıdakileri girin:

   **1. nesil VM 'ler**için bu komutu kullanın:

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /set {bootmgr} displaybootmenu yes`

   **2. nesil VM 'ler**için bu komutu kullanın:

   `bcdedit /store <VOLUME LETTER OF EFI SYSTEM PARTITION>:EFI\Microsoft\boot\bcd /set {bootmgr} displaybootmenu yes`

   "< metin >" gibi, sembolden büyük veya daha küçük simgeleri ve içindeki metni değiştirin.

3. Zaman aşımı değerini 5 saniye olarak değiştirin:

   **1. nesil VM 'ler**için bu komutu kullanın:

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /set {bootmgr} timeout 5`

   **2. nesil VM 'ler**için bu komutu kullanın:

   `bcdedit /store <VOLUME LETTER OF EFI SYSTEM PARTITION>:EFI\Microsoft\boot\bcd /set {bootmgr} timeout 5`

   "< metin >" gibi, sembolden büyük veya daha küçük simgeleri ve içindeki metni değiştirin.

### <a name="recommended-before-you-rebuild-the-vm-enable-serial-console-and-memory-dump-collection"></a>Önerilir: VM 'yi yeniden oluşturmadan önce, seri konsolu ve bellek dökümü toplamayı etkinleştirin

Bellek dökümü toplamayı ve seri konsolunu etkinleştirmek için aşağıdaki betiği çalıştırın:

1. Yükseltilmiş bir komut istemi oturumu açın (yönetici olarak çalıştır).
2. Aşağıdaki komutları çalıştırın:

   Seri konsolunu etkinleştir

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON`

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

   "< metin >" gibi, sembolden büyük veya daha küçük simgeleri ve içindeki metni değiştirin.

3. İşletim sistemi diskindeki boş alanın, VM 'deki bellek boyutu (RAM) kadar olduğundan emin olun.

   İşletim sistemi diskinde yeterli alan yoksa, bellek dökümü dosyasının oluşturulacağı konumu değiştirmeniz ve yeterli boş alana sahip olan VM 'ye bağlı tüm veri diskine başvurmanız gerekir. Konumu değiştirmek için, "% SystemRoot%" değerini aşağıdaki komutlarda bulunan veri diskinin sürücü harfiyle (örneğin, "F:") değiştirin.

#### <a name="suggested-configuration-to-enable-os-dump"></a>İşletim sistemi dökümünü etkinleştirmek için önerilen yapılandırma

**Bozuk işletim sistemi diski yükle**:

`REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM`

**ControlSet001 üzerinde etkinleştir:**

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

**ControlSet002 üzerinde etkinleştir:**

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

**Bozuk işletim sistemi diskini kaldır:**

`REG UNLOAD HKLM\BROKENSYSTEM`

### <a name="rebuild-the-original-vm"></a>Özgün VM 'yi yeniden oluşturma

VM 'yi yeniden birleştirmek için [VM onarım komutlarının 5. adımını](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) kullanın.