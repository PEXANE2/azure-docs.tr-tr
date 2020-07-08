---
title: Azure Data Factory içinde şirket içinde barındırılan tümleştirme çalışma zamanı sorunlarını giderme
description: Azure Data Factory ' de şirket içinde barındırılan tümleştirme çalışma zamanı sorunlarını giderme hakkında bilgi edinin.
services: data-factory
author: nabhishek
ms.service: data-factory
ms.topic: troubleshooting
ms.date: 06/24/2020
ms.author: abnarain
ms.openlocfilehash: e77d621d5699c434e691de0a523e58e49166d8d6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85315141"
---
# <a name="troubleshoot-self-hosted-integration-runtime"></a>Şirket içinde barındırılan tümleştirme çalışma zamanı sorunlarını giderme

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede Azure Data Factory içindeki şirket içinde barındırılan tümleştirme çalışma zamanı için genel sorun giderme yöntemleri incelenmektedir.

## <a name="common-errors-and-resolutions"></a>Genel sorunlar ve çözümleri

### <a name="error-message"></a>Hata iletisi: 

`Self-hosted integration runtime can't connect to cloud service`

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

### <a name="error-message"></a>Hata iletisi: 
`Self-hosted integration runtime node/ logical SHIR is in Inactive/ "Running (Limited)" state`

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


## <a name="troubleshoot-connectivity-issue"></a>Bağlantı sorununu giderme

### <a name="troubleshoot-connectivity-issue-between-self-hosted-ir-and-data-factory-or-self-hosted-ir-and-data-sourcesink"></a>Şirket içinde barındırılan IR ve Data Factory veya şirket içinde barındırılan IR ve veri kaynağı/havuz arasında bağlantı sorununu giderme

