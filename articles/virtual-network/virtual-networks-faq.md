---
title: Azure Sanal Ağ SSS
titlesuffix: Azure Virtual Network
description: Microsoft Azure sanal ağları hakkında en sık sorulan soruların yanıtları.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/12/2019
ms.author: kumud
ms.openlocfilehash: e45d5393833973889b28a95ec86b89593a091f99
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244816"
---
# <a name="azure-virtual-network-frequently-asked-questions-faq"></a>Azure Sanal Ağ hakkında sık sorulan sorular (SSS)

## <a name="virtual-network-basics"></a>Sanal Ağ temelleri

### <a name="what-is-an-azure-virtual-network-vnet"></a>Azure Sanal Ağ (VNet) nedir?
Azure Sanal Ağı (VNet), bulutta kendi ağınızın bir temsilidir. Azure bulutunun aboneliğinize adanmış mantıksal bir yalıtımıdır. Azure'da sanal özel ağları (VPN'ler) sağlamak ve yönetmek ve isteğe bağlı olarak, VNets'leri Azure'daki diğer VNet'lere veya karma veya şirket içi çözümler oluşturmak için şirket içi BT altyapınızla bağlamak için VNets'i kullanabilirsiniz. Oluşturduğunuz her VNet'in kendi CIDR bloğu vardır ve CIDR blokları çakışmadığı sürece diğer VNet'lere ve şirket içi ağlara bağlanabilir. Ayrıca VNets için DNS sunucu ayarları nın denetimine ve VNet'in alt ağlara bölümletilmesine de sahipsiniz.

VNets'i şu şekilde kullanın:

* Özel bir özel bulut ayarı yalnızca VNet oluşturun. Bazen çözümünüz için bir tesis içi yapılandırma gerekmez. Bir VNet oluşturduğunuzda, VNet'iniz deki hizmetleriniz ve VM'leriniz bulutta birbirleriyle doğrudan ve güvenli bir şekilde iletişim kurabilir. Çözümünüzün bir parçası olarak, Internet iletişimi gerektiren VM'ler ve hizmetler için bitiş noktası bağlantılarını yapılandırmaya devam edebilirsiniz.

* Veri merkezinizi güvenli bir şekilde genişletin. VNets ile, veri merkezi kapasitenizi güvenli bir şekilde ölçeklendirmek için geleneksel siteden siteye (S2S) VPN'ler oluşturabilirsiniz. S2S VPN'ler, kurumsal VPN ağ geçidiniz ile Azure arasında güvenli bir bağlantı sağlamak için IPSEC'i kullanır.

* Karma bulut senaryolarını etkinleştirin. VNets, çeşitli karma bulut senaryolarını destekleme esnekliği sağlar. Bulut tabanlı uygulamaları ana bilgisayarlar ve Unix sistemleri gibi her türlü şirket içi sisteme güvenli bir şekilde bağlayabilirsiniz.

