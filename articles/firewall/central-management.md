---
title: Azure Güvenlik Duvarı merkezi yönetimi
description: Azure Güvenlik Duvarı Yöneticisi merkezi yönetimi hakkında bilgi edinin
author: vhorne
ms.service: firewall
services: firewall
ms.topic: conceptual
ms.date: 02/18/2020
ms.author: victorh
ms.openlocfilehash: 58f670f3f55a63f0c1823adc13c98f5863d4d650
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77444556"
---
# <a name="azure-firewall-central-management"></a>Azure Güvenlik Duvarı merkezi yönetimi

[!INCLUDE [Preview](../../includes/firewall-manager-preview-notice.md)]

Birden çok güvenlik duvarı yönetiyorsanız, sürekli değişen güvenlik duvarı kurallarının onları eşit tutmayı zorlaştırdığını bilirsiniz. Merkezi BT ekiplerinin temel güvenlik duvarı ilkelerini tanımlamak ve bunları birden çok iş biriminde uygulamak için bir yol gerekir. Aynı zamanda, DevOps ekipleri daha iyi çeviklik için kendi yerel türetilmiş güvenlik duvarı ilkelerini oluşturmak ister.

Azure Güvenlik Duvarı Yöneticisi Önizlemebu sorunların çözümüne yardımcı olabilir.


## <a name="azure-firewall-manager-preview"></a>Azure Güvenlik Duvarı Yöneticisi Önizlemesi

Azure Güvenlik Duvarı Yöneticisi Önizleme, bulut tabanlı güvenlik çevreleri için merkezi güvenlik ilkesi ve rota yönetimi sağlayan bir ağ güvenliği yönetimi hizmetidir. Kurumsal BT ekiplerinin birden çok Azure Güvenlik Duvarı örneğinde trafik filtreleme için ağ ve uygulama düzeyi kurallarını merkezi olarak tanımlamasını kolaylaştırır. Trafik yönetimi ve koruması için hub ve kollu mimarilerde farklı Azure bölgelerine ve aboneliklerine açabilirsiniz. Ayrıca, kuruluşlar arasında uygulanan türemiş yerel güvenlik duvarı güvenlik ilkeleriyle DevOps'lere daha iyi çeviklik sağlar.

### <a name="firewall-policy"></a>Güvenlik duvarı ilkesi

Güvenlik Duvarı ilkesi, NAT, ağ ve uygulama kuralı koleksiyonları ve Tehdit İstihbaratı ayarlarını içeren bir Azure kaynağıdır. *Güvenli Sanal Hub'larda* ve Hub *Sanal Ağlarda*birden çok Azure Güvenlik Duvarı örneğinde kullanılabilen genel bir kaynaktır. Yeni ilkeler sıfırdan oluşturulabilir veya varolan ilkelerden devralınabilir. Devralma, DevOps'lerin kuruluş zorunlu temel ilkesinin üstünde yerel güvenlik duvarı ilkeleri oluşturmasına olanak tanır. İlkeler bölgeler ve abonelikler arasında çalışır.
 
Güvenlik Duvarı İlkesi ve Azure Güvenlik Duvarı Yöneticisi ile ilişkilendirmeler oluşturabilirsiniz. Ancak, REST API, şablonlar, Azure PowerShell ve CLI'yi kullanarak bir ilke oluşturabilir ve yönetebilirsiniz. Bir ilke oluşturduktan sonra, onu sanal WAN hub'ındaki bir güvenlik duvarıyla ilişkilendirerek güvenli sanal *hub* ve/veya sanal ağdaki bir güvenlik duvarı haline getirerek *Hub Sanal Ağ'ı oluşturabilirsiniz.*

### <a name="pricing"></a>Fiyatlandırma

İlkeler, güvenlik duvarı ilişkilendirmelerine göre faturalandırılır. Sıfır veya bir güvenlik duvarı ilişkisi olan bir ilke ücretsizdir. Birden çok güvenlik duvarı ilişkilendirmeleri içeren bir ilke sabit bir oranda faturalandırılır. Daha fazla bilgi için Azure [Güvenlik Duvarı Yöneticisi Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/firewall-manager/)bakın.

## <a name="azure-firewall-management-partners"></a>Azure Güvenlik Duvarı Yönetimi ortakları

Aşağıdaki önde gelen üçüncü taraf çözümleri, standart Azure REST API'lerini kullanarak Azure Güvenlik Duvarı merkezi yönetimini destekler. Bu çözümlerin her birinin kendine özgü özellikleri ve özellikleri vardır:

- [AlgoSec CloudFlow](https://www.algosec.com/azure/) 
- [Barracuda Bulut Güvenlik Guardian](https://www.barracuda.com/products/cloudsecurityguardian/for_azure)
- [Tufin Orca](https://www.tufin.com/products/tufin-orca)


## <a name="next-steps"></a>Sonraki adımlar

Azure Güvenlik Duvarı Yöneticisi Önizlemesi hakkında daha fazla bilgi [için](../firewall-manager/overview.md) bkz.