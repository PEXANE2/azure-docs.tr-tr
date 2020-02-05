---
title: VMware çözümleri (AVS)-Azure ağ bağlantıları
description: Azure Sanal ağınızı AVS bölge ağınıza bağlama hakkında bilgi edinin
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: bf11d4e2676179e8b71d3a03f8ed3cbcb4cfba9d
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025087"
---
# <a name="azure-network-connections-overview"></a>Azure ağ bağlantılarına genel bakış

Bir bölgede bir AVS hizmeti oluşturduğunuzda ve düğümleri oluştururken şunları yapabilirsiniz:

* Bir Azure ExpressRoute bağlantı hattı isteyin ve bu bölgedeki AVS ağına ekleyin.
* Azure ExpressRoute kullanarak AVS bölge ağınızı Azure sanal ağınıza veya şirket içi ağınıza bağlayın.
* Özel bulut ortamınızdan Azure aboneliğinizde veya şirket içi ağınızda çalışan hizmetlere erişim sağlayın.

ExpressRoute bağlantısı, düşük gecikme süresine sahip yüksek bant genişliğidir.

## <a name="benefits"></a>Avantajlar

Azure ağ bağlantısı şunları yapmanıza olanak sağlar:

* Özel bulutunuzda sanal makineler için Azure 'u yedekleme hedefi olarak kullanın.
* Özel bulut vSAN veri deposunu şifrelemek için Azure aboneliğinizde KMS sunucuları dağıtın.
* Uygulama ve veritabanı katmanları özel bulutunuzda çalışırken uygulamanın web katmanının AVS genel bulutunda çalıştırıldığı karma uygulamaları kullanın.

## <a name="azure-virtual-network-connection"></a>Azure sanal ağ bağlantısı

AVS özel bulutları, ExpressRoute kullanarak Azure kaynaklarınıza bağlanabilir. ExpressRoute bağlantısı, AVS özel bulutunuzda Azure aboneliğinizde çalışan kaynaklara erişmenize olanak tanır. Bu bağlantı, AVS özel bulut ağınızı Azure sanal ağınıza genişletmenizi sağlar. Bir AVS ağından yollar BGP üzerinden Azure sanal ağınızla birlikte takas edilir. Sanal ağ eşlemesi yapılandırdıysanız, eşlenen tüm sanal ağlara AVS ağınızdan erişilebilecektir.

![Sanal ağa Azure ExpressRoute bağlantısı](media/cloudsimple-azure-network-connection.png)

## <a name="expressroute-connection-to-on-premises-network"></a>Şirket içi ağa ExpressRoute bağlantısı

Mevcut Azure ExpressRoute devrenizi AVS bölgenize bağlayabilirsiniz. ExpressRoute Global Reach özelliği, iki devreleri birbirleriyle bağlamak için kullanılır. Şirket içi ve AVS ExpressRoute devreleri arasında bir bağlantı oluşturulur. Bu bağlantı, şirket içi ağlarınızı bir AVS özel bulut ağına genişletmenizi sağlar. AVS ağınızdan gelen yollar, şirket içi ağınızla BGP aracılığıyla değiştirilir.

![Şirket içi ExpressRoute bağlantısı-Global Reach](media/cloudsimple-global-reach-connection.png)

## <a name="connection-to-on-premises-network-and-azure-virtual-network"></a>Şirket içi ağ ve Azure sanal ağı bağlantısı

Şirket içi ağ ve Azure sanal ağı bağlantıları, AVS ağınızdan birlikte çalışabilir. Bağlantı, bir şirket içi ağ, Azure sanal ağı ve bir AVS ağı arasındaki yolları Exchange için BGP kullanır. AVS ağınızı bir Global Reach bağlantısı olması durumunda Azure sanal ağınıza bağladığınızda, Azure sanal ağ yolları şirket içi ağınızda görünür olur. Yönlendirme değişimi, Azure 'da Edge yönlendiricileri arasında gerçekleşir.

![Azure sanal ağ bağlantısıyla şirket içi ExpressRoute bağlantısı](media/cloudsimple-global-reach-and-vnet-connection.png)

### <a name="important-considerations"></a>Önemli noktalar

Bir şirket içi ağdan ve/veya Azure sanal ağından bir AVS ağına bağlanmak, Exchange 'e tüm ağlar arasında yönlendirme sağlar.

* Azure sanal ağı, hem şirket içi ağ hem de bir AVS ağı tarafından görülebilir.
* Azure sanal ağınıza şirket içi bir ağdan bağlandıysanız, Global Reach kullanarak bir AVS ağına bağlantı, AVS ağından sanal ağlara erişime izin verir.
* Alt ağ adresleri bağlı ağların hiçbiriyle **çakışmamalıdır** .
* AVS, ExpressRoute bağlantılarına varsayılan bir **yol bildirmeyecektir**
* Şirket içi yönlendiricinizde varsayılan yol bildiriliyorsa, AVS ağından ve Azure sanal ağından gelen trafik tanıtılan varsayılan yolu kullanır. Sonuç olarak, Azure 'daki sanal makinelere genel IP adresleri kullanılarak erişilemez.

## <a name="next-steps"></a>Sonraki adımlar

* [ExpressRoute kullanarak Azure sanal ağını AVS 'ye bağlama](virtual-network-connection.md)
* [ExpressRoute kullanarak şirket içinden AVS 'ye bağlanma](on-premises-connection.md)
