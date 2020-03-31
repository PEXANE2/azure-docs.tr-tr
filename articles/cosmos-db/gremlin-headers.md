---
title: Azure Cosmos DB Gremlin yanıt başlıkları
description: Ek sorun giderme sağlayan sunucu yanıt meta verileri için başvuru belgeleri
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: reference
ms.date: 09/03/2019
author: luisbosquez
ms.author: lbosq
ms.openlocfilehash: 95677f4c45c0213de5ffac5521bac1c6bf7294e4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72755091"
---
# <a name="azure-cosmos-db-gremlin-server-response-headers"></a>Azure Cosmos DB Gremlin sunucu yanıt üstgeçitleri
Bu makalede, istek yürütmesi sırasında Cosmos DB Gremlin sunucusunun çağırana döndürdüğü başlıklar ele alınır. Bu üst bilgiler, istek performansı sorunlarının giderilmesi, Cosmos DB hizmetiyle yerel olarak tümleştirilen uygulamaların oluşturulması ve müşteri desteğinin basitleştirilmesi için kullanışlıdır.

Bu üstelere bağımlı olduğunuzu ve uygulamanızın taşınabilirliğini diğer Gremlin uygulamalarıyla sınırladığınızı unutmayın. Karşılığında, Cosmos DB Gremlin ile daha sıkı entegrasyon kazanıyor. Bu başlıklar TinkerPop standardı değildir.

## <a name="headers"></a>Üst bilgiler

