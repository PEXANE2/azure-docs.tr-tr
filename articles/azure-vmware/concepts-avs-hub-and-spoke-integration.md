---
title: Kavram-bir hub ve bağlı bileşen mimarisinde bir Azure VMware çözüm dağıtımını tümleştirme
description: Azure 'da var olan veya yeni bir hub ve bağlı bileşen mimarisinde Azure VMware çözüm dağıtımını tümleştirme önerileri hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: a1ba75369a1d6f19ca7ae1f36107170f219f72d0
ms.sourcegitcommit: 271601d3eeeb9422e36353d32d57bd6e331f4d7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88649593"
---
# <a name="integrate-azure-vmware-solution-in-a-hub-and-spoke-architecture"></a>Azure VMware çözümünü bir hub ve bağlı bileşen mimarisinde tümleştirme

Bu makalede, Azure 'da var olan veya yeni bir [hub ve bağlı bileşen mimarisinde](/azure/architecture/reference-architectures/hybrid-networking/shared-services) Azure VMware çözüm dağıtımını tümleştirmeyle ilgili öneriler sağlıyoruz. 

Hub ve bağlı bileşen senaryosu, üzerinde iş yükleri içeren bir karma bulut ortamı olduğunu varsayar:

* IaaS veya PaaS hizmetlerini kullanarak yerel Azure
* Azure VMware Çözümü 
* Şirket içi sanal küre

## <a name="architecture"></a>Mimari

*Hub* , şirket Içi ve Azure VMware Çözüm özel bulutunuz için merkezi bir bağlantı noktası görevi gören bir Azure sanal ağı. Sanal *ağlar, platformlar* arası ağ iletişimini etkinleştirmek için hub ile ilişkilendirilen sanal ağlardır.

Şirket içi veri merkezi, Azure VMware çözümü özel bulutu ve hub arasındaki trafik, ExpressRoute bağlantıları üzerinden geçer. Bağlı bileşen sanal ağları genellikle IaaS tabanlı iş yükleri içerir, ancak sanal ağla doğrudan tümleştirme veya [Azure özel bağlantısı](../private-link/index.yml) etkinleştirilmiş diğer PaaS hizmetleri olan [App Service ortamı](../app-service/environment/intro.md)gibi PaaS hizmetlerine sahip olabilir. 

Diyagramda, Azure 'da ExpressRoute aracılığıyla şirket içi ve Azure VMware çözümüne bağlı bir hub ve bağlı bileşen dağıtımı örneği gösterilmektedir.

:::image type="content" source="./media/hub-spoke/avs-hub-and-spoke-deployment.png" alt-text="Azure VMware Çözüm Merkezi ve bağlı bileşen tümleştirme dağıtımı":::




Mimaride aşağıdaki ana bileşenler bulunur:

-   **Şirket içi site:** Müşteri şirket içi veri merkezi, bir ExpressRoute bağlantısı üzerinden Azure 'a bağlandı.

-   **Azure VMware çözümü özel bulutu:** Her biri en fazla 16 düğüm içeren bir veya daha fazla vSphere kümesi tarafından oluşturulan Azure VMware Çözüm SDDC.

-   **ExpressRoute ağ geçidi:** Azure VMware çözümü özel bulutu, şirket içi ağ, hub sanal ağı üzerindeki paylaşılan hizmetler ve bağlı bileşen sanal ağlarında çalışan iş yükleri arasındaki iletişimi mümkün bir şekilde sunar.

    > [!NOTE]
    > **S2S VPN konuları:** Azure VMware Çözüm üretim dağıtımları için, HCX ağ gereksinimleri nedeniyle Azure S2S desteklenmez. Ancak, HCX gerektirmeyen bir PoC veya üretim dışı dağıtımı için kullanılabilir.

-   **Hub sanal ağı:** Şirket içi ağınıza ve Azure VMware çözümü özel buluta merkezi bağlantı noktası görevi görür.

-   **Bağlı bileşen sanal ağı**

    -   **IaaS bağlı bileşen:** IaaS bağlı bileşeni, VM kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri dahil olmak üzere Azure IaaS tabanlı iş yüklerini ve ilgili ağ bileşenlerini barındırır.

    -   **PaaS bağlı bileşen:** PaaS bağlı, özel bir [uç nokta](../private-link/private-endpoint-overview.md) ve [özel bağlantı](../private-link/private-link-overview.md)sayesinde özel adresleme kullanarak Azure PaaS hizmetlerini barındırır.

-   **Azure Güvenlik Duvarı:** Bağlı bileşenler, şirket içi ve Azure VMware çözümü arasındaki trafiği segmentlere ayırmak için merkezi bir parça olarak davranır.

