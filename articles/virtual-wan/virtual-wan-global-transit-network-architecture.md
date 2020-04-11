---
title: 'Mimari: Küresel transit ağ mimarisi'
titleSuffix: Azure Virtual WAN
description: Virtual WAN için küresel transit ağ mimarisi hakkında bilgi edinin
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: article
ms.date: 02/06/2020
ms.author: cherylmc
ms.openlocfilehash: 9515058bc78a2d56dc1734c046dac5d5b04f68d9
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81113171"
---
# <a name="global-transit-network-architecture-and-virtual-wan"></a>Küresel transit ağ mimarisi ve Virtual WAN

Modern işletmeler, bulut ve şirket içinde hiper-dağıtılmış uygulamalar, veriler ve kullanıcılar arasında her yerde bağlantı gerektirir. Küresel transit ağı mimarisi, bulut merkezli modern, küresel kurumsal BT ayak izini birleştirmek, bağlamak ve kontrol etmek için işletmeler tarafından benimsenmektedir.

Küresel geçiş ağı mimarisi, bulut barındırılan ağ 'hub'ının farklı sözcü türleri arasında dağıtılabilen uç noktalar arasında geçişli bağlantı sağladığı klasik hub ve kollu bağlantı modeline dayanır.

Bu modelde, bir spoke olabilir:
* Sanal ağ (VNets)
* Fiziksel şube sitesi
* Uzak kullanıcı
* Internet

![hub ve konuştu](./media/virtual-wan-global-transit-network-architecture/figure1.png)

**Şekil 1: Küresel transit hub ve kollu ağ**

Şekil 1, coğrafi olarak dağıtılmış kullanıcıların, fiziksel sitelerin ve VNet'lerin bulutta barındırılan bir ağ hub'ı üzerinden birbirine bağlandığı küresel geçiş ağının mantıksal görünümünü gösterir. Bu mimari, ağ uç noktaları arasında mantıksal tek atlamalı geçiş bağlantısı sağlar.

## <a name="global-transit-network-with-virtual-wan"></a><a name="globalnetworktransit"></a>Virtual WAN ile küresel transit ağı

Azure Virtual WAN, Microsoft tarafından yönetilen bir bulut ağı hizmetidir. Bu hizmetin oluşturduğu tüm ağ bileşenleri Microsoft tarafından barındırılır ve yönetilir. Virtual WAN hakkında daha fazla bilgi için [Sanal WAN Genel Bakış](virtual-wan-about.md) makalesine bakın.

Azure Virtual WAN, VNets, şube siteleri, SaaS ve PaaS uygulamalarında ve kullanıcılarda küresel olarak dağıtılan bulut iş yükleri kümeleri arasında her yerde, her biri gibi her türlü bağlantıyı etkinleştirerek küresel bir aktarım ağı mimarisine olanak tanır.

![Azure Sanal WAN](./media/virtual-wan-global-transit-network-architecture/figure2.png)

**Şekil 2: Küresel transit ağı ve Sanal WAN**

Azure Virtual WAN mimarisinde, şubelerinizi, VNet'lerinizi ve uzak kullanıcıları bağlamayı seçebileceğiniz Azure bölgelerinde sanal WAN hub'ları verilir. Fiziksel şube siteleri Hub'a Premium ExpressRoute veya siteye bağlı VPN'ler, VNet'ler VNet bağlantıları yla hub'a bağlanır ve uzak kullanıcılar Kullanıcı VPN'i (noktaya yer VPN'leri) kullanarak hub'a doğrudan bağlanabilir. Virtual WAN, bir bölgedeki bir VNet'in farklı bir bölgedeki sanal WAN hub'ına bağlanabileceği bölgeler arası VNet bağlantısını da destekler.

En fazla sözcü sayısına (şube, VNet, kullanıcı) sahip olan bölgede tek bir sanal WAN hub'ı oluşturarak ve diğer bölgelerdeki sözcüleri hub'a bağlayarak sanal bir WAN oluşturabilirsiniz. Bu, kurumsal bir ayak izinin çoğunlukla birkaç uzaktan kumandalı bir bölgede olması durumunda iyi bir seçenektir.  
  
