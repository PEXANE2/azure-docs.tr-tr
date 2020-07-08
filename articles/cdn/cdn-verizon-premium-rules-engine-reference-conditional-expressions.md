---
title: Azure CDN-Verizon Premium kuralları altyapısı için Koşullu ifadeler
description: Verizon Premium kural altyapısından Azure CDN için başvuru belgeleri, koşullar ve özelliklerle eşleşir.
services: cdn
author: asudbring
ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: allensu
ms.openlocfilehash: 25b3c1a011e39adea651a6daa1d4ab9aa3a9d186
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81253518"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-conditional-expressions"></a>Verizon Premium kural altyapısı koşullu ifadelerinden Azure CDN

Bu makalede, Azure Content Delivery Network (CDN) [kuralları motorunun](cdn-verizon-premium-rules-engine.md)koşullu ifadelerin ayrıntılı açıklamaları listelenmektedir.

Kuralın ilk bölümü koşullu Ifadedir.

Koşullu Ifade | Açıklama
-----------------------|-------------
IF | Bir If ifadesi her zaman kuraldaki ilk deyimin bir parçasıdır. Diğer tüm koşullu ifadeler gibi, bu IF deyimi bir eşleşme ile ilişkilendirilmelidir. Ek Koşullu ifadeler tanımlanmazsa, bu eşleşme bir istek için bir dizi özellik uygulanmadan önce karşılanması gereken ölçütü belirler.
VE ISE | Bir ve If ifadesi yalnızca aşağıdaki koşullu ifade türlerinden sonra eklenebilir: Ise ve. İlk IF ifadesinde karşılanması gereken başka bir koşul olduğunu gösterir.
AKSI TAKDIRDE| Bir ELSE IF ifadesi, bu başka bir durum deyimi gerçekleşmişse, bu özelliğe özgü bir özellik kümesinden önce karşılanması gereken alternatif bir koşulu belirtir. Bir ELSE IF ifadesinin var olması, önceki deyimin sonunu gösterir. Bir ELSE IF deyimi daha sonra yerleştirilebilecek tek koşullu ifade başka bir IF deyimi ise. Bu, başka bir IF ifadesinin yalnızca yerine getirilmesi gereken tek bir ek koşul belirtmek için kullanılabilir olması anlamına gelir.

**Örnek**: ![ CDN eşleşme koşulu](./media/cdn-rules-engine-reference/cdn-rules-engine-conditional-expression.png)

 > [!TIP]
   > Sonraki bir kural, önceki bir kural tarafından belirtilen eylemleri geçersiz kılabilir.
   > Örnek: bir catch-all kuralı, belirteç tabanlı kimlik doğrulama aracılığıyla tüm istekleri korur. Belirli istek türleri için bir özel durum oluşturmak üzere, doğrudan altında başka bir kural oluşturulabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure CDN genel bakış](cdn-overview.md)
- [Kural altyapısı başvurusu](cdn-verizon-premium-rules-engine-reference.md)
- [Kural altyapısı eşleştirme koşulları](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [Kural altyapısı özellikleri](cdn-verizon-premium-rules-engine-reference-features.md)
- [Kurallar altyapısını kullanarak varsayılan HTTP davranışını geçersiz kılma](cdn-verizon-premium-rules-engine.md)