-   **Application Gateway:** Azure IaaS/PaaS veya Azure VMware Çözüm sanal makineleri (VM 'Ler) üzerinde çalışan Web uygulamalarını gösterir ve korur. API Management gibi diğer hizmetlerle tümleştirilir.

## <a name="network-and-security-considerations"></a>Ağ ve güvenlik konuları

ExpressRoute bağlantıları, trafiği şirket içi, Azure VMware çözümü ve Azure ağ yapısı arasında akışa sağlar. Azure VMware çözümü, bu bağlantıyı uygulamak için [ExpressRoute Global Reach](../expressroute/expressroute-global-reach.md) kullanır.

Şirket içi bağlantı, ExpressRoute Global Reach de kullanabilir, ancak zorunlu değildir.

* **Şirket içinden Azure VMware Çözüm trafiği akışı**

  :::image type="content" source="media/hub-spoke/on-prem-to-avs-traffic-flow.png" alt-text="Şirket içinden Azure VMware Çözüm trafiği akışı":::


* **Azure VMware çözümü ile hub VNET trafik akışı**

  :::image type="content" source="media/hub-spoke/avs-to-hub-vnet-traffic-flow.png" alt-text="Azure VMware çözümünü hub sanal ağ trafiği akışına":::


Azure VMware Çözüm ağı ve ınterconnectivity kavramları hakkında daha fazla ayrıntıyı [Azure VMware çözüm ürün belgelerinde](./concepts-networking.md)bulabilirsiniz.

### <a name="traffic-segmentation"></a>Trafik kesimlemesi

[Azure Güvenlik Duvarı](../firewall/index.yml) , hub sanal ağında dağıtılan hub ve bağlı bileşen topolojisinin merkezi parçasıdır. Trafik kuralları oluşturmak ve farklı bağlı bileşenler, şirket içi ve Azure VMware Çözüm iş yükleri arasındaki iletişimi segmentlere ayırmak için Azure Güvenlik Duvarı veya başka bir Azure desteklenen ağ sanal gereci kullanın.

Trafiği Azure Güvenlik Duvarı 'na yönlendirmek için yol tabloları oluşturun.  Bağlı olan sanal ağlar için, Azure Güvenlik duvarının iç arabirimine varsayılan yolu ayarlayan bir yol oluşturun, bu şekilde sanal ağdaki bir iş yükünün Azure VMware çözüm adres alanına ulaşması gerektiğinde güvenlik duvarının bunu değerlendirebilmesi ve buna izin vermek veya reddetmek üzere ilgili trafik kuralını uygulamanız gerekir.  

:::image type="content" source="media/hub-spoke/create-route-table-to-direct-traffic.png" alt-text="Trafiği Azure Güvenlik Duvarı 'na yönlendirmek için rota tabloları oluşturma":::


> [!IMPORTANT]
> **Gatewaysubnet** ayarında 0.0.0.0/0 adres ön ekine sahip bir yol desteklenmez.

Karşılık gelen yol tablosundaki belirli ağların yollarını ayarlayın. Örneğin, Azure VMware Çözüm yönetimine ve iş yüklerinden IP öneklerine şirket içi ve tek başına ulaşacak yollar, Şirket içinden gelen trafiği Azure Güvenlik Duvarı aracılığıyla Azure VMware çözümü özel bulutuna yönlendirme.

:::image type="content" source="media/hub-spoke/specify-gateway-subnet-for-route-table.png" alt-text="Karşılık gelen yol tablosundaki belirli ağlar için yolları ayarla":::

Daha ayrıntılı bir trafik ilkesi oluşturmak için, tekerlek ve hub içindeki ağ güvenlik grupları kullanılarak yapılan ikinci bir trafik düzeyi. 


### <a name="application-gateway"></a>Application Gateway

Azure Application Gateway v1 ve v2, Azure VMware Çözüm VM 'lerinde arka uç havuzu olarak çalışan Web Apps ile test edilmiştir. Application Gateway Şu anda Azure VMware Çözüm VM 'lerinde çalışan Web uygulamalarını internet 'e sunmak için desteklenen tek yöntemdir. Ayrıca, uygulamaları iç kullanıcılara güvenli bir şekilde kullanıma sunabilir.

:::image type="content" source="media/hub-spoke/avs-second-level-traffic-segmentation.png" alt-text="Ağ güvenlik grupları kullanılarak ikinci trafik segmentinin düzeyi":::


### <a name="jumpbox-and-azure-bastion"></a>Sıçrama kutusu ve Azure savunma

Hub sanal ağı içindeki paylaşılan hizmet alt ağında dağıtılan bir Windows 10 veya Windows Server VM olan JumpBox ile Azure VMware Çözüm ortamına erişin.

En iyi güvenlik uygulaması olarak, hub sanal ağı içinde [Microsoft Azure](../bastion/index.yml) savunma hizmeti dağıtın. Azure savunma, Azure 'da dağıtılan VM 'lere, bu kaynaklara genel IP adresleri sağlamaya gerek kalmadan sorunsuz RDP ve SSH erişimi sağlar. Azure savunma hizmetini sağladığınızda, seçili VM 'ye Azure portal erişebilirsiniz. Bağlantı kurulduktan sonra, sıçrama kutusu Desktop ' ı gösteren yeni bir sekme açılır ve bu masaüstünden Azure VMware çözümüne özel bulut yönetim düzlemine erişebilirsiniz.

> [!IMPORTANT]
> Sıçrama kutusu VM 'sine genel bir IP adresi vermeyin veya 3389/TCP bağlantı noktasını genel İnternet 'e sunmaz. 


:::image type="content" source="media/hub-spoke/azure-bastion-hub-vnet.png" alt-text="Azure savunma merkezi sanal ağı":::


## <a name="azure-dns-resolution-considerations"></a>Azure DNS çözümleme konuları

Azure DNS çözümleme için kullanılabilecek iki seçenek vardır:

-   Hub üzerinde dağıtılan Azure Active Directory (Azure AD) etki alanı denetleyicilerini ( [kimlik hususları](#identity-considerations)bölümünde açıklanmıştır) ad sunucuları olarak kullanın.

-   Azure DNS bir özel bölge dağıtın ve yapılandırın.

En iyi yaklaşım, Azure VMware çözümü, şirket içi ve Azure için güvenilir ad çözümlemesi sağlamak üzere her ikisini de birleştirmek içindir.

Genel bir tasarım önerisi olarak, hub sanal ağında dağıtılan ve bağlı olan sanal ağlarda, DNS ayarlarında bu Azure DNS sunucularını kullanmak için yapılandırılmış olan en az iki Azure VM 'ye dağıtılan mevcut Azure DNS altyapısını (Bu örnekte Active Directory tümleşik DNS) kullanın.

Azure Özel DNS, Azure Özel DNS bölgesinin sanal ağlarla bağlantılı olduğu yerlerde hala kullanılabilir ve DNS sunucuları, müşteri Azure Özel DNS altyapısını kullanan DNS çalıştıran şirket içi/Azure VMware çözümüne koşullu iletme ile karma çözümleyiciler olarak kullanılır.

Azure DNS özel bölgeler için göz önünde bulundurmanız gereken bazı noktalar vardır:

* Bağlı olan sanal ağlarda dağıtılan VM 'Ler için DNS kayıtlarının yaşam döngüsünü otomatik olarak yönetmesi Azure DNS için otomatik kayıt etkinleştirilmelidir.
* Bir sanal ağın, oto kaydı etkin ile bağlantılandırılan en fazla özel DNS bölgesi sayısı yalnızca bir tane olabilir.
* Bir sanal ağın bağlı olduğu en fazla özel DNS bölgesi sayısı, tekrar kayıt özelliği olmadan 1000 ' dir.

Şirket içi ve Azure VMware Çözüm sunucuları, Azure Özel DNS bölgesi için Azure 'daki sanal makine çözümleyicilerine koşullu ileticilerle yapılandırılabilir.

## <a name="identity-considerations"></a>Kimlik konuları

Kimlik açısından en iyi yaklaşım, paylaşılan hizmet alt ağını kullanarak hub üzerinde en az bir AD etki alanı denetleyicisi dağıtmaktır. Bu, her ikisi de bölge tarafından dağıtılan veya bir VM kullanılabilirlik kümesidir. Şirket içi AD etki alanınızı Azure 'a genişletmek için [Azure mimari merkezi](/azure/architecture/reference-architectures/identity/adds-extend-domain) bakın.

Ayrıca, vSphere ortamında kimlik ve DNS kaynağı olarak görev yapmak için Azure VMware Çözüm tarafında başka bir etki alanı denetleyicisi dağıtın.

VCenter ve SSO için, ** \> kimlik \> kimlik kaynaklarını yönetme**üzerinde Azure Portal bir kimlik kaynağı ayarlayın.

Önerilen en iyi yöntem olarak, [ad etki alanını Azure Active Directory ile](/azure/architecture/reference-architectures/identity/azure-ad)tümleştirin.

<!-- LINKS - external -->
[Azure Architecture Center]: /azure/architecture/

[Hub & Spoke topology]: /azure/architecture/reference-architectures/hybrid-networking/hub-spoke

[Azure networking documentation]: ../networking/index.yml

<!-- LINKS - internal -->
