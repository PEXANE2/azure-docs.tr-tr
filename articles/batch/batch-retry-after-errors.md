---
title: Bir hatadan sonra görevleri yeniden deneme
description: Hataları denetleyip yeniden deneyin.
ms.topic: article
ms.date: 02/15/2020
ms.custom: seodec18
ms.openlocfilehash: 8addc4418f268a2c27b730543bdb309ef45fd5f4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82116545"
---
# <a name="detecting-and-handling-batch-service-errors"></a>Batch hizmeti hatalarını algılama ve işleme

REST hizmeti API 'SI ile çalışırken hataları denetlemeyi unutmamak önemlidir. Toplu işler çalıştırılırken hata oluşması yaygın değildir.

## <a name="common-errors"></a>Sık karşılaşılan hatalar 

- Ağ sorunları-toplu Işe erişilemeyen isteklerdir veya Batch yanıtı istemciye zamanında ulaşamamıştır.
- İç sunucu hataları-Bunlar standart 5xx durum kodu HTTP yanıtdır.
- Daraltma, yeniden deneme üst bilgisi ile 429 veya 503 durum kodu HTTP yanıtları gibi hatalara neden olabilir.
- 4xx hatası, AlreadyExists ve InvalidOperation gibi hataları içerir. Bu, kaynağın durum geçişi için doğru durumda olmadığı anlamına gelir.

Çeşitli hata kodu türleri ve belirli hata kodları hakkında ayrıntılı bilgi için bkz. [Batch durumu ve hata kodları](https://docs.microsoft.com/rest/api/batchservice/batch-status-and-error-codes).

## <a name="when-to-retry"></a>Ne zaman yeniden deneneceğini

Bir hata oluşursa Batch API 'Leri size bildirir. Hepsi yeniden denenebilirler ve hepsi bu amaçla küresel bir yeniden deneme işleyicisi içerir. Bu yerleşik mekanizmayı kullanmak en iyisidir.

Bir hatadan sonra yeniden denemeden önce bir bit (denemeler arasında birkaç saniye) beklemeniz gerekir. Çok sık veya çok hızlı yeniden deneme yaparsanız, yeniden deneme işleyicisi azalacaktır.

### <a name="for-more-information"></a>Daha fazla bilgi edinmek için  

API başvuru bilgilerine yönelik [Batch API 'leri ve araçlar](batch-apis-tools.md) bağlantıları. .NET API, örneğin, gerekli yeniden deneme ilkesinin belirtilmesi gereken bir [Retrypolicyprovider sınıfına]( https://docs.microsoft.com/dotnet/api/microsoft.azure.batch.retrypolicyprovider?view=azure-dotnet) sahiptir. 

Her API ve varsayılan yeniden deneme ilkeleri hakkında ayrıntılı bilgi için [Batch durumu ve hata kodlarını](https://docs.microsoft.com/rest/api/batchservice/batch-status-and-error-codes)okuyun.
