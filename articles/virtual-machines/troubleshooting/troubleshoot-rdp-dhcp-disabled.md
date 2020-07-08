---
title: DHCP devre dışı bırakıldığından Azure sanal makinelerine uzaktan bağlanılamıyor | Microsoft Docs
description: Microsoft Azure 'de DHCP istemci hizmetinin neden devre dışı bırakıldığını gösteren RDP sorunuyla ilgili sorunları nasıl giderebileceğinizi öğrenin. | Microsoft Docs
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
ms.openlocfilehash: 278d976f044deb8a7387763306cf07f8b6b55d90
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86087801"
---
#  <a name="cannot-rdp-to-azure-virtual-machines-because-the-dhcp-client-service-is-disabled"></a>DHCP Istemci hizmeti devre dışı bırakıldığından Azure sanal makinelerinde RDP yapılamıyor

Bu makalede, DHCP Istemci hizmeti VM 'de devre dışı bırakıldıktan sonra Azure Windows Sanal Makineleri (VM) ile uzak masaüstü 'nü bir sorunla ilgili bir sorun açıklanmaktadır.


## <a name="symptoms"></a>Belirtiler
DHCP Istemci hizmeti VM 'de devre dışı bırakıldığından Azure 'da bir RDP bağlantısı yapamazsınız. Azure portal [önyükleme tanılamasında](../troubleshooting/boot-diagnostics.md) ekran görüntüsünü DENETLEDIĞINIZDE, VM 'nin normal şekilde önyüklenmesini ve oturum açma ekranında kimlik bilgileri beklediğini görürsünüz. Olay Görüntüleyicisi kullanarak VM 'deki olay günlüklerini uzaktan görüntüleyebilirsiniz. DHCP Istemci hizmetinin başlatıldığını veya başlayalamayacağını görürsünüz. Aşağıdaki örnek günlük:

**Günlük adı**: sistem </br>
**Kaynak**: hizmet denetimi Yöneticisi </br>
**Tarih**: 12/16/2015 11:19:36 </br>
**Olay kimliği**: 7022 </br>
**Görev kategorisi**: yok </br>
**Düzey**: hata </br>
**Anahtar sözcükler**: klasik</br>
**Kullanıcı**: yok </br>
**Bilgisayar**: myvm.cosotos.com</br>
**Açıklama**: DHCP istemci hizmeti başlatılırken askıda kaldı.</br>

Kaynak Yöneticisi VM 'Ler için, aşağıdaki komutu kullanarak olay günlüklerini 7022 sorgulamak üzere seri erişim konsolu özelliğini kullanabilirsiniz:

```console
wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Service Control Manager'] and EventID=7022 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more
```

Klasik VM 'Ler için ÇEVRIMDıŞı modda çalışmanız ve günlükleri el ile toplamanız gerekecektir.

## <a name="cause"></a>Nedeni

DHCP Istemci hizmeti VM üzerinde çalışmıyor.

> [!NOTE]
> Bu makale yalnızca DHCP Istemci hizmeti için geçerlidir ve DHCP sunucusu için geçerli değildir.

## <a name="solution"></a>Çözüm

Bu adımları izlemeden önce, etkilenen VM 'nin işletim sistemi diskinin bir yedek olarak anlık görüntüsünü alın. Daha fazla bilgi için bkz. [disk anlık görüntüsü](../windows/snapshot-copy-managed-disk.md).

Bu sorunu çözmek için seri denetim 'i kullanarak VM için DHCP 'yi etkinleştirin veya [ağ arabirimini sıfırlayın](reset-network-interface.md) .

### <a name="use-serial-control"></a>Seri denetim kullan

1. [Seri konsoluna bağlanın ve cmd örneğini açın](serial-console-windows.md#use-cmd-or-powershell-in-serial-console).
). VM 'niz üzerinde seri konsol etkinleştirilmemişse, bkz. [ağ arabirimini sıfırlama](reset-network-interface.md).
2. Ağ arabiriminde DHCP 'nin devre dışı olup olmadığını denetleyin:

    ```console
    sc query DHCP
    ```

