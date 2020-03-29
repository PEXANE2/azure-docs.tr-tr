---
title: Azure Veri Fabrikası'nda kendi barındırılan tümleştirme çalışma süresini giderme
description: Azure Veri Fabrikası'nda kendi barındırılan tümleştirme çalışma zamanı sorunlarını nasıl gidereceklerini öğrenin.
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 11/07/2019
ms.author: abnarain
ms.openlocfilehash: b8492e8934c782451fb77d5a0ff56b96c34c9a00
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75439884"
---
# <a name="troubleshoot-self-hosted-integration-runtime"></a>Sorun giderme kendi barındırılan tümleştirme çalışma zamanı

Bu makalede, Azure Veri Fabrikası'nda kendi kendine barındırılan tümleştirme çalışma zamanı için sık karşılaşılan sorun giderme yöntemleri incelenmektedir.

## <a name="common-errors-and-resolutions"></a>Genel sorunlar ve çözümleri

### <a name="error-message-self-hosted-integration-runtime-cant-connect-to-cloud-service"></a>Hata iletisi: Kendi kendine barındırılan tümleştirme çalışma zamanı bulut hizmetine bağlanamıyor

![Kendi barındırılan IR bağlantısı sorunu](media/self-hosted-integration-runtime-troubleshoot-guide/unable-to-connect-to-cloud-service.png)

#### <a name="cause"></a>Nedeni 

Kendi barındırılan tümleştirme çalışma süresi Veri Fabrikası hizmetine (arka uç) bağlanamaz. Bu sorun genellikle güvenlik duvarındaki ağ ayarlarını oluşturur.

#### <a name="resolution"></a>Çözüm

1. Tümleştirme çalışma zamanı hizmetinin çalışıp çalışmadığını denetleyin.
    
   ![Kendi kendine barındırılan IR hizmeti çalıştırma durumu](media/self-hosted-integration-runtime-troubleshoot-guide/integration-runtime-service-running-status.png)
    
1. Hizmet çalışıyorsa, adım 3'e geçin.

1. Kendi kendine barındırılan tümleştirme çalışma zamanında yapılandırılan proxy yoksa (varsayılan ayar dır), kendi kendine barındırılan tümleştirme çalışma süresinin yüklendiği makinede aşağıdaki PowerShell komutunu çalıştırın:

    ```powershell
    (New-Object System.Net.WebClient).DownloadString("https://wu2.frontend.clouddatahub.net/")
    ```
        
   > [!NOTE]     
   > Hizmet URL'si, Veri Fabrikası konumunuza bağlı olarak değişebilir. Hizmet URL'sini **ADF UI** > **Connections** > **Integration runtimes** > altında, Kendi**barındırılan IR** > **Düğümlerini** > **Görüntüleme Hizmeti URL'lerini**düzenle altında bulabilirsiniz.
            
    Beklenen yanıt aşağıdakigibidir:
            
    ![PowerShell komut yanıtı](media/self-hosted-integration-runtime-troubleshoot-guide/powershell-command-response.png)
            
1. Beklenen yanıtı almazsanız, durumunuza uygun olarak aşağıdaki yöntemlerden birini kullanın:
            
    * "Uzak ad çözülemedi" iletisi alırsanız, bir Etki Alanı Adı Sistemi (DNS) sorunu vardır. Bu sorunu gidermek için ağ ekibinize başvurun.
    * "SSL/tls sertifikasıgüvenilir değil" iletisi alırsanız, sertifikanın https://wu2.frontend.clouddatahub.net/ makinede güvenilir olup olmadığını kontrol edin ve sertifika yöneticisini kullanarak genel sertifikayı yükleyin. Bu eylem sorunu azaltmak gerekir.
    * **Windows** > **Event görüntüleyiciye (günlükler)** > **Uygulamalar ve Hizmetler Günlükleri** > **Tümleştirme Çalışma Zamanı'na** gidin ve DNS, güvenlik duvarı kuralı veya şirket ağ ayarlarından kaynaklanan hataları denetleyin. (Böyle bir hata bulursanız, bağlantıyı zorla kapatın.) Her şirket ağ ayarlarını özelleştirdiğinden, bu sorunları gidermek için ağ ekibinize başvurun.

1. Kendi kendine barındırılan tümleştirme çalışma zamanında "proxy" yapılandırıldıysa, proxy sunucunuzun hizmet bitiş noktasına erişebileceğini doğrulayın. Örnek bir komut için [PowerShell, web istekleri ve yakınlıklar'a](https://stackoverflow.com/questions/571429/powershell-web-requests-and-proxies)bakın.    
                
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

Beklenen yanıt aşağıdakigibidir:
            
![Powershell komut yanıtı 2](media/self-hosted-integration-runtime-troubleshoot-guide/powershell-command-response.png)

> [!NOTE] 
> Proxy hususlar:
> * Proxy sunucusunun Güvenli Alıcılar listesine alınması gerekip gerekmediğini kontrol edin. Bu nedenle, [bu etki alanlarının](https://docs.microsoft.com/azure/data-factory/data-movement-security-considerations#firewall-requirements-for-on-premisesprivate-network) Güvenli Alıcılar listesinde olduğundan emin olun.
> * TLS/SSL sertifikası "wu2.frontend.clouddatahub.net/" proxy sunucusunda güvenilir olup olmadığını kontrol edin.
> * Proxy'de Active Directory kimlik doğrulamasını kullanıyorsanız, hizmet hesabını proxy'ye erişebilecek kullanıcı hesabıyla değiştirin: "Tümleştirme Runtime Hizmeti."

### <a name="error-message-self-hosted-integration-runtime-node-logical-shir-is-in-inactive-running-limited-state"></a>Hata iletisi: Kendinden barındırılan tümleştirme çalışma zamanı düğümü/ mantıksal ŞIR Etkin değil/ "Running (Sınırlı)" durumunda

#### <a name="cause"></a>Nedeni 

Kendi barındırılan tümleşik çalışma zamanı düğümü, aşağıdaki ekran görüntüsünde gösterildiği gibi **Etkin olmayan** bir duruma sahip olabilir:

![Etkin olmayan Kendi Kendine Barındırılan IR düğümü](media/self-hosted-integration-runtime-troubleshoot-guide/inactive-self-hosted-ir-node.png)

Bu davranış, düğümler birbirleriyle iletişim kuramadığında oluşur.

#### <a name="resolution"></a>Çözüm

1. Düğüm barındırılan VM'de oturum açın. **Uygulamalar ve Hizmetler Günlükleri** > **Tümleştirme Çalışma Süresi**altında, Olay Görüntüleyicisi'ni açın ve tüm hata günlüklerini filtreleyin.

1. Hata günlüğünün aşağıdaki hatayı bulunup bulmadığını denetleyin: 
    
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
        
1.  Check whether the error log contains the following:

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
