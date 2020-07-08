---
title: 'Senaryo: Azure Güvenlik Duvarı-ınterhub yönlendirme'
titleSuffix: Azure Virtual WAN
description: Azure Güvenlik duvarı olan birden çok hub arasında yönlendirme yönlendirmesi için senaryolar
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: 1057dc28682c90374b2f8e8e0297f3d769c08bd6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85568983"
---
# <a name="scenario-azure-firewall---interhub-preview"></a>Senaryo: Azure Güvenlik Duvarı-ınterhub (Önizleme)

Sanal WAN sanal hub 'ı yönlendirme ile çalışırken, kullanılabilecek oldukça az sayıda senaryo vardır. Bu senaryoda, hedef Azure Güvenlik duvarı içeren birden çok hub arasında yönlendirme sağlamaktır. Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).

## <a name="scenario-architecture"></a><a name="architecture"></a>Senaryo mimarisi

Bu senaryoda, aşağıdaki yapılandırmayı varsayıyoruz:

* Her hub 'da Azure Güvenlik Duvarı ile birden fazla hub 'ınız var.
* Güvenli sanal hub kullanıyorsunuz.
* Özel trafik (VNet), Internet ve şube trafiği için uygun ilkeler ayarlandı.
* VNet 'Ten Internet 'e (V2I), sanal ağdan dala (V2B), Dalla sanal ağa (B2V), hepsi her hub 'da Azure Güvenlik Duvarı üzerinden geçer.

Ek hususlar:

* Azure Güvenlik Duvarı ile hub arası bir senaryo için, bağlı olan VNET 'lerin ayrı rota tablolarıyla ilişkilendirilmediğinden emin olmanız gerekir. (örneğin, **yol tablosu A**Ile ilişkili **VNET 1** ve **yol tablosu B**ile ilişkili **VNET 2** ). Tüm VNET 'ler, Azure Güvenlik Duvarı yollarının bulunduğu aynı yol tablosuyla ilişkilendirir.
* Azure Güvenlik Duvarı üzerinden güvenli hale getirme, şu anda **şube < > VNET** ve yalnızca **Internet** trafiği ile sınırlıdır. Azure Güvenlik Duvarı aracılığıyla Dalla dal akışı şu anda desteklenmiyor.

**Şekil 1**

:::image type="content" source="./media/routing-scenarios/between-hubs-firewall/architecture.png" alt-text="mimarisini":::

## <a name="scenario-workflow"></a><a name="workflow"></a>Senaryo iş akışı

Bu senaryoyu yapılandırmak için aşağıdaki adımları göz önünde bulundurun:

### <a name="step-1"></a><a name="step-1"></a>1\. Adım

Azure Güvenlik Duvarı Yöneticisi aracılığıyla bağlantıları zaten güvenli hale getirebildiğiniz varsayılırsa, ilk adım tüm dalın ve VNet bağlantılarının **hiçbirini**dağıtmayı sağlamaktır. Bu, trafiğin Azure Güvenlik Duvarı aracılığıyla ayarlanmakta olduğundan emin olmak için gereklidir.

1. Hub 'ı seçin ve **yok** yol tablosunu düzenleyin.
1. Tüm dalları ve tüm sanal ağları seçmek için **yayılmayı** güncelleştirin.

### <a name="step-2"></a><a name="step-2"></a>2. adım

Hub başına özel bir yol tablosu ayarlayın.

1. **Merkez 1**Için **RT_Hub1**yol tablosu oluşturun.

    * Azure Güvenlik Duvarı yöneticisini kullandıysanız, hub 'ın varsayılan yol tablosundaki sonraki atlama **AZFW1** ile 0.0.0.0/0 için otomatik olarak bir yol oluşturur. Bu ayarı adım 3 ' te değiştireceğiz. **RT_Hub1**için, sonraki atlama **AZFW1**ile 0.0.0.0/0 için açıkça bir giriş oluşturun. Bu ayar AZFW1 aracılığıyla V2V, B2V ve V2I 'yi etkinleştirir.
    * Merkez **2** dallarına ve sanal ağlara **AZFW2** üzerinden **hub 1** ' den (AZFW1 aracılığıyla değil) ulaşıldığından, **Merkez 2** dalları (10.5.0.0/16->AZFW2) ve sanal ağlar (10.2.0.0/16->AZFW2) için başka 2 toplanmış yollar eklemeniz gerekir.

1. **Merkez 2**Için **RT_Hub2**yol tablosu oluşturun.

    * Azure Güvenlik Duvarı yöneticisini kullandıysanız, hub 'ın varsayılan yol tablosundaki sonraki atlama **AZFW2** ile 0.0.0.0/0 için otomatik olarak bir yol oluşturur. Bu ayarı adım 3 ' te değiştireceğiz. **RT_Hub2**için, sonraki atlama **AZFW2**ile 0.0.0.0/0 için açıkça bir giriş oluşturun. Bu ayar **AZFW2**aracılığıyla V2V, B2V ve V2I 'yi etkinleştirir.
    * Hub 2 ' nin **AZFW2**ile korunmasını istediğiniz için, önceki Merkez 1 adımındaki ikinci madde işaretine benzer bir adım eklemeniz gerekmez.

### <a name="step-3"></a><a name="step-3"></a>3. adım

Azure Güvenlik Duvarı aracılığıyla **VNET 'e** (B2V) ve **daldan Internet** (B2I) akışlarına yönelik statik bir yol eklemek Için her hub 'daki **varsayılan yol tablosunu** değiştirin. Dala dallan Şu anda Azure Güvenlik Duvarı üzerinden desteklenmiyor.

1. **Hub 1 varsayılan yol tablosu**için:

    * AZFW2:10.5.0.0/16->AZFW2 **aracılığıyla Merkez 2 dallarına dallan**. Bu yapılandırma, Merkez 2 güvenlik duvarı için bir yol ayarlar.
    * AZFW2:10.2.0.0/16->AZFW2 **aracılığıyla Merkez 2 sanal ağlarına dallan**. Bu yapılandırma, Merkez 2 güvenlik duvarı için bir yol ayarlar.
    * Dala dalı **(yerel)/dala sanal ağa (yerel)/** dala dallan: 0.0.0.0/0->AZFW1.

2. **Merkez 2 varsayılan yol tablosu**için:

    * Dala dalı (yerel ve uzak)/sanal ağa (yerel ve uzak)/dala dallan: 0.0.0.0/0->AZFW2.

Bu, aşağıdaki şekilde görüldüğü gibi yönlendirme yapılandırması değişikliklerine neden olur

**Şekil 2**

:::image type="content" source="./media/routing-scenarios/between-hubs-firewall/workflow.png" alt-text="iş akışı":::

## <a name="next-steps"></a>Sonraki adımlar

* Sanal WAN hakkında daha fazla bilgi için bkz. [SSS](virtual-wan-faq.md).
* Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).
* Sanal hub yönlendirmeyi yapılandırma hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmeyi yapılandırma](how-to-virtual-hub-routing.md).
