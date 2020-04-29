---
title: Azure CDN Web içeriğinin kullanım süresini yönetme | Microsoft Docs
description: Azure CDN Azure Web Apps/Cloud Services, ASP.NET veya IIS içeriğinin süresinin dolmasının nasıl yönetileceğini öğrenin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81260573"
---
# <a name="manage-expiration-of-web-content-in-azure-cdn"></a>Azure CDN'de web içeriğinin süre sonunu yönetme
> [!div class="op_single_selector"]
> * [Azure web içeriği](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Blob depolama](cdn-manage-expiration-of-blob-content.md)
> 

Herkese açık kaynaklı Web sunucularından dosyalar, yaşam süresi (TTL) sona erene kadar Azure Content Delivery Network (CDN) ile önbelleğe alınabilir. TTL, kaynak sunucudan gelen HTTP `Cache-Control` yanıtındaki üst bilgi tarafından belirlenir. Bu makalede, hepsi benzer şekilde `Cache-Control` yapılandırılmış Microsoft Azure App Service, Azure Cloud Services, ASP.NET uygulamaları ve Internet INFORMATION SERVICES (IIS) sitelerinin Web Apps özelliği için üst bilgilerin nasıl ayarlanacağı açıklanır. `Cache-Control` Üst bilgiyi yapılandırma dosyalarını kullanarak veya programlama yoluyla ayarlayabilirsiniz. 

Ayrıca, [CDN önbelleğe alma kurallarını](cdn-caching-rules.md)ayarlayarak Azure Portal önbellek ayarlarını kontrol edebilirsiniz. Bir veya daha fazla önbelleğe alma kuralı oluşturursanız ve önbellek ayarlarını **geçersiz kılacak** veya **atlayacak**şekilde ayarlarsanız, bu makalede ele alınan kaynak tarafından sunulan önbelleğe alma ayarları yok sayılır. Genel önbelleğe alma kavramları hakkında daha fazla bilgi için bkz. [önbelleğe alma nasıl kullanılır](cdn-how-caching-works.md).

> [!TIP]
> Bir dosya üzerinde TTL ayarı ayarlamayı tercih edebilirsiniz. Bu durumda, Azure portal önbelleğe alma kurallarını ayarlamadığınız müddetçe, Azure CDN otomatik olarak yedi günlük bir varsayılan TTL uygular. Bu varsayılan TTL yalnızca genel Web teslim iyileştirmeleri için geçerlidir. Büyük dosya iyileştirmeleri için varsayılan TTL bir gündür ve medya akışı iyileştirmeleri için varsayılan TTL bir yıldır.
> 
> Azure CDN dosyalara ve diğer kaynaklara erişimi hızlandırmak için nasıl çalıştığı hakkında daha fazla bilgi için bkz. [Azure Content Delivery Network genel bakış](cdn-overview.md).
> 

## <a name="setting-cache-control-headers-by-using-cdn-caching-rules"></a>CDN önbelleğe alma kurallarını kullanarak Cache-Control üst bilgilerini ayarlama
Bir Web sunucusunun `Cache-Control` üst bilgisini ayarlamak için tercih edilen yöntem Azure Portal önbelleğe alma kurallarını kullanmaktır. CDN önbelleğe alma kuralları hakkında daha fazla bilgi için bkz. [önbelleğe alma kurallarıyla denetim Azure CDN önbelleğe alma davranışı](cdn-caching-rules.md).

> [!NOTE] 
> Önbelleğe alma kuralları yalnızca Verizon **Azure CDN ve Akamai** profillerindeki standart **Azure CDN Standart** için kullanılabilir. **Verizon profillerden Azure CDN Premium** Için, **Manage** Portal içindeki [Azure CDN Rules altyapısını](cdn-rules-engine.md) benzer işlevler için kullanmanız gerekir.

**CDN önbelleğe alma kuralları sayfasına gitmek için**:

1. Azure portal, bir CDN profili seçin ve ardından Web sunucusu için uç noktayı seçin.

1. Ayarların altındaki sol bölmede **Önbelleğe alma kuralları**’nı seçin.

   ![CDN önbelleğe alma kuralları düğmesi](./media/cdn-manage-expiration-of-cloud-service-content/cdn-caching-rules-btn.png)

   **Önbelleğe alma kuralları** sayfası görüntülenir.

   ![CDN önbelleğe alma sayfası](./media/cdn-manage-expiration-of-cloud-service-content/cdn-caching-page.png)


**Genel önbelleğe alma kurallarını kullanarak bir Web sunucusunun Cache-Control üst bilgilerini ayarlamak için:**

