---
title: Windows sanal makine windows önyükleme yöneticisi nedeniyle önyükleme yapamıyor
description: Bu makalede, Windows Boot Manager bir Azure Sanal Makine önyükleme engeller sorunları gidermek için adımlar sağlar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80373355"
---
# <a name="windows-vm-cannot-boot-due-to-windows-boot-manager"></a>Windows VM, Windows Boot Manager nedeniyle önyükleme yapamıyor

Bu makalede, Windows Boot Manager bir Azure Sanal Makine (VM) önyükleme engeller sorunları gidermek için adımlar sağlar.

## <a name="symptom"></a>Belirti

VM bir kullanıcı istemi üzerinde bekleyen sıkışmış ve el ile talimat sürece önyükleme yapmaz.

VM ekran görüntüsünü görüntülemek için [Önyükleme tanılama](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) kullandığınızda, ekran görüntüsünün Windows Boot Manager'ı mesajla görüntülediğini göreceksiniz *Başlatılabilmek için bir işletim sistemi seçin veya bir araç seçmek için TAB tuşuna basın:*.

Şekil 1
 
![Windows Boot Manager belirten "Başlatmak için bir işletim sistemi seçin veya bir araç seçmek için TAB tuşuna basın:"](media/troubleshoot-guide-windows-boot-manager-menu/1.jpg)

## <a name="cause"></a>Nedeni

Hata, Windows Boot Manager'daki BCD bayrak *görüntüleme menüsünden* kaynaklanır. Bayrak etkinleştirildiğinde, Windows Boot Manager kullanıcıdan önyükleme işlemi sırasında hangi yükleyiciyi çalıştırmak istediklerini seçmesini ister ve önyükleme gecikmesine neden olur. Azure'da bu özellik, bir VM'yi önyüklemek için gereken süreyi ekleyebilir.

## <a name="solution"></a>Çözüm

İşleme Genel Bakış:

1. Seri Konsol'u kullanarak Daha Hızlı Önyükleme Süresi için yapılandırın.
2. Onarım VM'si oluşturma ve erişme.
3. Onarım VM'de Daha Hızlı Önyükleme Süresi için yapılandırın.
4. **Önerilen**: VM'yi yeniden oluşturmadan önce seri konsol ve bellek dökümü koleksiyonunu etkinleştirin.
5. VM'yi yeniden inşa edin.

### <a name="configure-for-faster-boot-time-using-serial-console"></a>Seri Konsol'u kullanarak Daha Hızlı Önyükleme Süresi için yapılandırın

Seri konsola erişiminiz varsa, daha hızlı önyükleme süreleri elde etmenin iki yolu vardır. *Displaybootmenu* bekleme süresini kısaltın veya bayrağı tamamen kaldırın.

1. Metin tabanlı konsola erişmek [için Windows için Azure Seri Konsolu'na](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-windows) erişmek için yönergeleri izleyin.

   > [!NOTE]
   > Seri konsoluna erişemiyorsanız, Onarım [VM Oluştur ve Eriş'e](#create-and-access-a-repair-vm)geçmek için ileri ye atlayın.

2. **Seçenek A:** Bekleme Süresini Azaltın

   a. Bekleme süresi varsayılan olarak 30 saniye olarak ayarlanır, ancak daha hızlı bir zamana (örn. 5 saniye) değiştirilebilir.

   b. Zaman ayarı değerini ayarlamak için seri konsolda aşağıdaki komutu kullanın:

      `bcdedit /set {bootmgr} timeout 5`

3. **Seçenek B**: BCD Bayrağını Kaldır

   a. Önyükleme Menüsü'nü tamamen engellemek için aşağıdaki komutu girin:

      `bcdedit /deletevalue {bootmgr} displaybootmenu`

      > [!NOTE]
      > Yukarıdaki adımlarda daha hızlı bir önyükleme süresini yapılandırmak için seri konsolu kullanamadıysanız, bunun yerine aşağıdaki adımlarla devam edebilirsiniz. Artık bu sorunu gidermek için çevrimdışı modda sorun giderme olacak.

### <a name="create-and-access-a-repair-vm"></a>Onarım VM Oluşturma ve Eriş

1. Onarım VM'i hazırlamak için [VM Onarım Komutları'nın 1-3 adımlarını](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands) kullanın.
2. Onarım VM'ye bağlanmak için Uzak Masaüstü Bağlantısını kullanın.

### <a name="configure-for-faster-boot-time-on-a-repair-vm"></a>Onarım VM'de Daha Hızlı Önyükleme Süresi için yapılandır

1. Yükseltilmiş bir komut istemini açın.
2. DisplayBootMenu'yi etkinleştirmek için aşağıdakileri girin:

   Nesil 1 **VM'ler**için bu komutu kullanın:

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /set {bootmgr} displaybootmenu yes`

   Nesil 2 **VM'ler**için bu komutu kullanın:

   `bcdedit /store <VOLUME LETTER OF EFI SYSTEM PARTITION>:EFI\Microsoft\boot\bcd /set {bootmgr} displaybootmenu yes`

   "< metin >" gibi sembollerin içindeki metni ve sembollerden daha büyük veya daha az ını değiştirin.

3. Zaman ekme değerini 5 saniyeye değiştirin:

   Nesil 1 **VM'ler**için bu komutu kullanın:

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /set {bootmgr} timeout 5`

   Nesil 2 **VM'ler**için bu komutu kullanın:

   `bcdedit /store <VOLUME LETTER OF EFI SYSTEM PARTITION>:EFI\Microsoft\boot\bcd /set {bootmgr} timeout 5`

   "< metin >" gibi sembollerin içindeki metni ve sembollerden daha büyük veya daha az ını değiştirin.

### <a name="recommended-before-you-rebuild-the-vm-enable-serial-console-and-memory-dump-collection"></a>Önerilen: VM'yi yeniden oluşturmadan önce seri konsol ve bellek dökümü koleksiyonunu etkinleştirin

Bellek dökümü toplama ve Seri Konsol'u etkinleştirmek için aşağıdaki komut dosyasını çalıştırın:

1. Yükseltilmiş bir komut istemi oturumu açın (Yönetici olarak çalıştırın).
2. Aşağıdaki komutları çalıştırın:

   Seri Konsolu Etkinleştir

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON`

   `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

   "< metin >" gibi sembollerin içindeki metni ve sembollerden daha büyük veya daha az ını değiştirin.

3. Os diskindeki boş alanın VM'deki bellek boyutu (RAM) kadar olduğunu doğrulayın.

   Os diskinde yeterli alan yoksa, bellek dökümü dosyasının oluşturulacağı konumu değiştirmeli ve bunu Yeterli boş alana sahip VM'ye bağlı herhangi bir veri diskine yönlendirmelisiniz. Konumu değiştirmek için, aşağıdaki komutlarda veri diskinin sürücü harfiyle "%SystemRoot%" (örneğin, "F:") değiştirin.

#### <a name="suggested-configuration-to-enable-os-dump"></a>İşletim Sistemi Döküm'ü etkinleştirmek için önerilen yapılandırma

**Yük Kırık İşletim Sistemi Diski**:

`REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM`

**ControlSet001'de etkinleştirin:**

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

**ControlSet002'de etkinleştirin:**

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

**Bozuk Işletim Sistemi Diskini Boşaltın:**

`REG UNLOAD HKLM\BROKENSYSTEM`

### <a name="rebuild-the-original-vm"></a>Orijinal VM'yi yeniden oluşturma

VM'yi yeniden birleştirmek için [VM Onarım Komutlarının adım](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) 5'ini kullanın.