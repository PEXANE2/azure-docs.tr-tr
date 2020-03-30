---
title: Azure Sanal Ağı | Microsoft Docs
description: Azure Sanal Ağı kavramları ve özellikleri hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: anavinahar
tags: azure-resource-manager
Customer intent: As someone with a basic network background that is new to Azure, I want to understand the capabilities of Azure Virtual Network, so that my Azure resources such as VMs, can securely communicate with each other, the internet, and my on-premises resources.
ms.service: virtual-network
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2019
ms.author: anavin
ms.openlocfilehash: 3b908406c8717d2fa8834bc4dff1bcd27ec4761f
ms.sourcegitcommit: fe6c9a35e75da8a0ec8cea979f9dec81ce308c0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79241426"
---
# <a name="what-is-azure-virtual-network"></a>Azure Sanal Ağı nedir?

Azure Sanal Ağı (VNet), Azure'daki özel ağınızın temel yapı taşıdır. VNet, Azure Sanal Makineleri (VM) gibi birçok Azure kaynağının birbirleriyle, internetle ve şirket içi ağlarla güvenli bir şekilde iletişim kurmasını sağlar. VNet, kendi veri merkezinizde çalıştıracağınız geleneksel ağa benzer, ancak azure altyapısının ölçek, kullanılabilirlik ve yalıtım gibi ek avantajlarını da beraberinde getirir.

## <a name="vnet-concepts"></a>VNet kavramları

- **Adres alanı:** Bir VNet oluştururken, ortak ve özel (RFC 1918) adreslerini kullanarak özel bir özel IP adresi alanı belirtmeniz gerekir. Azure, bir sanal ağdaki kaynaklara, atadığınız adres alanından özel bir IP adresi atar. Örneğin, 10.0.0.0/16 adresindeki bir VNet'e VM dağıtırsanız, VM'ye 10.0.0.4 gibi özel bir IP atanır.
- **Alt ağlar:** Alt ağlar, sanal ağı bir veya daha fazla alt ağa bölmenize ve sanal ağın adres alanının bir bölümünü her alt ağa ayırmanıza olanak tanır. Daha sonra Azure kaynaklarını belirli bir alt ağda dağıtabilirsiniz. Geleneksel bir ağda olduğu gibi, alt ağlar da VNet adres alanınızı kuruluşun iç ağına uygun segmentlere bölmenize olanak tanır. Bu aynı zamanda adres tahsisverimliliğini artırır. Ağ Güvenlik Grupları'nı kullanarak alt ağlardaki kaynakları güvence altına alabilirsiniz. Daha fazla bilgi için [Güvenlik gruplarına](security-overview.md)bakın.
- **Bölgeler**: VNet tek bir bölge/konum kapsamındadır; ancak, farklı bölgelerden birden fazla sanal ağ Sanal Ağ Eşleme kullanarak birbirine bağlanabilir.
- **Abonelik:** VNet bir abonelik kapsamındadır. Her Azure [aboneliğinde](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) ve Azure [bölgesinde](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region) birden çok sanal ağ uygulayabilirsiniz.

## <a name="best-practices"></a>En iyi uygulamalar

Ağınızı Azure'da oluştururken, aşağıdaki evrensel tasarım ilkelerini göz önünde bulundurmanız önemlidir:

