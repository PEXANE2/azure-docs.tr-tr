---
title: Windows durdurma hatası-
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
ms.openlocfilehash: c04f3b27c7214dcf821c7698796bfaea399b947d
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86509112"
---
# <a name="windows-stop-error---0x000000ef-critical-process-died"></a>Windows durma hatası-#0x000000EF "kritik Işlem died"

Bu makalede, bir Azure VM 'de önyükleme sırasında kritik bir işlemin bulunduğu sorunları çözmek için adımlar sağlanmaktadır.

## <a name="symptom"></a>Belirti

VM 'nin ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](./boot-diagnostics.md) kullandığınızda, ekran görüntüsünde Ileti *kritik işlemiyle*ilgili hata *#0x000000EF* görüntülediğini görürsünüz.

!["Bilgisayarınız bir sorunla karşılaştı ve yeniden başlatılması gerekiyor. Yalnızca bazı hata bilgilerini topluyoruz ve sonra yeniden başlatabilirsiniz. (% #% Tamam) Daha fazla bilgi edinmek istiyorsanız, daha sonra çevrimiçi olarak bu hata için arama yapabilirsiniz: 0x000000EF "](media/troubleshoot-guide-critical-process-died/1.jpg)

## <a name="cause"></a>Nedeni

Genellikle bunun nedeni, önyükleme sırasında kritik bir sistem işlemi başarısız olur. Kritik işlem sorunları hakkında daha fazla bilgiyi "[hata denetimi 0xEF: CRITICAL_PROCESS_DIED](/windows-hardware/drivers/debugger/bug-check-0xef--critical-process-died)" adresinde bulabilirsiniz.

## <a name="solution"></a>Çözüm

### <a name="process-overview"></a>İşleme genel bakış:

1. Bir onarım VM 'si oluşturun ve erişin.
2. Tüm işletim sistemi bozulmasını düzeltin.
3. **Önerilir**: VM 'yi yeniden oluşturmadan önce, seri konsolu ve bellek dökümü toplamayı etkinleştirin.
4. VM 'yi yeniden derleyin.

> [!NOTE]
> Bu önyükleme hatasıyla karşılaşıldığında, Konuk işletim sistemi çalışır durumda değildir. Bu sorunu çözmek için çevrimdışı modda sorun gidermeye olursunuz.

### <a name="create-and-access-a-repair-vm"></a>Bir onarım VM 'si oluşturma ve erişme

1. Bir onarım VM 'si hazırlamak için [VM onarım komutlarının 1-3 adımlarını](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) kullanın.
2. Uzak Masaüstü Bağlantısı kullanarak, onarım VM 'ye bağlanın.

### <a name="fix-any-os-corruption"></a>Tüm işletim sistemi bozulmasını düzeltin

1. Yükseltilmiş bir komut istemi açın.
2. Aşağıdaki sistem dosyası denetleyicisi (SFC) komutunu çalıştırın:

   `sfc /scannow /offbootdir=<BOOT DISK DRIVE>:\ /offwindir=<BROKEN DISK DRIVE>:\windows`

   * < önyükleme DISK sürücüsü >, onarım sanal makinesinin (genellikle "C:") önyükleme birimidir ve bozuk DISK sürücüsüne < >, bozuk VM 'den eklenen diskin sürücü harfi olacaktır. Büyük/küçüktür sembollerinin yanı sıra bunlar içindeki metni, örneğin "< metin >" yazın ve uygun harfle değiştirin.

3. Sonra, VM 'yi yeniden birleştirmek ve önyükleme olup olmadığını görmek için [VM onarım komutlarının 5. adımını](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) kullanın.
4. VM hala önaçılmadığından, bellek dökümü dosyasını toplamaya devam edin.

### <a name="collect-the-memory-dump-file"></a>Bellek dökümü dosyasını toplayın

SFC çalıştırıldıktan sonra sorun devam ederse, sorunun nedenini öğrenmek için bir bellek dökümü dosyası analizi gerekecektir. Bellek dökümü dosyasını toplamak için aşağıdaki adımları izleyin:

### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>İşletim sistemi diskini yeni bir onarım VM 'sine iliştirme

1. Yeni bir onarım VM 'si hazırlamak için [VM onarım komutlarının 1-3 adımlarını](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) kullanın.
2. Uzak Masaüstü Bağlantısı kullanarak, onarım VM 'ye bağlanın.

### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Döküm dosyasını bulma ve destek bileti gönderme

3. VM 'yi Onar sayfasında, bağlı işletim sistemi diskinde Windows klasörü ' ne gidin. Bağlı işletim sistemi diskine atanan sürücü harfi *F*ise, *f:\Windows*'a gitmeniz gerekir.
4. *Memory. dmp* dosyasını bulun ve ardından bellek dökümü dosyası ile [bir destek bileti gönderebilirsiniz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

   > [!NOTE]
   > Döküm dosyasını bulamıyorsanız, bellek dökümü toplamayı ve seri konsolunu etkinleştirmek için aşağıdaki adımları tamamlayarak bu bölüme dönün ve bellek dökümü dosyasını toplamak için yukarıdaki görevdeki adımları tekrarlayın.

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

VM 'yi yeniden birleştirmek için [VM onarım komutlarının 5. adımını](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) kullanın.