## <a name="hub-to-hub-connectivity-preview"></a><a name="hubtohub"></a>Hub'dan hub'a bağlantı (Önizleme)

Kurumsal bulut ayak izi birden çok bulut bölgesine yayılabilir ve buluta fiziksel sitesine ve kullanıcılarına en yakın bölgeden erişmek en uygun uyrabilir (gecikme süresi açısından). Küresel geçiş ağı mimarisinin temel ilkelerinden biri, tüm bulut ve şirket içi ağ uç noktaları arasında bölgeler arası bağlantı sağlamaktır. Bu, bir bölgedeki buluta bağlı bir daldan gelen trafiğin [Azure Global Network](https://azure.microsoft.com/global-infrastructure/global-network/)tarafından etkinleştirilen hub'dan hub'a bağlantı özelliğini kullanarak başka bir şubeye veya farklı bir bölgedeki VNet'e ulaşabileceği anlamına gelir.

![bölgeler arası](./media/virtual-wan-global-transit-network-architecture/figure3.png)

**Şekil 3: Sanal WAN bölgeler arası bağlantı**

Tek bir sanal WAN'da birden çok hub etkinleştirildiğinde, hub'lar hub'dan hub'a otomatik olarak bağlanır ve böylece birden çok bölgeye dağıtılan şubeler ve Vnet'ler arasında küresel bağlantı sağlanır. 

Ayrıca, tümü aynı sanal WAN'ın parçası olan hub'lar, farklı bölgesel erişim ve güvenlik ilkeleriyle ilişkilendirilebilir. Daha fazla bilgi için bu makalenin ilerleyen saatlerinde [Güvenlik ve ilke denetimine](#security) bakın.

## <a name="any-to-any-connectivity"></a><a name="anytoany"></a>Herhangi bir bağlantıdan herhangi bir bağlantı

Global transit ağ mimarisi, sanal WAN hub'ları aracılığıyla her türlü bağlantıya olanak tanır. Bu mimari, oluşturmak ve korumak için daha karmaşık olan kollar arasında tam örgü veya kısmi örgü bağlantısı ihtiyacını ortadan kaldırır veya azaltır. Buna ek olarak, hub ve spoke vs mesh ağlarında yönlendirme denetiminin yapılandırılması ve sürdürülmesi daha kolaydır.

Herhangi bir bağlantı (küresel mimari bağlamında) küresel olarak dağıtılan kullanıcılar, şubeler, veri merkezleri, VNets ve uygulamalar ile bir kuruluş "transit" hub(lar) üzerinden birbirlerine bağlanmak için izin verir. Azure Virtual WAN, küresel geçiş sistemi olarak görev yapıyor.

![herhangi bir](./media/virtual-wan-global-transit-network-architecture/figure4.png)

**Şekil 4: Sanal WAN trafik yolları**

Azure Virtual WAN aşağıdaki genel geçiş bağlantı yollarını destekler. Parantez içinde harfler Şekil 4'e göre haritaoluşturur.

* Şubeden VNet'e (a)
* Daldan şubeye (b)
  * ExpressRoute Global Erişim ve Sanal WAN
* Uzaktan Kullanıcıdan VNet'e (c)
* Uzaktan Kullanıcıdan şubeye (d)
* VNet-to-VNet (e)
* Daldan hub'a(f)
* Daldan hub'a-hub'dan VNet'e (g)
* VNet-hub-hub-to-VNet (h)

### <a name="branch-to-vnet-a-and-branch-to-vnet-cross-region-g"></a>Şubeden VNet'e (a) ve Şubeden VNet'e Çapraz Bölge (g)

Branch'dan VNet'e, Azure Virtual WAN tarafından desteklenen birincil yoldur. Bu yol, dalları Azure VNet'lerde dağıtılan Azure IAAS kurumsal iş yüklerine bağlamanızı sağlar. Şubeler ExpressRoute veya siteden siteye VPN üzerinden sanal WAN'a bağlanabilir. Trafik, VNet Connections aracılığıyla sanal WAN hub'larına bağlanan VNet'lere geçiş eder. Virtual WAN şube sitesine ağ geçidi geçişini otomatik olarak etkinleştirdiği için Virtual WAN için açık [ağ geçidi geçişi](../virtual-network/virtual-network-peering-overview.md#gateways-and-on-premises-connectivity) gerekmez. Bir SD-WAN CPE'yi Sanal WAN'a nasıl bağlayabilirsiniz hakkındaki [Sanal WAN İş Ortakları](virtual-wan-configure-automation-providers.md) makalesine bakın.

### <a name="expressroute-global-reach-and-virtual-wan"></a>ExpressRoute Global Erişim ve Sanal WAN

ExpressRoute, şirket içi ağlarınızı Microsoft Bulutu'na bağlamanın özel ve esnek bir yoludur. Virtual WAN, Express Route devre bağlantılarını destekler. Bir şube yi Express Route ile Virtual WAN'a bağlamak 1) Premium Devre 2) Devrenin Global Reach özellikli bir konumda olmasını gerektirir.

ExpressRoute Global Reach, ExpressRoute için bir eklenti özelliğidir. Global Reach ile, şirket içi ağlarınız arasında özel bir ağ yapmak için ExpressRoute devrelerini birbirine bağlayabilirsiniz. ExpressRoute kullanarak Azure Virtual WAN'a bağlanan dallar, ExpressRoute Global Reach'in birbirleriyle iletişim kurmasını gerektirir.

Bu modelde, ExpressRoute'u kullanarak sanal WAN hub'ına bağlanan her dal, şubeden VNet'e yolu kullanarak VNets'e bağlanabilir. ExpressRoute Global Reach, Azure WAN üzerinde daha uygun bir yol sağladığından, şubeden şubeye trafik hub'dan geçiş yapmaz.

### <a name="branch-to-branch-b-and-branch-to-branch-cross-region-f"></a>Daldan şubeye (b) ve Dal-Dal arası bölge (f)

Dallar, ExpressRoute devreleri ve/veya siteden siteye VPN bağlantıları kullanılarak bir Azure sanal WAN hub'ına bağlanabilir. Dalları, dala en yakın bölgede bulunan sanal WAN hub'ına bağlayabilirsiniz.

Bu seçenek, kuruluşların dalları bağlamak için Azure omurgasını kullanmasına olanak tanır. Ancak, bu özellik kullanılabilse bile, özel bir WAN kullanarak dalları Azure Virtual WAN üzerinden bağlamanın avantajlarını tartmalısınız.  

### <a name="remote-user-to-vnet-c"></a>Uzaktan Kullanıcıdan VNet'e (c)

Uzak bir kullanıcı istemcisinden sanal WAN'a sayfa-to-site bağlantısını kullanarak Azure'a doğrudan ve güvenli uzaktan erişimi etkinleştirebilirsiniz. Kurumsal uzak kullanıcılar artık kurumsal bir VPN kullanarak buluta saç tokası yapmak zorunda değil.

### <a name="remote-user-to-branch-d"></a>Uzaktan Kullanıcıdan şubeye (d)

Uzaktan Kullanıcıdan şubeye yol, buluttan geçiş yaparak Azure'a yerinde iş yüklerine ve uygulamalara noktadan siteye bağlantı kullanan uzak kullanıcıların izin verir. Bu yol, uzak kullanıcılara hem Azure'da hem de şirket içinde dağıtılan iş yüklerine erişme esnekliği sağlar. Kuruluşlar, Azure Virtual WAN'da merkezi bulut tabanlı güvenli uzaktan erişim hizmeti etkinleştirebilir.

### <a name="vnet-to-vnet-transit-e-and-vnet-to-vnet-cross-region-h"></a>VNet-to-VNet transit (e) ve VNet-to-VNet çapraz bölge (h)

VNet'ten VNet'e geçiş, birden fazla VNet'te uygulanan çok katmanlı uygulamaları birbirine bağlamak için VNet'lerin birbirine bağlanmasını sağlar. İsteğe bağlı olarak, VNet'leri VNet Peering aracılığıyla birbirine bağlayabilirsiniz ve bu, VWAN hub üzerinden geçişin gerekli olmadığı bazı senaryolar için uygun olabilir.

## <a name="security-and-policy-control"></a><a name="security"></a>Güvenlik ve politika denetimi

Azure Virtual WAN hub'ları, karma ağdaki tüm ağ bitiş noktalarını birbirine bağlar ve tüm transit ağ trafiğini görebilir. Sanal WAN hub'ları, bulut tabanlı güvenlik, erişim ve ilke denetimi sağlamak için Azure Güvenlik Duvarını VWAN hub'larına dağıtarak Güvenli Sanal Hub'lara dönüştürülebilir. Sanal WAN hub'larında Azure Güvenlik Duvarları'nın düzenlenmesi Azure Güvenlik Duvarı Yöneticisi tarafından gerçekleştirilebilir.

[Azure Güvenlik Duvarı Yöneticisi,](https://go.microsoft.com/fwlink/?linkid=2107683) genel geçiş ağlarıiçin güvenliği yönetme ve ölçeklendirme özellikleri sağlar. Azure Güvenlik Duvarı Yöneticisi, Azure Güvenlik Duvarı ile birlikte üçüncü taraf üzerinden yönlendirme, küresel ilke yönetimi ve gelişmiş Internet güvenlik hizmetlerini merkezi olarak yönetme olanağı sağlar.

![Azure Güvenlik Duvarı ile güvenli sanal hub](./media/virtual-wan-global-transit-network-architecture/figure5.png)

**Şekil 5: Azure Güvenlik Duvarı ile güvenli sanal hub**

Sanal WAN'a azure güvenlik duvarı aşağıdaki genel güvenli geçiş bağlantı yollarını destekler. Parantez içinde harfler Şekil 5'e göre haritaoluşturur.

* VNet-to-VNet güvenli transit (e)
* VNet-to-Internet veya üçüncü taraf Güvenlik Hizmeti (i)
* Şubeden Internet'e veya üçüncü taraf Güvenlik Hizmeti (j)

### <a name="vnet-to-vnet-secured-transit-e"></a>VNet-to-VNet güvenli transit (e)

VNet'ten VNet'e güvenli geçiş, Sanal WAN hub'ındaki Azure Güvenlik Duvarı üzerinden VNet'lerin birbirlerine bağlanmasını sağlar.

### <a name="vnet-to-internet-or-third-party-security-service-i"></a>VNet-to-Internet veya üçüncü taraf Güvenlik Hizmeti (i)

VNet'ten Internet'e veya üçüncü taraf güvenli geçiş, Sanal WAN hub'ındaki Azure Güvenlik Duvarı üzerinden VNet'lerin Internet'e veya desteklenen bir üçüncü taraf güvenlik hizmetlerine bağlanmasını sağlar.

### <a name="branch-to-internet-or-third-party-security-service-j"></a>Şubeden Internet'e veya üçüncü taraf Güvenlik Hizmeti (j)
Şubeden Internet'e veya üçüncü taraf Güvenli geçiş, şubelerin sanal WAN hub'ındaki Azure Güvenlik Duvarı üzerinden Internet'e veya desteklenen üçüncü taraf güvenlik hizmetlerine bağlanmasını sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN kullanarak bağlantı oluşturun ve VWAN hub(ları)'nda Azure Güvenlik Duvar'ı dağıtın.

* [Virtual WAN kullanarak siteden siteye bağlantılar](virtual-wan-site-to-site-portal.md)
* [Virtual WAN kullanarak ExpressRoute bağlantıları](virtual-wan-expressroute-portal.md)
* [Azure Güvenlik Duvarı Yöneticisi, Azure FW'yi VWAN'da dağıtacak](https://go.microsoft.com/fwlink/?linkid=2107683)