- Çakışmayan adres boşlukları sağlayın. VNet adres alanınızın (CIDR bloğu) kuruluşunuzun diğer ağ aralıklarıyla örtüşmediğinden emin olun.
- Alt ağlarınız VNet'in tüm adres alanını kapsamamalıdır. Önceden planlayın ve gelecek için bazı adres alanı ayırın.
- Birden çok küçük VNets daha az büyük VNets olması önerilir. Bu, yönetim yükü önleyecektir.
- Ağ Güvenlik Grupları (NSG' ler) alt ağlarına atayarak VNet'lerinizi güvence altına alayın.

## <a name="communicate-with-the-internet"></a>İnternet ile iletişim kurma

Bir VNet'teki tüm kaynaklar varsayılan olarak internete giden iletişim kurabilir. Bir kaynağa genel IP adresi veya genel Load Balancer atayarak o kaynağa gelen yönde iletişim kurabilirsiniz. Giden bağlantılarınızı yönetmek için genel IP adresi veya genel Load Balancer da kullanabilirsiniz.  Azure'daki giden bağlantılar hakkında daha fazla bilgi edinmek için bkz. [Giden bağlantılar](../load-balancer/load-balancer-outbound-connections.md), [Genel IP adresleri](virtual-network-public-ip-address.md) ve [Load Balancer](../load-balancer/load-balancer-overview.md).

>[!NOTE]
>Yalnızca sistem içi [Standart Load Balancer](../load-balancer/load-balancer-standard-overview.md) kullanıldığında [giden bağlantıların](../load-balancer/load-balancer-outbound-connections.md) örnek düzeyinde genel IP veya genel Load Balancer ile nasıl çalışacağını tanımlamadığınız sürece giden bağlantı kullanılamaz.

## <a name="communicate-between-azure-resources"></a>Azure kaynakları arasında iletişim kurma

Azure kaynakları, aşağıdaki yöntemlerden birini uygulayarak birbiriyle güvenli şekilde iletişim kurar:

- **Bir sanal ağ üzerinden**: Azure App Service Ortamları, Azure Kubernetes Service (AKS) ve Azure Sanal Makine Ölçek Kümeleri gibi sanal makineleri ve diğer birçok türde Azure kaynağını bir sanal ağa dağıtabilirsiniz. Bir sanal ağa dağıtabileceğiniz Azure kaynaklarının tam listesini görüntülemek için bkz. [Sanal ağ hizmeti tümleştirmesi](virtual-network-for-azure-services.md).
- **Sanal ağ hizmeti bitiş noktası aracılığıyla**: Sanal ağ özel adres alanınızı ve sanal ağınızın kimliğini doğrudan bağlantı üzerinden Azure Depolama hesapları ve Azure SQL veritabanları gibi Azure hizmet kaynaklarına genişletin. Hizmet uç noktaları, kritik Azure hizmeti kaynaklarınızı yalnızca bir sanal ağla sınırlayarak güvenliğini sağlamanıza imkan verir. Daha fazla bilgi için bkz. [Sanal ağ hizmet uç noktalarına genel bakış](virtual-network-service-endpoints-overview.md).
- **VNet Peering :** Sanal ağ ları birbirine bağlayarak sanal ağdaki kaynakların birbirleriyle iletişim kurmasını sağlayabilirsiniz. Bağlandığınız sanal ağlar aynı veya farklı Azure bölgelerinde bulunabilir. Daha fazla bilgi edinmek için bkz. [Sanal ağ eşlemesi](virtual-network-peering-overview.md).

## <a name="communicate-with-on-premises-resources"></a>Şirket içi kaynaklarla iletişim kurma

Aşağıdaki seçenekleri bir arada kullanarak şirket içi bilgisayarlarınızı ve ağlarınızı bir sanal ağa bağlayabilirsiniz:

- **Noktadan siteye sanal özel ağ (VPN):** Ağınızdaki tek bir bilgisayar ile sanal ağ arasında oluşur. Bir sanal ağ ile bağlantı kurmak isteyen her bilgisayar, bağlantısını yapılandırmalıdır. Azure’ı kullanmaya yeni başladıysanız bu bağlantı türü mükemmeldir. Mevcut ağınız üzerinde çok az bir değişiklik gerektirdiğinden veya hiç değişiklik gerektirmediğinden geliştiriciler için de mükemmeldir. Bilgisayarınız ile bir sanal ağ arasındaki iletişim, İnternet üzerinden şifrelenmiş bir tünel aracılığıyla gönderilir. Daha fazla bilgi için bkz. [Noktadan siteye VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#P2S).
- **Siteden siteye VPN:** Şirket içi VPN cihazınız ile bir sanal ağda dağıtılan Azure VPN Gateway arasında oluşur. Bu bağlantı türü, yetkilendirdiğiniz şirket içi kaynakların bir sanal ağa erişmesini sağlar. Şirket içi VPN cihazınız ile Azure VPN ağ geçidi arasındaki iletişim, İnternet üzerinden şifrelenmiş bir tünel aracılığıyla gönderilir. Daha fazla bilgi için bkz. [Siteden siteye VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti).
- **Azure ExpressRoute:** Bir ExpressRoute iş ortağı aracılığıyla ağınız ile Azure arasında oluşur. Bu bağlantı özeldir. Trafik, İnternet üzerinden geçmez. Daha fazla bilgi edinmek için bkz. [ExpressRoute](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#ExpressRoute).

## <a name="filter-network-traffic"></a>Ağ trafiğini filtreleme

Aşağıdaki seçeneklerden birini veya her ikisini de kullanarak alt ağlar arasındaki ağ trafiğini filtreleyebilirsiniz:

- **Güvenlik grupları:** Ağ güvenlik grupları ve uygulama güvenlik grupları, kaynak ve hedef IP adresi, bağlantı noktası ve protokole göre kaynaklara gelen ve gelen trafiği filtrelemenizi sağlayan birden çok gelen ve giden güvenlik kuralı içerebilir. Daha fazla bilgi edinmek için [Ağ güvenlik gruplarına](security-overview.md#network-security-groups) veya Uygulama güvenlik [gruplarına](security-overview.md#application-security-groups)bakın.
- **Ağ sanal gereçleri:** Ağ sanal gereci; güvenlik duvarı, WAN iyileştirmesi veya diğer ağ işlevi gibi ağ işlevlerini gerçekleştiren bir sanal makinedir. Bir sanal ağda dağıtabileceğiniz kullanılabilir ağ sanal gereçleri listesini görüntülemek için bkz. [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances).

## <a name="route-network-traffic"></a>Ağ trafiğini yönlendirme

Azure varsayılan olarak alt ağlar, bağlı sanal ağlar, şirket içi ağlar ve İnternet arasındaki trafiği yönlendirir. Azure’ın oluşturduğu varsayılan rotaları geçersiz kılmak için aşağıdaki seçeneklerden birini veya her ikisini uygulayabilirsiniz:

- **Rota tabloları:** Her bir alt ağ için trafiğin nereye yönlendirileceğini denetleyen rotalarla özel rota tabloları oluşturabilirsiniz. [Rota tabloları](virtual-networks-udr-overview.md#user-defined) hakkında daha fazla bilgi edinin.
- **Sınır ağ geçidi protokolü (BGP) rotaları:** Azure VPN Gateway veya ExpressRoute bağlantısı kullanarak sanal ağınızı şirket içi ağınıza bağlarsanız, şirket içi BGP rotalarınızı sanal ağlarınıza yayabilirsiniz. [Azure VPN Gateway](../vpn-gateway/vpn-gateway-bgp-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [ExpressRoute](../expressroute/expressroute-routing.md?toc=%2fazure%2fvirtual-network%2ftoc.json#dynamic-route-exchange) ile BGP’yi kullanma hakkında daha fazla bilgi edinin.

## <a name="azure-vnet-limits"></a>Azure VNet sınırları

Dağıtabileceğiniz Azure kaynaklarının sayısıyla ilgili belirli sınırlar vardır. Azure ağ sınırlarının çoğu maksimum değerlerdedir. Ancak, [VNet sınırları sayfasında](../azure-resource-manager/management/azure-subscription-service-limits.md#networking-limits)belirtildiği gibi [belirli ağ sınırlarını artırabilirsiniz.](../azure-portal/supportability/networking-quota-requests.md) 

## <a name="pricing"></a>Fiyatlandırma

Azure VNet kullanmak ücretsizdir, ücretsizdir. Sanal Makineler (VM' ler) ve diğer ürünler gibi kaynaklar için standart ücretler geçerlidir. Daha fazla bilgi için [VNet fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-network/) ve Azure [fiyatlandırma hesap layıcısına](https://azure.microsoft.com/pricing/calculator/)bakın.

## <a name="next-steps"></a>Sonraki adımlar

 Bir sanal ağı kullanmaya başlamak için bir sanal ağ oluşturun, bu sanal ağa birkaç sanal makine dağıtın ve sanal makineler arasında iletişim kurun. Nasıl olduğunu öğrenmek için [Sanal ağ oluşturma](quick-create-portal.md) başlıklı hızlı başlangıca bakın.
