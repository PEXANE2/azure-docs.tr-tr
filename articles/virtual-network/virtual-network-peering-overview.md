---
title: Azure Sanal Ağ eşleme
titlesuffix: Azure Virtual Network
description: Azure'daki sanal ağ eşlemesi hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: anavinahar
ms.service: virtual-network
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/15/2019
ms.author: anavin
ms.openlocfilehash: 5fb54e812e72b9393ffdf632085d0f32ab8b1988
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79279552"
---
# <a name="virtual-network-peering"></a>Sanal ağ eşleme

Sanal ağ eşleme, [Azure Sanal Ağ'daki](virtual-networks-overview.md)ağları sorunsuz bir şekilde bağlamanızı sağlar. Sanal ağlar bağlantı amacıyla bir ağ olarak görünür. Sanal makineler arasındaki trafik, Microsoft omurga altyapısını kullanır. Aynı ağdaki sanal makineler arasındaki trafik gibi, trafik yalnızca Microsoft'un *özel* ağı üzerinden yönlendirilir.

Azure aşağıdaki tür de şu türe bakmayı destekler:

* Sanal ağ eşleme: Aynı Azure bölgesindeki sanal ağları birbirine bağlayın.
* Küresel sanal ağ eşlemi: Azure bölgelerindesanal ağları bağlama.

Yerel veya genel sanal ağ eşlemesini kullanmanın avantajları şunlardır:

* Farklı sanal ağlardaki kaynaklar arasında düşük gecikme süresi ve yüksek bant genişlikli bağlantı.
* Bir sanal ağdaki kaynakların farklı bir sanal ağdaki kaynaklarla iletişim kurabilme yeteneği.
* Azure abonelikleri, Azure Etkin Dizin kiracıları, dağıtım modelleri ve Azure bölgeleri arasında sanal ağlar arasında veri aktarımı yapma özelliği.
* Azure Kaynak Yöneticisi aracılığıyla oluşturulan sanal ağlara göz görebilme özelliği.
* Kaynak Yöneticisi aracılığıyla oluşturulan sanal ağı klasik dağıtım modeli yle oluşturulan ağa eşlenebilme yeteneği. Azure dağıtım modelleri hakkında daha fazla bilgi edinmek için bkz. [Azure dağıtım modellerini kavrama](../azure-resource-manager/management/deployment-models.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* Eşleme oluştururken veya eşleme oluşturulduktan sonra iki sanal ağdaki kaynaklarda da kesinti süresi yaşanmaz.

Eşlenen sanal ağlar arasındaki ağ trafiği gizlidir. Sanal ağlar arasındaki trafik Microsoft omurga ağı üzerinde tutulur. Sanal ağlar arasındaki iletişimde genel İnternet, ağ geçidi veya şifreleme gerekli değildir.

## <a name="connectivity"></a>Bağlantı

Eşli sanal ağlar için, her iki sanal ağdaki kaynaklar, eşlenen sanal ağdaki kaynaklarla doğrudan bağlanabilir.

Aynı bölgede yer alan eşlenmiş sanal ağlardaki sanal makineler arasındaki ağ gecikme süresi, tek bir sanal ağdaki gecikme süresiyle aynıdır. Ağ verimi, büyüklüğüne orantılı olarak sanal makine için izin verilen bant genişliğine bağlıdır. Eşleme içindeki bant genişliği ile ilgili herhangi bir ek kısıtlama yoktur.

Eşlenmiş sanal ağlarda bulunan sanal makineler arasındaki trafik bir ağ geçidi veya genel İnternet üzerinden değil, doğrudan Microsoft omurga altyapısı aracılığıyla yönlendirilir.

Diğer sanal ağlara veya alt ağlara erişimi engellemek için sanal ağdaki ağ güvenlik gruplarını uygulayabilirsiniz.
Sanal ağ eşlemenin yapılandırılmasında, sanal ağlar arasındaki ağ güvenlik grubu kurallarını açın veya kapatın. Bakan sanal ağlar arasında tam bağlantı açarsanız, belirli erişimi engellemek veya reddetmek için ağ güvenlik gruplarını uygulayabilirsiniz. Tam bağlantı varsayılan seçenektir. Ağ güvenlik grupları hakkında daha fazla bilgi edinmek için [Güvenlik gruplarına](security-overview.md)bakın.

## <a name="service-chaining"></a>Hizmet zinciri

Hizmet zinciri, trafiği bir sanal ağdan sanal bir cihaza veya kullanıcı tanımlı rotalar aracılığıyla eşlenen bir ağdaki ağ geçidine yönlendirmenize olanak tanır.

Hizmet zincirini etkinleştirmek için, bir *sonraki atlama* IP adresi olarak eşlenen sanal ağlardaki sanal makinelere işaret eden kullanıcı tanımlı rotaları yapılandırın. Kullanıcı tanımlı rotalar, hizmet zincirini etkinleştirmek için sanal ağ ağ ağ geçitlerini de gösterebilir.

Hub sanal ağın ağ sanal cihaz veya VPN ağ geçidi gibi altyapı bileşenlerini barındırdığı hub ve kollu ağları dağıtabilirsiniz. Daha sonra, tüm ağlı sanal ağlar merkezi sanal ağla eşlenebilir. Trafik, hub sanal ağındaki ağ sanal cihazları veya VPN ağ geçitlerinden geçer.

Sanal ağ eşlemesi sayesinde, kullanıcı tanımlı yolda bir sonraki atlama, eşlenen sanal ağdaki veya bir VPN ağ geçidindeki bir sanal makinenin IP adresi olabilir. Bir sonraki atlama türü olarak Azure ExpressRoute ağ geçidini belirten kullanıcı tanımlı bir rotayla sanal ağlar arasında yol alamazsınız. Kullanıcı tanımlı yollar hakkında daha fazla bilgi için bkz. [Kullanıcı tanımlı yollara genel bakış](virtual-networks-udr-overview.md#user-defined). Hub ve kollu ağ topolojisini nasıl oluşturabilirsiniz öğrenmek için [Azure'da Hub ile konuşan ağ topolojisine](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın.

## <a name="gateways-and-on-premises-connectivity"></a>Ağ geçitleri ve şirket içi bağlantı

Eşli bir sanal ağ da dahil olmak üzere her sanal ağ kendi ağ geçidine sahip olabilir. Sanal ağ, ağ geçidini şirket içi ağa bağlanmak için kullanabilir. Ayrıca, eşlenen sanal ağlar için bile ağ geçitlerini kullanarak [sanal ağdan sanal ağ bağlantılarını](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) yapılandırabilirsiniz.

Sanal ağ bağlantısı için her iki seçeneği de yapılandırdığınızda, sanal ağlar arasındaki trafik eşleme yapılandırması üzerinden akar. Trafik, Azure omurgasını kullanır.

Ayrıca, eşlenen sanal ağdaki ağ geçidini şirket içi bir ağa geçiş noktası olarak da yapılandırabilirsiniz. Bu durumda, uzak bir ağ geçidi kullanan sanal ağ kendi ağ geçidi olamaz. Sanal ağın yalnızca bir ağ geçidi vardır. Ağ geçidi, aşağıdaki diyagramda gösterildiği gibi, eşlenen sanal ağdaki yerel veya uzak bir ağ geçididir:

![Sanal ağ eşleme geçişi](./media/virtual-networks-peering-overview/local-or-remote-gateway-in-peered-virual-network.png)

Hem sanal ağ izleme hem de global sanal ağ izleme destek ağ geçidi geçişi.

Farklı dağıtım modelleri aracılığıyla oluşturulan sanal ağlar arasında ağ geçidi geçişi desteklenir. Ağ geçidi, Kaynak Yöneticisi modelinde sanal ağda olmalıdır. Geçiş için bir ağ geçidi kullanma hakkında daha fazla bilgi için bkz. [Sanal ağ eşlemesinde geçiş için bir VPN ağ geçidi yapılandırma](../vpn-gateway/vpn-gateway-peering-gateway-transit.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Tek bir Azure ExpressRoute bağlantısını paylaşan sanal ağlara baktığınızda, aralarındaki trafik eşleme ilişkisinden geçer. Bu trafik, Azure omurga ağını kullanır. Şirket içi devreye bağlanmak için her bir sanal ağ üzerindeki yerel ağ geçitlerini kullanmaya devam edebilirsiniz. Aksi takdirde, paylaşılan bir ağ geçidi kullanabilir ve şirket içi bağlantı için geçişi yapılandırabilirsiniz.

## <a name="troubleshoot"></a>Sorun giderme

Sanal ağların bakılmış olduğunu doğrulamak için etkili yolları denetleyebilirsiniz. Sanal ağdaki herhangi bir alt ağdaki ağ arabirimi yollarını denetleyin. Bir sanal ağ eşlemesi zaten varsa, sanal ağ içindeki tüm alt ağlar, eşlenen her bir sanal ağdaki her bir adres alanı için sonraki atlama türü *VNet eşlemesi* olan yollara sahip olur. Daha fazla bilgi için [bkz.](diagnose-network-routing-problem.md)

Azure Ağ İzleyicisi'ni kullanarak, eşli bir sanal ağdaki sanal makineye bağlantı sorunu da giderebilirsiniz. Bağlantı denetimi, trafiğin kaynak sanal makinenin ağ arabiriminden hedef sanal makinenin ağ arabirimine nasıl yönlendirildiğini görmenizi sağlar. Daha fazla bilgi için Azure [portalını kullanarak Azure Ağ İzleyicisi ile sorun giderme bağlantılarına](../network-watcher/network-watcher-connectivity-portal.md#check-connectivity-to-a-virtual-machine)bakın.

[Sorun Giderme sanal ağ eşleme sorunlarını](virtual-network-troubleshoot-peering-issues.md)da deneyebilirsiniz.

## <a name="constraints-for-peered-virtual-networks"></a>Eşlenen sanal ağlar için kısıtlamalar<a name="requirements-and-constraints"></a>

Aşağıdaki kısıtlamalar yalnızca sanal ağlara genel olarak bakıldığında uygulanır:

* Bir sanal ağdaki kaynaklar, küresel olarak karşılanan bir sanal ağdaki Temel İç Yük Dengeleyicisi'nin (ILB) ön uç IP adresiyle iletişim kuramez.
* Temel yük dengeleyicisi kullanan bazı hizmetler, genel sanal ağ eşleme üzerinde çalışmaz. Daha fazla bilgi için Global [VNet Peering ve Load Balancers ile ilgili kısıtlamalar nelerdir?](virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers)

Daha fazla bilgi için [Gereksinimler ve kısıtlamalara](virtual-network-manage-peering.md#requirements-and-constraints)bakın. Desteklenen eşleme sayısı hakkında daha fazla bilgi edinmek için [Ağ sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits)bakın.

## <a name="permissions"></a>İzinler

Sanal ağ eşlemesi oluşturmak için gereken izinler hakkında bilgi edinmek için [İzinler'e](virtual-network-manage-peering.md#permissions)bakın.

## <a name="pricing"></a>Fiyatlandırma

Sanal ağ izleme bağlantısı kullanan giriş ve çıkış trafiği için nominal bir ücret vardır. Daha fazla bilgi için [Sanal Ağ fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-network)için bkz.

Ağ Geçidi Geçişi, sanal bir ağın, bir eşlenen sanal ağda VPN/ExpressRoute ağ geçidini kullanmasını sağlayan bir eşleme özelliğidir. Ağ geçidi geçişi hem tesisler arası hem de ağdan ağa bağlantı için çalışır. Eşlenen sanal ağdaki ağ geçidine (giriş veya çıkış) giden trafik, vnet(veya ağ geçidi olmayan VNet) üzerinde sanal ağ izleme ücretlerine neden olur. Daha fazla bilgi için VPN ağ geçidi ücretleri ve ExpressRoute ağ geçidi ücretleri için ExpressRoute Ağ Geçidi fiyatlandırması için [VPN Ağ Geçidi fiyatlandırması'na](https://azure.microsoft.com/pricing/details/vpn-gateway/) bakın.

>[!NOTE]
> Bu belgenin önceki bir sürümünde, ağ geçidi geçişi ile konuşan VNet'te (veya ağ geçidi olmayan VNet) sanal ağ izleme ücretlerinin geçerli olmayacağını belirtmiştir. Şimdi fiyatlandırma sayfası başına doğru fiyatlandırma yansıtır.

## <a name="next-steps"></a>Sonraki adımlar

* İki sanal ağ arasında bir eşleme oluşturabilirsiniz. Ağlar aynı aboneye, aynı abonelikteki farklı dağıtım modellerine veya farklı aboneliklere ait olabilir. Aşağıdaki senaryolardan biri için öğreticiyi tamamlayın:

    |Azure dağıtım modeli             | Abonelik  |
    |---------                          |---------|
    |Her ikisi de Resource Manager              |[Aynı](tutorial-connect-virtual-networks-portal.md)|
    |                                   |[Farklı](create-peering-different-subscriptions.md)|
    |Biri Resource Manager, diğeri klasik  |[Aynı](create-peering-different-deployment-models.md)|
    |                                   |[Farklı](create-peering-different-deployment-models-subscriptions.md)|

* Hub ve kollu ağ topolojisini nasıl oluşturabilirsiniz öğrenmek için [Azure'da Hub ile konuşan ağ topolojisine](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın.
* Tüm sanal ağ eşleme ayarları hakkında bilgi edinmek için [bkz.](virtual-network-manage-peering.md)
* Ortak sanal ağ eşleme ve genel sanal ağ eşleme sorularının yanıtları için [VNet Peering'e](virtual-networks-faq.md#vnet-peering)bakın.
