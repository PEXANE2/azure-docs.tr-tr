---
title: Azure Güvenlik Duvarı Yöneticisi Önizleme ilkesine genel bakış
description: Azure Güvenlik Duvarı Yöneticisi ilkeleri hakkında bilgi edinin
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: overview
ms.date: 02/18/2020
ms.author: victorh
ms.openlocfilehash: 1308f4ba3335f2fd2633f6e39a679cd6477a4b5c
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77445024"
---
# <a name="azure-firewall-manager-preview-policy-overview"></a>Azure Güvenlik Duvarı Yöneticisi Önizleme ilkesine genel bakış

[!INCLUDE [Preview](../../includes/firewall-manager-preview-notice.md)]

Güvenlik duvarı ilkesi, Tehdit İstihbaratı ayarlarının yanı sıra NAT, ağ ve uygulama kuralı koleksiyonları da içeren bir Azure kaynağıdır. Güvenli Sanal Hub'larda ve Hub Sanal Ağlarda birden çok Azure Güvenlik Duvarı örneğinde kullanılabilen küresel bir kaynaktır. İlkeler bölgeler ve abonelikler arasında çalışır.

![Azure Güvenlik Duvarı Yöneticisi ilkesi](media/policy-overview/policy-overview.png)

## <a name="policy-creation-and-association"></a>İlke oluşturma ve ilişkilendirme

Bir ilke, Azure portalı, REST API, şablonlar, Azure PowerShell ve CLI dahil olmak üzere birden çok şekilde oluşturulabilir ve yönetilebilir.

İlkeler oluşturmak için portalı veya Azure PowerShell'i kullanarak varolan kuralları Azure Güvenlik Duvarı'ndan da geçirebilirsiniz. Daha fazla bilgi için Azure [Güvenlik Duvarı yapılandırmalarını Azure Güvenlik Duvarı ilkesine (önizleme) nasıl geçirebilirsiniz'](migrate-to-policy.md)e bakın. 

İlkeler bir veya daha fazla sanal hub veya VNets ile ilişkilendirilebilir. Güvenlik duvarı hesabınızla ilişkili herhangi bir abonelikte ve herhangi bir bölgede olabilir.

## <a name="hierarchical-policies"></a>Hiyerarşik ilkeler

Yeni ilkeler sıfırdan oluşturulabilir veya varolan ilkelerden devralınabilir. Devralma, DevOps'lerin kuruluş zorunlu temel ilkesinin üstünde yerel güvenlik duvarı ilkeleri oluşturmasına olanak tanır.

Boş olmayan üst ilkelerle oluşturulan ilkeler, tüm kural koleksiyonlarını üst ilkeden devralır. Bir üst ilkeden devralınan ağ kuralı koleksiyonları her zaman yeni bir ilkenin parçası olarak tanımlanan ağ kuralı koleksiyonlarının üzerinde öncelikverilir. Aynı mantık uygulama kuralı koleksiyonları için de geçerlidir. Ancak, ağ kuralı koleksiyonları her zaman devralma ne olursa olsun uygulama kuralı koleksiyonları önce işlenir.

Tehdit İstihbaratı modu da üst politikadan devralır. Bu davranışı geçersiz kılmak için tehdit İstihbarat modunuzu farklı bir değere ayarlayabilirsiniz, ancak kapatamazsınız. Sadece daha katı bir değerle geçersiz kılmak mümkündür. Örneğin, ana politikanız yalnızca **Uyarı olarak**ayarlanmışsa, bu yerel ilkeyi **Uyarı ve reddetmek**üzere yapılandırabilirsiniz.

NAT kural koleksiyonları, belirli bir güvenlik duvarına özgü olduğundan devralınmaz.

Devralma ile, üst politikadaki tüm değişiklikler otomatik olarak ilişkili güvenlik duvarı alt ilkelerine uygulanır.

## <a name="traditional-rules-and-policies"></a>Geleneksel kurallar ve politikalar

Azure Güvenlik Duvarı hem geleneksel kuralları hem de ilkeleri destekler. Aşağıdaki tablo ilkeleri ve kuralları karşılaştırır:


|         |İlke  |Kurallar  |
|---------|---------|---------|
|Contains     |NAT, Ağ, Uygulama kuralları ve Tehdit İstihbaratı ayarları|NAT, Ağ ve Uygulama kuralları |
|Korur     |Sanal hub'lar ve Sanal Ağlar|Yalnızca Sanal Ağlar|
|Portal deneyimi     |Güvenlik Duvarı Yöneticisi'ni kullanarak merkezi yönetim|Bağımsız güvenlik duvarı deneyimi|
|Birden çok güvenlik duvarı desteği     |Güvenlik Duvarı İlkesi, güvenlik duvarlarında kullanılabilecek ayrı bir kaynaktır|El ile verme ve alma kuralları veya üçüncü taraf yönetim çözümlerini kullanma |
|Fiyatlandırma     |Güvenlik duvarı ilişkisine göre faturalandırılır. [Bkz. Fiyatlandırma](#pricing).|Ücretsiz|
|Desteklenen dağıtım mekanizmaları     |Portal, REST API, şablonlar, Azure PowerShell ve CLI|Portal, REST API, şablonlar, PowerShell ve CLI. |
|Sürüm Durumu     |Genel Önizleme|Genel Kullanılabilirlik|

## <a name="pricing"></a>Fiyatlandırma

İlkeler, güvenlik duvarı ilişkilendirmelerine göre faturalandırılır. Sıfır veya bir güvenlik duvarı ilişkisi olan bir ilke ücretsizdir. Birden çok güvenlik duvarı ilişkilendirmeleri içeren bir ilke sabit bir oranda faturalandırılır. Daha fazla bilgi için Azure [Güvenlik Duvarı Yöneticisi Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/firewall-manager/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure Güvenlik Duvarı'nı nasıl dağıtabileceğinizi öğrenmek için [Bkz.](secure-cloud-network.md)
