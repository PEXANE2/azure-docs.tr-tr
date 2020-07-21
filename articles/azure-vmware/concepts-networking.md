---
title: Kavramlar-ağ bağlantısı
description: Azure VMware çözümünde (AVS), ağ ve bağlantı durumları hakkında bilgi edinin
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: 2378ad56e2754b2d2fde7f895f6673e7d7d561c9
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86539151"
---
# <a name="azure-vmware-solution-avs-preview-networking-and-interconnectivity-concepts"></a>Azure VMware çözümü (AVS) önizleme ağı ve karşılıklı bağlantı kavramları

Azure VMware çözümünüz (AVS) özel bulutları ve şirket içi ortamlar ya da Azure 'daki sanal ağlar arasındaki ağ bağlantısı, özel bulutunuzun erişimine ve kullanılmasına olanak sağlar. Bu makalede, karşılıklı bağlantının temelini oluşturan birkaç anahtar ağ ve birbirine bağlı kavramlar açıklanmaktadır.

Birbirine bağlı bir perspektifle, iki tür AVS özel bulut uygulaması vardır: temel Azure ile şirket içi uygulamalar ve yalnızca şirket içinde tam olarak şirket içi ve özel bulut bağlantılarını içeren uygulamalar.

AVS özel bulutları için kullanım örnekleri şunları içerir:
- buluttaki yeni VMware VM iş yükleri
- VM iş yükü buluta gömülmüş
- VM iş yükünü buluta geçirme
- Olağanüstü durum kurtarma
- Azure hizmetleri kullanımı

 AVS hizmeti için tüm kullanım örnekleri, şirket içi ve özel bulut bağlantısı ile etkinleştirilir. Temel ınterbağlantı modeli, AVS değerlendirmeleri veya şirket içi ortamlarından erişim gerektirmeyen uygulamalar için idealdir.

İki tür AVS özel bulut bağlantısı aşağıdaki bölümlerde açıklanmıştır.  En temel karşılıklı bağlantı "Azure sanal ağ bağlantısı" ' dır. Azure 'da yalnızca tek bir sanal ağ ile özel bulutunuzu yönetmenizi ve kullanmanızı sağlar. "Şirket içi bağlantı" bölümünde açıklanan bağlantı, sanal ağ bağlantısını, şirket içi ortamlar ve AVS özel bulutları arasında birbirine bağlı olarak da kapsayacak şekilde genişletir.

## <a name="azure-virtual-network-interconnectivity"></a>Azure sanal ağ bağlantısı

Özel bir bulut dağıtımı sırasında belirlenen temel ağ bağlantılarını aşağıdaki diyagramda gösterilir. Azure 'daki bir sanal ağ ile özel bir bulut arasındaki mantıksal, ExpressRoute tabanlı ağı gösterir. Interconnectivity, birincil kullanım örneklerinin üçünü yerine getirir:
- VCenter Server ve NSX-T Yöneticisi 'nin bulunduğu yönetim ağlarına gelen erişim.
    - Azure aboneliğinizdeki VM 'lerden erişilebilen, şirket içi sistemlerinizden değil.
- VM 'lerden Azure hizmetlerine giden erişim.
- Özel bir bulut çalıştıran iş yüklerinin gelen erişimi ve tüketimi.

![Temel sanal ağdan özel bulut bağlantısı](./media/concepts/adjacency-overview-drawing-single.png)

Bu sanal ağdaki ExpressRoute bağlantı hattı, aboneliğinizdeki bir sanal ağdan özel bulutunuzun ExpressRoute devresine bir bağlantı oluşturduğunuzda oluşturulur. Eşleme, Azure portal istekte bulunan bir yetkilendirme anahtarını ve devre KIMLIĞINI kullanır. Eşleme aracılığıyla kurulan ExpressRoute bağlantısı, özel bulutunuz ile sanal ağ arasında özel, bire bir bağlantıdır. Özel bulutunuzu yönetebilir, özel bulutunuzda iş yüklerini tüketebilir ve Azure hizmetlerine bu ExpressRoute bağlantısı üzerinden erişebilirsiniz.

Bir AVS özel bulutu dağıttığınızda tek bir/22 özel ağ adresi alanı gerekir. Bu adres alanı, aboneliğinizdeki diğer sanal ağlarda kullanılan adres alanları ile çakışmamalıdır. Bu adres alanı içindeki yönetim, sağlama ve vMotion ağları otomatik olarak sağlanır. Yönlendirme BGP tabanlıdır ve her özel bulut dağıtımı için otomatik olarak sağlanır ve varsayılan olarak etkinleştirilir.

Özel bir bulut dağıtıldığında, vCenter ve NSX-T Yöneticisi için IP adresleri sunulur. Bu yönetim arabirimlerine erişmek için aboneliğinizdeki bir sanal ağda ek kaynaklar oluşturacaksınız. Bu kaynakları oluşturma ve ExpressRoute özel eşlemesi oluşturma yordamları öğreticilerde sunulmaktadır.

