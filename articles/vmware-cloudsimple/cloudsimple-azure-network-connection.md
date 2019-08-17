---
title: CloudSimple-Azure ağ bağlantıları tarafından VMware çözümü
description: Azure Sanal ağınızı CloudSimple bölgesi ağınıza bağlama hakkında bilgi edinin
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: eca3e316d866814f6727dd8ef2c3fa490a551383
ms.sourcegitcommit: 39d95a11d5937364ca0b01d8ba099752c4128827
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69563170"
---
# <a name="azure-network-connections-overview"></a>Azure ağ bağlantılarına genel bakış

Bir bölgede CloudSimple hizmeti oluşturduğunuzda, şunları yapabilirsiniz:

* Bir Azure ExpressRoute bağlantı hattı oluşturur ve bu bölgedeki hizmete iliştirir.
* Azure ExpressRoute kullanarak CloudSimple bölge ağınızı Azure sanal ağınıza veya şirket içi ağınıza bağlar.
* Özel bulut ortamınızdan Azure aboneliğinizde veya şirket içi ağınızda çalışan hizmetlere erişim sağlar.

ExpressRoute bağlantısı, düşük gecikme süresine sahip yüksek bant genişliğidir.

## <a name="benefits"></a>Avantajlar

Azure ağ bağlantısı şunları yapmanıza olanak sağlar:

* Özel bulutunuzda sanal makineler için Azure 'u yedekleme hedefi olarak kullanın.
* Özel bulut vSAN veri deposunu şifrelemek için Azure aboneliğinizde KMS sunucuları dağıtın.
* Uygulama ve veritabanı katmanları özel bulutunuzda çalışırken uygulamanın web katmanının genel bulutta çalıştığı karma uygulamaları kullanın.

## <a name="azure-virtual-network-connection"></a>Azure sanal ağ bağlantısı

Özel bulutlar, ExpressRoute kullanarak Azure kaynaklarınıza bağlanabilir.  ExpressRoute bağlantısı, özel bulutunuzda Azure aboneliğinizde çalışan kaynaklara erişmenizi sağlar.  Bu bağlantı, özel bulut ağınızı Azure sanal ağınıza genişletmenizi sağlar.

[![Sanal ağa Azure ExpressRoute bağlantısı](media/cloudsimple-azure-network-connection.png)

## <a name="expressroute-connection-to-on-premises-network"></a>Şirket içi ağa ExpressRoute bağlantısı

Mevcut Azure ExpressRoute devrenizi CloudSimple bölgenize bağlayabilirsiniz. ExpressRoute Global Reach özelliği, iki devreleri birbirleriyle bağlamak için kullanılır.  Şirket içi ve CloudSimple ExpressRoute devreleri arasında bir bağlantı oluşturulur.  Bu bağlantı, şirket içi ağlarınızı özel bulut ağına genişletmenizi sağlar.

![Şirket içi ExpressRoute bağlantısı-Global Reach](media/cloudsimple-global-reach-connection.png)

## <a name="next-steps"></a>Sonraki adımlar

* [ExpressRoute kullanarak Azure sanal ağını CloudSimple 'a bağlama](virtual-network-connection.md)
* [ExpressRoute kullanarak şirket içinden CloudSimple 'a bağlanma](on-premises-connection.md)
