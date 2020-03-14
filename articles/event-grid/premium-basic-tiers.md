---
title: Azure Event Grid Premium ve temel katmanları
description: Bu makalede Azure Event Grid Premium ve temel Katmanlar arasındaki farklar ve her birinin ne zaman kullanılacağı açıklanmaktadır.
services: event-grid
author: banisadr
ms.service: event-grid
ms.topic: overview
ms.date: 03/11/2020
ms.author: babanisa
ms.openlocfilehash: 7ae73337ccccc2b7d2402cfa2a54ae93552d2465
ms.sourcegitcommit: c29b7870f1d478cec6ada67afa0233d483db1181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79300721"
---
# <a name="azure-event-grid-premium-and-basic-tiers"></a>Azure Event Grid Premium ve temel katmanları
Azure Event Grid iki katmana sahiptir: **Premium** ve **Basic**. Temel katman, tüketim veya kullandıkça öde fiyatlandırmasını kullanır. Olay odaklı programlama modelleri için Event Grid kullanmanız gereken tüm temel yayın/alt araçları sağlar. Premium katmanı, bu adımı kuruluşa yönelik güvenlik özellikleriyle birlikte gerçekleştirir. Premium katman, bazı özelliklerinin çoğunu geliştirme aşamasında olan erken **önizlemededir** .

## <a name="overview"></a>Genel Bakış
Event Grid tüm özel konular ve etki alanları temel katmana veya Premium katmana aittir. `2020-04-01-preview` API sürümünden itibaren, bir konu veya etki alanı oluşturmanın bir parçası olarak istenen katmanı seçebilirsiniz. Varsayılan değer temel katmandır. Eski API sürümleri kullanılarak oluşturulan konular ve etki alanları temel katmanda varsayılan değer. Konu ve etki alanlarınızın fiyatlandırma katmanını değiştirmek için bkz. [konular ve etki alanları için katmanı güncelleştirme](update-tier.md).

## <a name="capabilities-and-features"></a>Özellikler ve Özellikler

Aşağıdaki tabloda Katmanlar arasındaki farklılıklar açıklanmaktadır:

|       &nbsp;                                           | Temel           | Premium        |
| ------------------------------------------------------ | --------------- | -------------- |
| Giriş için IP güvenlik duvarı kuralları                          | Önizleme  | Önizleme |
| Çıkış için hizmet etiketleri                                | Önizleme  | Önizleme |
| Giriş üzerinde özel uç nokta VNet tümleştirmesi          | Kullanılamaz   | Önizleme |

## <a name="availability"></a>Kullanılabilirlik
İlk önizleme sırasında, Özel uç nokta tümleştirmesine sahip Premium katman konuları ve etki alanları aşağıdaki bölgelerde kullanılabilir:

- Doğu ABD
- Batı ABD 2
- Orta Güney ABD

## <a name="pricing-and-quotas"></a>Fiyatlandırma ve kotalar
Temel katmanı kullanmanın fiyatlandırma ayrıntıları için bkz. [Event Grid fiyatlandırması](https://azure.microsoft.com/pricing/details/event-grid/) . Premium katman fiyatlandırması henüz duyurulmaz ve fiyatlandırma kullanılabilir olana kadar ücretsizdir.

Konu ve etki alanı sayısı ile mevcut kotalar ve verimlilik, Premium katman fiyatlandırması duyuruncaya kadar hem Premium hem de temel katman kaynakları için geçerlidir.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- Temel katmandan Premium katmana yükseltmek için bkz. [fiyatlandırma katmanını güncelleştirme](update-tier.md) makalesi. 
- Genel internet üzerinden erişimi yalnızca belirli bir IP adresi veya IP adresi aralığı kümesinden kısıtlamak için, Event Grid kaynağınız için IP güvenlik duvarını yapılandırabilirsiniz. Adım adım yönergeler için bkz. [güvenlik duvarını yapılandırma](configure-firewall.md).
- Yalnızca seçili sanal ağlardan erişimi kısıtlamak için özel uç noktaları yapılandırabilirsiniz. Adım adım yönergeler için bkz. [Özel uç noktaları yapılandırma](configure-private-endpoints.md).