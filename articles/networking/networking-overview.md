---
title: Azure ağ hizmetleri 'ne genel bakış
description: Azure 'da ağ hizmetleri ve bunların yeteneklerini bağlantı Hizmetleri, uygulama koruma hizmetleri, uygulama teslim hizmetleri ve ağ izleme hakkında bilgi edinin.
services: networking
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 03/12/2020
ms.author: kumud
ms.openlocfilehash: 42d3360b7defaab2ff0a62dc125a213860b13a6a
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82133606"
---
# <a name="azure-networking-services-overview"></a>Azure ağ hizmetleri 'ne genel bakış

Azure 'daki ağ hizmetleri, birlikte veya ayrı olarak kullanılabilecek çeşitli ağ özellikleri sağlar. Bunlarla ilgili daha fazla bilgi edinmek için aşağıdaki önemli yeteneklerden birine tıklayın:
- [**Bağlantı Hizmetleri**](#connect): Azure sanal ağ (VNet), sanal WAN, expressroute, VPN Gateway, sanal ağ NAT ağ geçidi, Azure DNS, eşleme hizmeti ve Azure savunma 'da bu ağ hizmetlerinin herhangi birini veya birleşimini kullanarak Azure kaynaklarını ve şirket içi kaynakları bağlayın.
- [**Uygulama koruma hizmetleri**](#protect) Azure-Private link, DDoS koruması, güvenlik duvarı, ağ güvenlik grupları, Web uygulaması güvenlik duvarı ve sanal ağ uç noktalarında bu ağ hizmetlerinin herhangi birini veya birleşimini kullanarak uygulamalarınızı koruyun.
- [**Uygulama Teslim Hizmetleri**](#deliver) Azure-Content Delivery Network (CDN), Azure ön kapı hizmeti, Traffic Manager, Application Gateway, Internet Çözümleyicisi ve Load Balancer bu ağ hizmetlerinin herhangi birini veya birleşimini kullanarak Azure ağı 'nda uygulamalar sunun.
- [**Ağ izleme**](#monitor) – Azure-ağ Izleyicisi, ExpressRoute Izleyicisi, Azure Izleyici veya VNET Terminal erişim noktası (TAP) ' de bu ağ hizmetlerinin herhangi birini veya birleşimini kullanarak ağ kaynaklarınızı izleyin.

## <a name="connectivity-services"></a><a name="connect"></a>Bağlantı hizmetleri
 
Bu bölümde Azure kaynakları, şirket içi ağdan Azure kaynaklarına bağlantı ve Azure sanal ağ (VNet), sanal WAN, ExpressRoute, VPN Gateway, sanal ağ NAT ağ geçidi, Azure DNS, Azure eşleme hizmeti ve Azure savunma ile şube bağlantısı arasında bağlantı sağlayan hizmetler açıklanmaktadır.

|Hizmet|Neden kullanılmalıdır?|Senaryolar|
|---|---|---|
|[Sanal ağ](#vnet)|Azure kaynaklarının birbirleriyle, internet ve şirket içi ağlarla güvenli bir şekilde iletişim kurmasına olanak sağlar.| <p>[Ağ trafiğini filtreleme](../virtual-network/tutorial-filter-network-traffic.md)</p> <p>[Ağ trafiğini yönlendirme](../virtual-network/tutorial-create-route-table-portal.md)</p> <p>[Kaynaklar için ağ erişimini kısıtlama](../virtual-network/tutorial-restrict-network-access-to-resources.md)</p> <p>[Sanal ağları bağlama](../virtual-network/tutorial-connect-virtual-networks-portal.md)</p>|
|[ExpressRoute](#expressroute)|Şirket içi ağlarınızı bir bağlantı sağlayıcısı tarafından kolaylaştırarak özel bir bağlantı üzerinden Microsoft bulutuna genişletir.|<p>[ExpressRoute bağlantı hattını oluşturma ve değiştirme](../expressroute/expressroute-howto-circuit-portal-resource-manager.md)</p> <p>[Bir ExpressRoute bağlantı hattı için eşlemeyi oluşturma ve değiştirme](../expressroute/expressroute-howto-routing-portal-resource-manager.md)</p> <p>[ExpressRoute bağlantı hattına bir Sanal Ağ bağlama](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md)</p> <p>[ExpressRoute devreleri için yol filtrelerini yapılandırma ve yönetme](../expressroute/how-to-routefilter-portal.md)</p>|
|[VPN Gateway](#vpngateway)|Ortak Internet üzerinden bir Azure sanal ağı ile şirket içi bir konum arasında şifrelenmiş trafik gönderir.|<p>[Siteler arası bağlantılar](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)</p> <p>[Sanal Ağdan Sanal Ağa bağlantılar](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)</p> <p>[Noktadan siteye bağlantılar](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)</p>|
|[Sanal WAN](#virtualwan)|Azure ile ve arasındaki dal bağlantısını iyileştirir ve otomatikleştirir. Azure bölgeleri, Dallarınızı bağlamak için seçebileceğiniz hub olarak görev yapar.|<p>[Siteden siteye bağlantılar](../virtual-wan/virtual-wan-site-to-site-portal.md), [ExpressRoute bağlantıları](../virtual-wan/virtual-wan-expressroute-portal.md)</p>|
|[Azure DNS](#dns)|Microsoft Azure altyapısını kullanarak ad çözümlemesi sağlayan DNS etki alanlarını barındırır.|<p>[Azure DNS’te etki alanınızı barındırma](../dns/dns-delegate-domain-azure-dns.md)</p><p>[Bir Web uygulaması için DNS kayıtları oluşturma](../dns/dns-web-sites-custom-domain.md)</p> <p>[Traffic Manager için bir diğer ad kaydı oluşturma](../dns/tutorial-alias-tm.md)</p> <p>[Genel IP adresi için bir diğer ad kaydı oluşturma](../dns/tutorial-alias-pip.md)</p> <p>[Bölge kaynak kaydı için bir diğer ad kaydı oluşturma](../dns/tutorial-alias-rr.md)</p>|
|[Azure Bastion](#bastion)|TLS üzerinden Azure portal doğrudan sanal makinelerinize güvenli ve sorunsuz RDP/SSH bağlantısı yapılandırın. Azure savunma aracılığıyla bağlandığınızda, sanal makinelerinizde ortak bir IP adresi gerekmez|<p>[Azure Bastion konağı oluşturma](../bastion/bastion-create-host-portal.md)</p><p>[Linux VM 'ye SSH kullanarak bağlanma](../bastion/bastion-connect-vm-ssh.md)</p><p>[Windows VM 'ye RDP kullanarak bağlanma](../bastion/bastion-connect-vm-rdp.md)</p>|
|[Sanal ağ NAT ağ geçidi](#nat)|Bir sanal makine için giden bağlantı sağlamak üzere bir NAT ağ geçidi oluşturun.|<p>[NAT ağ geçidi oluşturma](../virtual-network/quickstart-create-nat-gateway-portal.md)</p>|
|[Azure eşleme hizmeti (Önizleme)](#azurepeeringservice)|Genel ağ üzerinden Microsoft bulutuna en iyi ve güvenilir yönlendirme için hizmet sağlayıcılarıyla işbirliği yapın.|<p>[Azure eşleme hizmetini Kaydet](../peering-service/azure-portal.md)</p>|
||||


### <a name="virtual-network"></a><a name="vnet"></a>Sanal ağ

Azure sanal ağı (VNet), Azure 'daki özel ağınız için temel yapı taşdır. Bir sanal ağları kullanarak şunları yapabilirsiniz:
- **Azure kaynakları arasında Iletişim kurma**: VM 'leri ve diğer birçok Azure kaynağı türünü, Azure App Service ortamları, Azure Kubernetes hizmeti (aks) ve Azure sanal makine ölçek kümeleri gibi sanal bir ağa dağıtabilirsiniz. Bir sanal ağa dağıtabileceğiniz Azure kaynaklarının tam listesini görüntülemek için bkz. [Sanal ağ hizmeti tümleştirmesi](../virtual-network/virtual-network-for-azure-services.md).
- Birbirleriyle **Iletişim kurma**: sanal ağları birbirlerine bağlanarak sanal ağ eşleme kullanarak her iki sanal ağdaki kaynakların birbirleriyle iletişim kurmasına olanak sağlayabilirsiniz. Bağlandığınız sanal ağlar aynı veya farklı Azure bölgelerinde bulunabilir. Daha fazla bilgi için bkz. [sanal ağ eşlemesi](../virtual-network/virtual-network-peering-overview.md).
- **İnternet Ile Iletişim kurma**: bir sanal ağ içindeki tüm kaynaklar varsayılan olarak Internet ile giden iletişim kurabilir. Bir kaynağa genel IP adresi veya genel Load Balancer atayarak o kaynağa gelen yönde iletişim kurabilirsiniz. Giden bağlantılarınızı yönetmek için [genel IP adresleri](../virtual-network/virtual-network-public-ip-address.md) veya genel [Load Balancer](../load-balancer/load-balancer-overview.md) de kullanabilirsiniz.
- Şirket **içi ağlarla Iletişim kurma**: [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) veya [ExpressRoute](../expressroute/expressroute-introduction.md)kullanarak şirket içi bilgisayarlarınızı ve ağlarınızı sanal bir ağa bağlayabilirsiniz.

Daha fazla bilgi için bkz. [Azure sanal ağı nedir?](../virtual-network/virtual-networks-overview.md).

### <a name="expressroute"></a><a name="expressroute"></a>ExpressRoute
ExpressRoute, şirket içi ağlarınızı bir bağlantı sağlayıcısı tarafından kolaylaştırarak özel bir bağlantı üzerinden Microsoft bulutuna genişletmenizi sağlar. Bu bağlantı özeldir. Trafik, İnternet üzerinden geçmez. ExpressRoute'u kullanarak Microsoft Azure, Office 365 ve Dynamics 365 gibi Microsoft bulut hizmetleriyle bağlantı kurabilirsiniz.  Daha fazla bilgi için bkz. [ExpressRoute nedir?](../expressroute/expressroute-introduction.md).

![Azure ExpressRoute](./media/networking-overview/expressroute-connection-overview.png)

### <a name="vpn-gateway"></a><a name="vpngateway"></a>VPN Gateway
VPN Gateway, şirket içi konumlardan sanal ağınıza yönelik şifrelenmiş şirketler arası bağlantılar oluşturmanıza veya VNET 'ler arasında şifrelenmiş bağlantılar oluşturmanıza yardımcı olur. Siteden siteye, Noktadan siteye veya VNET 'ten VNet 'e kadar VPN Gateway bağlantı için kullanılabilir farklı yapılandırma vardır.
Aşağıdaki diyagramda aynı sanal ağa ait birden çok siteden siteye VPN bağlantısı gösterilmektedir.

![Siteden siteye Azure VPN Gateway bağlantıları](./media/networking-overview/vpngateway-multisite-connection-diagram.png)

Farklı türlerde VPN bağlantıları hakkında daha fazla bilgi için bkz. [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).

### <a name="virtual-wan"></a><a name="virtualwan"></a>Sanal WAN
Azure sanal WAN, Azure ile ve arasında iyileştirilmiş ve otomatik şube bağlantısı sağlayan bir ağ hizmetidir. Azure bölgeleri, Dallarınızı bağlamak için seçebileceğiniz hub olarak görev yapar. Aynı zamanda dalları bağlamak ve şube-VNet bağlantısının avantajlarından yararlanmak için Azure omurgasına sahip olabilirsiniz. Azure sanal WAN, tek bir işletim arabirimine siteden siteye VPN, ExpressRoute, Noktadan siteye kullanıcı VPN gibi birçok Azure bulut bağlantısı hizmeti sunar. Azure VNet bağlantısı, sanal ağ bağlantıları kullanılarak oluşturulur. Daha fazla bilgi için bkz. [Azure sanal WAN nedir?](../virtual-wan/virtual-wan-about.md).

![Sanal WAN diyagramı](./media/networking-overview/virtualwan1.png)

### <a name="azure-dns"></a><a name="dns"></a>Azure DNS
Azure DNS, DNS etki alanları için Microsoft Azure altyapısı kullanılarak ad çözümlemesi olanağı sağlayan bir hizmettir. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz. Daha fazla bilgi için bkz. [Azure DNS nedir?](../dns/dns-overview.md).

### <a name="azure-bastion"></a><a name="bastion"></a>Azure Bastion
Azure savunma hizmeti, sanal ağınızın içinde sağladığınız yeni, platform tarafından yönetilen yeni bir PaaS hizmetidir. Doğrudan TLS üzerinden Azure portal sanal makinelerinize güvenli ve sorunsuz RDP/SSH bağlantısı sağlar. Azure Bastion aracılığıyla bağlandığınızda, sanal makinelerinizin bir genel IP adresi olması gerekmez. Daha fazla bilgi için bkz. Azure savunma nedir [?](../bastion/bastion-overview.md).

![Azure savunma mimarisi](./media/networking-overview/architecture.png)

### <a name="virtual-network-nat-gateway"></a><a name="nat"></a>Sanal ağ NAT ağ geçidi
Sanal ağ NAT (ağ adresi çevirisi), sanal ağlar için yalnızca giden Internet bağlantısını basitleştirir. Bir alt ağda yapılandırıldığında, tüm giden bağlantılar belirttiğiniz statik genel IP adreslerini kullanır. Doğrudan sanal makinelere bağlı yük dengeleyici veya genel IP adresleri olmadan giden bağlantı kurulabilir. Daha fazla bilgi için bkz. [sanal ağ NAT ağ geçidi nedir?](../virtual-network/nat-overview.md) 

![Sanal ağ NAT ağ geçidi](./media/networking-overview/flow-map.png)

### <a name="azure-peering-service"></a><a name="azurepeeringservice"></a>Azure eşleme hizmeti
Azure eşleme hizmeti, Office 365, Dynamics 365, hizmet olarak yazılım (SaaS) Hizmetleri, Azure veya genel İnternet üzerinden erişilebilen herhangi bir Microsoft hizmeti gibi Microsoft bulut hizmetleriyle müşteri bağlantısını geliştirir. Daha fazla bilgi için bkz. [Azure eşleme hizmeti nedir?](../peering-service/about.md).

## <a name="application-protection-services"></a><a name="protect"></a>Uygulama koruma hizmetleri

Bu bölümde, Azure 'da ağ kaynaklarınızı korumaya yardımcı olan ağ hizmetleri açıklanmaktadır. Azure-Private bağlantısı, DDoS koruması, güvenlik duvarı, ağ güvenlik grupları, Web uygulaması güvenlik duvarı ve sanal ağ uç noktalarında bu ağ hizmetlerinin herhangi birini veya birleşimini kullanarak uygulamalarınızı koruyun.

|Hizmet|Neden kullanılmalıdır?|Senaryo|
|---|---|---|
|[DDoS koruması](#ddosprotection) |Fazla IP trafiği ücretlerinden koruma sayesinde uygulamalarınız için yüksek kullanılabilirlik|[Azure DDoS korumasını yönetme](../virtual-network/manage-ddos-protection.md)|
|[Web uygulaması güvenlik duvarı](#waf)|<p>[Application Gateway Ile Azure WAF](../web-application-firewall/ag/ag-overview.md) , ortak ve özel adres alanındaki varlıklara bölgesel koruma sağlar</p><p>[Ön kapılı Azure WAF](../web-application-firewall/afds/afds-overview.md) , ağ kenarından genel uç noktalara koruma sağlar.</p>|<p>[Bot koruma kurallarını yapılandırma](../frontdoor/waf-front-door-policy-configure-bot-protection.md)</p> <p>[Özel yanıt kodunu yapılandırma](../frontdoor/waf-front-door-configure-custom-response-code.md)</p> <p>[IP kısıtlama kurallarını yapılandırma](../frontdoor/waf-front-door-configure-ip-restriction.md)</p> <p>[Hız limiti kuralını Yapılandır](../frontdoor/waf-front-door-rate-limit-powershell.md)</p> |
|[Azure Güvenlik Duvarı](#firewall)|Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir. Yerleşik yüksek kullanılabilirliğe ve sınırsız bulut ölçeklenebilirliğine sahip, tam durum bilgisi olan bir hizmet olarak güvenlik duvarıdır.|<p>[VNET 'te Azure Güvenlik Duvarı dağıtma](../firewall/tutorial-firewall-deploy-portal.md)</p> <p>[-Azure Güvenlik duvarını karma ağda dağıtma](../firewall/tutorial-hybrid-ps.md)</p> <p>[Azure Güvenlik Duvarı ile gelen trafiği Filtreleme DNAT](../firewall/tutorial-firewall-dnat.md)</p>|
|[Ağ güvenlik grupları](#nsg)|Tüm ağ trafiği akışları için VM/alt ağdaki tam ayrıntılı dağıtılmış son düğüm denetimi|[Ağ güvenlik gruplarını kullanarak ağ trafiğini filtreleme](../virtual-network/tutorial-filter-network-traffic.md)|
|[Sanal ağ hizmet uç noktaları](#serviceendpoints)|Bazı Azure hizmet kaynaklarına ağ erişimini bir sanal ağ alt ağıyla sınırlamanıza olanak sağlar|[PaaS kaynaklarına ağ erişimini kısıtlama](../virtual-network/tutorial-restrict-network-access-to-resources-powershell.md)|
[Özel Bağlantı](#privatelink)|Azure PaaS hizmetlerine (örneğin, Azure depolama ve SQL veritabanı) ve Azure 'da barındırılan, müşteriye ait/iş ortağı hizmetlerini sanal ağınızdaki özel bir uç nokta üzerinden erişmenize izin vermez.|<p>[Özel uç nokta oluşturma](../private-link/create-private-endpoint-portal.md)</p><p>[Özel Bağlantı hizmeti oluşturma](../private-link/create-private-link-service-portal.md)</p>|
|||
### <a name="ddos-protection"></a><a name="ddosprotection"></a>DDOS Koruması 
[Azure DDoS koruması](../virtual-network/manage-ddos-protection.md) , en gelişmiş DDoS tehditlerine karşı karşı önlemler sağlar. Hizmet, uygulamanız ve sanal ağlarınızda dağıtılan kaynaklarınız için gelişmiş DDoS azaltma özellikleri sağlar. Ayrıca, Azure DDoS koruması kullanan müşterilerin, etkin bir saldırı sırasında DDoS uzmanlarını sağlamak için DDoS hızlı yanıt desteğine erişimi vardır.

![DDOS Koruması](./media/networking-overview/ddos-protection.png)

### <a name="web-application-firewall"></a><a name="waf"></a>Web uygulaması güvenlik duvarı

[Azure Web uygulaması güvenlik duvarı](../web-application-firewall/overview.md) (WAF), Web UYGULAMALARıNıZ için SQL ekleme ve siteler arası betik oluşturma gibi sık kullanılan Web açıklarından ve güvenlik açıklarına karşı koruma sağlar. Azure WAF, yönetilen kurallar aracılığıyla OWASP ilk 10 güvenlik açığına karşı kullanıma hazır koruma sağlar. Ayrıca müşteriler, kaynak IP aralığına dayalı ek koruma sağlamak için müşteri tarafından yönetilen kurallar ve üstbilgiler, tanımlama bilgileri, form veri alanları veya sorgu dizesi parametreleri gibi özel kurallar da yapılandırabilir.

Müşteriler, ortak ve özel adres alanındaki varlıklara bölgesel koruma sağlayan [Application Gateway Azure WAF](../application-gateway/waf-overview.md) 'yi dağıtmayı seçebilirler. Müşteriler Ayrıca, ağ ucunda genel uç noktalara koruma sağlayan [ön kapılı Azure WAF](../frontdoor/waf-overview.md) 'i dağıtmayı tercih edebilir.

![Web Uygulaması Güvenlik Duvarı](./media/networking-overview/waf-overview.png)


### <a name="azure-firewall"></a><a name="firewall"></a>Azure Güvenlik Duvarı
Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir. Azure Güvenlik Duvarı 'nı kullanarak, abonelikler ve sanal ağlar arasında merkezi olarak uygulama ve ağ bağlantısı ilkeleri oluşturabilir, uygulayabilir ve bunları günlüğe kaydedebilirsiniz. Azure Güvenlik Duvarı, dış güvenlik duvarlarının sanal ağınızdan kaynaklanan trafiği tanımlamasına olanak tanımak amacıyla sanal ağ kaynaklarınız için statik genel bir IP adresi kullanır. 

Azure Güvenlik Duvarı hakkında daha fazla bilgi için bkz. [Azure Güvenlik Duvarı belgeleri](../firewall/overview.md).

![Güvenlik duvarına genel bakış](./media/networking-overview/firewall-threat.png)

### <a name="network-security-groups"></a><a name="nsg"></a>Ağ güvenlik grupları
Ağ güvenlik grupları ile bir Azure sanal ağı içindeki Azure kaynaklarına gelen ve bu kaynaklardan giden ağ trafiğini filtreleyebilirsiniz. Daha fazla bilgi için bkz. [Güvenliğe genel bakış](../virtual-network/security-overview.md).

### <a name="service-endpoints"></a><a name="serviceendpoints"></a>Hizmet uç noktaları
Sanal Ağ hizmet uç noktaları, sanal ağ özel adres alanınızı ve sanal ağınızın kimliğini doğrudan bağlantı üzerinden Azure hizmetlerine genişletir. Uç noktalar kritik Azure hizmeti kaynaklarınızı sanal ağlarınızla sınırlayarak güvenliğini sağlamanıza imkan verir. Sanal ağınızdan Azure hizmetine giden trafik her zaman Microsoft Azure omurga ağında kalır. Daha fazla bilgi için bkz. [sanal ağ hizmeti uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md).

![Sanal ağ hizmet uç noktaları](./media/networking-overview/vnet-service-endpoints-overview.png)

### <a name="azure-private-link"></a><a name="privatelink"></a>Azure Özel Bağlantı
[Azure özel bağlantısı](../private-link/private-link-overview.md) , Azure PaaS hizmetlerine (örneğin, Azure depolama ve SQL veritabanı) ve Azure 'da barındırılan, müşteriye ait/iş ortağı hizmetlerine sanal ağınızdaki özel bir uç nokta üzerinden erişmenizi sağlar.
Sanal ağınız ve hizmet arasındaki trafik, Microsoft omurga ağını de dolaşır. Hizmetinizi genel İnternet 'e sunma artık gerekli değildir. Kendi özel bağlantı hizmetinizi sanal ağınızda oluşturabilir ve müşterilerinize iletebilirsiniz.

![Özel uç noktaya genel bakış](./media/networking-overview/private-endpoint.png)


## <a name="application-delivery-services"></a><a name="deliver"></a>Uygulama Teslim Hizmetleri

Bu bölümde, Azure 'da uygulamalar-ağ Izleyicisi, ExpressRoute Izleyicisi, Azure Izleyici veya VNet Terminal erişim noktası (TAP) sunmaya yardımcı olan ağ hizmetleri açıklanmaktadır.

|Hizmet|Neden kullanılmalıdır?|Senaryo|
|---|---|---|
|[Content Delivery Network](#cdn)|Kullanıcılara yüksek bant genişliğine sahip içerik sunar. CDNs, gecikme süresini en aza indirmek için son kullanıcılara yakın olan varlık (POP) konumları içinde önbelleğe alınmış içeriği uç sunucular üzerinde depolar|<p>[Web uygulamasına CDN ekleme](../cdn/cdn-add-to-web-app.md)</p> <p>[-HTTPS üzerinden Azure CDN özel bir etki alanı kullanarak depolama bloblarına erişin](..//cdn/cdn-storage-custom-domain-https.md)</p> <p>[Azure CDN uç noktanıza özel etki alanı ekleme](../cdn/cdn-map-content-to-custom-domain.md)</p> <p>[Azure CDN özel etki alanı üzerinde HTTPS yapılandırma](../cdn/cdn-custom-ssl.md?tabs=option-1-default-enable-https-with-a-cdn-managed-certificate)</p>|
|[Azure Front Door Hizmeti](#frontdoor)|Yüksek kullanılabilirlik için en iyi performansı ve hızlı genel yük devretmeyi iyileştirerek Web trafiğiniz için küresel yönlendirmeyi tanımlamanıza, yönetmenize ve izlemenize olanak sağlar.|<p>[Azure Front Door Service’inize özel etki alanı ekleme](../frontdoor/front-door-custom-domain.md)</p> <p>[Ön kapı özel etki alanında HTTPS 'yi yapılandırma](../frontdoor/front-door-custom-domain-https.md)</p><p>[Coğrafi filtreleme Web uygulaması güvenlik duvarı ilkesini ayarlama](../frontdoor/front-door-tutorial-geo-filtering.md)|
|[Traffic Manager](#trafficmanager)|Yüksek kullanılabilirlik ve yanıt hızı sağlarken, DNS tabanlı trafiği küresel Azure bölgelerindeki hizmetlere dağıtır|<p> [Düşük gecikme süresi için trafiği yönlendirme](../traffic-manager/tutorial-traffic-manager-improve-website-response.md)</p><p>[Trafiği bir öncelik uç noktasına yönlendirme](../traffic-manager/traffic-manager-configure-priority-routing-method.md)</p><p> [Ağırlıklı uç noktalarla trafiği yönetme](../traffic-manager/tutorial-traffic-manager-weighted-endpoint-routing.md)</p><p>[Uç noktanın coğrafi konumunu temel alarak trafiği yönlendirme](../traffic-manager/traffic-manager-configure-geographic-routing-method.md)</p> <p> [Kullanıcının alt ağına göre trafik yönlendirme](../traffic-manager/tutorial-traffic-manager-subnet-routing.md)</p>|
|[Load Balancer](#loadbalancer)|, Trafiği kullanılabilirlik alanları arasında ve sanal ağlarınız arasında yönlendirerek bölgesel yük dengelemesi sağlar. , Bölgesel uygulamanızı oluşturmak için kaynaklarınızın tamamında ve arasında trafiği yönlendirerek iç yük dengelemesi sağlar.|<p> [Sanal makinelerin internet trafiğinde yük dengeleme](../load-balancer/tutorial-load-balancer-standard-manage-portal.md)</p> <p>[Bir sanal ağ içindeki VM 'lerde Yük Dengeleme trafiği](../load-balancer/tutorial-load-balancer-basic-internal-portal.md)<p>[Belirli VM 'lerde belirli bir bağlantı noktasına giden trafik iletme trafiği](../load-balancer/tutorial-load-balancer-port-forwarding-portal.md)</p><p> [Yük dengelemeyi ve giden kuralları yapılandırma](../load-balancer/configure-load-balancer-outbound-cli.md)</p>|
|[Application Gateway](#applicationgateway)|Azure Application Gateway, web uygulamalarınıza trafiği yönetmenizi sağlayan bir web trafiği yük dengeleyicisidir.|<p>[Azure Application Gateway ile web trafiğini yönlendirme](../application-gateway/quick-create-portal.md)</p><p>[Öğretici: Azure portal kullanarak bir uygulama ağ geçidini TLS sonlandırmasıyla yapılandırma](../application-gateway/create-ssl-portal.md)</p><p>[URL yolu tabanlı yönlendirme ile bir uygulama ağ geçidi oluşturma](../application-gateway/create-url-route-portal.md) </p>|
|

### <a name="content-delivery-network"></a><a name="cdn"></a>Content Delivery Network
Azure Content Delivery Network (CDN), dünya genelindeki stratejik olarak yerleştirilen fiziksel düğümlerde içeriği önbelleğe alarak, yüksek bant genişliği içeriğinin hızlı bir şekilde kullanıcılara teslimi konusunda geliştiricilere genel bir çözüm sunar. Azure CDN hakkında daha fazla bilgi için bkz. [Azure Content Delivery Network](../cdn/cdn-overview.md)

![Azure CDN](./media/networking-overview/cdn-overview.png)

### <a name="azure-front-door-service"></a><a name="frontdoor"></a>Azure ön kapı hizmeti
Azure Front Door Service, yüksek kullanılabilirliğe yönelik en iyi performans ve anında genel yük devretme için iyileştirerek, web trafiğinizin genel yönlendirmesini tanımlamanızı, yönetmenizi ve izlemenizi sağlar. Front Door ile, genel (çok bölgeli) tüketici ve kurumsal uygulamalarınızı Azure ile küresel çapta bir hedef kitleye ulaşan sağlam, yüksek performanslı, kişiselleştirilmiş modern uygulamalara, API’lere ve içeriğe dönüştürebilirsiniz. Daha fazla bilgi için bkz. [Azure ön kapısı](../frontdoor/front-door-overview.md).


### <a name="traffic-manager"></a><a name="trafficmanager"></a>Traffic Manager

Azure Traffic Manager, trafiği farklı Azure bölgelerindeki hizmetlere en uygun şekilde dağıtırken yüksek kullanılabilirlik ve yanıtlama hızı sağlayan DNS tabanlı bir trafik yük dengeleyicidir. Traffic Manager öncelik, ağırlıklı, performans, coğrafi, çok değerli veya alt ağ gibi trafiği dağıtmak için bir dizi trafik yönlendirme yöntemi sağlar. Trafik yönlendirme yöntemleri hakkında daha fazla bilgi için bkz. [Traffic Manager yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md).

Aşağıdaki diyagramda Traffic Manager ile Endpoint Priority tabanlı yönlendirme gösterilmektedir:

![Azure Traffic Manager ' Priority ' trafiği yönlendirme yöntemi](./media/networking-overview/priority.png)

Traffic Manager hakkında daha fazla bilgi için bkz. [Azure Traffic Manager nedir?](../traffic-manager/traffic-manager-overview.md)

### <a name="load-balancer"></a><a name="loadbalancer"></a>Load Balancer
Azure Load Balancer, tüm UDP ve TCP protokolleri için yüksek performanslı ve düşük gecikmeli katman 4 yük dengelemesi sağlar. Gelen ve giden bağlantıları yönetir. Ortak ve iç yük dengeli uç noktaları yapılandırabilirsiniz. Hizmet kullanılabilirliğini yönetmek için TCP ve HTTP sistem durumu-yoklama seçeneklerini kullanarak, arka uç havuzu hedeflerine gelen bağlantıları eşlemek için kurallar tanımlayabilirsiniz. Load Balancer hakkında daha fazla bilgi edinmek için [Load Balancer genel bakış](../load-balancer/load-balancer-overview.md) makalesini okuyun.

Aşağıdaki resimde hem dış hem de iç yük dengeleyicileri kullanan Internet 'e yönelik çok katmanlı bir uygulama gösterilmektedir:

![Azure Load Balancer örneği](./media/networking-overview/load-balancer.png)


### <a name="application-gateway"></a><a name="applicationgateway"></a>Application Gateway
Azure Application Gateway, web uygulamalarınıza trafiği yönetmenizi sağlayan bir web trafiği yük dengeleyicisidir. Bu bir hizmet olarak uygulama teslim denetleyicisi (ADC), uygulamalarınız için farklı katman 7 Yük Dengeleme özelliği sunar. Daha fazla bilgi için bkz. [Azure Application Gateway nedir?](../application-gateway/overview.md).

Aşağıdaki diyagramda Application Gateway ile URL yolu tabanlı yönlendirme gösterilmektedir.

![Application Gateway örneği](./media/networking-overview/figure1-720.png)

## <a name="network-monitoring-services"></a><a name="monitor"></a>Ağ izleme hizmetleri
Bu bölümde, Azure 'da ağ kaynaklarınızı izlemeye, ExpressRoute Izleyicisinden, Azure Izleyicisinden ve sanal ağa DOKUNMANıZA yardımcı olan ağ hizmetleri açıklanmaktadır.

|Hizmet|Neden kullanılmalıdır?|Senaryo|
|---|---|---|
|[Ağ İzleyicisi](#networkwatcher)|Bağlantı sorunlarını izlemeye ve sorun gidermeye yardımcı olur, VPN, NSG ve yönlendirme sorunlarını tanılamaya yardımcı olur, sanal makinenizde paketleri yakalayıp Azure Işlevleri 'ni kullanarak tanılama araçlarını tetiklemeyi otomatikleştirir ve Logic Apps|<p>[VM trafik filtresi sorununu tanılama](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md)</p><p>[VM yönlendirme sorunlarını tanılama](../network-watcher/diagnose-vm-network-routing-problem.md)</p><p>[VM 'Ler arasındaki iletişimleri izleme](../network-watcher/connection-monitor.md)</p><p>[Ağlar arasındaki iletişim sorunlarını tanılama](../network-watcher/diagnose-communication-problem-between-networks.md)</p><p>[Bir VM’ye gelen ve VM’den giden ağ trafiğini günlüğe kaydetme](../network-watcher/network-watcher-nsg-flow-logging-portal.md)</p>|
|[ExpressRoute Izleyicisi](#expressroutemonitor)|Ağ performansının, kullanılabilirliğinin ve kullanımının gerçek zamanlı olarak izlenmesini sağlar, ağ topolojisinin otomatik olarak bulunmasına yardımcı olur, daha hızlı hata yalıtımı sağlar, geçici ağ sorunlarını algılar, geçmiş ağ performansı özelliklerini çözümlemeye yardımcı olur ve çoklu aboneliği destekler|<p>[ExpressRoute için Ağ Performansı İzleyicisi’ni Yapılandırma](../expressroute/how-to-npm.md)</p><p>[ExpressRoute izleme, ölçümler ve uyarılar](../expressroute/expressroute-monitoring-metrics-alerts.md)</p>|
|[Azure İzleyici](#azuremonitor)|Uygulamalarınızın nasıl çalıştığını ve bunları etkileyen sorunları ve bunların bağımlı oldukları kaynakları nasıl tanımladığını anlamanıza yardımcı olur.|<p>[Traffic Manager ölçümleri ve uyarıları](../traffic-manager/traffic-manager-metrics-alerts.md)</p><p>[Standart Load Balancer için Azure izleyici tanılaması](../load-balancer/load-balancer-standard-diagnostics.md)</p><p>[Azure Güvenlik Duvarı günlüklerini ve ölçümlerini izleme](../firewall/tutorial-diagnostics.md)</p><p>[Azure web uygulaması güvenlik duvarı izleme ve günlüğe kaydetme](../frontdoor/waf-front-door-monitor.md)</p>|
|[Sanal ağ dokunma](#vnettap)|Paket toplayıcısına sanal makine ağ trafiği sürekli akışı sağlar, ağ ve uygulama performansı yönetimi çözümlerini ve güvenlik analizi araçlarını sağlar|[VNet dokunma kaynağı oluşturma](../virtual-network/tutorial-tap-virtual-network-cli.md)|
|

### <a name="network-watcher"></a><a name="networkwatcher"></a>Ağ İzleyicisi
Azure Ağ İzleyicisi, Azure sanal ağındaki kaynaklarda izleme, tanılama, ölçümleri görüntüleme ve günlükleri etkinleştirme veya devre dışı bırakma işlemleri için araçlar sağlar. Daha fazla bilgi için bkz. [Ağ İzleyicisi nedir?](../network-watcher/network-watcher-monitoring-overview.md?toc=%2fazure%2fnetworking%2ftoc.json).
### <a name="expressroute-monitor"></a><a name="expressroutemonitor"></a>ExpressRoute Izleyicisi
ExpressRoute devre ölçümlerini, kaynak günlüklerini ve uyarılarını görüntüleme hakkında bilgi edinmek için bkz. [ExpressRoute izleme, ölçümler ve uyarılar](../expressroute/expressroute-monitoring-metrics-alerts.md?toc=%2fazure%2fnetworking%2ftoc.json).
### <a name="azure-monitor"></a><a name="azuremonitor"></a>Azure İzleyici
Azure İzleyici, bulut ve şirket içi ortamlardaki telemetriyi toplamak, analiz etmek ve bunlarda eylem gerçekleştirmek için kapsamlı bir çözüm sunarak uygulamalarınızın kullanılabilirliğini ve performansını en üst düzeye çıkarır. Uygulamalarınızın performansını anlamanıza ve uygulamalarla bağlı oldukları kaynakları etkileyen sorunları önceden tespit etmenize yardımcı olur. Daha fazla bilgi için bkz. [Azure Izleyiciye genel bakış](../azure-monitor/overview.md?toc=%2fazure%2fnetworking%2ftoc.json).
### <a name="virtual-network-tap"></a><a name="vnettap"></a>Sanal ağ dokunma
Azure sanal ağ TAP (Terminal erişim noktası), sanal makine ağ trafiğinizi bir ağ paketi toplayıcısına veya analiz aracına sürekli olarak akışla kullanmanıza olanak sağlar. Toplayıcı veya Analiz Aracı bir [ağ sanal gereç](https://azure.microsoft.com/solutions/network-appliances/) ortağı tarafından sağlanır. 

Aşağıdaki resimde, sanal ağ TAP 'ın nasıl çalıştığı gösterilmektedir. 

![Sanal ağ dokunma çalışma şekli](./media/networking-overview/virtual-network-tap-architecture.png)

Daha fazla bilgi için bkz. [sanal ağ dokunma nedir](../virtual-network/virtual-network-tap-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

- İlk [Sanal ağınızı oluşturun ve ilk sanal ağınızı oluşturma](../virtual-network/quick-create-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesindeki adımları tamamlayarak birkaç VM 'ye bağlanın.
- [Noktadan siteye bağlantı yapılandırma makalesindeki](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)adımları tamamlayarak bilgisayarınızı bir VNET 'e bağlayın.
- [İnternet 'e yönelik yük dengeleyici oluşturma](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesindeki adımları tamamlayarak, internet trafiğinin genel sunuculara yükünü dengeleyin.
 
 
   
