---
title: Azure sanal WAN küresel aktarım ağı mimarisi | Microsoft Docs
description: Sanal WAN için küresel aktarım ağı mimarisi hakkında bilgi edinin
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: article
ms.date: 07/23/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand global transit network architecture as it relates to Virtual WAN.
ms.openlocfilehash: 2376c77ecc328788c842e045aafb618cbad39b0e
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68421434"
---
# <a name="global-transit-network-architecture-and-virtual-wan"></a>Küresel aktarım ağı mimarisi ve sanal WAN

Bulut merkezli modern kurumsal BT parmak izini birleştirmek, bağlanmak ve denetlemek için kuruluşlar tarafından küresel aktarım ağı mimarisi benimsemekte. Modern bulut merkezli bir kuruluşta, ağ trafiğinin HQ 'ye geri dönüşü yoktur. Küresel aktarım ağı mimarisi, hem tanıdık ağ kavramlarını hem de bulut ve bulut tabanlı mimarilere özgü yeni kavramları temel alır.

![architecture](./media/virtual-wan-global-transit-network-architecture/architecture2.png)

**Şekil 1: Sanal WAN ile küresel transit ağı**

Modern kuruluşlar, bulutta ve Şirket içindeki hiper dağıtılan uygulamalar, veriler ve kullanıcılar arasında ubititous bağlantısı gerektirir. Azure sanal WAN, genel olarak dağıtılmış VNET 'ler, siteler, uygulamalar ve kullanıcılar arasında herhangi bir bağlantıyı etkinleştirerek küresel bir aktarım ağı mimarisine izin verir. Azure sanal WAN, Microsoft tarafından yönetilen bir hizmettir. Bu hizmetin tarafından oluşturulan tüm ağ bileşenleri Microsoft tarafından barındırılır ve yönetilir. Sanal WAN hakkında daha fazla bilgi için bkz. [sanal WAN genel bakış](virtual-wan-about.md) makalesi.

Azure sanal WAN mimarisinde Azure bölgeleri, Dallarınızı bağlamayı seçebileceğiniz hub olarak görev yapar. Dallar bağlandıktan sonra, şube ve isteğe bağlı olarak dal bağlantısı kurmak için Azure omurgasına yararlanabilirsiniz.

En fazla sayıda bağlı bileşen (dal, VNet, Kullanıcı) ve daha sonra diğer bölgelerdeki alt dalları hub 'a bağlayarak bir sanal WAN 'ı oluşturabilirsiniz. Alternatif olarak, bağlı bileşen coğrafi olarak dağıtılmışsa, bölgesel hub 'ları de oluşturabilir ve hub 'lara bağlantı oluşturabilirsiniz. Hub 'lar aynı sanal WAN 'ın her kısmıdır, ancak farklı bölgesel ilkelerle ilişkilendirilebilir.

## <a name="hub"></a>Hub ve bağlı bileşen geçişi

Küresel aktarım ağı mimarisi, bulut barındırılan Ağı ' hub ' 'ın farklı türlerde dağıtılan uç noktalar arasında geçişli bağlantı sağladığından, klasik bir hub ve bağlı bileşen bağlantı modelini temel alır.
  
Bu modelde, bir bağlı bileşen şu olabilir:

* Sanal ağ (VNet)
* Fiziksel şube sitesi
* Uzak kullanıcı
* İnternet

![Hub ve bağlı bileşen küresel geçiş diyagramı](./media/virtual-wan-global-transit-network-architecture/architecture.png)

**Şekil 2: Hub-ve-bağlı**

Şekil 2 ' de, coğrafi olarak dağıtılmış kullanıcıların, fiziksel sitelerin ve VNET 'lerin bulutta barındırılan bir ağ hub 'ı aracılığıyla birbirine bağlanmış olduğu küresel ağın mantıksal görünümü gösterilmektedir. Bu mimari, ağ uç noktaları arasında mantıksal bir atlama aktarım bağlantısı sunar. Bağlı bileşenler, fiziksel dallar için ExpressRoute veya siteden siteye VPN gibi çeşitli Azure Ağ Hizmetleri, sanal ağlar için VNet eşlemesi ve uzak kullanıcılar için Noktadan siteye VPN gibi çeşitli Azure ağ hizmetleriyle hub 'a bağlanır.

## <a name="crossregion"></a>Bölgeler arası bağlantı

Bir kuruluş için, bulut ayak izi genellikle fiziksel ayak izi izler. Çoğu kuruluş, buluta fiziksel siteleri ve kullanıcılara en yakın bir bölgeden erişir. Küresel ağ mimarisinin temel asıl ilkelerine birisi, Ağ varlıkları ve uç noktalar arasında bölgeler arası bağlantıyı etkinleştirmektir. Bir bulut ayak izi birden çok bölgeye yayılabilir. Bu, bir bölgedeki buluta bağlı bir daldan gelen trafiğin, şu anda yol haritasında olan hub-hub bağlantısı kullanılarak farklı bir bölgedeki başka bir dala veya VNet 'e ulaşabilme anlamına gelir.

## <a name="any"></a>Her türlü bağlantı

Küresel aktarım ağı mimarisi, merkezi bir ağ hub 'ı aracılığıyla herhangi bir *bağlantıyı* sağlar. Bu mimari, derleme ve bakım için daha karmaşık olan tam ağ veya kısmi ağ bağlantısı modellerinin gereksinimini ortadan kaldırır veya azaltır. Ayrıca, hub ve bağlı bileşen ve ağ ağları arasındaki yönlendirme denetimi daha kolay yapılandırılır ve korunur.