1. **Genel önbelleğe alma kuralları**' nın altında sorgu **dizelerini yok say** ve **önbelleğe alma davranışını** **geçersiz kılmak**için **sorgu dizesi önbelleğe alma davranışını** ayarlayın.
      
1. **Önbellek sona erme süresi**Için, **saniye** kutusuna 3600 veya **saat** kutusuna 1 yazın. 

   ![CDN genel önbelleğe alma kuralları örneği](./media/cdn-manage-expiration-of-cloud-service-content/cdn-global-caching-rules-example.png)

   Bu genel önbelleğe alma kuralı bir saatin önbellek süresini ayarlar ve uç noktaya yapılan tüm istekleri etkiler. Uç nokta tarafından `Cache-Control` belirtilen `Expires` kaynak sunucu tarafından gönderilen tüm veya HTTP üstbilgilerini geçersiz kılar.   

1. **Kaydet**’i seçin.

**Özel önbelleğe alma kurallarını kullanarak bir Web sunucusu dosyasının Cache-Control üst bilgilerini ayarlamak için:**

1. **Özel önbelleğe alma kuralları**altında, iki eşleşme koşulu oluşturun:

     a. İlk eşleşme koşulu için **eşleşme koşulu** ' nı **yol** olarak ayarlayın ve `/webfolder1/*` **eşleşme değeri**için girin. **Önbelleğe alma davranışını** **geçersiz kılmak** için ayarlayın ve **saat** kutusuna 4 girin.

     b. İkinci eşleşme koşulu için **eşleşme koşulu** ' nı **yol** olarak ayarlayın ve `/webfolder1/file1.txt` **eşleşme değeri**için girin. **Önbelleğe alma davranışını** **geçersiz kılmak** için ayarlayın ve **saat** kutusuna 2 girin.

    ![CDN özel önbelleğe alma kuralları örneği](./media/cdn-manage-expiration-of-cloud-service-content/cdn-custom-caching-rules-example.png)

    İlk özel önbelleğe alma kuralı, uç noktanız tarafından belirtilen kaynak sunucu üzerindeki `/webfolder1` klasörde bulunan tüm dosyalar için dört saatlik bir önbellek süresi ayarlar. İkinci kural yalnızca `file1.txt` dosyanın ilk kuralını geçersiz kılar ve kendisi için iki saatlik bir önbellek süresi ayarlar.

1. **Kaydet**’i seçin.


## <a name="setting-cache-control-headers-by-using-configuration-files"></a>Yapılandırma dosyalarını kullanarak Cache-Control üst bilgilerini ayarlama
Görüntüler ve stil sayfaları gibi statik içerik için, Web uygulamanız için **ApplicationHost. config** veya **Web. config** yapılandırma dosyalarını değiştirerek güncelleştirme sıklığını kontrol edebilirsiniz. İçeriğinizin `Cache-Control` üst bilgisini ayarlamak için, her iki dosyadaki `<system.webServer>/<staticContent>/<clientCache>` öğesini kullanın.

### <a name="using-applicationhostconfig-files"></a>ApplicationHost. config dosyalarını kullanma
**ApplicationHost. config** dosyası IIS yapılandırma sisteminin kök dosyasıdır. Bir **ApplicationHost. config** dosyasındaki yapılandırma ayarları sitedeki tüm uygulamaları etkiler, ancak bir Web uygulaması için var olan herhangi bir **Web. config** dosyasının ayarları tarafından geçersiz kılınır.

### <a name="using-webconfig-files"></a>Web. config dosyalarını kullanma
Web **. config** dosyası ile, Web uygulamanızın tamamının veya Web uygulamanızdaki belirli bir dizinin nasıl davranacağını özelleştirebilirsiniz. Genellikle, Web uygulamanızın kök klasöründe en az bir **Web. config** dosyanız vardır. Belirli bir klasördeki her **Web. config** dosyası için, yapılandırma ayarları başka bir **Web. config** dosyası tarafından alt klasör düzeyinde geçersiz kılınmadıkça, bu klasördeki ve alt klasörlerindeki her şeyi etkiler. 

