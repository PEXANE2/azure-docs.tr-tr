---
title: Windows durma hatası - Durum Bellek Yok
description: Bu makalede, Windows 'un başlayaamamakta olduğu sorunları gidermeye yönelik adımlar ve "durum No Memory" durumu görüntülenir.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: d29c7e49-4578-43c8-b829-831da4e48932
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 06/26/2020
ms.author: v-mibufo
ms.openlocfilehash: 67064cf694445acf8472b958660133c2f2d31db9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85660986"
---
# <a name="windows-stop-error---status-no-memory"></a>Windows durma hatası - Durum Bellek Yok

Bu makalede, Windows 'un başlayaamadığı sorunları gidermeye yönelik adımlar sağlanmaktadır ve "durum No Memory" olarak da bilinen durum veya hata kodu #0xC0000017 görüntülenir.

## <a name="symptom"></a>Belirti

Sanal makinenin (VM) ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) kullandığınızda, ekran görüntüsünde hata kodu görüntülenir: `0xC0000017` . Çalıştırmakta olduğunuz Windows sürümüne bağlı olarak, bu kodun **Windows Önyükleme Yöneticisi** veya **Kurtarma ekranında**görüntülendiğini görebilirsiniz.

   **Windows Önyükleme Yöneticisi**

   ![Windows Önyükleme Yöneticisi "Windows 'un başlayamadığını belirten bir hata. Yeni bir donanım veya yazılım değişikliği nedeni "olabilir. Aşağı kaydırarak "0xC0000017" durum kodunu ve "gerekli bir dosya eksik veya hata içerdiğinden" uygulamanın veya işletim sisteminin yüklenemediğini belirten bilgileri görürsünüz.](./media/troubleshoot-windows-stop-error/1.png)

   **Kurtarma ekranı**
 
   !["PC/cihazınızın onarılması gereken" olduğunu belirten kurtarma ekranı. Ramdisk cihazı oluşturmak için yeterli bellek yok ". Ayrıca "0xC0000017" hata kodunu görmeniz gerekir.](./media/troubleshoot-windows-stop-error/2.png)

## <a name="cause"></a>Nedeni

İşletim sisteminin diski dolu, çok parçalanmış ya da işletim sistemi (OS) belleğe veya sayfa dosyasına veya her ikisine de erişemez.

## <a name="solution"></a>Çözüm

### <a name="process-overview"></a>İşleme genel bakış:

1. Bir onarım VM 'si oluşturma ve erişme
1. Diskte yer açın
1. BCD deposundan bozuk belleği temizleme
1. Sayfa dosyasını varsayılan konumuna geri yükleme
1. Seri konsolu ve bellek dökümü toplamayı etkinleştir
1. VM 'yi yeniden oluşturma

> [!NOTE]
> Bu hatayla karşılaşıldığında, Konuk işletim sistemi çalışır durumda değildir. Bu sorunu çözmek için çevrimdışı modda sorun gidermeye başlayacaksınız.

### <a name="create-and-access-a-repair-vm"></a>Bir onarım VM 'si oluşturma ve erişme

1. Bir onarım VM 'si hazırlamak için [VM onarım komutlarının 1-3 adımlarını](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands) kullanın.
1. Uzak Masaüstü Bağlantısı kullanarak, onarım VM 'ye bağlanın.

### <a name="for-generation-2-vms-assign-a-letter-to-the-extensible-firmware-interface-efi-partition"></a>2. nesil VM 'Ler için Genişletilebilir Bellenim Arabirimi (EFı) bölümüne bir harf atayın:

2. nesil bir VM kullanıyorsanız, eklenen diskin EFı bölümünün kendisine atanmış bir harfi olmayabilir. Bu sorun giderme kılavuzuna geçmeden önce bölüme bir harf atamak için aşağıdaki adımları izlemeniz gerekir.

1. Windows Search 'te `diskmgmt` **Disk Yönetimi konsolunu**girip açın.
1. Onarım sanal makinesine bağlı bozuk diski tanımla. Genellikle, bu disk konsolunda son olarak listelenir ve en yüksek sayısal değere sahiptir.
1. Bu diskte, kendisine atanmış bir harf değerine sahip olmayan **EFI sistem bölümünü**tutan bir bölüm (örneğin, *F:*) varsa unutmayın. Tüm bölümler atanırsa diskte boş alan kazanmak için ilerlemeye geçebilirsiniz. Aksi takdirde, bu diske bir harf atamaya devam edin.

   !["Disk 2" ve "EFı sistem bölümü" 100 olan atanmamış bölüm ile birlikte Disk Yönetimi konsolu.](./media/troubleshoot-windows-stop-error/3.png)

1. Yönetici olarak yükseltilmiş bir komut istemi açın ve `diskpart` **DISKPART** aracını başlatmak için girin.
1. Aşağıdaki komutları girin:

   ```
   list disk
   sel disk <NUMBER OF THE ATTACHED DISK>
   list partition
   sel partition <NUMBER OF THE SYSTEM (EFI) PARTION>
   assign
   ```
   
   - Komutunda, öğesini `<NUMBER OF THE ATTACHED DISK>` Adım 2 ' de tanımladığınız disk numarasıyla değiştirin.
   - Komutunda, öğesini `<NUMBER OF THE SYSTEM PARTION>` EFI sistem bölümünün bölüm numarasıyla değiştirin. Bu bölüme henüz bir harf atanmadı, ancak tür **sistemi** olmalıdır ve boyutu 100 MB olmalıdır.

   > [!NOTE]
   > Disk yönetimi konsolundaki bölümleri DISKPART aracında listelenenlere göre karşılaştırmak, eklenen diskteki EFı sistem bölümüne karşılık gelen bölüm numarasını belirlemede yardımcı olabilir.

1. Komut istemi penceresini kapatın.

### <a name="free-up-space-on-the-disk"></a>Diskte yer açın

Artık bozuk disk onarım sanal makinesine bağlı olduğuna göre, o diskteki işletim sisteminin düzgün şekilde çalışması için yeterli alanı olduğunu doğrulamanız gerekir. 

1. Eklenen diskin sürücüsüne sağ tıklayıp **Özellikler**' i seçerek diskin dolu olup olmadığını denetleyin.
1. Diskte **300 MB 'tan az boş alan**varsa, [PowerShell kullanarak en fazla 1 TB 'a genişletin](https://docs.microsoft.com/azure/virtual-machines/windows/expand-os-disk).
1. Disk boyutu **1 TB**olduğunda disk temizleme işlemi gerçekleştirmeniz gerekir. [Disk Temizleme aracını](https://support.microsoft.com/help/4026616/windows-10-disk-cleanup) kullanarak boş alan açabilirsiniz.
1. Yükseltilmiş bir komut istemi (yönetici olarak çalıştır) örneği açın ve sürücüde parçalama gerçekleştirin:

   ``
   defrag <LETTER ASSIGNED TO THE OS DISK>: /u /x /g
   ``
   
   - Parçalama düzeyine bağlı olarak, parçalama zaman alabilir.
   - Komutunda, `<LETTER ASSIGNED TO THE OS DISK>` işletim sistemi diskinin harfiyle değiştirin (örneğin, *F:*).

### <a name="clean-out-bad-memory-from-the-boot-configuration-data-bcd-store"></a>Önyükleme Yapılandırma Verileri (BCD) deposundan hatalı belleği temizleme

1. Yükseltilmiş bir komut istemi açın (yönetici olarak çalıştır).
1. Aşağıdaki komutla hatalı bellek bayrakları için önyükleme yapılandırma dosyasını sorgulayın:

   ``
   bcdedit /store <LETTER ASSIGNED TO THE OS DISK>:\boot\bcd /enum {badmemory}
   ``
   
   - Komutunda, `<LETTER ASSIGNED TO THE OS DISK>` işletim sistemi diskinin harfiyle değiştirin (örneğin, *F:*).

1. Sorgu bozuk bellek blokları gösteriyorsa, sonraki göreve atlayın. Aksi takdirde, 4. adıma geçin.
1. Hatalı bellek blokları tanımlanmışsa, bu bloklar bir Ramdisk oluşturulmasını engeller ve aşağıdaki komutla silinmelidir:

   ``
   bcdedit /store <LETTER ASSIGNED TO THE OS DISK>:\boot\bcd /deletevalue {badmemory} badmemorylist
   ``
   
   - Komutunda, `<LETTER ASSIGNED TO THE OS DISK>` işletim sistemi diskinin harfiyle değiştirin (örneğin, *F:*).

### <a name="restore-the-page-file-to-its-default-location"></a>Sayfa dosyasını varsayılan konumuna geri yükleme

Sayfa dosyası, bilgisayarınızın rastgele erişim belleği (RAM) tarafından bir taşma/yedekleme biçimi olarak tutulmayan verileri saklar. Dosya, varsayılan Azure konumu olan geçici sürücü yerine bir VHD 'de barındırılıyor olabilir. True ise, dosya erişilebilir olmayabilir ve varsayılan konuma geri yüklenmelidir.

Herhangi bir adımı gerçekleştirmeden önce, iyi durumda bir diskte **\Windows\System32\Config** klasörünün bir kopyasını oluşturmanız gerekir. Bu adım, istenmeyen değişiklikleri geri almanızı sağlar. Önemli sistem dosyaları üzerinde çalışıyoruz, bu nedenle bu önlem kesinlikle önerilir.

1. Windows Search 'te **Regedit** girin ve kayıt defteri Düzenleyicisi uygulamasını açın.
1. Kayıt Defteri Düzenleyicisi 'nde, anahtar **HKEY_LOCAL_MACHINE** vurgulayın ve menüden **Dosya > Hive... Yükle** ' yi seçin.

   ![Kayıt defteri düzenleyicisinin Hive yükle menüsü.](./media/troubleshoot-windows-stop-error/4.png)

1. Hive Yükle iletişim kutusunda **\Windows\system32\config\system** ' i seçin ve Aç ' a tıklayın.
   1. Bir ad girmeniz istenir, bu, **brokensystem**girmelisiniz. Bu ad, sorun giderirken etkilenen kovanları ayırt etmenize yardımcı olur.
   1. Eklediğiniz yeni BROKENSYSTEM anahtarını görmek için **HKEY_LOCAL_MACHINE** ' ı genişletin.
1. Kayıt Defteri Düzenleyicisi 'ni kullanarak makinenin hangi ControlSet 'e önyükleme olduğunu belirleyin.
   1. **HKEY_LOCAL_MACHINE >> BROKENSYSTEM >> Seç**' e gidin.
   1. Listelenen anahtarlar ' da, geçerli öğesinin veri değerini aklınızda yapın. Örneğin, bu değer **1** veya **0x00000001 (1)** Ise, denetim kümesi ControlSet001 olur.
1. Disk belleği oluşturma 'nın yapılandırıldığı Konumu kontrol edin.
   1. HKEY_LOCAL_MACHINE \BROKENSYSTEM sırasında, **ControlSet001**gibi 4. adımda tanımladığınız CONTROLSET numarasıyla eşleşen dizini genişletin.
   1. **Denetim >> oturum yöneticisi >> bellek yönetimi** ' ne gidin ve **existingpagefiles** anahtarının konumunu aklınızda edin.
   1. Bu anahtar, geçici sürücünün varsayılan Azure konumunda olmalıdır. Bu yoksa ve veri diski sürücüsü ya da işletim sistemi sürücüsü gibi başka bir konumdaki VHD 'de yer alıyorsa, silinmesi gerekecektir.
   1. Dosya Gezgini 'nde bu konuma gidin ve **pagefile.sys** dosyayı silin.

### <a name="enable-the-serial-console-and-memory-dump-collection"></a>Seri konsol ve bellek dökümü toplamayı etkinleştirme

**Önerilir**: VM 'yi yeniden oluşturmadan önce, aşağıdaki betiği çalıştırarak seri konsol ve bellek dökümü toplamasını etkinleştirin:

Bellek dökümü toplamayı ve seri konsolunu etkinleştirmek için aşağıdaki betiği çalıştırın:

1. Yükseltilmiş bir komut istemi oturumunu yönetici olarak açın.
1. BCD deposu verilerini listeleyin ve sonraki adımda kullanacağınız önyükleme Yükleyici tanımlayıcısını saptayın.

   1. 1. nesil bir VM için aşağıdaki komutu girin ve listelenen tanımlayıcıyı aklınızda yapın:
   
      ``
      bcdedit /store <BOOT PARTITON>:\boot\bcd /enum
      ``
   
   - Komutunda, öğesini `<BOOT PARTITON>` önyükleme klasörünü içeren, eklenen diskteki bölümünün harfiyle değiştirin.

      ![Windows önyükleme yükleyicisi 'nin kimlik numarası altında listelendiği, 1. nesil bir VM 'de BCD deposunun listelenme çıktısı.](./media/troubleshoot-windows-stop-error/5.png)

   1. 2. nesil bir VM için aşağıdaki komutu girin ve listelenen tanımlayıcıyı aklınızda yapın:
   
      ``
      bcdedit /store <LETTER OF THE EFI SYSTEM PARTITION>:EFI\Microsoft\boot\bcd /enum
      ``
   
   - Komutunda, `<LETTER OF THE EFI SYSTEM PARTITION>` EFI sistem bölümünün harfiyle değiştirin.
   - **EFI sistem bölümü**olarak etiketlenmiş uygun sistem bölümünü belirlemek Için Disk Yönetimi konsolunu başlatmak faydalı olabilir.
   - Tanımlayıcı, benzersiz bir GUID olabilir veya varsayılan **Bootmgr**olabilir.

1. Seri konsolunu etkinleştirmek için aşağıdaki komutları çalıştırın:

   ```
   bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON 
   bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200
   ```
   
   - Komutunda, `<VOLUME LETTER WHERE THE BCD FOLDER IS>` BCD klasörünün harfiyle değiştirin.
   - Komutunda, `<BOOT LOADER IDENTIFIER>` önceki adımda bulduğunuz tanımlayıcıyla değiştirin.

1. İşletim sistemi diskindeki boş alanın, VM 'deki bellek boyutundan (RAM) daha büyük olduğunu doğrulayın.

   İşletim sistemi diskinde yeterli alan yoksa, bellek dökümü dosyasının oluşturulacağı konumu değiştirin ve bu konumu, yeterli boş alana sahip olan VM 'ye bağlı herhangi bir veri diskine başvurun. Konumu değiştirmek için, aşağıdaki komutlarda **% systemroot%** değerini, **F:** sürücüsü gibi veri diskinin sürücü harfiyle değiştirin.

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
