---
title: Azure sanal ağ eşlemesi
titlesuffix: Azure Virtual Network
description: Azure 'da sanal ağ eşlemesi hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: anavinahar
ms.service: virtual-network
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/07/2019
ms.author: anavin
ms.openlocfilehash: a53d5810b20aa8389c152889fed5d7f4e8cfc5b7
ms.sourcegitcommit: 42748f80351b336b7a5b6335786096da49febf6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72177670"
---
# <a name="virtual-network-peering"></a>Sanal ağ eşlemesi

Sanal ağ eşlemesi, Azure [sanal ağlarına](virtual-networks-overview.md)sorunsuz bir şekilde bağlanmanıza olanak sağlar. Eşlendikten sonra, bağlantı amacıyla sanal ağlar bir tane olarak görünür. Eşlenen sanal ağlardaki sanal makineler arasındaki trafik, Microsoft omurga altyapısı aracılığıyla yönlendirilir ve aynı sanal ağdaki sanal makineler arasında çok benzer trafik, yalnızca *özel* IP adresleri üzerinden yönlendirilir. Azure şunları destekler:
* VNet eşlemesi-sanal ağları aynı Azure bölgesi içinde bağlama
* Küresel VNet eşlemesi-Azure bölgelerinde sanal ağlar bağlama

Yerel veya genel olsun, sanal ağ eşlemesi kullanmanın avantajları şunlardır:

