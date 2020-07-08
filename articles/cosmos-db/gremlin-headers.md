---
title: Azure Cosmos DB Gremlin yanıt üst bilgileri
description: Ek sorun gidermeyi sağlayan sunucu yanıtı meta verileri için başvuru belgeleri
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: reference
ms.date: 09/03/2019
author: luisbosquez
ms.author: lbosq
ms.openlocfilehash: d244a5bfb6d0a1e2a0965cc72a8f223e0646fa77
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85390865"
---
# <a name="azure-cosmos-db-gremlin-server-response-headers"></a>Azure Cosmos DB Gremlin sunucu yanıtı üstbilgileri
Bu makalede, istek yürütmesi sırasında Cosmos DB Gremlin sunucusunun çağırana döndürdüğü başlıklar ele alınır. Bu üst bilgiler, istek performansı sorunlarının giderilmesi, Cosmos DB hizmetiyle yerel olarak tümleştirilen uygulamaların oluşturulması ve müşteri desteğinin basitleştirilmesi için kullanışlıdır.

Bu üst bilgilere bağımlılık alan diğer Gremlin uygulamalarına yönelik taşınabilirlik sınırlandırdığını aklınızda bulundurun. Sonuç olarak, Cosmos DB Gremlin ile daha sıkı bir tümleştirme elde edersiniz. Bu üstbilgiler bir TinkerPop standardı değildir.

## <a name="headers"></a>Üst bilgiler

