---
title: REST API sürüm 2019-05-06-Önizleme
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama Service REST API sürüm 2019-05-06-önizleme, bilgi deposu ve artımlı zenginleştirme için Dizin Oluşturucu önbelleği gibi deneysel özellikleri içerir.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 01/15/2020
ms.openlocfilehash: 71c6879f467823ab01f4c60ac4d9f26cffcd4eea
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76896127"
---
# <a name="azure-cognitive-search-service-rest-api-version-2019-05-06-preview"></a>Azure Bilişsel Arama Service REST API-sürüm 2019-05-06-Önizleme

Bu makalede, arama hizmeti REST API 'ın `api-version=2019-05-06-Preview` sürümü açıklanmakta ve henüz genel kullanıma sunulmayan Deneysel özellikler sunulmaktadır.

> [!NOTE]
> Önizleme özellikleri, test ve deneme ile geri bildirim toplama amacını ve değişikliğe tabi bir şekilde kullanılabilir. Üretim uygulamalarında önizleme API 'Lerini kullanmayı önemle tavsiye ederiz.

## <a name="features-in-2019-05-06-preview"></a>2019-05-06 sürümündeki özellikler-önizleme

Bu bölümde önizleme durumu olan özellikler listelenmiştir. Çoğu geçerli 2019-05-06-Preview API 'sine eklenmiştir, ancak `moreLikeThis` benzer bir şekilde en son önizleme API 'sine eklenen önceki önizleme sürümlerinden bazılarıdır. 

Önizleme özelliği genel kullanıma sunulduğunda, bu listeden kaldırılır. [Hizmet güncelleştirmelerini](https://azure.microsoft.com/updates/?product=search) veya genel kullanıma yönelik duyurulara [yönelik yenilikler '](whats-new.md) i kontrol edebilirsiniz.

+ [Artımlı zenginleştirme (Önizleme)](cognitive-search-incremental-indexing-conceptual.md) bir zenginleştirme işlem hattına önbelleğe alma özelliği ekler ve bir beceri veya başka bir nesne için bir güncelleştirme gibi hedeflenen bir değişiklik, içeriği değiştirmezse mevcut çıktıyı yeniden kullanmanıza olanak tanır. Önbelleğe alma yalnızca bir beceri tarafından üretilen zenginleştirilmiş belgeler için geçerlidir.

+ [Cosmos DB Dizin Oluşturucu](search-howto-index-cosmosdb.md) MongoDB API 'si (Önizleme), Gremlin API (Önizleme) ve Cassandra API (Önizleme) destekler.

+ [Azure Data Lake Storage 2. Dizin Oluşturucu (Önizleme)](search-howto-index-azure-data-lake-storage.md) , Data Lake Storage 2. içerik ve meta verileri dizinleyebilir.

+ [Bilgi deposu](knowledge-store-concept-intro.md) , bir AI tabanlı zenginleştirme işlem hattının yeni bir hedefi. Fiziksel veri yapısı Azure Blob depolama ve Azure Tablo depolamada bulunur ve ekli bilişsel beceri sahip bir Dizin Oluşturucu çalıştırdığınızda oluşturulur ve doldurulur. Bilgi deposunun kendisi tanımı bir beceri tanımı içinde belirtilir. Bilgi deposu tanımı içinde, verilerin nasıl şekillendirilmiş olduğunu, verilerin tablo depolama veya blob depolama alanında mi depolandığını ve birden çok görünüm olup olmadığını belirten *projeksiyon* öğeleri aracılığıyla verilerinizin fiziksel yapılarını kontrol edersiniz.

+ [Morelikethis sorgu parametresi](search-more-like-this.md) , belirli bir belgeyle ilgili belgeleri bulur. Bu özellik daha önceki önizlemelerde yer aldı. 

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