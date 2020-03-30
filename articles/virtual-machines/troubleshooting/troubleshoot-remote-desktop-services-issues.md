---
title: Uzak Masaüstü Hizmetleri Azure VM|'nde başlatılmıyor | Microsoft Dokümanlar
description: Sanal bir makineye bağlandığınızda Uzak Masaüstü Hizmetleri ile ilgili sorunları nasıl gidereceklerini öğrenin | Microsoft Dokümanlar
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/23/2018
ms.author: genli
ms.openlocfilehash: 4b314fbdb9cbc0c0b797cbee8e92ee4702bbea81
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77919473"
---
# <a name="remote-desktop-services-isnt-starting-on-an-azure-vm"></a>Uzak Masaüstü Hizmetleri Azure VM'de başlatılmıyor

Bu makalede, bir Azure sanal makinesine (VM) ve Uzak Masaüstü Hizmetlerine veya TermService'e bağlandığınızda, başlangıç olmadığında veya başlatıldığında sorunları nasıl giderilemeyeceğiniz açıklanmaktadır.


## <a name="symptoms"></a>Belirtiler

Bir VM'ye bağlanmaya çalıştığınızda aşağıdaki senaryoları yaşarsınız:

- VM ekran görüntüsü, işletim sisteminin tamamen yüklü olduğunu ve kimlik bilgilerini beklediğini gösterir.

    ![VM durumunun ekran görüntüsü](./media/troubleshoot-remote-desktop-services-issues/login-page.png)

- Olay Görüntüleyicisi'ni kullanarak VM'deki olay günlüklerini uzaktan görüntüleyebilirsiniz. Uzak Masaüstü Hizmetleri, TermService,'in başlamadığını veya başlayamadığını görürsünüz. Aşağıdaki günlük bir örnektir:

    **Günlük Adı**: Sistem </br>
    **Kaynak**: Servis Kontrol Müdürü </br>
    **Tarih**: 12.12.2017 11:19:36</br>
    **Etkinlik Kodu**: 7022</br>
    **Görev Kategorisi**: Yok</br>
    **Level**: Hata</br>
    **Anahtar Kelimeler**: Klasik</br>
    **Kullanıcı**: N/A</br>
    **Bilgisayar**: vm.contoso.com</br>
    **Açıklama**: Uzak Masaüstü Hizmetleri hizmeti başlangıç asılı. 

    Aşağıdaki sorguyu çalıştırarak bu hataları aramak için Seri Erişim Konsolu özelliğini de kullanabilirsiniz: 

        wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Service Control Manager'] and EventID=7022 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more 

## <a name="cause"></a>Nedeni
 
Uzak Masaüstü Hizmetleri VM'de çalışmadığından bu sorun oluşur. Nedeni aşağıdaki senaryolara bağlı olabilir: 

- TermService hizmeti Devre **Dışı bırakılmış**olarak ayarlanır. 
- TermService hizmeti çöküyor veya yanıt vermiyor. 
- TermService yanlış bir yapılandırma nedeniyle başlatmıyor.

## <a name="solution"></a>Çözüm

