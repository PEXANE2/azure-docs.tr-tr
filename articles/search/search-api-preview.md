---
title: REST API'deki önizleme özellikleri
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama hizmeti REST API Sürüm 2019-05-06-Önizleme, bilgi deposu ve artımlı zenginleştirme için dizin oluşturma önbelleğe alma gibi deneysel özellikleri içerir.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/11/2020
ms.openlocfilehash: db941152186127302680b5e659e43cd2d82a8908
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77162285"
---
# <a name="preview-features-in-azure-cognitive-search"></a>Azure Bilişsel Arama'da önizleme özellikleri

Bu makalede, şu anda önizlemedeki özellikler listelenebedilir. Önizlemeden genel kullanılabilirliğe geçiş yapan özellikler bu listeden kaldırılır. Genel kullanılabilirlikle ilgili duyurular için [Hizmet Güncelleştirmeleri](https://azure.microsoft.com/updates/?product=search) veya [Yenilikleri](whats-new.md) kontrol edebilirsiniz.

Bazı önizleme özellikleri portalda ve .NET SDK'da kullanılabilir olsa da, REST API'nin her zaman önizleme özellikleri vardır.

+ Arama işlemleri [**`2019-05-06-Preview`**](https://docs.microsoft.com/rest/api/searchservice/index-2019-05-06-preview) için geçerli önizleme sürümüdür.
+ Yönetim işlemleri [**`2019-10-01-Preview`**](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview) için geçerli önizleme sürümüdür.

> [!IMPORTANT]
> Önizleme işlevi hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="ai-enrichment-features"></a>AI zenginleştirme özellikleri

[Önizleme Arama API'si](https://docs.microsoft.com/rest/api/searchservice/index-2019-05-06-preview)aracılığıyla AI zenginleştirmeiçin en son geliştirmeleri keşfedin.

|||
|-|-|
| [Özel Varlık Arama becerisi (önizleme)](cognitive-search-skill-custom-entity-lookup.md ) | Özel, kullanıcı tarafından tanımlanmış sözcük ve tümcecikler listesinden metin arayan bilişsel beceri. Bu listeyi kullanarak, tüm belgeleri eşleşen varlıklarla etiketler. Beceri aynı zamanda benzer ama oldukça kesin olmayan eşleşmeleri bulmak için uygulanabilir bulanık eşleştirme bir dereceyi destekler. | 
| [PII Algılama becerisi (önizleme)](cognitive-search-skill-pii-detection.md) | Bir giriş metninden kişisel olarak tanımlanabilir bilgileri ayıklayan ve bu metinden çeşitli şekillerde gizleme seçeneği veren dizin oluşturma sırasında kullanılan bilişsel bir beceri.| 
| [Artımlı zenginleştirme (önizleme)](cognitive-search-incremental-indexing-conceptual.md) | Bir zenginleştirme ardışık boru hattına önbelleğe alma ekler, bir beceri veya başka bir nesneye güncelleştirme gibi hedeflenen bir değişiklik içeriği değiştirmezse varolan çıktıyı yeniden kullanmanıza olanak sağlar. Önbelleğe alma, yalnızca bir skillset tarafından üretilen zenginleştirilmiş belgeler için geçerlidir.| 
| [Bilgi deposu (önizleme)](knowledge-store-concept-intro.md) | AI tabanlı zenginleştirme boru hattının yeni bir varış noktası. Fiziksel veri yapısı Azure Blob depolama ve Azure Tablo depolama bulunmaktadır ve bağlı bilişsel becerilere sahip bir dizin oluşturucu çalıştırdığınızda oluşturulur ve doldurulur. Bir bilgi deposunun tanımı bir skillset tanımı içinde belirtilir. Bilgi deposu tanımı içinde, verilerinizin nasıl şekillenip şekillendirilemeyeceğini, verilerin Tablo depolama alanında mı yoksa Blob depolamaalanında mı depolanıp depolanmasını ve birden çok görünüm olup olmadığını belirleyen *projeksiyon* öğeleri aracılığıyla verilerinizin fiziksel yapılarını denetlersiniz.| 

## <a name="indexing-and-query-features"></a>Dizin oluşturma ve sorgu özellikleri

Dizinleyici önizleme özellikleri önizleme Arama API'sinde kullanılabilir. 

|||
|-|-|
| [Azure Cosmos DB dizin oluşturucu](search-howto-index-cosmosdb.md) | MongoDB API (önizleme), Gremlin API (önizleme) ve Cassandra API (önizleme) API türleri için destek. | 
|  [Azure Veri Gölü Depolama Gen2 dizinleyici (önizleme)](search-howto-index-azure-data-lake-storage.md) | Data Lake Storage Gen2'den dizin içeriği ve meta veriler.| 
| [moreLikeThis sorgu parametresi (önizleme)](search-more-like-this.md) | Belirli bir belgeyle ilgili belgeleri bulur. Bu özellik daha önceki önizlemelerde yer almıştır. | 

## <a name="management-features"></a>Yönetim özellikleri

|||
|-|-|
| [Özel Endpoint desteği](service-create-private-endpoint.md) | Güvenli bir istemci (sanal makine gibi) ile sanal ağ oluşturabilir ve ardından Özel Bitiş Noktası kullanan bir arama hizmeti oluşturabilirsiniz. |
| IP erişim kısıtlaması | Yönetim [`api-version=2019-10-01-Preview`](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview) REST API'sini kullanarak, IP adreslerine erişime izin verilen kısıtlamalariçeren bir hizmet oluşturabilirsiniz. |

## <a name="earlier-preview-features"></a>Önceki önizleme özellikleri

Önceki önizlemelerde duyurulan özellikler, genel kullanılabilirliğe geçmemişse, genel önizlemede kalır. Daha önceki bir önizleme api sürümüne sahip bir API'yi arıyorsanız, bu `2019-05-06-Preview` sürümü kullanmaya devam edebilir veya beklenen davranışta değişiklik olmadan geçiş yapabilirsiniz.

## <a name="how-to-call-a-preview-api"></a>Önizleme API'si nasıl çağrılır?

Eski önizlemeler hala çalışır durumda, ancak zamanla bayatlaşıyor. Kod aramalarınız `api-version=2016-09-01-Preview` `api-version=2017-11-11-Preview`veya bu aramalar hala geçerliyse. Ancak, yalnızca en yeni önizleme sürümü geliştirmelerle yenilenir. 

Aşağıdaki örnek sözdizimi, önizleme API sürümüne yapılan bir çağrıyı göstermektedir.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2019-05-06-Preview

Azure Bilişsel Arama hizmeti birden çok sürümde kullanılabilir. Daha fazla bilgi için [API sürümlerine](search-api-versions.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Arama REST API başvuru belgelerini gözden geçirin. Sorunlarla karşılaşırsanız, [StackOverflow](https://stackoverflow.com/) hakkında bizden yardım isteyin veya [desteğe başvurun.](https://azure.microsoft.com/support/community/?product=search)

> [!div class="nextstepaction"]
> [Arama hizmeti REST API Başvuru](https://docs.microsoft.com/rest/api/searchservice/)