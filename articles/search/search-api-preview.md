---
title: REST API Önizleme özellikleri
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama Service REST API sürüm 2019-05-06-önizleme, bilgi deposu ve artımlı zenginleştirme için Dizin Oluşturucu önbelleği gibi deneysel özellikleri içerir.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 02/11/2020
ms.openlocfilehash: db941152186127302680b5e659e43cd2d82a8908
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77162285"
---
# <a name="preview-features-in-azure-cognitive-search"></a>Azure Bilişsel Arama Önizleme özellikleri

Bu makalede, şu anda önizleme aşamasında olan özellikler listelenmiştir. Önizlemeden genel kullanılabilirliğe geçiş yapan özellikler bu listeden kaldırılır. [Hizmet güncelleştirmelerini](https://azure.microsoft.com/updates/?product=search) veya genel kullanıma yönelik duyurulara [yönelik yenilikler '](whats-new.md) i kontrol edebilirsiniz.

Portalda ve .NET SDK 'sında bazı Önizleme özellikleri kullanılabilir olsa da REST API her zaman önizleme özelliklerine sahiptir.

+ Arama işlemleri [**`2019-05-06-Preview`**](https://docs.microsoft.com/rest/api/searchservice/index-2019-05-06-preview) için geçerli önizleme sürümüdür.
+ Yönetim işlemleri [**`2019-10-01-Preview`**](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview) için geçerli önizleme sürümüdür.

> [!IMPORTANT]
> Önizleme işlevselliği, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="ai-enrichment-features"></a>AI zenginleştirme özellikleri

[Önizleme arama API 'si](https://docs.microsoft.com/rest/api/searchservice/index-2019-05-06-preview)aracılığıyla AI zenginleştirme ile ilgili en son geliştirmeleri keşfedelim.

|||
|-|-|
| [Özel varlık arama becerisi (Önizleme)](cognitive-search-skill-custom-entity-lookup.md ) | Özel, Kullanıcı tanımlı bir sözcük ve tümcecik listesinden metin gibi görünen bilişsel bir yetenek. Bu listeyi kullanarak tüm belgeleri eşleşen varlıklarla Etiketler. Bu beceri, benzer ancak tam olmayan eşleşmeleri bulmak için uygulanabilecek belirsiz eşleştirmeyi de destekler. | 
| [PII algılama yeteneği (Önizleme)](cognitive-search-skill-pii-detection.md) | Dizin oluşturma sırasında, kişisel olarak tanımlanabilir bilgileri bir giriş metinden çıkaran ve bu metni çeşitli yollarla maskeleme seçeneği sunan bilişsel bir yetenek.| 
| [Artımlı zenginleştirme (Önizleme)](cognitive-search-incremental-indexing-conceptual.md) | Bir zenginleştirme işlem hattına önbelleğe alma ekler ve bir beceri veya başka bir nesne için bir güncelleştirme gibi hedeflenen bir değişiklik, içeriği değiştirmezse mevcut çıktıyı yeniden kullanmanıza olanak tanır. Önbelleğe alma yalnızca bir beceri tarafından üretilen zenginleştirilmiş belgeler için geçerlidir.| 
| [Bilgi deposu (Önizleme)](knowledge-store-concept-intro.md) | Bir AI tabanlı enzenginleştirme işlem hattının yeni hedefi. Fiziksel veri yapısı Azure Blob depolama ve Azure Tablo depolamada bulunur ve ekli bilişsel beceri sahip bir Dizin Oluşturucu çalıştırdığınızda oluşturulur ve doldurulur. Bilgi deposunun kendisi tanımı bir beceri tanımı içinde belirtilir. Bilgi deposu tanımı içinde, verilerin nasıl şekillendirilmiş olduğunu, verilerin tablo depolama veya blob depolama alanında mi depolandığını ve birden çok görünüm olup olmadığını belirten *projeksiyon* öğeleri aracılığıyla verilerinizin fiziksel yapılarını kontrol edersiniz.| 

## <a name="indexing-and-query-features"></a>Dizin oluşturma ve sorgu özellikleri

Dizin Oluşturucu Önizleme özellikleri, önizleme arama API 'sinde bulunabilir. 

|||
|-|-|
| [Azure Cosmos DB dizin oluşturucu](search-howto-index-cosmosdb.md) | MongoDB API (Önizleme), Gremlin API (Önizleme) ve Cassandra API (Önizleme) API türleri için destek. | 
|  [Azure Data Lake Storage 2. Dizin Oluşturucu (Önizleme)](search-howto-index-azure-data-lake-storage.md) | Data Lake Storage 2. ' den içerik ve meta verileri dizine edin.| 
| [moreLikeThis sorgu parametresi (Önizleme)](search-more-like-this.md) | Belirli bir belgeyle ilgili belgeleri bulur. Bu özellik daha önceki önizlemelerde yer aldı. | 

## <a name="management-features"></a>Yönetim özellikleri

|||
|-|-|
| [Özel uç nokta desteği](service-create-private-endpoint.md) | Güvenli bir istemci (örneğin, bir sanal makine) ile bir sanal ağ oluşturabilir ve ardından özel uç nokta kullanan bir arama hizmeti oluşturabilirsiniz. |
| IP erişim kısıtlaması | Yönetim [`api-version=2019-10-01-Preview`](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview) REST API kullanarak, erişim IZNI verilen IP adreslerine yönelik kısıtlamalara sahip bir hizmet oluşturabilirsiniz. |

## <a name="earlier-preview-features"></a>Önceki Önizleme özellikleri

Daha önceki önizlemelerde duyurulan özellikler genel kullanıma sunulmadığı takdirde hala genel önizlemede. Daha önceki bir Preview API sürümü olan bir API 'yi arıyorsanız, bu sürümü kullanmaya devam edebilir veya beklenen davranışa hiçbir değişiklik yapmadan öğesine `2019-05-06-Preview` geçebilirsiniz.

## <a name="how-to-call-a-preview-api"></a>Önizleme API 'sini çağırma

Daha eski önizlemeler hala çalışır, ancak zaman içinde eski hale gelir. Kodunuz veya `api-version=2017-11-11-Preview`çağırıyorsa `api-version=2016-09-01-Preview` , bu çağrılar hala geçerlidir. Ancak, geliştirmelerle yalnızca en yeni önizleme sürümü yenilenir. 

Aşağıdaki örnek sözdizimi, önizleme API sürümüne yapılan çağrıyı gösterir.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2019-05-06-Preview

Azure Bilişsel Arama hizmeti birden çok sürümde kullanılabilir. Daha fazla bilgi için bkz. [API sürümleri](search-api-versions.md).

## <a name="next-steps"></a>Sonraki adımlar

Arama REST API başvuru belgelerini gözden geçirin. Sorunlarla karşılaşırsanız [StackOverflow](https://stackoverflow.com/) hakkında yardım isteyin veya [desteğe başvurun](https://azure.microsoft.com/support/community/?product=search).

> [!div class="nextstepaction"]
> [Arama hizmeti REST API başvurusu](https://docs.microsoft.com/rest/api/searchservice/)