---
title: Metin Analizi API'si için veri sınırları
titleSuffix: Azure Cognitive Services
description: Azure bilişsel hizmetler 'deki Metin Analizi API'si yönelik veri sınırlamaları.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: overview
ms.date: 04/27/2020
ms.author: aahi
ms.reviewer: chtufts
ms.openlocfilehash: 0dc79132ae75be511b53bf272e578ff0ce7a7775
ms.sourcegitcommit: 25bb515efe62bfb8a8377293b56c3163f46122bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87986823"
---
# <a name="data-and-rate-limits-for-the-text-analytics-api"></a>Metin Analizi API'si için veri ve hız sınırları
<a name="data-limits"></a>

Boyut sınırlarını bulmak için bu makaleyi kullanın ve Metin Analizi API'si veri gönderebileceğinizi ücretleri. 

## <a name="data-limits"></a>Veri sınırları

> [!NOTE]
> * Sınırın izin verdiğinden daha büyük belgeler çözümlemeniz gerekiyorsa, API 'ye göndermeden önce metni daha küçük parçalara ayırın. 
> * Belge, tek bir metin karakteri dizesidir.  

| Sınır | Değer |
|------------------------|---------------|
| Tek belge için maksimum boyut | [StringInfo. LengthInTextElements](https://docs.microsoft.com/dotnet/api/system.globalization.stringinfo.lengthintextelements)ile ölçülen 5.120 karakter. |
| İsteğin tamamının maksimum boyutu | 1 MB |

Tek bir istekte gönderebilmeniz için en fazla belge sayısı, kullanmakta olduğunuz API sürümüne ve özelliğine bağlı olacaktır.

#### <a name="version-3"></a>[Sürüm 3](#tab/version-3)

Aşağıdaki sınırlar API 'nin v3 sürümünde değiştirilmiştir. Aşağıdaki sınırları aşmamak, bir HTTP 400 hata kodu üretir.


| Özellik | Istek başına en fazla belge | 
|----------|-----------|
| Dil Algılama | 1000 |
| Yaklaşım Analizi | 10 |
| Anahtar İfade Ayıklama | 10 |
| Adlandırılmış Varlık Tanıma | 5 |
| Varlık Bağlama | 5 |

#### <a name="version-2"></a>[Sürüm 2](#tab/version-2)

| Özellik | Istek başına en fazla belge | 
|----------|-----------|
| Dil Algılama | 1000 |
| Yaklaşım Analizi | 1000 |
| Anahtar İfade Ayıklama | 1000 |
| Adlandırılmış Varlık Tanıma | 1000 |
| Varlık Bağlama | 1000 |

---

## <a name="rate-limits"></a>Hız sınırları

Oran sınırınız [fiyatlandırma katmanınızda](https://azure.microsoft.com/pricing/details/cognitive-services/text-analytics/)farklılık gösterecektir. Bu sınırlar, her iki API sürümü için de aynıdır.

| Katman          | Saniyedeki istek | Dakika başına istek |
|---------------|---------------------|---------------------|
| S/Multi-Service | 1000                | 1000                |
| S0/F0         | 100                 | 300                 |
| S1            | 200                 | 300                 |
| S2            | 300                 | 300                 |
| S3            | 500                 | 500                 |
| S4            | 1000                | 1000                |

İstekler her bir Metin Analizi özelliği için ayrı olarak ölçülür. Örneğin, fiyatlandırma katmanınız için en fazla istek sayısını aynı anda her bir özelliğe gönderebilirsiniz.  


## <a name="see-also"></a>Ayrıca bkz.

* [Metin Analizi API'si nedir?](../overview.md)
* [Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/text-analytics/)