### <a name="how-do-i-get-started"></a>Nasıl kullanmaya başlayabilirim?
Başlamak için [Sanal ağ belgelerini](https://docs.microsoft.com/azure/virtual-network/) ziyaret edin. Bu içerik, tüm VNet özellikleri için genel bakış ve dağıtım bilgileri sağlar.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>VNets'i tesisler arası bağlantı olmadan kullanabilir miyim?
Evet. Bir VNet'i tesisinize bağlamadan kullanabilirsiniz. Örneğin, Microsoft Windows Server Active Directory etki alanı denetleyicilerini ve SharePoint büyüklemlerini yalnızca bir Azure VNet'te çalıştırabilirsiniz.

### <a name="can-i-perform-wan-optimization-between-vnets-or-a-vnet-and-my-on-premises-data-center"></a>VNet veya VNet ile şirket içi veri merkezim arasında WAN optimizasyonu gerçekleştirebilir miyim?
Evet. Azure Marketi aracılığıyla birkaç satıcıdan bir [WAN optimizasyon ağı sanal cihazı](https://azuremarketplace.microsoft.com/en-us/marketplace/?term=wan%20optimization) dağıtabilirsiniz.

## <a name="configuration"></a>Yapılandırma

### <a name="what-tools-do-i-use-to-create-a-vnet"></a>VNet oluşturmak için hangi araçları kullanırım?
Bir VNet oluşturmak veya yapılandırmak için aşağıdaki araçları kullanabilirsiniz:

* Azure portalında
* PowerShell
* Azure CLI
* Bir ağ yapılandırma dosyası (netcfg - sadece klasik VNets için). Bir [ağ yapılandırma dosyası makalesini kullanarak Bir VNet Yapılandır'a](virtual-networks-using-network-configuration-file.md) bakın.

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>VNet'lerimde hangi adres aralıklarını kullanabilirim?
[RFC 1918'de](https://tools.ietf.org/html/rfc1918)tanımlanan herhangi bir IP adresi aralığı. Örneğin, 10.0.0.0/16. Aşağıdaki adres aralıklarını ekleyemezsiniz:
* 224.0.0.0/4 (Çok Noktaya Yayın)
* 255.255.255.255/32 (Yayın)
* 127.0.0.0/8 (Geri Dönüş)
* 169.254.0.0/16 (Bağlantı-yerel)
* 168.63.129.16/32 (Dahili DNS)

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>VNet'lerimde herkese açık IP adresleri olabilir miyim?
Evet. Genel IP adresi aralıkları hakkında daha fazla bilgi için [bkz.](manage-virtual-network.md#create-a-virtual-network) Genel IP adreslerine internetten doğrudan erişilemez.

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-vnet"></a>VNet'imdeki alt ağ sayısının bir sınırı var mı?
Evet. Ayrıntılar için [Azure sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) bakın. Alt net adres boşlukları birbiriyle çakışamaz.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Bu alt ağlarda IP adreslerinin kullanılmasında herhangi bir kısıtlama var mı?
Evet. Azure, her alt ağ içindeki 5 IP adresini ayırır. Bunlar x.x.x.0-x.x.x.3 ve alt netin son adresidir. x.x.x.1-x.x.x.3, Azure hizmetleri için her alt ağda ayrılmıştır.   
- x.x.x.0: Ağ adresi
- x.x.x.1: Varsayılan ağ geçidi için Azure tarafından ayrılmıştır
- x.x.x.2, x.x.x.3: Azure DNS IP'lerini VNet alanıyla eşlemek için Azure tarafından ayrılmıştır
- x.x.x.255: Ağ yayın adresi

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>VNets ve alt ağlar ne kadar küçük ve ne kadar büyük olabilir?
Desteklenen en küçük IPv4 alt ağı /29 ve en büyüğü /8 'dir (CIDR alt ağ tanımlarını kullanarak).  IPv6 alt ağları tam /64 boyutunda olmalıdır.  

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>VNET'lerimi VNets kullanarak Azure'a getirebilir miyim?
Hayır. VNets Katman-3 kaplamaları vardır. Azure herhangi bir Katman-2 anlambilimini desteklemez.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>VNet'lerimde ve alt ağlarımda özel yönlendirme ilkeleri belirtebilir miyim?
Evet. Bir rota tablosu oluşturabilir ve bir alt ağla ilişkilendirebilirsiniz. Azure'da yönlendirme hakkında daha fazla bilgi için [Yönlendirme'ye genel bakış](virtual-networks-udr-overview.md#custom-routes)bilgisine bakın.

### <a name="do-vnets-support-multicast-or-broadcast"></a>VNet'ler çok yayını veya yayını destekliyor mu?
Hayır. Çok yayınlı yayın desteklenmez.

### <a name="what-protocols-can-i-use-within-vnets"></a>VNets içinde hangi protokolleri kullanabilirim?
VNets içinde TCP, UDP ve ICMP TCP/IP protokollerini kullanabilirsiniz. Unicast, Unicast (kaynak bağlantı noktası UDP/68 / hedef port UDP/67) üzerinden Dinamik Ana Bilgisayar Yapılandırma Protokolü (DHCP) hariç VNets içinde desteklenir. Çok noktaya yayın, IP kapsüllü paketler ve Genel Yönlendirme Kapsülleme (GRE) paketleri VNets içinde engellenir. 

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Varsayılan yönlendiricilerimi Bir VNet içinde pingleyebilir miyim?
Hayır.

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>Bağlantıyı tanılamak için tracert kullanabilir miyim?
Hayır.

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>VNet oluşturulduktan sonra alt ağlar ekleyebilir miyim?
Evet. Alt ağ aralığı başka bir alt ağın parçası olmadığı ve sanal ağın adres aralığında kalan boş alan olduğu sürece, alt ağlar vnet'lere herhangi bir zamanda eklenebilir.

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>Oluşturduktan sonra alt ağımın boyutunu değiştirebilir miyim?
Evet. Alt ağın içinde dağıtılmış VM'ler veya hizmetler yoksa alt ağı ekleyebilir, kaldırabilir, genişletebilir veya daraltabilirsiniz.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Alt ağları oluşturduktan sonra değiştirebilir miyim?
Evet. Bir VNet tarafından kullanılan CIDR bloklarını ekleyebilir, kaldırabilir ve değiştirebilirsiniz.

### <a name="if-i-am-running-my-services-in-a-vnet-can-i-connect-to-the-internet"></a>Hizmetlerimi bir VNet'te çalıştırıyorsam, internete bağlanabilir miyim?
Evet. Bir VNet içinde dağıtılan tüm hizmetler giden internete bağlanabilir. Azure'da giden internet bağlantıları hakkında daha fazla bilgi edinmek için [Giden bağlantılara](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın. Kaynak Yöneticisi aracılığıyla dağıtılan bir kaynağa bağlanmak istiyorsanız, kaynağın herkese açık bir IP adresi olması gerekir. Genel IP adresleri hakkında daha fazla bilgi edinmek için [Genel IP adreslerine](virtual-network-public-ip-address.md)bakın. Azure'da dağıtılan her Azure Bulut Hizmeti'nin herkese açık bir VIP'si vardır. Bu hizmetlerin internetten bağlantıları kabul etmesini sağlamak için Sanal makineler için PaaS rolleri ve uç noktaları için giriş uç noktaları tanımlarsınız.

### <a name="do-vnets-support-ipv6"></a>VNets IPv6'yı destekliyor mu?
Evet, VNet'ler yalnızca IPv4 veya çift yığın (IPv4+IPv6) olabilir.  Ayrıntılar için Azure [Sanal Ağlar için IPv6'ya Genel Bakış bölümüne](./ipv6-overview.md)bakın.

### <a name="can-a-vnet-span-regions"></a>Bir VNet bölgeleri kaplayabilir mi?
Hayır. VNet tek bir bölgeyle sınırlıdır. Ancak, sanal ağ kullanılabilirlik bölgelerini kapsar. Kullanılabilirlik alanları hakkında daha fazla bilgi için bkz. [Kullanılabilirlik alanlarına genel bakış](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Farklı bölgelerdeki sanal ağları sanal ağ eşlemesiyle bağlayabilirsiniz. Ayrıntılar için [Sanal ağ ağına genel bakış](virtual-network-peering-overview.md)

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>Azure'daki başka bir VNet'e Bir VNet bağlayabilir miyim?
Evet. Aşağıdakilerden birini kullanarak bir VNet'i başka bir VNet'e bağlayabilirsiniz:
- **Sanal ağ eşleme**: Ayrıntılar için [VNet'e genel bakış](virtual-network-peering-overview.md)
- **Azure VPN Ağ Geçidi**: Ayrıntılar için [VNet'den VNet bağlantısına yapılandırışla' ya](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın. 

## <a name="name-resolution-dns"></a>Ad Çözümlemesi (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>VNets için DNS seçeneklerim nelerdir?
Kullanılabilir tüm DNS seçeneklerinde size rehberlik etmek [için VM'ler ve Rol Örnekleri](virtual-networks-name-resolution-for-vms-and-role-instances.md) için Ad Çözümü sayfasındaki karar tablosunu kullanın.

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>Bir VNet için DNS sunucularını belirtebilir miyim?
Evet. VNet ayarlarında DNS sunucu IP adreslerini belirtebilirsiniz. Ayar, VNet'teki tüm VM'ler için varsayılan DNS sunucusu(lar) olarak uygulanır.

### <a name="how-many-dns-servers-can-i-specify"></a>Kaç Tane DNS sunucusu belirtebilirim?
Başvuru [Azure sınırları](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits).

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>Ağı oluşturduktan sonra DNS sunucularımı değiştirebilir miyim?
Evet. VNet'inizin DNS sunucu listesini istediğiniz zaman değiştirebilirsiniz. DNS sunucu listenizi değiştirirseniz, yeni DNS ayarlarının etkili olması için VNet'teki etkilenen tüm VM'lerde DHCP kira yenilemesi gerçekleştirmeniz gerekir. Windows işletim sistemi çalıştıran VM'ler için bunu doğrudan VM'ye yazarak `ipconfig /renew` yapabilirsiniz. Diğer işletim sistemi türleri için, belirli işletim sistemi türü için DHCP kira yenileme belgelerine bakın. 

### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>Azure tarafından sağlanan DNS nedir ve VNets ile çalışır?
Azure tarafından sağlanan DNS, Microsoft tarafından sunulan çok kiracılı bir DNS hizmetidir. Azure, bu hizmetteki tüm VM'lerinizi ve bulut hizmeti rol örneklerini kaydeder. Bu hizmet, aynı bulut hizmetinde bulunan VM'ler ve rol örnekleri için ana bilgisayar adına ve aynı VNet'teki VM'ler ve rol örnekleri için FQDN'ye göre ad çözümlemesi sağlar. DNS hakkında daha fazla bilgi edinmek [için, VM'ler ve Bulut Hizmetleri rol örnekleri için Ad Çözümlemesi'ne](virtual-networks-name-resolution-for-vms-and-role-instances.md)bakın.

Azure tarafından sağlanan DNS'yi kullanarak kiracılar arası ad çözümlemesi için Bir VNet'teki ilk 100 bulut hizmetinde bir sınırlama vardır. Kendi DNS sunucunuzu kullanıyorsanız, bu sınırlama geçerli değildir.

### <a name="can-i-override-my-dns-settings-on-a-per-vm-or-cloud-service-basis"></a>DNS ayarlarımı VM veya bulut hizmeti bazında geçersiz kılabilir miyim?
Evet. Varsayılan ağ ayarlarını geçersiz kılmak için DNS sunucularını VM veya bulut hizmeti başına ayarlayabilirsiniz. Ancak, ağ genelinde mümkün olduğunca DNS kullanmanız önerilir.

### <a name="can-i-bring-my-own-dns-suffix"></a>Kendi DNS sonekimi getirebilir miyim?
Hayır. VNet'leriniz için özel bir DNS soneki belirtemezsiniz.

## <a name="connecting-virtual-machines"></a>Sanal makineleri bağlama

### <a name="can-i-deploy-vms-to-a-vnet"></a>VNet'e VM dağıtabilir miyim?
Evet. Kaynak Yöneticisi dağıtım modeli aracılığıyla dağıtılan bir VM'ye bağlı tüm ağ arabirimleri (NIC) bir VNet'e bağlanmalıdır. Klasik dağıtım modeli aracılığıyla dağıtılan VM'ler isteğe bağlı olarak bir VNet'e bağlanabilir.

### <a name="what-are-the-different-types-of-ip-addresses-i-can-assign-to-vms"></a>VM'lere atayabileceğim farklı IP adresleri nelerdir?
* **Özel:** Her VM içindeki her NIC'e atanmış. Adres statik veya dinamik yöntem kullanılarak atanır. Özel IP adresleri, VNet'inizin alt net ayarlarında belirttiğiniz aralıktan atanır. Klasik dağıtım modeli aracılığıyla dağıtılan kaynaklara, bir VNet'e bağlı olmasalar bile özel IP adresleri atanır. Ayırma yönteminin davranışı, kaynak yöneticisi yle mi yoksa klasik dağıtım modeliyle mi dağıtıldığına bağlı olarak farklıdır: 

  - **Kaynak Yöneticisi**: Dinamik veya statik yöntemle atanan özel bir IP adresi, kaynak silinene kadar sanal bir makineye (Kaynak Yöneticisi) atanmaya devam eder. Aradaki fark, statik kullanırken atamak için adresi seçmeniz ve Azure dinamik kullanırken seçer. 
  - **Klasik**: Dinamik yöntemle atanan özel bir IP adresi, sanal bir makine (klasik) VM durduruldu (ayrılmış) durumda olduktan sonra yeniden başlatıldığında değişebilir. Klasik dağıtım modeli aracılığıyla dağıtılan bir kaynağın özel IP adresinin asla değişmediğinden emin olmanız gerekiyorsa, statik yöntemle özel bir IP adresi atayın.

* **Genel:** Azure Kaynak Yöneticisi dağıtım modeli aracılığıyla dağıtılan VM'lere bağlı NIC'lere isteğe bağlı olarak atanmış. Adres statik veya dinamik ayırma yöntemi ile atanabilir. Klasik dağıtım modeli aracılığıyla dağıtılan tüm VM'ler ve Bulut Hizmetleri rol örnekleri, *dinamik,* genel sanal IP (VIP) adresi atanan bir bulut hizmeti içinde bulunur. [Ayrılmış IP adresi](virtual-networks-reserved-public-ip.md)olarak adlandırılan genel *statik* IP adresi isteğe bağlı olarak VIP olarak atanabilir. Genel IP adreslerini, klasik dağıtım modeli aracılığıyla dağıtılan tek tek VM'lere veya Bulut Hizmetleri rol örneklerine atayabilirsiniz. Bu adreslere [Örnek düzeyi ortak IP (ILPIP](virtual-networks-instance-level-public-ip.md) adresleri) adı verilir ve dinamik olarak atanabilir.

### <a name="can-i-reserve-a-private-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Daha sonra oluşturacağım bir VM için özel bir IP adresi rezerve edebilir miyim?
Hayır. Özel bir IP adresi rezerve edemezsiniz. Özel bir IP adresi varsa, DHCP sunucusu tarafından bir VM veya rol örneğine atanır. VM, özel IP adresinin atanmasını istediğiniz kişi olabilir veya olmayabilir. Ancak, zaten oluşturulmuş bir VM'nin özel IP adresini kullanılabilir herhangi bir özel IP adresiyle değiştirebilirsiniz.

### <a name="do-private-ip-addresses-change-for-vms-in-a-vnet"></a>VNet'teki VM'ler için özel IP adresleri değişir mi?
Duruma göre değişir. VM Kaynak Yöneticisi aracılığıyla dağıtıldıysa, IP adresinin statik veya dinamik ayırma yöntemiyle atanmış olup olmadığına bakılmaksızın hayır. VM klasik dağıtım modeli aracılığıyla dağıtıldıysa, durdurulan (ayrılan) durumda olduktan sonra bir VM başlatıldığında dinamik IP adresleri değişebilir. Adres, VM silindiğinde her iki dağıtım modeli aracılığıyla dağıtılan bir VM'den serbest bırakılır.

### <a name="can-i-manually-assign-ip-addresses-to-nics-within-the-vm-operating-system"></a>VM işletim sistemindeKI NIC'lere IP adreslerini el ile atayabilir miyim?
Evet, ancak sanal bir makineye birden çok IP adresi atamak gibi gerekli olmadıkça önerilmez. Ayrıntılar için bkz: [Sanal makineye birden çok IP adresi ekleme.](virtual-network-multiple-ip-addresses-portal.md#os-config) VM'ye bağlı bir Azure NIC'ine atanan IP adresi değişirse ve VM işletim sistemindeki IP adresi farklıysa, VM'ye bağlantı kaybedersiniz.

### <a name="if-i-stop-a-cloud-service-deployment-slot-or-shutdown-a-vm-from-within-the-operating-system-what-happens-to-my-ip-addresses"></a>Bir Bulut Hizmeti dağıtım yuvasını durdurursam veya işletim sistemi içinden bir VM'yi kapattırırsam, IP adreslerime ne olur?
Hiçbir şey. IP adresleri (genel VIP, genel ve özel) bulut hizmeti dağıtım yuvasına veya VM'ye atanmış durumdadır.

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-redeploying"></a>VM'leri yeniden dağıtmadan VNet'teki bir alt ağdan başka bir alt ağa taşıyabilir miyim?
Evet. [VM veya rol örneğini farklı bir alt ağ makalesine nasıl taşıyabileceğinizkonusunda](virtual-networks-move-vm-role-to-subnet.md) daha fazla bilgi bulabilirsiniz.

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>VM'im için statik bir MAC adresi yapılandırabilir miyim?
Hayır. MAC adresi statik olarak yapılandırılamaz.

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-its-created"></a>Mac adresi oluşturulduktan sonra VM için aynı kalacak mı?
Evet, MAC adresi silinene kadar hem Kaynak Yöneticisi hem de klasik dağıtım modelleri aracılığıyla dağıtılan bir VM için aynı kalır. Daha önce, VM durdurulduysa (ayrılmışsa) MAC adresi serbest bırakılmıştı, ancak şimdi VM ayrılan durumdayken bile MAC adresi korunur. Ağ arabirimi silinene veya birincil ağ arabiriminin birincil IP yapılandırmasına atanan özel IP adresi değiştirilene kadar MAC adresi ağ arabirimine atanmaya devam eder. 

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>VNet'teki bir VM'den internete bağlanabilir miyim?
Evet. Bir VNet içinde dağıtılan tüm VM'ler ve Bulut Hizmetleri rol örnekleri Internet'e bağlanabilir.

## <a name="azure-services-that-connect-to-vnets"></a>VNets'e bağlanan Azure hizmetleri

### <a name="can-i-use-azure-app-service-web-apps-with-a-vnet"></a>Azure App Service Web Apps'ı VNet ile kullanabilir miyim?
Evet. Web Apps'ı bir ASE (App Service Environment) kullanarak Bir VNet içinde dağıtabilir, uygulamalarınızın arka ucunu VNet Tümleştirme ile VNet'lerinize bağlayabilir ve hizmet bitiş noktalarıyla uygulamanız için gelen trafiği kilitleyebilirsiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [App Service ağ özellikleri](../app-service/networking-features.md)
* [Uygulama Hizmeti Ortamında Web Uygulamaları Oluşturma](../app-service/environment/app-service-web-how-to-create-a-web-app-in-an-ase.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
* [Uygulamanızı bir Azure Sanal Ağı ile tümleştirme](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
* [Uygulama Hizmeti erişim kısıtlamaları](../app-service/app-service-ip-restrictions.md)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Bulut Hizmetlerini web ve çalışan rolleri (PaaS) ile bir VNet'te dağıtabilir miyim?
Evet. Bulut Hizmetleri rol örneklerini VNets içinde dağıtabilirsiniz (isteğe bağlı olarak). Bunu yapmak için, hizmet yapılandırmanızın ağ yapılandırma bölümünde VNet adını ve rol/alt ağ eşlemelerini belirtirsiniz. İkili günlüklerinizi güncellemeniz gerekmez.

### <a name="can-i-connect-a-virtual-machine-scale-set-to-a-vnet"></a>Sanal makine ölçeği kümesini VNet'e bağlayabilir miyim?
Evet. Sanal makine ölçeği kümesini bir VNet'e bağlamanız gerekir.

### <a name="is-there-a-complete-list-of-azure-services-that-can-i-deploy-resources-from-into-a-vnet"></a>Kaynakları bir VNet'e dağıtabileceğim Azure hizmetlerinin tam bir listesi var mı?
Evet, ayrıntılar [için Azure hizmetleri için Sanal ağ tümleştirmesi'ne](virtual-network-for-azure-services.md)bakın.

### <a name="how-can-i-restrict-access-to-azure-paas-resources-from-a-vnet"></a>Bir VNet'ten Azure PaaS kaynaklarına erişimi nasıl kısıtlayabilirim?

Bazı Azure PaaS hizmetleri (Azure Depolama ve Azure SQL Veritabanı gibi) aracılığıyla dağıtılan kaynaklar, sanal ağ hizmeti bitiş noktaları veya Azure Özel Bağlantı'yı kullanarak VNet'e ağ erişimini kısıtlayabilir. Ayrıntılar için Bkz. [Sanal ağ hizmeti uç noktalarına genel bakış](virtual-network-service-endpoints-overview.md), Azure Özel Bağlantı genel [bakışı](../private-link/private-link-overview.md)

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Hizmetlerimi VNets'e girip çıkabilir miyim?
Hayır. Hizmetleri VNets'e girip çıkaramazsınız. Bir kaynağı başka bir VNet'e taşımak için kaynağı silmeniz ve yeniden dağıtmanız gerekir.

## <a name="security"></a>Güvenlik

### <a name="what-is-the-security-model-for-vnets"></a>VNets için güvenlik modeli nedir?
VNet'ler birbirinden izole edilir ve Azure altyapısında barındırılan diğer hizmetler. VNet bir güven sınırıdır.

### <a name="can-i-restrict-inbound-or-outbound-traffic-flow-to-vnet-connected-resources"></a>Gelen veya giden trafik akışını VNet'e bağlı kaynaklarla sınırlayabilir miyim?
Evet. [Ağ Güvenlik Grupları'nı](security-overview.md) bir VNet içindeki tek tek alt ağlara, bir VNet'e bağlı NIC'lere veya her ikisine de uygulayabilirsiniz.

### <a name="can-i-implement-a-firewall-between-vnet-connected-resources"></a>VNet'e bağlı kaynaklar arasında bir güvenlik duvarı uygulayabilir miyim?
Evet. Azure Marketi aracılığıyla birkaç satıcıdan gelen bir [güvenlik duvarı ağı sanal cihazı](https://azure.microsoft.com/marketplace/?term=firewall) dağıtabilirsiniz.

### <a name="is-there-information-available-about-securing-vnets"></a>VNet'leri güvence altına alma hakkında bilgi var mı?
Evet. Ayrıntılar için Azure [Ağ Güvenliğine Genel Bakış'a](../security/fundamentals/network-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın.

## <a name="apis-schemas-and-tools"></a>API'ler, şemalar ve araçlar

### <a name="can-i-manage-vnets-from-code"></a>VNets'i koddan yönetebilir miyim?
Evet. [Azure Kaynak Yöneticisi](/rest/api/virtual-network) ve [klasik](https://go.microsoft.com/fwlink/?LinkId=296833) dağıtım modellerinde VNets için REST API'lerini kullanabilirsiniz.

### <a name="is-there-tooling-support-for-vnets"></a>VNets için araç desteği var mı?
Evet. Kullanma hakkında daha fazla bilgi edinin:
- [Azure Kaynak Yöneticisi](manage-virtual-network.md#create-a-virtual-network) ve [klasik](virtual-networks-create-vnet-classic-pportal.md) dağıtım modelleri aracılığıyla VNet dağıtmak için Azure portalı.
- [PowerShell, Kaynak Yöneticisi](/powershell/module/az.network) ve [klasik](/powershell/module/servicemanagement/azure/?view=azuresmps-3.7.0) dağıtım modelleri aracılığıyla dağıtılan VNet'leri yönetmek için.
- [Kaynak Yöneticisi](/cli/azure/network/vnet) ve [klasik](../virtual-machines/azure-cli-arm-commands.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-resources) dağıtım modelleri aracılığıyla dağıtılan VNet'leri dağıtmak ve yönetmek için Azure komut satırı arabirimi (CLI).  

## <a name="vnet-peering"></a>VNet eşlemesi

### <a name="what-is-vnet-peering"></a>VNet'in bakışları nedir?
VNet eşleme (veya sanal ağ eşleme) sanal ağları bağlamanızı sağlar. Sanal ağlar arasındaki VNet bakış bağlantısı, aralarındaki trafiği IPv4 adresleri üzerinden özel olarak yönlendirmenize olanak tanır. Eşlenen VNet'lerindeki sanal makineler, aynı ağ içindelermiş gibi birbirleriyle iletişim kurabilir. Bu sanal ağlar aynı bölgede veya farklı bölgelerde (Global VNet Eşleme olarak da bilinir) olabilir. Azure abonelikleri genelinde VNet eşleme bağlantıları da oluşturulabilir.

### <a name="can-i-create-a-peering-connection-to-a-vnet-in-a-different-region"></a>Farklı bir bölgedeki bir VNet'e bir bakış bağlantısı oluşturabilir miyim?
Evet. Global VNet eşleme, farklı bölgelerdeki VNet'lere bakmanızı sağlar. Global VNet eşleme, tüm Azure genel bölgelerinde, Çin bulut bölgelerinde ve Devlet bulut bölgelerinde kullanılabilir. Azure genel bölgelerinden ulusal bulut bölgelerine genel olarak eşleyemezsiniz.

### <a name="what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers"></a>Global VNet Peering ve Load Balancers ile ilgili kısıtlamalar nelerdir?
İki farklı bölgede bulunan iki sanal ağ Global VNet Peering üzerinden bakılıyorsa, Yük Dengeleyicisinin Ön Uç IP'si aracılığıyla Temel Yük Dengeleyicisinin arkasındaki kaynaklara bağlanamazsınız. Standart Yük Dengeleyicisi için bu kısıtlama yoktur.
Aşağıdaki kaynaklar Temel Yük Dengeleyicileri'ni kullanabilir, bu da Yük Dengeleyicisinin Global VNet Peering üzerinden Ön Uç IP'si üzerinden onlara ulaşamayacağınız anlamına gelir. Ancak, izin verebilmek için kaynaklara doğrudan özel VNet IP'leri aracılığıyla ulaşmak için Global VNet eşlemesini kullanabilirsiniz. 
- Temel Yük Dengeleyicilerinin Arkasındaki VM'ler
- Temel Yük Dengeleyicileri ile sanal makine ölçek setleri 
- Redis Cache 
- Uygulama Ağ Geçidi (v1) SKU
- Service Fabric
- SQL MI
- API Management
- Active Directory Etki Alanı Hizmeti (ADDS)
- Logic Apps
- HDInsight
-   Azure Batch
- App Service Ortamı

Bu kaynaklara ExpressRoute veya VNet-to-VNet üzerinden VNet Ağ Geçitleri üzerinden bağlanabilirsiniz.

### <a name="can-i-enable-vnet-peering-if-my-virtual-networks-belong-to-subscriptions-within-different-azure-active-directory-tenants"></a>Sanal ağlarım farklı Azure Etkin Dizin kiracıları içindeki aboneliklere aitse VNet Peering'i etkinleştirebilir miyim?
Evet. Abonelikleriniz farklı Azure Etkin Dizin kiracılarına aitse VNet Peering'i (yerel veya genel olsun) oluşturmak mümkündür. Bunu PowerShell veya CLI üzerinden yapabilirsiniz. Portal henüz desteklenmedi.

### <a name="my-vnet-peering-connection-is-in-initiated-state-why-cant-i-connect"></a>VNet peering bağlantım *Başlatılan* durumda, neden bağlanamıyorum?
Karşıdan bakan bağlantınız *Başlatılan* durumdaysa, bu yalnızca bir bağlantı oluşturduğunuz anlamına gelir. Başarılı bir bağlantı kurmak için çift yönlü bir bağlantı oluşturulmalıdır. Örneğin, VNet A ile VNet B arasında eşlenebilmek için VNetA'dan VNetB'ye ve VNetB'den VNetA'ya bir bağlantı oluşturulmalıdır. Her iki bağlantının oluşturulması durumu *Bağlı*olarak değiştirir.

### <a name="my-vnet-peering-connection-is-in-disconnected-state-why-cant-i-create-a-peering-connection"></a>VNet peering bağlantım *Bağlantısı Kesildi,* neden bir peering bağlantısı oluşturamıyorum?
VNet'teki bakan bağlantınız *Bağlantısı Kesildiyse,* oluşturulan bağlantılardan birinin silindiği anlamına gelir. Bir eşleme bağlantısını yeniden kurmak için bağlantıyı silmeniz ve yeniden oluşturmanız gerekir.

### <a name="can-i-peer-my-vnet-with-a-vnet-in-a-different-subscription"></a>VNet'imi farklı bir abonelikte VNet ile eşleyebilir miyim?
Evet. VNet'leri abonelikler ve bölgeler arasında eşleyebilirsiniz.

### <a name="can-i-peer-two-vnets-with-matching-or-overlapping-address-ranges"></a>Eşleşen veya çakışan adres aralıklarına sahip iki VNet'i eşleyebilir miyim?
Hayır. VNet Peering'i etkinleştirmek için adres boşlukları çakışmamalıdır.

### <a name="how-much-do-vnet-peering-links-cost"></a>VNet izleme bağlantılarının maliyeti nedir?
VNet peering bağlantısı oluşturmak için herhangi bir ücret alınmaz. Eşleyen bağlantılar arasında veri aktarımı ücretlendirilir. [Buraya bakın](https://azure.microsoft.com/pricing/details/virtual-network/).

### <a name="is-vnet-peering-traffic-encrypted"></a>VNet'in trafik leziz olması şifreli midir?
Hayır. Eşlenen VNet'lerde kaynaklar arasındaki trafik özel dir ve yalıtılır. Tamamen Microsoft Omurgası'nda kalır.

### <a name="why-is-my-peering-connection-in-a-disconnected-state"></a>Neden benim bakış bağlantım *Bağlantısı Kesildi?*
Bir VNet bakan bağlantısı silindiğinde VNet eşleme bağlantıları *Bağlantısı kesildi* durumuna gider. Başarılı bir eşleme bağlantısını yeniden kurmak için her iki bağlantıyı da silmeniz gerekir.

### <a name="if-i-peer-vneta-to-vnetb-and-i-peer-vnetb-to-vnetc-does-that-mean-vneta-and-vnetc-are-peered"></a>VNetA'yı VNetB'ye ve VNetB'yi VNetC'ye göregörürsem, bu VNetA ve VNetC'nin bakılmış olduğu anlamına mı gelir?
Hayır. Geçişli bakış desteklenmez. Bunun gerçekleşmesi için VNetA ve VNetC ile eşlemelisiniz.

### <a name="are-there-any-bandwidth-limitations-for-peering-connections"></a>Bakan bağlantılar için herhangi bir bant genişliği sınırlaması var mı?
Hayır. VNet'in yerel veya genel olarak bakması herhangi bir bant genişliği kısıtlaması getirmez. Bant genişliği yalnızca VM veya bilgi işlem kaynağı ile sınırlıdır.

### <a name="how-can-i-troubleshoot-vnet-peering-issues"></a>VNet Peering sorunlarını nasıl giderebilirim?
Burada deneyebileceğiniz bir [sorun giderici kılavuzu.](https://support.microsoft.com/en-us/help/4486956/troubleshooter-for-virtual-network-peering-issues)

## <a name="virtual-network-tap"></a>Sanal ağ TAP

### <a name="which-azure-regions-are-available-for-virtual-network-tap"></a>Sanal ağ TAP için hangi Azure bölgeleri kullanılabilir?
Sanal ağ TAP önizlemesi tüm Azure bölgelerinde kullanılabilir. İzlenen ağ arabirimleri, sanal ağ TAP kaynağı ve toplayıcı veya analiz çözümü aynı bölgede dağıtılmalıdır.

### <a name="does-virtual-network-tap-support-any-filtering-capabilities-on-the-mirrored-packets"></a>Virtual Network TAP, yansıtılan paketlerdeki herhangi bir filtreleme özelliğini destekliyor mu?
Filtreleme özellikleri sanal ağ TAP önizlemesi ile desteklenmez. Bir TAP yapılandırması ağ arabirimine eklendiğinde, ağ arabirimindeki tüm giriş ve çıkış trafiğinin derin bir kopyası TAP hedefine aktarılır.

### <a name="can-multiple-tap-configurations-be-added-to-a-monitored-network-interface"></a>İzlenen ağ arabirimine birden çok TAP yapılandırması eklenebilir mi?
İzlenen ağ arabiriminde yalnızca bir TAP yapılandırması olabilir. TAP trafiğinin birden fazla kopyasını seçtiğiniz analiz araçlarına aktarabilmek için tek tek [iş ortağı çözümüne](virtual-network-tap-overview.md#virtual-network-tap-partner-solutions) danışın.

### <a name="can-the-same-virtual-network-tap-resource-aggregate-traffic-from-monitored-network-interfaces-in-more-than-one-virtual-network"></a>Aynı sanal ağ TAP kaynağı, birden fazla sanal ağda izlenen ağ arabirimlerinden trafiği toplayabilir mi?
Evet. Aynı sanal ağ TAP kaynağı, aynı abonelikveya farklı bir abonelikteki eşlenmiş sanal ağlarda izlenen ağ arabirimlerindeki yansıtılmış trafiği toplamak için kullanılabilir. Sanal ağ TAP kaynağı ve hedef yük dengeleyicisi veya hedef ağ arabirimi aynı abonelikte olmalıdır. Tüm abonelikler aynı Azure Etkin Dizin kiracıaltında olmalıdır.

### <a name="are-there-any-performance-considerations-on-production-traffic-if-i-enable-a-virtual-network-tap-configuration-on-a-network-interface"></a>Bir ağ arabiriminde sanal ağ TAP yapılandırmasını etkinleştirisem, üretim trafiğinde herhangi bir performans hususu var mı?

Sanal ağ TAP önizlemede. Önizleme sırasında hizmet düzeyi sözleşmesi yoktur. Bu özellik üretim iş yükleri için kullanılmamalıdır. TAP yapılandırmasıyla sanal makine ağı arabirimi etkinleştirildiğinde, yansıtma işlevini gerçekleştirmek ve yansıtılan paketleri göndermek için sanal makineye ayrılan Azure ana bilgisayarındaki aynı kaynaklar kullanılır. Sanal makinenin üretim trafiğini ve yansıtılan trafiği göndermesi için yeterli kaynağın bulunduğundan emin olmak için doğru [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Windows](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makine boyutunu seçin.

### <a name="is-accelerated-networking-for-linux-or-windows-supported-with-virtual-network-tap"></a>[Linux](create-vm-accelerated-networking-cli.md) veya [Windows](create-vm-accelerated-networking-powershell.md) için hızlandırılmış ağ sanal ağ TAP ile desteklenir mi?

Hızlandırılmış ağ ile etkinleştirilen sanal bir makineye bağlı bir ağ arabirimine TAP yapılandırması ekleyebilirsiniz. Ancak sanal makinedeki performans ve gecikme, yansıtma trafiği için boşaltma şu anda Azure hızlandırılmış ağ tarafından desteklenmediğinden TAP yapılandırması eklenerek etkilenecektir.

## <a name="virtual-network-service-endpoints"></a>Sanal ağ hizmet uç noktaları

### <a name="what-is-the-right-sequence-of-operations-to-set-up-service-endpoints-to-an-azure-service"></a>Bir Azure hizmetinin hizmet bitiş noktalarını ayarlamak için doğru işlem sırası nedir?
Bir Azure hizmet kaynağını hizmet bitiş noktaları aracılığıyla güvence altına almak için iki adım vardır:
1. Azure hizmeti için hizmet bitiş noktalarını açın.
2. Azure hizmetinde VNet ALA'ları ayarlayın.

İlk adım bir ağ tarafı işlemi, ikinci adım ise bir hizmet kaynağı tarafı işlemidir. Her iki adım da yönetici rolüne verilen RBAC izinlerine bağlı olarak aynı yönetici veya farklı yöneticiler tarafından gerçekleştirilebilir. Azure hizmet tarafında VNet ALA'ları kurmadan önce sanal ağınız için hizmet bitiş noktalarını açmanızı öneririz. Bu nedenle, VNet hizmet uçnoktalarını ayarlamak için adımların yukarıda listelenen sırada gerçekleştirilmesi gerekir.

>[!NOTE]
> Azure hizmeti erişimini izin verilen VNet ve alt ağla sınırlandırabilmek için yukarıda açıklanan her iki işlemin de tamamlanması gerekir. Yalnızca ağ tarafındaki Azure hizmetiiçin hizmet bitiş noktalarını açmak size sınırlı erişim sağlamaz. Ayrıca, Azure hizmet tarafında VNet ALA'ları da ayarlamanız gerekir.

Bazı hizmetler (SQL ve CosmosDB gibi) **IgnoreMissingVnetServiceEndpoint** bayrağı aracılığıyla yukarıdaki sıranın istisnalarına izin verir. Bayrak **True**olarak ayarlandıktan sonra, Ağ tarafında hizmet bitiş noktalarını ayarlamadan önce Azure hizmet tarafında VNet ALA'ları ayarlanabilir. Azure hizmetleri, azure hizmetlerinde belirli IP güvenlik duvarlarının yapılandırıldığı durumlarda müşterilere yardımcı olmak için bu bayrağı sağlar ve ağ tarafındaki hizmet bitiş noktalarını açmak, kaynak IP genel bir IPv4 adresinden özel bir adres. Ağ tarafında hizmet uç noktalarını ayarlamadan önce Azure hizmet tarafında VNet ALA'ları ayarlamak, bağlantının düşmesini önlemeye yardımcı olabilir.

### <a name="do-all-azure-services-reside-in-the-azure-virtual-network-provided-by-the-customer-how-does-vnet-service-endpoint-work-with-azure-services"></a>Tüm Azure hizmetleri müşteri tarafından sağlanan Azure sanal ağında mı bulunur? VNet hizmet bitiş noktası Azure hizmetleriyle nasıl çalışır?

Hayır, tüm Azure hizmetleri müşterinin sanal ağında bulunmaz. Azure Depolama, Azure SQL ve Azure Cosmos DB gibi Azure veri hizmetlerinin çoğu, genel IP adresleri üzerinden erişilebilen çok kiracılı hizmetlerdir. Azure [hizmetleri](virtual-network-for-azure-services.md)için sanal ağ tümleştirmesi hakkında daha fazla bilgi edinebilirsiniz. 

VNet hizmet bitiş noktaları özelliğini kullandığınızda (ağ tarafında VNet hizmet bitiş noktasını açmak ve Azure hizmet tarafında uygun VNet ALA'ları ayarlamak), bir Azure hizmetine erişim izin verilen bir VNet ve alt ağdan kısıtlanır.

### <a name="how-does-vnet-service-endpoint-provide-security"></a>VNet hizmet bitiş noktası güvenliği nasıl sağlar?

VNet hizmeti bitiş noktası özelliği (ağ tarafında VNet hizmet bitiş noktasını açmak ve Azure hizmet tarafında uygun VNet AÇ'ları ayarlamak) Azure hizmet erişimini izin verilen VNet ve alt ağa sınırlandırarak ağ düzeyinde güvenlik sağlar ve Azure hizmet trafiğinin yalıtımı. VNet hizmet uç noktalarını kullanan tüm trafik Microsoft omurgası üzerinden akar ve böylece genel internetten başka bir yalıtım katmanı sağlar. Ayrıca, müşteriler Azure hizmet kaynaklarına genel Internet erişimini tamamen kaldırmayı seçebilir ve IP güvenlik duvarı ve VNet ALA'larının bir kombinasyonu yla sanal ağlarından trafiğe izin vererek Azure hizmet kaynaklarını yetkisiz olmaktan koruyabilir Erişim.      

### <a name="what-does-the-vnet-service-endpoint-protect---vnet-resources-or-azure-service"></a>VNet hizmeti bitiş noktası neleri korur - VNet kaynakları veya Azure hizmeti?
VNet hizmet bitiş noktaları Azure hizmet kaynaklarının korunmasına yardımcı olur. VNet kaynakları Ağ Güvenlik Grupları (NSG'ler) aracılığıyla korunur.

### <a name="is-there-any-cost-for-using-vnet-service-endpoints"></a>VNet hizmet bitiş noktalarını kullanmanın herhangi bir maliyeti var mı?

Hayır, VNet hizmet bitiş noktalarını kullanmak için ek bir ücret yoktur.

### <a name="can-i-turn-on-vnet-service-endpoints-and-set-up-vnet-acls-if-the-virtual-network-and-the-azure-service-resources-belong-to-different-subscriptions"></a>Sanal ağ ve Azure hizmet kaynakları farklı aboneliklere aitse VNet hizmet uçnoktalarını açıp VNet ALA'ları ayarlayabilir miyim?

Evet, olabilir. Sanal ağlar ve Azure hizmet kaynakları aynı veya farklı aboneliklerde olabilir. Tek gereksinim, hem sanal ağ hem de Azure hizmet kaynaklarının aynı Etkin Dizin (AD) kiracıaltında olması gerektiğidir.

### <a name="can-i-turn-on-vnet-service-endpoints-and-set-up-vnet-acls-if-the-virtual-network-and-the-azure-service-resources-belong-to-different-ad-tenants"></a>Sanal ağ ve Azure hizmet kaynakları farklı AD kiracılarına aitse VNet hizmet uçnoktalarını açıp VNet ALA'ları ayarlayabilir miyim?
Hayır, VNet hizmet bitiş noktaları ve VNet ALA'ları AD kiracılar arasında desteklenmez.

### <a name="can-an-on-premises-devices-ip-address-that-is-connected-through-azure-virtual-network-gateway-vpn-or-expressroute-gateway-access-azure-paas-service-over-vnet-service-endpoints"></a>Bir şirket içi aygıtın Azure Sanal Ağ ağ geçidi (VPN) veya ExpressRoute ağ geçidi üzerinden bağlanan IP adresi, VNet hizmet bitiş noktaları üzerinden Azure PaaS Hizmetine erişebilir mi?
Varsayılan olarak sanal ağlara ayrılmış olan Azure hizmeti kaynaklarına şirket içi ağlardan erişmek mümkün değildir. Şirket içi trafiğe izin vermek istiyorsanız, şirket içi veya ExpressRoute'unuzdan herkese açık (genellikle NAT) IP adreslerine de izin vermelisiniz. Bu IP adresleri, Azure hizmet kaynakları için IP güvenlik duvarı yapılandırması aracılığıyla eklenebilir.

### <a name="can-i-use-vnet-service-endpoint-feature-to-secure-azure-service-to-multiple-subnets-within-a-virtual-network-or-across-multiple-virtual-networks"></a>Sanal ağdaki veya birden çok sanal ağda birden çok alt ağa Azure hizmetini güvence altına almak için VNet Hizmet Bitiş Noktası özelliğini kullanabilir miyim?
Azure hizmetlerini sanal ağiçinde veya birden çok sanal ağda birden çok alt ağa sabitlemek için, alt ağların her birinde ağ tarafındaki hizmet bitiş noktalarını bağımsız olarak etkinleştirin ve ardından azure hizmet kaynaklarını ayarlayarak tüm alt ağlara güvenli hale Azure hizmet tarafında uygun VNet ALA'ları.
 
### <a name="how-can-i-filter-outbound-traffic-from-a-virtual-network-to-azure-services-and-still-use-service-endpoints"></a>Bir sanal ağdan Azure hizmetlerine giden trafiği nasıl filtreleyebilir ve hizmet bitiş noktalarını kullanmaya devam edebilirim?
Bir Sanal ağdan Azure hizmetine yönelik trafiği incelemek veya filtrelemek istiyorsanız, sanal ağ içinde bir ağ sanal cihazı dağıtabilirsiniz. Daha sonra, ağ sanal cihazının dağıtıldığı alt ağa hizmet bitiş noktaları uygulayabilir ve Azure hizmet kaynaklarını yalnızca VNet ALA'ları aracılığıyla bu alt ağa güvenli hale getirebilirsiniz. Bu senaryo, sanal ağınızdan Azure hizmet erişimini yalnızca ağ sanal cihaz filtrelemenin belirli Azure kaynaklarına kısıtlamak istiyorsanız da yararlı olabilir. Daha fazla bilgi için bkz. [Ağ sanal gereçleri ile çıkış](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha).

### <a name="what-happens-when-you-access-an-azure-service-account-that-has-a-virtual-network-access-control-list-acl-enabled-from-outside-the-vnet"></a>VNet dışından sanal ağ erişim denetim listesi (ACL) etkinleştirilmiş bir Azure hizmet hesabına erişdiğinizde ne olur?
HTTP 403 veya HTTP 404 hatası döndürülür.

### <a name="are-subnets-of-a-virtual-network-created-in-different-regions-allowed-to-access-an-azure-service-account-in-another-region"></a>Farklı bölgelerde oluşturulan sanal ağın alt ağlarının başka bir bölgedeki bir Azure hizmet hesabına erişmesine izin veriliyor mu? 
Evet, Azure hizmetlerinin çoğunda, farklı bölgelerde oluşturulan sanal ağlar VNet hizmet bitiş noktaları aracılığıyla başka bir bölgedeki Azure hizmetlerine erişebilir. Örneğin, bir Azure Cosmos DB hesabı Batı ABD veya Doğu ABD'deyse ve sanal ağlar birden çok bölgedeyse, sanal ağ Azure Cosmos DB'ye erişebilir. Depolama ve SQL istisnadır ve bölgesel niteliktedir ve hem sanal ağ hem de Azure hizmetinin aynı bölgede olması gerekir.
  
### <a name="can-an-azure-service-have-both-a-vnet-acl-and-an-ip-firewall"></a>Bir Azure hizmetinde hem VNet ACL hem de IP güvenlik duvarı olabilir mi?
Evet, bir VNet ACL ve IP güvenlik duvarı birlikte bulunabilir. Yalıtım ve güvenlik sağlamak için her iki özellik de birbirini tamamlar.
 
### <a name="what-happens-if-you-delete-a-virtual-network-or-subnet-that-has-service-endpoint-turned-on-for-azure-service"></a>Azure hizmeti için hizmet bitiş noktası açık olan bir sanal ağı veya alt ağı silerseniz ne olur?
VNet'lerin ve alt ağların silinmesi bağımsız işlemlerdir ve Azure hizmetleri için hizmet bitiş noktaları açık olsa bile desteklenir. Azure hizmetlerinin VNet ACL'lerinin ayarlandığı durumlarda, bu VNet'ler ve alt ağlar için, VNet hizmet bitiş noktası açık olan bir VNet veya alt ağ silindiğinde bu Azure hizmetiyle ilişkili VNet ACL bilgileri devre dışı bırakılır.
 
### <a name="what-happens-if-an-azure-service-account-that-has-a-vnet-service-endpoint-enabled-is-deleted"></a>VNet Hizmeti bitiş noktası etkin leştirilmiş bir Azure hizmet hesabı silinirse ne olur?
Azure hizmet hesabının silinmesi bağımsız bir işlemdir ve hizmet bitiş noktası ağ tarafında etkinleştirildiğinde ve Azure hizmet tarafında VNet ALA'ları ayarlandığında bile desteklenir. 

### <a name="what-happens-to-the-source-ip-address-of-a-resource-like-a-vm-in-a-subnet-that-has-vnet-service-endpoint-enabled"></a>VNet hizmet bitiş noktası etkin leştirilmiş bir kaynağın (alt ağdaki VM gibi) kaynak IP adresine ne olur?
Sanal ağ hizmeti uç noktaları etkinleştirildiğinde, sanal ağınızın alt ağındaki kaynakların kaynak IP adresleri, ortak IPV4 adreslerini kullanmaktan Azure hizmetine trafik için Azure sanal ağının özel IP adreslerine geçer. Bunun, Azure hizmetlerinde daha önce genel IPV4 adresine ayarlanmış belirli IP güvenlik duvarlarının başarısız lığa neden olabileceğini unutmayın. 

### <a name="does-the-service-endpoint-route-always-take-precedence"></a>Hizmet bitiş noktası rotası her zaman öncelikli midir?
Hizmet bitiş noktaları, BGP rotalarından önce gelen ve hizmet bitiş noktası trafiği için optimum yönlendirme sağlayan bir sistem rotası ekler. Hizmet bitiş noktaları her zaman doğrudan sanal aağınızdan Microsoft Azure omurga ağındaki hizmete hizmet trafiğini alır. Azure'un bir rotayı nasıl seçtiği hakkında daha fazla bilgi için [Azure Sanal ağ trafiği yönlendirmesi'ne](virtual-networks-udr-overview.md)bakın.
 
### <a name="how-does-nsg-on-a-subnet-work-with-service-endpoints"></a>Bir alt ağdaki NSG hizmet bitiş noktalarıyla nasıl çalışır?
Azure hizmetine ulaşmak için NSG'lerin giden bağlantıya izin vermeleri gerekir. NSG'leriniz tüm Internet giden trafiğine açılıyorsa, hizmet bitiş noktası trafiği çalışmalıdır. Ayrıca, giden trafiği yalnızca Hizmet etiketlerini kullanarak servis IP'leriyle sınırlandırabilirsiniz.  
 
### <a name="what-permissions-do-i-need-to-set-up-service-endpoints"></a>Hizmet bitiş noktalarını ayarlamak için hangi izinlere ihtiyacım var?
Hizmet bitiş noktaları, sanal ağa yazma erişimi olan bir kullanıcı tarafından bağımsız olarak sanal ağ üzerinde yapılandırılabilir. Azure hizmet kaynaklarını bir VNet'e sabitlemek için, kullanıcının eklenen alt ağlar için **Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action** iznine sahip olması gerekir. Bu izin, varsayılan olarak yerleşik hizmet yöneticisi rolüne dahil edilir ve özel roller oluşturarak değiştirilebilir. Yerleşik roller ve [özel rollere](https://docs.microsoft.com/azure/role-based-access-control/custom-roles?toc=%2fazure%2fvirtual-network%2ftoc.json) belirli izinlerin atanması hakkında daha fazla bilgi edinin.
 

### <a name="can-i-filter-virtual-network-traffic-to-azure-services-allowing-only-specific-azure-service-resources-over-vnet-service-endpoints"></a>Sanal ağ trafiğini Azure hizmetlerine filtreleyerek VNet hizmet bitiş noktaları üzerinden yalnızca belirli azure hizmet kaynaklarına izin verebilir miyim? 

Sanal ağ (VNet) hizmet bitiş noktası ilkeleri, sanal ağ trafiğini Azure hizmetlerine filtrelemenize olanak sağlayarak hizmet bitiş noktaları üzerinden yalnızca belirli Azure hizmet kaynaklarına izin verir. Uç nokta ilkeleri, sanal ağ trafiğinden Azure hizmetlerine kadar parçalı erişim denetimi sağlar. Hizmet bitiş noktası [ilkeleri](virtual-network-service-endpoint-policies-overview.md)hakkında daha fazla bilgi edinebilirsiniz.

### <a name="does-azure-active-directory-azure-ad-support-vnet-service-endpoints"></a>Azure Etkin Dizin (Azure AD) VNet hizmet bitiş noktalarını destekliyor mu?

Azure Etkin Dizin (Azure AD), hizmet bitiş noktalarını yerel olarak desteklemez. VNet hizmet bitiş noktalarını destekleyen Azure Hizmetlerinin tam listesini [burada](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview)görebilirsiniz. Hizmet destek noktaları nın altında listelenen "Microsoft.AzureActiveDirectory" etiketinin ADLS Gen 1'in hizmet uç noktalarını desteklemek için kullanıldığını unutmayın. ADLS Gen 1 için, Azure Veri Gölü Depolama Gen1 için sanal ağ tümleştirmesi, erişim belirtecinde ek güvenlik talepleri oluşturmak için sanal ağınız ile Azure Etkin Dizin (Azure AD) arasındaki sanal ağ hizmeti uç noktası güvenliğini kullanır. Ardından bu talepler sanal ağınız için Data Lake Storage 1. Nesil hesabınızda kimlik doğrulaması gerçekleştirme ve erişim izni verme amacıyla kullanılır. [Azure Veri Gölü Deposu Gen 1 VNet Entegrasyonu](../data-lake-store/data-lake-store-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hakkında daha fazla bilgi edinin

### <a name="are-there-any-limits-on-how-many-vnet-service-endpoints-i-can-set-up-from-my-vnet"></a>VNet'imden kaç VNet hizmet bitiş noktası ayarlayabildiğimin bir sınırı var mı?
Sanal ağdaki toplam VNet hizmet bitiş noktası sayısında bir sınır yoktur. Bir Azure hizmet kaynağı (Azure Depolama hesabı gibi) için, hizmetler kaynağın güvenliğini sağlamak için kullanılan alt ağ sayısına sınırlamalar getirebilir. Aşağıdaki tabloda bazı örnek sınırlar gösterilmektedir: 

|||
|---|---|
|Azure hizmeti| VNet kurallarındaki sınırlar|
|Azure Storage| 100|
|Azure SQL| 128|
|Azure SQL Veri Ambarı|  128|
|Azure KeyVault|    127|
|Azure Cosmos DB|   64|
|Azure Event Hub|   128|
|Azure Service Bus| 128|
|Azure Veri Gölü Deposu V1|  100|
 
>[!NOTE]
> Bu sınırlar, Azure hizmetinin takdirine bağlı olarak değişikliklere tabi tutulur. Hizmet ayrıntıları için ilgili hizmet belgelerine bakın. 




  



