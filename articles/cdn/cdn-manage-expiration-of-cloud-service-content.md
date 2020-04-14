---
title: Azure CDN'de web içeriğinin sona ermesini yönetme | Microsoft Dokümanlar
description: Azure CDN'de Azure Web Apps/Bulut Hizmetleri, ASP.NET veya IIS içeriğinin sona ermesini nasıl yöneteceğimiz öğrenin.
services: cdn
documentationcenter: .NET
author: asudbring
manager: danielgi
editor: ''
ms.assetid: bef53fcc-bb13-4002-9324-9edee9da8288
ms.service: azure-cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/15/2018
ms.author: allensu
ms.openlocfilehash: 4598e6cee6ffbaaeb2a99727842fcd17fe0046c7
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81260573"
---
# <a name="manage-expiration-of-web-content-in-azure-cdn"></a>Azure CDN'de web içeriğinin süre sonunu yönetme
> [!div class="op_single_selector"]
> * [Azure web içeriği](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob depolama](cdn-manage-expiration-of-blob-content.md)
> 

Herkese açık kaynak web sunucularından gelen dosyalar, canlı (TTL) süresi dolana kadar Azure İçerik Teslim Ağı'nda (CDN) önbelleğe alınabilir. TTL, başlangıç sunucusundan HTTP yanıtındaki `Cache-Control` üstbilgi tarafından belirlenir. Bu makalede, Microsoft `Cache-Control` Azure Uygulama Hizmeti, Azure Bulut Hizmetleri, ASP.NET uygulamaları ve Internet Information Services (IIS) sitelerinin Web Apps özelliği için üstbilgileri nasıl ayarlanabileceğiniz açıklanmaktadır ve bunların tümü benzer şekilde yapılandırılmıştır. Üstbilgiyi `Cache-Control` yapılandırma dosyalarını kullanarak veya programlı olarak ayarlayabilirsiniz. 

[CDN önbelleğe alma kurallarını](cdn-caching-rules.md)ayarlayarak Azure portalından önbellek ayarlarını da denetleyebilirsiniz. Bir veya daha fazla önbelleğe alma kuralı oluşturur ve önbelleğe alma davranışlarını **Geçersiz Kılma** veya **Atlama önbelleği**olarak ayarlarsanız, bu makalede tartışılan kaynak sağlanan önbelleğe alma ayarları yoksayılır. Genel önbelleğe alma kavramları hakkında bilgi [için](cdn-how-caching-works.md)bkz.

> [!TIP]
> Bir dosyada TTL ayarlamamayı seçebilirsiniz. Bu durumda, Azure portalında önbelleğe alma kuralları ayarlamadığınız sürece Azure CDN varsayılan yedi günlük ttl'yi otomatik olarak uygular. Bu varsayılan TTL yalnızca genel web teslim optimizasyonları için geçerlidir. Büyük dosya optimizasyonları için varsayılan TTL bir gündür ve ortam akışı optimizasyonları için varsayılan TTL bir yıldır.
> 
> Dosyalara ve diğer kaynaklara erişimi hızlandırmak için Azure CDN'nin nasıl çalıştığı hakkında daha fazla bilgi için Azure [İçerik Dağıtım Ağına Genel Bakış bölümüne](cdn-overview.md)bakın.
> 

