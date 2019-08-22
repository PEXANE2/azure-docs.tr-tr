---
title: Önizleme REST API Azure Search 2019-05-06-önizleme-Azure Search
description: Azure Search Service REST API sürüm 2019-05-06-önizleme bilgi deposu ve müşteri tarafından yönetilen şifreleme anahtarları gibi deneysel özellikleri içerir.
services: search
author: HeidiSteen
manager: nitinme
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 05/02/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 7fa280742556b7bc42d2c7fb30c880f836eef62b
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69649977"
---
# <a name="azure-search-service-rest-api-version-2019-05-06-preview"></a>Azure Search Service REST API-sürüm 2019-05-06-Önizleme
Bu makalede, henüz `api-version=2019-05-06-Preview` genel kullanıma sunulmayan Deneysel özellikler sunan Azure Search hizmeti REST API sürümü açıklanmaktadır.

> [!NOTE]
> Önizleme özellikleri, test ve deneme ile geri bildirim toplama amacını ve değişikliğe tabi bir şekilde kullanılabilir. Üretim uygulamalarında önizleme API 'Lerini kullanmayı önemle tavsiye ederiz.


## <a name="new-in-2019-05-06-preview"></a>2019-05-06 içinde yeni-önizleme

[**Bilgi deposu**](knowledge-store-concept-intro.md) , bir AI tabanlı zenginleştirme işlem hattının yeni bir hedefi. Bir dizine ek olarak, Azure depolama 'da dizin oluşturma sırasında oluşturulan doldurulmuş veri yapılarını artık kalıcı hale getirebilirsiniz. Verilerin nasıl şekillendirilmiş olduğunu, verilerin tablo depolama veya blob depolama alanında mi depolandığını, yoksa birden çok görünüm mi olduğunu da kapsayan, verilerinizin fiziksel yapılarını bir beceri öğeleri aracılığıyla kontrol edersiniz.

Hizmet tarafı şifreleme için [**müşteri tarafından yönetilen şifreleme anahtarları**](search-security-manage-encryption-keys.md) Ayrıca yeni bir önizleme özelliğidir. Microsoft tarafından yönetilen yerleşik şifrelemeye ek olarak, anahtarların tek sahibi olduğunuz sırada ek bir şifreleme katmanı da uygulayabilirsiniz.

## <a name="other-preview-features"></a>Diğer Önizleme özellikleri

Önceki önizlemelerde duyurulan Özellikler hala genel önizlemede. Daha önceki bir Preview API sürümü olan bir API 'yi arıyorsanız, bu sürümü kullanmaya devam edebilir veya beklenen davranışa hiçbir değişiklik yapmadan öğesine `2019-05-06-Preview` geçebilirsiniz.

+ [Morelikethis sorgu parametresi](search-more-like-this.md) , belirli bir belgeyle ilgili belgeleri bulur. Bu özellik daha önceki önizlemelerde yer aldı. 
* [CSV blobu dizin oluşturma](search-howto-index-csv-blobs.md) , her metin blobu için bir belge yerine her satır için bir belge oluşturur.
* [Cosmos DB Dizin oluşturucular Için MongoDB API desteği](search-howto-index-cosmosdb.md) önizlemededir.


## <a name="how-to-call-a-preview-api"></a>Önizleme API 'sini çağırma

Daha eski önizlemeler hala çalışır, ancak zaman içinde eski hale gelir. Kodunuz veya `api-version=2016-09-01-Preview` `api-version=2017-11-11-Preview`çağırıyorsa, bu çağrılar hala geçerlidir. Ancak, geliştirmelerle yalnızca en yeni önizleme sürümü yenilenir. 

Aşağıdaki örnek sözdizimi, önizleme API sürümüne yapılan çağrıyı gösterir.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?search=*&api-version=2019-05-06-Preview

Azure Search hizmeti birden çok sürümde kullanılabilir. Daha fazla bilgi için bkz. [API sürümleri](search-api-versions.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure Search hizmeti REST API başvuru belgelerini inceleyin. Sorunlarla karşılaşırsanız [StackOverflow](https://stackoverflow.com/) hakkında yardım isteyin veya [desteğe başvurun](https://azure.microsoft.com/support/community/?product=search).

> [!div class="nextstepaction"]
> [Arama hizmeti REST API başvurusu](https://docs.microsoft.com/rest/api/searchservice/)