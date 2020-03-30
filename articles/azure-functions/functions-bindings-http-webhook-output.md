---
title: Azure İşlevler HTTP çıktı bağlamaları
description: Azure İşlevleri'nde HTTP yanıtlarını nasıl döndüreceklerini öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/21/2020
ms.author: cshoe
ms.openlocfilehash: a25658677e436edf4d001599bb4981f527016596
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277628"
---
# <a name="azure-functions-http-output-bindings"></a>Azure İşlevler HTTP çıktı bağlamaları

HTTP isteği gönderene yanıt vermek için HTTP çıkış bağlamasını kullanın. Bu bağlama bir HTTP tetikleyici gerektirir ve tetikleyicinin isteğiyle ilişkilendirilen yanıtı özelleştirme imkanı sunar.

HTTP tarafından tetiklenen bir işlevin varsayılan getiri değeri:

- `HTTP 204 No Content`Fonksiyonlar 2.x ve daha yüksek boş bir gövde ile
- `HTTP 200 OK`Fonksiyonlar 1.x boş bir gövde ile

## <a name="configuration"></a>Yapılandırma

Aşağıdaki *tabloda, function.json* dosyasında ayarladığınız bağlama yapılandırma özellikleri açıklanmaktadır. C# sınıfı kitaplıklar için, bu *işlev.json* özelliklerine karşılık gelen hiçbir öznitelik özellikleri vardır.

|Özellik  |Açıklama  |
|---------|---------|
| **Türü** |Ayarlanmış `http`olmalı. |
| **Yön** | Ayarlanmış `out`olmalı. |
| **Adı** | Yanıt için işlev kodunda veya `$return` iade değerini kullanmak için kullanılan değişken adı. |

## <a name="usage"></a>Kullanım

HTTP yanıtı göndermek için dil standardı yanıt modellerini kullanın. C# veya C# komut dosyasında, `IActionResult` `Task<IActionResult>`işlevin dönüş türünü veya . C#'da, iade değeri özniteliği gerekmez.

Örneğin yanıtlar, [tetikleyici örneğe](./functions-bindings-http-webhook-trigger.md#example)bakın.

## <a name="hostjson-settings"></a>host.json ayarları

Bu bölümde, bu bağlama için 2.x ve üstü sürümlerde kullanılabilen genel yapılandırma ayarları açıklanmaktadır. Aşağıdaki örnek host.json dosyası, bu bağlama için yalnızca sürüm 2.x+ ayarlarını içerir. 2.x ve sonrası sürümlerde genel yapılandırma ayarları hakkında daha fazla bilgi için [Azure İşlevleri için host.json başvurusuna](functions-host-json.md)bakın.

> [!NOTE]
> Functions 1.x'teki host.json başvurusu [için Azure İşlevler 1.x için host.json başvurusuna](functions-host-json-v1.md#http)bakın.

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
| özel Headers|yok|HTTP yanıtında özel üstbilgi ayarlamanızı sağlar. Önceki örnek, `X-Content-Type-Options` içerik türü koklamaönlemek için yanıtüstbilgi ekler. |
|dynamicThrottlesEnabled|True<sup>\*</sup>|Etkinleştirildiğinde, bu ayar istek işleme ardışık ardışık ardışık sistem performans sayaçlarını düzenli olarak kontrol eder `connections/threads/processes/memory/cpu/etc` ve bu sayaçlardan herhangi `429 "Too Busy"` biri yerleşik yüksek eşiğin (%80) üzerindeyse, sayaç normal düzeylere dönene kadar istekler yanıtla birlikte reddedilir.<br/><sup>\*</sup>Tüketim planındaki varsayılan `true`değer. Adanmış bir plandaki `false`varsayılan değer.|
|hsts|etkinleştirildi|Ne `isEnabled` zaman `true`ayarlanır , [http Sıkı Aktarım Güvenliği (HSTS) davranış .NET Core,](/aspnet/core/security/enforcing-ssl?view=aspnetcore-3.0&tabs=visual-studio#hsts) [ `HstsOptions` sınıfta](/dotnet/api/microsoft.aspnetcore.httpspolicy.hstsoptions?view=aspnetcore-3.0)tanımlandığı gibi. Yukarıdaki örnekte de [`maxAge`](/dotnet/api/microsoft.aspnetcore.httpspolicy.hstsoptions.maxage?view=aspnetcore-3.0#Microsoft_AspNetCore_HttpsPolicy_HstsOptions_MaxAge) özelliği 10 güne ayarlar. Desteklenen özellikleri `hsts` şunlardır: <table><tr><th>Özellik</th><th>Açıklama</th></tr><tr><td>hariçHosts</td><td>HSTS üstbilgisinin eklenmediği ana bilgisayar adları dizisi.</td></tr><tr><td>includeSubDomains</td><td>Katı Aktarım-Güvenlik üstbilgisinin includeSubDomain parametresinin etkin olup olmadığını gösteren boolean değeri.</td></tr><tr><td>Maxage</td><td>Sıkı Taşıma-Güvenlik üstbilgisinin maksimum yaş parametresini tanımlayan dize.</td></tr><tr><td>preload</td><td>Katı Aktarım-Güvenlik üstbilgisinin ön yük parametresinin etkin olup olmadığını gösteren boolean.</td></tr></table>|
|maxConcurrentRequests|100<sup>\*</sup>|Paralel olarak yürütülen en fazla HTTP işlevi sayısı. Bu değer, kaynak kullanımını yönetmenize yardımcı olabilecek eşzamanlılığı denetlemenize olanak tanır. Örneğin, eşzamanlılık çok yüksek olduğunda sorunlara neden olacak şekilde çok sayıda sistem kaynağı (bellek/işlemci/soket) kullanan bir HTTP işleviniz olabilir. Veya bir üçüncü taraf hizmetine giden isteklerde bulunan bir işleviniz olabilir ve bu çağrıların oran sınırı olması gerekir. Bu gibi durumlarda, burada bir gaz uygulayarak yardımcı olabilir. <br/><sup>*</sup>Tüketim planı için varsayılan değer 100'dür. Adanmış bir plan için varsayılan sınırdışı (`-1`).|
|maxOutstandingRequests|200<sup>\*</sup>|Herhangi bir zamanda tutulan en fazla bekleyen istek sayısı. Bu sınır, sıraya dizilen ancak yürütmeye başlamamış istekleri ve devam eden yürütmeleri içerir. Bu sınır üzerinden gelen tüm istekler 429 "Çok Meşgul" yanıtıyla reddedilir. Bu, arayanların zaman tabanlı yeniden deneme stratejilerini kullanmalarına olanak tanır ve aynı zamanda maksimum istek gecikmelerini denetlemenize yardımcı olur. Bu yalnızca komut dosyası ana bilgisayar yürütme yolu içinde oluşan sıraya denetler. ASP.NET istek sırası gibi diğer kuyruklar yine de etkin ve bu ayardan etkilenmez. <br/><sup>\*</sup>Tüketim planı için varsayılan değer 200'dür. Adanmış bir plan için varsayılan sınırdışı (`-1`).|
|rotaÖnek|API|Tüm rotalar için geçerli olan rota öneki. Varsayılan önekikaldırmak için boş bir dize kullanın. |

## <a name="next-steps"></a>Sonraki adımlar

- [BIR HTTP isteğinden bir işlev çalıştırma](./functions-bindings-http-webhook-trigger.md)