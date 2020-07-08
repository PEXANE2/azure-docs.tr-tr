---
author: baanders
description: Azure dijital TWINS sınırları için dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 6/9/2020
ms.author: baanders
ms.openlocfilehash: 76ccd6f42f2cfd83512a2e817e8c2ac9aa61a28b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85391510"
---
### <a name="functional-limits"></a>İşlevsel sınırlar

Aşağıdaki tabloda, geçerli önizlemede Azure dijital TWINS 'in işlevsel sınırları listelenmiştir.

| Alan | Özellik | Varsayılan limit | Ayarlanabilir? |
| --- | --- | --- | --- |
| Azure kaynağı | Bir bölgedeki Azure dijital TWINS örneği sayısı, abonelik başına | 10 | Yes |
| Dijital TWINS | Bir Azure dijital TWINS örneğindeki TWINS sayısı | 200,000 | Yes |
| Yönlendirme | Tek bir Azure dijital TWINS örneği için uç nokta sayısı | 6 | No |
| Yönlendirme | Tek bir Azure dijital TWINS örneği için yol sayısı | 6 | Yes |
| Modeller | Tek bir Azure dijital TWINS örneği içindeki model sayısı | 10,000 | Yes |
| Modeller | Tek bir API çağrısında karşıya yüklenebilen model sayısı | 250 | No |
| Modeller | Tek bir sayfada döndürülen öğelerin sayısı | 100 | No |
| Sorgu | Tek bir sayfada döndürülen öğelerin sayısı | 100 | No |
| Sorgu | `AND`  /  `OR` Sorgudaki ifade sayısı | 50 | Yes |
| Sorgu | Bir `IN`  /  `NOT IN` yan tümcedeki dizi öğelerinin sayısı | 50 | Yes |
| Sorgu | Sorgudaki karakter sayısı | 8,000 | Yes |
| Sorgu | Sorgudaki sayı `JOINS` | 1 | Yes |

### <a name="rate-limits"></a>Hız sınırları

Bu tablo, farklı API 'lerin hız sınırlarını yansıtır.

| API | Özellik | Varsayılan limit | Ayarlanabilir? |
| --- | --- | --- | --- |
| Modeller API 'SI | Saniye başına istek sayısı | 100 | Yes |
| Dijital TWINS API 'SI | Saniye başına istek sayısı | 1000 | Yes |
| Sorgu API'si | Saniye başına istek sayısı | 500 | Yes |
| Sorgu API'si | Saniye başına sorgu birimi | 4.000 | Yes |
| Olay rotaları API 'SI | Saniye başına istek sayısı | 100 | Yes |

### <a name="other-limits"></a>Diğer sınırlar

Azure Digital TWINS modellerine yönelik DTDL belgelerindeki veri türleri ve alanları için sınırlamalar, GitHub: [dijital TWINS tanım dili (DTDL)-sürüm 2](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md)' deki kendi Özellikler belgelerinde bulunabilir.
 
Sorgu gecikme ayrıntıları ve Önizleme sırasında sorgu yazmaya yönelik diğer yönergeler [, nasıl yapılır: ikizi grafiğini sorgulama](../articles/digital-twins/how-to-query-graph.md)bölümünde bulunabilir.