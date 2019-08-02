---
title: Azure ağ güvenliği | Microsoft Docs
description: Uygulamanızın veya kuruluşunuzun ihtiyaçlarını karşılamak üzere otomatik olarak ölçeklenebilen ve azaltılabilen hizmet & çeşitli bilgi işlem örnekleri içeren bulut tabanlı bilgi işlem hizmetleri hakkında bilgi edinin.
services: security
documentationcenter: na
author: UnifyCloud
manager: barbkess
editor: TomSh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: TomSh
ms.openlocfilehash: db3f5aca0240c19f67d5d0775148d5eec76daa03
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68726981"
---
# <a name="azure-network-security"></a>Azure ağ güvenliği

Güvenliğin bulutta bir iş olduğunu ve Azure güvenliğiyle ilgili doğru ve güncel bilgileri nerede bulabileceğinizi biliyoruz. Uygulamalarınız ve hizmetleriniz için Azure kullanmanın en iyi nedenlerinden biri, Azure 'un geniş güvenlik araçları ve özellikleri dizisinden faydalanabilir. Bu araçlar ve yetenekler, Azure platformunda güvenli çözümler oluşturmayı olanaklı hale getirmeye yardımcı olur.

Microsoft Azure, müşteri verilerinin gizliliğini, bütünlüğünü ve kullanılabilirliğini sağlar. Ayrıca, saydam sorumluluklığa de olanak tanır. Müşterinin perspektifinden Microsoft Azure içinde uygulanan ağ güvenlik denetimlerinin toplanmasını daha iyi anlamanıza yardımcı olmak için, "Azure ağ güvenliği" başlıklı makalede ağ güvenlik denetimlerine kapsamlı bir görünüm sağlamak üzere yazılmıştır Microsoft Azure ile kullanılabilir.

