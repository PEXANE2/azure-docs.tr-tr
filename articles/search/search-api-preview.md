---
title: REST API Önizleme özellikleri
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama Service REST API sürüm 2019-05-06-önizleme, bilgi deposu ve artımlı zenginleştirme için Dizin Oluşturucu önbelleği gibi deneysel özellikleri içerir.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/03/2020
ms.openlocfilehash: fd21a4b821e1911e94d542a0922e5269786c365d
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76991074"
---
# <a name="preview-features-in-azure-cognitive-search"></a>Azure Bilişsel Arama Önizleme özellikleri

Bu makalede, şu anda önizleme aşamasında olan özellikler listelenmiştir. Önizlemeden genel kullanılabilirliğe geçiş yapan özellikler bu listeden kaldırılır. [Hizmet güncelleştirmelerini](https://azure.microsoft.com/updates/?product=search) veya genel kullanıma yönelik duyurulara [yönelik yenilikler '](whats-new.md) i kontrol edebilirsiniz.

Portalda ve .NET SDK 'sında bazı Önizleme özellikleri kullanılabilir olsa da REST API her zaman önizleme özelliklerine sahiptir. 

+ Arama işlemleri için geçerli önizleme API sürümü [`2019-05-06-Preview`](https://docs.microsoft.com/rest/api/searchservice/index-2019-05-06-preview)
+ Yönetim işlemleri için geçerli önizleme sürümü [`2019-10-01-Preview`](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview)

> [!IMPORTANT]
> Önizleme işlevselliği, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="ai-enrichment-features"></a>AI zenginleştirme özellikleri

[Önizleme arama API 'si](https://docs.microsoft.com/rest/api/searchservice/index-2019-05-06-preview)aracılığıyla AI zenginleştirme ile ilgili en son geliştirmeleri keşfedelim.

+ [Özel varlık arama becerisi (Önizleme)](cognitive-search-skill-custom-entity-lookup.md ) , özel, Kullanıcı tanımlı bir sözcük ve tümcecik listesinden metin arayan bilişsel bir yetenişdir. Bu listeyi kullanarak tüm belgeleri eşleşen varlıklarla Etiketler. Bu beceri, benzer ancak tam olmayan eşleşmeleri bulmak için uygulanabilecek belirsiz eşleştirmeyi de destekler. 

+ [PII algılama yeteneği (Önizleme)](cognitive-search-skill-pii-detection.md) , dizin oluşturma sırasında bir giriş metninde kişisel olarak tanımlanabilir bilgiler çıkaran ve bu metinden çeşitli yollarla maske sağlamak için kullanılan bilişsel bir yetenseçenektir.

+ [Artımlı zenginleştirme (Önizleme)](cognitive-search-incremental-indexing-conceptual.md) bir zenginleştirme işlem hattına önbelleğe alma özelliği ekler ve bir beceri veya başka bir nesne için bir güncelleştirme gibi hedeflenen bir değişiklik, içeriği değiştirmezse mevcut çıktıyı yeniden kullanmanıza olanak tanır. Önbelleğe alma yalnızca bir beceri tarafından üretilen zenginleştirilmiş belgeler için geçerlidir.

+ [Bilgi deposu (Önizleme)](knowledge-store-concept-intro.md) , bir AI tabanlı zenginleştirme işlem hattının yeni bir hedefi. Fiziksel veri yapısı Azure Blob depolama ve Azure Tablo depolamada bulunur ve ekli bilişsel beceri sahip bir Dizin Oluşturucu çalıştırdığınızda oluşturulur ve doldurulur. Bilgi deposunun kendisi tanımı bir beceri tanımı içinde belirtilir. Bilgi deposu tanımı içinde, verilerin nasıl şekillendirilmiş olduğunu, verilerin tablo depolama veya blob depolama alanında mi depolandığını ve birden çok görünüm olup olmadığını belirten *projeksiyon* öğeleri aracılığıyla verilerinizin fiziksel yapılarını kontrol edersiniz.

## <a name="indexing-and-query-features"></a>Dizin oluşturma ve sorgu özellikleri

Dizin Oluşturucu Önizleme özellikleri, önizleme arama API 'sinde bulunabilir. 

+ [Cosmos DB Dizin Oluşturucu](search-howto-index-cosmosdb.md) MongoDB API 'si (Önizleme), Gremlin API (Önizleme) ve Cassandra API (Önizleme) destekler.

+ [Azure Data Lake Storage 2. Dizin Oluşturucu (Önizleme)](search-howto-index-azure-data-lake-storage.md) , Data Lake Storage 2. içerik ve meta verileri dizinleyebilir.

+ [Morelikethis sorgu parametresi (Önizleme)](search-more-like-this.md) belirli bir belgeyle ilgili belgeleri bulur. Bu özellik daha önceki önizlemelerde yer aldı. 

## <a name="management-features"></a>Yönetim özellikleri

+ Yönetim REST API [`api-version=2019-10-01-Preview`](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview) aracılığıyla [Özel uç nokta desteği](service-create-private-endpoint.md) . Uç noktasına nasıl erişildiği konusunda kısıtlamalar içeren bir hizmet oluşturabilirsiniz.

## <a name="earlier-preview-features"></a>Önceki Önizleme özellikleri

Daha önceki önizlemelerde duyurulan özellikler genel kullanıma sunulmadığı takdirde hala genel önizlemede. Önceki bir Preview API sürümü ile bir API 'yi arıyorsanız, bu sürümü kullanmaya devam edebilir veya beklenen davranışa hiçbir değişiklik yapmadan `2019-05-06-Preview` geçebilirsiniz.

## <a name="how-to-call-a-preview-api"></a>Önizleme API 'sini çağırma

Daha eski önizlemeler hala çalışır, ancak zaman içinde eski hale gelir. Kodunuz `api-version=2016-09-01-Preview` veya `api-version=2017-11-11-Preview`çağırırsa, bu çağrılar hala geçerlidir. Ancak, geliştirmelerle yalnızca en yeni önizleme sürümü yenilenir. 

Aşağıdaki örnek sözdizimi, önizleme API sürümüne yapılan çağrıyı gösterir.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2019-05-06-Preview

Azure Bilişsel Arama hizmeti birden çok sürümde kullanılabilir. Daha fazla bilgi için bkz. [API sürümleri](search-api-versions.md).

## <a name="next-steps"></a>Sonraki adımlar

Arama REST API başvuru belgelerini gözden geçirin. Sorunlarla karşılaşırsanız [StackOverflow](https://stackoverflow.com/) hakkında yardım isteyin veya [desteğe başvurun](https://azure.microsoft.com/support/community/?product=search).

> [!div class="nextstepaction"]
> [Arama hizmeti REST API başvurusu](https://docs.microsoft.com/rest/api/searchservice/)