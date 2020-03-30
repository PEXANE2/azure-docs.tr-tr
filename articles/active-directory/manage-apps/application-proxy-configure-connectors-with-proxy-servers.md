---
title: Mevcut şirket içi proxy sunucuları ve Azure AD | Microsoft Dokümanlar
description: Varolan şirket içi proxy sunucularıyla nasıl çalışılabildiğini kapsar.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5fe3a63e119fed6825982b9de13bc78cb7da5415
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481407"
---
# <a name="work-with-existing-on-premises-proxy-servers"></a>Varolan şirket içi proxy sunucularıyla çalışma

Bu makalede, giden proxy sunucuları ile çalışmak için Azure Active Directory (Azure AD) Application Proxy bağlayıcıları nasıl yapılandırılabilen ler açıklanmaktadır. Varolan yakınlıkları olan ağ ortamları olan müşteriler için tasarlanmıştır.

Bu ana dağıtım senaryolarına bakarak başlıyoruz:

* Konektörleri şirket içi giden vekillerinizi atlayacak şekilde yapılandırın.
* Azure AD Application Proxy'ye erişmek için giden proxy kullanacak şekilde bağlayıcıları yapılandırın.

Bağlayıcıların nasıl çalıştığı hakkında daha fazla bilgi için [bkz.](application-proxy-connectors.md)

## <a name="bypass-outbound-proxies"></a>Çıkış lı yakınlıkları bypass

Bağlayıcılar, giden isteklerde yatan temel işletim sistemi bileşenlerine sahiptir. Bu bileşenler otomatik olarak Web Proxy Auto-Discovery (WPAD) kullanarak ağ üzerinde bir proxy sunucusu bulmaya çalışır.

İşletim sistemi bileşenleri wpad.domainsonefix için bir DNS araması gerçekleştirerek bir proxy sunucusu bulmaya çalışır. Arama DNS'de çözülürse, wpad.dat'ın IP adresine bir HTTP isteği yapılır. Bu istek, ortamınızdaki proxy yapılandırma komut dosyası olur. Bağlayıcı, giden bir proxy sunucusu seçmek için bu komut dosyasını kullanır. Ancak, proxy üzerinde gerekli ek yapılandırma ayarları nedeniyle bağlayıcı trafiği yine de geçmeyebilir.

Konektörü, Azure hizmetlerine doğrudan bağlantı sağladığından emin olmak için şirket içi proxy'nizi atlayacak şekilde yapılandırabilirsiniz. Bu yaklaşımı, ağ politikanız izin verdiği sürece öneririz, çünkü bu, korumanız gereken bir yapılandırmanız daha az olduğu anlamına gelir.

Bağlayıcı için giden proxy kullanımını devre dışı katmak için, C:\Program Files\Microsoft AAD App Proxy Bağlayıcısı\ApplicationProxyConnectorService.exe.config dosyasını düzenlediniz ve bu kod örneğinde gösterilen *system.net* bölümü ekleyin:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.net>
    <defaultProxy enabled="false"></defaultProxy>
  </system.net>
  <runtime>
    <gcServer enabled="true"/>
  </runtime>
  <appSettings>
    <add key="TraceFilename" value="AadAppProxyConnector.log" />
  </appSettings>