Bu sayfa, Azure 'da dağıttığınız çözümlerin güvenliğini geliştirmek için yapılandırabileceğiniz çok çeşitli ağ denetimleri hakkında bilgi almak üzere tasarlanmıştır. Microsoft 'un Azure platformunun ağ dokusunun güvenliğini sağlama konusunda bilgi edinmek istiyorsanız, [Microsoft Güven Merkezi](https://microsoft.com/en-us/trustcenter/cloudservices/azure)' nde Azure güvenliği bölümüne bakın.

## <a name="azure-platform"></a>Azure platformu

Azure, geniş bir işletim sistemi, programlama dili, çerçeve, araç, veritabanı ve cihaz seçimini destekleyen genel bir bulut hizmeti platformudur.  Docker tümleştirmesiyle Linux kapsayıcıları çalıştırabilir; JavaScript, Python, .NET, PHP, Java ve Node. js ile uygulamalar oluşturun; iOS, Android ve Windows cihazlar için arka uçlar oluşturun. Azure bulut Hizmetleri, milyonlarca geliştirici ve BT uzmanlarının zaten kullandığı ve güvendiği teknolojilerin aynısını destekler.

Ortak bir bulut hizmeti sağlayıcısı üzerinde derleme veya BT varlıkları geçirme yaptığınızda, bu kuruluşun, uygulama ve verilerinizi hizmetler ve bulut tabanlı varlıklarınızın güvenliğini yönetmek için sağladıkları denetimlerle koruma yeteneklerine bağlı olursunuz.

Azure 'un altyapısı, tesis aracılığıyla milyonlarca müşteriyi aynı anda barındırmak için uygulamalara tasarlanmıştır ve işletmelerin güvenlik gereksinimlerini karşılayabilecekleri güvenilir bir temel sağlar. Ayrıca, Azure, güvenli bir şekilde yapılandırılabilir güvenlik seçenekleri ve bunları, kuruluşunuzun dağıtımlarınızın benzersiz gereksinimlerini karşılayacak şekilde güvenliği özelleştirebilmeniz için denetleyebilme olanağı sağlar.

## <a name="abstract"></a>Özet

Microsoft genel bulut Hizmetleri, karma bağlantı için hiper ölçekli hizmetler ve altyapı, kurumsal düzeyde yetenekler ve birçok seçenek sunar. Bu hizmetlere Internet aracılığıyla ya da özel ağ bağlantısı sağlayan Azure ExpressRoute ile erişmeyi seçebilirsiniz. Microsoft Azure platformu, altyapınızı buluta sorunsuzca genişletmenizi ve çok katmanlı mimariler oluşturmayı sağlar. Ayrıca, üçüncü taraflar güvenlik hizmetleri ve sanal gereçler sunarak gelişmiş özellikleri de etkinleştirebilir.

Azure 'un ağ hizmetleri, tasarıma göre esneklik, kullanılabilirlik, dayanıklılık, güvenlik ve bütünlüğü en üst düzeye çıkarır. Bu Teknik İnceleme, Azure 'un ağ işlevleri hakkında ayrıntılar ve müşterilerin Azure 'un yerel güvenlik özelliklerini kullanarak bilgi varlıklarını korumanıza yardımcı olacak bilgiler sağlar.

Bu teknik incelemeye yönelik hedef kitleler şunları içerir:

- Azure 'da kullanılabilen ve desteklenen güvenlik çözümlerini arayan teknik Yöneticiler, ağ yöneticileri ve geliştiriciler.

-   Azure genel bulutundaki ağ güvenliği etrafında tartışmalarda alakalı olan Azure ağ teknolojileri ve hizmetleri hakkında üst düzey bir genel bakış almak isteyen SMEs veya iş süreci yöneticileri.

## <a name="azure-networking-big-picture"></a>Azure ağ büyük resmi
Microsoft Azure, uygulamanızı ve hizmet bağlantı gereksinimlerinizi desteklemek için sağlam bir ağ altyapısı içerir. Azure 'da bulunan kaynaklar arasında, şirket içi ve Azure 'da barındırılan kaynaklar arasında ve Internet ve Azure arasında ağ bağlantısı kurulabilir.

![Azure ağ büyük resmi](./media/network-security/azure-network-security-fig-1.png)

[Azure ağ altyapısı](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-networking-guidelines) , sanal ağlar (VNet) ile Azure kaynaklarını birbirine güvenli bir şekilde bağlamanıza olanak sağlar. VNet, buluttaki kendi ağınızın bir gösterimidir. VNet, aboneliğiniz için ayrılmış olan Azure bulut ağının mantıksal bir yalıtımına sahiptir. VNET 'leri şirket içi ağlarınıza bağlayabilirsiniz.

Azure, şirket içi ağınız ve [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction)Ile bir Azure sanal ağı IÇIN adanmış WAN bağlantısı bağlantısını destekler. Azure ile siteniz arasındaki bağlantı, genel Internet üzerinden gitmeyecek özel bir bağlantı kullanır. Azure uygulamanız birden çok veri merkezinde çalışıyorsa, kullanıcılardan gelen istekleri uygulama örnekleri arasında akıllıca yönlendirmek için [azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview) kullanabilirsiniz. Internet 'ten erişilebiliyorsa, trafiği Azure 'da çalıştırmayan hizmetlere de yönlendirebilirsiniz.

## <a name="enterprise-view-of-azure-networking-components"></a>Azure ağ bileşenlerinin kurumsal görünümü
Azure 'da ağ güvenliği tartışmalarına uygun birçok ağ bileşeni vardır. Bu ağ bileşenlerini anladık ve bunlarla ilgili güvenlik sorunlarına odaklanıyoruz.

> [!Note]
> Azure ağ iletişimi 'nin tüm yönleri açıklanmamaktadır; Azure 'da dağıttığınız hizmetlerinizin ve uygulamalarınızın yanı sıra güvenli bir ağ altyapısı planlama ve tasarlama konusunda ele alınanlar tartışıyoruz.

Bu yazıda, aşağıdaki Azure ağı kurumsal özellikleri ele alınacaktır:

-   Temel ağ bağlantısı

-   Karma bağlantı

-   Güvenlik Denetimleri

-   Ağ doğrulaması

### <a name="basic-network-connectivity"></a>Temel ağ bağlantısı

[Azure sanal ağ](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) hizmeti, sanal ağlar (VNet) ile Azure kaynaklarını birbirine güvenli bir şekilde bağlamanıza olanak sağlar. VNet, buluttaki kendi ağınızın bir gösterimidir. VNet, aboneliğinize adanmış Azure ağ altyapısının mantıksal yalıtımına sahiptir. Sanal ağlar birbirlerine ve siteden siteye VPN kullanarak şirket içi ağlarınız da bağlanabilirsiniz ve ayrılmış [WAN bağlantıları](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

![Temel ağ bağlantısı](./media/network-security/azure-network-security-fig-2.png)

Azure 'da sunucuları barındırmak için VM 'Leri kullanacağınızı anlamak için bu VM 'Lerin bir ağa nasıl bağlanacağını görürsünüz. Yanıt, VM 'Lerin bir [Azure sanal ağına](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)bağlanmasını sağlar.

Azure sanal ağları, Microsoft Hyper-V veya VMware gibi kendi sanallaştırma platformu çözümlerinizle şirket içinde kullandığınız sanal ağlara benzer.

#### <a name="intra-vnet-connectivity"></a>VNet içi bağlantı

VNet 'ler genelinde birbirleriyle iletişim kurmak için VNet 'e bağlı kaynakları etkinleştirerek, sanal ağları birbirlerine bağlayabilirsiniz. VNET 'leri birbirine bağlamak için aşağıdaki seçeneklerden birini veya her ikisini birden kullanabilirsiniz:

- **Eşleme** Aynı Azure konumunda bulunan farklı Azure sanal ağlarına bağlı kaynakların birbirleriyle iletişim kurmasını sağlar. Sanal ağ genelindeki bant genişliği ve gecikme süresi, kaynakların aynı VNet 'e bağlıymış ile aynıdır. Eşleme hakkında daha fazla bilgi edinmek için [sanal ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)makalesini okuyun.

  ![Eşleme](./media/network-security/azure-network-security-fig-3.png)

- **VNet-VNet bağlantısı:** Aynı veya farklı Azure konumlarında farklı Azure VNet 'e bağlı kaynakları sağlar. Eşlemenin aksine, trafiğin bir Azure VPN Gateway ile akışı gerektiğinden sanal ağlar arasında bant genişliği sınırlıdır.

![VNet-VNet bağlantısı](./media/network-security/azure-network-security-fig-4.png)


VNet 'ten VNet 'e bağlantı ile sanal ağları bağlama hakkında daha fazla bilgi edinmek için, [VNET-VNet bağlantısı yapılandırma makalesini](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal?toc=%2fazure%2fvirtual-network%2ftoc.json)okuyun.

#### <a name="azure-virtual-network-capabilities"></a>Azure sanal ağ özellikleri:

Görebileceğiniz gibi, bir Azure sanal ağı, sanal makinelerin ağa bağlanmasını sağlar, böylece güvenli bir şekilde diğer ağ kaynaklarına bağlanabilirler. Ancak temel bağlantı yalnızca başlangıcıdır. Azure sanal ağ hizmeti 'nin aşağıdaki özellikleri, Azure sanal ağının güvenlik özelliklerini kullanıma sunar:

-   Yalıtım

-   İnternet bağlantısı

-   Azure kaynak bağlantısı

-   VNet bağlantısı

-   Şirket içi bağlantı

-   Trafik filtreleme

-   Yönlendirme

**Yalıtımı**

VNET 'ler diğerinden [yalıtılmıştır](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) . Geliştirme, test ve üretim için aynı [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) adres bloklarını kullanan ayrı sanal ağlar oluşturabilirsiniz. Buna karşılık, farklı CıDR adres blokları kullanan ve ağları birbirine bağlayan birden fazla sanal ağ oluşturabilirsiniz. VNet 'i birden çok alt ağa segmentleyebilirsiniz.

Azure, sanal makineler ve sanal ağa bağlı [Cloud Services](https://azure.microsoft.com/services/cloud-services/) rol örnekleri için iç ad çözümlemesi sağlar. İsteğe bağlı olarak, Azure iç ad çözümlemesi kullanmak yerine kendi DNS sunucularınızı kullanmak üzere bir sanal ağ yapılandırabilirsiniz.

Her bir Azure [aboneliği](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology?toc=%2fazure%2fvirtual-network%2ftoc.json) ve Azure [bölgesinde](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology?toc=%2fazure%2fvirtual-network%2ftoc.json)birden çok VNET uygulayabilirsiniz. Her VNet diğer Vnet'lere yalıtılır. Her VNet için şunları yapabilirsiniz:

-   Genel ve özel (RFC 1918) adresleri kullanarak özel bir gizli IP adresi alanı belirtin. Azure, VNet 'e bağlı kaynakları adres alanından özel bir IP adresi olarak atar, atarsınız.

-   VNet 'i bir veya daha fazla alt ağda segmentlere ayırın ve her alt ağa VNet adres alanının bir kısmını ayırın.

-   Azure tarafından sağlanmış ad çözümlemesini kullanın veya bir sanal ağa bağlı kaynaklar tarafından kullanılmak üzere kendi DNS sunucunuzu belirtin. VNET 'lerde ad çözümlemesi hakkında daha fazla bilgi edinmek için, [VM 'ler Için ad çözümlemesini ve Cloud Services](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances)okuyun.

**İnternet bağlantısı**

Bir sanal ağa bağlı tüm [Azure sanal makineleri (VM)](https://docs.microsoft.com/azure/virtual-machines/windows/) ve Cloud Services rol örnekleri, varsayılan olarak Internet erişimine sahiptir. Gerektiğinde belirli kaynaklara gelen erişimi de etkinleştirebilirsiniz. (VM) ve bir sanal ağa bağlı Cloud Services rol örnekleri, varsayılan olarak Internet erişimine sahiptir. Gerektiğinde belirli kaynaklara gelen erişimi de etkinleştirebilirsiniz.

Bir sanal ağa bağlı tüm kaynakların varsayılan olarak Internet 'e giden bağlantısı vardır. Kaynağın özel IP adresi, Azure altyapısı tarafından genel bir IP adresine çevrilen kaynak ağ adresidir (SNAT). Özel yönlendirme ve trafik filtrelemeyi uygulayarak varsayılan bağlantıyı değiştirebilirsiniz. Giden Internet bağlantısı hakkında daha fazla bilgi edinmek için [Azure 'da giden bağlantıları anlama](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections?toc=%2fazure%2fvirtual-network%2ftoc.json)konusunu okuyun.

Internet 'ten gelen Azure kaynaklarıyla iletişim kurmak veya SNAT olmadan Internet 'e giden iletişim kurmak için bir kaynağa genel bir IP adresi atanmalıdır. Genel IP adresleri hakkında daha fazla bilgi edinmek için [genel IP adreslerini](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address)okuyun.

**Azure kaynak bağlantısı**

Cloud Services ve sanal makineler gibi [Azure kaynakları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) aynı VNET 'e bağlanabilir. Kaynaklar, farklı alt ağlarda olsalar bile, özel IP adresleri kullanarak birbirlerine bağlanabilir. Azure, alt ağlar, VNET 'ler ve şirket içi ağlar arasında varsayılan yönlendirme sağlar, bu sayede rotaları yapılandırmanız ve yönetmeniz gerekmez.

Çeşitli Azure kaynaklarını sanal makineler (VM), Cloud Services, App Service ortamları ve sanal makine ölçek kümeleri gibi bir VNet 'e bağlayabilirsiniz. VM 'Ler bir ağ arabirimi (NIC) aracılığıyla bir sanal ağ içindeki bir alt ağa bağlanır. NIC 'ler hakkında daha fazla bilgi edinmek için [ağ arabirimlerini](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface)okuyun.

**VNet bağlantısı**

[Sanal](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) ağlar birbirine bağlanabilir ve herhangi bir VNET 'e bağlı kaynakların diğer tüm sanal ağlardaki herhangi bir kaynakla iletişim kurmasını sağlar.

VNet 'ler genelinde birbirleriyle iletişim kurmak için VNet 'e bağlı kaynakları etkinleştirerek, sanal ağları birbirlerine bağlayabilirsiniz. VNET 'leri birbirine bağlamak için aşağıdaki seçeneklerden birini veya her ikisini birden kullanabilirsiniz:

- **Eşleme** Aynı Azure konumunda bulunan farklı Azure sanal ağlarına bağlı kaynakların birbirleriyle iletişim kurmasını sağlar. Sanal ağlar arasındaki bant genişliği ve gecikme süresi, kaynakların aynı VNet.To bağlı olup olmadığı ile aynıdır. eşleme hakkında daha fazla bilgi edinmek için [sanal ağ eşlemesini](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)okuyun.

- **VNet-VNet bağlantısı:** Aynı veya farklı Azure konumlarında farklı Azure VNet 'e bağlı kaynakları sağlar. Eşlemenin aksine, trafiğin bir Azure VPN Gateway ile akışı gerektiğinden sanal ağlar arasında bant genişliği sınırlıdır. VNet 'ten VNet 'e bağlantı ile sanal ağları bağlama hakkında daha fazla bilgi edinmek için. Daha fazla bilgi edinmek için, [VNET-VNET bağlantısını yapılandırma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal?toc=%2fazure%2fvirtual-network%2ftoc.json) makalesini okuyun.

**Şirket içi bağlantı**

VNET 'ler, ağınız ve Azure arasında özel ağ bağlantıları aracılığıyla veya Internet üzerinden siteden siteye VPN bağlantısı aracılığıyla [Şirket içi](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) ağlara bağlanabilir.

Aşağıdaki seçeneklerin birleşimini kullanarak şirket içi ağınızı bir sanal ağa bağlayabilirsiniz:

- **Noktadan siteye sanal özel ağ (VPN):** Ağınıza ve VNet 'e bağlı tek bir BILGISAYAR arasında oluşturulmuştur. Azure’ı kullanmaya yeni başladıysanız bu bağlantı türü mükemmeldir. Mevcut ağınız üzerinde çok az bir değişiklik gerektirdiğinden veya hiç değişiklik gerektirmediğinden geliştiriciler için de mükemmeldir. Bağlantı, bılgısayar ile VNet arasında Internet üzerinden şifreli iletişim sağlamak için SSTP protokolünü kullanır. Trafik Internet 'Ten geçiş yaptığından Noktadan siteye VPN gecikmesi tahmin edilemez.

- **Siteden siteye VPN:** VPN cihazınız ile Azure VPN Gateway arasında oluşturulmuştur. Bu bağlantı türü, yetkilendirdiğiniz şirket içi kaynakların VNet 'e erişmesine olanak sağlar. Bağlantı, şirket içi cihazınız ile Azure VPN ağ geçidi arasında Internet üzerinden şifreli iletişim sağlayan bir IPSec/ıKE VPN 'dir. Trafik Internet 'Ten geçiş yaptığından siteden siteye bağlantı gecikmesi tahmin edilemez.

- **Azure ExpressRoute:** Bir ExpressRoute iş ortağı aracılığıyla ağınız ve Azure arasında oluşturulmuştur. Bu bağlantı özeldir. Trafik Internet 'e çapraz geçiş yapmaz. Trafik Internet 'Te geçiş yaptığından ExpressRoute bağlantısı gecikmesi tahmin edilebilir. Önceki tüm bağlantı seçenekleri hakkında daha fazla bilgi edinmek için [bağlantı topolojisi diyagramlarını](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json)okuyun.

**Trafik filtreleme**

VM ve Cloud Services rol örnekleri [ağ trafiğine](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) kaynak IP adresi ve bağlantı noktası, hedef IP adresi ve bağlantı noktası ve protokol tarafından gelen ve giden filtreleme yapılabilir.

Aşağıdaki seçeneklerden birini veya her ikisini de kullanarak alt ağlar arasındaki ağ trafiğini filtreleyebilirsiniz:

- **Ağ güvenlik grupları (NSG):** Her NSG, trafiği kaynak ve hedef IP adresi, bağlantı noktası ve protokole göre filtrelemenize olanak tanıyan birden fazla gelen ve giden güvenlik kuralı içerebilir. Bir VM 'deki her NIC 'ye bir NSG uygulayabilirsiniz. Ayrıca, bir NIC 'e veya diğer Azure kaynaklarına bağlı olan alt ağa bir NSG de uygulayabilirsiniz. NSG 'ler hakkında daha fazla bilgi edinmek için [ağ güvenlik gruplarını](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)okuyun.

- **Sanal ağ gereçleri:** Sanal ağ gereci, güvenlik duvarı gibi bir ağ işlevi gerçekleştiren bir VM çalıştıran yazılımdır. Azure Marketi 'nde kullanılabilir NVA 'lar listesini görüntüleyin. WAN iyileştirmesi ve diğer ağ trafiği işlevlerini sağlayan NVA 'lar de mevcuttur. NVA 'lar genellikle Kullanıcı tanımlı veya BGP rotalarıyla kullanılır. VNET 'ler arasındaki trafiği filtrelemek için de bir NVA kullanabilirsiniz.

**Yönlendirme**

İsteğe bağlı olarak, kendi rotalarınızı yapılandırarak veya ağ geçidi aracılığıyla BGP yollarını kullanarak Azure 'un varsayılan yönlendirmesini geçersiz kılabilirsiniz.

Azure, her VNet 'teki herhangi bir alt ağa bağlı kaynakların varsayılan olarak birbirleriyle iletişim kurmasını sağlayan rota tabloları oluşturur. Azure’ın oluşturduğu varsayılan rotaları geçersiz kılmak için aşağıdaki seçeneklerden birini veya her ikisini uygulayabilirsiniz:

- **Kullanıcı tanımlı rotalar:** Her alt ağ için trafiğin nereye yönlendirildiğini denetleyen yollarla özel yol tabloları oluşturabilirsiniz. Kullanıcı tanımlı rotalar hakkında daha fazla bilgi edinmek için [Kullanıcı tanımlı yolları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview)okuyun.

- **BGP yolları:** VNet 'i Azure VPN Gateway veya ExpressRoute bağlantısı kullanarak şirket içi ağınıza bağladığınızda, BGP yollarını sanal ağlarınızı yayabilirsiniz.

### <a name="hybrid-internet-connectivity-connect-to-an-on-premises-network"></a>Karma internet bağlantısı: Şirket içi ağa bağlanma
Aşağıdaki seçeneklerin birleşimini kullanarak şirket içi ağınızı bir sanal ağa bağlayabilirsiniz:

-   İnternet bağlantısı

-   Noktadan siteye VPN (P2S VPN)

-   Siteden siteye VPN (S2S VPN)

-   ExpressRoute

#### <a name="internet-connectivity"></a>Internet bağlantısı

Adından da anlaşılacağı gibi, Internet bağlantısı, sanal ağ içinde canlı olan iş yükleri için farklı genel uç noktaları kullanıma sunarak iş yüklerinizi Internet 'ten erişilebilir hale getirir. Bu iş yükleri, [Internet 'e yönelik Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview) veya yalnızca VM 'ye genel bir IP adresi atayarak gösterilebilir. Bu şekilde, Internet üzerindeki herhangi bir şeyin, bir ana bilgisayar güvenlik duvarı, [ağ güvenlik grupları (NSG)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)ve [Kullanıcı tanımlı yolların gerçekleşmelerinin](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) gerçekleşmesini sağlamak için bu sanal makineye erişebilmesi mümkün olur.

Bu senaryoda, Internet 'e genel olması gereken ve iş yüklerinizin yapılandırmasına bağlı olarak herhangi bir yerden veya belirli konumlardan bağlanabilir bir uygulamayı kullanıma sunabileceksiniz.

#### <a name="point-to-site-vpn-or-site-to-site-vpn"></a>Noktadan siteye VPN veya siteden siteye VPN
Bu iki ikisi de aynı kategoriye denk gelir. Her ikisi de sanal ağınızın bir VPN Gateway sahip olmasını gerektirir ve bu sunucuya, [Noktadan siteye yapılandırmanın](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) bir parçası olarak iş Istasyonunuzun VPN istemcisini kullanarak bağlanabilir ya da şirket içi [VPN cihazınızı](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices) siteden siteye sonlandırabilecek şekilde yapılandırabilirsiniz Sanal. Bu şekilde, şirket içi cihazlar VNet içindeki kaynaklara bağlanabilir.

Noktadan Siteye (P2S) yapılandırması, ayrı bir istemci bilgisayardan bir sanal ağa yönelik güvenli bağlantı oluşturmanıza olanak sağlar. P2S, SSTP (Güvenli Yuva Tünel Protokolü) aracılığıyla gerçekleşen bir VPN bağlantısıdır.

![Noktadan siteye VPN](./media/network-security/azure-network-security-fig-5.png)

Noktadan siteye bağlantılar, VNet 'iniz ile veya bir konferans merkezi gibi uzak bir konumdan veya bir sanal ağa bağlanması gereken yalnızca birkaç istemciniz olduğunda, sanal ağınıza bağlanmak istediğinizde faydalıdır.

P2S bağlantılarının bir VPN cihazına veya genel kullanıma yönelik bir IP adresine gerek yoktur. VPN bağlantısını istemci bilgisayardan kurarsınız. Bu nedenle, Azure ağınıza birçok şirket içi cihazdan ve bilgisayardan kalıcı bir bağlantı olması gerekebileceği için P2S Azure 'a bağlanmak için önerilmez.

![Siteden siteye VPN](./media/network-security/azure-network-security-fig-6.png)

> [!Note]
> Noktadan siteye bağlantılar hakkında daha fazla bilgi için bkz. [noktadan sıteye FA v Q](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal).

Siteden Siteye VPN ağ geçidi bağlantısı, şirket içi ağınızı bir IPsec/IKE (IKEv1 veya IKEv2) tüneli üzerinden Azure sanal ağına bağlamak için kullanılır.

Bu bağlantı türü için, şirket içinde yer alan ve kendisine atanmış dışarıya yönelik bir genel IP adresi atanmış olan bir VPN cihazı gerekir. Bu bağlantı Internet üzerinden gerçekleşir ve ağınız ile Azure arasında şifrelenmiş bir bağlantı içindeki bilgileri "tünellerinizi" sağlar. Siteden siteye VPN, delikler için tüm boyutlardaki kuruluşlar tarafından dağıtılan güvenli, Olgun bir teknolojidir. Tünel şifreleme, [IPSec tünel modu](https://technet.microsoft.com/library/cc786385.aspx)kullanılarak gerçekleştirilir.

Siteden siteye VPN güvenilen, güvenilir ve yerleşik bir teknolojide, tüneldeki trafik Internet üzerinden geçiş yapar. Buna ek olarak, bant genişliği görece en fazla 200 Mbps ile sınırlıdır.

Şirketler arası bağlantılarınız için olağanüstü düzeyde güvenlik veya performans istiyorsanız şirket içi bağlantınız için Azure ExpressRoute kullanmanızı öneririz. ExpressRoute, şirket içi konumunuz veya bir Exchange barındırma sağlayıcısı arasında adanmış bir WAN bağlantıdır. Bu bir telekomünikasyon bağlantısı olduğundan, verileriniz Internet üzerinden hareket etmez ve bu nedenle Internet iletişimlerinde bulunan olası risklere maruz değildir.

> [!Note]
> VPN ağ geçitleri hakkında daha fazla bilgi için bkz. [VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)hakkında.

#### <a name="dedicated-wan-link"></a>Adanmış WAN bağlantısı
Microsoft Azure ExpressRoute, şirket içi ağlarınızı bir bağlantı sağlayıcısı tarafından kolaylaştırarak adanmış bir özel bağlantı üzerinden Azure 'a genişletmenizi sağlar.

ExpressRoute bağlantıları ortak İnternet üzerinden geçmemektedir. Bu, ExpressRoute bağlantılarına İnternet üzerindeki sıradan bağlantılara göre daha fazla güvenilirlik, yüksek hız, düşük gecikme ve normal bağlantılardan daha yüksek güvenlik sağlar.

![ Adanmış WAN bağlantısı](./media/network-security/azure-network-security-fig-7.png)

> [!Note]
> ExpressRoute kullanarak ağınızı Microsoft 'a bağlama hakkında bilgi için bkz. [ExpressRoute bağlantı modelleri](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) ve [ExpressRoute teknik genel bakış](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

Siteden siteye VPN seçeneklerinde olduğu gibi, ExpressRoute da yalnızca bir VNet 'te olması gerekmeyen kaynaklara bağlanmanızı sağlar. Aslında, SKU 'ya bağlı olarak, 10 VNET 'e bağlanabilirsiniz. [Premium eklentiye](https://docs.microsoft.com/azure/expressroute/expressroute-faqs)sahipseniz, bant genişliğine bağlı olarak 100 VNET 'e kadar bağlantı kurulabilir. Bu bağlantı türlerinin ne gibi göründüğünü öğrenmek için [bağlantı topolojisi diyagramlarını](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json)okuyun.

### <a name="security-controls"></a>Güvenlik denetimleri
Azure sanal ağ, diğer sanal ağlardan yalıtılmış güvenli, mantıksal bir ağ sağlar ve şirket içi ağlarınızda kullandığınız birçok güvenlik denetimini destekler. Müşteriler, iş yüklerini bulutta çalıştırmak için kendi özel IP adresi aralığını kullanır, rota tabloları, ağ güvenlik grupları, erişim denetim listeleri (ACL 'Ler), ağ geçitleri ve sanal gereçler kullanarak kendi yapılarını oluşturur.

Azure sanal ağlarınızda kullanabileceğiniz güvenlik denetimleri aşağıda verilmiştir:

-   Ağ erişim denetimleri

-   Kullanıcı tanımlı yollar

-   Ağ güvenlik gereci

-   Application Gateway

-   Azure Web uygulaması güvenlik duvarı

-   Ağ kullanılabilirlik denetimi

#### <a name="network-access-controls"></a>Ağ erişim denetimleri
Azure sanal ağı (VNet), Azure ağ modelinin temel taşıdır ve yalıtım ve koruma sağlar. ağ [güvenlik grupları (NSG)](https://blogs.msdn.microsoft.com/igorpag/2016/05/14/azure-network-security-groups-nsg-best-practices-and-lessons-learned/) , ağ trafiği kurallarını ağ düzeyinde zorlamak ve denetlemek için kullandığınız ana araçtır.

![ Ağ erişim denetimleri](./media/network-security/azure-network-security-fig-8.png)


Bir sanal ağ içindeki iş yükleri arasında, şirket içi bağlantı aracılığıyla müşteri ağlarındaki sistemlerden veya doğrudan Internet iletişiminden gelen iletişime izin vererek veya bağlantıyı reddeterek erişimi denetleyebilirsiniz.

Diyagramda, hem VNET 'ler hem de NSG 'ler, Azure genel güvenlik yığınındaki belirli bir katmanda bulunur; burada NSG 'ler, UDR ve ağ sanal gereçleri, korunan ağdaki uygulama dağıtımlarını korumak üzere güvenlik sınırları oluşturmak için kullanılabilir.

NSG 'ler trafiği değerlendirmek için 5 tanımlama grubu kullanır (ve NSG için yapılandırdığınız kurallarda kullanılır):

-   [Kaynak ve hedef IP adresi](https://support.microsoft.com/help/969029/the-functionality-for-source-ip-address-selection-in-windows-server-2008-and-in-windows-vista-differs-from-the-corresponding-functionality-in-earlier-versions-of-windows)

-   [Kaynak ve hedef bağlantı noktası](https://technet.microsoft.com/library/dd197515)

-   Protokol: [Iletim Denetimi Protokolü (TCP)](https://technet.microsoft.com/library/cc940037.aspx) veya [Kullanıcı veri BIRIMI Protokolü (UDP)](https://technet.microsoft.com/library/cc940034.aspx)

Bu, tek bir VM ile bir VM grubu ya da tek bir VM 'den başka bir VM 'ye veya tüm alt ağlar arasında erişimi denetleyebilmeniz anlamına gelir. Bunun için, tam paket inceleme değil, bunun basit durum bilgisi olmayan paket filtrelemesi olduğunu unutmayın. Bir ağ güvenlik grubunda protokol doğrulama veya ağ düzeyi KIMLIĞI veya IP 'nin yeteneği yoktur.

Bir NSG, bilmeniz gereken bazı yerleşik kurallara sahiptir. Bunlar:

-   **Belirli bir sanal ağ içindeki tüm trafiğe izin ver:** Aynı Azure sanal ağındaki tüm VM 'Ler birbirleriyle iletişim kurabilir.

-   **Gelen Azure Yük dengelemeye izin ver:**  bu kural, Azure Yük Dengeleyici için herhangi bir kaynak adresinden herhangi bir hedef adrese giden trafiğe izin verir.

-   **Tüm geleni Reddet:**  bu kural, açıkça izin verilen Internet 'ten kaynaklandı tüm trafiği engeller.

-   **Internet 'e giden tüm trafiğe izin ver:** Bu kural, VM 'Lerin Internet bağlantılarını başlatmasına olanak tanır. Bu bağlantıların başlatılmasını istemiyorsanız, bu bağlantıları engellemek veya zorlamalı tüneli zorlamak için bir kural oluşturmanız gerekir.

#### <a name="system-routes-and-user-defined-routes"></a>Sistem rotaları ve Kullanıcı tanımlı yollar

Azure 'daki bir sanal ağa (VNet) sanal makineler (VM 'Ler) eklediğinizde, sanal makinelerin ağ üzerinden otomatik olarak iletişim kurabildiğini fark edersiniz. VM'ler farklı alt ağlarda bulunsa bile bir ağ geçidini belirtmenize gerek yoktur.

Aynı şey VM'lerden genel İnternet'e giden iletişimlerde ve hatta Azure'dan kendi veri merkezinize karma bir bağlantı bulunduğunda şirket içi ağınıza giden iletişimlerde de geçerlidir.

![Sistem Yolları](./media/network-security/azure-network-security-fig-9.png)

Bu iletişim akışının mümkün olmasını sağlayan şey, Azure'ın IP trafiğinin nasıl akacağını belirlemek için kullandığı bir dizi sistem yoludur. Sistem yolları aşağıdaki senaryolarda iletişim akışını denetler:

-   Aynı alt ağ içinden.

-   Bir sanal ağ içinde bir alt ağdan başka bir alt ağa.

-   VM'lerden İnternet'e.

-   Bir VPN ağ geçidi yoluyla bir sanal ağdan başka bir sanal ağa.

-   VNET eşlemesi ([hizmet zinciri](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview)) aracılığıyla VNET 'ten başka bir sanal ağa.

-   Bir VPN ağ geçidi yoluyla bir sanal ağdan şirket içi ağınıza.

Birçok kuruluş, belirli ilkeleri zorlamak üzere tüm ağ paketlerinin şirket içi incelemesini gerektiren katı güvenlik ve uyumluluk gereksinimlerine sahiptir. Azure, ExpressRoute veya VPN aracılığıyla özel bir yol veya [Sınır Ağ Geçidi Protokolü (BGP)](https://docs.microsoft.com/windows-server/remote/remote-access/bgp/border-gateway-protocol-bgp) reklamları oluşturarak sanal makinelerden şirket içine trafiği yönlendiren [Zorlamalı tünel](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-forced-tunneling) adlı bir mekanizma sağlar.

Zorlamalı tünel Azure'da sanal ağ kullanıcı tanımlı yollar (UDR) yapılandırılır. Bir şirket içi siteye trafik yönlendirme Azure VPN ağ geçidi için varsayılan bir yol olarak ifade edilir.

Aşağıdaki bölümde, Azure sanal ağı için yönlendirme tablosu ve yol geçerli sınırlama listelenmektedir:

- Her sanal ağ alt ağı, yerleşik bir sistem yönlendirme tablosu vardır. Sistem yönlendirme tablosu yolların aşağıdaki üç grup vardır:

  -  **Yerel VNet yolları:** Aynı sanal ağdaki hedef VM 'lere doğrudan

  - **Şirket içi yollar:** Azure VPN Gateway 'e

  -  **Varsayılan yol:** Doğrudan Internet 'e. Önceki iki yolun kapsamadığı özel IP adreslerine gidecek paketler bırakılır.

- Kullanıcı tanımlı yolların yayını ile, varsayılan bir yol eklemek için bir yönlendirme tablosu oluşturabilir ve ardından bu alt ağlarda Zorlamalı tünel sağlamak için yönlendirme tablosunu VNet alt ağınızla ilişkilendirebilirsiniz.

- "Sanal ağa bağlı bir varsayılan site" şirket içi yerel siteleri arasında ayarlamanız gerekir.

- Zorlamalı tünel dinamik yönlendirme VPN ağ geçidi (bir statik ağ geçidi sorunsuz değil) sahip bir sanal ağ ile ilişkilendirilmiş olması gerekir.

- Zorlamalı tünel ExpressRoute Bu mekanizma yapılandırılmamış, ancak bunun yerine, bir varsayılan rota üzerinden ExpressRoute BGP eşliği oturumlarını reklam tarafından etkinleştirilir.

> [!Note]
> Daha fazla bilgi için bkz. [ExpressRoute belgeleri](https://azure.microsoft.com/documentation/services/expressroute/) .

#### <a name="network-security-appliances"></a>Ağ güvenlik gereçleri
Ağ güvenlik grupları ve Kullanıcı tanımlı yollar, [OSI modelinin](https://en.wikipedia.org/wiki/OSI_model)ağ ve aktarım katmanlarındaki belirli bir ağ güvenliği ölçüsünü sağlayabildiği sırada, daha yüksek düzeylerde güvenliği etkinleştirmek istediğiniz durumlar olacaktır. ağ yığını. Bu gibi durumlarda, Azure iş ortakları tarafından sunulan sanal ağ güvenliği gereçlerini dağıtmanızı öneririz.

![Ağ güvenlik gereçleri](./media/network-security/azure-network-security-fig-10.png)

Azure ağ güvenlik araçları, VNet güvenlik ve ağ işlevlerini geliştirir ve [Azure Marketi](https://azuremarketplace.microsoft.com)aracılığıyla çok sayıda satıcı tarafından kullanılabilir. Bu sanal güvenlik gereçleri, şunları sağlamak üzere dağıtılabilir:

-   Yüksek oranda kullanılabilir güvenlik duvarları

-   Yetkisiz giriş önleme

-   Yetkisiz giriş algılama

-   Web uygulaması güvenlik duvarları (WAFs)

-   WAN iyileştirmesi

-   Yönlendirme

-   Yük dengeleme

-   VPN

-   Sertifika yönetimi

-   Active Directory

-   Çok faktörlü kimlik doğrulaması

#### <a name="application-gateway"></a>Uygulama ağ geçidi

[Microsoft Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) , bir hizmet olarak uygulama teslim DENETLEYICISI (ADC) sağlayan ayrılmış bir sanal gereç.

 ![Application Gateway](./media/network-security/azure-network-security-fig-11.png)

Application Gateway, uygulama ağ geçidine (SSL-boşaltma) CPU yoğun SSL sonlandırmasını boşaltarak Web grubu performansını ve kullanılabilirliğini iyileştirmenize olanak sağlar. Ayrıca aşağıdakiler dahil diğer katman 7 yönlendirme özelliklerini de sağlar:

-   Gelen trafiğin hepsini bir kez deneme dağıtımı

-   Tanımlama bilgisi tabanlı oturum benzeşimi

-   URL yolu tabanlı yönlendirme

-   Tek bir Application Gateway arkasında birden fazla Web sitesi barındırma yeteneği


Application Gateway 'in bir parçası olarak bir [Web uygulaması güvenlik duvarı (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) de sağlanır. Bu, yaygın web güvenlik açıklarına ve açıklardan yararlanan Web uygulamalarına yönelik koruma sağlar. Application Gateway Internet 'e yönelik ağ geçidi, yalnızca dahili ağ geçidi veya her ikisinin bir birleşimi olarak yapılandırılabilir.

Application Gateway WAF, algılama veya önleme modunda çalıştırılabilir. Yaygın kullanım durumu, yöneticilerin kötü amaçlı desenlerdeki trafiği gözlemlemek için algılama modunda çalışmasına yöneliktir. Kötü amaçlı yazılımlar algılandığında önleme modunu açmak şüpheli gelen trafiği engeller.

 ![Application Gateway](./media/network-security/azure-network-security-fig-12.png)

Ayrıca, WAF Application Gateway, WAF uyarılarını izlemek ve eğilimleri kolayca izlemek üzere [Azure izleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview) ve [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) ile tümleştirilmiş gerçek zamanlı bir WAF günlüğü kullanarak saldırılara karşı Web uygulamalarını izlemenize yardımcı olur.

JSON biçimli günlük doğrudan müşterinin depolama hesabına gider. Bu Günlükler üzerinde tam denetime sahipsiniz ve kendi bekletme ilkelerinizi uygulayacaksınız.

Ayrıca, [Azure günlük tümleştirmesi](https://aka.ms/AzLog)kullanarak bu günlükleri kendi analiz sisteminize alabilirsiniz. WAF günlükleri [Azure izleyici günlükleriyle](/azure/log-analytics/log-analytics-overview) de tümleştirilerek, gelişmiş ayrıntılı sorgular yürütmek Için Azure izleyici günlüklerini kullanabilirsiniz.

#### <a name="azure-web-application-firewall-waf"></a>Azure Web uygulaması güvenlik duvarı (WAF)

Web uygulamaları, SQL ekleme, siteler arası komut dosyası saldırıları ve [OWASP en iyi 10](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)' da görünen diğer saldırılar gibi yaygın olarak bilinen güvenlik açıklarından yararlanan çok sayıda kötü amaçlı saldırılara karşı artan hedeflerdir. Uygulamada bu tür güvenlik açıklarını önlemek, uygulama topolojisinin birden çok katmanında kapsamlı bakım, düzeltme eki uygulama ve izleme gerektirir.

 ![Azure Web uygulaması güvenlik duvarı (WAF)](./media/network-security/azure-network-security-fig-13.png)

Merkezi bir [Web uygulaması güvenlik duvarı (WAF)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) Web saldırılarına karşı koruma sağlayabilir ve herhangi bir uygulama değişikliğine gerek duymadan güvenlik yönetimini basitleştirir.

Bir WAF çözümü, bilinen bir güvenlik açığına merkezi bir konumda düzeltme eki uygulayarak güvenlik tehdidine karşı, web uygulamalarının her birinin güvenliğini sağlamaya göre daha hızlı tepki verebilir. Var olan uygulama ağ geçitleri, web uygulaması güvenlik duvarı bulunan bir uygulama ağ geçidine kolaylıkla dönüştürülebilir.

#### <a name="network-availability-controls"></a>Ağ kullanılabilirliği denetimleri

Microsoft Azure’u kullanarak ağ trafiğini dağıtmak için farklı seçenekler bulunur. Bu seçenekler birbirlerinden farklı şekilde çalışır, farklı özelliklere sahiptir ve farklı senaryoları destekler. Birbirlerinden ayrı olarak veya birleştirilerek kullanılabilirler.

Ağ kullanılabilirliği denetimleri aşağıda verilmiştir:

-   Azure Load Balancer

-   Application Gateway

-   Traffic Manager

**Azure yük dengeleyici**

Uygulamalarınıza yüksek düzeyde kullanılabilirlik ve ağ performansı sunar. Bu, gelen trafiği yük dengeli bir küme içinde tanımlanan sağlıklı hizmet örnekleri arasında dağıtan bir katman 4 (TCP, UDP) yük dengeleyicidir.

 ![Azure Load Balancer](./media/network-security/azure-network-security-fig-14.png)


Azure Load Balancer, şu şekilde yapılandırılabilir:

-   Sanal makinelere gelen Internet trafiğinin yükünü dengeleyin. Bu yapılandırma [Internet 'e yönelik yük dengeleme](https://docs.microsoft.com/azure/load-balancer/load-balancer-internet-overview)olarak bilinir.

-   Bir sanal ağ içindeki sanal makineler arasında, bulut hizmetlerindeki sanal makineler arasında veya şirket içi bilgisayarlar ile şirketler arası bir sanal ağdaki sanal makineler arasında yük dengeleme trafiği. Bu yapılandırma, [İç Yük Dengeleme](https://docs.microsoft.com/azure/load-balancer/load-balancer-internal-overview)olarak bilinir.

-   Dış trafiği belirli bir sanal makineye ilet.

Buluttaki tüm kaynakların Internet 'ten erişilebilen genel bir IP adresi olması gerekir. Azure 'daki bulut altyapısı, kaynakları için yönlendirilebilir olmayan IP adresleri kullanır. Azure, Internet ile iletişim kurmak için genel IP adresleriyle ağ adresi çevirisi (NAT) kullanır.

 **Uygulama ağ geçidi**

 Application Gateway, uygulama katmanında (OSı ağ başvurusu yığınında katman 7) işe yarar. İstemci bağlantısını sonlandıran ve istekleri arka uç noktalarına ileten ters proxy hizmeti olarak çalışır.

 **Traffic Manager**

Microsoft Azure Traffic Manager, farklı veri merkezlerindeki hizmet uç noktaları için Kullanıcı trafiğinin dağıtımını denetlemenize olanak tanır. Traffic Manager tarafından desteklenen hizmet uç noktaları, Azure VM 'Leri, Web Apps ve bulut hizmetlerini içerir. Traffic Manager’ı harici, Azure dışı uç noktalar için de kullanabilirsiniz.

Traffic Manager, istemci isteklerini bir [trafik yönlendirme yöntemine](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods) ve uç noktaların sistem durumuna göre en uygun uç noktaya yönlendirmek Için etki alanı adı sistemi 'NI (DNS) kullanır. Traffic Manager, farklı uygulama ihtiyaçlarına, uç nokta durumuna [izlemeye](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring)ve otomatik yük devretmeye uyacak bir dizi trafik yönlendirme yöntemi sağlar. Traffic Manager, bir Azure bölgesinin tamamının devre dışı kalması dahil olmak üzere hatalara dayanıklıdır.

Azure Traffic Manager, uygulama uç noktalarınız genelinde trafiğin dağıtımını denetlemenize olanak sağlar. Uç nokta, Azure içinde veya dışında barındırılan İnternet'e yönelik bir hizmettir.

Traffic Manager iki temel avantaj sağlar:

-   Trafiğin birkaç [trafik yönlendirme yönteminden](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods)birine göre dağıtılması.

-   Uç noktalar başarısız olduğunda, [uç nokta durumunu](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring) ve otomatik yük devretmeyi sürekli izleme.

Bir istemci bir hizmete bağlanmaya çalıştığında, önce hizmetin DNS adını bir IP adresine çözümlemelidir. İstemci daha sonra hizmete erişmek için o IP adresine bağlanır. Traffic Manager, istemcileri trafik yönlendirme yönteminin kurallarına göre belirli hizmet uç noktalarına yönlendirmek için DNS kullanır. İstemciler seçili uç noktaya doğrudan bağlanır. Traffic Manager bir proxy veya ağ geçidi değil. Traffic Manager istemci ile hizmet arasında geçen trafiği görmez.

### <a name="azure-network-validation"></a>Azure ağ doğrulaması

Azure ağ doğrulaması, Azure ağının yapılandırıldığı gibi çalıştığından ve ağın izlenmesi için kullanılabilen hizmetler ve özellikler kullanılarak doğrulanmasının yapılabileceğini sağlamaktır. Azure ağ Izleyicisi ile ağ performansınızı ve sistem durumunu anlamak için öngörülere yönelik bir günlük ve tanılama özellikleri Plethora erişebilirsiniz. Bu yeteneklere Portal, Power Shell, CLı, REST API ve SDK aracılığıyla erişilebilir.

Azure operasyonel güvenlik, kullanıcıların verilerini, uygulamalarını ve diğer varlıklarını Microsoft Azure korumak için kullanıcılara sunulan hizmetleri, denetimleri ve özellikleri ifade eder. Azure operasyonel güvenlik, Microsoft güvenlik geliştirme yaşam döngüsü (SDL), Microsoft Güvenlik Yanıt Merkezi programı da dahil olmak üzere Microsoft 'a özgü çeşitli yetenekler aracılığıyla elde edilen bilgileri içeren bir çerçeve üzerine kurulmuştur. ve siber güvenlik tehdidi yatay olarak ayrıntılı bir şekilde tanıma.

-   [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro)

-   [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

-   [Azure Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

-   [Azure depolama Analizi](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)

-   Azure Resource Manager

#### <a name="azure-resource-manager"></a>Azure Resource Manager

Microsoft Azure çalışan kişiler ve süreçler, büyük olasılıkla platformun en önemli güvenlik özelliğidir. Bu bölümde, Microsoft 'un küresel veri merkezi altyapısının güvenliği, sürekliliği ve gizliliği geliştirmeye ve korumaya yardımcı olan özellikler açıklanmaktadır.

Uygulamanızın altyapısı genellikle bir sanal makine, depolama hesabı, sanal ağ veya web uygulaması, veritabanı, veritabanı sunucusu ya da üçüncü taraf hizmetler gibi birçok bileşenden meydana gelir. Bu bileşenleri ayrı varlıklar olarak değerlendirmez, bunun yerine bunları tek bir varlığın ilgili ve birbirine bağımlı parçaları olarak kabul edersiniz. Bunları gruplar halinde dağıtmak, yönetmek ve izlemek isteyebilirsiniz. Azure Resource Manager, çözümünüzdeki kaynaklar ile gruplar halinde çalışmanıza olanak sağlar.

Çözümünüzdeki tüm kaynakları tek ve eşgüdümlü bir işlemle dağıtabilir, güncelleştirebilir veya silebilirsiniz. Dağıtım için bir şablon kullanabilirsiniz. Üstelik bu şablon test, hazırlık ve üretim gibi farklı ortamlarda da çalışabilir. Resource Manager kaynaklarınızı dağıttıktan sonra yönetmenize yardımcı olmak için güvenlik, denetleme ve etiketleme özellikleri sunar.

**Kaynak Yöneticisi kullanmanın avantajları**

Resource Manager çeşitli avantajlar sunar:

-   Çözümünüzdeki tüm kaynakları ayrı ayrı ele almak yerine bunları grup halinde dağıtabilir, yönetebilir ve izleyebilirsiniz.

-   Çözümünüzü geliştirme yaşam döngüsü boyunca defalarca dağıtabilirsiniz. Kaynaklarınızın tutarlı bir durumda dağıtılması da size güven verir.

-   Altyapınızı betikler yerine bildirim temelli şablonları kullanarak yönetebilirsiniz.

-   Kaynaklar arasında bağımlılıkları tanımlayabilir, bu nedenle doğru sırada dağıtılır.

-   Rol Tabanlı Erişim Denetimi (RBAC) yönetim platformuyla doğrudan tümleşik olduğu için kaynak grubunuzdaki tüm hizmetlere erişim denetimi uygulayabilirsiniz.

-   Aboneliğinizdeki tüm kaynakları mantıksal olarak düzenlemek için kaynaklarınıza etiketler ekleyebilirsiniz.

-   Bir grup kaynak paylaşımı etiketi için maliyetleri görüntüleyerek kuruluşunuzun faturalandırmasını açıklığa kavuşturun.

> [!Note]
> Resource Manager çözümlerinizi dağıtmanın ve yönetmenin yeni bir yolunu sunar. Önceki dağıtım modelini kullandıysanız ve değişiklikler hakkında bilgi edinmek isterseniz, bkz. [Resource Manager dağıtımını ve klasik dağıtımı anlama](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model)

## <a name="azure-network-logging-and-monitoring"></a>Azure ağ günlüğü ve izleme

Azure, ağ güvenliği olaylarını izlemek, engellemek, algılamak ve yanıtlamak için birçok araç sunmaktadır. Bu alanda kullanabileceğiniz en güçlü araçlardan bazıları şunlardır:

-   Ağ İzleyicisi

-   Ağ kaynak düzeyinde Izleme

-   Azure İzleyici günlükleri

### <a name="network-watcher"></a>Ağ izleyicisi

[Ağ İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) -senaryo tabanlı Izleme, ağ izleyicisinden özelliklerle sağlanır. Bu hizmet, paket yakalama, sonraki atlama, IP akışı doğrulama, güvenlik grubu görünümü, NSG akış günlükleri içerir. Senaryo düzeyi izleme, tek tek ağ kaynağı izlemeye karşılık olarak ağ kaynaklarının uçtan uca bir görünümünü sağlar.

 ![Ağ İzleyicisi](./media/network-security/azure-network-security-fig-15.png)

Ağ Izleyicisi, Azure 'daki ve Azure 'dan bir ağ senaryosu düzeyinde koşulları izlemenizi ve tanılamanıza olanak tanıyan bölgesel bir hizmettir. Ağ Izleyicisi ile kullanılabilen ağ tanılama ve görselleştirme araçları, Azure 'da ağınızı anlamanıza, tanılamanıza ve elde etmenize yardımcı olur.

Ağ Izleyicisi Şu anda aşağıdaki yeteneklere sahiptir:

#### <a name="topology"></a>Topoloji

[Topoloji](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview) , bir sanal ağdaki ağ kaynaklarının bir grafiğini döndürür. Grafik, uçtan uca ağ bağlantısını temsil edecek kaynaklar arasındaki bağlantıyı gösterir. Portalda topoloji, sanal ağ bazında kaynak nesnelerini döndürür. İlişkiler, kaynak grubu gösterilmese bile, ağ Izleyicisi bölgesinin dışındaki kaynaklar arasındaki satırlara göre gösterilmiştir. Portal görünümünde döndürülen kaynaklar, grafiği oluşturulan ağ bileşenlerinin bir alt kümesidir. Ağ kaynaklarının tam listesini görmek için [PowerShell](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-powershell) veya [rest](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-rest)kullanabilirsiniz.

Kaynaklar, bu bağlantı, iki ilişki altında modellenir.

- **Kapsama** -sanal ağ, NIC Içeren bir alt ağ içerir.

- **İlişkili** -bir NIC, bir VM ile ilişkilendirilmiştir.

#### <a name="variable-packet-capture"></a>Değişken paket yakalama

Ağ Izleyicisi [değişken paket yakalama](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) , bir sanal makineden gelen ve giden trafiği izlemek için paket yakalama oturumları oluşturmanıza olanak sağlar. Paket yakalama, hem yeniden etkin hem de Proactivity ağ anormallarını tanılamaya yardımcı olur. Diğer kullanımlar, istemci-sunucu iletişimlerinde hata ayıklamak ve çok daha fazlasını yapmak için ağ istatistiklerini toplamayı, ağ izinsiz kullanım hakkında bilgi kazanmanızı içerir.

Paket yakalama, ağ Izleyicisi aracılığıyla uzaktan başlatılan bir sanal makine uzantısıdır. Bu özellik, bir paket yakalamanın, istenen sanal makinede el ile çalıştırılması yükünü kolaylaştırır ve bu da değerli süreyi kaydeder. Paket yakalama, Portal, PowerShell, CLı veya REST API aracılığıyla tetiklenebilir. Paket yakalamanın tetiklenebilecek bir örnek sanal makine uyarılarıyla birlikte.

#### <a name="ip-flow-verify"></a>IP akışı doğrulama

[IP akışları](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) , 5 demet bilgilerine göre bir sanal makineye ya da bir pakete izin verilip verilmediğini denetler. Bu bilgiler yön, protokol, yerel IP, uzak IP, yerel bağlantı noktası ve uzak bağlantı noktasından oluşur. Paket bir güvenlik grubu tarafından reddedilirse, paketi reddeden kuralın adı döndürülür. Herhangi bir kaynak veya hedef IP 'si seçilebilir, bu özellik yöneticilerin internet 'ten veya şirket içi ortamdan gelen bağlantı sorunlarını hızla tanılamasına yardımcı olur.

IP akışları, bir sanal makinenin ağ arabirimini hedeflemesini doğrular. Trafik akışı daha sonra bu ağ arabiriminden veya bu arabirimden yapılandırılan ayarlara göre doğrulanır. Bu özellik, bir ağ güvenlik grubundaki bir kuralın bir sanal makineye giriş veya çıkış trafiğini engelleyip engellemediğini onaylamadığında yararlıdır.

#### <a name="next-hop"></a>Sonraki atlama

Azure ağ dokusunda yönlendirilmekte olan paketlerin bir [sonraki atlamasını](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) belirler ve bu, Kullanıcı tanımlı hatalı yapılandırılmış yolların tanılanmasına olanak tanır. Bir VM 'den gelen trafik, bir NIC ile ilişkili etkin yollara göre bir hedefe gönderilir. Sonraki atlama, belirli bir sanal makineden ve NIC 'den bir paketin sonraki atlama türünü ve IP adresini alır. Bu, paketin hedefe yönlendirilip yönlendirilmeyeceğini veya trafiğin siyah bir şekilde mi olduğunu belirlemenize yardımcı olur.

Sonraki atlama Ayrıca sonraki atlama ile ilişkili yol tablosunu da döndürür. Yol Kullanıcı tanımlı bir yol olarak tanımlanmışsa bir sonraki durak sorgulanırken, bu yol döndürülür. Aksi halde sonraki atlama "sistem rotası" döndürür.

#### <a name="security-group-view"></a>Güvenlik grubu görünümü

Bir VM 'ye uygulanan geçerli ve uygulanan güvenlik kurallarını alır. Ağ güvenlik grupları bir alt ağ düzeyiyle veya NIC düzeyinde ilişkilendirilir. Alt ağ düzeyinde ilişkilendirildiğinde, alt ağdaki tüm VM örnekleri için geçerli olur. Ağ [güvenlik grubu görünümü](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview) , yapılandırma hakkında bilgi sağlayan bir sanal makıne için NIC ve alt ağ düzeyinde ilişkili tüm yapılandırılmış NSG 'leri ve kuralları döndürür. Ayrıca, etkin güvenlik kuralları bir VM 'deki her NIC için döndürülür. Ağ güvenlik grubu görünümünü kullanarak, açık bağlantı noktaları gibi ağ güvenlik açıkları için bir sanal makineyi değerlendirebilirsiniz. Ayrıca, [yapılandırılmış ve etkin güvenlik kuralları arasındaki bir karşılaştırmaya](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-auditing-powershell)göre ağ güvenlik grubunuzun beklenen şekilde çalışıp çalışmadığını doğrulayabilirsiniz.

#### <a name="nsg-flow-logging"></a>NSG akış günlüğü

 Ağ güvenlik grupları için akış günlükleri, gruptaki güvenlik kuralları tarafından izin verilen veya reddedilen trafikle ilgili günlükleri yakalamanızı sağlar. Akış, 5 demet bilgi ile tanımlanır: kaynak IP, hedef IP, kaynak bağlantı noktası, hedef bağlantı noktası ve protokol.

[Ağ güvenlik grubu akış günlükleri](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) , bir ağ güvenlik grubu üzerinden giriş ve çıkış IP trafiği hakkındaki bilgileri görüntülemenize olanak tanıyan bir ağ izleyicisi özelliğidir.

#### <a name="virtual-network-gateway-and-connection-troubleshooting"></a>Sanal ağ geçidi ve bağlantı sorunlarını giderme

Ağ Izleyicisi, Azure 'daki ağ kaynaklarınızı kavramak üzere birçok özellik sağlar. Bu yeteneklerden biri kaynak sorun gidermeye sahiptir. [Kaynak sorunlarını giderme](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest) , POWERSHELL, clı veya REST API tarafından çağrılabilir. Çağrıldığında, ağ Izleyicisi bir sanal ağ geçidinin veya bir bağlantının sistem durumunu inceler ve bulgularını döndürür.

Bu bölüm, kaynak sorun giderme için şu anda kullanılabilir olan farklı yönetim görevlerinde size kılavuzluk eden bir görev gerçekleştirir.

-   [Sanal ağ geçidi sorunlarını giderme](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest)

-   [Bağlantı sorunlarını giderme](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest)

#### <a name="network-subscription-limits"></a>Ağ abonelik limitleri

[Ağ abonelik limitleri](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) , bir bölgedeki bir abonelikte bulunan her bir ağ kaynağının kullanımının ayrıntılarını, kullanılabilir maksimum kaynak sayısına göre sağlar.

#### <a name="configuring-diagnostics-log"></a>Tanılama günlüğünü yapılandırma

Ağ Izleyicisi bir [tanılama günlükleri](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) görünümü sağlar. Bu görünüm, tanılama günlüğünü destekleyen tüm ağ kaynaklarını içerir. Bu görünümden, ağ kaynaklarını kolay ve hızlı bir şekilde etkinleştirebilir ve devre dışı bırakabilirsiniz.

### <a name="network-resource-level-monitoring"></a>Ağ kaynak düzeyinde izleme

Kaynak düzeyinde izleme için aşağıdaki özellikler mevcuttur:

#### <a name="audit-log"></a>Denetleme günlüğü

Ağ yapılandırmasının bir parçası olarak gerçekleştirilen işlemler günlüğe kaydedilir. Bu denetim günlükleri, çeşitli karmaşıkların kurulabilmesi için gereklidir. Bu Günlükler Azure portal görüntülenebilir veya Power BI ya da üçüncü taraf araçları gibi Microsoft araçları kullanılarak alınabilir. Denetim günlükleri Portal, PowerShell, CLı ve REST API aracılığıyla kullanılabilir.

> [!Note]
> Denetim günlükleri hakkında daha fazla bilgi için bkz. [Kaynak Yöneticisi Ile denetim işlemleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
Denetim günlükleri tüm ağ kaynaklarında yapılan işlemler için kullanılabilir.


#### <a name="metrics"></a>Ölçümler

Ölçümler, bir dönemde toplanan performans ölçümleri ve sayaçlardır. Ölçümler Şu anda Application Gateway için kullanılabilir. Ölçümler, eşiğe göre uyarıları tetiklemek için kullanılabilir. Azure Application Gateway, arka uç havuzundaki tüm kaynakların sistem durumunu izler ve havuzdan sağlıksız olarak kabul edilen kaynakları otomatik olarak kaldırır. Application Gateway, sağlıksız örnekleri izlemeye devam eder ve mevcut olduktan sonra sistem durumu araştırmalarını yanıtladıktan sonra yeniden sağlıklı arka uç havuzuna ekler. Application Gateway, arka uç HTTP ayarlarında tanımlanan aynı bağlantı noktasıyla sistem durumu araştırmalarını gönderir. Bu yapılandırma, araştırmanın, müşterilerin arka uca bağlanmak için kullandığı bağlantı noktasını test edilmesini sağlar.

> [!Note]
> Ölçümlerin uyarı oluşturmak için nasıl kullanılabileceğini görüntülemek için bkz. [Application Gateway tanılama](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview) .

#### <a name="diagnostic-logs"></a>Tanılama günlükleri

Düzenli ve Spontaneous olayları, ağ kaynakları tarafından oluşturulur ve depolama hesaplarında günlüğe kaydedilir, bir olay hub 'ına veya Azure Izleyici günlüklerine gönderilir. Bu günlükler bir kaynağın sistem durumuna ilişkin öngörüler sağlar. Bu Günlükler, Power BI ve Azure Izleyici günlükleri gibi araçlarla görüntülenebilir. Tanılama günlüklerini görüntülemeyi öğrenmek için [Azure izleyici günlüklerini](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics)ziyaret edin.

Tanılama günlükleri [Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log), [ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log), rotalar ve [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)için kullanılabilir.

Ağ Izleyicisi bir tanılama günlükleri görünümü sağlar. Bu görünüm, tanılama günlüğünü destekleyen tüm ağ kaynaklarını içerir. Bu görünümden, ağ kaynaklarını kolay ve hızlı bir şekilde etkinleştirebilir ve devre dışı bırakabilirsiniz.

### <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

[Azure izleyici günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) , Azure 'da bulut ve şirket içi ortamlarınızı izleyen ve kullanılabilirlik ve performanslarını koruyacak bir hizmettir. Birden fazla kaynak arasında analiz sağlamak üzere bulut ve şirket içi ortamlarınızdaki kaynaklar ile diğer izleme araçları tarafından oluşturulan verileri toplar.

Azure Izleyici günlükleri, ağlarınızı izlemek için aşağıdaki çözümleri sunar:

-   Ağ Performansı İzleyicisi (NPM)

-   Azure Application Gateway Analizi

-   Azure ağ güvenlik grubu Analizi

#### <a name="network-performance-monitor-npm"></a>Ağ Performansı İzleyicisi (NPM)
[Ağ performansı İzleyicisi](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor) yönetim çözümü, ağların sistem durumunu, kullanılabilirliğini ve erişilebilirliğini izleyen bir ağ izleme çözümüdür.

Arasındaki bağlantıyı izlemek için kullanılır:

-   genel bulut ve şirket içi

-   veri merkezleri ve kullanıcı konumları (şube ofisleri)

-   çok katmanlı bir uygulamanın çeşitli katmanlarını barındıran alt ağlar.


#### <a name="azure-application-gateway-analytics-in-azure-monitor-logs"></a>Azure Izleyici günlüklerinde Azure Application Gateway Analytics

Uygulama ağ geçitleri için aşağıdaki Günlükler desteklenir:

-   ApplicationGatewayAccessLog

-   ApplicationGatewayPerformanceLog

-   ApplicationGatewayFirewallLog

Uygulama ağ geçitleri için aşağıdaki ölçümler desteklenir:

-   5 dakikalık aktarım hızı

#### <a name="azure-network-security-group-analytics-in-azure-monitor-logs"></a>Azure Izleyici günlüklerinde Azure ağ güvenlik grubu Analizi

Aşağıdaki Günlükler [ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log)için desteklenir:

- **NetworkSecurityGroupEvent:** VM 'lere ve MAC adresine göre örnek rollere uygulanan NSG kurallarının girdilerini içerir. Bu kuralların durumu her 60 saniyede toplanır.

- **NetworkSecurityGroupRuleCounter:** Her NSG kuralının, trafiği reddetme veya izin verme için kaç kez uygulanacağını gösteren girişleri içerir.

## <a name="next-steps"></a>Sonraki adımlar
Ayrıntılı güvenlik konularımızın bazılarını okuyarak güvenlik hakkında daha fazla bilgi edinin:

-   [Ağ güvenlik grupları (NSG 'ler) için Azure Izleyici günlükleri](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log)

-   [Bulut kesintisini sağlayan ağ yenilikleri](https://azure.microsoft.com/blog/networking-innovations-that-drive-the-cloud-disruption/)

-   [SONiC Microsoft genel bulutunu destekleyen ağ anahtar yazılımı](https://azure.microsoft.com/blog/sonic-the-networking-switch-software-that-powers-the-microsoft-global-cloud/)

-   [Microsoft, hızlı ve güvenilir küresel ağı nasıl oluşturur](https://azure.microsoft.com/blog/how-microsoft-builds-its-fast-and-reliable-global-network/)

-   [Ağ yeniliklerini aydınlatma](https://azure.microsoft.com/blog/lighting-up-network-innovation/)
