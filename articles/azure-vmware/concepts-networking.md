---
title: Kavramlar-ağ bağlantısı
description: Azure VMware çözümünde (AVS), ağ ve bağlantı durumları hakkında bilgi edinin
ms.topic: conceptual
ms.date: 07/23/2020
ms.openlocfilehash: c0416da9c745ccf92970ff39f623a782d5784983
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87062848"
---
# <a name="azure-vmware-solution-avs-preview-networking-and-interconnectivity-concepts"></a>Azure VMware çözümü (AVS) önizleme ağı ve karşılıklı bağlantı kavramları

Azure VMware çözümünüz (AVS) özel bulutları ve şirket içi ortamlar ya da Azure 'daki sanal ağlar arasındaki ağ bağlantısı, özel bulutunuzun erişimine ve kullanılmasına olanak tanır. Bu makalede, ağ ve karşılıklı bağlantı temelini oluşturan bazı önemli kavramlar ele alınacaktır.

Karşılıklı bağlantı üzerinde yararlı bir perspektif, iki tür AVS özel bulut uygulaması olarak dikkate alınmaktadır:

1. [**Temel Azure-yalnızca karşılıklı bağlantı**](#azure-virtual-network-interconnectivity) , özel bulutunuzu yalnızca Azure 'daki tek bir sanal ağla yönetmenizi ve kullanmanızı sağlar. Bu uygulama, şirket içi ortamlarından erişim gerektirmeyen AVS değerlendirmeleri veya uygulamaları için idealdir.

1. Şirket içinde [**tam ve özel bulut bağlantısı**](#on-premises-interconnectivity) , şirket ıçı ve AVS özel bulutları arasında birbirine bağlantı dahil olmak üzere yalnızca temel Azure uygulamasını genişletir.
 
Gereksinimler ve aşağıdaki bölümlerde açıklanan iki tür AVS özel bulut bağlantısı uygulaması hakkında daha fazla bilgi edinebilirsiniz.

## <a name="avs-private-cloud-use-cases"></a>AVS özel bulut kullanım örnekleri

AVS özel bulutları için kullanım örnekleri şunları içerir:
- buluttaki yeni VMware VM iş yükleri
- VM iş yükü buluta gömülmüş (yalnızca şirket içi-AVS 'ye)
- Buluta VM iş yükü geçişi (yalnızca şirket içi-AVS 'ye)
- Olağanüstü durum kurtarma (AVS 'ye veya Şirket içinden AVS 'ye AVS)
- Azure hizmetleri kullanımı

 AVS hizmeti için tüm kullanım örnekleri, şirket içi ve özel bulut bağlantısı ile etkinleştirilir. 

## <a name="virtual-network-and-expressroute-circuit-requirements"></a>Sanal ağ ve ExpressRoute devre gereksinimleri
 
Aboneliğinizdeki bir sanal ağdan bağlantı oluşturduğunuzda, ExpressRoute bağlantı hattı eşleme yoluyla oluşturulur, bir yetkilendirme anahtarı ve Azure portal isteğiniz bir eşleme KIMLIĞI kullanır. Eşleme, özel bulutunuz ile sanal ağ arasında özel, bire bir bağlantıdır.

> [!NOTE] 
> ExpressRoute bağlantı hattı, özel bir bulut dağıtımının bir parçası değildir. Şirket içi ExpressRoute bağlantı hattı, bu belgenin kapsamı dışındadır. Özel bulutunuzun şirket içi bağlantısına ihtiyacınız varsa, mevcut ExpressRoute devrelerinizi kullanabilir veya Azure portal bir tane satın alabilirsiniz.

Özel bir bulut dağıtımında, vCenter ve NSX-T Yöneticisi için IP adresleri alırsınız. Bu yönetim arabirimlerine erişmek için aboneliğinizdeki bir sanal ağda ek kaynaklar oluşturmanız gerekir. Bu kaynakları oluşturma ve öğreticilerde ExpressRoute özel eşlemesi oluşturma yordamlarını bulabilirsiniz.

Özel bulut mantıksal ağı, önceden sağlanmış NSX-T ile gelir. Katman 0 ağ geçidi ve Katman 1 ağ geçidi sizin için önceden sağlanmış. Bir segment oluşturabilir ve var olan katman 1 ağ geçidine iliştirebilir veya tanımladığınız yeni bir katman 1 ağ geçidine iliştirebilirsiniz. NSX-T mantıksal ağ bileşenleri, iş yükleri arasında Doğu Batı bağlantısı sağlar ve ayrıca Internet ve Azure hizmetlerine yönelik Kuzey-Güney bağlantısını sağlar. 

## <a name="routing-and-subnet-requirements"></a>Yönlendirme ve alt ağ gereksinimleri

Yönlendirme, her özel bulut dağıtımı için otomatik olarak sağlanan ve varsayılan olarak etkinleştirilen Sınır Ağ Geçidi Protokolü (BGP) tabanlıdır. AVS özel bulutları için, aşağıdaki tabloda gösterildiği gibi, alt ağlar için en az/22 önek uzunluğu CıDR ağ adres blokları ile özel bulut ağ adres alanları planlamanız gerekir. Adres bloğu, aboneliğinizdeki ve şirket içi ağlardaki diğer sanal ağlarda kullanılan adres bloklarıyla çakışmamalıdır. Bu adres bloğu, yönetim, sağlama ve vMotion ağları içinde otomatik olarak sağlanır.

Örnek `/22` CIDR ağ adresi bloğu:`10.10.0.0/22`

Alt ağlar:

| Ağ kullanımı             | Alt ağ | Örnek        |
| ------------------------- | ------ | -------------- |
| Özel bulut yönetimi  | `/24`  | `10.10.0.0/24` |
| vMotion ağı           | `/24`  | `10.10.1.0/24` |
| VM iş yükleri              | `/24`  | `10.10.2.0/24` |
| ExpressRoute eşlemesi      | `/24`  | `10.10.3.8/30` |


## <a name="azure-virtual-network-interconnectivity"></a>Azure sanal ağ bağlantısı

Sanal ağda özel bulut uygulamasına, AVS özel bulutunuzu yönetebilir, özel bulutunuzda iş yüklerini tüketebilir ve ExpressRoute bağlantısı üzerinden Azure hizmetlerine erişebilirsiniz. 

Aşağıdaki diyagramda, özel bir bulut dağıtımı sırasında belirlenen temel ağ bağlantısı gösterilmektedir. Azure 'daki bir sanal ağ ile özel bir bulut arasındaki mantıksal, ExpressRoute tabanlı ağı gösterir. Interconnectivity, birincil kullanım örneklerinin üçünü yerine getirir:
* Azure aboneliğinizdeki VM 'lerden erişilebilen ve şirket içi sistemlerinizden değil, vCenter Server ve NSX-T yöneticisine gelen erişim. 
* VM 'lerden Azure hizmetlerine giden erişim. 
* Özel bir bulut çalıştıran iş yüklerinin gelen erişimi ve tüketimi.

:::image type="content" source="media/concepts/adjacency-overview-drawing-single.png" alt-text="Temel sanal ağdan özel bulut bağlantısına" border="false":::

## <a name="on-premises-interconnectivity"></a>Şirket içi karşılıklı bağlantı

Sanal ağda ve şirket içinde tam özel bulut uygulamasına, şirket içi ortamlarınızdaki AVS özel bulutlarınıza erişebilirsiniz. Bu uygulama, önceki bölümde açıklanan temel uygulamanın bir uzantısıdır. Temel uygulama gibi, bir ExpressRoute bağlantı hattı gerekir, ancak bu uygulamayla birlikte Şirket içi ortamlarından Azure 'daki özel buluta bağlanmak için kullanılır. 

Aşağıdaki diyagramda, şirket içi ile özel bulut bağlantısı, aşağıdaki kullanım durumlarını sağlar:
* Etkin/soğuk çapraz vCenter vMotion
* Şirket içinden AVS 'ye özel bulut yönetimi erişimi

:::image type="content" source="media/concepts/adjacency-overview-drawing-double.png" alt-text="Sanal ağ ve şirket içi tam özel bulut bağlantısı" border="false":::

Özel bulutunuzun tam bağlantısı için, ExpressRoute Global Reach etkinleştirin ve sonra Azure portal Global Reach için bir yetkilendirme anahtarı ve özel eşleme KIMLIĞI isteyin. Yetkilendirme anahtarı ve eşleme KIMLIĞI, aboneliğinizdeki bir ExpressRoute bağlantı hattı ile yeni özel bulutunuz için ExpressRoute bağlantı hattı arasında Global Reach oluşturmak için kullanılır. Bağlantı kurulduktan sonra, iki ExpressRoute devre dışı, şirket içi ortamlarınızla özel bulutunuz arasında ağ trafiğini yönlendirir.  Yetkilendirme anahtarını ve eşleme KIMLIĞINI isteme ve kullanma yordamlarına yönelik bir [özel buluta eşleme Global Reach için bir ExpressRoute oluşturma öğreticisine](tutorial-expressroute-global-reach-private-cloud.md) bakın.


## <a name="next-steps"></a>Sonraki adımlar 

Bir sonraki adım, [özel bulut depolaması kavramlarını](concepts-storage.md)öğrenecekti.

<!-- LINKS - external -->
[enable Global Reach]: ../expressroute/expressroute-howto-set-global-reach.md

<!-- LINKS - internal -->
