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
ms.date: 11/19/2020
ms.author: aahi
ms.reviewer: chtufts
ms.openlocfilehash: c60adb09da05ba945bcf6ccb55e71c395f064211
ms.sourcegitcommit: cd9754373576d6767c06baccfd500ae88ea733e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2020
ms.locfileid: "94965111"
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
| Tek belge için maksimum boyut | [StringInfo. LengthInTextElements](/dotnet/api/system.globalization.stringinfo.lengthintextelements)ile ölçülen 5.120 karakter. Ayrıca sistem durumu için Metin Analizi için de geçerlidir. |
| Tek bir belgenin en büyük boyutu ( `/analyze` uç nokta)  | [StringInfo. LengthInTextElements](/dotnet/api/system.globalization.stringinfo.lengthintextelements)Ile ölçülen 125K karakter. Sistem durumu için Metin Analizi uygulanmaz. |
| İsteğin tamamının maksimum boyutu | 1 MB. Ayrıca sistem durumu için Metin Analizi için de geçerlidir. |

Tek bir istekte gönderebilmeniz için en fazla belge sayısı, kullanmakta olduğunuz API sürümüne ve özelliğine bağlı olacaktır. `/analyze`Herhangi bir belge en büyük boyutu aşarsa, uç nokta tüm isteği reddeder (125K karakter)

#### <a name="version-3"></a>[Sürüm 3](#tab/version-3)

Aşağıdaki sınırlar geçerli v3 API 'sidir. Aşağıdaki sınırları aşmamak, bir HTTP 400 hata kodu üretir.


| Öne çıkan özelliği | Istek başına en fazla belge | 
|----------|-----------|
| Dil Algılama | 1000 |
| Yaklaşım Analizi | 10 |
| Görüşün madenciliği | 10 |
| Anahtar İfade Ayıklama | 10 |
| Adlandırılmış Varlık Tanıma | 5 |
| Varlık Bağlama | 5 |
| Sistem durumu için Metin Analizi  | Web tabanlı API için 10, kapsayıcı için 1000. |
| Uç noktayı çözümle | Tüm işlemler için 25. |

#### <a name="version-2"></a>[Sürüm 2](#tab/version-2)

| Öne çıkan özelliği | Istek başına en fazla belge | 
|----------|-----------|
| Dil Algılama | 1000 |
| Yaklaşım Analizi | 1000 |
| Anahtar İfade Ayıklama | 1000 |
| Adlandırılmış Varlık Tanıma | 1000 |
| Varlık Bağlama | 1000 |

---

## <a name="rate-limits"></a>Hız sınırları

Oran sınırınız [fiyatlandırma katmanınızda](https://azure.microsoft.com/pricing/details/cognitive-services/text-analytics/)farklılık gösterecektir. Bu sınırlar, her iki API sürümü için de aynıdır. Bu hız limitleri, ayarlanan hız sınırına sahip olmayan sistem durumu kapsayıcısı için Metin Analizi uygulanmaz.

| Katman          | Saniyedeki istek | Dakika başına istek |
|---------------|---------------------|---------------------|
| S/Multi-Service | 1000                | 1000                |
| S0/F0         | 100                 | 300                 |
| S1            | 200                 | 300                 |
| S2            | 300                 | 300                 |
| S3            | 500                 | 500                 |
| S4            | 1000                | 1000                |

İstek ücretleri her bir Metin Analizi özelliği için ayrı olarak ölçülür. Fiyatlandırma katmanınız için en fazla istek sayısını aynı anda her bir özelliğe gönderebilirsiniz. Örneğin, `S` katmanda çalışıyorsanız ve 1000 istek bir kez gönderiyorsanız, 59 saniyelik başka bir istek gönderemeyebilirsiniz.


## <a name="see-also"></a>Ayrıca bkz.

* [Metin Analizi API'si nedir?](../overview.md)
* [Fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/cognitive-services/text-analytics/)