## <a name="setting-cache-control-headers-by-using-cdn-caching-rules"></a>CDN önbelleğe alma kurallarını kullanarak Önbellek Denetimi üstleri ayarlama
Bir web sunucusunun `Cache-Control` üstbilgisini ayarlamak için tercih edilen yöntem, Azure portalında önbelleğe alma kurallarını kullanmaktır. CDN önbelleğe alma kuralları hakkında daha fazla bilgi için [önbelleğe alma kurallarıyla Azure CDN önbelleğe alma davranışını denetle'](cdn-caching-rules.md)ye bakın.

> [!NOTE] 
> Önbelleğe alma kuralları yalnızca **Akamai profillerinden** Verizon ve Azure **CDN Standard'dan Azure CDN Standardı** için kullanılabilir. **Verizon profillerinden Azure CDN Premium** için, benzer işlevler için **Yönet** portalındaki [Azure CDN kuralları altyapısını](cdn-rules-engine.md) kullanmanız gerekir.

**CDN önbelleğe alma kuralları sayfasına gitmek için:**

1. Azure portalında bir CDN profili seçin ve ardından web sunucusuiçin bitiş noktasını seçin.

1. Ayarların altındaki sol bölmede **Önbelleğe alma kuralları**’nı seçin.

   ![CDN önbelleğe alma kuralları düğmesi](./media/cdn-manage-expiration-of-cloud-service-content/cdn-caching-rules-btn.png)

   **Önbelleğe alma kuralları** sayfası görüntülenir.

   ![CDN önbelleğe alma sayfası](./media/cdn-manage-expiration-of-cloud-service-content/cdn-caching-page.png)


**Genel önbelleğe alma kurallarını kullanarak bir web sunucusunun Önbellek Denetimi üstbilgilerini ayarlamak için:**

1. **Genel önbelleğe alma kuralları**altında, **sorgu dizeleri yoksaymak** için **Sorgu dize önbelleğe alma davranışı** ayarlayın ve **Önbelleğe Alma davranışını** **geçersiz kılmak**için ayarlayın.
      
1. **Önbellek son kullanma süresi için** **Saniye** kutusuna 3600 veya **Saat** kutusuna 1 girin. 

   ![CDN genel önbelleğe alma kuralları örneği](./media/cdn-manage-expiration-of-cloud-service-content/cdn-global-caching-rules-example.png)

   Bu genel önbelleğe alma kuralı bir saatlik önbellek süresi ayarlar ve bitiş noktasına kadar tüm istekleri etkiler. Bitiş noktası `Cache-Control` tarafından `Expires` belirtilen başlangıç sunucusu tarafından gönderilen herhangi bir veya HTTP üstbilgisini geçersiz kılar.   

1. **Kaydet'i**seçin.

**Özel önbelleğe alma kuralları kullanarak bir web sunucusu dosyasının Önbellek Denetimi üstbilgilerini ayarlamak için:**

1. **Özel önbelleğe alma kuralları altında,** iki maç koşulu oluşturun:

     a. İlk eşleşme koşulu **için, Match koşulunu** **Yol'a** ayarlayın ve Maç `/webfolder1/*` **değeri**için girin. **Önbelleğe Alma davranışını** **Geçersiz kılmaya** ayarlayın ve **Saat** kutusuna 4 girin.

     b. İkinci eşleşme koşulu için **Maç koşulunu** `/webfolder1/file1.txt` **Yol'a** ayarlayın ve Match **değeri**için girin. **Önbelleğe Alma davranışını** **Geçersiz kılmaya** ayarlayın ve **Saat** kutusuna 2 girin.

    ![CDN özel önbelleğe alma kuralları örneği](./media/cdn-manage-expiration-of-cloud-service-content/cdn-custom-caching-rules-example.png)

    İlk özel önbelleğe alma kuralı, bitiş noktanızda belirtilen `/webfolder1` başlangıç sunucusundaki klasördeki tüm dosyalar için dört saatlik bir önbellek süresi ayarlar. İkinci kural `file1.txt` yalnızca dosya için ilk kuralı geçersiz kılar ve bunun için iki saatlik bir önbellek süresi ayarlar.

1. **Kaydet'i**seçin.


## <a name="setting-cache-control-headers-by-using-configuration-files"></a>Yapılandırma dosyalarını kullanarak Önbellek Denetimi üsterlerini ayarlama
Resimler ve stil sayfaları gibi statik içerik için, web uygulamanız için **applicationHost.config** veya **Web.config** yapılandırma dosyalarını değiştirerek güncelleştirme sıklığını denetleyebilirsiniz. İçeriğiniz `Cache-Control` için üstbilgi ayarlamak için `<system.webServer>/<staticContent>/<clientCache>` her iki dosyadaki öğeyi kullanın.

### <a name="using-applicationhostconfig-files"></a>ApplicationHost.config dosyalarını kullanma
**ApplicationHost.config** dosyası IIS yapılandırma sisteminin kök dosyasıdır. **ApplicationHost.config** dosyasındaki yapılandırma ayarları sitedeki tüm uygulamaları etkiler, ancak bir web uygulaması için var olan herhangi bir **Web.config** dosyasının ayarları tarafından geçersiz kılındı.

### <a name="using-webconfig-files"></a>Web.config dosyalarını kullanma
With a **Web.config** file, you can customize the way your entire web application or a specific directory on your web application behaves. Genellikle, web uygulamanızın kök klasöründe en az bir **Web.config** dosyanız vardır. Belirli bir klasördeki her **Web.config** dosyası için yapılandırma ayarları, alt klasör düzeyinde başka bir **Web.config** dosyası tarafından geçersiz kılınmadığı sürece, o klasördeki ve alt klasörlerideki her şeyi etkiler. 

Örneğin, web uygulamanızın `<clientCache>` kök klasöründe bir **Web.config** dosyasında bir öğe ayarlayabilirsiniz, web uygulamanızdaki tüm statik içeriği üç gün süreyle önbelleğe alabilirsiniz. Ayrıca, daha değişken içeriğe (örneğin) `\frequent`sahip bir alt klasöre bir **Web.config** dosyası ekleyebilir ve öğesini `<clientCache>` alt klasörün içeriğini altı saat önbelleğe alacak şekilde ayarlayabilirsiniz. Net sonuç, yalnızca altı saat için önbelleğe alınmış olan `\frequent` dizindeki herhangi bir içerik dışında, tüm web sitesindeki içeriğin üç gün süreyle önbelleğe alınmış olmasıdır.  

Aşağıdaki XML yapılandırma dosyası örneği, `<clientCache>` öğenin en fazla üç günlük bir yaş belirtecek şekilde nasıl ayarlanabildiğini gösterir:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

**ÖnbellekDenetimiMaxAge** özniteliğini kullanmak için, **önbellekDenetimModu** özniteliğinin değerini `UseMaxAge`. Bu ayar, http üstbilgi `Cache-Control: max-age=<nnn>`ve yönergesi, yanıta eklenmesine neden oldu. **ÖnbellekControlMaxAge** özniteliği için zaman alanı değerinin `<days>.<hours>:<min>:<sec>`biçimi . Değeri saniyeye dönüştürülür ve `Cache-Control` `max-age` yönerge değeri olarak kullanılır. `<clientCache>` Öğe hakkında daha fazla bilgi için, [>bkz. \< ](https://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)  

## <a name="setting-cache-control-headers-programmatically"></a>Önbellek denetimi üstbilgilerini programlı olarak ayarlama
ASP.NET uygulamalar için, .NET API'nin **HttpResponse.Cache** özelliğini ayarlayarak CDN önbelleğe alma davranışını programlı olarak denetlersiniz. **HttpResponse.Cache** özelliği hakkında bilgi için bkz: [HttpResponse.Cache Özelliği](/dotnet/api/system.web.httpresponse.cache#System_Web_HttpResponse_Cache) ve [HttpCachePolicy Sınıf.](/dotnet/api/system.web.httpcachepolicy)  

Uygulama içeriğini ASP.NET programlı olarak önbelleğe almak için aşağıdaki adımları izleyin:
   1. 'ye ayarlayarak `HttpCacheability` içeriğin önbelleğe `Public`alındığını doğrulayın. 
   1. Aşağıdaki `HttpCachePolicy` yöntemlerden birini arayarak önbellek doğrulayıcısını ayarlayın:
      - Üstbilgi için bir zaman damgası değeri ayarlamak için arayın. `SetLastModified` `Last-Modified`
      - Üstbilgi için bir değer ayarlamak için arayın. `SetETag` `ETag`
   1. İsteğe bağlı olarak, üstbilgi `SetExpires` için bir değer `Expires` ayarlamak için arayarak önbellek son kullanma süresini belirtin. Aksi takdirde, bu belgede daha önce açıklanan varsayılan önbellek sezgisel uygulanır.

Örneğin, içeriği bir saat önbelleğe almak için aşağıdaki C# kodunu ekleyin:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="testing-the-cache-control-header"></a>Önbellek Denetimi üstbilgisini sına
Web içeriğinizin TTL ayarlarını kolayca doğrulayabilirsiniz. Tarayıcınızın geliştirici [araçlarıyla,](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/)web içeriğinizin `Cache-Control` yanıt üstbilgisini içerdiğini test edin. Ayrıca, yanıt üstbilgilerini incelemek için **wget,** [Postman](https://www.getpostman.com/)veya [Fiddler](https://www.telerik.com/fiddler) gibi bir araç da kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar
* [**Istemci Önbellek** öğesi hakkındaki ayrıntıları okuyun](https://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [**HttpResponse.Cache** Özelliği için belgeleri okuyun](/dotnet/api/system.web.httpresponse.cache#System_Web_HttpResponse_Cache) 
* [**HttpCachePolicy Sınıfı** için belgeleri okuyun](/dotnet/api/system.web.httpcachepolicy)  
* [Önbelleğe alma kavramları hakkında bilgi edinin](cdn-how-caching-works.md)
