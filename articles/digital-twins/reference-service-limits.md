---
title: Genel önizleme hizmet sınırları
titleSuffix: Azure Digital Twins
description: Genel Önizleme sırasında Azure Digital TWINS hizmetinin sınırlarını gösteren grafik.
author: baanders
ms.author: baanders
ms.date: 05/05/2020
ms.topic: article
ms.service: digital-twins
ms.openlocfilehash: 4497e1222904b1dad5fbeccd942854443e756ed5
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84612848"
---
# <a name="service-limits-in-public-preview"></a>Hizmet sınırları genel önizlemede

Bunlar, genel önizleme sırasında Azure dijital TWINS 'in hizmet sınırlamalarıdır.

> [!NOTE]
> Bazı hizmetler, *ayarlanabilir?* sütunuyla aşağıdaki tablolarda temsil edilen ayarlanabilir sınırlara sahiptir. Sınır ayarlanabilirken, *ayarlanabilir?* değeri *Evet*' tir.
>
> İşiniz varsayılan sınırın üzerinde ayarlanabilir bir sınırı veya kotayı yükseltmeyi gerektiriyorsa, [bir destek bileti açarak](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest)ek kaynaklar isteyebilirsiniz.

## <a name="functional-limits"></a>İşlevsel sınırlar

Aşağıdaki tabloda, geçerli önizlemede Azure dijital TWINS 'in işlevsel sınırları listelenmiştir.

| Alan | Özellik | Sınır | Ayarlanabilir? |
| --- | --- | --- | --- |
| Azure kaynağı | Bir bölgedeki Azure dijital TWINS örneği sayısı, abonelik başına | 10 | Yes |
| Dijital TWINS | Bir Azure dijital TWINS örneğindeki TWINS sayısı | 200,000 | Yes |
| Yönlendirme | Tek bir Azure dijital TWINS örneği için uç nokta sayısı | 6 | Hayır |
| Yönlendirme | Tek bir Azure dijital TWINS örneği için yol sayısı | 6 | Yes |
| Modeller | Tek bir Azure dijital TWINS örneği içindeki model sayısı | 10,000 | Yes |
| Modeller | Tek bir API çağrısında karşıya yüklenebilen model sayısı | 250 | Hayır |
| Modeller | Tek bir sayfada döndürülen öğelerin sayısı | 100 | Hayır |
| Sorgu | Tek bir sayfada döndürülen öğelerin sayısı | 100 | Hayır |
| Sorgu | `AND`  /  `OR` Sorgudaki ifade sayısı | 50 | Yes |
| Sorgu | Bir `IN`  /  `NOT IN` yan tümcedeki dizi öğelerinin sayısı | 50 | Yes |
| Sorgu | Sorgudaki karakter sayısı | 8,000 | Yes |
| Sorgu | Sorgudaki sayı `JOINS` | 1 | Yes |

## <a name="rate-limits"></a>Hız sınırları

Bu tablo, farklı API 'lerin hız sınırlarını yansıtır.

| API | Özellik | Sınır | Ayarlanabilir? |
| --- | --- | --- | --- |
| Modeller API 'SI | Saniye başına istek sayısı | 100 | Yes |
| Dijital TWINS API 'SI | Saniye başına istek sayısı | 1000 | Yes |
| Sorgu API'si | Saniye başına istek sayısı | 500 | Yes |
| Sorgu API'si | Saniye başına sorgu birimi | 4.000 | Yes |
| Olay rotaları API 'SI | Saniye başına istek sayısı | 100 | Yes |

## <a name="other-limits"></a>Diğer sınırlar

Azure Digital TWINS modellerine yönelik DTDL belgelerindeki veri türleri ve alanları için sınırlamalar, GitHub: [dijital TWINS tanım dili (DTDL)-sürüm 2](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/dtdlv2.md)' deki kendi Özellikler belgelerinde bulunabilir.
 
Sorgu gecikme ayrıntıları ve Önizleme sırasında sorgu yazmaya yönelik diğer yönergeler [, nasıl yapılır: ikizi grafiğini sorgulama](how-to-query-graph.md)bölümünde bulunabilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital TWINS 'nin geçerli önizleme sürümü hakkında daha fazla bilgi için bkz. hizmet genel bakış:
* [Genel Bakış: Azure dijital TWINS nedir?](overview.md)
