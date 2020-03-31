---
title: Azure Güvenlik Duvarı Yöneticisi Önizleme dağıtımına genel bakış
description: Azure Güvenlik Duvarı Yöneticisi Önizlemesi için gereken üst düzey dağıtım adımlarını öğrenin
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: overview
ms.date: 02/18/2020
ms.author: victorh
ms.openlocfilehash: c3a94cea838609f65511a21ee2f64e8782a6adea
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77443134"
---
# <a name="azure-firewall-manager-preview-deployment-overview"></a>Azure Güvenlik Duvarı Yöneticisi Önizleme dağıtımına genel bakış

[!INCLUDE [Preview](../../includes/firewall-manager-preview-notice.md)]

Azure Güvenlik Duvarı Yöneticisi Önizlemesini dağıtmanın birden fazla yolu vardır, ancak aşağıdaki genel işlem önerilir.

## <a name="general-deployment-process"></a>Genel dağıtım işlemi

### <a name="hub-virtual-networks"></a>Hub sanal ağlar

1.  Güvenlik duvarı ilkesi oluşturma

    - Yeni ilke oluşturma
<br>*Veya*<br>
    - Temel bir ilke türetin ve yerel bir ilkeyi özelleştirin
<br>*Veya*<br>
    - Kuralları varolan bir Azure Güvenlik Duvarı'ndan alın. Birden çok güvenlik duvarında uygulanması gereken ilkelerden NAT kurallarını kaldırdıktan emin olun
1. Hub ve kollu mimarinizi oluşturun
   - Azure Güvenlik Duvarı Yöneticisi ve eş konuşan sanal ağları kullanarak sanal ağ eşlemi kullanarak Hub Sanal Ağı oluşturun
<br>*Veya*<br>
    - Sanal ağ oluşturma ve sanal ağ eşlemi kullanarak sanal ağ bağlantıları ve eş kollu sanal ağlar ekleyin

3. Güvenlik sağlayıcılarını seçin ve güvenlik duvarı ilkesini ilişkilendirin. Şu anda yalnızca Azure Güvenlik Duvarı desteklenen bir sağlayıcıdır.

   - Hub Sanal Ağ oluştururken bu işlem yapılır
<br>*Veya*<br>
    - Varolan bir sanal ağı Hub Sanal Ağa dönüştürün. Birden çok sanal ağı dönüştürmek de mümkündür.

4. Trafiği Hub Sanal Ağ güvenlik duvarınıza yönlendirmek için Kullanıcı Tanımrotalarını yapılandırın.


### <a name="secured-virtual-hubs"></a>Güvenli sanal hub'lar

1. Hub ve kollu mimarinizi oluşturun

   - Azure Güvenlik Duvarı Yöneticisi'ni kullanarak Güvenli Sanal Hub oluşturun ve sanal ağ bağlantıları ekleyin.<br>*Veya*<br>
   - Sanal WAN Hub'ı oluşturun ve sanal ağ bağlantıları ekleyin.
2. Güvenlik sağlayıcılarını seçin

   - Güvenli Sanal Hub oluştururken yapılır.<br>*Veya*<br>
   - Varolan bir Sanal WAN Hub'ı Güvenli Sanal Hub'a dönüştürün.
3. Bir güvenlik duvarı ilkesi oluşturun ve hub'ınızla ilişkilendirin

   - Yalnızca Azure Güvenlik Duvarı kullanıyorsanız geçerlidir.
   - Hizmet olarak üçüncü taraf güvenlik (SECaaS) ilkeleri iş ortağı yönetimi deneyimi ile yapılandırılır.
4. Trafiği güvenli hub'ınıza yönlendirmek için rota ayarlarını yapılandırın

   - Güvenli Sanal Hub Rota Ayarı sayfasını kullanarak, kollu Sanal Ağlarda Kullanıcı Tanımlı Rotalar (UDR) olmadan filtreleme ve günlüğe kaydetme için trafiği güvenli merkezinize kolayca yönlendirin.

> [!NOTE]
> - Bölge başına sanal wan başına birden fazla hub'A sahip olamazsınız. Ancak bunu başarmak için bölgeye birden fazla sanal WAN ekleyebilirsiniz.
> - VWAN'daki hub'lar için çakışan IP alanlarına sahip olamazsınız.
> - Hub VNet bağlantılarınız hub ile aynı bölgede olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Azure portalını kullanarak Bulut ağınızı Azure Güvenlik Duvarı Yöneticisi Önizlemeile güvenli hale](secure-cloud-network.md)