---
title: Azure Data Factory içinde şirket içinde barındırılan tümleştirme çalışma zamanı sorunlarını giderme
description: Azure Data Factory ' de şirket içinde barındırılan tümleştirme çalışma zamanı sorunlarını giderme hakkında bilgi edinin.
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 11/07/2019
ms.author: abnarain
ms.openlocfilehash: f298b331d53eb8bab67a6f99194065dc5f889236
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81414885"
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
   > Data Factory konumunuza bağlı olarak hizmet URL 'SI farklılık gösterebilir. Hizmet URL 'sini **ADF Kullanıcı arabirimi** > **bağlantıları** > **tümleştirme çalışma zamanları** > altında bulabilirsiniz**Şirket içinde barındırılan IR** > **düğümlerini** > düzenleme**hizmeti URL 'lerini**düzenleyin.
            
    Beklenen yanıt aşağıda verilmiştir:
            
    ![PowerShell komut yanıtı](media/self-hosted-integration-runtime-troubleshoot-guide/powershell-command-response.png)
            
1. Beklenen yanıtı almazsanız, durumunuza uygun şekilde aşağıdaki yöntemlerden birini kullanın:
            
    * "Uzak ad çözümlenemedi" iletisini alırsanız, bir etki alanı adı sistemi (DNS) sorunu vardır. Bu sorunu çözmesi için ağ ekibinize başvurun.
    * "SSL/TLS sertifikası güvenilir değil" iletisini alırsanız, için https://wu2.frontend.clouddatahub.net/ sertifikasının makinede güvenilir olup olmadığını denetleyin ve ardından Sertifika Yöneticisi 'ni kullanarak ortak sertifikayı yükler. Bu eylem sorunu azaltmalıdır.
    * **Windows** > **Olay Görüntüleyicisi (Günlükler)** > **uygulama ve hizmet günlükleri** > **Integration Runtime** gidin ve DNS, güvenlik duvarı kuralı veya şirket ağı ayarlarından kaynaklanan tüm hataları kontrol edin. (Böyle bir hata bulursanız bağlantıyı zorla kapatın.) Her şirket ağ ayarlarını özelleştirdiğinden, bu sorunları gidermek için ağ ekibinize başvurun.

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

1. Düğüm tarafından barındırılan VM 'de oturum açın. **Uygulama ve hizmet günlükleri** > altında**Integration Runtime**, Olay Görüntüleyicisi açın ve tüm hata günlüklerini filtreleyin.

1. Hata günlüğünde aşağıdaki hatayı içerip içermediğini kontrol edin: 
    
    ```System.ServiceModel.EndpointNotFoundException: Could not connect to net.tcp://xxxxxxx.bwld.com:8060/ExternalService.svc/WorkerManager. The connection attempt lasted for a time span of 00:00:00.9940994. TCP error code 10061: No connection could be made because the target machine actively refused it 10.2.4.10:8060. 
    System.Net.Sockets.SocketException: No connection could be made because the target machine actively refused it. 
    10.2.4.10:8060
        
    at System.Net.Sockets.Socket.DoConnect(EndPoint endPointSnapshot, SocketAddress socketAddress)
               
    at System.Net.Sockets.Socket.Connect(EndPoint remoteEP)
               
    at System.ServiceModel.Channels.SocketConnectionInitiator.Connect(Uri uri, TimeSpan timeout)
       
1. If you see this error, run the following on a command line: 

   **telnet 10.2.4.10 8060**.
1. If you receive the following error, contact your IT department for help with fixing this issue. After you can successfully telnet, contact Microsoft Support if you still have issues with the integrative runtime node status.
        
   ![Command-line error](media/self-hosted-integration-runtime-troubleshoot-guide/command-line-error.png)
        
1.    Check whether the error log contains the following:

    ```Error log: Cannot connect to worker manager: net.tcp://xxxxxx:8060/ExternalService.svc/ No DNS entries exist for host azranlcir01r1. No such host is known Exception detail: System.ServiceModel.EndpointNotFoundException: No DNS entries exist for host xxxxx. ---> System.Net.Sockets.SocketException: No such host is known at System.Net.Dns.GetAddrInfo(String name) at System.Net.Dns.InternalGetHostByName(String hostName, Boolean includeIPv6) at System.Net.Dns.GetHostEntry(String hostNameOrAddress) at System.ServiceModel.Channels.DnsCache.Resolve(Uri uri) --- End of inner exception stack trace --- Server stack trace: at System.ServiceModel.Channels.DnsCache.Resolve(Uri uri)```
    
1. To resolve the issue, try one or both of the following methods:
    - Put all the nodes in the same domain.
    - Add the IP to host mapping in all the hosted VM's host files.


## Next steps

For more help with troubleshooting, try the following resources:

*  [Data Factory blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory feature requests](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videos](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [MSDN forum](https://social.msdn.microsoft.com/Forums/home?sort=relevancedesc&brandIgnore=True&searchTerm=data+factory)
*  [Stack overflow forum for Data Factory](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Twitter information about Data Factory](https://twitter.com/hashtag/DataFactory)
*  [Mapping data flows performance guide](concepts-data-flow-performance.md)
