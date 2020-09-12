---
title: Denetim ilkesi yapılandırma ilkesi uygulanırken sanal makine yanıt vermiyor
description: Bu makalede, bir Azure VM 'nin önyüklenmesini önleyen denetim Ilkesi yapılandırma ilkesi uygulanırken sanal makinenin (VM) yanıt vermediği sorunları gidermek için gereken adımlar sağlanmaktadır.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 3f6383b5-81fa-49ea-9434-2fe475e4cbef
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 08/24/2020
ms.author: v-miegge
ms.openlocfilehash: bc41783bf977806b5f9bba5b953f1f581ad07f18
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89299533"
---
# <a name="virtual-machine-is-unresponsive-while-applying-audit-policy-configuration-policy"></a>Denetim ilkesi yapılandırma ilkesi uygulanırken sanal makine yanıt vermiyor

Bu makalede, bir Azure VM 'nin önyüklenmesini önleyen denetim Ilkesi yapılandırma ilkesi uygulanırken sanal makinenin (VM) yanıt vermediği sorunları gidermek için gereken adımlar sağlanmaktadır.

## <a name="symptom"></a>Belirti

VM 'nin ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) kullandığınızda, ekran görüntüsünde, **Denetim Ilkesi yapılandırma ilkesini uygulayan**ileti ile bir önyükleme SıRASıNDA işletim sisteminin (OS) yanıt vermeyen bir şekilde görüntülediğini görürsünüz.

  !["Denetim Ilkesi yapılandırma ilkesi uygulanıyor" iletisiyle işletim sistemi önyüklemesi](./media/vm-unresponsive-applying-audit-configuration-policy/1.png)

  ![Windows Server 2012 ' de işletim sistemi, "Denetim Ilkesi yapılandırma ilkesi uygulanıyor" iletisiyle önyükleniyor](./media/vm-unresponsive-applying-audit-configuration-policy/2.png)

## <a name="cause"></a>Nedeni

İlke eski Kullanıcı profillerini temizlemeye çalıştığında çakışan kilitler vardır.

> [!NOTE]
> Bu yalnızca Windows Server 2012 ve Windows Server 2012 R2 için geçerlidir.

Sorunlu ilke şu şekildedir: *Bilgisayar Yapılandırması \ Yönetim Şablonları \ sistem/Kullanıcı profiller sistem yeniden başlatıldığında belirtilen gün sayısından daha eski Kullanıcı profillerini siler.*

## <a name="solution"></a>Çözüm

### <a name="process-overview"></a>İşleme genel bakış

1. Bir onarım VM 'si oluşturun ve erişin.
1. İlkeyi devre dışı bırakın.
1. Seri konsolu ve bellek dökümü toplamayı etkinleştirin.
1. VM 'yi yeniden derleyin.
1. Bellek dökümü dosyasını toplayın ve bir destek bileti gönderir.

### <a name="create-and-access-a-repair-vm"></a>Bir onarım VM 'si oluşturma ve erişme

1. Bir onarım VM 'si hazırlamak için [VM onarım komutlarının](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands) 1-3 adımlarını kullanın.
1. Uzak Masaüstü Bağlantısı kullanarak, onarım sanal makinesine bağlanın.

### <a name="disable-the-policy"></a>İlkeyi devre dışı bırak

1. VM 'yi Onar sayfasında, **kayıt defteri düzenleyicisini**açın.
1. **HKEY_LOCAL_MACHINE** anahtarı bulun ve menüden **Hive yükle > dosya** ' yı seçin.

   ![Hive yüklemek için kayıt defteri Düzenleyicisi içinde gezinme.](./media/vm-unresponsive-applying-audit-configuration-policy/3.png)

   - Çevrimdışı bir sistemden kayıt defteri anahtarlarını yüklemek için Hive Yükle ' ye yararlanabilirsiniz. Bu durumda, sistem, onarım sanal makinesine bağlı bozuk disktir.
   - Sistem genelinde ayarlar **HKEY_LOCAL_MACHINE** depolanır ve **HKLM**olarak kısaltılabilir.

1. Ekli diskte `\windows\system32\config\SOFTWARE` dosyasını açın.

   - Bir ad girmeniz istendiğinde, **Brokensoftware**' i girin.
   - **Brokensoftware** 'in yüklendiğini doğrulamak için **HKEY_LOCAL_MACHINE** genişletin ve eklenen **bromi yazılım** anahtarını arayın.

1. **Brokensoftware** ' e gidin ve yüklenen Hive Içinde **cleanupprofiles** anahtarının var olup olmadığını denetleyin.

   - Anahtar varsa, **Cleanupprofiles** ilkesi ayarlanır. Değeri, gün cinsinden ölçülen bekletme ilkesini temsil eder.
   - Anahtar yoksa, **Cleanupprofiles** ilkesi ayarlı değildir. Bu durumda, [bir bellek dökümü dosyası ile bir destek bileti göndermek](#collect-the-memory-dump-file-and-submit-a-support-ticket)için ileratlayın.

1. **Cleanupprofiles** anahtarını şu komutu kullanarak silin:

   `reg delete "HKLM\BROKENSOFTWARE\Policies\Microsoft\Windows\System" /v CleanupProfiles /f`

1. Şu komutu kullanarak **Brokensoftware** kovanını kaldırın:

   `reg unload HKLM\BROKENSOFTWARE`

### <a name="enable-the-serial-console-and-memory-dump-collection"></a>Seri konsol ve bellek dökümü toplamayı etkinleştirme

**Önerilir**: VM 'yi yeniden oluşturmadan önce, aşağıdaki betiği çalıştırarak seri konsol ve bellek dökümü toplamasını etkinleştirin:

1. Yükseltilmiş bir komut istemi oturumunu yönetici olarak açın.
1. BCD deposu verilerini listeleyin ve sonraki adımda kullanacağınız önyükleme Yükleyici tanımlayıcısını saptayın.

   1. 1. nesil bir VM için aşağıdaki komutu girin ve listelenen tanımlayıcıyı aklınızda yapın:

      `bcdedit /store <BOOT PARTITON>:\boot\bcd /enum`

      - Komutunda, öğesini `<BOOT PARTITON>` önyükleme klasörünü içeren, eklenen diskteki bölümünün harfiyle değiştirin.

        ![Şekil 4 ' te, BCD deposunun listesinin Windows önyükleme yükleyicisi 'nin kimlik numarası altında listelendiği 1. nesil bir VM 'de listelenmesi gösterilmektedir.](./media/vm-unresponsive-applying-audit-configuration-policy/4.png)

   1. 2. nesil bir VM için aşağıdaki komutu girin ve listelenen tanımlayıcıyı aklınızda yapın:

      `bcdedit /store <LETTER OF THE EFI SYSTEM PARTITION>:EFI\Microsoft\boot\bcd /enum`

      - Komutunda, `<LETTER OF THE EFI SYSTEM PARTITION>` EFI sistem bölümünün harfiyle değiştirin.
      - **EFI sistem bölümü**olarak etiketlenmiş uygun sistem bölümünü belirlemek Için Disk Yönetimi konsolunu başlatmak faydalı olabilir.
      - Tanımlayıcı, benzersiz bir GUID olabilir veya varsayılan **Bootmgr**olabilir.

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
   
### <a name="rebuild-the-virtual-machine"></a>Sanal makineyi yeniden derle

1. VM 'yi yeniden derlemek için [VM onarım komutlarının 5. adımını](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) kullanın.

1. Sorunun sorunu düzeltti olup olmadığını görmek için VM 'niz normal şekilde önyükleniyorsa test edin.

   - Sorun düzeltilmemişse, [bir döküm dosyası toplamaya ve bir destek bileti göndermeye](#collect-the-memory-dump-file-and-submit-a-support-ticket)devam edin.
   - Sorun giderilirse, başka bir adım gerekmez.

Sorun giderilmediyse, ilke artık yerel olarak devre dışıdır. Kalıcı bir çözüm için, Kullanıcı profillerini otomatik olarak silecektir, VM 'lerde CleanupProfiles ilkesini kullanmayın. Zamanlanmış bir görev veya betik gibi profil temiztaları gerçekleştirmek için farklı bir yöntem kullanın.

**Bu ilkeyi kullanmayın:** 
 *Yönetim Şablonları \ yönetici yöneticiler Kullanıcı profiller sistem yeniden başlatıldığında belirtilen gün sayısından daha eski Kullanıcı profillerini siler.*

### <a name="the-issue-should-now-be-fixed"></a>Sorun şimdi düzeltilmelidir

Normal gibi çalıştığından emin olmak için VM 'nizi test edin. Hala sorun yaşıyorsanız, daha fazla yardım almak için sonraki bölüme devam edebilirsiniz.

### <a name="collect-the-memory-dump-file-and-submit-a-support-ticket"></a>Bellek dökümü dosyasını toplayın ve bir destek bileti gönderir

Bu sorunu çözmek için öncelikle kilitlenme için bellek dökümü dosyasını toplamanız ve ardından bellek dökümü dosyası ile desteğe başvurmanız gerekir. Döküm dosyasını toplamak için aşağıdaki adımları izleyin:

#### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>İşletim sistemi diskini yeni bir onarım VM 'sine iliştirme

1. Yeni bir onarım VM 'si hazırlamak için [VM onarım komutlarının](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands) 1-3 adımlarını kullanın.
1. Uzak Masaüstü Bağlantısı kullanarak, onarım VM 'ye bağlanın.

#### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Döküm dosyasını bulma ve destek bileti gönderme

1. VM 'yi Onar sayfasında, bağlı işletim sistemi diskinde Windows klasörü ' ne gidin. Bağlı işletim sistemi diskine atanan sürücü harfi *F*olarak etiketlenmişse, adresine gitmeniz gerekir `F:\Windows` .
1. Dosyayı bulun `memory.dmp` ve ardından bellek dökümü dosyası ile [bir destek bileti gönderebilirsiniz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .
1. Dosyayı bulmada sorun yaşıyorsanız `memory.dmp` , bunun yerine, [seri konsolundaki maskelenemeyen kesme (NMI) çağrılarını](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-windows#use-the-serial-console-for-nmi-calls) kullanın. [Burada NMI çağrılarını kullanarak kilitlenme bilgi döküm dosyası oluşturma](https://docs.microsoft.com/windows/client-management/generate-kernel-or-complete-crash-dump)kılavuzunu izleyin.