Örneğin, Web uygulamanızdaki tüm statik içerikleri `<clientCache>` üç gün boyunca önbelleğe almak için Web uygulamanızın kök klasöründeki **Web. config** dosyasında bir öğesi ayarlayabilirsiniz. Ayrıca, daha fazla değişken içeriğe sahip bir alt klasöre **Web. config** dosyası ekleyebilir (örneğin, `\frequent`) ve `<clientCache>` öğesini altı saat alt klasörünün içeriğini önbelleğe almak üzere ayarlayabilirsiniz. Net sonuç, `\frequent` dizindeki herhangi bir içerik hariç olmak üzere, tüm Web sitesinin içeriği üç gün boyunca önbelleğe alınır ve yalnızca altı saat boyunca önbelleğe alınır.  

Aşağıdaki XML yapılandırma dosyası örneği, `<clientCache>` öğesinin üç güne ait maksimum yaşı belirtmek için nasıl ayarlanacağını gösterir:  

```xml
<configuration>
    <system.webServer>
        <staticContent>
            <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00" />
        </staticContent>
    </system.webServer>
</configuration>
```

**CacheControlMaxAge** özniteliğini kullanmak Için, **cachecontrolmode** özniteliğinin değerini olarak `UseMaxAge`ayarlamanız gerekir. Bu ayar, HTTP üst bilgisi ve yönergesinin `Cache-Control: max-age=<nnn>`yanıta eklenmesine neden oldu. **CacheControlMaxAge** özniteliği için TimeSpan değerinin biçimi `<days>.<hours>:<min>:<sec>`. Değeri saniyeye dönüştürülür ve `Cache-Control` `max-age` yönergesinin değeri olarak kullanılır. `<clientCache>` Öğesi hakkında daha fazla bilgi için bkz. [istemci önbellek \<clientcache>](https://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache).  

## <a name="setting-cache-control-headers-programmatically"></a>Önbellek denetimi üstbilgilerini programlama yoluyla ayarlama
ASP.NET uygulamalarında, .NET API 'nin **HttpResponse. Cache** ÖZELLIĞINI ayarlayarak CDN önbelleğe alma davranışını programlı bir şekilde kontrol edersiniz. **HttpResponse. Cache** özelliği hakkında daha fazla bilgi için bkz. [HttpResponse. Cache özelliği](/dotnet/api/system.web.httpresponse.cache#System_Web_HttpResponse_Cache) ve [HttpCachePolicy sınıfı](/dotnet/api/system.web.httpcachepolicy).  

ASP.NET ' de uygulama içeriğini programlı bir şekilde önbelleğe almak için şu adımları izleyin:
   1. İçeriğinin, olarak ayarlanarak `HttpCacheability` önbelleğe alınabilir olarak işaretlendiğinden emin olun. `Public` 
   1. Aşağıdaki `HttpCachePolicy` yöntemlerden birini çağırarak bir önbellek doğrulayıcısı ayarlayın:
      - Üst bilgi için zaman damgası değeri ayarlama çağrısı `SetLastModified` `Last-Modified`
      - Üst bilgi için bir değer ayarlamak üzere çağırın `SetETag` `ETag`
   1. İsteğe bağlı olarak, `SetExpires` `Expires` üst bilgi için bir değer ayarlamak üzere çağırarak bir önbellek süre sonu zamanı belirtin. Aksi takdirde, bu belgede daha önce açıklanan varsayılan önbellek buluşsal yöntemleri geçerlidir.

Örneğin, bir saatin içeriğini önbelleğe almak için aşağıdaki C# kodunu ekleyin:  

```csharp
// Set the caching parameters.
Response.Cache.SetExpires(DateTime.Now.AddHours(1));
Response.Cache.SetCacheability(HttpCacheability.Public);
Response.Cache.SetLastModified(DateTime.Now);
```

## <a name="testing-the-cache-control-header"></a>Cache-Control üst bilgisini test etme
Web içeriğinizin TTL ayarlarını kolayca doğrulayabilirsiniz. Tarayıcınızın [Geliştirici araçlarıyla](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/), Web içeriğinizin `Cache-Control` yanıt üst bilgisini içerdiğini test edin. Yanıt üst bilgilerini incelemek için **wget**, [Postman](https://www.getpostman.com/)veya [Fiddler](https://www.telerik.com/fiddler) gibi bir araç da kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar
* [**Clientcache** öğesiyle ilgili ayrıntıları okuyun](https://www.iis.net/ConfigReference/system.webServer/staticContent/clientCache)
* [**HttpResponse. Cache** özelliği için belgeleri okuyun](/dotnet/api/system.web.httpresponse.cache#System_Web_HttpResponse_Cache) 
* [**HttpCachePolicy sınıfının** belgelerini okuyun](/dotnet/api/system.web.httpcachepolicy)  
* [Önbelleğe alma kavramlarını öğrenin](cdn-how-caching-works.md)
