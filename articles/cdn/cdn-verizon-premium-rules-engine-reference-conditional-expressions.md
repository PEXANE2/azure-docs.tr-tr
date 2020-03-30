---
title: Azure CDN için koşullu ifadeler - Verizon Premium kuralları motoru
description: Verizon Premium kurallarına göre Azure CDN için başvuru belgeleri motor koşulları ve özellikleriyle eşleşir.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus
ms.openlocfilehash: d28bda28894477845c2a050666b3b4dd332b7d50
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74082964"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-conditional-expressions"></a>Verizon Premium'dan Azure CDN, motor koşullu ifadeleri yönetir

Bu makalede, Azure İçerik Teslim Ağı (CDN) [Kuralları Altyapısı](cdn-verizon-premium-rules-engine.md)için Koşullu İfadeler ayrıntılı açıklamaları listelenmiştir.

Kuralın ilk bölümü Koşullu İfade'dir.

Koşullu İfade | Açıklama
-----------------------|-------------
IF | IF ifadesi her zaman bir kuraldaki ilk ifadenin bir parçasıdır. Diğer tüm koşullu ifadeler gibi, bu IF deyimi de bir eşleşmeyle ilişkilendirilmelidir. Ek koşullu ifadeler tanımlanmamışsa, bu eşleşme, bir dizi özellik isteğe uygulanabilmesi için karşılanması gereken ölçütleri belirler.
VE EĞER | Bir VE IF ifadesi yalnızca aşağıdaki koşullu ifade türlerinden sonra eklenebilir:IF,VE IF. İlk IF deyimi için karşılanması gereken başka bir koşul olduğunu gösterir.
ELSE EĞER| ELSE IF ifadesi, bu ELSE IF deyimine özgü bir dizi özellik gerçekleşmeden önce karşılanması gereken alternatif bir koşul belirtir. ELSE IF deyiminin varlığı önceki deyimin sonunu gösterir. ELSE IF deyiminden sonra konulabilecek tek koşullu ifade başka bir ELSE IF deyimidir. Bu, ELSE IF deyiminin yalnızca karşılanması gereken tek bir ek koşul belirtmek için kullanılabildiği anlamına gelir.

**Örnek** ![: CDN eşleşme durumu](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)

 > [!TIP]
   > Sonraki bir kural önceki kural tarafından belirtilen eylemleri geçersiz kılabilir.
   > Örnek: Tümleri yakalama kuralı, Tüm istekleri Belirteç Tabanlı Kimlik Doğrulama ile güvence altına alar. Belirli istek türleri için özel bir durum yapmak için doğrudan altında başka bir kural oluşturulabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure CDN'ye genel bakış](cdn-overview.md)
- [Kural altyapısı başvurusu](cdn-verizon-premium-rules-engine-reference.md)
- [Kural altyapısı eşleştirme koşulları](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [Kural altyapısı özellikleri](cdn-verizon-premium-rules-engine-reference-features.md)
- [Kurallar altyapısını kullanarak varsayılan HTTP davranışını geçersiz kılma](cdn-verizon-premium-rules-engine.md)