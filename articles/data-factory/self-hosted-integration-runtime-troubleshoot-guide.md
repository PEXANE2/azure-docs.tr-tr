---
title: Azure Data Factory içinde şirket içinde barındırılan tümleştirme çalışma zamanı sorunlarını giderme
description: Azure Data Factory ' de şirket içinde barındırılan tümleştirme çalışma zamanı sorunlarını giderme hakkında bilgi edinin.
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 11/07/2019
ms.author: abnarain
ms.openlocfilehash: f27132eb21d245d0d26de910abba088ba3b8efde
ms.sourcegitcommit: 1692e86772217fcd36d34914e4fb4868d145687b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2020
ms.locfileid: "84170985"
---
# <a name="troubleshoot-self-hosted-integration-runtime"></a>Şirket içinde barındırılan tümleştirme çalışma zamanı sorunlarını giderme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede Azure Data Factory içindeki şirket içinde barındırılan tümleştirme çalışma zamanı için genel sorun giderme yöntemleri incelenmektedir.

## <a name="common-errors-and-resolutions"></a>Genel sorunlar ve çözümleri

### <a name="error-message-self-hosted-integration-runtime-cant-connect-to-cloud-service"></a>Hata iletisi: şirket içinde barındırılan tümleştirme çalışma zamanı bulut hizmetine bağlanamıyor

![Şirket içinde barındırılan IR bağlantı sorunu](media/self-hosted-integration-runtime-troubleshoot-guide/unable-to-connect-to-cloud-service.png)

#### <a name="cause"></a>Nedeni 

Şirket içinde barındırılan tümleştirme çalışma zamanı Data Factory hizmetine bağlanamaz (arka uç). Bu sorun genellikle güvenlik duvarındaki ağ ayarlarından kaynaklanır.

#### <a name="resolution"></a>Çözüm

1. Integration Runtime hizmetinin çalışıp çalışmadığını denetleyin.
    
   ![Şirket içinde barındırılan IR hizmeti çalıştırma durumu](media/self-hosted-integration-runtime-troubleshoot-guide/integration-runtime-service-running-status.png)
    
1. Hizmet çalışıyorsa Adım 3 ' e gidin.

1. Şirket içinde barındırılan tümleştirme çalışma zamanı (varsayılan ayar) üzerinde yapılandırılmış bir proxy yoksa, şirket içinde barındırılan tümleştirme çalışma zamanının yüklendiği makinede aşağıdaki PowerShell komutunu çalıştırın:

    ```powershell
    (New-Object System.Net.WebClient).DownloadString("https://wu2.frontend.clouddatahub.net/")
    ```
        
   > [!NOTE]     
   > Data Factory konumunuza bağlı olarak hizmet URL 'SI farklılık gösterebilir. Hizmet URL 'sini **ADF Kullanıcı arabirimi**  >  **bağlantıları**  >  **tümleştirme çalışma zamanları**altında bulabilirsiniz  >  **Şirket içinde barındırılan IR**  >  **düğümlerini**düzenleme  >  **hizmeti URL 'lerini**düzenleyin.
            
    Beklenen yanıt aşağıda verilmiştir:
            
    ![PowerShell komut yanıtı](media/self-hosted-integration-runtime-troubleshoot-guide/powershell-command-response.png)
            
1. Beklenen yanıtı almazsanız, durumunuza uygun şekilde aşağıdaki yöntemlerden birini kullanın:
            
    * "Uzak ad çözümlenemedi" iletisini alırsanız, bir etki alanı adı sistemi (DNS) sorunu vardır. Bu sorunu çözmesi için ağ ekibinize başvurun.
    * "SSL/TLS sertifikası güvenilir değil" iletisini alırsanız, için sertifikasının makinede güvenilir olup olmadığını denetleyin https://wu2.frontend.clouddatahub.net/ ve ardından Sertifika Yöneticisi 'ni kullanarak ortak sertifikayı yükler. Bu eylem sorunu azaltmalıdır.
    * **Windows**  >  **Olay Görüntüleyicisi (Günlükler)**  >  **uygulama ve hizmet günlükleri**  >  **Integration Runtime** gidin ve DNS, güvenlik duvarı kuralı veya şirket ağı ayarlarından kaynaklanan tüm hataları kontrol edin. (Böyle bir hata bulursanız bağlantıyı zorla kapatın.) Her şirket ağ ayarlarını özelleştirdiğinden, bu sorunları gidermek için ağ ekibinize başvurun.

