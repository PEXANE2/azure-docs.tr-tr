---
title: Azure Güvenlik Duvarı Yöneticisi Önizlemesi nedir?
description: Azure Güvenlik Duvarı Yöneticisi özellikleri hakkında bilgi edinin
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: overview
ms.date: 03/13/2020
ms.author: victorh
ms.openlocfilehash: 149782f627d586e927c828506a7d4f1b5437b987
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79366283"
---
# <a name="what-is-azure-firewall-manager-preview"></a>Azure Güvenlik Duvarı Yöneticisi Önizlemesi nedir?

[!INCLUDE [Preview](../../includes/firewall-manager-preview-notice.md)]

Azure Güvenlik Duvarı Yöneticisi Önizleme, bulut tabanlı güvenlik çevreleri için merkezi güvenlik ilkesi ve rota yönetimi sağlayan bir güvenlik yönetimi hizmetidir. 

Güvenlik Duvarı Yöneticisi iki ağ mimarisi türü için güvenlik yönetimi sağlayabilir:

- **güvenli sanal hub**

   [Azure Sanal WAN Hub'ı,](../virtual-wan/virtual-wan-about.md#resources) hub ve kollu mimarileri kolayca oluşturmanıza olanak tanıyan Microsoft tarafından yönetilen bir kaynaktır. Güvenlik ve yönlendirme ilkeleri böyle bir hub ile ilişkilendirildiğinde, güvenli sanal *[hub](secured-virtual-hub.md)* olarak adlandırılır. 
- **hub sanal ağ**

   Bu, oluşturduğunuz ve yönettiğiniz standart bir Azure sanal ağıdır. Güvenlik ilkeleri böyle bir hub ile ilişkilendirildiğinde, hub *sanal ağ*olarak adlandırılır. Şu anda yalnızca Azure Güvenlik Duvarı İlkesi desteklenir. İş yükü sunucularınızı ve hizmetlerinizi içeren kollu sanal ağları eşleyebilirsiniz. Ayrıca, herhangi bir konuşmayla bakmayan bağımsız sanal ağlarda güvenlik duvarlarını da yönetebilirsiniz.

*Güvenli sanal hub* ve hub *sanal ağ* mimarilerinin ayrıntılı bir karşılaştırması için Azure Güvenlik Duvarı Yöneticisi mimari seçenekleri [nelerdir?](vhubs-and-vnets.md)

![güvenlik duvarı yöneticisi](media/overview/firewallmanagerv5.png)

## <a name="azure-firewall-manager-preview-features"></a>Azure Güvenlik Duvarı Yöneticisi Önizleme özellikleri

Azure Güvenlik Duvarı Yöneticisi Önizleme aşağıdaki özellikleri sunar:

### <a name="central-azure-firewall-deployment-and-configuration"></a>Merkezi Azure Güvenlik Duvarı dağıtımı ve yapılandırması

Farklı Azure bölgelerine ve aboneliklerine yayılan birden çok Azure Güvenlik Duvarı örneğini merkezi olarak dağıtabilir ve yapılandırabilirsiniz. 

### <a name="hierarchical-policies-global-and-local"></a>Hiyerarşik ilkeler (genel ve yerel)

Birden çok güvenli sanal hub'da Azure Güvenlik Duvarı ilkelerini merkezi olarak yönetmek için Azure Güvenlik Duvarı Yöneticisi Önizleme'yi kullanabilirsiniz. Merkezi BT ekipleriniz, ekipler arasında kuruluş genelinde geniş güvenlik duvarı ilkesini uygulamak için genel güvenlik duvarı ilkeleri yazabilir. Yerel olarak yazılmış güvenlik duvarı ilkeleri, Daha iyi çeviklik için DevOps self servis modeline izin verir.

### <a name="integrated-with-third-party-security-as-a-service-for-advanced-security"></a>Gelişmiş güvenlik için hizmet olarak üçüncü taraf güvenlikle entegre

Azure Güvenlik Duvarı'na ek olarak, VNet ve şube Internet bağlantılarınız için ek ağ koruması sağlamak için üçüncü taraf güvenliği hizmet (SECaaS) sağlayıcıları olarak tümleştirebilirsiniz.

Bu özellik yalnızca güvenli sanal hub dağıtımlarında kullanılabilir.

- VNet'den Internet'e (V2I) trafik filtreleme

   - Giden sanal ağ trafiğini tercih ettiğiniz üçüncü taraf güvenlik sağlayıcınıza filtreuygulayın.
   - Azure'da çalışan bulut iş yükleri için gelişmiş kullanıcı tarafından çalışan Internet korumasından yararlanın.

- Şubeden İnternete (B2I) trafik filtreleme

   Internet senaryolarına dal için üçüncü taraf filtresini kolayca eklemek için Azure bağlantınızdan ve genel dağıtımınızdan yararlanın.

Güvenilen güvenlik sağlayıcıları hakkında daha fazla bilgi için Azure [Güvenlik Duvarı Yöneticisi'nin güvendiği güvenlik ortakları (önizleme) nedir?](trusted-security-partners.md)

### <a name="centralized-route-management"></a>Merkezi rota yönetimi

Kullanıcı Tanımlı Rotalar'ı (UDR) kollu olarak ayarlamaya gerek kalmadan, trafiği filtreleme ve günlüğe kaydetme için güvenli merkezinize kolayca yönlendirin. 

Bu özellik yalnızca güvenli sanal hub dağıtımlarında kullanılabilir.

Branch to Internet (B2I) trafik filtrelemi için üçüncü taraf sağlayıcıları, Branch to VNet (B2V), VNet to VNet (V2V) ve VNet to Internet (V2I) için Azure Güvenlik Duvarı ile yan yana kullanabilirsiniz. Azure Güvenlik Duvarı B2V veya V2V için gerekli olmadığı sürece V2I trafik filtreleme için üçüncü taraf sağlayıcıları da kullanabilirsiniz. 

## <a name="region-availability"></a>Bölge kullanılabilirliği

Azure Güvenlik Duvarı İlkeleri bölgeler arasında kullanılabilir. Örneğin, Batı ABD'de bir politika oluşturabilir ve Doğu ABD'de kullanabilirsiniz. 

## <a name="known-issues"></a>Bilinen sorunlar

Azure Güvenlik Duvarı Yöneticisi Önizleme'nin bilinen sorunları şunlardır:

|Sorun  |Açıklama  |Risk azaltma  |
|---------|---------|---------|
|Üçüncü taraf filtreleme sınırlamaları.|Üçüncü taraf sağlayıcılarla V2I trafik filtreleme, Azure Güvenlik Duvarı B2V ve V2V ile desteklenmez.|Araştırılıyor|
|Trafik bölme şu anda desteklenmiyor.|Office 365 ve Azure Genel PaaS trafik bölme şu anda desteklenmez. Bu nedenle, V2I veya B2I için bir üçüncü taraf sağlayıcı seçmek, iş ortağı hizmeti aracılığıyla tüm Azure Genel PaaS ve Office 365 trafiğini de gönderir.|Merkezde trafik bölmesini araştırıyoruz.
|Bölge başına güvenli bir sanal hub.|Bölge başına birden fazla güvenli sanal hub'A sahip olamazsınız.|Bir bölgede birden çok sanal WAN oluşturun.|
|Temel ilkeler yerel politikayla aynı bölgede olmalıdır.|Tüm yerel ilkelerinizi temel ilkeyle aynı bölgede oluşturun. Bir bölgede oluşturulan bir ilkeyi başka bir bölgeden güvenli bir hub'da yine uygulayabilirsiniz.|Araştırılıyor|
|Güvenli Sanal Hub ile çalışmıyor hub arası iletişim|Güvenli Sanal Hub ile Güvenli Sanal Hub iletişimi henüz desteklenmedi.|Araştırılıyor|
|Aynı sanal WAN'ı paylaşan tüm Güvenli Sanal Hub'lar aynı kaynak grubunda olmalıdır.|Bu davranış, bugün Virtual WAN Hub'ları ile uyumludur.|Güvenli Sanal Hub'ların farklı kaynak gruplarında oluşturulmasına izin vermek için birden çok Sanal WAN oluşturun.|
|IP Grupları Güvenlik Duvarı Politikası'nda desteklenmez.|IP Grupları genel önizlemededir ve şu anda yalnızca geleneksel güvenlik duvarı kurallarıyla desteklenir.|Düzeltme devam ediyor.
|Bulut Çözüm Sağlayıcısı (CSP) abonelikleri desteklenmez.|Şu [anda, CSP abonelikleri](https://azure.microsoft.com/offers/ms-azr-0145p/) desteklenmez.|Araştırılıyor

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Duvarı Yöneticisi Önizleme dağıtımına genel bakışı](deployment-overview.md) gözden geçirin
- Güvenli [Sanal Hub'lar](secured-virtual-hub.md)hakkında bilgi edinin.