| Üst bilgi | Tür | Örnek Değer | Dahil Edildiğinde | Açıklama |
| --- | --- | --- | --- | --- |
| **x-ms-istek-şarj** | double | 11.3243 | Başarı ve Hata | Kısmi yanıt iletisi için [istek birimlerinde (RU/s veya RUs)](request-units.md) tüketilen toplama veya veritabanı iş miktarı. Bu üstbilgi, birden çok öbek olan istekler için her devamı bulunur. Belirli bir yanıt yığınının yükünü yansıtır. Yalnızca tek bir yanıt ödeneğinden oluşan istekler için bu üstbilgi, toplam geçiş maliyetiyle eşleşir. Ancak, karmaşık geçişlerin çoğunluğu için bu değer kısmi bir maliyeti temsil eder. |
| **x-ms-toplam-istek-şarj** | double | 423.987 | Başarı ve Hata | [İstek birimlerinde (RU/s veya RUs)](request-units.md) tüm istek için tüketilen toplama veya veritabanı iş miktarı. Bu üstbilgi, birden çok öbek olan istekler için her devamı bulunur. İsteğin başlangıcından bu yana kümülatif ücret gösterir. Bu üstbilginin son yığındaki değeri tam istek ücretini gösterir. |
| **x-ms-sunucu-zaman-ms** | double | 13.75 | Başarı ve Hata | Bu üstbilgi, gecikme sorunu giderme amacıyla dahil edilir. Cosmos DB Gremlin sunucusunun kısmi bir yanıt iletisi yürütmek ve üretmek için aldığı süreyi milisaniye cinsinden gösterir. Bu üstbilginin değerini kullanarak ve genel istek gecikme satıruygulamalarıyla karşılaştırmak ağ gecikme yükü hesaplayabilir. |
| **x-ms-toplam-sunucu-zaman-ms** | double | 130.512 | Başarı ve Hata | Toplam süre, milisaniye cinsinden, Cosmos DB Gremlin sunucusunun tüm geçişi yürütmek için aldığı. Bu üstbilgi her kısmi yanıta dahildir. İsteğin başlangıcından bu yana kümülatif yürütme süresini temsil eder. Son yanıt, toplam yürütme süresini gösterir. Bu üstbilgi, istemci ve sunucu arasında bir gecikme kaynağı olarak ayırt etmek için yararlıdır. İstemcinin geçiş yürütme süresini bu üstbilginin değeriyle karşılaştırabilirsiniz. |
| **x-ms-durum kodu** | long | 200 | Başarı ve Hata | Üstbilgi, istek tamamlama veya sonlandırma için iç nedeni gösterir. Uygulama bu üstbilginin değerine bakmak ve düzeltici eylemde bulunmalı önerilir. |
| **x-ms-alt durum kodu** | long | 1003 | Yalnızca Hata | Cosmos DB, birleşik depolama katmanının üzerine inşa edilmiş çok modelli bir veritabanıdır. Bu üstbilgi, hata yüksek kullanılabilirlik yığınının alt katmanları içinde oluştuğunda hata nedeni hakkında ek bilgiler içerir. Uygulamanın bu üstbilgide saklanması ve Cosmos DB müşteri desteğiyle iletişime geçerken kullanması önerilir. Bu üstbilginin değeri, cosmos DB mühendisi için hızlı sorun giderme için yararlıdır. |
| **x-ms-retry-sonrası-ms** | dize (TimeSpan) | "00:00:03.9500000" | Yalnızca Hata | Bu üstbilgi,.NET [TimeSpan](https://docs.microsoft.com/dotnet/api/system.timespan) türünün dize gösterimidir. Bu değer yalnızca verilen iş artışı nedeniyle başarısız olan isteklere dahil edilecektir. Başvuru, talimat süresinden sonra geçiş tekrar gönderilmelidir. |
| **x-ms-etkinlik-id** | dize (Kılavuz) | "A9218E01-3A3A-4716-9636-5BD86B056613" | Başarı ve Hata | Üstbilgi, bir isteğin sunucu tarafındaki benzersiz bir tanımlayıcısı içerir. Her istek, izleme amacıyla sunucu tarafından benzersiz bir tanımlayıcı atanır. Uygulamalar, müşterilerin müşteri desteğine başvurmak isteyebileceği istekler için sunucu tarafından döndürülen etkinlik tanımlayıcılarını günlüğe kaydetmelidir. Cosmos DB destek personeli, cosmos DB servis telemetrisinde bu tanımlayıcılar tarafından belirli istekler bulabilir. |

## <a name="status-codes"></a>Durum kodları

Sunucu tarafından döndürülen en yaygın durum kodları aşağıda listelenmiştir.

| Durum | Açıklama |
| --- | --- |
| **401** | Kimlik `"Unauthorized: Invalid credentials provided"` doğrulama parolası Cosmos DB hesap anahtarıyla eşleşmediğinde hata iletisi döndürülür. Azure portalındaki Cosmos DB Gremlin hesabınıza gidin ve anahtarın doğru olduğunu onaylayın.|
| **404** | Aynı kenarı veya tepe yi aynı anda silmeye ve güncelleştirmeye çalışan eşzamanlı işlemler. `"Owner resource does not exist"` hata iletisi bağlantı parametrelerinde `/dbs/<database name>/colls/<collection or graph name>` biçiminde belirtilen veritabanı veya koleksiyonun doğru olmadığını gösterir.|
| **408** | `"Server timeout"`geçişin **30 saniyeden** fazla sürdüğünü ve sunucu tarafından iptal edildiğini gösterir. Arama kapsamını daraltmak için geçişlerinizi her geçiş atlamada vertices veya kenarları filtreleyerek hızlı çalışması için optimize edin.|
| **409** | `"Conflicting request to resource has been attempted. Retry to avoid conflicts."` Bu durum genellikle grafta zaten aynı tanımlayıcıya sahip bir köşe veya kenar bulunduğunda oluşur.| 
| **412** | Durum kodu hata iletisi `"PreconditionFailedException": One of the specified pre-condition is not met`ile tamamlanır. Bu hata, bir kenarı veya tepe noktasını okuma ve değişiklikten sonra mağazaya geri yazma arasında iyimser bir eşzamanlılık denetimi ihlalinin göstergesidir. Bu hata oluştuğunda en sık karşılaşılan `g.V('identifier').property('name','value')`durumlar özellik değişikliğidir, örneğin. Gremlin motoru tepe yi okur, değiştirir ve cevap yazardı. Aynı tepe noktası veya kenarı yazmaya çalışırken paralel olarak çalışan başka bir geçiş varsa, bunlardan biri bu hatayı alır. Uygulama yine sunucuya geçiş göndermelidir.| 
| **429** | İstek azaltıldı ve **x-ms-retry-after-ms** değeri kadar süre sonra yeniden denenmelidir| 
| **500** | `"NotFoundException: Entity with the specified id does not exist in the system."` ifadesini içeren hata iletisi bir veritabanı ve/veya koleksiyonun aynı adla yeniden oluşturulduğunu gösterir. Değişikliğin dağıtılması ve farklı Cosmos DB bileşenlerindeki önbellekleri geçersiz kılmasıyla bu hata 5 dakika içinde görüntüden kaldırılır. Bu sorundan kaçınmak için her zaman benzersiz veritabanı ve koleksiyon adları kullanın.| 
| **1000** | Sunucu bir iletiyi başarıyla ayrıştırdığında ancak yürütülemediğinde bu durum kodu döndürülür. Genellikle sorguyla ilgili bir sorunu gösterir.| 
| **1001** | Bu kod, sunucu geçiş yürütmesini tamamladığında döndürülür, ancak istemciye yanıtı seri hale getiremediğinde. Bu hata, geçiş çok büyük veya TinkerPop protokol belirtimine uymayan karmaşık bir sonuç oluşturduğunda ortaya çıkabilir. Uygulama, bu hatayla karşılaştığında geçişi basitleştirmelidir. | 
| **1003** | `"Query exceeded memory limit. Bytes Consumed: XXX, Max: YYY"`geçiş izin verilen bellek sınırını aştığında döndürülür. Bellek sınırı geçiş başına **2 GB'dır.**| 
| **1004** | Bu durum kodu hatalı grafik isteğini gösterir. İstek deserialization başarısız olduğunda yanlış biçimlendirilmiş olabilir, değer olmayan türü değer türü veya desteklenmeyen gremlin işlemi talep olarak deserialized ediliyor. Uygulama, başarılı olmayacaktır, çünkü isteği yeniden denememelidir. | 
| **1007** | Genellikle bu durum kodu hata `"Could not process request. Underlying connection has been closed."`iletisi ile döndürülür. İstemci sürücüsü sunucu tarafından kapatılan bir bağlantıyı kullanmaya çalışırsa bu durum olabilir. Uygulama, geçişi farklı bir bağlantıda yeniden denemelidir.
| **1008** | Cosmos DB Gremlin sunucusu kümedeki trafiği yeniden dengelemek için bağlantıları sonlandırabilir. İstemci sürücüleri bu durumu ele almalı ve sunucuya istek göndermek için yalnızca canlı bağlantıları kullanmalıdır. Bazen istemci sürücüleri bağlantının kapalı olduğunu algılayamayabilir. Uygulama bir hatayla `"Connection is too busy. Please retry after sometime or open more connections."` karşılaştığında, farklı bir bağlantıda geçişi yeniden denemelidir.

## <a name="samples"></a>Örnekler

Bir durum özniteliğini okuyan Gremlin.Net dayalı örnek istemci uygulaması:

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

Gremlin java istemcisinden durum özniteliğinin nasıl okunduğunu gösteren bir örnek:

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
* [Azure Cosmos DB için HTTP durum kodları](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb) 
* [Ortak Azure Cosmos DB REST yanıt üstbilgi](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers)
* [TinkerPop Grafik Sürücü Sağlayıcı Gereksinimleri]( http://tinkerpop.apache.org/docs/current/dev/provider/#_graph_driver_provider_requirements)
