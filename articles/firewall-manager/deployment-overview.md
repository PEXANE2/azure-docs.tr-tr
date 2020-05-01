---
title: Azure Güvenlik Duvarı Yöneticisi önizleme dağıtımına genel bakış
description: Azure Güvenlik Duvarı Yöneticisi önizlemesi için gereken üst düzey dağıtım adımlarını öğrenin
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: overview
ms.date: 02/18/2020
ms.author: victorh
ms.openlocfilehash: c3a94cea838609f65511a21ee2f64e8782a6adea
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "77443134"
---
# <a name="azure-firewall-manager-preview-deployment-overview"></a>Azure Güvenlik Duvarı Yöneticisi önizleme dağıtımına genel bakış

[!INCLUDE [Preview](../../includes/firewall-manager-preview-notice.md)]

Azure Güvenlik Duvarı Yöneticisi önizlemesi dağıtmanın birden çok yolu vardır, ancak aşağıdaki genel süreç önerilir.

## <a name="general-deployment-process"></a>Genel dağıtım işlemi

### <a name="hub-virtual-networks"></a>Hub sanal ağları

1.  Güvenlik duvarı ilkesi oluşturma

    - Yeni ilke oluşturma
<br>*veya*<br>
    - Temel ilke türetebilir ve yerel bir ilkeyi özelleştirin
<br>*veya*<br>
    - Mevcut bir Azure Güvenlik duvarındaki kuralları içeri aktarın. NAT kurallarını, birden çok güvenlik duvarı arasında uygulanması gereken ilkelerden kaldırdığınızdan emin olun
1. Hub ve bağlı bileşen mimarinizi oluşturma
   - Azure Güvenlik Duvarı Yöneticisi 'Ni kullanarak hub sanal ağı oluşturma ve sanal ağ eşlemesi kullanarak BT 'ye bağlı sanal ağlar
<br>*veya*<br>
    - Sanal ağ eşlemesi ile sanal ağ bağlantıları oluşturma ve sanal ağ bağlantılarını ve eş bağlı bileşen sanal ağlarını bu ağa ekleme

3. Güvenlik sağlayıcıları ' nı seçin ve güvenlik duvarı ilkesini ilişkilendirin. Şu anda yalnızca Azure Güvenlik Duvarı desteklenen bir sağlayıcıdır.

   - Bu, bir hub sanal ağı oluştururken yapılır
<br>*veya*<br>
    - Var olan bir sanal ağı hub sanal ağına dönüştürün. Birden çok sanal ağı dönüştürmek da mümkündür.

4. Kullanıcı yapılandırma trafiği hub sanal ağ güvenlik duvarınızla yönlendirmek için yollar tanımlayın.


### <a name="secured-virtual-hubs"></a>Güvenli sanal hub 'lar

1. Hub ve bağlı bileşen mimarinizi oluşturma

   - Azure Güvenlik Duvarı Yöneticisi 'Ni kullanarak güvenli bir sanal hub oluşturun ve sanal ağ bağlantıları ekleyin.<br>*veya*<br>
   - Sanal bir WAN hub 'ı oluşturun ve sanal ağ bağlantıları ekleyin.
2. Güvenlik sağlayıcıları seçin

   - Güvenli bir sanal hub oluşturulurken bitti.<br>*veya*<br>
   - Mevcut bir sanal WAN hub 'ını güvenli sanal hub 'a dönüştürün.
3. Bir güvenlik duvarı ilkesi oluşturup hub 'ınız ile ilişkilendirin

   - Yalnızca Azure Güvenlik Duvarı kullanılıyorsa geçerlidir.
   - Hizmet olarak üçüncü taraf güvenlik (SECaaS) ilkeleri, iş ortakları yönetim deneyimi aracılığıyla yapılandırılır.
4. Trafiği güvenli hub 'ınıza yönlendirmek için yol ayarlarını yapılandırma

   - Güvenli sanal hub yolu ayarı sayfasını kullanarak, bağlı olan sanal ağlarda Kullanıcı tanımlı yollar (UDR) olmadan filtreleme ve günlüğe kaydetme için trafiği güvenli bir şekilde kolayca yönlendirin.

> [!NOTE]
> - Her bölge için sanal WAN başına birden fazla hub 'ınız olamaz. Ancak bunu elde etmek için bölgeye birden çok sanal WAN ekleyebilirsiniz.
> - VWAN 'da hub 'lar için çakışan IP alanları olamaz.
> - Hub VNet bağlantılarınızın hub ile aynı bölgede olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure portal kullanarak bulut ağınızı Azure Güvenlik Duvarı Yöneticisi önizleme ile koruyun](secure-cloud-network.md)