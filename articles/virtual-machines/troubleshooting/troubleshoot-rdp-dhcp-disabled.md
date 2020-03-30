---
title: DHCP devre dışı bırakıldığından Azure Sanal Makineleri'ne uzaktan bağlanamıyor| Microsoft Dokümanlar
description: Microsoft Azure'da DHCP istemci hizmetinin devre dışı bırakıldığında NEDEN olduğu RDP sorununu nasıl gidereceklerini öğrenin.| Microsoft Dokümanlar
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/13/2018
ms.author: genli
ms.openlocfilehash: 2c5b0556554d280e57b2df51875e1b057b5fb4a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75749884"
---
#  <a name="cannot-rdp-to-azure-virtual-machines-because-the-dhcp-client-service-is-disabled"></a>DHCP İstemci hizmeti devre dışı bırakıldığından Azure Sanal Makineleri'ne RDP yapılamaz

Bu makalede, DHCP İstemci hizmeti VM'de devre dışı bırakıldıktan sonra masaüstünü Azure Windows Sanal Makineleri'ne (VM) uzaklatalamayacağınız bir sorun açıklanmaktadır.


## <a name="symptoms"></a>Belirtiler
DHCP İstemci hizmeti VM'de devre dışı bırakıldığından, RDP bağlantısını Azure'da VM yapamazsınız. Azure portalındaki [Önyükleme tanılama](../troubleshooting/boot-diagnostics.md) bölümündeekran görüntüsünü kontrol ettiğinizde, VM önyüklemelerini normal olarak görürsünüz ve giriş ekranında kimlik bilgilerini beklersiniz. Olay Görüntüleyicisi'ni kullanarak VM'deki olay günlüklerini uzaktan görüntüleyebilirsiniz. DHCP İstemci Hizmetinin başlatılmadığını veya başlatılmadığını görürsünüz. Aşağıdaki örnek günlük:

**Günlük Adı**: Sistem </br>
**Kaynak**: Servis Kontrol Müdürü </br>
**Tarih**: 16.12.2015 11:19:36 </br>
**Etkinlik Kodu**: 7022 </br>
**Görev Kategorisi**: Yok </br>
**Level**: Hata </br>
**Anahtar Kelimeler**: Klasik</br>
**Kullanıcı**: N/A </br>
**Bilgisayar**: myvm.cosotos.com</br>
**Açıklaması**: DHCP İstemci hizmeti başlangıç asılı.</br>

Kaynak Yöneticisi VM'leri için, olay günlükleri 7022'yi sorgulamak için Seri Erişim Konsolu özelliğini aşağıdaki komutu kullanarak kullanabilirsiniz:

    wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Service Control Manager'] and EventID=7022 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more

Klasik VM'ler için ÇEVRIMDıŞı modda çalışmanız ve günlükleri el ile toplamanız gerekir.

## <a name="cause"></a>Nedeni

DHCP İstemci Hizmeti VM üzerinde çalışmıyor.

> [!NOTE]
> Bu makale, YALNıZCA DHCP İstemci hizmeti için geçerlidir, DHCP Sunucusu için geçerli değildir.

## <a name="solution"></a>Çözüm

Bu adımları izlemeden önce, etkilenen VM'nin işletim sistemi diskinin bir anlık görüntüsünü yedek olarak alın. Daha fazla bilgi için [bir diskanlık anlık görüntüsüne](../windows/snapshot-copy-managed-disk.md)bakın.

Bu sorunu gidermek için, DHCP'yi etkinleştirmek için Seri denetimini kullanın veya VM için [ağ arabirimini sıfırlayın.](reset-network-interface.md)

### <a name="use-serial-control"></a>Seri denetimi kullanma

1. Seri [Konsola bağlanın ve CMD örneğini açın.](serial-console-windows.md#use-cmd-or-powershell-in-serial-console)
). Seri Konsol VM'nizde etkinleştirilemiyorsa, [ağ arabirimini sıfırla'ya](reset-network-interface.md)bakın.
2. DHCP'nin ağ arabiriminde devre dışı bırakıldığını denetleyin:

        sc query DHCP
3. DHCP durdurulursa, hizmeti başlatmayı deneyin

        sc start DHCP