3. DHCP durdurulmuşsa, hizmeti başlatmayı deneyin

    ```console
    sc start DHCP
    ```

4. Hizmetin başarıyla başlatıldığından emin olmak için hizmeti tekrar sorgulayın.

    ```console
    sc query DHCP
    ```

    SANAL makineye bağlanmayı deneyin ve sorunun çözümlenip çözümlenmediğini görün.
5. Hizmet başlamazsa, aldığınız hata iletisine bağlı olarak aşağıdaki uygun çözümü kullanın:

    | Hata  |  Çözüm |
    |---|---|
    | 5-ERIŞIM ENGELLENDI  | [Erişim reddedildi hatası nedeniyle bkz. DHCP istemci hizmeti durduruldu](#dhcp-client-service-is-stopped-because-of-an-access-denied-error).  |
    |1053-ERROR_SERVICE_REQUEST_TIMEOUT   | Bkz. [DHCP istemci hizmeti kilitleniyor veya askıda kalıyor](#dhcp-client-service-crashes-or-hangs).  |
    | 1058-ERROR_SERVICE_DISABLED  | Bkz. [DHCP istemci hizmeti devre dışı](#dhcp-client-service-is-disabled).  |
    | 1059-ERROR_CIRCULAR_DEPENDENCY  |Sorun hızla çözülebilmenizi sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .   |
    | 1067-ERROR_PROCESS_ABORTED |Bkz. [DHCP istemci hizmeti kilitleniyor veya askıda kalıyor](#dhcp-client-service-crashes-or-hangs).   |
    |1068-ERROR_SERVICE_DEPENDENCY_FAIL   | Sorun hızla çözülebilmenizi sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .  |
    |1069-ERROR_SERVICE_LOGON_FAILED   |  Bkz. [oturum açma hatası nedeniyle DHCP istemci hizmeti başarısız oldu](#dhcp-client-service-fails-because-of-logon-failure) |
    | 1070-ERROR_SERVICE_START_HANG  | Bkz. [DHCP istemci hizmeti kilitleniyor veya askıda kalıyor](#dhcp-client-service-crashes-or-hangs).  |
    | 1077-ERROR_SERVICE_NEVER_STARTED  | Bkz. [DHCP istemci hizmeti devre dışı](#dhcp-client-service-is-disabled).  |
    |1079-ERROR_DIFERENCE_SERVICE_ACCOUNT   | Sorun hızla çözülebilmenizi sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .  |
    |1053 | Sorun hızla çözülebilmenizi sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .  |


#### <a name="dhcp-client-service-is-stopped-because-of-an-access-denied-error"></a>Erişim reddedildi hatası nedeniyle DHCP Istemci hizmeti durduruldu

1. [Seri konsoluna](serial-console-windows.md) bağlanın ve bir PowerShell örneği açın.
2. Aşağıdaki betiği çalıştırarak Işlem Izleyicisi aracını indirin:

   ```powershell
   remove-module psreadline
   $source = "https://download.sysinternals.com/files/ProcessMonitor.zip"
   $destination = "c:\temp\ProcessMonitor.zip"
   $wc = New-Object System.Net.WebClient
   $wc.DownloadFile($source,$destination)
   ```
3. Şimdi bir **procmon** izlemesi başlatın:

   ```
   procmon /Quiet /Minimized /BackingFile c:\temp\ProcMonTrace.PML
   ```
4. **Erişim reddedildi** iletisini üreten hizmeti başlatarak sorunu yeniden üretin:

   ```
   sc start DHCP
   ```

   Başarısız olduğunda, Işlem Izleyicisi izlemesini sonlandırın:

   ```
   procmon /Terminate
   ```
5. **C:\temp\procmontrace. PML** dosyasını toplayın:

    1. [Sanal makineye bir veri diski ekleyin](../windows/attach-managed-disk-portal.md
).
    2. Seri konsolu 'Nu kullanarak dosyayı yeni sürücüye kopyalayabilirsiniz. Örneğin, `copy C:\temp\ProcMonTrace.PML F:\`. Bu komutta, F, ekli veri diskinin sürücü harfidir. Harfi doğru değer ile uygun şekilde değiştirin.
    3. Veri sürücüsünü ayırın ve sonra Işlem Izleyicisi ubstakke yüklü olan çalışan bir sanal makineye ekleyin.

6. Çalışan VM 'de Işlem Izleyicisi 'ni kullanarak **Procmontrace. PML** dosyasını açın. Ardından, aşağıdaki ekran görüntüsünde gösterildiği gibi, sonuca göre filtrele **ERIŞIMI reddedildi**:

    ![Işlem Izleyicisinde sonuca göre filtrele](./media/troubleshoot-remote-desktop-services-issues/process-monitor-access-denined.png)

7. Kayıt defteri anahtarlarını, klasörlerini veya çıktıda bulunan dosyaları onarın. Genellikle bu sorun, hizmette kullanılan oturum açma hesabının bu nesnelere erişmek için ACL iznine sahip olmaması nedeniyle oluşur. Oturum açma hesabına yönelik doğru ACL iznini öğrenmek için sağlıklı bir VM 'yi kontrol edebilirsiniz.

#### <a name="dhcp-client-service-is-disabled"></a>DHCP Istemci hizmeti devre dışı

1. Hizmeti varsayılan başlangıç değerine geri yükleyin:

   ```
   sc config DHCP start= auto
   ```

2. Hizmeti başlatın:

   ```
   sc start DHCP
   ```

3. Çalışır durumda olduğundan emin olmak için hizmet durumunu yeniden sorgulayın:

   ```
   sc query DHCP
   ```

4. Uzak Masaüstü kullanarak sanal makineye bağlanmayı deneyin.

#### <a name="dhcp-client-service-fails-because-of-logon-failure"></a>DHCP Istemci hizmeti, oturum açma hatası nedeniyle başarısız oluyor

1. Bu sorun, bu hizmetin başlangıç hesabı değiştiyse, hesabı varsayılan durumuna döndürür:

    ```console
    sc config DHCP obj= 'NT Authority\Localservice'
    ```

2. Hizmeti başlatın:

    ```console
    sc start DHCP
    ```

3. Uzak Masaüstü kullanarak sanal makineye bağlanmayı deneyin.

#### <a name="dhcp-client-service-crashes-or-hangs"></a>DHCP Istemci hizmeti kilitleniyor veya askıda kalıyor

1. Hizmet durumu **Başlangıç** veya **durdurma** durumunda takılırsa, hizmeti durdurmayı deneyin:

    ```console
    sc stop DHCP
    ```

2. Hizmeti kendi ' Svchost ' kapsayıcısında yalıtın:

    ```console
    sc config DHCP type= own
    ```

3. Hizmeti başlatın:

    ```console
    sc start DHCP
    ```

4. Hizmet yine başlamazsa, [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="repair-the-vm-offline"></a>VM 'yi çevrimdışı onarma

#### <a name="attach-the-os-disk-to-a-recovery-vm"></a>İşletim sistemi diskini bir kurtarma VM 'sine iliştirme

1. [İşletim sistemi diskini bir kurtarma sanal makinesine ekleyin](../windows/troubleshoot-recovery-disks-portal.md).
2. Kurtarma VM 'sine bir Uzak Masaüstü bağlantısı başlatın. Disk Yönetimi konsolunda, eklenen diskin **çevrimiçi** olarak işaretlendiğinden emin olun. Bağlı işletim sistemi diskine atanan sürücü harfini unutmayın.
3.  Yükseltilmiş bir komut istemi örneği açın (**yönetici olarak çalıştır**). Ardından aşağıdaki betiği çalıştırın. Bu betik, bağlı işletim sistemi diskine atanan sürücü harfinin **F**olduğunu varsayar. Harfi, sanal makinenizin değeri ile uygun şekilde değiştirin.

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

4. [İşletim sistemi diskini ayırın ve VM 'yi yeniden oluşturun](../windows/troubleshoot-recovery-disks-portal.md). Sonra sorunun çözümlenip çözümlenmediğini denetleyin.

## <a name="next-steps"></a>Sonraki adımlar

Hala yardıma ihtiyacınız varsa, sorununuzun çözülmesi için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .