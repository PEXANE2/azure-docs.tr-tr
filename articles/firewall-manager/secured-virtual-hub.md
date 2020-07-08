---
title: Güvenli sanal merkez nedir?
description: Güvenli sanal merkezler hakkında bilgi edinin
author: vhorne
ms.service: firewall-manager
services: firewall-manager
ms.topic: conceptual
ms.date: 06/30/2020
ms.author: victorh
ms.openlocfilehash: 56e0d40bcbfb97f57b63dc82da1a6604f83dffad
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85563618"
---
# <a name="what-is-a-secured-virtual-hub"></a>Güvenli sanal merkez nedir?

Sanal hub, diğer kaynaklardan bağlantı sağlayan, Microsoft tarafından yönetilen bir sanal ağ. Azure portal sanal bir WAN 'dan sanal bir hub oluşturulduğunda, bileşenleri olarak bir sanal hub VNet ve ağ geçitleri (isteğe bağlı) oluşturulur.

*Güvenli* bir sanal hub, Azure Güvenlik Duvarı Yöneticisi tarafından yapılandırılan ilişkili güvenlik ve yönlendirme ilkelerine sahip bir [Azure sanal WAN hub 'ına](../virtual-wan/virtual-wan-about.md#resources) sahiptir. Trafik yönetimi ve koruması için yerel güvenlik hizmetleriyle hub ve bağlı bileşen ve geçişli mimarilerin kolayca oluşturulmasını sağlamak için güvenli sanal hub 'ları kullanın. 

Şirket içi bağlantı olmadan yönetilen merkezi VNet olarak güvenli bir sanal hub kullanabilirsiniz. Daha önce bir Azure Güvenlik Duvarı dağıtımı için gerekli olan merkezi VNet 'in yerini alır. Güvenli sanal hub otomatik yönlendirme sağladığından, trafiği güvenlik duvarınız üzerinden yönlendirmek için kendi UDRs 'nizi (Kullanıcı tanımlı yollar) yapılandırmanız gerekmez.

Tam sanal WAN mimarisinin bir parçası olarak güvenli sanal hub 'ları kullanmak da mümkündür. Bu mimari, Azure ile ve arasında güvenli, iyileştirilmiş ve otomatik bir dal bağlantısı sağlar. Azure Güvenlik Duvarı ve hizmet olarak diğer üçüncü taraf güvenlik (SECaaS) sağlayıcıları da dahil olmak üzere ağ trafiğinizi korumak ve yönetmek için Hizmetleri seçebilirsiniz.

## <a name="create-a-secured-virtual-hub"></a>Güvenli sanal hub oluşturma

Azure portal güvenlik duvarı Yöneticisi 'Ni kullanarak, yeni bir güvenli sanal hub oluşturabilir ya da daha önce Azure sanal WAN kullanarak oluşturduğunuz var olan bir sanal hub 'ı dönüştürebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Güvenli bir sanal hub oluşturmak ve bir hub ve bağlı ağ ağını güvenli hale getirmek ve yönetmek için kullanmak üzere, [Azure Portal kullanarak bkz. Öğretici: bulut ağınızı Azure Güvenlik Duvarı Yöneticisi Ile güvenli hale getirme](secure-cloud-network.md).