</configuration>
```

Bağlayıcı Güncelleyici hizmetinin proxy'yi de atlamasını sağlamak için ApplicationProxyConnectorUpdaterService.exe.config dosyasında benzer bir değişiklik yapın. Bu dosya C:\Program Files\Microsoft AAD App Proxy Bağlayıcısı'nda bulunur.

Varsayılan .config dosyalarına geri dönmek zorunda ysanız, orijinal dosyaların kopyalarını yaptığınızdan emin olun.

## <a name="use-the-outbound-proxy-server"></a>Giden proxy sunucusunu kullanma

Bazı ortamlar, istisnasız, giden bir proxy üzerinden gitmek için tüm giden trafik gerektirir. Sonuç olarak, proxy atlayarak bir seçenek değildir.

Aşağıdaki diyagramda gösterildiği gibi, bağlayıcı trafiğini giden proxy'den geçecek şekilde yapılandırabilirsiniz:

 ![Bağlayıcı trafiğini Azure AD Application Proxy'ye giden bir proxy üzerinden geçecek şekilde yapılandırma](./media/application-proxy-configure-connectors-with-proxy-servers/configure-proxy-settings.png)

Yalnızca giden trafiğe sahip olmanın bir sonucu olarak, gelen erişimi güvenlik duvarlarınızdan yapılandırmanıza gerek yoktur.

> [!NOTE]
> Uygulama Proxy diğer proxy'ler için kimlik doğrulaması desteklemez. Bağlayıcı/güncelleyici ağ hizmeti hesapları, kimlik doğrulaması için itiraz edilmeden proxy'ye bağlanabilmeli.

### <a name="step-1-configure-the-connector-and-related-services-to-go-through-the-outbound-proxy"></a>Adım 1: Giden proxy üzerinden gitmek için bağlayıcı ve ilgili hizmetleri yapılandırma

WPAD ortamda etkinleştirilir ve uygun şekilde yapılandırılırsa, bağlayıcı giden proxy sunucusunu otomatik olarak keşfeder ve kullanmaya çalışır. Ancak, bağlayıcıyı giden bir proxy'den geçecek şekilde açıkça yapılandırabilirsiniz.

Bunu yapmak için C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config dosyasını düzenlediniz ve bu kod örneğinde gösterilen *system.net* bölümü ekleyin. *Proxyserver:8080'i* yerel proxy sunucu adınızı veya IP adresinizi ve dinlediği bağlantı noktasını yansıtacak şekilde değiştirin. Bir IP adresi kullanıyor sanız bile değerin öneki http:// olmalıdır.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.net>  
    <defaultProxy>   
      <proxy proxyaddress="http://proxyserver:8080" bypassonlocal="True" usesystemdefault="True"/>   
    </defaultProxy>  
  </system.net>
  <runtime>
    <gcServer enabled="true"/>
  </runtime>
  <appSettings>
    <add key="TraceFilename" value="AadAppProxyConnector.log" />
  </appSettings>
</configuration>
```

Ardından, C:\Program Files\Microsoft AAD App Proxy Connector Updater\ApplicationProxyConnectorUpdaterService.exe.config dosyasında benzer bir değişiklik yaparak proxy'yi kullanmak üzere Bağlayıcı Güncelleyici hizmetini yapılandırın.

### <a name="step-2-configure-the-proxy-to-allow-traffic-from-the-connector-and-related-services-to-flow-through"></a>Adım 2: Proxy'yi konektörden ve ilgili hizmetlerden gelen trafiğin akmasına izin verecek şekilde yapılandırın

Giden proxy göz önünde bulundurulması gereken dört yönü vardır:

* Proxy giden kurallar
* Proxy kimlik doğrulaması
* Proxy bağlantı noktaları
* TLS denetimi

#### <a name="proxy-outbound-rules"></a>Proxy giden kurallar

Aşağıdaki URL'lere erişime izin verin:

| URL'si | Nasıl kullanılır? |
| --- | --- |
| \*.msappproxy.net<br>\*.servicebus.windows.net | Bağlayıcı ve Uygulama Proxy bulut hizmeti arasındaki iletişim |
| mscrl.microsoft.com:80<br>crl.microsoft.com:80<br>ocsp.msocsp.com:80<br>www.microsoft.com:80 | Bağlayıcı, sertifikaları doğrulamak için bu URL'leri kullanır |
| login.windows.net<br>secure.aadcdn.microsoftonline-p.com<br>*.microsoftonline.com<br>*.microsoftonline-p.com<br>*.msauth.net<br>*.msauthimages.net<br>*.msecnd.net<br>*.msftauth.net<br>*.msftauthimages.net<br>*.phonefactor.net<br>enterpriseregistration.windows.net<br>management.azure.com<br>policykeyservice.dc.ad.msft.net<br>ctdl.windowsupdate.com:80 | Bağlayıcı, kayıt işlemi sırasında bu URL'leri kullanır. |

