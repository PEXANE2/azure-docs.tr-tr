---
title: Azure Işlevleri HTTP çıkış bağlamaları
description: Azure Işlevleri 'nde HTTP yanıtlarını döndürme hakkında bilgi edinin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/21/2020
ms.author: cshoe
ms.openlocfilehash: a25658677e436edf4d001599bb4981f527016596
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78357947"
---
# <a name="azure-functions-http-output-bindings"></a>Azure Işlevleri HTTP çıkış bağlamaları

Http istek göndericisine yanıt vermek için HTTP çıkış bağlamasını kullanın. Bu bağlama bir HTTP tetikleyicisi gerektirir ve tetikleyicinin isteğiyle ilişkili yanıtı özelleştirmenize olanak sağlar.

HTTP ile tetiklenen bir işlev için varsayılan dönüş değeri:

- 2\. x ve üzeri Işlevlerde boş bir gövdeye sahip `HTTP 204 No Content`
- 1\. x Işlevlerinde boş bir gövdeye sahip `HTTP 200 OK`

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, *function. JSON* dosyasında ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır. Sınıf C# kitaplıkları için, bu *function. JSON* özelliklerine karşılık gelen bir öznitelik özelliği yok.

|Özellik  |Açıklama  |
|---------|---------|
| **type** |`http`olarak ayarlanmalıdır. |
| **direction** | `out`olarak ayarlanmalıdır. |
| **ada** | Yanıt için işlev kodunda kullanılan değişken adı veya dönüş değerini kullanmak için `$return`. |

## <a name="usage"></a>Kullanım

HTTP yanıtı göndermek için, dil standardı yanıt düzenlerini kullanın. Veya C# C# betik içinde, işlev dönüş türü `IActionResult` veya `Task<IActionResult>`. ' C#De, dönüş değeri özniteliği gerekli değildir.

Örneğin, bkz. [tetikleyici örneği](./functions-bindings-http-webhook-trigger.md#example).

## <a name="hostjson-settings"></a>Host.JSON ayarları

Bu bölümde, 2. x ve üzeri sürümlerde bu bağlama için kullanılabilen genel yapılandırma ayarları açıklanmaktadır. Aşağıdaki örnek Host. JSON dosyası, bu bağlamanın yalnızca sürüm 2. x + ayarlarını içerir. 2\. x ve daha ötesi sürümlerindeki genel yapılandırma ayarları hakkında daha fazla bilgi için bkz. [Azure işlevleri için Host. JSON başvurusu](functions-host-json.md).

> [!NOTE]
> 1\. x Işlevleri içindeki Host. JSON başvurusu için bkz. [Azure işlevleri için Host. JSON başvurusu 1. x](functions-host-json-v1.md#http).

```json
{
    "extensions": {
        "http": {
            "routePrefix": "api",
            "maxOutstandingRequests": 200,
            "maxConcurrentRequests": 100,
            "dynamicThrottlesEnabled": true,
            "hsts": {
                "isEnabled": true,
                "maxAge": "10"
            },
            "customHeaders": {
                "X-Content-Type-Options": "nosniff"
            }
        }
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
| customHeaders|yok|HTTP yanıtında özel üstbilgiler ayarlamanıza olanak sağlar. Önceki örnek, içerik türü algılaması olmaması için `X-Content-Type-Options` üst bilgisini yanıta ekler. |
|dynamicThrottlesEnabled|doğru<sup>\*</sup>|Bu ayar etkinleştirildiğinde, istek işleme işlem hattının `connections/threads/processes/memory/cpu/etc` gibi sistem performans sayaçlarını düzenli olarak denetlemesini sağlar ve bu sayaçlardan herhangi biri yerleşik yüksek eşikten (%80%), sayaçlar normal düzeylere dönene kadar `429 "Too Busy"` bir Yanıt ile reddedilir.<br/><sup>\*</sup> Bir tüketim planında varsayılan değer `true`. Adanmış bir planda varsayılan değer `false`.|
|HSTS|etkin değil|`isEnabled` `true`olarak ayarlandığında, [.NET Core 'un http katı taşıma güvenliği (HSTS) davranışı](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.0&tabs=visual-studio#hsts) [`HstsOptions` sınıfında](/dotnet/api/microsoft.aspnetcore.httpspolicy.hstsoptions?view=aspnetcore-3.0)tanımlandığı gibi zorlanır. Yukarıdaki örnek ayrıca [`maxAge`](/dotnet/api/microsoft.aspnetcore.httpspolicy.hstsoptions.maxage?view=aspnetcore-3.0#Microsoft_AspNetCore_HttpsPolicy_HstsOptions_MaxAge) özelliğini 10 gün olarak ayarlar. `hsts` desteklenen özellikleri şunlardır: <table><tr><th>Özellik</th><th>Açıklama</th></tr><tr><td>Excludedkonakları</td><td>HSTS üstbilgisinin eklendiği ana bilgisayar adlarının dize dizisi.</td></tr><tr><td>includeSubDomains</td><td>Strict-Transport-Security üstbilgisinin ıncludealt etki alanı parametresinin etkinleştirilip etkinleştirilmeyeceğini gösteren Boolean değer.</td></tr><tr><td>maxAge</td><td>Strict-Transport-Security üstbilgisinin Max-Age parametresini tanımlayan dize.</td></tr><tr><td>preload</td><td>Strict-Transport-Security üstbilgisinin preload parametresinin etkin olup olmadığını gösteren Boolean.</td></tr></table>|
|maxConcurrentRequests|100<sup>\*</sup>|Paralel olarak yürütülen HTTP işlevlerinin maksimum sayısı. Bu değer, kaynak kullanımının yönetilmesine yardımcı olabilecek eşzamanlılık denetlemenize olanak tanır. Örneğin, eşzamanlılık çok yüksek olduğunda sorunlara yol açacağından çok sayıda sistem kaynağı (bellek/CPU/yuva) kullanan bir HTTP işleviniz olabilir. Ya da bir üçüncü taraf hizmetine giden istekleri yapan bir işleviniz olabilir ve bu çağrıların hız sınırlı olması gerekir. Bu durumlarda, burada bir kısıtlama uygulanması yardımcı olabilir. <br/><sup>*</sup> Tüketim planı için varsayılan değer 100 ' dir. Adanmış bir plan için varsayılan değer sınırsız (`-1`).|
|maxOutstandingRequests|200<sup>\*</sup>|Belirli bir zamanda tutulan bekleyen istek sayısı üst sınırı. Bu sınır, kuyruğa alınmış ancak yürütmeyi başlatmayan isteklerin yanı sıra devam eden yürütmeler içerir. Bu sınırın üzerindeki tüm gelen istekler, 429 "çok meşgul" yanıtıyla reddedilir. Bu, çağıranların zamana dayalı yeniden deneme stratejileri kullanmasına izin verir ve ayrıca en fazla istek gecikme sürelerini denetlemenize yardımcı olur. Bu, yalnızca betik ana bilgisayar yürütme yolu içinde oluşan kuyruğu denetler. ASP.NET istek kuyruğu gibi diğer kuyruklar da etkin olmaya devam eder ve bu ayardan etkilenmez. <br/><sup>\*</sup> Tüketim planı için varsayılan değer 200 ' dir. Adanmış bir plan için varsayılan değer sınırsız (`-1`).|
|routePrefix|API|Tüm yollar için geçerli olan rota öneki. Varsayılan ön eki kaldırmak için boş bir dize kullanın. |

## <a name="next-steps"></a>Sonraki adımlar

- [HTTP isteğinden bir işlev çalıştırma](./functions-bindings-http-webhook-trigger.md)