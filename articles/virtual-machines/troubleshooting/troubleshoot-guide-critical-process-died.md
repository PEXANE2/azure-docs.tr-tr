---
title: Windows stop hatası -
description: ''
services: virtual-machines-windows
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 5b3ed56a-5a11-4ff9-9ee8-76aea4a5689b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 03/26/2020
ms.author: v-mibufo
ms.openlocfilehash: 9e4c4b9c809a626c71b4a7e9235d917b442be160
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80373368"
---
# <a name="windows-stop-error---0x000000ef-critical-process-died"></a>Windows Stop Hatası - #0x000000EF "Kritik İşlem Öldü"

Bu makalede, bir Azure VM önyükleme sırasında kritik bir işlemin öldüğü sorunları gidermek için adımlar sağlanmaktadır.

## <a name="symptom"></a>Belirti

VM ekran görüntüsünü görüntülemek için [Önyükleme tanılama](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) kullandığınızda, ekran görüntüsü kritik *işlem öldü*mesajı ile *#0x000000EF* hata görüntüler göreceksiniz.

!["Bilgisayarınız bir sorunla karşılaştı ve yeniden başlatılması gerekiyor. Sadece bazı hata bilgileri topluyoruz, ve sonra yeniden başlatabilirsiniz. (##% tamamlandı) Daha fazla bilgi almak isterseniz, daha sonra bu hata için çevrimiçi arama yapabilirsiniz: 0x000000EF"](media/troubleshoot-guide-critical-process-died/1.jpg)

## <a name="cause"></a>Nedeni

Genellikle, bu önyükleme sırasında başarısız kritik bir sistem işlemi kaynaklanmaktadır. Kritik işlem konuları hakkında daha fazla bilgi için "[Hata Denetimi 0xEF: CRITICAL_PROCESS_DIED](https://docs.microsoft.com/windows-hardware/drivers/debugger/bug-check-0xef--critical-process-died)" adresinden edinebilirsiniz.

## <a name="solution"></a>Çözüm

### <a name="process-overview"></a>İşleme Genel Bakış:

1. Onarım VM'si oluşturma ve erişme.
2. Herhangi bir işletim sistemi bozulmasını düzeltin.
3. **Önerilen**: VM'yi yeniden oluşturmadan önce seri konsol ve bellek dökümü koleksiyonunu etkinleştirin.
4. VM'yi yeniden inşa edin.

> [!NOTE]
> Bu önyükleme hatasıyla karşılaştığında, Konuk Işletim Sistemi çalışmaz. Bu sorunu gidermek için Çevrimdışı modda sorun giderme olacaktır.

### <a name="create-and-access-a-repair-vm"></a>Onarım VM Oluşturma ve Eriş

1. Onarım VM'i hazırlamak için [VM Onarım Komutları'nın 1-3 adımlarını](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands) kullanın.
2. Uzak Masaüstü Bağlantısını Kullanarak Onarım VM'sine bağlanın.

### <a name="fix-any-os-corruption"></a>Herhangi bir işletim sistemi bozulmasını düzeltme

1. Yükseltilmiş bir komut istemini açın.
2. Aşağıdaki Sistem Dosya Denetleyicisi (SFC) komutunu çalıştırın:

   `sfc /scannow /offbootdir=<BOOT DISK DRIVE>:\ /offwindir=<BROKEN DISK DRIVE>:\windows`

   * < BOOT DISK SÜRÜCÜ > Onarım VM'nin önyükleme hacmi (genellikle "C:") ve > < kırık VM'den eklenen diskin sürücü harfi olacaktır. Sembollerden daha büyük/ daha az olan Metni ve bunların içinde yer alan metni (örneğin burada > < metin" gibi uygun harfle değiştirin.

3. Ardından, VM'yi yeniden birleştirmek ve önyükleme yapıp yapmayaca bilmek için [VM Onarım Komutları'nın 5.](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example)
4. VM hala önyükleme değilse, bellek dökümü dosyasını toplamaya devam edin.

### <a name="collect-the-memory-dump-file"></a>Bellek Döküm Dosyasını Topla

Sorun SFC çalıştırdıktan sonra devam ederse, sorunun nedenini belirlemek için bir bellek dökümü dosyasının çözümlemesi gerekir. Bellek dökümü dosyasını toplamak için aşağıdaki adımları izleyin:

### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>Os diskini yeni bir Onarım VM'ine takın

1. Yeni bir Onarım VM hazırlamak için [VM Onarım Komutlarının 1-3 adımlarını](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands) kullanın.
2. Uzak Masaüstü Bağlantısını Kullanarak Onarım VM'sine bağlanın.

### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Döküm dosyasını bulun ve destek bileti gönderin

3. Onarım VM'sinde, bağlı işletim sistemi diskindeki windows klasörüne gidin. Bağlı işletim sistemi diskine atanan sürücü harfi *F*ise, *F:\Windows'a*gitmeniz gerekir.
4. *Memory.dmp* dosyasını bulun ve ardından bellek dökümü dosyasıyla [birlikte bir destek bileti gönderin.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)

   > [!NOTE]
   > Döküm dosyasını bulamıyorsanız, bellek dökümü toplamayı ve Seri Konsolu etkinleştirmek için aşağıdaki adımları tamamlayın, ardından bu bölüme dönün ve bellek dökümü dosyasını toplamak için yukarıdaki görevdeki adımları yineleyin.

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
