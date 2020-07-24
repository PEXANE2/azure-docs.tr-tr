---
title: Windows Önyükleme Yöneticisi hata sorunlarını giderme-0xC0000225 "durum bulunamadı"
description: Bir Azure VM 'de 0xC0000225 hata kodu sırasında oluşan sorunları çözme adımları.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: c17899a4-b270-4725-9530-0dcd829b178c
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 05/11/2020
ms.author: v-miegge
ms.openlocfilehash: 3677d67f55cfccdc80245b2ec870ffa76b0a1940
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87088674"
---
# <a name="troubleshoot-windows-boot-manager-error----0xc0000225-status-not-found"></a>Windows Önyükleme Yöneticisi hata sorunlarını giderme-0xC0000225 "durum bulunamadı"
 
Bu makalede, bir Azure VM 'de 0xC0000225 hata kodu oluştuğunda oluşan sorunları gidermek için adımlar sağlanmaktadır. Bu hata durum veya nesnenin bulunamadığını belirtir.

## <a name="symptoms"></a>Belirtiler

VM 'nin ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](./boot-diagnostics.md) kullandığınızda, ekran görüntüsünde, *0xc0000225*durum koduyla bir Windows 'un hata başlatma başarısız olduğunu görürsünüz.

Bu hata koduyla ilişkili dosya, sorunu çözmek için hangi adımların ele alınacağı hakkında bilgi sağlayacaktır. Uygun eylem kursu belirlemek için **dosyanın** metin bölümünü bulun.

### <a name="drivers-os-related-or-third-party"></a>Sürücüler, işletim sistemi ile Ilgili veya üçüncü taraf

