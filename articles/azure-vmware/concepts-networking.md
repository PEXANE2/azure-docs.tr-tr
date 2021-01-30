---
title: Kavramlar-ağ bağlantısı
description: Azure VMware çözümünde önemli yönleri ve ağ ve bağlantı durumlarını kullanma hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 09/21/2020
ms.openlocfilehash: 9369c0fff31de4c6575d8e4903420cfa01461bf4
ms.sourcegitcommit: 1a98b3f91663484920a747d75500f6d70a6cb2ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99062129"
---
# <a name="azure-vmware-solution-networking-and-interconnectivity-concepts"></a>Azure VMware Çözüm ağı ve karşılıklı bağlantı kavramları

[!INCLUDE [avs-networking-description](includes/azure-vmware-solution-networking-description.md)]

Karşılıklı bağlantı üzerinde yararlı bir bakış, Azure VMware çözümü özel bulut uygulamalarının iki türünü göz önünde bulundurabilmelidir:

1. [**Temel Azure-yalnızca karşılıklı bağlantı**](#azure-virtual-network-interconnectivity) , özel bulutunuzu yalnızca Azure 'daki tek bir sanal ağla yönetmenizi ve kullanmanızı sağlar. Bu uygulama, Azure VMware Çözüm değerlendirmeleri veya şirket içi ortamlarından erişim gerektirmeyen uygulamalar için idealdir.

1. Şirket içinde [**tam ve özel bulut bağlantısı**](#on-premises-interconnectivity) , şirket Içi ve Azure VMware çözümü özel bulutları arasında karşılıklı bağlantı dahil olmak üzere yalnızca temel Azure uygulamasını genişletir.
 
Bu makalede, gereksinimler ve sınırlamalar dahil olmak üzere ağ ve bağlantı kurmaya yönelik birkaç temel kavram ele alınacaktır. Ayrıca, Azure VMware çözümü özel bulut bağlantısı uygulamalarının iki türü de daha fazla bilgi ele alınacaktır. Bu makale, ağınızı Azure VMware çözümüyle düzgün şekilde çalışacak şekilde yapılandırmak için bilmeniz gereken bilgileri sağlar.

## <a name="azure-vmware-solution-private-cloud-use-cases"></a>Azure VMware çözümü özel bulut kullanım örnekleri

Azure VMware çözümü özel bulutları için kullanım örnekleri şunları içerir:
- Buluttaki yeni VMware VM iş yükleri
- VM iş yükü buluta gömülmüş (yalnızca şirket içi Azure VMware çözümüne)
- VM iş yükü buluta geçiş (yalnızca şirket içi Azure VMware çözümüne)
- Olağanüstü durum kurtarma (Azure VMware çözümüne Azure VMware çözümü veya şirket içi Azure VMware çözümüne)
- Azure hizmetleri kullanımı

> [!TIP]
> Azure VMware çözüm hizmeti için tüm kullanım örnekleri, şirket içi ve özel bulut bağlantısı ile etkinleştirilir.

## <a name="azure-virtual-network-interconnectivity"></a>Azure sanal ağ bağlantısı

Sanal ağda özel bulut uygulamasına, Azure VMware çözümünüz özel bulutunuzu yönetebilir, özel bulutunuzda iş yüklerini tüketebilir ve ExpressRoute bağlantısı üzerinden Azure hizmetlerine erişebilirsiniz. 

Aşağıdaki diyagramda, özel bir bulut dağıtımı sırasında belirlenen temel ağ bağlantısı gösterilmektedir. Azure 'daki bir sanal ağ ile özel bir bulut arasındaki mantıksal, ExpressRoute tabanlı ağı gösterir. Interconnectivity, birincil kullanım örneklerinin üçünü yerine getirir:
* Azure aboneliğinizdeki VM 'lerden erişilebilen ve şirket içi sistemlerinizden değil, vCenter Server ve NSX-T yöneticisine gelen erişim. 
* VM 'lerden Azure hizmetlerine giden erişim. 
* Özel bir bulut çalıştıran iş yüklerinin gelen erişimi ve tüketimi.

:::image type="content" source="media/concepts/adjacency-overview-drawing-single.png" alt-text="Temel sanal ağdan özel bulut bağlantısına" border="false":::

## <a name="on-premises-interconnectivity"></a>Şirket içi karşılıklı bağlantı

Sanal ağda ve şirket içinde tam özel bulut uygulamasına, şirket içi ortamlarınızdaki Azure VMware çözümünüz özel bulutlarınıza erişebilirsiniz. Bu uygulama, önceki bölümde açıklanan temel uygulamanın bir uzantısıdır. Temel uygulama gibi, bir ExpressRoute bağlantı hattı gerekir, ancak bu uygulamayla birlikte Şirket içi ortamlarından Azure 'daki özel buluta bağlanmak için kullanılır. 

Aşağıdaki diyagramda, şirket içi ile özel bulut bağlantısı, aşağıdaki kullanım durumlarını sağlar:
* Etkin/soğuk çapraz vCenter vMotion
* Şirket Içinden Azure VMware çözümüne özel bulut yönetimi erişimi

:::image type="content" source="media/concepts/adjacency-overview-drawing-double.png" alt-text="Sanal ağ ve şirket içi tam özel bulut bağlantısı" border="false":::

Özel bulutunuzun tam bağlantısı için, ExpressRoute Global Reach etkinleştirin ve sonra Azure portal Global Reach için bir yetkilendirme anahtarı ve özel eşleme KIMLIĞI isteyin. Yetkilendirme anahtarı ve eşleme KIMLIĞI, aboneliğinizdeki bir ExpressRoute bağlantı hattı ile yeni özel bulutunuz için ExpressRoute bağlantı hattı arasında Global Reach oluşturmak için kullanılır. Bağlantı kurulduktan sonra, iki ExpressRoute devre dışı, şirket içi ortamlarınızla özel bulutunuz arasında ağ trafiğini yönlendirir.  Yetkilendirme anahtarını ve eşleme KIMLIĞINI isteme ve kullanma yordamları hakkında daha fazla bilgi için, bir [özel buluta eşleme Global Reach ExpressRoute oluşturma öğreticisine](tutorial-expressroute-global-reach-private-cloud.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar 
[Özel bulut depolama kavramları](concepts-storage.md)hakkında bilgi edinin.


<!-- LINKS - external -->
[enable Global Reach]: ../expressroute/expressroute-howto-set-global-reach.md

<!-- LINKS - internal -->

