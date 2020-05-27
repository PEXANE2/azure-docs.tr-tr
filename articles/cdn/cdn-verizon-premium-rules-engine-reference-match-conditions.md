---
title: Verizon Premium kural altyapısından eşleşme koşullarına Azure CDN | Microsoft Docs
description: Verizon Premium kuralları altyapısı eşleştirme koşullarından Azure Content Delivery Network için başvuru belgeleri.
services: cdn
author: asudbring
ms.service: azure-cdn
ms.topic: article
ms.date: 05/26/2020
ms.author: allensu
ms.openlocfilehash: ecec7c3c5e4cc72ee637eb91033b9f2b0f158f1a
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83872548"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-match-conditions"></a>Verizon Premium kural altyapısından eşleşme koşullarına Azure CDN

Bu makalede, Verizon Premium [kural altyapısından](cdn-verizon-premium-rules-engine.md)Azure Content DELIVERY Network (CDN) için kullanılabilen eşleşme koşullarının ayrıntılı açıklamaları listelenmektedir.

Bir kuralın ikinci bölümü eşleşme durumudur. Bir eşleşme koşulu, bir özellik kümesinin gerçekleştirileceği belirli istek türlerini tanımlar.

Örneğin, bir eşleşme koşulunu şu şekilde kullanabilirsiniz:

- Belirli bir konumdaki içerik isteklerini filtreleyin.
- Belirli bir IP adresi veya ülke/bölge tarafından oluşturulan filtre istekleri.
- İstekleri üst bilgi bilgisine göre filtreleyin.

En son eşleşme koşulları için [Verizon Rules Engine belgelerine](https://docs.vdms.com/cdn/index.html#Quick_References/HRE_QR.htm#Conditio)bakın.


## <a name="next-steps"></a>Sonraki adımlar

- [Azure Content Delivery Network genel bakış](cdn-overview.md)
- [Kural altyapısı başvurusu](cdn-verizon-premium-rules-engine-reference.md)
- [Kural altyapısı koşullu ifadeleri](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Kural altyapısı özellikleri](cdn-verizon-premium-rules-engine-reference-features.md)
- [Kurallar altyapısını kullanarak varsayılan HTTP davranışını geçersiz kılma](cdn-verizon-premium-rules-engine.md)
