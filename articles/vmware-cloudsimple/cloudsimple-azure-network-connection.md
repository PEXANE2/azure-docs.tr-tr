---
title: CloudSimple tarafından VMware Çözümü - Azure ağ bağlantıları
description: Azure sanal ağınızı CloudSimple bölge ağınıza bağlama hakkında bilgi edinin
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: cfd4d65b07cf255ac2b60d6bf8376723a997374e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77025087"
---
# <a name="azure-network-connections-overview"></a>Azure ağ bağlantılarına genel bakış

Bir bölgede bir CloudSimple hizmeti oluşturduğunuzda ve düğümler oluşturduğunuzda şunları yapabilirsiniz:

* Bir Azure ExpressRoute devresi isteyin ve bu bölgedeki CloudSimple ağına takın.
* CloudSimple bölge ağınızı Azure Sanal Ağınıza veya Azure ExpressRoute'u kullanarak şirket içi ağınıza bağlayın.
* Azure aboneliğinizde veya şirket içi ağınızda çalışan hizmetlere Özel Bulut ortamınızdan erişim sağlayın.

ExpressRoute bağlantısı düşük gecikme gecikmesi ile yüksek bant genişliğine sahiptir.

## <a name="benefits"></a>Avantajlar

Azure ağ bağlantısı şunları yapmanızı sağlar:

* Azure'u Özel Bulut'unuzdaki sanal makineler için yedek hedef olarak kullanın.
* Özel Bulut vSAN veri mağazanızı şifrelemek için Azure aboneliğinizde KMS sunucularını dağıtın.
* Uygulama ve veritabanı katmanları Özel Bulut'unuzda çalışırken uygulamanın web katmanının genel bulutta çalıştığı karma uygulamaları kullanın.

## <a name="azure-virtual-network-connection"></a>Azure sanal ağ bağlantısı

Özel Bulutlar, ExpressRoute kullanarak Azure kaynaklarınıza bağlanabilir.  ExpressRoute bağlantısı, Azure aboneliğinizde çalışan kaynaklara Özel Bulut'unuzdan erişmenizi sağlar.  Bu bağlantı, Özel Bulut ağınızı Azure sanal ağınıza genişletmenize olanak tanır.  CloudSimple ağındaki rotalar, BGP üzerinden Azure sanal ağınızla değiştirilir.  Sanal ağ eşleme yapılandırıldıysanız, tüm bakan sanal ağlara CloudSimple ağınızdan erişilebilir.

![Azure ExpressRoute Sanal Ağa Bağlantı](media/cloudsimple-azure-network-connection.png)

## <a name="expressroute-connection-to-on-premises-network"></a>Şirket içi ağa ExpressRoute bağlantısı

Mevcut Azure ExpressRoute devrenizi CloudSimple bölgenize bağlayabilirsiniz. ExpressRoute Global Reach özelliği, iki devreyi birbirine bağlamak için kullanılır.  Şirket içi ve CloudSimple ExpressRoute devreleri arasında bir bağlantı kurulur.  Bu bağlantı, şirket içi ağlarınızı Özel Bulut ağına genişletmenize olanak tanır. CloudSimple ağınızdaki rotalar BGP üzerinden şirket içi ağınızla değiştirilir.

![Şirket Içi ExpressRoute Bağlantısı - Global Reach](media/cloudsimple-global-reach-connection.png)

## <a name="connection-to-on-premises-network-and-azure-virtual-network"></a>Şirket içi ağa ve Azure sanal ağına bağlantı

Şirket içi ağa ve Azure sanal ağına bağlantılar CloudSimple aağınızdan bir arada bulunabilir.  Bağlantı, şirket içi ağ, Azure sanal ağı ve CloudSimple ağı arasında yol alışverişi yapmak için BGP kullanır.  Global Erişim bağlantısı yla CloudSimple ağınızı Azure sanal ağınıza bağladığınızda, Azure sanal ağ yolları şirket içi ağınızda görünür.  Yol değişimi, kenar yönlendiricileri arasında Azure'da gerçekleşir.

![Azure sanal ağ bağlantısı ile şirket içi ExpressRoute Bağlantısı](media/cloudsimple-global-reach-and-vnet-connection.png)

### <a name="important-considerations"></a>Önemli noktalar

CloudSimple ağına şirket içi ağdan ve Azure sanal ağından bağlanmak tüm ağlar arasında rota alışverişine olanak tanır.

* Azure sanal ağı hem şirket içi ağdan hem de CloudSimple ağdan görünür.
* Azure sanal ağınıza şirket içi ağınızdan bağlandıysanız, Global Reach'i kullanarak CloudSimple ağına bağlantı, CloudSimple ağından sanal ağlara erişime olanak tanır.
* Alt ağ adresleri bağlı ağların hiçbiri arasında **çakışmamalıdır.**
* CloudSimple, ExpressRoute bağlantılarına varsayılan rotanın reklamını **yapmaz**
* Şirket içi yönlendiriciniz varsayılan rotanın reklamını yaparsa, CloudSimple ağındaki trafik ve Azure sanal ağı, reklamı yapılan varsayılan rotayı kullanır.  Sonuç olarak, Azure'daki sanal makinelere ortak IP adresleri kullanılarak erişilemez.

## <a name="next-steps"></a>Sonraki adımlar

* [ExpressRoute kullanarak Azure sanal ağını CloudSimple'a bağlayın](virtual-network-connection.md)
* [ExpressRoute'u kullanarak şirket içinde CloudSimple'a bağlanın](on-premises-connection.md)