* Eşlenen sanal ağlar arasındaki ağ trafiği özeldir. Sanal ağlar arasındaki trafik, Microsoft omurga ağında tutulur. Sanal ağlar arasındaki iletişimde hiçbir ortak Internet, ağ geçidi veya şifreleme gerekli değildir.
* Farklı sanal ağlardaki kaynaklar arasında düşük gecikme süreli, yüksek bant genişliğine sahip bir bağlantı.
* Sanal ağlar eşlendikten sonra, bir sanal ağdaki kaynakların farklı bir sanal ağdaki kaynaklarla iletişim kurmasına olanak tanır.
* Azure abonelikleri, dağıtım modelleri ve Azure bölgeleri arasında veri aktarma özelliği.
* Azure Resource Manager veya kullanılarak oluşturulan sanal ağları, klasik dağıtım modeliyle oluşturulan bir sanal ağa Kaynak Yöneticisi ile oluşturulmuş bir sanal ağ ile eşleyebilme özelliği. Azure dağıtım modelleri hakkında daha fazla bilgi edinmek için bkz. [Azure dağıtım modellerini anlama](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* Eşleme oluştururken veya eşleme oluşturulduktan sonra herhangi bir sanal ağdaki kaynaklara kesinti olmaz.

## <a name="connectivity"></a>Bilirlik

Sanal ağlar eşlendikten sonra, her iki sanal ağdaki kaynaklar eşlenen sanal ağdaki kaynaklarla doğrudan bağlantı sağlayabilir.

Aynı bölgedeki eşlenmiş sanal ağlardaki sanal makineler arasındaki ağ gecikmesi, tek bir sanal ağ içindeki gecikme süresiyle aynıdır. Ağ aktarım hızı, müşterinizin istekleriyle orantılı sanal makinesi için izin verilen bant genişliğine bağlıdır. Eşleme içinde bant genişliği üzerinde herhangi bir ek kısıtlama yoktur.

Eşlenen sanal ağlardaki sanal makineler arasındaki trafik, bir ağ geçidi veya genel Internet üzerinden değil, doğrudan Microsoft omurga altyapısı aracılığıyla yönlendirilir.

Ağ güvenlik grupları, isterseniz, diğer sanal ağlara veya alt ağlara erişimi engellemek için sanal ağ üzerinde uygulanabilir.
Sanal Ağ eşlemesini yapılandırırken, sanal ağlar arasındaki ağ güvenlik grubu kurallarını açabilir veya kapatabilirsiniz. Eşlenen sanal ağlar arasında tam bağlantıyı (varsayılan seçenek) açarsanız, belirli erişimleri engellemek ya da reddetmek için belirli alt ağlara veya sanal makinelere ağ güvenlik grupları uygulayabilirsiniz. Ağ güvenlik grupları hakkında daha fazla bilgi edinmek için bkz. [ağ güvenlik gruplarına genel bakış](security-overview.md).

## <a name="service-chaining"></a>Hizmet zincirleme

Hizmet zincirlemesini etkinleştirmek için, eşlenmiş sanal ağlardaki sanal makineleri bir *sonraki atlama* IP adresi veya sanal ağ geçitleri olarak işaret eden Kullanıcı tanımlı yollar yapılandırabilirsiniz. Hizmet zincirleme, trafiği bir sanal ağdan sanal bir gereç veya sanal ağ geçidine, eşlenmiş bir sanal ağda, Kullanıcı tanımlı yollarla yönlendirmenizi sağlar.

Hub sanal ağının ağ sanal gereci veya VPN Gateway gibi altyapı bileşenlerini barındırabileceği, hub ve bağlı olan ağları dağıtabilirsiniz. Tüm bağlı olan sanal ağlar daha sonra hub sanal ağıyla eş alabilir. Trafik, hub sanal ağındaki ağ sanal cihazları veya VPN ağ geçitleri üzerinden akabilir. 

Sanal ağ eşlemesi, Kullanıcı tanımlı bir yoldaki sonraki atlamanın, eşlenen sanal ağdaki bir sanal makinenin IP adresi veya bir VPN ağ geçidi olmasını sağlar. Ancak, bir sonraki atlama türü olarak bir ExpressRoute ağ geçidi belirten Kullanıcı tanımlı bir yol ile sanal ağlar arasında yönlendirme yapabilirsiniz. Kullanıcı tanımlı rotalar hakkında daha fazla bilgi edinmek için bkz. [Kullanıcı tanımlı yollara genel bakış](virtual-networks-udr-overview.md#user-defined). Hub ve bağlı bileşen ağ topolojisi oluşturmayı öğrenmek için bkz. [hub ve bağlı bileşen ağ topolojisi](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="gateways-and-on-premises-connectivity"></a>Ağ geçitleri ve şirket içi bağlantı

Başka bir sanal ağla eşlenip eşlenmediğine bakılmaksızın her sanal ağ kendi ağ geçidine sahip olabilir ve şirket içi ağa bağlanmak için onu kullanabilir. Sanal ağlar eşlenmiş olsa bile ağ geçitlerini kullanarak [sanal ağdan sanal ağ bağlantılarını](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) da yapılandırabilirsiniz.

Sanal ağ bağlantısı için her iki seçenek de yapılandırıldığında, sanal ağlar arasındaki trafik, eşleme yapılandırması (Azure omurga aracılığıyla) üzerinden akar.

Sanal ağlar eşlenirse, eşlenen sanal ağdaki ağ geçidini şirket içi bir ağa geçiş noktası olarak da yapılandırabilirsiniz. Bu durumda, uzak ağ geçidi kullanan sanal ağın kendi ağ geçidi olamaz. Bir sanal ağın yalnızca bir ağ geçidi olabilir. Ağ Geçidi, aşağıdaki resimde gösterildiği gibi yerel veya uzak bir ağ geçidi (eşlenen sanal ağ) olabilir:

![sanal ağ eşleme geçişi](./media/virtual-networks-peering-overview/figure04.png)

Ağ Geçidi geçişi hem VNet eşlemesi hem de küresel VNet eşlemesi için desteklenir. Farklı dağıtım modelleri (Kaynak Yöneticisi ve klasik) aracılığıyla oluşturulan sanal ağlar arasındaki ağ geçidi geçişi, yalnızca ağ geçidi sanal ağda (Kaynak Yöneticisi) olduğunda desteklenir. Aktarım için bir ağ geçidi kullanma hakkında daha fazla bilgi edinmek için bkz. [sanal ağ eşağında aktarım için BIR VPN ağ geçidi yapılandırma](../vpn-gateway/vpn-gateway-peering-gateway-transit.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Tek bir Azure ExpressRoute bağlantısını paylaşan sanal ağlar eşlenirken, aralarındaki trafik eşleme ilişkisine (Azure omurga ağı aracılığıyla) gider. Şirket içi devreye bağlanmak için her bir sanal ağ üzerindeki yerel ağ geçitlerini kullanmaya devam edebilirsiniz. Alternatif olarak, paylaşılan bir ağ geçidini kullanabilir ve şirket içi bağlantı için geçişi yapılandırabilirsiniz.

## <a name="troubleshoot"></a>Sorunları Gider

Bir sanal ağ eşlemesini onaylamak için, bir sanal ağ içindeki herhangi bir alt ağda bulunan bir ağ arabirimi için [geçerli yolları kontrol](diagnose-network-routing-problem.md) edebilirsiniz. Bir sanal ağ eşlemesi varsa, sanal ağ içindeki tüm alt ağların bir sonraki atlama türü *VNET eşlemesi*olan ve her bir eşlenen sanal ağdaki her adres alanı için yolları vardır.

Ayrıca, ağ izleyicisinin [bağlantı denetimini](../network-watcher/network-watcher-connectivity-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)kullanarak eşlenmiş bir sanal ağdaki sanal makineye bağlantı sorunlarını giderebilirsiniz. Bağlantı denetimi, trafiğin bir kaynak sanal makinenin ağ arabiriminden hedef sanal makinenin ağ arabirimine nasıl yönlendirildiğini görmenizi sağlar.

Ayrıca, [sanal ağ eşleme sorunları Için sorun gidericiyi](https://support.microsoft.com/help/4486956/troubleshooter-for-virtual-network-peering-issues)deneyebilirsiniz.

## <a name="requirements-and-constraints"></a>Gereksinimler ve kısıtlamalar

Aşağıdaki kısıtlamalar yalnızca sanal ağlar genel olarak eşlendikten sonra geçerlidir:
- Bir sanal ağdaki kaynaklar, genel olarak eşlenmiş bir sanal ağdaki temel bir iç yük dengeleyicinin ön uç IP adresiyle iletişim kuramaz. Temel Load Balancer desteği yalnızca aynı bölgede bulunur. Hem VNet eşlemesi hem de küresel VNet eşlemesi için Standart Load Balancer desteği vardır. Küresel VNet eşlemesi üzerinden çalışmayan temel bir yük dengeleyici kullanan hizmetler [burada belgelenmiştir.](virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers)

Gereksinimler ve kısıtlamalar hakkında daha fazla bilgi için bkz. [sanal ağ eşleme gereksinimleri ve kısıtlamaları](virtual-network-manage-peering.md#requirements-and-constraints). Bir sanal ağ için oluşturabileceğiniz eşleme sayısına yönelik sınırlar hakkında bilgi edinmek için bkz. [Azure ağ sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). 

## <a name="permissions"></a>İzinler

Sanal ağ eşlemesi oluşturmak için gereken izinler hakkında bilgi edinmek için bkz. [sanal ağ eşleme izinleri](virtual-network-manage-peering.md#permissions).

## <a name="pricing"></a>Fiyat

Bir sanal ağ eşleme bağlantısını kullanan giriş ve çıkış trafiği için nominal bir ücret uygulanır. VNet eşlemesi ve küresel VNet eşleme fiyatlandırması hakkında daha fazla bilgi için [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/virtual-network)bakın.

Ağ Geçidi geçişi, bir sanal ağın, şirket içi veya VNet-VNet bağlantısı için eşlenmiş bir sanal ağda VPN/ExpressRoute Gateway kullanmasını sağlayan bir eşleme özelliğidir. Eşlenen VNet 'teki ağ geçidine (giriş veya çıkış) giden trafik, VNet eşleme ücretlerine tabi olacaktır. Daha fazla ayrıntı için [VPN Gateway ücretlerine](https://azure.microsoft.com/pricing/details/vpn-gateway/) veya ExpressRoute ağ geçidi ücretlerine ve [VNET eşleme ücretlerine bakın.](https://azure.microsoft.com/pricing/details/virtual-network)

## <a name="next-steps"></a>Sonraki adımlar

* Aynı veya farklı aboneliklerde bulunan farklı dağıtım modelleri arasında oluşturulan sanal ağlar arasında bir sanal ağ eşlemesi oluşturulur. Aşağıdaki senaryolardan biri için öğreticiyi doldurun:

    |Azure dağıtım modeli             | Abonelik  |
    |---------                          |---------|
    |Her iki Kaynak Yöneticisi              |[Naklettiğiniz](tutorial-connect-virtual-networks-portal.md)|
    |                                   |[Farklı](create-peering-different-subscriptions.md)|
    |Bir Kaynak Yöneticisi, klasik bir  |[Naklettiğiniz](create-peering-different-deployment-models.md)|
    |                                   |[Farklı](create-peering-different-deployment-models-subscriptions.md)|

* [Hub ve bağlı bileşen ağ topolojisi](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json)oluşturmayı öğrenin.
* Tüm [sanal ağ eşleme ayarları ve bunların nasıl değiştirileceği](virtual-network-manage-peering.md)hakkında bilgi edinin.
* [VNET EŞLEMESI hakkında](virtual-networks-faq.md#vnet-peering) sık kullanılan VNET eşlemesi ve küresel VNET eşleme sorularına yanıt alın