Her türlü bağlantı, genel mimari bağlamında, genel olarak dağıtılmış kullanıcılar, dallar, veri merkezleri, sanal ağlar ve uygulamalar ile geçiş hub 'ı aracılığıyla birbirlerine bağlanmasına olanak sağlar. Transit hub 'ı küresel aktarım sistemi olarak davranır.

![trafik yolları](./media/virtual-wan-global-transit-network-architecture/trafficpath.png)

**Şekil 3: Sanal WAN trafiği yolları**

Azure sanal WAN, aşağıdaki genel transit bağlantı yollarını destekler. Parantez içindeki harfler şekil 3 ' e eşlenir.

* Daldan VNet (a)  
* Dala dalı (b)
* Uzak Kullanıcı-VNet (c)
* Uzak kullanıcıdan dala (d)
* VNet eşlemesi (e) kullanarak VNet 'ten VNet 'e
* ExpressRoute Global Reach 

### <a name="branchvnet"></a>Daldan VNet

Daldan VNet, Azure sanal WAN tarafından desteklenen birincil yoldur. Bu yol, dalları Azure sanal ağları 'nda dağıtılan Azure ıAAS kurumsal iş yüklerine bağlamanıza olanak tanır. Dallar, ExpressRoute veya siteden siteye VPN aracılığıyla sanal WAN 'a bağlanabilir. Trafik, sanal WAN hub 'larına VNet bağlantıları aracılığıyla bağlı sanal ağlara geçiş yapılır.

### <a name="branchbranch"></a>Daldan dala

Dallar, ExpressRoute devreleri ve/veya siteden siteye VPN bağlantıları kullanarak bir Azure sanal WAN hub 'ına bağlanabilir. Dalları dala en yakın bölgede bulunan sanal WAN hub 'ına bağlayabilirsiniz.

Bu seçenek, kuruluşların şubelerle bağlantı kurmak için Azure omurgasına yararlanmasını sağlar. Ancak, bu özellik kullanılabilir olsa bile, dalları Azure sanal WAN üzerinden bağlama avantajlarına ve özel bir WAN kullanmaya yönelik avantajlardan yararlanabilirsiniz.

### <a name="usertovnet"></a>Uzak kullanıcıdan sanal ağa

Uzak bir Kullanıcı istemcisinden bir sanal WAN 'a Noktadan siteye bağlantıları kullanarak Azure 'a doğrudan, güvenli uzaktan erişim sağlayabilirsiniz. Kurumsal uzak kullanıcıların artık bir kurumsal VPN kullanarak buluta daha ince bir şekilde PIN 'i olması gerekmez.

### <a name="usertobranch"></a>Uzak kullanıcıdan dala

Uzak kullanıcıdan dal yolu, Azure 'a Noktadan siteye bağlantı kullanan uzak kullanıcıların, bulut üzerinden geçiş yaparak şirket içi iş yüklerine ve uygulamalarına erişmesini sağlar. Bu yol, uzak kullanıcılara hem Azure 'da hem de şirket içinde dağıtılan iş yüklerine erişme esnekliği sağlar. Kuruluşlar, Azure sanal WAN 'da Merkezi bulut tabanlı güvenli uzaktan erişim hizmetini etkinleştirebilir.

### <a name="vnetvnet"></a>VNET eşlemesi kullanarak VNet 'ten VNet 'e aktarma

Birden çok sanal ağ arasında uygulanan çok katmanlı uygulamaları desteklemek için VNet 'leri birbirlerine bağlamak için sanal ağ eşleme kullanın. Azure sanal WAN aracılığıyla VNet-VNet aktarma senaryosu şu anda desteklenmemektedir, ancak Azure yol haritası üzerinde. VNet eşlemesi ile sanal ağları bağlama, birbirine bağlanması gereken sanal ağlar için önerilen çözümdür. [Ağ geçidi geçişi](../virtual-network/virtual-network-peering-overview.md#gateways-and-on-premises-connectivity) (VNet eşlemesi bağlamında), sanal WAN otomatik olarak ağ geçidi geçişi sağladığından sanal WAN için gerekli değildir.

### <a name="globalreach"></a>ExpressRoute Global Reach

ExpressRoute, şirket içi ağlarınızı Microsoft Bulut bağlamak için özel ve dayanıklı bir yoldur. ExpressRoute Global Reach ExpressRoute için bir eklenti özelliğidir. Global Reach ile, şirket içi ağlarınızla özel bir ağ oluşturmak için ExpressRoute bağlantı devresine bir araya getirebilirsiniz. ExpressRoute kullanarak Azure sanal WAN 'a bağlı dallar, ExpressRoute Global Reach birbirleriyle iletişim kurmasını gerektirir.

Bu modelde, ExpressRoute kullanılarak sanal WAN hub 'ına bağlanan her dal, sanal ağlar arası yolu kullanarak VNet 'lere bağlanabilir. ExpressRoute Global Reach, Azure WAN üzerinde daha iyi bir yol sağladığından, Dalla olan trafik hub 'ını iletimez.

## <a name="security"></a>Güvenlik ve ilke denetimi

Sanal ağ hub 'ı birbirine bağlanır ve potansiyel olarak tüm geçiş trafiğini görür. Bu, bir bulut yönlendirmesi, ağ ilkesi ve güvenlik ve Internet erişim denetimi gibi merkezi ağ işlevlerini ve hizmetlerini barındırmak için yer alabilir.

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN kullanarak bağlantı oluşturun.

* [Sanal WAN kullanarak siteden siteye bağlantılar](virtual-wan-site-to-site-portal.md)
* [Sanal WAN kullanarak ExpressRoute bağlantıları](virtual-wan-expressroute-portal.md)
