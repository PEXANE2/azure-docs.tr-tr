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
ms.openlocfilehash: a8e99da05f71cb01744111b41c301b11a0969057
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812718"
---
# <a name="azure-network-connections-overview"></a>Azure ağ bağlantılarına genel bakış

Bir bölgede CloudSimple hizmeti oluşturduğunuzda, şunları yapabilirsiniz:

* Bir Azure ExpressRoute bağlantı hattı oluşturur ve bu bölgedeki hizmete iliştirir
* Azure ExpressRoute kullanarak CloudSimple bölge ağınızdan Azure sanal ağınıza veya şirket içi ağınıza bağlantı sağlar
* Özel bulut ortamınızdan Azure aboneliğinizde veya şirket içi ağınızda çalışan erişim hizmetleri sağlar

Bağlantı:

* Güvenlik
* Özel
* Yüksek bant genişliği
* Düşük gecikme süresi

## <a name="benefits"></a>Avantajlar

Azure ağ bağlantısı şunları yapmanıza olanak sağlar:

* Özel bulutunuzda sanal makineler için Azure 'u yedekleme hedefi olarak kullanın.
* Özel bulut vSAN veri deposunu şifrelemek için Azure aboneliğinizde KMS sunucuları dağıtın.
* Uygulama ve veritabanı katmanları özel bulutunuzda çalışırken uygulamanın web katmanının genel bulutta çalıştığı karma uygulamaları kullanın.

## <a name="azure-virtual-network-connection"></a>Azure sanal ağ bağlantısı

Özel bulutlar, ExpressRoute kullanarak Azure kaynaklarınıza bağlanabilir.  Bu bağlantıyı, özel bulutunuzda Azure aboneliğinizde çalışan farklı kaynaklara erişmek için kullanabilirsiniz.  Bu bağlantı, özel bulut ağınızı Azure sanal ağınıza genişletmenizi sağlar.

![Sanal ağa Azure ExpressRoute bağlantısı](media/cloudsimple-azure-network-connection.png)

## <a name="expressroute-connection-to-on-premises-network"></a>Şirket içi ağa ExpressRoute bağlantısı

Mevcut Azure ExpressRoute devrenizi CloudSimple bölgenize bağlayabilirsiniz. ExpressRoute Global Reach özelliği, iki devreleri birbirleriyle bağlamak için kullanılır.  Şirket içi ve CloudSimple ExpressRoute devreleri arasında bir bağlantı oluşturulur.  Bu bağlantı, şirket içi ağlarınızı özel bulut ağına genişletmenizi sağlar.

![Şirket içi ExpressRoute bağlantısı-Global Reach](media/cloudsimple-global-reach-connection.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure sanal ağı ile CloudSimple bağlantısı arasındaki eşleme bilgilerini alın](https://docs.azure.cloudsimple.com/virtual-network-connection)
* [ExpressRoute kullanarak şirket içinden CloudSimple 'a bağlanma](https://docs.azure.cloudsimple.com/on-premises-connection)
