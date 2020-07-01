---
title: Azure Güvenlik Duvarı Yöneticisi nedir?
description: Azure Güvenlik Duvarı Yöneticisi özellikleri hakkında bilgi edinin
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: overview
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: 8b457198655af50427545a0e93e2cfe6903131c8
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/30/2020
ms.locfileid: "85563745"
---
# <a name="what-is-azure-firewall-manager"></a>Azure Güvenlik Duvarı Yöneticisi nedir?

Azure Güvenlik Duvarı Yöneticisi, bulut tabanlı güvenlik perimeters için merkezi güvenlik ilkesi ve Yönlendirme yönetimi sağlayan bir güvenlik yönetim hizmetidir. 

Güvenlik Duvarı Yöneticisi, iki ağ mimarisi türü için güvenlik yönetimi sağlayabilir:

- **Güvenli sanal hub**

   [Azure sanal WAN hub](../virtual-wan/virtual-wan-about.md#resources) 'ı, hub ve bağlı bileşen mimarilerini kolayca oluşturmanıza olanak tanıyan, Microsoft tarafından yönetilen bir kaynaktır. Güvenlik ve yönlendirme ilkeleri böyle bir hub ile ilişkilendirildiğinde, bu, *[güvenli bir sanal hub](secured-virtual-hub.md)* olarak adlandırılır. 
- **Hub sanal ağı**

   Bu, kendi oluşturduğunuz ve yönettiğiniz standart bir Azure sanal ağı. Güvenlik ilkeleri böyle bir hub ile ilişkilendirildiğinde, *hub sanal ağı*olarak adlandırılır. Şu anda yalnızca Azure Güvenlik Duvarı Ilkesi desteklenir. İş yükü sunucularınızı ve hizmetlerinizi içeren sanal ağları eşleyebilir. Ayrıca, herhangi bir bağlı ağa eşlenmiş olmayan tek başına sanal ağlarda güvenlik duvarlarını yönetebilirsiniz.

*Güvenli sanal hub* ve *hub sanal ağ* mimarilerinin ayrıntılı bir karşılaştırması Için bkz. [Azure Güvenlik Duvarı Yöneticisi mimari seçenekleri nelerdir?](vhubs-and-vnets.md).

![Güvenlik Duvarı-yönetici](media/overview/trusted-security-partners.png)

## <a name="azure-firewall-manager-features"></a>Azure Güvenlik Duvarı Yöneticisi özellikleri

Azure Güvenlik Duvarı Yöneticisi aşağıdaki özellikleri sunar:

### <a name="central-azure-firewall-deployment-and-configuration"></a>Merkezi Azure Güvenlik Duvarı dağıtımı ve yapılandırması

Farklı Azure bölgelerine ve aboneliklerine yayılan birden çok Azure Güvenlik Duvarı örneğini merkezi olarak dağıtabilir ve yapılandırabilirsiniz. 

### <a name="hierarchical-policies-global-and-local"></a>Hiyerarşik ilkeler (genel ve yerel)

Azure Güvenlik Duvarı ilkelerini birden çok güvenli sanal hub genelinde merkezi olarak yönetmek için Azure Güvenlik Duvarı yöneticisini kullanabilirsiniz. Merkezi BT ekipleriniz, ekipler genelinde kurumsal çapta güvenlik duvarı ilkesini zorlamak için küresel güvenlik duvarı ilkelerini yazabilir. Yerel olarak yazılmış güvenlik duvarı ilkeleri, daha iyi çeviklik için bir DevOps self servis modeline izin verir.

### <a name="integrated-with-third-party-security-as-a-service-for-advanced-security"></a>Gelişmiş güvenlik için üçüncü taraf hizmet olarak güvenlik ile tümleşik

Azure Güvenlik Duvarı 'na ek olarak, VNet ve şube Internet bağlantılarınız için ek ağ koruması sağlamak üzere üçüncü taraf güvenlik hizmeti (SECaaS) sağlayıcılarını tümleştirebilirsiniz.

Bu özellik yalnızca güvenli sanal hub dağıtımları ile kullanılabilir.

- VNet 'Ten Internet 'e (V2I) trafik filtreleme

   - Tercih ettiğiniz üçüncü taraf güvenlik sağlayıcınızla giden sanal ağ trafiğini filtreleyin.
   - Azure üzerinde çalışan bulut iş yükleriniz için Gelişmiş Kullanıcı tarafından uyumlu Internet korumasından yararlanın.

- Internet 'Ten dallan (B2I) trafik filtreleme

   Internet senaryolarına şube için üçüncü taraf filtrelemesini kolayca eklemek üzere Azure bağlantınız ve küresel dağıtım özelliğinden yararlanın.

Güvenlik iş ortağı sağlayıcıları hakkında daha fazla bilgi için bkz. [Azure Güvenlik Duvarı Yöneticisi güvenlik iş ortağı sağlayıcıları nelerdir?](trusted-security-partners.md)

### <a name="centralized-route-management"></a>Merkezi yol yönetimi

Bağlı olan sanal ağlarda Kullanıcı tanımlı yolları (UDR) el ile ayarlama gereksinimi olmadan, filtreleme ve günlüğe kaydetme için güvenli hub 'ınıza trafiği kolayca yönlendirin. 

Bu özellik yalnızca güvenli sanal hub dağıtımları ile kullanılabilir.

Şube için üçüncü taraf sağlayıcıları (B2I) trafik filtrelemesini, VNet 'Ten sanal ağa (B2V), VNET 'Ten VNET 'e (V2V) ve VNET 'Ten Internet 'e (V2I) yönelik Azure Güvenlik Duvarı ile yan yana kullanabilirsiniz. V2I trafik filtreleme için üçüncü taraf sağlayıcıları, B2V veya V2V için Azure Güvenlik Duvarı gerekmediği sürece de kullanabilirsiniz. 

## <a name="region-availability"></a>Bölge kullanılabilirliği

Azure Güvenlik Duvarı Ilkeleri bölgeler arasında kullanılabilir. Örneğin, Batı ABD bir ilke oluşturabilir ve Doğu ABD ' de kullanabilirsiniz. 

## <a name="known-issues"></a>Bilinen sorunlar

Azure Güvenlik Duvarı Yöneticisi aşağıdaki bilinen sorunlara sahiptir:

|Sorun  |Description  |Risk azaltma  |
|---------|---------|---------|
|Trafik bölme şu anda desteklenmiyor.|Office 365 ve Azure genel PaaS trafik bölünmesi Şu anda desteklenmiyor. Bu nedenle, V2I veya B2I için bir üçüncü taraf sağlayıcı seçilirse ortak hizmet aracılığıyla tüm Azure genel PaaS ve Office 365 trafiği de gönderilir.|Hub 'da trafik bölmeyi araştırma.
|Bölge başına bir güvenli sanal hub.|Bölge başına birden fazla güvenli sanal hub 'ınız olamaz.|Bir bölgede birden çok sanal WAN oluşturun.|
|Temel ilkeler yerel ilkeyle aynı bölgede olmalıdır.|Tüm yerel ilkelerinizi temel ilkeyle aynı bölgede oluşturun. Başka bir bölgeden güvenli bir hub 'daki tek bir bölgede oluşturulmuş bir ilkeyi yine de uygulayabilirsiniz.|Araştırılıyor|
|Güvenli sanal hub ile hub arası iletişim çalışmıyor|Güvenli sanal hub, güvenli sanal hub iletişimine henüz desteklenmiyor.|Araştırılıyor|
|Aynı sanal WAN 'ı paylaşan tüm güvenli sanal hub 'Lar aynı kaynak grubunda olmalıdır.|Bu davranış, bugün sanal WAN hub 'Ları ile hizalanır.|Farklı kaynak gruplarında güvenli sanal hub 'Ların oluşturulmasını sağlamak için birden çok sanal WAN oluşturun.|

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Güvenlik Duvarı Yöneticisi dağıtımına genel bakış](deployment-overview.md) 'ı inceleyin
- [Güvenli sanal hub 'lar](secured-virtual-hub.md)hakkında bilgi edinin.