Ağ bağlantısı sorununu gidermek için, [Ağ izlemesini nasıl toplayacağınızı](#how-to-collect-netmon-trace), nasıl kullanacağınızı anlamanız gerektiğini ve NetMon 'u şirket IÇINDE barındırılan IR 'den gerçek zamanlı olarak uygulamadan önce [Netmon izlemesini nasıl analiz](#how-to-analyze-netmon-trace) edeceğinizi bilmeniz gerekir.

Bazen, şirket içinde barındırılan IR ve Data Factory arasındaki bağlantı sorunlarını giderirken: 

![HTTP isteği başarısız oldu](media/self-hosted-integration-runtime-troubleshoot-guide/http-request-error.png)

Ya da şirket içinde barındırılan IR ve veri kaynağı/havuz arasında aşağıdaki hatalarla karşılaşacağız:

**Hata iletisi:**
`Copy failed with error:Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Cannot connect to SQL Server: ‘IP address’`

**Hata iletisi:**
`One or more errors occurred. An error occurred while sending the request. The underlying connection was closed: An unexpected error occurred on a receive. Unable to read data from the transport connection: An existing connection was forcibly closed by the remote host. An existing connection was forcibly closed by the remote host Activity ID.`

**Çözüm:** Yukarıdaki sorunlar hakkında daha fazla bilgi için aşağıdaki yönergelere bakın:

Netmon izlemesini alın ve daha fazla analiz edin.
- İlk olarak, bir filtreyi sunucudan istemci tarafına herhangi bir sıfırlamayı görmek için ayarlayabilirsiniz. Aşağıdaki örnekte, sunucu tarafının Data Factory sunucu olduğunu görebilirsiniz.

    ![Data Factory sunucusu](media/self-hosted-integration-runtime-troubleshoot-guide/data-factory-server.png)

- Sıfırlama paketini aldığınızda, TCP 'yi izleyerek konuşmayı bulabilirsiniz.

    ![Konuşmayı bul](media/self-hosted-integration-runtime-troubleshoot-guide/find-conversation.png)

- Daha sonra filtreyi kaldırarak istemci ve Data Factory sunucusu arasında dönüştürme yapabilirsiniz.

    ![Konuşmayı al](media/self-hosted-integration-runtime-troubleshoot-guide/get-conversation.png)
- Toplanan Netmon izlemesini temel alan TTL (TimeToLive) toplamının 64 olduğunu söyleyebilir. [Bu makalede](https://packetpushers.net/ip-time-to-live-and-hop-limit-basics/) BAHSEDILEN **varsayılan TTL ve atlama sınırı değerlerine** göre (aşağıda ayıklanan gibi), paketin sıfırlamasına ve bağlantısının kesilmesine neden olan Linux sistem olduğunu görebiliriz.

    Varsayılan TTL ve atlama sınırı değerleri, farklı işletim sistemleri arasında farklılık gösterir, aşağıda varsayılan değer verilmiştir:
    - Linux Kernel 2,4 (Circa 2001): TCP, UDP ve ıCMP için 255
    - Linux Kernel 4,10 (2015): TCP, UDP ve ıCMP için 64
    - Windows XP (2001): TCP, UDP ve ıCMP için 128
    - Windows 10 (2015): TCP, UDP ve ıCMP için 128
    - Windows Server 2008: TCP, UDP ve ıCMP için 128
    - Windows Server 2019 (2018): TCP, UDP ve ıCMP için 128
    - macOS (2001): TCP, UDP ve ıCMP için 64

    ![TTL 61](media/self-hosted-integration-runtime-troubleshoot-guide/ttl-61.png)
    
    Ancak, yukarıdaki örnekte 64 yerine 61 olarak gösterilir. Bu, ağ paketi hedefe ulaştığında yönlendiriciler/ağ cihazları gibi farklı atlamalar arasında geçmesi gerekir. Yönlendirici/ağ cihazlarının sayısı son TTL 'ye kesilecek.
    Bu durumda, sıfırlama 'nın TTL 64 ile Linux sisteminden gönderileolabileceğini görebiliriz.

- Sıfırlama cihazının nereden olabileceğini onaylamak için, kendinden konak IR 'den dördüncü atlamayı denetliyoruz.
 
    *Linux sistem A 'dan TTL 64 ile ağ paketi-> B TTL 64 eksi 1 = 63-> C TTL 63, eksi 1 = 62-> TTL 62 eksi 1 = 61 kendinden konak IR*

- İdeal durumda, TTL 128 olur, bu da Windows sisteminin Data Factory çalıştırdığı anlamına gelir. Aşağıdaki örnekte gösterildiği gibi, *128 – 107 = 21 atlamaları*, TCP 3 el sıkışması sırasında paketin Data Factory 21 ' den kendınden konak IR 'ye gönderildiği anlamına gelir.
 
    ![TTL 107](media/self-hosted-integration-runtime-troubleshoot-guide/ttl-107.png)

    Bu nedenle, dördüncü atlamanın kendinden konak IR 'den ne olduğunu denetlemek için ağ ekibine ihtiyacınız vardır. Bu, Linux sistemi olarak güvenlik duvarıdır, bu nedenle cihazın TCP 3 el sıkışması sonrasında paketi sıfırlamalarında herhangi bir günlüğe bakın. Ancak, araştırmanın nerede olduğundan emin değilseniz, bu paketi hangi cihazın sıfırlayabileceğinizi ve bağlantısının kesilmesine neden olduğunu anlamak için sorunlu süre içinde kendinden konak IR ve güvenlik duvarından alınan Netmon izlemesini almayı deneyin. Bu durumda, ileride ilerlemek için ağ ekibinize de ihtiyacınız vardır.

### <a name="how-to-collect-netmon-trace"></a>Netmon Trace nasıl toplanır

1.  [Bu Web sitesinden](https://www.microsoft.com/en-sg/download/details.aspx?id=4865)alınan Netmon araçlarını Indirin ve sunucu makinenize (sorunu yaşayan sunucu) ve istemciye (Şirket IÇINDE barındırılan IR gibi) yükleyin.

2.  Örneğin, şu yolda bir klasör oluşturun: *D:\netmon*. Günlüğü kaydetmek için yeterli alana sahip olduğundan emin olun.

3.  IP ve bağlantı noktası bilgilerini yakalayın. 
    1. Bir komut Istemi başlatın.
    2. Yönetici olarak Çalıştır ' ı seçin ve aşağıdaki komutu çalıştırın:
       
        ```
        Ipconfig /all >D:\netmon\IP.txt
        netstat -abno > D:\netmon\ServerNetstat.txt
        ```

4.  Netmon Trace 'i (ağ paketi) yakalayın.
    1. Bir komut Istemi başlatın.
    2. Yönetici olarak Çalıştır ' ı seçin ve aşağıdaki komutu çalıştırın:
        
        ```
        cd C:\Program Files\Microsoft Network Monitor 3
        ```
    3. Ağ sayfasını yakalamak için üç farklı komut kullanabilirsiniz:
        - Seçenek A: RoundRobin dosya komutu (Bu, yalnızca bir dosya yakalar ve eski günlüklerin üzerine yazacak).

            ```
            nmcap /network * /capture /file D:\netmon\ServerConnection.cap:200M
            ```         
        - Seçenek B: zincirleme dosya komutu (200 MB ulaşıldığında bu yeni dosya oluşturur).
        
            ```
            nmcap /network * /capture /file D:\netmon\ServerConnection.chn:200M
            ```          
        - Seçenek C: zamanlanmış dosya komutu.

            ```
            nmcap /network * /capture /StartWhen /Time 10:30:00 AM 10/28/2011 /StopWhen /Time 11:30:00 AM 10/28/2011 /file D:\netmon\ServerConnection.chn:200M
            ```  

5.  Netmon Trace 'i yakalamayı durdurmak için **CTRL + C** tuşlarına basın.
 
> [!NOTE]
> İstemci makinesinde yalnızca Netmon izlemesini toplayabiliyorsanız, izlemeyi analiz etmenize yardımcı olması için sunucu IP adresini alın.

### <a name="how-to-analyze-netmon-trace"></a>Netmon izlemesini çözümleme

Telnet **8.8.8.8 888** ' i yukarıdaki Netmon izleme toplanmış olarak çalıştırdığınızda, izlemeyi aşağıda görmeniz gerekir:

![Netmon izleme 1](media/self-hosted-integration-runtime-troubleshoot-guide/netmon-trace-1.png)

![Netmon Trace 2](media/self-hosted-integration-runtime-troubleshoot-guide/netmon-trace-2.png)
 

Bu, **888**numaralı bağlantı noktasına bağlı olarak **8.8.8.8** sunucu tarafında TCP bağlantısı yapamayacağı anlamına gelir; bu nedenle, burada iki **synyeniden aktarım** ek paketi görürsünüz. Kaynak **self-konak2** , ilk pakette **8.8.8.8** ile bağlantı kuramadı, çünkü bağlantı kurmak için devam edecektir.

> [!TIP]
> - **Yükleme filtresi**  ->  **Standart filtre**  ->  **adresleri**  ->  **IPv4 adresleri**' ne tıklayabilirsiniz.
> - Giriş **IPv4. Address = = 8.8.8.8** as Filter ve **Uygula**' ya tıklayın. Bundan sonra, yalnızca yerel makineden hedef **8.8.8.8**iletişimi görürsünüz.

![adresleri filtrele 1](media/self-hosted-integration-runtime-troubleshoot-guide/filter-addresses-1.png)
        
![adresleri filtrele 2](media/self-hosted-integration-runtime-troubleshoot-guide/filter-addresses-2.png)

Aşağıda, iyi bir senaryonun nasıl görüneceğine ilişkin bir örnek gösterilmektedir. 

- Telnet **8.8.8.8 53** herhangi bir sorun olmadan düzgün ÇALıŞıYORSA, TCP 3 el sıkışma olduğunu görebilir ve oturum, TCP 4 el sıkışma ile biter.

    ![iyi senaryo örneği 1](media/self-hosted-integration-runtime-troubleshoot-guide/good-scenario-1.png)
     
    ![iyi senaryo örneği 2](media/self-hosted-integration-runtime-troubleshoot-guide/good-scenario-2.png)

- Yukarıdaki TCP 3 el sıkışma temelinde, aşağıdaki iş akışı ' na bakabilirsiniz:

    ![TCP 3 el sıkışma iş akışı](media/self-hosted-integration-runtime-troubleshoot-guide/tcp-3-handshake-workflow.png)
 
- Oturumu tamamlaması için TCP 4 el sıkışması ve iş akışı aşağıdaki gibi gösterilir:

    ![TCP 4 el sıkışma](media/self-hosted-integration-runtime-troubleshoot-guide/tcp-4-handshake.png)

    ![TCP 4 el sıkışma iş akışı](media/self-hosted-integration-runtime-troubleshoot-guide/tcp-4-handshake-workflow.png) 


## <a name="next-steps"></a>Sonraki adımlar

Sorun giderme konusunda daha fazla yardım için aşağıdaki kaynakları deneyin:

*  [Data Factory blogu](https://azure.microsoft.com/blog/tag/azure-data-factory/)
*  [Data Factory Özellik istekleri](https://feedback.azure.com/forums/270578-data-factory)
*  [Azure videoları](https://azure.microsoft.com/resources/videos/index/?sort=newest&services=data-factory)
*  [Soru sayfası Microsoft Q&](https://docs.microsoft.com/answers/topics/azure-data-factory.html)
*  [Data Factory için yığın taşma Forumu](https://stackoverflow.com/questions/tagged/azure-data-factory)
*  [Data Factory hakkında Twitter bilgileri](https://twitter.com/hashtag/DataFactory)
*  [Veri akışları eşleme performans Kılavuzu](concepts-data-flow-performance.md)