Özel bulut mantıksal ağını tasarlayabilmeniz ve NSX-T ile uygulamanız gerekir. Özel bulut, önceden sağlanmış NSX-T ile gelir. Katman 1 ağ geçidi & katman-1 ağ geçidi sizin için önceden sağlanmış. Bir segment oluşturabilir ve bunu var olan katman 1 ağ geçidine ekleyebilir ya da tanımlayabilmeniz için yeni bir katman 1 ağ geçidine iliştirebilirsiniz. NSX-T mantıksal ağ bileşenleri, iş yükleri arasında Doğu Batı bağlantısı sağlar ve ayrıca Internet ve Azure hizmetlerine yönelik Kuzey-Güney bağlantısını sağlar. 

## <a name="on-premises-interconnectivity"></a>Şirket içi karşılıklı bağlantı

Ayrıca, şirket içi ortamları AVS özel bulutlarınıza da bağlayabilirsiniz. Bu tür bir bağlantı, önceki bölümde açıklanan temel bağlantılar için bir uzantıdır.

![sanal ağ ve şirket içi tam özel bulut bağlantısı](./media/concepts/adjacency-overview-drawing-double.png)

Özel bir buluta tam bağlantı kurmak için, bir özel bulut ExpressRoute bağlantı hattı ve şirket içi ExpressRoute bağlantı hattı arasında ExpressRoute Global Reach etkinleştirmek üzere Azure portal kullanırsınız. Bu yapılandırma, şirket içi ortamlarından özel bulutlara erişim dahil etmek için temel bağlantıyı genişletir.

Şirket içi ortamlarından Azure 'da özel buluta bağlanmak için Azure Virtual Network ExpressRoute devresi gerekir. Bu ExpressRoute bağlantı hattı aboneliğinizdeki ve özel bulut dağıtımının bir parçası değil. Şirket içi ExpressRoute bağlantı hattı, bu belgenin kapsamı dışındadır. Özel bulutunuzun şirket içi bağlantısına ihtiyacınız varsa, mevcut ExpressRoute devrelerinizi kullanabilir veya Azure portal bir tane satın alabilirsiniz.

Global Reach ile bağlandıktan sonra, iki ExpressRoute bağlantı hattı, şirket içi ortamlarınız ve özel bulutunuz arasında ağ trafiğini yönlendirmeyecektir. Şirket içinden özel bulut bağlantısı, önceki diyagramda gösterilmiştir. Diyagramda temsil edilen bağlantılar aşağıdaki kullanım örneklerini sunar:

- Etkin/soğuk çapraz vCenter vMotion
- Şirket Içinden AVS 'ye özel bulut yönetimi erişimi

Tam bağlantıyı etkinleştirmek için, Azure portal Global Reach bir yetkilendirme anahtarı ve özel eşleme KIMLIĞI istenebilir. Aboneliğinizde bir ExpressRoute bağlantı hattı ve yeni özel bulutunuz için ExpressRoute bağlantı hattı arasında Global Reach oluşturmak için anahtar ve KIMLIĞI kullanın. [Özel bir bulut oluşturmaya yönelik öğretici](tutorial-create-private-cloud.md) , anahtar ve kimlik isteme ve kullanma yordamlarını sağlar.

Çözümün yönlendirme gereksinimleri, diğer sanal ağlarla ve şirket içi ağlarla örtüşmelere engel olmak için özel bulut ağ adresi alanları planlamanız gerekir. AVS özel bulutları `/22` , alt ağlar için aşağıda gösterilen en az bir CIDR ağ adres bloğu gerektirir. Bu ağ, şirket içi ağlarınızı tamamlar. Şirket içi ortamlara ve sanal ağlara bağlanmak için bu, çakışmayan bir ağ adresi bloğu olmalıdır.

Örnek `/22` CIDR ağ adresi bloğu:`10.10.0.0/22`

Alt ağlar:

| Ağ kullanımı             | Alt ağ | Örnek        |
| ------------------------- | ------ | -------------- |
| Özel bulut yönetimi            | `/24`    | `10.10.0.0/24`   |
| vMotion ağı       | `/24`    | `10.10.1.0/24`   |
| VM iş yükleri | `/24`   | `10.10.2.0/24`   |
| ExpressRoute eşlemesi | `/24`    | `10.10.3.8/30`   |

## <a name="next-steps"></a>Sonraki adımlar 

Bir sonraki adım, [özel bulut depolaması kavramlarını](concepts-storage.md)öğrenecekti.

<!-- LINKS - external -->
[enable Global Reach]: ../expressroute/expressroute-howto-set-global-reach.md

<!-- LINKS - internal -->