| Üst bilgi | Tür | Örnek değer | Dahil edildiğinde | Açıklama |
| --- | --- | --- | --- | --- |
| **x-MS-istek ücreti** | double | 11,3243 | Başarı ve Hata | Kısmi bir yanıt iletisi için [istek birimlerinde (ru/s veya ru)](request-units.md) tüketilen koleksiyon veya veritabanı aktarım hızı. Bu üst bilgi, birden çok öbekteki isteklerin her devamlıında bulunur. Belirli bir yanıt öbeğinin ücretlendirisini yansıtır. Yalnızca tek bir yanıt öbeklerinden oluşan istekler için bu üst bilgi, toplam çapraz geçiş maliyetiyle eşleşir. Ancak, karmaşık traversals çoğu için bu değer kısmi bir maliyeti temsil eder. |
| **x-MS-toplam-istek ücreti** | double | 423,987 | Başarı ve Hata | İsteğin tamamına yönelik [İstek birimleri (ru/s veya ru)](request-units.md) cinsinden tüketilen koleksiyon veya veritabanı verimlilik miktarı. Bu üst bilgi, birden çok öbekteki isteklerin her devamlıında bulunur. İsteğin başlangıcından bu yana birikmiş ücreti gösterir. Son öbekteki bu üstbilginin değeri, tüm istek ücretlendirdiğini gösterir. |
| **x-MS-sunucu-zaman-MS** | double | 13,75 | Başarı ve Hata | Bu üst bilgi, gecikme sorun giderme amaçları için eklenmiştir. Cosmos DB Gremlin Server 'ın yürütülmesi ve kısmi bir yanıt iletisi üretmek için geçen milisaniye cinsinden süreyi belirtir. Bu üst bilginin değerini kullanmak ve genel istek gecikme süresi uygulamalarıyla karşılaştırmak, ağ gecikmesi yükünü hesaplayabilir. |
| **x-MS-Total-Server-Time-MS** | double | 130,512 | Başarı ve Hata | Cosmos DB Gremlin sunucusunun tüm çapraz geçişi yürütebilmesi için geçen toplam süre (milisaniye cinsinden). Bu üst bilgi, her kısmi yanıta dahildir. İstek başlangıcından bu yana toplu yürütme süresini temsil eder. Son Yanıt toplam yürütme süresini gösterir. Bu üst bilgi, bir gecikme kaynağı olarak istemci ve sunucu arasında ayrım yapmak için yararlıdır. İstemci üzerindeki çapraz geçiş süresini, bu üst bilginin değerine göre karşılaştırabilirsiniz. |
| **x-MS-Status-Code** | long | 200 | Başarı ve Hata | Üstbilgi, istek tamamlamada veya sonlandırmasının iç nedenini gösterir. Uygulamanın bu üst bilgiyi değere bakmamız ve düzeltici eylem yapması önerilir. |
| **x-MS-alt durumu-kod** | long | 1003 | Yalnızca hata | Cosmos DB, Birleşik depolama katmanının üzerine yerleştirilmiş çok modelli bir veritabanıdır. Bu üst bilgi, yüksek kullanılabilirlik yığınının alt katmanlarında hata oluştuğunda oluşan hata nedeni hakkında ek bilgiler içerir. Uygulamanın bu üstbilgiyi depolaması ve Cosmos DB müşteri desteğiyle iletişim kurarken kullanması önerilir. Bu üstbilginin değeri, hızlı sorun giderme için Cosmos DB mühendis için yararlıdır. |
| **x-MS-yeniden deneme-sonra-MS** | dize (TimeSpan) | "00:00:03.9500000" | Yalnızca hata | Bu üst bilgi, bir .NET [TimeSpan](https://docs.microsoft.com/dotnet/api/system.timespan) türünün dize gösterimidir. Bu değer yalnızca sağlanan üretilen iş tükenmesi nedeniyle başarısız olan isteklere dahil edilecek. Uygulama, geçen süre sonunda çapraz geçişi yeniden göndermelidir. |
| **x-MS-etkinlik kimliği** | dize (GUID) | "A9218E01-3A3A-4716-9636-5BD86B056613" | Başarı ve Hata | Üst bilgi, bir isteğin benzersiz sunucu tarafı tanımlayıcısını içerir. Her isteğe, izleme amacıyla sunucu tarafından benzersiz bir tanımlayıcı atanır. Uygulamalar, müşterilerin müşteri desteğiyle iletişim kurmak isteyebileceğiniz istekler için sunucu tarafından döndürülen etkinlik tanımlayıcılarını günlüğe içermemelidir. Cosmos DB destek personeli, Bu tanımlayıcılara yönelik belirli istekleri Cosmos DB hizmeti telemetrisinde bulabilir. |

## <a name="status-codes"></a>Durum kodları

Sunucu tarafından döndürülen en yaygın durum kodları aşağıda listelenmiştir.

| Durum | Açıklama |
| --- | --- |
| **401** | `"Unauthorized: Invalid credentials provided"`Kimlik doğrulama parolası Cosmos DB hesap anahtarıyla eşleşmiyorsa hata iletisi döndürülür. Azure portal Gremlin hesabınıza Cosmos DB gidin ve anahtarın doğru olduğundan emin olun.|
| **404** | Aynı kenarı veya köşeyi aynı anda silmeye ve güncelleştirmeye çalışacak eşzamanlı işlemler. `"Owner resource does not exist"` hata iletisi bağlantı parametrelerinde `/dbs/<database name>/colls/<collection or graph name>` biçiminde belirtilen veritabanı veya koleksiyonun doğru olmadığını gösterir.|
| **408** | `"Server timeout"`çapraz geçişinin **30 saniyeden** fazla sürdüğünü ve sunucu tarafından iptal edildiğini gösterir. Çapraz arama kapsamını daraltmak için her geçiş atlamada köşeleri veya kenarları filtreleyerek, traversals uygulamanızı en uygun şekilde gerçekleştirin.|
| **409** | `"Conflicting request to resource has been attempted. Retry to avoid conflicts."` Bu durum genellikle grafta zaten aynı tanımlayıcıya sahip bir köşe veya kenar bulunduğunda oluşur.| 
| **412** | Durum kodu hata iletisiyle birlikte tamamlanır `"PreconditionFailedException": One of the specified pre-condition is not met` . Bu hata, bir kenar veya köşeyi okumak ve değiştirildikten sonra depoya geri yazmak arasındaki iyimser eşzamanlılık denetim ihlalinin göstergesi olur. Bu hatanın oluştuğu yaygın durumlar, örneğin özellik değişimdir `g.V('identifier').property('name','value')` . Gremlin motoru köşeyi okur, değiştirir ve tekrar yazar. Paralel olarak çalışan başka bir geçiş geçişi varsa, aynı köşeyi veya bir kenarı yazmaya çalışırken, bunlardan biri bu hatayı alır. Uygulamanın çapraz geçişi sunucuya yeniden göndermesi gerekir.| 
| **429** | İstek azaltıldı ve **x-ms-retry-after-ms** değeri kadar süre sonra yeniden denenmelidir| 
| **500** | `"NotFoundException: Entity with the specified id does not exist in the system."` ifadesini içeren hata iletisi bir veritabanı ve/veya koleksiyonun aynı adla yeniden oluşturulduğunu gösterir. Değişikliğin dağıtılması ve farklı Cosmos DB bileşenlerindeki önbellekleri geçersiz kılmasıyla bu hata 5 dakika içinde görüntüden kaldırılır. Bu sorundan kaçınmak için her zaman benzersiz veritabanı ve koleksiyon adları kullanın.| 
| **1000** | Bu durum kodu, sunucu başarıyla bir ileti ayrıştırdığında ancak yürütülemediğinden döndürülür. Genellikle sorguyla ilgili bir sorun olduğunu gösterir.| 
| **1001** | Bu kod, sunucu geçiş yürütmeyi tamamladığında döndürülür, ancak yanıtı istemciye geri serileştirmezse. Geçiş, çok büyük olan veya TinkerPop protokol belirtimine uygun olmayan bir karmaşık sonuç oluşturduğunda bu hata oluşabilir. Uygulamanın bu hatayla karşılaştığında geçiş geçişini basitleştirmesi gerekir. | 
| **1003** | `"Query exceeded memory limit. Bytes Consumed: XXX, Max: YYY"`geçiş, izin verilen bellek sınırını aştığında döndürülür. Bellek sınırı, çapraz geçiş başına **2 GB** 'dir.| 
| **1004** | Bu durum kodu hatalı biçimlendirilmiş grafik isteğini gösterir. Seri durumdan çıkarma işlemi başarısız olduğunda istek hatalı olabilir, değer türü veya desteklenmeyen Gremlin işlemi istendi olarak değer olmayan türün serisi kaldırılıyor. Uygulama başarılı olmayacak, isteği yeniden denememelidir. | 
| **1007** | Genellikle bu durum kodu hata iletisiyle birlikte döndürülür `"Could not process request. Underlying connection has been closed."` . İstemci sürücüsü sunucu tarafından kapatılan bir bağlantıyı kullanmayı denerse bu durum oluşabilir. Uygulamanın çapraz geçişi farklı bir bağlantıda yeniden denemesi gerekir.
| **1008** | Cosmos DB Gremlin Server, kümedeki trafiği yeniden dengelemek için bağlantıları sonlandırabilirler. İstemci sürücüleri bu durumu işlemeli ve istekleri sunucuya göndermek için yalnızca canlı bağlantıları kullanmalıdır. Bazen istemci sürücüleri bağlantının kapatıldığını algılayamayabilir. Uygulama bir hatayla karşılaştığında, `"Connection is too busy. Please retry after sometime or open more connections."` farklı bir bağlantıda çapraz geçişi yeniden denemelidir.

## <a name="samples"></a>Örnekler

Bir durum özniteliğini okuyan Gremlin.Net tabanlı örnek bir istemci uygulaması:

```csharp
// Following example reads a status code and total request charge from server response attributes.
// Variable "server" is assumed to be assigned to an instance of a GremlinServer that is connected to Cosmos DB account.
using (GremlinClient client = new GremlinClient(server, new GraphSON2Reader(), new GraphSON2Writer(), GremlinClient.GraphSON2MimeType))
{
  ResultSet<dynamic> responseResultSet = await GremlinClientExtensions.SubmitAsync<dynamic>(client, requestScript: "g.V().count()");
  long statusCode = (long)responseResultSet.StatusAttributes["x-ms-status-code"];
  double totalRequestCharge = (double)responseResultSet.StatusAttributes["x-ms-total-request-charge"];

  // Status code and request charge are logged into application telemetry.
}
```

Gremlin Java istemcisinden durum özniteliğinin nasıl okunacağını gösteren bir örnek:

```java
try {
  ResultSet resultSet = this.client.submit("g.addV().property('id', '13')");
  List<Result> results = resultSet.all().get();

  // Process and consume results

} catch (ResponseException re) {
  // Check for known errors that need to be retried or skipped
  if (re.getStatusAttributes().isPresent()) {
    Map<String, Object> attributes = re.getStatusAttributes().get();
    int statusCode = (int) attributes.getOrDefault("x-ms-status-code", -1);

    // Now we can check for specific conditions
    if (statusCode == 409) {
        // Handle conflicting writes
      }
    }

    // Check if we need to delay retry
    if (attributes.containsKey("x-ms-retry-after-ms")) {
      // Read the value of the attribute as is
      String retryAfterTimeSpan = (String) attributes.get("x-ms-retry-after-ms"));

      // Convert the value into actionable duration
            LocalTime locaTime = LocalTime.parse(retryAfterTimeSpan);
            Duration duration = Duration.between(LocalTime.MIN, locaTime);

      // Perform a retry after "duration" interval of time has elapsed
    }
  }
}

```

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Cosmos DB için HTTP durum kodları](/rest/api/cosmos-db/http-status-codes-for-cosmosdb) 
* [Ortak Azure Cosmos DB REST yanıt üst bilgileri](/rest/api/cosmos-db/common-cosmosdb-rest-response-headers)
* [Inkerpop Graph sürücü sağlayıcısı gereksinimleri]( http://tinkerpop.apache.org/docs/current/dev/provider/#_graph_driver_provider_requirements)
