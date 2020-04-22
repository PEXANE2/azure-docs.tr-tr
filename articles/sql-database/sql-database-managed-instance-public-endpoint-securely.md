---
title: Güvenli yönetilen örnek ortak uç noktaları
description: Yönetilen bir örnekle Azure'da ortak uç noktaları güvenli bir şekilde kullanın
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto, carlrab
ms.date: 05/08/2019
ms.openlocfilehash: f11c19ba33ee2fbae0fef265371bedad2fe29cb7
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81684948"
---
# <a name="use-an-azure-sql-database-managed-instance-securely-with-public-endpoints"></a>Azure SQL Veritabanı yönetilen örneklerini ortak uç noktalarıyla güvenli bir şekilde kullanma

Azure SQL Veritabanı yönetilen [örnekleri, ortak uç noktalar](../virtual-network/virtual-network-service-endpoints-overview.md)üzerinden kullanıcı bağlantısı sağlayabilir. Bu makalede, bu yapılandırmanın nasıl daha güvenli hale getirilen açıklanmaktadır.

## <a name="scenarios"></a>Senaryolar

SQL Veritabanı yönetilen örneği, sanal ağının içinden bağlantıya izin vermek için özel bir bitiş noktası sağlar. Varsayılan seçenek maksimum yalıtım sağlamaktır. Ancak, genel bir bitiş noktası bağlantısı sağlamanız gereken senaryolar vardır:

- Yönetilen örnek, hizmet olarak çok sayıda kiracıya özel platform (PaaS) teklifleri ile tümleştirilmelidir.
- VPN kullanırken mümkün olandan daha yüksek veri alışverişi veri alışverişine ihtiyacınız vardır.
- Şirket politikaları, Şirket ağları içinde PaaS yasaklar.

## <a name="deploy-a-managed-instance-for-public-endpoint-access"></a>Genel uç nokta erişimi için yönetilen bir örneği dağıtma

Zorunlu olmasa da, genel uç nokta erişimine sahip yönetilen bir örnek için ortak dağıtım modeli, özel bir yalıtılmış sanal ağda örnek oluşturmaktır. Bu yapılandırmada, sanal ağ yalnızca sanal küme yalıtımı için kullanılır. Yönetilen örneğin IP adresi alanının bir şirket ağının IP adres alanıyla çakışması önemli değildir.

## <a name="secure-data-in-motion"></a>Güvenli veri hareket halinde

İstemci sürücü şifrelemeyi destekliyorsa, yönetilen örnek veri trafiği her zaman şifrelenir. Yönetilen örnek le diğer Azure sanal makineleri veya Azure hizmetleri arasında gönderilen veriler Azure'un omurgasını asla bırakmaz. Yönetilen örnekle şirket içi ağ arasında bir bağlantı varsa, Azure ExpressRoute'u kullanmanızı öneririz. ExpressRoute, verileri genel internet üzerinden taşımaktan kaçınmanıza yardımcı olur. Yönetilen örnek özel bağlantı için yalnızca özel eşleme kullanılabilir.

## <a name="lock-down-inbound-and-outbound-connectivity"></a>Gelen ve giden bağlantıyı kilitleme

Aşağıdaki diyagram, önerilen güvenlik yapılandırmalarını gösterir:

![Gelen ve giden bağlantının kilitlenmeleri için güvenlik yapılandırmaları](media/sql-database-managed-instance-public-endpoint-securely/managed-instance-vnet.png)

Yönetilen bir örneğin özel bir [genel bitiş noktası adresi](sql-database-managed-instance-find-management-endpoint-ip-address.md)vardır. İstemci tarafındaki giden güvenlik duvarında ve ağ güvenlik grubu kurallarında, giden bağlantının sınırını sınırlamak için bu genel uç nokta IP adresini ayarlayın.

Yönetilen örneğin trafiğin güvenilir kaynaklardan geldiğini sağlamak için, tanınmış IP adreslerine sahip kaynaklardan bağlanmanızı öneririz. Bağlantı noktası 3342'de yönetilen örnek ortak bitiş noktasına erişimi sınırlamak için bir ağ güvenlik grubu kullanın.

İstemcilerin şirket içi bir ağdan bağlantı başlatması gerektiğinde, kaynak adresin tanınmış bir IP adresi kümesine çevrildiğinden emin olun. Bunu yapamıyorsanız (örneğin, mobil iş gücü tipik bir senaryodur), [noktadan siteye VPN bağlantıları ve özel bir bitiş noktası](sql-database-managed-instance-configure-p2s.md)kullanmanızı öneririz.

Bağlantılar Azure'dan başlatılırsa, trafiğin iyi bilinen bir [atanmış sanal IP adresinden](../virtual-network/virtual-networks-reserved-public-ip.md) (örneğin, sanal makine) gelmesini öneririz. Sanal IP (VIP) adreslerini yönetmeyi kolaylaştırmak için [genel IP adresi önekleri](../virtual-network/public-ip-address-prefix.md)kullanmak isteyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Örnekleri yönetmek için ortak bitiş noktasını nasıl yapılandırılabekleyeceğizi öğrenin: [Genel bitiş noktasını yapılandırma](sql-database-managed-instance-public-endpoint-configure.md)
