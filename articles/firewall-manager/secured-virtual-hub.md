---
title: Güvenli sanal merkez nedir?
description: Güvenli sanal merkezler hakkında bilgi edinin
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: victorh
ms.openlocfilehash: d93c3b47c518962bf45762b846eaf53e490ae8f8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73518443"
---
# <a name="what-is-a-secured-virtual-hub"></a>Güvenli sanal merkez nedir?

[!INCLUDE [Preview](../../includes/firewall-manager-preview-notice.md)]

Sanal hub, diğer kaynaklardan bağlantı sağlayan Microsoft tarafından yönetilen bir sanal ağdır. Azure portalındaki sanal WAN'dan sanal bir hub oluşturulduğunda, bileşenleri olarak sanal hub VNet ve ağ geçitleri (isteğe bağlı) oluşturulur.

*Güvenli* bir sanal hub, Azure Güvenlik Duvarı Yöneticisi tarafından yapılandırılan ilişkili güvenlik ve yönlendirme ilkelerine sahip bir Azure Sanal WAN Hub'ıdır. [Azure Virtual WAN Hub](../virtual-wan/virtual-wan-about.md#resources) Trafik yönetimi ve koruması için yerel güvenlik hizmetleriyle hub ve kollu ve geçişli mimarileri kolayca oluşturmak için güvenli sanal hub'ları kullanın. 

Güvenli bir sanal hub'ı, ön-ön-bağlılık olmadan yönetilen merkezi VNet olarak kullanabilirsiniz. Azure Güvenlik Duvarı dağıtımı için daha önce gerekli olan merkezi VNet'in yerini alır. Güvenli sanal hub otomatik yönlendirme sağladığından, trafiği güvenlik duvarınızdan yönlendirmek için kendi ÜrR'lerinizi (kullanıcı tanımlı rotalar) yapılandırmaya gerek yoktur.

Güvenli sanal hub'ları tam bir Sanal WAN mimarisinin parçası olarak da kullanmak mümkündür. Bu mimari, Azure'a ve Azure üzerinden güvenli, optimize edilmiş ve otomatikleştirilmiş şube bağlantısı sağlar. Azure Güvenlik Duvarı ve diğer üçüncü taraf güvenlik hizmetleri (SECaaS) sağlayıcıları da dahil olmak üzere ağ trafiğinizi korumak ve yönetmek için hizmetleri seçebilirsiniz.

## <a name="create-a-secured-virtual-hub"></a>Güvenli bir sanal hub oluşturma

Azure portalında Güvenlik Duvarı Yöneticisi'ni kullanarak, yeni bir güvenli sanal hub oluşturabilir veya Azure Virtual WAN kullanarak daha önce oluşturduğunuz varolan bir sanal hub'ı dönüştürebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Güvenli bir sanal hub oluşturmak ve hub ve kollu ağı güvenli ve yönetim de kullanmak için [bkz.](secure-cloud-network.md)