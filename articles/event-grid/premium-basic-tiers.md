---
title: Azure Olay Grid Premium ve Temel katmanlar
description: Bu makalede, Azure Olay Grid Premium ve Temel katmanları arasındaki fark ve her birinin ne zaman kullanılacağı açıklanmaktadır
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: overview
ms.date: 03/11/2020
ms.author: babanisa
ms.openlocfilehash: 7ae73337ccccc2b7d2402cfa2a54ae93552d2465
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79300721"
---
# <a name="azure-event-grid-premium-and-basic-tiers"></a>Azure Olay Izgara premium ve temel katmanlar
Azure Etkinlik Izgarası iki katmana sahiptir: **Premium** ve **Temel.** Temel katman tüketimi veya istediğiniz kadar öde fiyatlandırmasını kullanır. Olay odaklı programlama modelleri için Event Grid'i kullanmanız gereken tüm temel pub/alt araçları sağlar. Premium katman, işletmeyi hedefleyen güvenlik özellikleriyle bunu bir adım daha ileri ye götürür. Premium katman, özelliklerinin çoğu hala geliştirilmekte olan erken **önizlemededir.**

## <a name="overview"></a>Genel Bakış
Olay Izgara'daki tüm özel konular ve etki alanları temel katmana veya premium katmana aittir. API `2020-04-01-preview` sürümünden başlayarak, bir konu veya etki alanı oluşturmanın bir parçası olarak istediğiniz katmanı seçebilirsiniz. Varsayılan değer temel katmandır. Eski API sürümleri kullanılarak oluşturulan konular ve etki alanları temel katmanda varsayılan dır. Konular ve etki alanlarınız için fiyatlandırma katmanını değiştirmek [için, konular ve etki alanları için katmanı nasıl güncelleştireceğinize](update-tier.md)bakın.

## <a name="capabilities-and-features"></a>Özellikler ve özellikler

Aşağıdaki tablokatmanlar arasındaki farklılıkları açıklar:

|       &nbsp;                                           | Temel           | Premium        |
| ------------------------------------------------------ | --------------- | -------------- |
| Giriş için IP güvenlik duvarı kuralları                          | Önizleme  | Önizleme |
| Çıkış için servis etiketleri                                | Önizleme  | Önizleme |
| Girişte özel uç nokta VNet tümleştirmesi          | Kullanılamaz   | Önizleme |

## <a name="availability"></a>Kullanılabilirlik
İlk önizleme sırasında, özel bitiş noktası tümleştirmesi olan premium katman konuları ve etki alanları aşağıdaki bölgelerde kullanılabilir:

- Doğu ABD
- Batı ABD 2
- Orta Güney ABD

## <a name="pricing-and-quotas"></a>Fiyatlandırma ve kotalar
Temel katmanı kullanmanın fiyatlandırma ayrıntıları için [Olay Ağıfiyatlandırması](https://azure.microsoft.com/pricing/details/event-grid/) fiyatlandırması bölümüne bakın. Premium katman fiyatlandırması henüz duyurulmadı ve fiyatlandırma kullanılabilir olana kadar ücretsizdir.

Konu ve etki alanı sayımı ve iş miktarı yla ilgili mevcut kotalar, premium katman fiyatlandırması duyurulana kadar hem premium hem de temel katman kaynakları için geçerlidir.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- Temel katmandan premium katmana yükseltmek için [fiyatlandırma katmanını güncelleştir](update-tier.md) makalesine bakın. 
- Olay Izgara kaynağınız için IP güvenlik duvarını, yalnızca belirli bir IP Adresi kümesinden veya IP Adresi aralıklarından genel internet üzerinden erişimi kısıtlamak için yapılandırabilirsiniz. Adım adım yönergeler için [güvenlik duvarLarını yapılandır'a](configure-firewall.md)bakın.
- Yalnızca seçili sanal ağlardan erişimi kısıtlamak için özel uç noktaları yapılandırabilirsiniz. Adım adım yönergeler için [bkz.](configure-private-endpoints.md)