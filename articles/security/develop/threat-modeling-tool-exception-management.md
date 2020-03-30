---
title: Özel Durum Yönetimi - Microsoft Tehdit Modelleme Aracı - Azure | Microsoft Dokümanlar
description: Tehdit Modelleme Aracı'nda açığa çıkan tehditler için azaltıcı etkenler
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.subservice: security-develop
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: jegeib
ms.openlocfilehash: b8fad566b54ab645660011ad3188394b6f8190b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68728068"
---
# <a name="security-frame-exception-management--mitigations"></a>Güvenlik Çerçevesi: Özel Durum Yönetimi | Azaltıcı etken 
| Ürün / Hizmet | Makale |
| --------------- | ------- |
| **WCF** | <ul><li>[WCF- Yapılandırma dosyasına serviceDebug düğümü eklemeyin](#servicedebug)</li><li>[WCF- Configuration dosyasına serviceMetadata düğümü eklemeyin](#servicemetadata)</li></ul> |
| **Web API** | <ul><li>[Web API'ASP.NET uygun özel durum işlemenin yapıldığından emin olun](#exception)</li></ul> |
| **Web Uygulaması** | <ul><li>[Güvenlik ayrıntılarını hata iletilerinde ifşa etmeyin](#messages)</li><li>[Varsayılan hata işleme sayfasını uygulama](#default)</li><li>[Dağıtım Yöntemini IIS'de Perakende Olarak Ayarlama](#deployment)</li><li>[Özel durumlar güvenli bir şekilde başarısız olmalıdır](#fail)</li></ul> |

## <a name="wcf--do-not-include-servicedebug-node-in-configuration-file"></a><a id="servicedebug"></a>WCF- Yapılandırma dosyasına serviceDebug düğümü eklemeyin

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel, NET Framework 3 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Türkiye](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_debug_information) |
| **Adımlar** | Windows Communication Framework (WCF) hizmetleri hata ayıklama bilgilerini ortaya çıkaracak şekilde yapılandırılabilir. Hata ayıklama bilgileri üretim ortamlarında kullanılmamalıdır. Etiket, `<serviceDebug>` hata ayıklama bilgi özelliğinin bir WCF hizmeti için etkin olup olmadığını tanımlar. Öznitelik ExceptionDetailInFaults true ayarlanırsa, uygulamadan özel durum bilgileri istemcilere döndürülür. Saldırganlar, uygulama tarafından kullanılan çerçeve, veritabanı veya diğer kaynaklara yönelik saldırılar oluşturmak için çıktı hata ayıklamadan elde ettikleri ek bilgilerden yararlanabilirler. |

### <a name="example"></a>Örnek
Aşağıdaki yapılandırma dosyası `<serviceDebug>` etiketi içerir: 
```
<configuration> 
<system.serviceModel> 
<behaviors> 
<serviceBehaviors> 
<behavior name=""MyServiceBehavior""> 
<serviceDebug includeExceptionDetailInFaults=""True"" httpHelpPageEnabled=""True""/> 
... 
```
Hizmetteki hata ayıklama bilgilerini devre dışı. Bu, `<serviceDebug>` uygulamanızın yapılandırma dosyasından etiketi kaldırarak gerçekleştirilebilir. 

## <a name="wcf--do-not-include-servicemetadata-node-in-configuration-file"></a><a id="servicemetadata"></a>WCF- Configuration dosyasına serviceMetadata düğümü eklemeyin

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Genel, NET Framework 3 |
| **Başvurular**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Türkiye](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_service_enumeration) |
| **Adımlar** | Bir hizmetle ilgili bilgileri genel olarak açığa çıkarmak, saldırganlara hizmeti nasıl kullanabilecekleri konusunda değerli bilgiler sağlayabilir. Etiket `<serviceMetadata>` meta veri yayımlama özelliğini etkinleştirer. Hizmet meta verileri, genel olarak erişilmemesi gereken hassas bilgiler içerebilir. En azından, yalnızca güvenilen kullanıcıların meta verilere erişmesine ve gereksiz bilgilerin açığa alınmadığından emin olun. Daha da iyisi, meta verileri yayımlama yeteneğini tamamen devre dışı kılabilir. Güvenli bir WCF yapılandırması `<serviceMetadata>` etiketi içermez. |

## <a name="ensure-that-proper-exception-handling-is-done-in-aspnet-web-api"></a><a id="exception"></a>Web API'ASP.NET uygun özel durum işlemenin yapıldığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | MVC 5, MVC 6 |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [ASP.NET Web API'sinde Özel Durum Kullanımı,](https://www.asp.net/web-api/overview/error-handling/exception-handling) [ASP.NET Web API'sinde Model Doğrulama](https://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api) |
| **Adımlar** | Varsayılan olarak, ASP.NET Web API'deki yakalanmayan çoğu özel durum durum koduyla birlikte bir HTTP yanıtına çevrilir`500, Internal Server Error`|

### <a name="example"></a>Örnek
API tarafından döndürülen durum kodunu `HttpResponseException` denetlemek için aşağıda gösterildiği gibi kullanılabilir: 
```csharp
public Product GetProduct(int id)
{
    Product item = repository.Get(id);
    if (item == null)
    {
        throw new HttpResponseException(HttpStatusCode.NotFound);
    }
    return item;
}
```

### <a name="example"></a>Örnek
Özel durum yanıtı üzerinde daha `HttpResponseMessage` fazla denetim için, sınıf aşağıda gösterildiği gibi kullanılabilir: 
```csharp
public Product GetProduct(int id)
{
    Product item = repository.Get(id);
    if (item == null)
    {
        var resp = new HttpResponseMessage(HttpStatusCode.NotFound)
        {
            Content = new StringContent(string.Format("No product with ID = {0}", id)),
            ReasonPhrase = "Product ID Not Found"
        }
        throw new HttpResponseException(resp);
    }
    return item;
}
```
Türde `HttpResponseException`olmayan işlenmemiş özel durumları yakalamak için Özel Durum Filtreleri kullanılabilir. Özel durum filtreleri `System.Web.Http.Filters.IExceptionFilter` arabirimi uygular. Özel durum filtresi yazmanın en basit yolu `System.Web.Http.Filters.ExceptionFilterAttribute` sınıftan türemiş ve OnException yöntemini geçersiz kılmaktır. 

### <a name="example"></a>Örnek
Özel `NotImplementedException` durumları HTTP durum koduna `501, Not Implemented`dönüştüren bir filtre aşağıda veda edebilirsiniz: 
```csharp
namespace ProductStore.Filters
{
    using System;
    using System.Net;
    using System.Net.Http;
    using System.Web.Http.Filters;

    public class NotImplExceptionFilterAttribute : ExceptionFilterAttribute 
    {
        public override void OnException(HttpActionExecutedContext context)
        {
            if (context.Exception is NotImplementedException)
            {
                context.Response = new HttpResponseMessage(HttpStatusCode.NotImplemented);
            }
        }
    }
}
```

Web API özel durum filtresini kaydetmenin birkaç yolu vardır:
- Eyleme göre
- Denetleyiciye göre
- Genel

### <a name="example"></a>Örnek
Filtreyi belirli bir eyleme uygulamak için, filtreyi eyleme bir öznitelik olarak ekleyin: 
```csharp
public class ProductsController : ApiController
{
    [NotImplExceptionFilter]
    public Contact GetContact(int id)
    {
        throw new NotImplementedException("This method is not implemented");
    }
}
```
### <a name="example"></a>Örnek
Filtreyi tüm `controller`eylemlere uygulamak için, filtreyi `controller` sınıfa öznitelik olarak ekleyin: 

```csharp
[NotImplExceptionFilter]
public class ProductsController : ApiController
{
    // ...
}
```

### <a name="example"></a>Örnek
Filtreyi tüm Web API denetleyicilerine genel olarak uygulamak için, `GlobalConfiguration.Configuration.Filters` koleksiyona filtrenin bir örneğini ekleyin. Bu koleksiyondaki özel durum filtreleri herhangi bir Web API denetleyici eylemi için geçerlidir. 
```csharp
GlobalConfiguration.Configuration.Filters.Add(
    new ProductStore.NotImplExceptionFilterAttribute());
```

### <a name="example"></a>Örnek
Model doğrulama için, model durumu aşağıda gösterildiği gibi CreateErrorResponse yöntemine geçirilebilir: 
```csharp
public HttpResponseMessage PostProduct(Product item)
{
    if (!ModelState.IsValid)
    {
        return Request.CreateErrorResponse(HttpStatusCode.BadRequest, ModelState);
    }
    // Implementation not shown...
}
```

ASP.NET Web API'de olağanüstü işleme ve model doğrulama hakkında ek ayrıntılar için referanslar bölümündeki bağlantıları kontrol edin 

## <a name="do-not-expose-security-details-in-error-messages"></a><a id="messages"></a>Güvenlik ayrıntılarını hata iletilerinde ifşa etmeyin

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | Yok  |
| **Adımlar** | <p>Genel hata iletileri, hassas uygulama verileri dahil edilmeden doğrudan kullanıcıya sağlanır. Hassas verilere örnek olarak şunlar verilebilir:</p><ul><li>Sunucu adları</li><li>Bağlantı dizeleri</li><li>Usernames</li><li>Parolalar</li><li>SQL yordamları</li><li>Dinamik SQL hatalarının ayrıntıları</li><li>Yığın izleme ve kod satırları</li><li>Bellekte depolanan değişkenler</li><li>Sürücü ve klasör konumları</li><li>Uygulama yükleme noktaları</li><li>Ana bilgisayar yapılandırma ayarları</li><li>Diğer dahili uygulama ayrıntıları</li></ul><p>Bir uygulamadaki tüm hataları yakalama ve genel hata iletileri sağlamanın yanı sıra IIS içinde özel hataları etkinleştirmek bilgi nin açıklanmasını önlemeye yardımcı olur. SQL Server veritabanı ve .NET Özel Durum işleme, diğer hata işleme mimarileri arasında, özellikle ayrıntılı ve son derece kötü niyetli bir kullanıcı uygulamaprofilleme için yararlıdır. .NET Özel Durum sınıfından türetilen bir sınıfın içeriğini doğrudan görüntülemeyin ve beklenmeyen bir özel durum istemeden doğrudan kullanıcıya yükseltilmesin diye uygun özel durum işlemesahip olduğundan emin olun.</p><ul><li>Özel durum/hata iletisinde doğrudan bulunan belirli ayrıntıları özetleyen genel hata iletilerini doğrudan kullanıcıya sağlayın</li><li>Bir .NET özel durum sınıfının içeriğini doğrudan kullanıcıya görüntülemeyin</li><li>Tüm hata iletilerini yalıtmak ve uygunsa uygulama istemcisine gönderilen genel bir hata iletisi aracılığıyla kullanıcıyı bilgilendirin</li><li>Özel Durum sınıfının içeriğini doğrudan kullanıcıya, özellikle İleti `.ToString()`veya StackTrace özelliklerinin değerlerinden gelen iade değerini ifşa etmeyin. Bu bilgileri güvenli bir şekilde kaydedin ve kullanıcıya daha zararsız bir ileti görüntüleyin</li></ul>|

## <a name="implement-default-error-handling-page"></a><a id="default"></a>Varsayılan hata işleme sayfasını uygulama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [ASP.NET Hata Sayfası Ayarlarını Düzenle İletişim Kutusu](https://technet.microsoft.com/library/dd569096(WS.10).aspx) |
| **Adımlar** | <p>bir ASP.NET uygulaması başarısız olduğunda ve bir HTTP/1.x 500 Dahili Sunucu Hatasına neden olduğunda veya bir özellik yapılandırması (İstek Filtreleme gibi) bir sayfanın görüntülenmesini engelliyorsa, bir hata iletisi oluşturulur. Yöneticiler, uygulamanın istemciye dostça bir ileti, istemciye ayrıntılı hata iletisi veya yalnızca localhost'a ayrıntılı hata iletisi gösterip göstermeyeceğini seçebilir. web.config'deki `<customErrors>` etiketin üç modu vardır:</p><ul><li>**Şu şekilde:** Özel hataların etkin olduğunu belirtir. DefaultRedirect özniteliği belirtilmemişse, kullanıcılar genel bir hata görür. Özel hatalar uzak istemcilere ve yerel ana bilgisayara gösterilir</li><li>**Kapalı:** Özel hataların devre dışı bırakıldığını belirtir. Ayrıntılı ASP.NET hataları uzak istemcilere ve yerel ana bilgisayara gösterilir</li><li>**Yalnızca Uzaktan Kumanda:** Özel hataların yalnızca uzak istemcilere gösterildiğini ve ASP.NET hatalarının yerel ana bilgisayara gösterildiğini belirtir. Bu varsayılan değerdir</li></ul><p>Uygulama/site için dosyayı `web.config` açın ve etiketin `<customErrors mode="RemoteOnly" />` `<customErrors mode="On" />` veya tanımlı olduğundan emin olun.</p>|

## <a name="set-deployment-method-to-retail-in-iis"></a><a id="deployment"></a>Dağıtım Yöntemini IIS'de Perakende Olarak Ayarlama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Dağıtım |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [dağıtım Öğesi (ASP.NET Ayarlar Şeması)](https://msdn.microsoft.com/library/ms228298(VS.80).aspx) |
| **Adımlar** | <p>Anahtar, `<deployment retail>` üretim IIS sunucuları tarafından kullanılmak üzere tasarlanmıştır. Bu anahtar, uygulamanın bir sayfada izleme çıktısı oluşturma yeteneğini devre dışı bırakarak, uygulamaların mümkün olan en iyi performansla çalışmasına ve mümkün olan en az güvenlik bilgi sızıntısıyla çalışmasına yardımcı olmak için kullanılır. son kullanıcılar ve hata ayıklama anahtarı devre dışı bırakma.</p><p>Çoğu zaman, anahtarlar ve başarısız istek izleme ve hata ayıklama gibi geliştirici odaklı seçenekler, etkin geliştirme sırasında etkinleştirilir. Herhangi bir üretim sunucusundaki dağıtım yönteminin perakende olarak ayarlanması önerilir. machine.config dosyasını açın `<deployment retail="true" />` ve doğru olarak ayarlandığından emin olun.</p>|

## <a name="exceptions-should-fail-safely"></a><a id="fail"></a>Özel durumlar güvenli bir şekilde başarısız olmalıdır

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL Fazı**               | Oluşturma |  
| **Uygulanabilir Teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvurular**              | [Güvenli bir şekilde başarısız olun](https://www.owasp.org/index.php/Fail_securely) |
| **Adımlar** | Uygulama güvenli bir şekilde başarısız olmalıdır. Belirli bir kararın verildiği boolean değerini döndüren herhangi bir yöntem, dikkatle oluşturulan özel durum bloğuna sahip olmalıdır. Özel durum bloğu dikkatsizce yazıldığında, güvenlik sorunlarının süründüğü birçok mantıksal hata vardır.|

### <a name="example"></a>Örnek
```csharp
        public static bool ValidateDomain(string pathToValidate, Uri currentUrl)
        {
            try
            {
                if (!string.IsNullOrWhiteSpace(pathToValidate))
                {
                    var domain = RetrieveDomain(currentUrl);
                    var replyPath = new Uri(pathToValidate);
                    var replyDomain = RetrieveDomain(replyPath);

                    if (string.Compare(domain, replyDomain, StringComparison.OrdinalIgnoreCase) != 0)
                    {
                        //// Adding additional check to enable CMS urls if they are not hosted on same domain.
                        if (!string.IsNullOrWhiteSpace(Utilities.CmsBase))
                        {
                            var cmsDomain = RetrieveDomain(new Uri(Utilities.Base.Trim()));
                            if (string.Compare(cmDomain, replyDomain, StringComparison.OrdinalIgnoreCase) != 0)
                            {
                                return false;
                            }
                            else
                            {
                                return true;
                            }
                        }

                        return false;
                    }
                }

                return true;
            }
            catch (UriFormatException ex)
            {
                LogHelper.LogException("Utilities:ValidateDomain", ex);
                return true;
            }
        }
```
Bazı özel durum lar gerçekleşirse, yukarıdaki yöntem her zaman True döndürecektir. Son kullanıcı, tarayıcının saygı duyduğu, ancak `Uri()` oluşturucunun sunmadığı bozuk bir URL sağlarsa, bu bir özel durum oluşturur ve kurban geçerli ancak biçimsiz URL'ye alınır. 