4. Hizmetin başarıyla başlatıldıklıolduğundan emin olmak için hizmeti yeniden sorgula.

        sc query DHCP

    VM'ye bağlanmayı deneyin ve sorunun çözülüp çözülmediğini gör
5. Hizmet başlatılamazsa, aldığınız hata iletisini temel alan aşağıdaki uygun çözümü kullanın:

    | Hata  |  Çözüm |
    |---|---|
    | 5- ERIŞIM ENGELLENDI  | Bkz. [DHCP İstemci hizmeti, Erişim Reddedilen bir hata nedeniyle durduruldu.](#dhcp-client-service-is-stopped-because-of-an-access-denied-error)  |
    |1053 - ERROR_SERVICE_REQUEST_TIMEOUT   | [Bkz. DHCP İstemci hizmeti çöküyor veya asılı.](#dhcp-client-service-crashes-or-hangs)  |
    | 1058 - ERROR_SERVICE_DISABLED  | Bkz. [DHCP İstemci hizmeti devre dışı.](#dhcp-client-service-is-disabled)  |
    | 1059 - ERROR_CIRCULAR_DEPENDENCY  |Sorunuzun hızlı bir şekilde çözülmesi için [desteğe başvurun.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)   |
    | 1067 - ERROR_PROCESS_ABORTED |[Bkz. DHCP İstemci hizmeti çöküyor veya asılı.](#dhcp-client-service-crashes-or-hangs)   |
    |1068 - ERROR_SERVICE_DEPENDENCY_FAIL   | Sorunuzun hızlı bir şekilde çözülmesi için [desteğe başvurun.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)  |
    |1069 - ERROR_SERVICE_LOGON_FAILED   |  Bkz. [DHCP İstemci hizmeti oturum açma hatası nedeniyle başarısız oluyor](#dhcp-client-service-fails-because-of-logon-failure) |
    | 1070 - ERROR_SERVICE_START_HANG  | [Bkz. DHCP İstemci hizmeti çöküyor veya asılı.](#dhcp-client-service-crashes-or-hangs)  |
    | 1077 - ERROR_SERVICE_NEVER_STARTED  | Bkz. [DHCP İstemci hizmeti devre dışı.](#dhcp-client-service-is-disabled)  |
    |1079 - ERROR_DIFERENCE_SERVICE_ACCOUNT   | Sorunuzun hızlı bir şekilde çözülmesi için [desteğe başvurun.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)  |
    |1053 | Sorunuzun hızlı bir şekilde çözülmesi için [desteğe başvurun.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)  |


#### <a name="dhcp-client-service-is-stopped-because-of-an-access-denied-error"></a>Erişim Reddedilen bir hata nedeniyle DHCP İstemci hizmeti durduruldu

1. Seri [Konsol'a](serial-console-windows.md) bağlanın ve bir PowerShell örneğini açın.
2. Aşağıdaki komut dosyasını çalıştırarak İşlem İzleme aracını indirin:

   ```powershell
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
4. **Access Reddedilen** iletisini oluşturan hizmeti başlatarak sorunu yeniden oluştur:

   ```
   sc start DHCP
   ```

   Başarısız olduğunda, İşlem İzi'ni sonlandırın:

   ```
   procmon /Terminate
   ```
5. **c:\temp\ProcMonTrace.PML** dosyasını toplayın:

    1. [VM'ye bir veri diski takın.](../windows/attach-managed-disk-portal.md
)
    2. Seri Konsol'u kullanarak dosyayı yeni sürücüye kopyalayabilirsiniz. Örneğin, `copy C:\temp\ProcMonTrace.PML F:\`. Bu komutta, F ekli veri diskinin sürücü harfidir. Harfi uygun şekilde doğru değerle değiştirin.
    3. Veri sürücüsünün ayrılmasını ve ardından işlem monitörü ubstakke yüklü olan çalışan bir VM'ye takın.

6. **ProcMonTrace.PML'yi** çalışan VM'de Process Monitor kullanarak açın. Daha sonra Sonuç tarafından filtre **ACCESS DENIED ,** aşağıdaki ekran görüntüsünde gösterildiği gibi:

    ![İşlem Monitörü'nde sonuca göre filtreleme](./media/troubleshoot-remote-desktop-services-issues/process-monitor-access-denined.png)

7. Kayıt defteri anahtarlarını, klasörleri veya çıktıdaki dosyaları düzeltin. Genellikle, bu sorun, hizmette kullanılan oturum açma hesabının bu nesnelere erişmek için ACL izni yoksa kaynaklanır. Oturum açma hesabı için doğru ACL iznini belirlemek için sağlıklı bir VM'yi kontrol edebilirsiniz.

#### <a name="dhcp-client-service-is-disabled"></a>DHCP İstemci hizmeti devre dışı

1. Hizmeti varsayılan başlangıç değerine geri yükleme:

   ```
   sc config DHCP start= auto
   ```

2. Hizmeti başlatın:

   ```
   sc start DHCP
   ```

3. Çalışırken olduğundan emin olmak için hizmet durumunu yeniden sorgula:

   ```
   sc query DHCP
   ```

4. Uzak Masaüstü'nü kullanarak VM'ye bağlanmayı deneyin.

#### <a name="dhcp-client-service-fails-because-of-logon-failure"></a>DHCP İstemci hizmeti oturum açma hatası nedeniyle başarısız olur

1. Bu hizmetin başlangıç hesabı değiştirildiyse bu sorun oluştuğundan, hesabı varsayılan durumuna geri döndürün:

        sc config DHCP obj= 'NT Authority\Localservice'
2. Hizmeti başlatın:

        sc start DHCP
3. Uzak Masaüstü'nü kullanarak VM'ye bağlanmayı deneyin.

#### <a name="dhcp-client-service-crashes-or-hangs"></a>DHCP İstemci hizmeti çöküyor veya asılı

1. Hizmet durumu **Başlangıç** veya **Durdurma** durumunda sıkışmışsa, hizmeti durdurmaya çalışın:

        sc stop DHCP
2. Hizmeti kendi 'svchost' kapsayıcısına ayırın:

        sc config DHCP type= own
3. Hizmeti başlatın:

        sc start DHCP
4. Hizmet hala başlatılamazsa, [desteğe başvurun.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)

### <a name="repair-the-vm-offline"></a>VM'yi çevrimdışı onar

#### <a name="attach-the-os-disk-to-a-recovery-vm"></a>Os diskini kurtarma VM'ine takın

1. [Os diskini kurtarma VM'sine takın.](../windows/troubleshoot-recovery-disks-portal.md)
2. Kurtarma VM'sine Uzak Masaüstü bağlantısı başlatın. Ekli diskin Disk Yönetimi konsolunda **Çevrimiçi** olarak işaretlendiğini unutmayın. Bağlı işletim sistemi diskine atanan sürücü mektubuna dikkat edin.
3.  Yükseltilmiş komut istemi örneğini açın (**Yönetici olarak çalıştırın).** Ardından aşağıdaki komut dosyasını çalıştırın. Bu komut dosyası, ekli işletim sistemi diskine atanan sürücü harfinin **F**olduğunu varsayar. Mektubu uygun şekilde VM'nizdeki değerle değiştirin.

    ```
    reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM

    REM Set default values back on the broken service
    reg add "HKLM\BROKENSYSTEM\ControlSet001\services\DHCP" /v start /t REG_DWORD /d 2 /f
    reg add "HKLM\BROKENSYSTEM\ControlSet001\services\DHCP" /v ObjectName /t REG_SZ /d "NT Authority\LocalService" /f
    reg add "HKLM\BROKENSYSTEM\ControlSet001\services\DHCP" /v type /t REG_DWORD /d 16 /f
    reg add "HKLM\BROKENSYSTEM\ControlSet002\services\DHCP" /v start /t REG_DWORD /d 2 /f
    reg add "HKLM\BROKENSYSTEM\ControlSet002\services\DHCP" /v ObjectName /t REG_SZ /d "NT Authority\LocalService" /f
    reg add "HKLM\BROKENSYSTEM\ControlSet002\services\DHCP" /v type /t REG_DWORD /d 16 /f

    reg unload HKLM\BROKENSYSTEM
    ```

4. [İşletim sistemi diskini ayırın ve VM'yi yeniden oluşturun.](../windows/troubleshoot-recovery-disks-portal.md) Ardından sorunun çözülüp çözülmediğini kontrol edin.

## <a name="next-steps"></a>Sonraki adımlar

Hala yardıma ihtiyacınız varsa, sorunuzun çözülmesi için [desteğe başvurun.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)