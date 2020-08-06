---
author: baanders
description: Azure dijital TWINS sınırları için dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 6/9/2020
ms.author: baanders
ms.openlocfilehash: 22b00b41c7fce0af57fd9f92b0f42bbd9412afda
ms.sourcegitcommit: 5a37753456bc2e152c3cb765b90dc7815c27a0a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87771095"
---
### <a name="functional-limits"></a>İşlevsel sınırlar

Aşağıdaki tabloda, geçerli önizlemede Azure dijital TWINS 'in işlevsel sınırları listelenmiştir.

| Alan | Özellik | Varsayılan limit | Ayarlanabilir? |
| --- | --- | --- | --- |
| Azure kaynağı | Bir bölgedeki Azure dijital TWINS örneği sayısı, abonelik başına | 10 | Evet |
| Digital Twins | Bir Azure dijital TWINS örneğindeki TWINS sayısı | 200,000 | Evet |
| Digital Twins | Tek bir ikizi gelen ilişki sayısı | 5.000 | Hayır |
| Digital Twins | Tek bir ikizi giden ilişki sayısı | 5.000 | Hayır |
| Yönlendirme | Tek bir Azure dijital TWINS örneği için uç nokta sayısı | 6 | Hayır |
| Yönlendirme | Tek bir Azure dijital TWINS örneği için yol sayısı | 6 | Evet |
| Modeller | Tek bir Azure dijital TWINS örneği içindeki model sayısı | 10,000 | Evet |
| Modeller | Tek bir API çağrısında karşıya yüklenebilen model sayısı | 250 | Hayır |
| Modeller | Tek bir sayfada döndürülen öğelerin sayısı | 100 | Hayır |
| Sorgu | Tek bir sayfada döndürülen öğelerin sayısı | 100 | Hayır |
| Sorgu | `AND`  /  `OR` Sorgudaki ifade sayısı | 50 | Yes |
| Sorgu | Bir `IN`  /  `NOT IN` yan tümcedeki dizi öğelerinin sayısı | 50 | Yes |
| Sorgu | Sorgudaki karakter sayısı | 8,000 | Yes |
| Sorgu | Sorgudaki sayı `JOINS` | 5 | Evet |

### <a name="rate-limits"></a>Hız sınırları

Bu tablo, farklı API 'lerin hız sınırlarını yansıtır.

| API | Özellik | Varsayılan limit | Ayarlanabilir? |
| --- | --- | --- | --- |
| Modeller API 'SI | Saniye başına istek sayısı | 100 | Evet |
| Dijital TWINS API 'SI | Saniye başına istek sayısı | 1.000 | Evet |
| Sorgu API'si | Saniye başına istek sayısı | 500 | Evet |
| Sorgu API'si | Saniye başına sorgu birimi | 4.000 | Evet |
| Olay rotaları API 'SI | Saniye başına istek sayısı | 100 | Evet |

### <a name="other-limits"></a>Diğer sınırlar

Azure Digital TWINS modellerine yönelik DTDL belgelerindeki veri türleri ve alanları için sınırlamalar, GitHub: [*dijital TWINS tanım dili (DTDL)-sürüm 2*](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md)' deki kendi Özellikler belgelerinde bulunabilir.
 
Sorgu gecikme ayrıntıları ve Önizleme sırasında sorgu yazmaya yönelik diğer yönergeler [*, nasıl yapılır: ikizi grafiğini sorgulama*](../articles/digital-twins/how-to-query-graph.md)bölümünde bulunabilir.