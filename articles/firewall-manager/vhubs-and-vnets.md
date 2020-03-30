---
title: Azure Güvenlik Duvarı Yöneticisi mimari seçenekleri nelerdir?
description: Azure Güvenlik Duvarı Yöneticisi ile hub sanal ağı veya güvenli sanal hub mimarilerini kullanarak karşılaştırın ve karşılaştırın.
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: article
ms.date: 02/18/2020
ms.author: victorh
ms.openlocfilehash: b946a360ced05500a4ef89cda7c623d8ae16658e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77444582"
---
# <a name="what-are-the-azure-firewall-manager-architecture-options"></a>Azure Güvenlik Duvarı Yöneticisi mimari seçenekleri nelerdir?

Azure Güvenlik Duvarı Yöneticisi iki ağ mimarisi türü için güvenlik yönetimi sağlayabilir:

- **güvenli sanal hub**

   [Azure Sanal WAN Hub'ı,](../virtual-wan/virtual-wan-about.md#resources) hub ve kollu mimarileri kolayca oluşturmanıza olanak tanıyan Microsoft tarafından yönetilen bir kaynaktır. Güvenlik ve yönlendirme ilkeleri böyle bir hub ile ilişkilendirildiğinde, güvenli sanal *[hub](secured-virtual-hub.md)* olarak adlandırılır. 
- **hub sanal ağ**

   Bu, oluşturduğunuz ve yönettiğiniz standart bir Azure sanal ağıdır. Güvenlik ilkeleri böyle bir hub ile ilişkilendirildiğinde, hub *sanal ağ*olarak adlandırılır. Şu anda yalnızca Azure Güvenlik Duvarı İlkesi desteklenir. İş yükü sunucularınızı ve hizmetlerinizi içeren kollu sanal ağları eşleyebilirsiniz. Ayrıca, herhangi bir konuşmayla bakmayan bağımsız sanal ağlarda güvenlik duvarlarını da yönetebilirsiniz.

## <a name="comparison"></a>Karşılaştırma

Aşağıdaki tablo bu iki mimari seçeneği karşılaştırır ve kuruluşunuzun güvenlik gereksinimleri için hangisinin doğru olduğuna karar vermenize yardımcı olabilir:


|  |**Hub sanal ağı**|**Güvenli sanal hub**  |
|---------|---------|---------|
|**Altta yatan kaynak**     |Sanal ağ|Sanal WAN Hub|
|**Hub & Spoke**     |Sanal ağ eşleme kullanır|Hub sanal ağ bağlantısı kullanarak otomatik|
|**Ön-prem bağlantısı**     |10 Gbps ve 30 S2S bağlantıya kadar VPN Ağ Geçidi; Ekspres Rota|20 Gbps ve 1000 S2S bağlantısına kadar daha ölçeklenebilir VPN Ağ Geçidi; Ekspres Rota|
|**SDWAN kullanarak otomatik şube bağlantısı**      |Desteklenmiyor|Destekleniyor|
|**Bölge başına hub'lar**     |Bölge başına birden fazla Sanal Ağ|Bölge başına tek sanal hub. Birden çok Sanal WAN ile mümkün olan birden çok hub|
|**Azure Güvenlik Duvarı – birden çok genel IP adresi**      |Sağlanan müşteri|Otomatik oluşturuldu. GA tarafından kullanılabilir olması.|
|**Azure Güvenlik Duvarı Kullanılabilirlik Bölgeleri**     |Destekleniyor|Önizlemede kullanılamaz. GA tarafından kullanılabilir olması|
|**Hizmet ortakları olarak üçüncü taraf Güvenlik ile gelişmiş Internet güvenliği**     |Müşteri, tercih edilen iş ortağı hizmetine VPN bağlantısı kurmuş ve yönetmiş|Güvenilir Güvenlik İş Ortağı akışı ve iş ortağı yönetimi deneyimi ile otomatikleştirilmiş|
|**Trafiği hub'a yönlendirmek için merkezi rota yönetimi**     |Müşteri tarafından yönetilen Kullanıcı Tanımlı Rota|BGP kullanılarak desteklenir|
|**Application Gateway üzerinde Web Uygulaması Güvenlik Duvarı** |Sanal Ağda Desteklendi|Şu anda spoke ağında desteklenir|
|**Ağ Sanal Cihaz**|Sanal Ağda Desteklendi|Şu anda spoke ağında desteklenir|

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Duvarı Yöneticisi Önizleme dağıtımına genel bakışı](deployment-overview.md) gözden geçirin
- Güvenli [Sanal Hub'lar](secured-virtual-hub.md)hakkında bilgi edinin.