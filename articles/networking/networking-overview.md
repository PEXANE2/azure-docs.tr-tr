---
title: Azure ağ hizmetlerine genel bakış
description: Azure'daki ağ hizmetleri ve bunların özellikleri hakkında bilgi edinin - bağlantı hizmetleri, uygulama koruma hizmetleri, uygulama teslim hizmetleri ve ağ izleme.
services: networking
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.workload: infrastructure-services
ms.date: 03/12/2020
ms.author: kumud
ms.openlocfilehash: 90eddea839d2f6ae5235ac6a391b40dd667ab401
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80257844"
---
# <a name="azure-networking-services-overview"></a>Azure ağ hizmetlerine genel bakış

Azure'daki ağ hizmetleri, birlikte veya ayrı ayrı kullanılabilecek çeşitli ağ özellikleri sağlar. Bunlar hakkında daha fazla bilgi edinmek için aşağıdaki anahtar özelliklerinden herhangi birini tıklatın:
- [**Bağlantı hizmetleri**](#connect): Azure ' daki bu ağ hizmetlerinin herhangi birini veya birleşimini kullanarak Azure kaynaklarını ve şirket içi kaynakları bağlayın - Sanal Ağ (VNet), Virtual WAN, ExpressRoute, VPN Ağ Geçidi, Sanal ağ NAT Ağ Geçidi, Azure DNS, Peering hizmeti ve Azure Kalesi.
- [**Uygulama koruma hizmetleri**](#protect) Azure'daki bu ağ hizmetlerinin herhangi birini veya bir birleşimini kullanarak uygulamalarınızı koruyun - Private Link, DDoS koruması, Güvenlik Duvarı, Ağ Güvenlik Grupları, Web Uygulaması Güvenlik Duvarı ve Sanal Ağ Bitiş Noktaları.
- [**Uygulama teslim hizmetleri**](#deliver) Azure - İçerik Dağıtım Ağı (CDN), Azure Ön Kapı Hizmeti, Trafik Yöneticisi, Uygulama Ağ Geçidi, Internet Analyzer ve Yük Bakiyesi'nde bu ağ hizmetlerinin herhangi birini veya bir birleşimini kullanarak Azure ağında uygulamalar sunun.
- [**Ağ izleme**](#monitor) - Azure'daki bu ağ hizmetlerinin herhangi birini veya bir birleşimini kullanarak ağ kaynaklarınızı izleyin - Ağ İzleyicisi, ExpressRoute Monitor, Azure Monitörü veya VNet Terminal Erişim Noktası (TAP).

## <a name="connectivity-services"></a><a name="connect"></a>Bağlantı hizmetleri
 
Bu bölümde, Azure kaynakları arasında bağlantı sağlayan hizmetler, şirket içi ağdan Azure kaynaklarına bağlantı ve Azure - Virtual Network (VNet), Virtual WAN, ExpressRoute, VPN Ağ Geçidi'nde daldan şubeye bağlantı, Sanal ağ NAT Ağ Geçidi, Azure DNS, Azure Peering hizmeti ve Azure Bastion.

|Hizmet|Neden kullansın ki?|Senaryolar|
|---|---|---|
|[Sanal ağ](#vnet)|Azure kaynaklarının birbirleriyle, internetle ve şirket içi ağlarla güvenli bir şekilde iletişim kurmasını sağlar.| <p>[Ağ trafiğini filtreleme](../virtual-network/tutorial-filter-network-traffic.md)</p> <p>[Ağ trafiğini yönlendirme](../virtual-network/tutorial-create-route-table-portal.md)</p> <p>[Kaynaklar için ağ erişimini kısıtlama](../virtual-network/tutorial-restrict-network-access-to-resources.md)</p> <p>[Sanal ağları bağlama](../virtual-network/tutorial-connect-virtual-networks-portal.md)</p>|
|[Ekspres Rota](#expressroute)|Bir bağlantı sağlayıcısı tarafından kolaylaştırılan özel bir bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletir.|<p>[ExpressRoute bağlantı hattı oluşturma ve değiştirme](../expressroute/expressroute-howto-circuit-portal-resource-manager.md)</p> <p>[Bir ExpressRoute bağlantı hattı için eşlemeyi oluşturma ve değiştirme](../expressroute/expressroute-howto-routing-portal-resource-manager.md)</p> <p>[ExpressRoute bağlantı hattına bir Sanal Ağ bağlama](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md)</p> <p>[ExpressRoute devreleri için rota filtrelerini yapılandırma ve yönetme](../expressroute/how-to-routefilter-portal.md)</p>|
|[VPN Gateway](#vpngateway)|Azure sanal ağı ile genel Internet üzerinden şirket içi konum arasında şifreli trafik gönderir.|<p>[Siteden siteye bağlantılar](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)</p> <p>[VNet-to-VNet bağlantıları](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)</p> <p>[Noktadan siteye bağlantılar](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md)</p>|
|[Sanal WAN](#virtualwan)|Azure'a ve Azure'a şube bağlantısını optimize eder ve otomatikleştirir. Azure bölgeleri, dallarınızı bağlamayı seçebileceğiniz hub'lar olarak hizmet vermektedir.|<p>[Siteden siteye bağlantılar,](../virtual-wan/virtual-wan-site-to-site-portal.md) [ExpressRoute bağlantıları](../virtual-wan/virtual-wan-expressroute-portal.md)</p>|
|[Azure DNS](#dns)|Microsoft Azure altyapılarını kullanarak ad çözümlemesi sağlayan DNS etki alanlarını barındırAbilir.|<p>[Azure DNS’te etki alanınızı barındırma](../dns/dns-delegate-domain-azure-dns.md)</p><p>[Bir web uygulaması için DNS kayıtları oluşturma](../dns/dns-web-sites-custom-domain.md)</p> <p>[Trafik Yöneticisi için takma ad kaydı oluşturma](../dns/tutorial-alias-tm.md)</p> <p>[Genel IP Adresi için takma ad kaydı oluşturma](../dns/tutorial-alias-pip.md)</p> <p>[Bölge kaynak kaydı için takma ad kaydı oluşturma](../dns/tutorial-alias-rr.md)</p>|
|[Azure Bastion](#bastion)|Doğrudan Azure portalda SSL üzerinden sanal makinelere güvenli ve sorunsuz bir şekilde RDP/SSH bağlantısı yapılandırın. Azure Bastion üzerinden bağlandığınızda, sanal makinelerinizin genel bir IP adresine ihtiyacı yoktur|<p>[Azure Kalesi ana bilgisayar oluşturma](../bastion/bastion-create-host-portal.md)</p><p>[SSH kullanarak Linux VM'ye bağlanma](../bastion/bastion-connect-vm-ssh.md)</p><p>[RDP kullanarak Windows VM'ye bağlanma](../bastion/bastion-connect-vm-rdp.md)</p>|
|[Sanal ağ NAT Ağ Geçidi](#nat)|Sanal bir makine için giden bağlantı sağlamak için bir NAT ağ geçidi oluşturun.|<p>[NAT Ağ Geçidi Oluşturma](../virtual-network/quickstart-create-nat-gateway-portal.md)</p>|
|[Azure Bakma Hizmeti (Önizleme)](#azurepeeringservice)|Genel ağ üzerinden Microsoft bulutuna en iyi ve güvenilir yönlendirme için hizmet sağlayıcılarla işbirliği yapın.|<p>[Azure Bakma Hizmetini Kaydedin](../peering-service/azure-portal.md)</p>|
||||


### <a name="virtual-network"></a><a name="vnet"></a>Sanal ağ

Azure Sanal Ağı (VNet), Azure'daki özel ağınızın temel yapı taşıdır. Bir VNets kullanabilirsiniz:
- **Azure kaynakları arasında iletişim**: Sanal Kaynaklar ve azure uygulama kaynaklarının diğer türleri azure Uygulama Hizmet Ortamları, Azure Kubernetes Hizmeti (AKS) ve Azure Sanal Makine Ölçek Kümeleri gibi sanal ağa dağıtabilirsiniz. Bir sanal ağa dağıtabileceğiniz Azure kaynaklarının tam listesini görüntülemek için bkz. [Sanal ağ hizmeti tümleştirmesi](../virtual-network/virtual-network-for-azure-services.md).
- **Birbirleri arasında iletişim**: Sanal ağ ları birbirine bağlayabilir, sanal ağdaki kaynakların birbirleriyle iletişim kurmasını sağlayarak sanal ağ eşlemesini sağlayabilirsiniz. Bağlandığınız sanal ağlar aynı veya farklı Azure bölgelerinde bulunabilir. Daha fazla bilgi için [Sanal ağ eşleme'ye](../virtual-network/virtual-network-peering-overview.md)bakın.
- **İnternete iletişim**: Bir VNet'teki tüm kaynaklar varsayılan olarak internete giden iletişim kurabilir. Bir kaynağa genel IP adresi veya genel Load Balancer atayarak o kaynağa gelen yönde iletişim kurabilirsiniz. Giden bağlantılarınızı yönetmek için [Ortak IP adreslerini](../virtual-network/virtual-network-public-ip-address.md) veya ortak [Yük Dengeleyicisini](../load-balancer/load-balancer-overview.md) de kullanabilirsiniz.
- **Şirket içi ağlarla iletişim :** Şirket içi bilgisayarlarınızı ve ağlarınızı [VPN Ağ Geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md) veya [ExpressRoute'u](../expressroute/expressroute-introduction.md)kullanarak sanal ağa bağlayabilirsiniz.

Daha fazla bilgi için [bkz: Azure Sanal Ağ nedir?](../virtual-network/virtual-networks-overview.md)

### <a name="expressroute"></a><a name="expressroute"></a>ExpressRoute
ExpressRoute, bir bağlantı sağlayıcısı tarafından kolaylaştırılan özel bir bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenize olanak tanır. Bu bağlantı özeldir. Trafik, İnternet üzerinden geçmez. ExpressRoute'u kullanarak Microsoft Azure, Office 365 ve Dynamics 365 gibi Microsoft bulut hizmetleriyle bağlantı kurabilirsiniz.  Daha fazla bilgi için [ExpressRoute nedir?](../expressroute/expressroute-introduction.md)

![Azure ExpressRoute](./media/networking-overview/expressroute-connection-overview.png)

### <a name="vpn-gateway"></a><a name="vpngateway"></a>VPN Gateway
VPN Ağ Geçidi, şirket içi konumlardan sanal ağınıza şifreli binalar arası bağlantılar oluşturmanıza veya VNets arasında şifreli bağlantılar oluşturmanıza yardımcı olur. VPN Ağ Geçidi bağlantıları için siteden siteye, noktadan siteye veya VNet'den VNet'e farklı yapılandırmalar vardır.
Aşağıdaki diyagram, aynı sanal ağa birden çok site-to-site VPN bağlantısı gösterir.

![Siteden Siteye Azure VPN Ağ Geçidi bağlantıları](./media/networking-overview/vpngateway-multisite-connection-diagram.png)

Vpn bağlantılarının farklı türleri hakkında daha fazla bilgi için [VPN Ağ Geçidi'ne](../vpn-gateway/vpn-gateway-about-vpngateways.md)bakın.

### <a name="virtual-wan"></a><a name="virtualwan"></a>Sanal WAN
Azure Virtual WAN, Azure'a ve Azure'a optimize edilmiş ve otomatikleştirilmiş dal bağlantısı sağlayan bir ağ hizmetidir. Azure bölgeleri, dallarınızı bağlamayı seçebileceğiniz hub'lar olarak hizmet vermektedir. Dalları bağlamak ve şubeden VNet'e bağlantının keyfini çıkarmak için Azure omurgasını kullanabilirsin. Azure Virtual WAN, siteden siteye VPN, ExpressRoute, noktadan siteye kullanıcı VPN'i gibi birçok Azure bulut bağlantı hizmetlerini tek bir operasyonel arabirimde bir araya getirir. Azure VNet'lere bağlantı sanal ağ bağlantıları kullanılarak oluşturulur. Daha fazla bilgi için [bkz: Azure sanal WAN nedir?](../virtual-wan/virtual-wan-about.md)

![Sanal WAN diyagramı](./media/networking-overview/virtualwan1.png)

### <a name="azure-dns"></a><a name="dns"></a>Azure DNS
Azure DNS, DNS etki alanları için Microsoft Azure altyapısı kullanılarak ad çözümlemesi olanağı sağlayan bir hizmettir. Etki alanlarınızı Azure'da barındırarak DNS kayıtlarınızı diğer Azure hizmetlerinde kullandığınız kimlik bilgileri, API’ler, araçlar ve faturalarla yönetebilirsiniz. Daha fazla bilgi için Azure [DNS nedir?](../dns/dns-overview.md)

### <a name="azure-bastion"></a><a name="bastion"></a>Azure Bastion
Azure Bastion hizmeti, sanal ağınızda sağladığınız tam platform tarafından yönetilen yeni bir PaaS hizmetidir. Sanal makinelerinize doğrudan SSL üzerinden Azure portalında güvenli ve sorunsuz RDP/SSH bağlantısı sağlar. Azure Bastion aracılığıyla bağlandığınızda, sanal makinelerinizin bir genel IP adresi olması gerekmez. Daha fazla bilgi için [bkz: Azure Kalesi nedir?](../bastion/bastion-overview.md)

![Azure Bastion mimarisi](./media/networking-overview/architecture.png)

### <a name="virtual-network-nat-gateway"></a><a name="nat"></a>Sanal ağ NAT Ağ Geçidi
Sanal Ağ NAT (ağ adresi çevirisi) sanal ağlar için giden yalnızca Internet bağlantısını basitleştirir. Bir alt ağ üzerinde yapılandırıldığında, tüm giden bağlantı belirtilen statik genel IP adreslerini kullanır. Giden bağlantı, yük dengeleyicisi veya sanal makinelere doğrudan bağlı genel IP adresleri olmadan mümkündür. Daha fazla bilgi için sanal [ağ NAT ağ geçidi nedir?](../virtual-network/nat-overview.md) 

![Sanal ağ NAT ağ geçidi](./media/networking-overview/flow-map.png)

### <a name="azure-peering-service"></a><a name="azurepeeringservice"></a>Azure Peering Hizmeti
Azure Peering hizmeti, Office 365, Dynamics 365, hizmet olarak yazılım (SaaS) hizmetleri, Azure veya herkese açık internet üzerinden erişilebilen microsoft hizmetleri gibi Microsoft bulut hizmetlerine müşteri bağlantısını geliştirir. Daha fazla bilgi için [bkz: Azure Bakma Hizmeti nedir?](../peering-service/about.md)

## <a name="application-protection-services"></a><a name="protect"></a>Uygulama koruma hizmetleri

Bu bölümde, Azure'daki ağ kaynaklarınızın korunmasına yardımcı olan ağ hizmetleri açıklanır - Azure'daki bu ağ hizmetlerinin herhangi birini veya birleşimini kullanarak uygulamalarınızı koruyun - Özel Bağlantı, DDoS koruması, Güvenlik Duvarı, Ağ Güvenlik Grupları, Web Uygulama Güvenlik Duvarı ve Sanal Ağ Bitiş Noktaları.

|Hizmet|Neden kullansın ki?|Senaryo|
|---|---|---|
|[DDoS koruması](#ddosprotection) |Aşırı IP trafik ücretlerine karşı koruma ile uygulamalarınız için yüksek kullanılabilirlik|[Azure DDoS Korumayı Yönetme](../virtual-network/manage-ddos-protection.md)|
|[Web Uygulama Güvenlik Duvarı](#waf)|<p>[Uygulama Ağ Geçidi ile Azure WAF,](../web-application-firewall/ag/ag-overview.md) ortak ve özel adres alanındaki varlıklara bölgesel koruma sağlar</p><p>[Ön Kapılı Azure WAF,](../web-application-firewall/afds/afds-overview.md) ağ kenarından ortak uç noktalara koruma sağlar.</p>|<p>[Bot koruma kurallarını yapılandırma](../frontdoor/waf-front-door-policy-configure-bot-protection.md)</p> <p>[Özel yanıt kodunu yapılandırma](../frontdoor/waf-front-door-configure-custom-response-code.md)</p> <p>[IP kısıtlama kurallarını yapılandırma](../frontdoor/waf-front-door-configure-ip-restriction.md)</p> <p>[Oranı sınır kuralını yapılandırma](../frontdoor/waf-front-door-rate-limit-powershell.md)</p> |
|[Azure Güvenlik Duvarı](#firewall)|Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir. Yerleşik yüksek kullanılabilirliğe ve sınırsız bulut ölçeklenebilirliğine sahip, tam durum bilgisi olan bir hizmet olarak güvenlik duvarıdır.|<p>[Vnet'te Azure Güvenlik Duvarı dağıtma](../firewall/tutorial-firewall-deploy-portal.md)</p> <p>[- Karma ağda Azure Güvenlik Duvarı dağıtma](../firewall/tutorial-hybrid-ps.md)</p> <p>[Azure Güvenlik Duvarı DNAT ile gelen trafiği filtreleme](../firewall/tutorial-firewall-dnat.md)</p>|
|[Ağ güvenlik grupları](#nsg)|Tüm ağ trafiği akışları için VM/subnet'te tam parçalı dağıtılmış uç düğümü kontrolü|[Ağ güvenlik gruplarını kullanarak ağ trafiğini filtreleme](../virtual-network/tutorial-filter-network-traffic.md)|
|[Sanal ağ hizmet uç noktaları](#serviceendpoints)|Bazı Azure hizmet kaynaklarına ağ erişimini sanal ağ alt ağıyla sınırlamanızı sağlar|[PaaS kaynaklarına ağ erişimini kısıtlama](../virtual-network/tutorial-restrict-network-access-to-resources-powershell.md)|
[Özel Bağlantı](#privatelink)|Azure PaaS Hizmetlerine (örneğin, Azure Depolama ve SQL Veritabanı) erişmenizi ve Azure'un sanal ağınızdaki özel bir bitiş noktası üzerinden müşteriye ait/iş ortağı hizmetlerine erişmenizi sağlar.|<p>[Özel uç nokta oluşturma](../private-link/create-private-endpoint-portal.md)</p><p>[Özel Bağlantı hizmeti oluşturma](../private-link/create-private-link-service-portal.md)</p>|
|||
### <a name="ddos-protection"></a><a name="ddosprotection"></a>DDoS Koruması 
[Azure DDoS Koruması,](../virtual-network/manage-ddos-protection.md) en gelişmiş DDoS tehditlerine karşı karşı önlemler sağlar. Hizmet, uygulamanız ve sanal ağlarınızda dağıtılan kaynaklar için gelişmiş DDoS azaltma özellikleri sağlar. Ayrıca, Azure DDoS Koruması kullanan müşteriler, etkin bir saldırı sırasında DDoS uzmanlarını meşgul etmek için DDoS Hızlı Yanıt desteğine erişebilir.

![DDOS Koruması](./media/networking-overview/ddos-protection.png)

### <a name="web-application-firewall"></a><a name="waf"></a>Web Uygulama Güvenlik Duvarı

[Azure Web Uygulaması Güvenlik Duvarı](../web-application-firewall/overview.md) (WAF), web uygulamalarınız için SQL enjeksiyonu ve site ler arası komut dosyası uygulama gibi yaygın web açıklarına ve güvenlik açıklarından koruma sağlar. Azure WAF, yönetilen kurallar aracılığıyla OWASP'ın en iyi 10 güvenlik açığına karşı kutu dışı koruma sağlar. Ayrıca müşteriler, kaynak IP aralığına dayalı ek koruma sağlamak ve üstbilgiler, tanımlama bilgileri, form veri alanları veya sorgu dize parametreleri gibi öznitelikleri istemek için müşteri tarafından yönetilen kurallar olan özel kuralları da yapılandırabilir.

Müşteriler, ortak ve özel adres alanındaki varlıklara bölgesel koruma sağlayan [Uygulama Ağ Geçidi ile Azure WAF](../application-gateway/waf-overview.md) dağıtmayı seçebilir. Müşteriler ayrıca, ağ kenarından ortak uç noktalara koruma sağlayan [Ön Kapı ile Azure WAF](../frontdoor/waf-overview.md) dağıtmayı da seçebilir.

![Web Uygulaması Güvenlik Duvarı](./media/networking-overview/waf-overview.png)


### <a name="azure-firewall"></a><a name="firewall"></a>Azure Güvenlik Duvarı
Azure Güvenlik Duvarı, Azure Sanal Ağ kaynaklarınızı koruyan yönetilen, bulut tabanlı bir güvenlik hizmetidir. Azure Güvenlik Duvarı'nı kullanarak, abonelikler ve sanal ağlar arasında uygulama ve ağ bağlantısı ilkelerini merkezi olarak oluşturabilir, uygulayabilir ve günlüğe kaydedebilirsiniz. Azure Güvenlik Duvarı, dış güvenlik duvarlarının sanal ağınızdan kaynaklanan trafiği tanımlamasına olanak tanımak amacıyla sanal ağ kaynaklarınız için statik genel bir IP adresi kullanır. 

Azure Güvenlik Duvarı hakkında daha fazla bilgi için [Azure Güvenlik Duvarı belgelerine](../firewall/overview.md)bakın.

![Güvenlik duvarına genel bakış](./media/networking-overview/firewall-threat.png)

### <a name="network-security-groups"></a><a name="nsg"></a>Ağ güvenlik grupları
Ağ güvenlik grupları ile bir Azure sanal ağı içindeki Azure kaynaklarına gelen ve bu kaynaklardan giden ağ trafiğini filtreleyebilirsiniz. Daha fazla bilgi için [Güvenlik genel bakışı](../virtual-network/security-overview.md)na bakın.

### <a name="service-endpoints"></a><a name="serviceendpoints"></a>Hizmet uç noktaları
Sanal Ağ hizmet uç noktaları, sanal ağ özel adres alanınızı ve sanal ağınızın kimliğini doğrudan bağlantı üzerinden Azure hizmetlerine genişletir. Uç noktalar kritik Azure hizmeti kaynaklarınızı sanal ağlarınızla sınırlayarak güvenliğini sağlamanıza imkan verir. Sanal ağınızdan Azure hizmetine giden trafik her zaman Microsoft Azure omurga ağında kalır. Daha fazla bilgi için [Sanal ağ hizmeti uç noktalarına](../virtual-network/virtual-network-service-endpoints-overview.md)bakın.

![Sanal ağ hizmet uç noktaları](./media/networking-overview/vnet-service-endpoints-overview.png)

### <a name="azure-private-link"></a><a name="privatelink"></a>Azure Özel Bağlantı
[Azure Özel Bağlantı,](../private-link/private-link-overview.md) Azure PaaS Hizmetlerine (örneğin, Azure Depolama ve SQL Veritabanı) erişmenizi ve Azure'un sanal ağınızdaki özel bir bitiş noktası üzerinden müşteriye ait/iş ortağı hizmetlerine erişmenizi sağlar.
Sanal ağınızla hizmet arasındaki trafik Microsoft omurga ağı arasında seyahat eder. Hizmetinizi genel internete maruz bırakmak artık gerekli değildir. Sanal ağınızda kendi özel bağlantı hizmetinizi oluşturabilir ve müşterilerinize teslim edebilirsiniz.

![Özel bitiş noktasına genel bakış](./media/networking-overview/private-endpoint.png)


## <a name="application-delivery-services"></a><a name="deliver"></a>Uygulama teslim hizmetleri

Bu bölümde, Azure'da ağ hizmetleri açıklanır ve bu hizmetler (Ağ İzleyicisi, ExpressRoute Monitor, Azure Monitörü veya VNet Terminal Erişim Noktası (TAP) gibi uygulamaların teslim ine yardımcı olur.

|Hizmet|Neden kullansın ki?|Senaryo|
|---|---|---|
|[Content Delivery Network](#cdn)|Kullanıcılara yüksek bant genişliğine sahiptir. CDN'ler önbelleğe alınmış içeriği son kullanıcılara yakın olan durum noktası (POP) konumlarında, gecikmeyi en aza indirmek için kenar sunucularında depolar|<p>[Web uygulamasına CDN ekleme](../cdn/cdn-add-to-web-app.md)</p> <p>[- HTTPS üzerinden Azure CDN özel etki alanı kullanarak depolama lekelerine erişin](..//cdn/cdn-storage-custom-domain-https.md)</p> <p>[Azure CDN bitiş noktanıza özel bir etki alanı ekleme](../cdn/cdn-map-content-to-custom-domain.md)</p> <p>[Azure CDN özel etki alanı üzerinde HTTPS yapılandırma](../cdn/cdn-custom-ssl.md?tabs=option-1-default-enable-https-with-a-cdn-managed-certificate)</p>|
|[Azure Front Door Hizmeti](#frontdoor)|Yüksek kullanılabilirlik için en iyi performansı ve anında küresel başarısızlığı en iyi şekilde optimize ederek web trafiğiniz için küresel yönlendirmeyi tanımlamanızı, yönetmenize ve izlemenize olanak tanır.|<p>[Azure Ön Kapı Hizmetinize özel bir etki alanı ekleme](../frontdoor/front-door-custom-domain.md)</p> <p>[HTTPS'yi Ön Kapı özel etki alanında yapılandırma](../frontdoor/front-door-custom-domain-https.md)</p><p>[Coğrafi filtreleme Web Uygulaması Güvenlik Duvarı ilkesini ayarlama](../frontdoor/front-door-tutorial-geo-filtering.md)|
|[Trafik Müdürü](#trafficmanager)|Trafiği DNS'ye dayalı olarak küresel Azure bölgelerindeki hizmetlere dağıtır ve yüksek kullanılabilirlik ve yanıt verme|<p> [Düşük gecikme süresi için trafiği yönlendirme](../traffic-manager/tutorial-traffic-manager-improve-website-response.md)</p><p>[Trafiği bir öncelik uç noktasına yönlendirme](../traffic-manager/traffic-manager-configure-priority-routing-method.md)</p><p> [Ağırlıklı uç noktalarla trafiği yönetme](../traffic-manager/tutorial-traffic-manager-weighted-endpoint-routing.md)</p><p>[Bitiş noktasının coğrafi konumuna göre rota trafiği](../traffic-manager/traffic-manager-configure-geographic-routing-method.md)</p> <p> [Kullanıcının alt ağına göre trafik yönlendirme](../traffic-manager/tutorial-traffic-manager-subnet-routing.md)</p>|
|[Load Balancer](#loadbalancer)|Kullanılabilirlik bölgeleri arasında ve VNet'lerinize trafiği yönlendirme yaparak bölgesel yük dengeleme sağlar. Bölgesel uygulamanızı oluşturmak için kaynaklarınız arasındaki trafiği yönlendirme yaparak dahili yük dengeleme sağlar.|<p> [Sanal makinelerin internet trafiğinde yük dengeleme](../load-balancer/tutorial-load-balancer-standard-manage-portal.md)</p> <p>[Sanal ağ içinde VM'ler arasında yük dengesi trafiği](../load-balancer/tutorial-load-balancer-basic-internal-portal.md)<p>[Belirli VM'lerde belirli bir bağlantı noktasına trafiği iletme](../load-balancer/tutorial-load-balancer-port-forwarding-portal.md)</p><p> [Yük dengeleme ve giden kuralları yapılandırma](../load-balancer/configure-load-balancer-outbound-cli.md)</p>|
|[Uygulama Ağ Geçidi](#applicationgateway)|Azure Application Gateway, web uygulamalarınıza trafiği yönetmenizi sağlayan bir web trafiği yük dengeleyicisidir.|<p>[Azure Application Gateway ile web trafiğini yönlendirme](../application-gateway/quick-create-portal.md)</p><p>[SSL sonlandırma ile bir uygulama ağ geçidi yapılandırma](../application-gateway/create-ssl-portal.md)</p><p>[URL yolu tabanlı yönlendirme ile bir uygulama ağ geçidi oluşturma](../application-gateway/create-url-route-portal.md) </p>|
|

### <a name="content-delivery-network"></a><a name="cdn"></a>Content Delivery Network
Azure Content Delivery Network (CDN), dünya genelindeki stratejik olarak yerleştirilen fiziksel düğümlerde içeriği önbelleğe alarak, yüksek bant genişliği içeriğinin hızlı bir şekilde kullanıcılara teslimi konusunda geliştiricilere genel bir çözüm sunar. Azure CDN hakkında daha fazla bilgi için Azure [İçerik Dağıtım Ağı'na](../cdn/cdn-overview.md) bakın

![Azure CDN](./media/networking-overview/cdn-overview.png)

### <a name="azure-front-door-service"></a><a name="frontdoor"></a>Azure Ön Kapı hizmeti
Azure Front Door Service, yüksek kullanılabilirliğe yönelik en iyi performans ve anında genel yük devretme için iyileştirerek, web trafiğinizin genel yönlendirmesini tanımlamanızı, yönetmenizi ve izlemenizi sağlar. Front Door ile, genel (çok bölgeli) tüketici ve kurumsal uygulamalarınızı Azure ile küresel çapta bir hedef kitleye ulaşan sağlam, yüksek performanslı, kişiselleştirilmiş modern uygulamalara, API’lere ve içeriğe dönüştürebilirsiniz. Daha fazla bilgi için Azure [Ön Kapı'ya](../frontdoor/front-door-overview.md)bakın.


### <a name="traffic-manager"></a><a name="trafficmanager"></a>Trafik Müdürü

Azure Traffic Manager, trafiği farklı Azure bölgelerindeki hizmetlere en uygun şekilde dağıtırken yüksek kullanılabilirlik ve yanıtlama hızı sağlayan DNS tabanlı bir trafik yük dengeleyicidir. Trafik Yöneticisi, öncelikli, ağırlıklı, performans, coğrafi, çok değerli veya alt ağ gibi trafiği dağıtmak için bir dizi trafik yönlendirme yöntemi sağlar. Trafik yönlendirme yöntemleri hakkında daha fazla bilgi için [Trafik Yöneticisi yönlendirme yöntemlerine](../traffic-manager/traffic-manager-routing-methods.md)bakın.

Aşağıdaki diyagram, Trafik Yöneticisi ile uç nokta önceliği tabanlı yönlendirmeyi gösterir:

![Azure Trafik Yöneticisi 'Öncelik' trafik yönlendirme yöntemi](./media/networking-overview/priority.png)

Trafik Yöneticisi hakkında daha fazla bilgi için Azure [Trafik Yöneticisi nedir?](../traffic-manager/traffic-manager-overview.md)

### <a name="load-balancer"></a><a name="loadbalancer"></a>Load Balancer
Azure Yük Dengeleyicisi, tüm UDP ve TCP protokolleri için yüksek performanslı, düşük gecikmeli Katman 4 yük dengeleme sağlar. Gelen ve giden bağlantıları yönetir. Genel ve iç yük dengeli uç noktaları yapılandırabilirsiniz. Hizmet kullanılabilirliğini yönetmek için TCP ve HTTP sistem durumu yoklama seçeneklerini kullanarak gelen bağlantıları arka uç havuz hedeflerine eşlemek için kurallar tanımlayabilirsiniz. Yük Dengeleyicisi hakkında daha fazla bilgi edinmek için [Yük Dengeleyicisi genel bakış](../load-balancer/load-balancer-overview.md) makalesini okuyun.

Aşağıdaki resim, hem harici hem de dahili yük dengeleyicileri kullanan Internet'e bakan çok katmanlı bir uygulamayı gösterir:

![Azure Yük Dengeleyici örneği](./media/networking-overview/load-balancer.png)


### <a name="application-gateway"></a><a name="applicationgateway"></a>Uygulama Ağ Geçidi
Azure Application Gateway, web uygulamalarınıza trafiği yönetmenizi sağlayan bir web trafiği yük dengeleyicisidir. Bir hizmet olarak uygulama teslim denetleyicisi (ADC), uygulamalarınız için çeşitli katman 7 yük dengeleme özellikleri sunar. Daha fazla bilgi için azure [uygulama ağ geçidi nedir?](../application-gateway/overview.md)

Aşağıdaki diyagram, Uygulama Ağ Geçidi ile url yol tabanlı yönlendirmeyi gösterir.

![Uygulama Ağ Geçidi örneği](./media/networking-overview/figure1-720.png)

## <a name="network-monitoring-services"></a><a name="monitor"></a>Ağ izleme hizmetleri
Bu bölümde, Ağ Watcher, ExpressRoute Monitor, Azure Monitörü ve Sanal Ağ TAP gibi ağ kaynaklarınızı izlemeye yardımcı olan Azure ağ hizmetleri açıklanmaktadır.

|Hizmet|Neden kullansın ki?|Senaryo|
|---|---|---|
|[Ağ İzleyicisi](#networkwatcher)|Bağlantı sorunlarını izlemeye ve sorun gidermenize yardımcı olur, VPN, NSG ve yönlendirme sorunlarını tanılamaya, VM'nizdeki paketleri yakalamaya, Azure İşlevlerini ve Mantık Uygulamalarını kullanarak tanılama araçlarını otomatikleştirir|<p>[VM trafik filtresi sorununu tanılama](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md)</p><p>[VM yönlendirme sorunlarını tanılama](../network-watcher/diagnose-vm-network-routing-problem.md)</p><p>[VM'ler arasındaki iletişimi izleme](../network-watcher/connection-monitor.md)</p><p>[Ağlar arasındaki iletişim sorunlarını tanılama](../network-watcher/diagnose-communication-problem-between-networks.md)</p><p>[Bir VM’ye gelen ve VM’den giden ağ trafiğini günlüğe kaydetme](../network-watcher/network-watcher-nsg-flow-logging-portal.md)</p>|
|[ExpressRoute Monitör](#expressroutemonitor)|Ağ performansının, kullanılabilirliğinin ve kullanımının gerçek zamanlı olarak izlenmesini sağlar, ağ topolojisinin otomatik olarak keşfedilmesine yardımcı olur, daha hızlı hata yalıtımı sağlar, geçici ağ sorunlarını algılar, geçmiş ağ performansını analiz eder özellikleri, çoklu abonelik destekler|<p>[ExpressRoute için Ağ Performansı İzleyicisi’ni Yapılandırma](../expressroute/how-to-npm.md)</p><p>[ExpressRoute izleme, ölçümler ve uyarılar](../expressroute/expressroute-monitoring-metrics-alerts.md)</p>|
|[Azure İzleyici](#azuremonitor)|Uygulamalarınızın nasıl performans gösterdiğini anlamanıza yardımcı olur ve onları etkileyen sorunları ve bağlı oldukları kaynakları proaktif olarak tanımlar.|<p>[Trafik Yöneticisi ölçümleri ve uyarıları](../traffic-manager/traffic-manager-metrics-alerts.md)</p><p>[Standart Yük Dengeleyicisi için Azure monitör tanılama](../load-balancer/load-balancer-standard-diagnostics.md)</p><p>[Azure Güvenlik Duvarı günlüklerini ve ölçümlerini izleme](../firewall/tutorial-diagnostics.md)</p><p>[Azure web uygulaması güvenlik duvarı izleme ve günlüğe kaydetme](../frontdoor/waf-front-door-monitor.md)</p>|
|[Sanal Ağ TAP](#vnettap)|Sanal makine ağ trafiğinin paket toplayıcısına sürekli akışını sağlar, ağ ve uygulama performans yönetimi çözümleri ve güvenlik analiz araçları sağlar|[VNet TAP kaynağı oluşturma](../virtual-network/tutorial-tap-virtual-network-cli.md)|
|

### <a name="network-watcher"></a><a name="networkwatcher"></a>Ağ İzleyicisi
Azure Ağ İzleyicisi, Azure sanal ağındaki kaynaklarda izleme, tanılama, ölçümleri görüntüleme ve günlükleri etkinleştirme veya devre dışı bırakma işlemleri için araçlar sağlar. Daha fazla bilgi için [Bkz. Ağ İzleyicisi nedir?](../network-watcher/network-watcher-monitoring-overview.md?toc=%2fazure%2fnetworking%2ftoc.json)
### <a name="expressroute-monitor"></a><a name="expressroutemonitor"></a>ExpressRoute Monitör
ExpressRoute devre ölçümlerini, tanılama günlüklerini ve uyarıları nasıl görüntülenebildiğini öğrenmek için [ExpressRoute izleme, ölçümler ve uyarıları](../expressroute/expressroute-monitoring-metrics-alerts.md?toc=%2fazure%2fnetworking%2ftoc.json)görün.
### <a name="azure-monitor"></a><a name="azuremonitor"></a>Azure İzleyici
Azure İzleyici, bulut ve şirket içi ortamlardaki telemetriyi toplamak, analiz etmek ve bunlarda eylem gerçekleştirmek için kapsamlı bir çözüm sunarak uygulamalarınızın kullanılabilirliğini ve performansını en üst düzeye çıkarır. Uygulamalarınızın performansını anlamanıza ve uygulamalarla bağlı oldukları kaynakları etkileyen sorunları önceden tespit etmenize yardımcı olur. Daha fazla bilgi için Azure [Monitöre Genel Bakış'a](../azure-monitor/overview.md?toc=%2fazure%2fnetworking%2ftoc.json)bakın.
### <a name="virtual-network-tap"></a><a name="vnettap"></a>Sanal Ağ TAP
Azure sanal ağ TAP (Terminal Erişim Noktası), sanal makine ağ trafiğinizi sürekli olarak bir ağ paketi toplayıcısına veya analiz aracına aktarmanızı sağlar. Toplayıcı veya analiz aracı bir [ağ sanal cihaz](https://azure.microsoft.com/solutions/network-appliances/) ortağı tarafından sağlanır. 

Aşağıdaki resim, sanal ağ TAP'ın nasıl çalıştığını gösterir. 

![Sanal ağ TAP nasıl çalışır?](./media/networking-overview/virtual-network-tap-architecture.png)

Daha fazla bilgi için [Sanal Ağ TAP nedir'](../virtual-network/virtual-network-tap-overview.md)e bakın.

## <a name="next-steps"></a>Sonraki adımlar

- İlk Sanal [Ağ](../virtual-network/quick-create-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) makalenizi oluştur'daki adımları tamamlayarak ilk VNet'inizi oluşturun ve birkaç VM'yi ona bağlayın.
- Bir [noktaya nokta bağlantı makalesinde yapılandırılan](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fnetworking%2ftoc.json)adımları tamamlayarak bilgisayarınızı bir VNet'e bağlayın.
- [Internet'e bakan yük dengeleyici](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fnetworking%2ftoc.json) makalesindeki adımları tamamlayarak internet trafiğini ortak sunuculara yükleyin.
 
 
   
