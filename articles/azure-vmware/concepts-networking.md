---
title: Kavramlar-ağ bağlantısı
description: Azure VMware çözümünde önemli yönleri ve ağ ve bağlantı durumlarını kullanma hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 07/23/2020
ms.openlocfilehash: 3420f6aa61ced7632175f3e12edda9de72639517
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "88750572"
---
# <a name="azure-vmware-solution-preview-networking-and-interconnectivity-concepts"></a>Azure VMware Çözüm önizleme ağı ve karşılıklı bağlantı kavramları

Azure VMware çözümü (AVS), şirket içi ve Azure tabanlı ortamların veya kaynakların kullanıcıları ve uygulamaları için erişilebilen bir VMware özel bulut ortamı sunar. Azure ExpressRoute ve VPN bağlantıları gibi hizmetler bağlantı sağlar. Bu hizmetler, hizmetleri etkinleştirmek için belirli ağ adresi aralıkları ve güvenlik duvarı bağlantı noktaları gerektirir.  

Özel bir bulut dağıtımında, yönetim, sağlama ve vMotion için özel ağlar oluşturulur. Bunlar vCenter ve NSX-T Yöneticisi ve sanal makine vMotion veya dağıtımına erişim için kullanılır. Tüm özel ağlara Azure 'daki VNet 'ten veya şirket içi ortamlarından erişilebilir. ExpressRoute Global Reach, özel bulutları şirket içi ortamlara bağlamak için kullanılır ve bu bağlantı aboneliğinizde bir ExpressRoute devresine sahip VNet gerektirir.

Ayrıca, özel bir bulut dağıtımında Internet ve Azure hizmetlerine erişim sağlanır ve üretim ağlarında VM 'Lerin bunları kullanabilmesi için sağlanır.  Varsayılan olarak, internet erişimi yeni özel bulutlar için devre dışıdır ve herhangi bir zamanda etkinleştirilebilir veya devre dışı bırakılabilir.

Karşılıklı bağlantı üzerinde yararlı bir perspektif, iki tür AVS özel bulut uygulaması olarak dikkate alınmaktadır:

1. [**Temel Azure-yalnızca karşılıklı bağlantı**](#azure-virtual-network-interconnectivity) , özel bulutunuzu yalnızca Azure 'daki tek bir sanal ağla yönetmenizi ve kullanmanızı sağlar. Bu uygulama, şirket içi ortamlarından erişim gerektirmeyen AVS değerlendirmeleri veya uygulamaları için idealdir.

1. Şirket içinde [**tam ve özel bulut bağlantısı**](#on-premises-interconnectivity) , şirket ıçı ve AVS özel bulutları arasında birbirine bağlantı dahil olmak üzere yalnızca temel Azure uygulamasını genişletir.
 
Bu makalede, gereksinimler ve sınırlamalar dahil olmak üzere ağ ve bağlantı kurmaya yönelik birkaç temel kavram ele alınacaktır. Ayrıca, iki tür AVS özel bulut bağlantısı uygulaması ile daha fazla bilgi ele alınacaktır. Bu makale, ağınızı AVS ile düzgün çalışacak şekilde yapılandırmak için bilmeniz gereken bilgileri sağlar.

## <a name="avs-private-cloud-use-cases"></a>AVS özel bulut kullanım örnekleri

AVS özel bulutları için kullanım örnekleri şunları içerir:
- Buluttaki yeni VMware VM iş yükleri
- VM iş yükü buluta gömülmüş (yalnızca şirket içi-AVS 'ye)
- Buluta VM iş yükü geçişi (yalnızca şirket içi-AVS 'ye)
- Olağanüstü durum kurtarma (AVS 'ye veya Şirket içinden AVS 'ye AVS)
- Azure hizmetleri kullanımı

> [!TIP]
> AVS hizmeti için tüm kullanım örnekleri, şirket içi ve özel bulut bağlantısı ile etkinleştirilir.

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

Özel bulutunuzun tam bağlantısı için, ExpressRoute Global Reach etkinleştirin ve sonra Azure portal Global Reach için bir yetkilendirme anahtarı ve özel eşleme KIMLIĞI isteyin. Yetkilendirme anahtarı ve eşleme KIMLIĞI, aboneliğinizdeki bir ExpressRoute bağlantı hattı ile yeni özel bulutunuz için ExpressRoute bağlantı hattı arasında Global Reach oluşturmak için kullanılır. Bağlantı kurulduktan sonra, iki ExpressRoute devre dışı, şirket içi ortamlarınızla özel bulutunuz arasında ağ trafiğini yönlendirir.  Yetkilendirme anahtarını ve eşleme KIMLIĞINI isteme ve kullanma yordamlarına yönelik bir [özel buluta eşleme Global Reach için bir ExpressRoute oluşturma öğreticisine](tutorial-expressroute-global-reach-private-cloud.md) bakın.

## <a name="next-steps"></a>Sonraki adımlar 

- [Ağ bağlantısı konuları ve gereksinimleri](tutorial-network-checklist.md)hakkında daha fazla bilgi edinin. 
- [Özel bulut depolama kavramları](concepts-storage.md)hakkında bilgi edinin.


<!-- LINKS - external -->
[enable Global Reach]: ../expressroute/expressroute-howto-set-global-reach.md

<!-- LINKS - internal -->