Güvenlik duvarınız veya proxy'niz DNS izin listelerini yapılandırmanıza \*izin veriyorsa, .msappproxy.net ve \*.servicebus.windows.net bağlantılarına izin verebilirsiniz. Değilse, [Azure DataCenter IP aralıklarına](https://www.microsoft.com/download/details.aspx?id=41653)erişime izin verebilirsiniz. IP aralıkları her hafta güncellenir.

FQDN ile bağlantıya izin veramıyorsanız ve bunun yerine IP aralıklarını belirtmeniz gerekiyorsa, aşağıdaki seçenekleri kullanın:

* Konektörün tüm hedeflere giden erişimine izin verin.
* Bağlayıcının tüm Azure veri merkezi [IP aralıklarına](https://www.microsoft.com//download/details.aspx?id=41653)giden erişimine izin verin. Azure veri merkezi IP aralıkları listesini kullanmanın zorluğu haftalık olarak güncellenmesidir. Erişim kurallarınızın buna göre güncelleştirdiğinden emin olmak için bir işlem yapmanız gerekir. Yalnızca IP adreslerinin bir alt kümesini kullanmak yapılandırmanızın kırılmasına neden olabilir.

#### <a name="proxy-authentication"></a>Proxy kimlik doğrulaması

Proxy kimlik doğrulaması şu anda desteklenmiyor. Şu anki tavsiyemiz, konektörün Internet hedeflerine anonim olarak erişmesine izin vermektir.

#### <a name="proxy-ports"></a>Proxy bağlantı noktaları

Bağlayıcı, CONNECT yöntemini kullanarak giden TLS tabanlı bağlantılar yapar. Bu yöntem aslında giden proxy üzerinden bir tünel kurar. Proxy sunucusunu 443 ve 80 bağlantı noktalarına tünel açmaya izin verecek şekilde yapılandırın.

> [!NOTE]
> Servis Veri Servisi HTTPS üzerinden çalıştığında, 443 portu kullanır. Ancak, varsayılan olarak, Service Bus doğrudan TCP bağlantılarını dener ve yalnızca doğrudan bağlantı başarısız olursa HTTPS'ye geri döner.

#### <a name="tls-inspection"></a>TLS denetimi

Bağlayıcı trafiği için sorunlara neden olduğundan, bağlayıcı trafiği için TLS denetimi kullanmayın. Bağlayıcı, Uygulama Proxy hizmetine kimlik doğrulamak için bir sertifika kullanır ve bu sertifika TLS denetimi sırasında kaybolabilir.

## <a name="troubleshoot-connector-proxy-problems-and-service-connectivity-issues"></a>Sorun giderme bağlayıcı proxy sorunları ve hizmet bağlantısı sorunları

Şimdi tüm trafik proxy üzerinden akan görmelisiniz. Sorun yaşıyorsanız, aşağıdaki sorun giderme bilgileri yardımcı olur.

Bağlayıcı bağlantı sorunlarını belirlemenin ve sorun gidermenin en iyi yolu, bağlayıcı hizmetini başlatırken ağ yakalama sıcağına sahip olmaktır. Ağ izlemelerini yakalama ve filtreleme yle ilgili bazı hızlı ipuçları aşağıda verilmiştir.

Seçtiğiniz izleme aracını kullanabilirsiniz. Bu makalenin amaçları doğrultusunda Microsoft İleti Çözümleyicisi'ni kullandık. [Microsoft'tan indirebilirsiniz.](https://www.microsoft.com/download/details.aspx?id=44226)

Aşağıdaki örnekler İleti Çözümleyicisi'ne özgüdir, ancak ilkeler herhangi bir çözümleme aracına uygulanabilir.

### <a name="take-a-capture-of-connector-traffic"></a>Konektör trafiğini yakalama

İlk sorun giderme için aşağıdaki adımları gerçekleştirin:

1. services.msc'den Azure AD Uygulama Proxy Bağlayıcısı hizmetini durdurun.

   ![hizmetlerde Azure AD Uygulama Proxy Bağlayıcısı hizmeti.msc](./media/application-proxy-configure-connectors-with-proxy-servers/services-local.png)

1. İleti Çözümleyici'yi yönetici olarak çalıştırın.
1. **Yerel izlemeyi başlat'ı**seçin.
1. Azure AD Uygulama Proxy Bağlayıcısı hizmetini başlatın.
1. Ağ yakalamayı durdurun.

   ![Ekran görüntüsü, ağ yakalamayı durdur düğmesini gösterir](./media/application-proxy-configure-connectors-with-proxy-servers/stop-trace.png)

### <a name="check-if-the-connector-traffic-bypasses-outbound-proxies"></a>Konektör trafiğinin giden vekilleri atlayıp atlayıp atlayolmadığını denetleme

Proxy sunucularını atlayacak ve doğrudan Application Proxy hizmetine bağlanacak şekilde Uygulama Proxy bağlayıcınızı yapılandırtıysanız, başarısız TCP bağlantı denemeleri için ağ yakalamasına bakmak istersiniz.

Bu girişimleri tanımlamak için İleti Çözümleyicisi filtresini kullanın. Filtre `property.TCPSynRetransmit` kutusuna girin ve **Uygula'yı**seçin.

SYN paketi, TCP bağlantısı kurmak için gönderilen ilk pakettir. Bu paket bir yanıt döndürmezse, SYN yeniden denenir. Yeniden aktarılan SYN'leri görmek için önceki filtreyi kullanabilirsiniz. Ardından, bu SYN'lerin bağlayıcıyla ilgili herhangi bir trafiğe karşılık gelip gelmediğini kontrol edebilirsiniz.

Konektörün Azure hizmetlerine doğrudan bağlantı yapmasını bekliyorsanız, 443 bağlantı noktasındaki SynRetransmit yanıtları bir ağ veya güvenlik duvarı sorununuz olduğunun bir göstergesidir.

### <a name="check-if-the-connector-traffic-uses-outbound-proxies"></a>Konektör trafiğinin giden eksksleri kullanıp kullanmayıp kullanmayın

Uygulama Proxy bağlayıcı trafiğinizi proxy sunucularından geçecek şekilde yapılandırıldıysanız, proxy'nize başarısız https bağlantıları aramak istiyorsunuz.

To filter the network capture for these connection attempts, enter `(https.Request or https.Response) and tcp.port==8080` in the Message Analyzer filter, replacing 8080 with your proxy service port. Filtre sonuçlarını görmek için **Uygula'yı** seçin.

Önceki filtre yalnızca PROXY isteklerini ve proxy bağlantı noktasından gelen yanıtları gösterir. Proxy sunucusuyla iletişimi gösteren CONNECT isteklerini arıyorsunuz. Başarı üzerine, bir HTTP OK (200) yanıt olsun.

407 veya 502 gibi diğer yanıt kodlarını görürseniz, proxy'nin kimlik doğrulaması gerektirdiği veya başka bir nedenle trafiğe izin vermeyeceği anlamına gelir. Bu noktada, proxy sunucu destek ekibi meşgul.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD Uygulama Proxy bağlayıcılarını anlama](application-proxy-connectors.md)
* Bağlayıcı bağlantı sorunlarıyla ilgili sorunlarınız varsa, sorunuz Azure [Active Directory forumunda](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD&forum=WindowsAzureAD) sorun veya destek ekibimizle bir bilet oluşturun.