Bu sorunu gidermek için Seri Konsolu'nu kullanın. Yoksa [VM'nin](#repair-the-vm-offline) işletim sistemi diskini kurtarma VM'ine takarak VM'yi çevrimdışı onar.

### <a name="use-serial-console"></a>Seri Konsol kullan

1. **Destek & Sorun Giderme** > Seri konsolu seçerek Seri**Konsoluna**erişin. [Serial Console](serial-console-windows.md) Özellik VM'de etkinse, VM'yi başarıyla bağlayabilirsiniz.

2. CMD örneği için yeni bir kanal oluşturun. Kanalı başlatmak ve kanal adını almak için **CMD** girin.

3. CMD örneğini çalıştıran kanala geçin. Bu durumda, kanal 1 olmalıdır:

   ```
   ch -si 1
   ```

4. VM için yeniden **Enter'u** seçin ve geçerli bir kullanıcı adı ve parola, yerel veya etki alanı kimliği girin.

5. TermService hizmetinin durumunu sorgula:

   ```
   sc query TermService
   ```

6. Hizmet durumu **Durduruldu'yu**gösteriyorsa, hizmeti başlatmayı deneyin:

    ```
    sc start TermService
     ``` 

7. Hizmetin başarıyla başlatıldıklıolduğundan emin olmak için hizmeti yeniden sorgula:

   ```
   sc query TermService
   ```
8. Hizmet başlatılamazsa, aldığınız hataya göre çözümü izleyin:

    |  Hata |  Öneri |
    |---|---|
    |5- ERIŞIM ENGELLENDI |Bkz. [Bir Erişim Reddedilmiş hata nedeniyle TermService hizmeti durdurulur.](#termservice-service-is-stopped-because-of-an-access-denied-problem) |
    |1053 - ERROR_SERVICE_REQUEST_TIMEOUT  |Bkz. [TermService hizmeti devre dışı bırakılır.](#termservice-service-is-disabled)  |  
    |1058 - ERROR_SERVICE_DISABLED  |[Bkz. TermService hizmeti çöküyor veya asılı.](#termservice-service-crashes-or-hangs)  |
    |1059 - ERROR_CIRCULAR_DEPENDENCY |Sorunuzun hızlı bir şekilde çözülmesi için [desteğe başvurun.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)|
    |1067 - ERROR_PROCESS_ABORTED  |[Bkz. TermService hizmeti çöküyor veya asılı.](#termservice-service-crashes-or-hangs)  |
    |1068 - ERROR_SERVICE_DEPENDENCY_FAIL|Sorunuzun hızlı bir şekilde çözülmesi için [desteğe başvurun.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)|
    |1069 - ERROR_SERVICE_LOGON_FAILED  |Bkz. [Oturum açma hatası nedeniyle TermService hizmeti başarısız olur](#termservice-service-fails-because-of-logon-failure) |
    |1070 - ERROR_SERVICE_START_HANG   | [Bkz. TermService hizmeti çöküyor veya asılı.](#termservice-service-crashes-or-hangs) |
    |1077 - ERROR_SERVICE_NEVER_STARTED   | Bkz. [TermService hizmeti devre dışı bırakılır.](#termservice-service-is-disabled)  |
    |1079 - ERROR_DIFERENCE_SERVICE_ACCOUNT   |Sorunuzun hızlı bir şekilde çözülmesi için [desteğe başvurun.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) |
    |1753   |Sorunuzun hızlı bir şekilde çözülmesi için [desteğe başvurun.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)   |
    
#### <a name="termservice-service-is-stopped-because-of-an-access-denied-problem"></a>Erişim Reddedildi sorunu nedeniyle TermService hizmeti durduruldu

1. Seri [Konsol'a](serial-console-windows.md) bağlanın ve bir PowerShell örneğini açın.
2. Aşağıdaki komut dosyasını çalıştırarak İşlem İzleme aracını indirin:

   ```
   remove-module psreadline  
   $source = "https://download.sysinternals.com/files/ProcessMonitor.zip" 
   $destination = "c:\temp\ProcessMonitor.zip" 
   $wc = New-Object System.Net.WebClient 
   $wc.DownloadFile($source,$destination) 
   ```

3. Şimdi bir **procmon** izi başlatın:

   ```
   procmon /Quiet /Minimized /BackingFile c:\temp\ProcMonTrace.PML 
   ```

4. **Access Denied**veren hizmeti başlatarak sorunu yeniden oluşturma: 

   ```
   sc start TermService 
   ```

   Başarısız olduğunda, İşlem İzi'ni sonlandırın:

   ```   
   procmon /Terminate 
   ```

5. Dosyayı toplamak **c:\temp\ProcMonTrace.PML**:

    1. [VM'ye bir veri diski takın.](../windows/attach-managed-disk-portal.md
)
    2. Seri Konsol'u kullanarak dosyayı yeni sürücüye kopyalayabilirsiniz. Örneğin, `copy C:\temp\ProcMonTrace.PML F:\`. Bu komutta, F ekli veri diskinin sürücü harfidir.
    3. Veri sürücüsünün sökülüp, Işlem Monitörü ubstakke yüklü çalışan bir VM'ye takın.

6. **ProcMonTrace.PML'yi** Process Monitor kullanarak çalışan VM'yi açın. Daha sonra Sonuç tarafından filtre **ACCESS DENIED ,** aşağıdaki ekran görüntüsünde gösterildiği gibi:

    ![İşlem Monitörü'nde sonuca göre filtreleme](./media/troubleshoot-remote-desktop-services-issues/process-monitor-access-denined.png)

 
6. Kayıt defteri anahtarlarını, klasörleri veya çıktıdaki dosyaları düzeltin. Genellikle, bu sorun, hizmette kullanılan oturum açma hesabının bu nesnelere erişmek için ACL izni yoksa kaynaklanır. Oturum açma hesabı için doğru ACL iznini bilmek için sağlıklı bir VM'yi kontrol edebilirsiniz. 

#### <a name="termservice-service-is-disabled"></a>TermService hizmeti devre dışı

1. Hizmeti varsayılan başlangıç değerine geri yükleme:

   ```
   sc config TermService start= demand 
   ```

2. Hizmeti başlatın:

   ```
   sc start TermService
   ```

3. Hizmetin çalışmadığından emin olmak için durumunu yeniden sorgula:

   ```
   sc query TermService 
   ```

4. Uzak Masaüstü'nü kullanarak VM'ye bağlanmayı deneyin.

#### <a name="termservice-service-fails-because-of-logon-failure"></a>TermService hizmeti oturum açma hatası nedeniyle başarısız olur

1. Bu hizmetin başlangıç hesabı değiştirildiyse, bu sorun oluşur. Bu geri varsayılan olarak değiştirildi: 

        sc config TermService obj= 'NT Authority\NetworkService'
2. Hizmeti başlatın:

        sc start TermService
3. Uzak Masaüstü'nü kullanarak VM'ye bağlanmayı deneyin.

#### <a name="termservice-service-crashes-or-hangs"></a>TermService hizmeti çöküyor veya askıda
1. Hizmet durumu **Başlangıç** veya **Durdurma'da**sıkışmışsa, hizmeti durdurmaya çalışın: 

        sc stop TermService
2. Hizmeti kendi 'svchost' kapsayıcısına ayırın:

        sc config TermService type= own
3. Hizmeti başlatın:

        sc start TermService
4. Hizmet hala başlatılamazsa, [desteğe başvurun.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)

### <a name="repair-the-vm-offline"></a>VM'yi çevrimdışı onar

#### <a name="attach-the-os-disk-to-a-recovery-vm"></a>Os diskini kurtarma VM'ine takın

1. [Os diskini kurtarma VM'sine takın.](../windows/troubleshoot-recovery-disks-portal.md)
2. Kurtarma VM'sine Uzak Masaüstü bağlantısı başlatın. Ekli diskin Disk Yönetimi konsolunda **Çevrimiçi** olarak işaretlendiğini unutmayın. Bağlı işletim sistemi diskine atanan sürücü mektubuna dikkat edin.
3. Yükseltilmiş komut istemi örneğini açın (**Yönetici olarak çalıştırın).** Ardından aşağıdaki komut dosyasını çalıştırın. Ekli işletim sistemi diskine atanan sürücü harfinin **F**olduğunu varsayıyoruz. VM'inizdeki uygun değerle değiştirin. 

   ```
   reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv
        
   REM Set default values back on the broken service 
   reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v start /t REG_DWORD /d 3 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v ObjectName /t REG_SZ /d "NT Authority\NetworkService“ /f
   reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v type /t REG_DWORD /d 16 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v start /t REG_DWORD /d 3 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v ObjectName /t REG_SZ /d "NT Authority\NetworkService" /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v type /t REG_DWORD /d 16 /f
   ```

4. [İşletim sistemi diskini ayırın ve VM'yi yeniden oluşturun.](../windows/troubleshoot-recovery-disks-portal.md) Ardından sorunun çözülüp çözülmediğini kontrol edin.

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun

Hala yardıma ihtiyacınız varsa, sorunuzun çözülmesi için [desteğe başvurun.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
