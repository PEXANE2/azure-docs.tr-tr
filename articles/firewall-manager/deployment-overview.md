---
title: Azure Güvenlik Duvarı Yöneticisi dağıtımına genel bakış
description: Azure Güvenlik Duvarı Yöneticisi için gereken üst düzey dağıtım adımlarını öğrenin
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: overview
ms.date: 08/10/2020
ms.author: victorh
ms.openlocfilehash: 8cca982e291d5d4caf7856e0d807515511c41dc8
ms.sourcegitcommit: 269da970ef8d6fab1e0a5c1a781e4e550ffd2c55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88055046"
---
# <a name="azure-firewall-manager-deployment-overview"></a>Azure Güvenlik Duvarı Yöneticisi dağıtımına genel bakış

Azure Güvenlik Duvarı Yöneticisi 'Ni dağıtmanın birden çok yolu vardır, ancak aşağıdaki genel süreç önerilir.

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
>
> Daha bilinen sorunlar için bkz. [Azure Güvenlik Duvarı Yöneticisi nedir?](overview.md#known-issues)

## <a name="convert-virtual-networks"></a>Sanal ağları dönüştürme

Mevcut bir sanal ağı bir hub sanal ağına dönüştürürseniz aşağıdaki bilgiler geçerlidir:

- Sanal ağda mevcut bir Azure Güvenlik duvarı varsa, mevcut güvenlik duvarıyla ilişkilendirilecek bir güvenlik duvarı Ilkesi seçersiniz. Güvenlik duvarı ilkesi güvenlik duvarı kurallarını değiştirirken güvenlik duvarı sağlama durumu güncellenebilir. Sağlama durumu sırasında güvenlik duvarı, işlem trafiğini işleme devam eder ve kapalı kalma süresi yoktur. Güvenlik Duvarı Yöneticisi veya Azure PowerShell kullanarak mevcut kuralları bir güvenlik duvarı Ilkesine aktarabilirsiniz.
- Sanal ağın ilişkili bir Azure Güvenlik duvarı yoksa, güvenlik duvarı dağıtılır ve güvenlik duvarı Ilkesi yeni güvenlik duvarıyla ilişkilendirilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure portal kullanarak bulut ağınızı Azure Güvenlik Duvarı Yöneticisi ile güvenli hale getirin](secure-cloud-network.md)