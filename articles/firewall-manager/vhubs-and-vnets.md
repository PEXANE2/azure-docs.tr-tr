---
title: Azure Güvenlik Duvarı Yöneticisi mimari seçenekleri nelerdir?
description: Azure Güvenlik Duvarı Yöneticisi ile hub sanal ağını veya güvenli sanal hub mimarilerini kullanarak karşılaştırın ve karşıtlığı yapın.
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: article
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: 7a06111fbe38f167ddf3512fdb312d7de754a738
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85563581"
---
# <a name="what-are-the-azure-firewall-manager-architecture-options"></a>Azure Güvenlik Duvarı Yöneticisi mimari seçenekleri nelerdir?

Azure Güvenlik Duvarı Yöneticisi, iki ağ mimarisi türü için güvenlik yönetimi sağlayabilir:

- **güvenli sanal hub**

   [Azure sanal WAN hub](../virtual-wan/virtual-wan-about.md#resources) 'ı, hub ve bağlı bileşen mimarilerini kolayca oluşturmanıza olanak tanıyan, Microsoft tarafından yönetilen bir kaynaktır. Güvenlik ve yönlendirme ilkeleri böyle bir hub ile ilişkilendirildiğinde, bu, *[güvenli bir sanal hub](secured-virtual-hub.md)* olarak adlandırılır. 
- **Hub sanal ağı**

   Bu, kendi oluşturduğunuz ve yönettiğiniz standart bir Azure sanal ağı. Güvenlik ilkeleri böyle bir hub ile ilişkilendirildiğinde, *hub sanal ağı*olarak adlandırılır. Şu anda yalnızca Azure Güvenlik Duvarı Ilkesi desteklenir. İş yükü sunucularınızı ve hizmetlerinizi içeren sanal ağları eşleyebilir. Ayrıca, herhangi bir bağlı ağa eşlenmiş olmayan tek başına sanal ağlarda güvenlik duvarlarını yönetebilirsiniz.

## <a name="comparison"></a>Karşılaştırma

Aşağıdaki tabloda bu iki mimari seçeneği karşılaştırılmaktadır ve kuruluşunuzun güvenlik gereksinimlerine göre hangisinin doğru olduğuna karar vermenize yardımcı olabilir:


|  |**Hub sanal ağı**|**Güvenli sanal hub**  |
|---------|---------|---------|
|**Temel alınan kaynak**     |Sanal ağ|Sanal WAN hub 'ı|
|**Hub & bağlı bileşen**     |Sanal Ağ eşlemesini kullanır|Hub sanal ağ bağlantısı kullanılarak otomatik|
|**Şirket içi bağlantı**     |10 Gbps ve 30 S2S bağlantısı VPN Gateway; ExpressRoute|Daha ölçeklenebilir VPN Gateway 20 Gbps ve 1000 S2S bağlantısı; Express Route|
|**SDWAN kullanarak otomatik dal bağlantısı**      |Desteklenmiyor|Destekleniyor|
|**Bölge başına hub 'lar**     |Bölge başına birden çok sanal ağ|Her bölge için tek bir sanal hub. Birden çok sanal WAN ile birden çok hub olabilir|
|**Azure Güvenlik Duvarı – birden çok genel IP adresi**      |Müşteri tarafından sağlanmış|Otomatik olarak oluşturulan|
|**Azure Güvenlik Duvarı Kullanılabilirlik Alanları**     |Destekleniyor|Henüz kullanılamıyor|
|**Hizmet iş ortakları olarak üçüncü taraf güvenliği ile gelişmiş Internet güvenliği**     |Tercih edilen iş ortağı hizmetine müşteri tarafından sağlanan ve yönetilen VPN bağlantısı|Güvenlik iş ortağı sağlayıcısı Flow ve iş ortağı yönetim deneyimi aracılığıyla otomatikleştirilmiş|
|**Trafiği hub 'a yönlendirmek için merkezi yol yönetimi**     |Müşteri tarafından yönetilen Kullanıcı tanımlı rota|BGP kullanılarak desteklenir|
|**Birden çok güvenlik sağlayıcısı desteği**|Üçüncü taraf güvenlik duvarlarına Zorlamalı tünel elle yapılandırılmış şekilde desteklenir|İki güvenlik sağlayıcısı için otomatik destek: özel trafik filtrelemesi için Azure Güvenlik Duvarı ve Internet filtreleme için üçüncü taraf|
|**Application Gateway üzerinde Web Uygulaması Güvenlik Duvarı** |Sanal ağda destekleniyor|Şu anda bağlı olan ağda destekleniyor|
|**Ağ sanal gereci**|Sanal ağda destekleniyor|Şu anda bağlı olan ağda destekleniyor|

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Duvarı Yöneticisi dağıtımına genel bakış](deployment-overview.md) 'ı inceleyin
- [Güvenli sanal hub 'lar](secured-virtual-hub.md)hakkında bilgi edinin.