---
title: Azure sanal ağlarını planlayın | Microsoft Dokümanlar
description: Yalıtım, bağlantı ve konum gereksinimleriniz temelinde sanal ağları planlamayı öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/08/2020
ms.author: kumud
ms.openlocfilehash: 95dd7be118e869aed02bb55918ab0cefa0d05d03
ms.sourcegitcommit: 25490467e43cbc3139a0df60125687e2b1c73c09
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80998864"
---
# <a name="plan-virtual-networks"></a>Sanal ağları planlama

Deneme yapmak için sanal ağ oluşturmak yeterince kolay, ancak büyük olasılıkla, kuruluşunuzun üretim gereksinimlerini desteklemek için zaman içinde birden çok sanal ağ dağıtacak. Bazı planlamalarla, sanal ağları dağıtabilir ve ihtiyacınız olan kaynakları daha etkili bir şekilde bağlayabilirsiniz. Bu makaledeki bilgiler, sanal ağları zaten biliyorsanız ve onlarla çalışma konusunda biraz deneyime sahipseniz en yararlı dır. Sanal ağlara aşina değilseniz, [Sanal ağa genel bakış](virtual-networks-overview.md)okumanız önerilir.

## <a name="naming"></a>Adlandırma

Tüm Azure kaynaklarının bir adı var. Ad, her kaynak türü için farklılık gösterebilecek bir kapsam içinde benzersiz olmalıdır. Örneğin, sanal bir ağın adı bir [kaynak grubunda](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group)benzersiz olmalıdır, ancak [bir abonelik](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) veya Azure [bölgesinde](https://azure.microsoft.com/regions/#services)çoğaltılabilir. Kaynakları adlandırırken sürekli olarak kullanabileceğiniz bir adlandırma kuralı tanımlamak, zaman içinde birden fazla ağ kaynağını yönetirken yararlıdır. Öneriler [için, Adlandırma kurallarına](../azure-resource-manager/management/resource-name-rules.md#microsoftnetwork)bakın.

## <a name="regions"></a>Bölgeler

Tüm Azure kaynakları bir Azure bölgesinde ve abonelikte oluşturulur. Kaynak yalnızca aynı bölgede bulunan sanal bir ağda ve kaynakla abonelikte oluşturulabilir. Ancak, farklı aboneliklerde ve bölgelerde bulunan sanal ağları bağlayabilirsiniz. Daha fazla bilgi için [bağlantıya](#connectivity)bakın. Kaynakları dağıtacak ları bölgeye karar verirken, kaynakların tüketicilerinin fiziksel olarak nerede bulunduğunu göz önünde bulundurun:

- Kaynakların tüketicileri genellikle kaynaklarına en düşük ağ gecikmesi isterler. Belirli bir konum la Azure bölgeleri arasındaki göreli [gecikmesürelerini](../network-watcher/view-relative-latencies.md?toc=%2fazure%2fvirtual-network%2ftoc.json)belirlemek için bkz.
- Veri ikamet, egemenlik, uyumluluk veya esneklik gereksinimleri var mı? Bu durumda, gereksinimlere uygun bölge seçmek önemlidir. Daha fazla bilgi için Azure [coğrafyalarına](https://azure.microsoft.com/global-infrastructure/geographies/)bakın.
- Dağıttığınız kaynaklar için aynı Azure bölgesindeki Azure Kullanılabilirlik Bölgeleri'nde esneklik gerekiyor mu? Sanal makineler (VM) gibi kaynakları aynı sanal ağ daki farklı kullanılabilirlik bölgelerine dağıtabilirsiniz. Ancak tüm Azure bölgeleri kullanılabilirlik bölgelerini desteklemez. Kullanılabilirlik bölgeleri ve bunları destekleyen bölgeler hakkında daha fazla bilgi edinmek için [kullanılabilirlik bölgelerine](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın.

## <a name="subscriptions"></a>Abonelikler

Her abonelik içinde, [sınıra](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits)kadar gereken kadar sanal ağ dağıtabilirsiniz. Bazı kuruluşların farklı departmanlar için farklı abonelikleri vardır, örneğin. Aboneliklerle ilgili daha fazla bilgi ve dikkat edilmesi gerekenler için [Abonelik yönetimi'ne](/azure/cloud-adoption-framework/reference/migration-with-enterprise-scaffold#define-your-hierarchy)bakın.

## <a name="segmentation"></a>Kesimleme

Abonelik ve bölge başına birden çok sanal ağ oluşturabilirsiniz. Her sanal ağ içinde birden çok alt ağ oluşturabilirsiniz. İzleyen hususlar, kaç sanal ağa ve alt ağa ihtiyacınız olduğunu belirlemenize yardımcı olur:

### <a name="virtual-networks"></a>Sanal ağlar

Sanal ağ, Azure ortak ağının sanal ve yalıtılmış bir bölümüdür. Her sanal ağ aboneliğinize adanmıştır. Bir sanal ağ veya abonelikte birden çok sanal ağ oluşturup oluşturmamaya karar verirken göz önünde bulundurulması gerekenler:

- Trafiği ayrı sanal ağlara yalıtma için herhangi bir kuruluş güvenlik gereksinimi var mı? Sanal ağlara bağlanmayı veya bağlamamayı seçebilirsiniz. Sanal ağlara bağlanırsanız, sanal ağlar arasındaki trafik akışını denetlemek için güvenlik duvarı gibi bir ağ sanal cihazı uygulayabilirsiniz. Daha fazla bilgi için [güvenlik](#security) ve bağlantı ya da [güvenlik](#connectivity)bilgisine bakın.
- Sanal ağları ayrı [aboneliklere](#subscriptions) veya [bölgelere](#regions)ayırmak için herhangi bir kuruluş gereksinimi var mı?
- [Ağ arabirimi,](virtual-network-network-interface.md) VM'nin diğer kaynaklarla iletişim kurmasını sağlar. Her ağ arabiriminde bir veya daha fazla özel IP adresi bulunur. Sanal ağda kaç ağ arabirimi ve [özel IP adresi](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) ne kadar gerekir? Sanal ağ içinde sahip olabileceğiniz ağ arabirimleri ve özel IP adreslerinin [sayısının sınırları](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) vardır.
- Sanal ağı başka bir sanal ağa veya şirket içi ağa bağlamak istiyor musunuz? Bazı sanal ağları birbirine veya şirket içi ağlara bağlamayı seçebilirsiniz, ancak diğerlerini bağlamayabilirsiniz. Daha fazla bilgi için [bağlantıya](#connectivity)bakın. Başka bir sanal ağa veya şirket içi ağa bağlandığınız her sanal ağın benzersiz bir adres alanı olmalıdır. Her sanal ağ, adres alanına atanmış bir veya daha fazla genel veya özel adres aralığına sahiptir. Adres aralığı, 10.0.0.0/16 gibi sınıfsız internet etki alanı yönlendirme (CIDR) biçiminde belirtilir. Sanal ağlar için [adres aralıkları](manage-virtual-network.md#add-or-remove-an-address-range) hakkında daha fazla bilgi edinin.
- Farklı sanal ağlardaki kaynaklar için herhangi bir organizasyonel yönetim gereksinimleriniz var mı? Bu durumda, kuruluşunuzdaki kişilere izin [atamasını](#permissions) basitleştirmek veya farklı sanal ağlara farklı ilkeler atamak için kaynakları ayrı sanal ağa ayırabilirsiniz.
- Bazı Azure hizmet kaynaklarını sanal ağa dağıttığınızda, kendi sanal ağlarını oluştururlar. Bir Azure hizmetinin kendi sanal ağını oluşturup oluşturmadığını belirlemek için, [sanal ağa dağıtılabilen](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network)her Azure hizmetinin bilgilerini görün.

### <a name="subnets"></a>Alt ağlar

Sanal [ağ, sınırlara](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits)kadar bir veya daha fazla alt ağ olarak ayrılabilir. Bir abonelikte bir alt ağ mı yoksa birden çok sanal ağ mı oluşturulacağına karar verirken göz önünde bulundurulması gereken şeyler:

- Her alt ağ, sanal ağın adres alanı içinde CIDR biçiminde belirtilen benzersiz bir adres aralığına sahip olmalıdır. Adres aralığı sanal ağdaki diğer alt ağlarla çakışamaz.
- Bazı Azure hizmet kaynaklarını sanal bir ağa dağıtmayı planlıyorsanız, kendi alt ağlarına ihtiyaç duyabilir veya oluşturabilir, bu nedenle bunu yapmaları için yeterli tahsis edilmemiş alan olması gerekir. Bir Azure hizmetinin kendi alt netini oluşturup oluşturmadığını belirlemek için, [sanal ağa dağıtılabilen her Azure hizmetinin bilgilerine](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network)bakın. Örneğin, bir sanal ağı azure VPN Ağ Geçidi kullanarak şirket içi bir ağa bağlarsanız, sanal ağın ağ geçidi için özel bir alt ağına sahip olması gerekir. [Ağ geçidi alt ağları](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub)hakkında daha fazla bilgi edinin.
- Azure, varsayılan olarak sanal ağdaki tüm alt ağlar arasındaki ağ trafiğini yönlendirir. Alt ağlar arasında Azure yönlendirmesini önlemek veya örneğin bir ağ sanal cihazı üzerinden alt ağlar arasındaki trafiği yönlendirmek için Azure'un varsayılan yönlendirmesini geçersiz kılabilir. Bir ağ sanal cihaz (NVA) üzerinden aynı sanal ağ akışındaki kaynaklar arasındaki trafiğin olmasını istiyorsanız, kaynakları farklı alt ağlara dağıtın. [Güvenlik](#security)te daha fazla bilgi edinin.
- Azure depolama hesabı veya Azure SQL veritabanı gibi Azure kaynaklarına erişimi, sanal ağ hizmeti bitiş noktasına sahip belirli alt ağlarla sınırlandırabilirsiniz. Ayrıca, internetten kaynaklara erişimi inkar edebilirsiniz. Birden çok alt ağ oluşturabilir ve bazı alt ağlar için bir hizmet bitiş noktası etkinleştirebilirsiniz, ancak diğerleri için değil. [Hizmet bitiş noktaları](virtual-network-service-endpoints-overview.md)ve bunları etkinleştirebileceğiniz Azure kaynakları hakkında daha fazla bilgi edinin.
- Sıfır veya bir ağ güvenlik grubunu sanal ağdaki her alt ağa ilişkilendirebilirsiniz. Aynı veya farklı bir ağ güvenlik grubunu her alt ağla ilişkilendirebilirsiniz. Her ağ güvenlik grubu, kaynaklara ve hedeflere gelen ve gelen trafiğe izin veren veya reddeden kurallar içerir. [Ağ güvenlik grupları](#traffic-filtering)hakkında daha fazla bilgi edinin.

## <a name="security"></a>Güvenlik

Ağ güvenlik gruplarını ve ağ sanal cihazlarını kullanarak ağ trafiğini sanal ağdaki kaynaklara ve kaynaklardan filtreleyebilirsiniz. Azure'un trafiği alt ağlardan nasıl yönlendirirdenetebilirsiniz. Ayrıca, kuruluşunuzdaki kaynakları sanal ağlarda kimlerle çalıştırabileceğini de sınırlayabilirsiniz.

### <a name="traffic-filtering"></a>Trafik filtreleme

- Ağ güvenlik grubu, ağ trafiğini filtreleyen bir NVA veya her ikisini de kullanarak sanal ağdaki kaynaklar arasındaki ağ trafiğini filtreleyebilirsiniz. Ağ trafiğini filtrelemek için güvenlik duvarı gibi bir NVA dağıtmak için [Azure Marketi'ne](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?subcategories=appliances&page=1)bakın. NVA kullanırken, trafiği alt ağlardan NVA'ya yönlendirmek için özel yollar da oluşturursunuz. [Trafik yönlendirmesi](#traffic-routing)hakkında daha fazla bilgi edinin.
- Ağ güvenlik grubu, kaynaklara gelen veya gelen trafiğe izin veren veya reddeden birkaç varsayılan güvenlik kuralı içerir. Ağ güvenlik grubu bir ağ arabirimiyle, ağ arabiriminin içinde olduğu alt ağla veya her ikisiyle ilişkilendirilebilir. Güvenlik kurallarının yönetimini basitleştirmek için, bir ağ güvenlik grubunu mümkün olduğunca alt ağ daki tek tek ağ arabirimleri yerine tek tek alt ağlarla ilişkilendirmeniz önerilir.
- Bir alt ağdaki farklı VM'lerin kendilerine farklı güvenlik kuralları uygulanması gerekiyorsa, VM'deki ağ arabirimini bir veya daha fazla uygulama güvenlik grubuyla ilişkilendirebilirsiniz. Güvenlik kuralı, kaynağında, hedefinde veya her ikisinde de bir uygulama güvenlik grubu belirtebilir. Bu kural daha sonra yalnızca uygulama güvenlik grubunun üyesi olan ağ arabirimleri için geçerlidir. [Ağ güvenlik grupları](security-overview.md) ve uygulama güvenlik [grupları](security-overview.md#application-security-groups)hakkında daha fazla bilgi edinin.
- Azure, her ağ güvenlik grubunda birkaç varsayılan güvenlik kuralı oluşturur. Varsayılan bir kural, tüm trafiğin sanal ağdaki tüm kaynaklar arasında akmasına izin verir. Bu davranışı geçersiz kılmak için ağ güvenlik gruplarını, trafiği nva'ya veya her ikisine yönlendirmek için özel yönlendirme kullanın. Azure'un [varsayılan güvenlik kurallarının](security-overview.md#default-security-rules) tümünün hakkında bilgi edinmeniz ve ağ güvenliği grubu kurallarının bir kaynağa nasıl uygulandığını anlamanız önerilir.

Bir [NVA](/azure/architecture/reference-architectures/dmz/secure-vnet-dmz?toc=%2Fazure%2Fvirtual-network%2Ftoc.json)kullanarak Azure ve internet arasında çevre ağı (DMZ olarak da bilinir) uygulamak için örnek tasarımları görüntüleyebilirsiniz.

### <a name="traffic-routing"></a>Trafik yönlendirme

Azure, bir alt ağdan giden trafik için birkaç varsayılan yol oluşturur. Bir rota tablosu oluşturup bir alt ağa ilişkilendirerek Azure'un varsayılan yönlendirmesini geçersiz kılabilirsiniz. Azure'un varsayılan yönlendirmesi geçersiz kılınan yaygın nedenler şunlardır:
- Çünkü alt ağlar arasındaki trafiğin bir NVA'dan akmasını istiyorsunuz. NVA [üzerinden trafiği zorlamak için rota tablolarını](tutorial-create-route-table-portal.md)yapılandırma hakkında daha fazla bilgi edinmek için.
- Çünkü internete bağlı tüm trafiği bir NVA veya şirket içi bir Azure VPN ağ geçidi aracılığıyla zorlamak istiyorsunuz. Denetim ve günlük için şirket içinde internet trafiğini zorlamak genellikle zorunlu tünel leme olarak adlandırılır. [Zorunlu tünellemenin](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md?toc=%2Fazure%2Fvirtual-network%2Ftoc.json)nasıl yapılandırılabildiğini öğrenin.

Özel yönlendirme uygulamanız gerekiyorsa, [Azure'da yönlendirmeyi](virtual-networks-udr-overview.md)tanımanız önerilir.

## <a name="connectivity"></a>Bağlantı

Sanal ağ,sanal ağ eşlemi kullanarak diğer sanal ağlara veya azure VPN ağ geçidi kullanarak şirket içi ağınıza bağlayabilirsiniz.

### <a name="peering"></a>Eşleme

Sanal [ağ eşleme](virtual-network-peering-overview.md)kullanırken, sanal ağlar aynı veya farklı desteklenen Azure bölgelerinde olabilir. Sanal ağlar aynı veya farklı Azure aboneliklerinde (hatta farklı Azure Etkin Dizin kiracılarına ait abonelikler) bulunabilir. Bir eşleme oluşturmadan önce, tüm akran [gereksinimleri ve kısıtlamaları](virtual-network-manage-peering.md#requirements-and-constraints)hakkında bilgi edinmeniz önerilir. Aynı bölgede bakan sanal ağlardaki kaynaklar arasındaki bant genişliği, kaynakların aynı sanal ağda olmasıyla aynıdır.

### <a name="vpn-gateway"></a>VPN ağ geçidi

Sanal ağı nızı siteden [siteye VPN](../vpn-gateway/vpn-gateway-tutorial-vpnconnection-powershell.md?toc=%2fazure%2fvirtual-network%2ftoc.json)kullanarak veya Azure [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json)ile özel bir bağlantı kullanarak şirket içi ağınıza bağlamak için bir Azure [VPN Ağ Geçidi](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) kullanabilirsiniz.

Hub [ve kollu sanal ağların](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json)hub sanal ağa bağlandığı ve hub'ın şirket içi ağa bağlandığı hub ve kollu ağlar oluşturmak için eşleme ve VPN ağ geçidini birleştirebilirsiniz.

### <a name="name-resolution"></a>Ad çözümlemesi

Bir sanal ağdaki kaynaklar, Azure'un [yerleşik DNS'sini](virtual-networks-name-resolution-for-vms-and-role-instances.md)kullanarak eşlenen bir sanal ağdaki kaynakların adlarını çözemez. Eşli bir sanal ağdaki adları çözümlemek için [kendi DNS sunucunuzu dağıtın](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)veya Azure DNS [özel etki alanlarını](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)kullanın. Sanal ağdaki kaynaklar ve şirket içi ağlar arasındaki adları çözmek, kendi DNS sunucunuzu dağıtmanızı da gerektirir.

## <a name="permissions"></a>İzinler

Azure, kaynaklara [rol tabanlı erişim denetimi](../role-based-access-control/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (RBAC) kullanır. İzinler aşağıdaki hiyerarşideki bir [kapsama](../role-based-access-control/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#scope) atanır: yönetim grubu, abonelik, kaynak grubu ve tek tek kaynak. Hiyerarşi hakkında daha fazla bilgi edinmek için [kaynaklarınızı düzenle'ye](../azure-resource-manager/management-groups-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın. Azure sanal ağları ve eşleme, ağ güvenlik grupları, hizmet bitiş noktaları ve rota tabloları gibi ilgili tüm özellikleriyle çalışmak için, kuruluşunuzun üyelerini yerleşik [Sahibine](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#owner), [Katılımcıya](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#contributor)veya [Ağ katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rollerine atayabilir ve ardından rolü uygun kapsama atayabilirsiniz. Sanal ağ özelliklerinin bir alt kümesi için belirli izinler atamak, [özel](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) bir rol oluşturmak ve [sanal ağlar,](manage-virtual-network.md#permissions) [alt ağlar ve hizmet bitiş noktaları,](virtual-network-manage-subnet.md#permissions)ağ [arabirimleri,](virtual-network-network-interface.md#permissions) [eşleme,](virtual-network-manage-peering.md#permissions) [ağ ve uygulama güvenlik grupları](manage-network-security-group.md#permissions)veya rol için rota [tabloları](manage-route-table.md#permissions) için gerekli belirli izinleri atamak istiyorsanız.

## <a name="policy"></a>İlke

Azure İlkesi, ilke tanımları oluşturmanıza, atamanıza ve yönetmenize olanak tanır. İlke tanımları kaynaklarınız üzerinde farklı kurallar uygular, böylece kaynaklar kuruluş standartlarınızla ve hizmet düzeyi anlaşmalarınızla uyumlu kalır. Azure İlkesi, sahip olduğunuz ilke tanımlarıyla uyumlu olmayan kaynakları tarayarak kaynaklarınızın bir değerlendirmesini çalıştırıyor. Örneğin, yalnızca belirli bir kaynak grubunda veya bölgede sanal ağların oluşturulmasına izin veren bir ilke tanımlayabilir ve uygulayabilirsiniz. Başka bir ilke, her alt ağ ile ilişkili bir ağ güvenlik grubu olmasını gerektirebilir. İlkeler daha sonra kaynak oluşturulurken ve güncellerken değerlendirilir.

İlkeler aşağıdaki hiyerarşiye uygulanır: yönetim grubu, abonelik ve kaynak grubu. [Azure ilkesi](../governance/policy/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hakkında daha fazla bilgi edinin veya bazı sanal ağ [ilkesi şablonu](policy-samples.md) örneklerini dağıtın.

## <a name="next-steps"></a>Sonraki adımlar

[Sanal ağ,](manage-virtual-network.md) [alt ağ ve hizmet bitiş noktası,](virtual-network-manage-subnet.md) [ağ arabirimi,](virtual-network-network-interface.md) [bakanlık,](virtual-network-manage-peering.md) [ağ ve uygulama güvenlik grubu](manage-network-security-group.md)veya rota [tablosu](manage-route-table.md)için tüm görevler, ayarlar ve seçenekler hakkında bilgi edinin.