1. Şirket içinde barındırılan tümleştirme çalışma zamanında "proxy" yapılandırılmışsa, proxy sunucunuzun hizmet uç noktasına erişebileceğini doğrulayın. Örnek bir komut için bkz. [PowerShell, Web istekleri ve proxy 'ler](https://stackoverflow.com/questions/571429/powershell-web-requests-and-proxies).    
                
    ```powershell
    $user = $env:username
    $webproxy = (get-itemproperty 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet
    Settings').ProxyServer
    $pwd = Read-Host "Password?" -assecurestring
    $proxy = new-object System.Net.WebProxy
    $proxy.Address = $webproxy
    $account = new-object System.Net.NetworkCredential($user,[Runtime.InteropServices.Marshal]::PtrToStringAuto([Runtime.InteropServices.Marshal]::SecureStringToBSTR($pwd)), "")
    $proxy.credentials = $account
    $url = "https://wu2.frontend.clouddatahub.net/"
    $wc = new-object system.net.WebClient
    $wc.proxy = $proxy
    $webpage = $wc.DownloadData($url)
    $string = [System.Text.Encoding]::ASCII.GetString($webpage)
    $string
    ```

Beklenen yanıt aşağıda verilmiştir:
            
![PowerShell komut yanıtı 2](media/self-hosted-integration-runtime-troubleshoot-guide/powershell-command-response.png)

> [!NOTE] 
> Ara sunucu konuları:
> *    Proxy sunucusunun Güvenli Alıcılar listesine alınması gerekip gerekmediğini denetleyin. Bu durumda, [Bu etki alanlarının](https://docs.microsoft.com/azure/data-factory/data-movement-security-considerations#firewall-requirements-for-on-premisesprivate-network) Güvenli Alıcılar listesinde olduğundan emin olun.
> *    "Wu2.frontend.clouddatahub.net/" TLS/SSL sertifikasının proxy sunucusunda güvenilir olup olmadığını denetleyin.
> *    Proxy üzerinde Active Directory kimlik doğrulaması kullanıyorsanız, hizmet hesabını "Integration Runtime hizmeti" olarak proxy 'ye erişebilen kullanıcı hesabı olarak değiştirin.

### <a name="error-message-self-hosted-integration-runtime-node-logical-shir-is-in-inactive-running-limited-state"></a>Hata iletisi: şirket içinde barındırılan tümleştirme çalışma zamanı düğümü/mantıksal dolgu, etkin olmayan/"çalışıyor (sınırlı)" durumunda

#### <a name="cause"></a>Nedeni 

Şirket içinde barındırılan tümleşik çalışma zamanı düğümü, aşağıdaki ekran görüntüsünde gösterildiği gibi **etkin olmayan** bir duruma sahip olabilir:

![Etkin olmayan şirket içinde barındırılan IR düğümü](media/self-hosted-integration-runtime-troubleshoot-guide/inactive-self-hosted-ir-node.png)

Bu davranış, düğümler birbirleriyle iletişim kuramıyorsa oluşur.

#### <a name="resolution"></a>Çözüm

1. Düğüm tarafından barındırılan VM 'de oturum açın. **Uygulama ve hizmet günlükleri**altında  >  **Integration Runtime**, Olay Görüntüleyicisi açın ve tüm hata günlüklerini filtreleyin.

1. Hata günlüğünde aşağıdaki hatayı içerip içermediğini kontrol edin: 
    
    ```
    System.ServiceModel.EndpointNotFoundException: Could not connect to net.tcp://xxxxxxx.bwld.com:8060/ExternalService.svc/WorkerManager. The connection attempt lasted for a time span of 00:00:00.9940994. TCP error code 10061: No connection could be made because the target machine actively refused it 10.2.4.10:8060. 
    System.Net.Sockets.SocketException: No connection could be made because the target machine actively refused it. 
    10.2.4.10:8060
    at System.Net.Sockets.Socket.DoConnect(EndPoint endPointSnapshot, SocketAddress socketAddress)
    at System.Net.Sockets.Socket.Connect(EndPoint remoteEP)
    at System.ServiceModel.Channels.SocketConnectionInitiator.Connect(Uri uri, TimeSpan timeout)
    ```
       
1. Bu hatayı görürseniz, komut satırında aşağıdakileri çalıştırın: 

   ```
   telnet 10.2.4.10 8060
   ```
   
1. Aşağıdaki hatayı alıyorsanız, bu sorunu gidermeye yönelik yardım için BT bölümünüze başvurun. Başarılı bir şekilde Telnet 'i başardıktan sonra, tümleştirme çalışma zamanı düğümü durumu ile ilgili sorunlar yaşıyorsanız Microsoft Desteği başvurun.
        
   ![Komut satırı hatası](media/self-hosted-integration-runtime-troubleshoot-guide/command-line-error.png)
        
1. Hata günlüğünde şunları içerip içermediğini kontrol edin:

    ```
    Error log: Cannot connect to worker manager: net.tcp://xxxxxx:8060/ExternalService.svc/ No DNS entries exist for host azranlcir01r1. No such host is known Exception detail: System.ServiceModel.EndpointNotFoundException: No DNS entries exist for host xxxxx. ---> System.Net.Sockets.SocketException: No such host is known at System.Net.Dns.GetAddrInfo(String name) at System.Net.Dns.InternalGetHostByName(String hostName, Boolean includeIPv6) at System.Net.Dns.GetHostEntry(String hostNameOrAddress) at System.ServiceModel.Channels.DnsCache.Resolve(Uri uri) --- End of inner exception stack trace --- Server stack trace: at System.ServiceModel.Channels.DnsCache.Resolve(Uri uri)
    ```
    
1. Sorunu çözmek için aşağıdaki yöntemlerden birini veya her ikisini deneyin:
    - Tüm düğümleri aynı etki alanına yerleştirin.
    - Barındırılan tüm VM 'nin ana bilgisayar dosyalarındaki ana bilgisayar eşlemesine IP 'yi ekleyin.


## <a name="next-steps"></a>Sonraki adımlar

Sorun giderme konusunda daha fazla yardım için aşağıdaki kaynakları deneyin:

*  [Data Factory blogu](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory Özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videoları](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Soru sayfası Microsoft Q&](https://docs.microsoft.com/answers/topics/azure-data-factory.html)
*  [Data Factory için yığın taşma Forumu](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Data Factory hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)
*  [Veri akışları eşleme performans Kılavuzu](concepts-data-flow-performance.md)