Dosya varsa, ancak bir sürücüye başvuruyorsa (gösterildiği gibi) ya da işletim sistemi ile ilgili veya üçüncü taraf, [sistem dosyasını onarma](#repair-the-system-file)bölümünde verilen adımları izleyin.
 
Aşağıdaki görüntüde, Windows Önyükleme Yöneticisi "Windows başlatılamadı" olarak belirtilmiştir. Yeni bir donanım veya yazılım değişikliği nedeni olabilir. " Görüntüde Ayrıca durum "0xc0000225", **Dosya:** as `\windows\System32\drivers\atapi.sys` ve **INFO:** "bir kritik sistem sürücüsü eksik olduğundan veya hata içerdiğinden," işletim sistemi yüklenemedi. "olarak görüntülenir.

![Windows Önyükleme Yöneticisi durumları ' Windows başlatılamadı. Yeni bir donanım veya yazılım değişikliği nedeni olabilir. ' Ayrıca, durumu "0xc0000225", dosya ' \windows\System32\drivers\atapi.sys ' olarak ve bilgi bölümünde şu durumlar gösterilmektedir: "kritik sistem sürücüsü eksik veya hata içerdiğinden, işletim sistemi yüklenemedi."](./media/troubleshoot-boot-error-status-not-found/1.png)

### <a name="no-file"></a>Dosya yok

Durum kodu ile sunulursa ancak dosya gösterilmezse, [OSDEVICE değişkenini ekleme](#add-the-osdevice-variable)bölümündeki adımları izleyin.

Aşağıdaki görüntüde, Windows Önyükleme Yöneticisi "Windows başlatılamadı" olarak belirtilmiştir. Yeni bir donanım veya yazılım değişikliği nedeni olabilir. " Görüntüde Ayrıca "0xc0000225" durumu ve **INFO:** "gerekli bir cihaza erişilemediğinden önyükleme seçimi başarısız oldu" olarak gösterilir.

![Windows Önyükleme Yöneticisi durumları ' Windows başlatılamadı. Yeni bir donanım veya yazılım değişikliği nedeni olabilir. ' Ayrıca, durumu "0xc0000225" olarak ve bilgi bölümünde şu durumlar gösterilmektedir: "gerekli bir cihaza erişilemediğinden önyükleme seçimi başarısız oldu."](./media/troubleshoot-boot-error-status-not-found/2.png)

### <a name="registry-file"></a>Kayıt defteri dosyası

\Windows\System32\Config\System gibi kayıt defteri dosyalarından herhangi birine başvuruyorsa, [bir destek bileti oluşturma](#contact-support)bölümünde açıklanan adımları izleyin.
 
Aşağıdaki görüntüde, Windows Önyükleme Yöneticisi "Windows başlatılamadı" olarak belirtilmiştir. Yeni bir donanım veya yazılım değişikliği nedeni olabilir. " Görüntüde Ayrıca durum "0xc0000225", dosya as `\windows\System32\config\system` ve **INFO:** "sistem kayıt defteri dosyası eksik ya da hatalar içerdiğinden" işletim sistemi yüklenemedi. "şeklinde görüntülenir.

![Windows Önyükleme Yöneticisi durumları ' Windows başlatılamadı. Yeni bir donanım veya yazılım değişikliği nedeni olabilir. ' Ayrıca, durumu "0xc0000225", dosya ' \windows\System32\config\system ' olarak ve bilgi bölümünde şu durumlar gösterilmektedir: "sistem kayıt defteri dosyası eksik veya hatalar içerdiğinden, işletim sistemi yüklenemedi."](./media/troubleshoot-boot-error-status-not-found/3.png)

Aşağıdaki görüntüde, kurtarma ekranı "PC/cihazınızın onarılması gerekiyor" durumuna sahiptir. Sistem kayıt defteri dosyası eksik olduğundan veya hatalar içerdiğinden, işletim sistemi yüklenemedi. " Görüntüde Ayrıca hata kodu "0xc0000225" ve dosya olarak gösterilir `\windows\System32\config\system` .

![Kurtarma ekranı, bilgisayarınızın/cihazınızın onarılması gerekiyor. Sistem kayıt defteri dosyası eksik olduğundan veya hatalar içerdiğinden, işletim sistemi yüklenemedi. Ayrıca, hata kodu ' 0xc0000225 ' ve dosya ' \windows\System32\config\system ' olarak gösterilir.](./media/troubleshoot-boot-error-status-not-found/4.png)

## <a name="causes"></a>Nedenler

### <a name="missing-binary"></a>Eksik Ikili

Sistem **(. sys)** dosyanızda eksik veya bozuk ikiliye karşılaşıyor olabilirsiniz.

### <a name="bcd-corruption-or-improper-vhd-migration"></a>BCD bozulması veya Hatalı VHD geçişi

Bu durumda, **önyükleme yapılandırma verileri (BCD)** bozuk ya da **sanal sabıt sürücü (VHD)** Şirket içinden geçirildi, ancak düzgün şekilde hazırlanmadı. Sonuç, **osdevice** değişkeninin eksik olması ve eklenmesi gerekecektir.

### <a name="registry-hive-corruption"></a>Kayıt defteri Hive bozulması

Şu nedenlerle bir kayıt defteri kovanı bozulması olabilir:

- Hive başarısız oluyor
- Hive takar, ancak boştur
- Hive düzgün şekilde kapatılmadı
## <a name="solution"></a>Çözüm

### <a name="process-overview"></a>İşleme genel bakış

1. Bir onarım VM 'si oluşturun ve erişin.
1. Bir çözüm seçin:
   - [Sistem dosyasını onarma](#repair-the-system-file)
   - [OSDevice değişkenini ekleme](#add-the-osdevice-variable)
   - [Destek bileti oluşturma](#contact-support)
1. Seri konsolu ve bellek dökümü toplamayı etkinleştirin.
1. VM 'yi yeniden derleyin.

### <a name="create-and-access-a-repair-vm"></a>Bir onarım VM 'si oluşturma ve erişme

1. Bir onarım VM 'si hazırlamak için [VM onarım komutlarının](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) 1-3 adımlarını kullanın.
1. Uzak Masaüstü Bağlantısı kullanarak, onarım sanal makinesine bağlanın.

### <a name="select-a-solution"></a>Bir çözüm seçin

1. Yükseltilmiş bir komut istemi açın.
1. Daha önce tanımladığınız belirtiye göre, ilgili çözümdeki adımları izleyin. Sorununuz için uygulanmayadıklarından, diğer çözümlerdeki adımları atlayabilirsiniz:

- [Sistem dosyasını onarma](#repair-the-system-file)
- [OSDevice değişkenini ekleme](#add-the-osdevice-variable)
- [Destek bileti oluşturma](#contact-support)

### <a name="repair-the-system-file"></a>Sistem dosyasını onarma

1. Bağlı VHD 'yi kullanarak, sanal makine (VM) ekran görüntüsünde gösterilen ikilinin dosya konumuna gidin.
1. Dosyaya sağ tıklayın, **Özellikler**' i seçin ve ardından dosya hakkındaki bilgileri görmek için **Ayrıntılar** sekmesini seçin.
   1. Aşağıdaki görüntüde gösterildiği gibi dosyanın sürümünü aklınızda bulabilirsiniz:

      ![Dosya sürümü vurgulanmış olarak ' cng.sys ' dosyasının Özellikler penceresi.](./media/troubleshoot-boot-error-status-not-found/5.png)

1. Dosyayı **< BINARY.SYS >. old**olarak yeniden adlandırın ve **< BINARY.SYS >** dosyasını dosyanın adıyla değiştirin.

   Yukarıdaki adımdaki görüntüde, dosya **cng.sys** **cng.sys. old** olarak yeniden adlandırılacak

   > [!NOTE]
   > Dosyayı yeniden adlandırmaya ve "dosya bozuk ve okunamaz" iletisini almaya çalışırsanız, bu çözüm çalışmamakta olduğundan [Yardım için desteğe başvurun](https://azure.microsoft.com/support/create-ticket/).

1. Bozuk dosya yeniden adlandırıldığına göre, dosyayı iç deposundan geri yükleyerek dosyayı düzeltemedi.
   1. Bir **cmd** oturumu başlatın.
   1. **\Windows\winsxs**dizinine gidin.

   1. Aşağıdaki komutu kullanarak bu bölümün başlangıcında bulunan ikiliyi arayın:

      `dir <BINARY WITH ".SYS" EXTENSION>  /s`

      Bu komut, dosyanın sahip olduğu tüm dosya sürümlerini listelecektir ve size bu bileşenin yol geçmişini verir.
      
      Örneğin, **dir cng.sys** **dir cng.sys/s** olarak yeniden adlandırılır

   1. Listedeki dosyanın en son sürümünü (veya dilediğiniz herhangi birini) seçin ve önceki yolu ve aşağıdaki komutu kullanarak dosyayı **Windows\System32** klasörüne kopyalayın:

      `copy <drive>:\Windows\WinSxS\<DIRECTORY WHERE FILE IS>\<BINARY WITH ".SYS" EXTENSION> <DRIVE>:\Windows\System32\Drivers\`

      > [!NOTE]
      > En son ikili işe yaramazsa, önce bir sürümü veya bir düzeltme ekiyle önceki sürüm gibi kararlı bir dosya olduğunu bildiğiniz bir sürümü deneyin.

      Örneğin, aradığınız ikili **cmimcext.sys**, hatalı sürücü **F:** sürücüsüdür ve yalnızca en son sürüm için bir arama çalıştırdıysanız, komut isteminde bir sorgunun `dir cmim* /s` cmimcext.sys dosyasının en son sürümünü bulacağı aşağıdaki görüntüyü görürsünüz.

      ![cmimcext.sys dosyasının en son sürümünü bulmak için "dir cmım */s" komut isteminde bir sorgu.](./media/troubleshoot-boot-error-status-not-found/6.png)

      Yukarıdaki örnek görüntüde sorgu **C:**' de gerçekleştirildi, ancak sürücü harfi, onarım VM 'sinde bir veri diski olarak eklenmiş işletim sistemi diski olan **F:** hatalı sürücüde olmalıdır.

      Dosyayı kopyalamak için elde edilen komut şöyle olacaktır: `copy F:\Windows\WinSxS\amd64_xxxxxx\cmimcext.sys F:\Windows\System32\Drivers` .

Bu görev tamamlandıktan sonra [seri konsol ve bellek dökümü toplamayı etkinleştirmeye](#enable-the-serial-console-and-memory-dump-collection)devam edin.

### <a name="add-the-osdevice-variable"></a>OSDEVICE değişkenini ekleme

Geçerli kurulum bilgilerini toplayın ve etkin bölümdeki tanımlayıcıyı aklınızda edin. Daha sonra bu bilgileri, sanal makinenizin nesli için yönergeleri izleyerek **osdevice** değişkenini eklemek üzere kullanacaksınız.

Bu bilgi toplama, bir **\Boot\BCD** dosyası olmadığında bir hata verirse, bunun yerine [sistem dosyasını onarma](#repair-the-system-file) bölümündeki yönergeleri kullanın. 

1. 1. nesil sanal makineler için, yönetici olarak yükseltilmiş bir komut istemi açın ve aşağıdaki komutu girin:

   `bcdedit /store <LETTER OF BOOT FOLDER>:\boot\bcd /enum`

   Bu görüntüde, tanıtıcı özniteliği vurgulanmış şekilde, 1. nesil bir VM 'de Windows önyükleme yükleyicisi gösterilmektedir. Vurgulanan tanımlayıcı özniteliği benzersiz bir alfasayısal dize gösterir.

   ![Tanıtıcı özniteliği vurgulanmış şekilde, 1. nesil bir VM 'de gösterilen Windows önyükleme yükleyicisi. Vurgulanan tanımlayıcı özniteliği benzersiz bir alfasayısal dize gösterir.](./media/troubleshoot-boot-error-status-not-found/7.png)

   Yolu **\windows\system32\winload.exe**olan Windows önyükleme yükleyicisinin tanımlayıcısını aklınızda edin.

1. 2. nesil VM 'Ler için, işletim sistemi diskinin çevrimiçi olduğunu ve bölüm sürücü harflerinin atandığını doğrulayın. Bu doğrulandığında, önyükleme kurulum bilgilerini toplayın.
   1. **Windows Search**'Te **disk yönetimi** yazın ve disk yönetimi konsolunu açın. Bu konsolu, onarım sanal makinenize bağlı disk numarasını ve BCD deposunu tutan Genişletilebilir Bellenim Arabirimi (EFı) bölümünü belirlemek için kullanın.

   Aşağıdaki görüntüde disk 2, onarım sanal makinesine bağlı disk numarasıdır. Görüntüde Ayrıca, disk 2 ' de bulunan ve 100 MB boyutundaki ve atanmış bir harf olmayan disk 2 ' deki EFı sistem bölümü gösterilmektedir.

   ![Disk 2, onarım sanal makinesine bağlı disk numarası olarak gösterilir. Ayrıca, disk 2 ' de bulunan ve 100 MB olan ve kendisine atanmış bir harf olmayan, EFı sistem bölümünü gösterir.](./media/troubleshoot-boot-error-status-not-found/8.png)

   1. Yönetici olarak yükseltilmiş bir komut istemi açın ve aşağıdaki komutları girin:
      1. Komutunu kullanarak **DISKPART aracını** açın `diskpart` .
      1. Tüm diskleri listeleyin ve ardından önceki adımda tanımlanan ekli diski seçin:
      
         ```
         list disk
         sel disk <DISK #>
         ```

         Aşağıdaki görüntüde bir diski listeleme ve seçme sonuçları gösterilmektedir. Disk 0 (127 GB/çevrimiçi), disk 1 (32 GB/çevrimiçi) ve disk 2 (127 GB/çevrimiçi), komutunu kullanarak disk 2 ' nin seçilme ile listelenir `sel disk 2` .

         ![Disk listeleme ve sonra bir diski seçme sonuçları. Disk 0 (127 GB | Çevrimiçi), disk 1 (32 GB | Çevrimiçi) ve disk 2 (127 GB | Çevrimiçi), 2. disk seçiliyken listelenir.](./media/troubleshoot-boot-error-status-not-found/9.png)

      1. Bölümleri listeleyin ve önceki adımda tanımlanan EFı sistem bölümünü seçin:
      
         ```
         list partition
         sel partition <PARTITION #>
         ```

         Aşağıdaki görüntüde bir bölümü listeleme ve seçme sonuçları gösterilmektedir. Bölüm 1 (ayrılmış/16), Bölüm 2 (sistem/100 MB) ve Bölüm 3 (birincil/126 GB) listelenir. Bu komut, Bölüm 2 ' yi kullanarak seçilir `sel part 2` .

         ![Bölüm listeleme ve sonra bir bölümü seçme sonuçları. Bölüm 1 (ayrılmış | 16MB), Bölüm 2 (sistem | 100MB) ve Bölüm 3 (birincil | 126 GB), Bölüm 2 seçili olacak şekilde listelenmiştir.](./media/troubleshoot-boot-error-status-not-found/10.png)

      1. Komutunu kullanarak EFı bölümüne bir harf atayın `assign` .

         Aşağıdaki görüntüde, `assign` komut ve yeni sürücü **sisteminin (F:)** her Ikisi de dosya Gezgini 'nde görüntülenebilir.

         ![Assign komutu ve yeni sürücü SISTEMI (F:) Dosya Gezgini 'nde görüntülenebilir.](./media/troubleshoot-boot-error-status-not-found/11.png)

      1. Aşağıdaki komutu kullanarak BCD deposu verilerini listeleyin:
      
         `bcdedit /store <LETTER OF EFI SYSTEM PARTITION>:EFI\Microsoft\boot\bcd /enum`

         Aşağıdaki görüntüde, Windows önyükleme yükleyicisi tanımlayıcı özniteliği vurgulanmış bir 2. nesil VM 'de bulunur. Vurgulanan tanımlayıcı özniteliğinin değeri **{Default}**.

         ![Windows önyükleme yükleyicisi, tanımlayıcı özniteliği vurgulanmış şekilde 2. nesil bir VM 'de gösterilir. Tanımlayıcı özniteliği vurgulanmış olarak varsayılan değerini gösterir.](./media/troubleshoot-boot-error-status-not-found/12.png)

         Yolu **\windows\system32\wınload.exe**olan Windows önyükleme yükleyicisinin tanımlayıcısını aklınızda edin.

1. Etkin bölümdeki OSDEVICE değişkeninin eksik olduğuna dikkat edin:

   ![Windows Önyükleme Yöneticisi ve Windows önyükleme yükleyicisi öznitelikleri, komut isteminde, işletim sistemi cihaz özniteliği eksik olacak şekilde listelenmiştir.](./media/troubleshoot-boot-error-status-not-found/13.png)
   
   Bu görüntüde, Windows Önyükleme Yöneticisi ve Windows önyükleme yükleyicisi öznitelikleri komut isteminde listelenir, ancak OSDEVICE özniteliği eksiktir.

1. Aşağıdaki bilgilere göre OSDEVICE değişkenini ekleyin:

   Tek bölümlü işletim sistemi diskleri için, ekleyin `BOOT` .

   > [!NOTE]
   > Önyükleme yapılabilen klasör Windows klasörü **\Windows klasörüyle**aynı bölümde olacaktır.
   > - 1. nesil VM 'Ler için önyüklenebilir klasör **(\Boot\BCD klasörü)**.
   > - 2. nesil VM 'Lerin önyüklenebilir klasörü **Efi\microsoft\boot\bcd**şeklindedir.

   1. nesil VM 'Ler için aşağıdaki komutu girin:

   `bcdedit /store <LETTER OF BOOT FOLDER>:\boot\bcd /set {<IDENTIFIER>} OSDEVICE BOOT`

   2. nesil VM 'Ler için aşağıdaki komutu girin:

   `bcdedit /store <LETTER OF EFI FOLDER>:EFI\Microsoft\boot\bcd /set {<IDENTIFIER>} OSDEVICE BOOT`

   Birden çok bölüm işletim sistemi diski için, ekleyin `PARTITION=<LETTER OF WINDOWS FOLDER>:` .

   > [!NOTE]
   > Önyükleme yapılabilen klasör büyük olasılıkla Windows klasörü **\Windows klasöründen**farklı bir bölümde yer alır.
   > - 1. nesil VM 'Ler için önyüklenebilir klasör **(\Boot\BCD klasörü)**.
   > - 2. nesil VM 'Lerin önyüklenebilir klasörü **Efi\microsoft\boot\bcd**şeklindedir.

   1. nesil VM 'Ler için aşağıdaki komutu girin:

   `bcdedit /store <LETTER OF BOOT FOLDER>:\boot\bcd /set {<IDENTIFIER>} OSDEVICE partition=<LETTER OF WINDOWS FOLDER>:`

   2. nesil VM 'Ler için aşağıdaki komutu girin:

   `bcdedit /store <LETTER OF EFI FOLDER>:EFI\Microsoft\boot\bcd /set {< IDENTIFIER>} OSDEVICE partition=<LETTER OF WINDOWS FOLDER>:`

Bu görev tamamlandıktan sonra [seri konsol ve bellek dökümü toplamayı etkinleştirmeye](#enable-the-serial-console-and-memory-dump-collection)devam edin.

### <a name="contact-support"></a>Destek birimine başvurma

**Kayıt defteri dosyası** hatası bir çözüme sahiptir, ancak daha fazla yardım için [bir destek bileti oluşturmanız](https://azure.microsoft.com/support/create-ticket/) gerekir.

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

VM 'yi yeniden derlemek için [VM onarım komutlarının 5. adımını](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) kullanın.
