---
title: REST API sürüm 2019-05-06-Önizleme
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama Service REST API sürüm 2019-05-06-önizleme bilgi deposu ve müşteri tarafından yönetilen şifreleme anahtarları gibi deneysel özellikleri içerir.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: dff6473fd01fc2d41c5bb100eefd583afe4175b0
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73496484"
---
# <a name="azure-cognitive-search-service-rest-api-version-2019-05-06-preview"></a>Azure Bilişsel Arama Service REST API-sürüm 2019-05-06-Önizleme

Bu makalede, arama hizmeti REST API 'ın `api-version=2019-05-06-Preview` sürümü açıklanmakta ve henüz genel kullanıma sunulmayan Deneysel özellikler sunulmaktadır.

> [!NOTE]
> Önizleme özellikleri, test ve deneme ile geri bildirim toplama amacını ve değişikliğe tabi bir şekilde kullanılabilir. Üretim uygulamalarında önizleme API 'Lerini kullanmayı önemle tavsiye ederiz.


## <a name="new-in-2019-05-06-preview"></a>2019-05-06 içinde yeni-önizleme

[* * Artımlı Dizin](cognitive-search-incremental-indexing-conceptual.md) oluşturma işlemi, veri, Dizin Oluşturucu ve beceri tanımları değişmeden olduğunda var olan çıktıyı yeniden kullanmanıza olanak tanıyan bir dizin oluşturma için durum ve önbelleğe alma için yeni bir moddur. Bu özellik Bilişsel Beceri aracılığıyla yalnızca zenginleştirme için geçerlidir.

[**Bilgi deposu**](knowledge-store-concept-intro.md) , bir AI tabanlı zenginleştirme işlem hattının yeni bir hedefi. Bir dizine ek olarak, Azure depolama 'da dizin oluşturma sırasında oluşturulan doldurulmuş veri yapılarını artık kalıcı hale getirebilirsiniz. Verilerin nasıl şekillendirilmiş olduğunu, verilerin tablo depolama veya blob depolama alanında mi depolandığını, yoksa birden çok görünüm mi olduğunu da kapsayan, verilerinizin fiziksel yapılarını bir beceri öğeleri aracılığıyla kontrol edersiniz.

Hizmet tarafı şifreleme için [**müşteri tarafından yönetilen şifreleme anahtarları**](search-security-manage-encryption-keys.md) Ayrıca yeni bir önizleme özelliğidir. Microsoft tarafından yönetilen yerleşik şifrelemeye ek olarak, anahtarların tek sahibi olduğunuz sırada ek bir şifreleme katmanı da uygulayabilirsiniz.

## <a name="other-preview-features"></a>Diğer Önizleme özellikleri

Önceki önizlemelerde duyurulan Özellikler hala genel önizlemede. Önceki bir Preview API sürümü ile bir API 'yi arıyorsanız, bu sürümü kullanmaya devam edebilir veya beklenen davranışa hiçbir değişiklik yapmadan `2019-05-06-Preview` geçebilirsiniz.

+ [Morelikethis sorgu parametresi](search-more-like-this.md) , belirli bir belgeyle ilgili belgeleri bulur. Bu özellik daha önceki önizlemelerde yer aldı. 
* [CSV blobu dizin oluşturma](search-howto-index-csv-blobs.md) , her metin blobu için bir belge yerine her satır için bir belge oluşturur.
* [Cosmos DB Dizin oluşturucular Için MongoDB API desteği](search-howto-index-cosmosdb.md) önizlemededir.


## <a name="how-to-call-a-preview-api"></a>Önizleme API 'sini çağırma

Daha eski önizlemeler hala çalışır, ancak zaman içinde eski hale gelir. Kodunuz `api-version=2016-09-01-Preview` veya `api-version=2017-11-11-Preview`çağırırsa, bu çağrılar hala geçerlidir. Ancak, geliştirmelerle yalnızca en yeni önizleme sürümü yenilenir. 

Aşağıdaki örnek sözdizimi, önizleme API sürümüne yapılan çağrıyı gösterir.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2019-05-06-Preview

Azure Bilişsel Arama hizmeti birden çok sürümde kullanılabilir. Daha fazla bilgi için bkz. [API sürümleri](search-api-versions.md).

## <a name="next-steps"></a>Sonraki adımlar

Arama REST API başvuru belgelerini gözden geçirin. Sorunlarla karşılaşırsanız [StackOverflow](https://stackoverflow.com/) hakkında yardım isteyin veya [desteğe başvurun](https://azure.microsoft.com/support/community/?product=search).

> [!div class="nextstepaction"]
> [Arama hizmeti REST API başvurusu](https://docs.microsoft.com/rest/api/searchservice/)