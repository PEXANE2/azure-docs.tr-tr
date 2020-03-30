---
title: SSS - Azure ExpressRoute | Microsoft Dokümanlar
description: ExpressRoute SSS Desteklenen Azure Hizmetleri, Maliyet, Veri ve Bağlantılar, SLA, Sağlayıcılar ve Konumlar, Bant Genişliği ve ek Teknik Ayrıntılar hakkında bilgi içerir.
services: expressroute
author: jaredr80
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/13/2019
ms.author: jaredro
ms.openlocfilehash: 845c53ec970777901ae8d1c0abf5032ac705d3e3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79264927"
---
# <a name="expressroute-faq"></a>ExpressRoute SSS

## <a name="what-is-expressroute"></a>ExpressRoute nedir?

ExpressRoute, Microsoft veri merkezleri ve binalarınızda veya bir birlikte konumlandırma tesisinde bulunan altyapı arasında özel bağlantılar oluşturmanıza olanak tanıyan bir Azure hizmetidir. ExpressRoute bağlantıları genel Internet üzerinden gitmez ve Internet üzerinden tipik bağlantılara göre daha düşük gecikme süreleri ile daha yüksek güvenlik, güvenilirlik ve hız sunar.

### <a name="what-are-the-benefits-of-using-expressroute-and-private-network-connections"></a>ExpressRoute ve özel ağ bağlantılarını kullanmanın avantajları nelerdir?

ExpressRoute bağlantıları ortak İnternet üzerinden geçmemektedir. Internet üzerinden tipik bağlantılara göre daha düşük ve tutarlı gecikmelerle daha yüksek güvenlik, güvenilirlik ve hız lar sunarlar. Bazı durumlarda, şirket içi aygıtlar ve Azure arasında veri aktarmak için ExpressRoute bağlantılarını kullanmak önemli maliyet avantajları sağlayabilir.

### <a name="where-is-the-service-available"></a>Hizmet nerede kullanılabilir?

Hizmet konumu ve kullanılabilirliği için bu sayfaya bakın: [ExpressRoute iş ortakları ve konumları.](expressroute-locations.md)

### <a name="how-can-i-use-expressroute-to-connect-to-microsoft-if-i-dont-have-partnerships-with-one-of-the-expressroute-carrier-partners"></a>ExpressRoute taşıyıcı ortaklarından biriyle ortaklığım yoksa Microsoft'a bağlanmak için ExpressRoute'u nasıl kullanabilirim?

Desteklenen değişim sağlayıcı konumlarından birine bölgesel bir taşıyıcı ve arazi Ethernet bağlantıları seçebilirsiniz. Daha sonra sağlayıcı konumunda Microsoft ile eşleyebilirsiniz. Servis sağlayıcınızın değişim konumlarından herhangi birinde bulunup bulunmadığını görmek için [ExpressRoute iş ortaklarının ve konumlarının](expressroute-locations.md) son bölümünü kontrol edin. Daha sonra Azure'a bağlanmak için servis sağlayıcısı aracılığıyla bir ExpressRoute devresi sipariş edebilirsiniz.

### <a name="how-much-does-expressroute-cost"></a>ExpressRoute'un maliyeti nedir?

Fiyatlandırma bilgileri için [fiyatlandırma ayrıntılarını](https://azure.microsoft.com/pricing/details/expressroute/) kontrol edin.

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth-does-the-vpn-connection-i-purchase-from-my-network-service-provider-have-to-be-the-same-speed"></a>Belirli bir bant genişliğine sahip bir ExpressRoute devresi için ödeme yapıyorsam, ağ servis sağlayıcımdan satın aldığım VPN bağlantısı aynı hızda mı olmak zorunda?

Hayır. Servis sağlayıcınızdan istediğiniz hızda vpn bağlantısı satın alabilirsiniz. Ancak, Azure'a olan bağlantınız satın aldığınız ExpressRoute devre bant genişliğiyle sınırlıdır.

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth-do-i-have-the-ability-to-burst-up-to-higher-speeds-if-necessary"></a>Belirli bir bant genişliğine sahip bir ExpressRoute devresi için ödeme yaparsam, gerekirse daha yüksek hızlara kadar patlama yeteneğine sahip olabilir miyim?

Evet. ExpressRoute devreleri, ek ücret ödemeden sağladığınız bant genişliği sınırının iki katına kadar patlamanızı sağlayacak şekilde yapılandırılmıştır. Bu özelliği desteklediklerini görmek için servis sağlayıcınıza danışın. Bu süre için değildir ve garanti edilmez.  Trafik bir ExpressRoute Ağ Geçidi'nden akıyorsa, sku'nun bant genişliği sabittir ve patlamaz.

### <a name="can-i-use-the-same-private-network-connection-with-virtual-network-and-other-azure-services-simultaneously"></a>Sanal ağ ve diğer Azure hizmetleriyle aynı özel ağ bağlantısını aynı anda kullanabilir miyim?

Evet. Bir ExpressRoute devresi, kurulduktan sonra sanal ağ daki hizmetlere ve diğer Azure hizmetlerine aynı anda erişmenizi sağlar. Özel bakış yolu üzerinden sanal ağlara ve Microsoft bakış yolu üzerinden diğer hizmetlere bağlanırsınız.

### <a name="how-are-vnets-advertised-on-expressroute-private-peering"></a>ExpressRoute Özel Peering'de VNet'lerin reklamı nasıl yapılır?

ExpressRoute ağ geçidi, Azure VNet'in *Adres Alanı(lar)* tanıtımını yapacaktır, alt ağ düzeyinde dahil edemez/hariç tutamazsınız. Her zaman reklamı VNet Adres Alanıdır. Ayrıca, VNet Peering kullanılırsa ve bakan VNet "Uzaktan Ağ Geçidi kullan" etkinleştirilmişse, bakan VNet'in Adres Alanı da ilan edilir.

### <a name="how-many-prefixes-can-be-advertised-from-a-vnet-to-on-premises-on-expressroute-private-peering"></a>ExpressRoute Private Peering'de bir VNet'ten şirket içi ne kadar önek ilan edilebilir?

Tek bir ExpressRoute bağlantısında veya ağ geçidi geçişini kullanarak VNet'ten bakan en fazla 200 önek vardır. Örneğin, bir ExpressRoute devresine bağlı tek bir VNet'te 199 adres alanınız varsa, bu öneklerin 199'unun tümü şirket içinde duyurulur. Alternatif olarak, "Uzaktan Ağ Geçidine İzin Ver" seçeneğini kullanarak 1 adres alanı olan ağ geçidi geçişine izin veren bir VNet'iniz ve 150 kollu VNet'iniz etkinse, ağ geçidiyle birlikte dağıtılan VNet şirket içi 151 önek tanıtacaktır.

### <a name="what-happens-if-i-exceed-the-prefix-limit-on-an-expressroute-connection"></a>ExpressRoute bağlantısındaki önek sınırını aşarsam ne olur?

ExpressRoute devresi ile ağ geçidi arasındaki bağlantı (ve varsa ağ geçidi geçişini kullanan eşlenen VNet'ler) inecektir. Önek sınırı artık aşılmadığında yeniden kurulur.  

### <a name="can-i-filter-routes-coming-from-my-on-premises-network"></a>Şirket içi ağımdan gelen yolları filtreleyebilir miyim?

Yolları filtrelemenin/eklemenin tek yolu şirket içi kenar yönlendiricidir. Belirli yönlendirmeyi etkilemek için VNet'e kullanıcı tanımlı Rotalar eklenebilir, ancak bu statik olur ve BGP reklamının bir parçası değildir.

### <a name="does-expressroute-offer-a-service-level-agreement-sla"></a>ExpressRoute Bir Hizmet Düzeyi Sözleşmesi (SLA) sunuyor mu?

Daha fazla bilgi için [ExpressRoute SLA](https://azure.microsoft.com/support/legal/sla/) sayfasına bakın.

## <a name="supported-services"></a>Desteklenen hizmetler

ExpressRoute, çeşitli hizmet türleri için [üç yönlendirme etki alanını](expressroute-circuit-peerings.md) destekler: özel akran, Microsoft'a bakma ve genel bakış (amortismana uygun).

### <a name="private-peering"></a>Özel eşleme

**Desteklenen:**

* Tüm sanal makineler ve bulut hizmetleri de dahil olmak üzere sanal ağlar

### <a name="microsoft-peering"></a>Microsoft eşlemesi

ExpressRoute devreniz Azure Microsoft eşlemeleri için etkinse, devre üzerinden Azure'da kullanılan [genel IP adresi aralıklarına](../virtual-network/virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) erişebilirsiniz. Azure Microsoft bakışları, şu anda Azure'da barındırılan hizmetlere (devrenizin SKU'suna bağlı olarak coğrafi kısıtlamalarla) erişim sağlar. Belirli bir hizmetin kullanılabilirliğini doğrulamak için, bu hizmet için ayrılmış bir aralık olup olmadığını görmek için bu hizmetin belgelerini denetleyebilirsiniz. Ardından, hedef hizmetin IP aralıklarına bakın ve Azure IP [Aralıkları ve Hizmet Etiketleri – Public Cloud XML dosyasında](https://www.microsoft.com/download/details.aspx?id=56519)listelenen aralıklarla karşılaştırın. Alternatif olarak, açıklama için söz konusu hizmet için bir destek bileti açabilirsiniz.

**Desteklenen:**

* [Ofis 365](https://aka.ms/ExpressRouteOffice365)
* Power BI - Azure Bölgesel Topluluğu üzerinden kullanılabilir, Power BI kiracınızın bölgesini nasıl bulacağınız için [buraya](https://docs.microsoft.com/power-bi/service-admin-where-is-my-tenant-located) bakın.
* Azure Active Directory
* [Windows Sanal Masaüstü](https://azure.microsoft.com/services/virtual-desktop/)
* [Azure DevOps](https://blogs.msdn.microsoft.com/devops/2018/10/23/expressroute-for-azure-devops/) (Azure Global Hizmetler topluluğu)
* IaaS için Azure Genel IP adresleri (Sanal Makineler, Sanal Ağ Ağ Geçitleri, Yük Dengeleyicileri, vb.)  
* Diğer Azure hizmetlerinin çoğu da desteklenir. Lütfen desteği doğrulamak için kullanmak istediğiniz hizmete doğrudan danışın.

**Desteklenmiyor:**

* CDN
* Azure Front Door
* Çok faktörlü Kimlik Doğrulama Sunucusu (eski)
* Traffic Manager

### <a name="public-peering"></a>Ortak eşleme

Yeni ExpressRoute devrelerinde genel bakış devreleri devre dışı bırakıldı. Azure hizmetleri artık Microsoft'un bakışlarında kullanılabilir. Genel olarak bakmanın amortismana girmesinden önce oluşturulmuş bir devreyseniz, istediğiniz hizmetlere bağlı olarak Microsoft'a veya genel bakışla kullanmayı seçebilirsiniz.

Genel bakış için daha fazla bilgi ve yapılandırma adımları için [ExpressRoute genel bakış'a](about-public-peering.md)bakın.

### <a name="why-i-see-advertised-public-prefixes-status-as-validation-needed-while-configuring-microsoft-peering"></a>Microsoft'u yapılandırırken neden 'Reklamı yapılan genel önekleri' durumunu 'Doğrulama gerekli' olarak görüyorum?

Microsoft, Internet Yönlendirme Kayıt Defteri'nde belirtilen 'Reklamı yapılan genel önekleri' ve 'Eş ASN' (veya 'Müşteri ASN') atanmışsa bunu doğrular. Genel önekleri başka bir varlıktan alıyorsanız ve atama yönlendirme kayıt defterine kaydedilmezse, otomatik doğrulama tamamlanmaz ve el ile doğrulama gerektirir. Otomatik doğrulama başarısız olursa, 'Doğrulama gerekli' iletisini görürsünüz.

'Doğrulama gerekli' iletisini görüyorsanız, genel öneklerin kuruluşunuza yönlendirme kayıt defterindeki öneklerin sahibi olarak listelenen kuruluş tarafından atandığını gösteren belge(ler) toplayın ve bu belgeleri el ile doğrulama için gönderin. aşağıda gösterildiği gibi bir destek bileti açma.

![](./media/expressroute-faqs/ticket-portal-msftpeering-prefix-validation.png)

### <a name="is-dynamics-365-supported-on-expressroute"></a>Dynamics 365 ExpressRoute'da desteklendi mi?

Dynamics 365 ve Ortak Veri Hizmeti (CDS) ortamları Azure'da barındırılır ve bu nedenle müşteriler Azure kaynakları için temel ExpressRoute desteğinden yararlanır. Yönlendirici filtreniz Dynamics 365/CDS ortamlarınızın barındırılan Azure bölgelerini içeriyorsa, hizmet uç noktalarına bağlanabilirsiniz.

> [!NOTE]
> [Azure ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-faqs#expressroute-premium) üzerinden Dynamics 365 bağlantısı için ExpressRoute Premium **gerekmez.**

## <a name="data-and-connections"></a>Veriler ve bağlantılar

### <a name="are-there-limits-on-the-amount-of-data-that-i-can-transfer-using-expressroute"></a>ExpressRoute kullanarak aktarabileceğim veri miktarında sınırlamalar var mı?

Veri aktarım miktarına bir sınır belirlemeyiz. Bant genişliği oranları hakkında bilgi almak için [fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/expressroute/) bakın.

### <a name="what-connection-speeds-are-supported-by-expressroute"></a>ExpressRoute tarafından hangi bağlantı hızları desteklenir?

Desteklenen bant genişliği teklifleri:

50 Mb/sn, 100 Mb/sn, 200 Mb/sn, 500 Mb/sn, 1 Gb/sn, 2 Gb/sn, 5 Gb/sn, 10 Gb/sn

### <a name="which-service-providers-are-available"></a>Hangi hizmet sağlayıcılar kullanılabilir?

Servis sağlayıcıları ve konumları listesi için [ExpressRoute iş ortakları](expressroute-locations.md) ve konumlarına bakın.

## <a name="technical-details"></a>Teknik ayrıntılar

### <a name="what-are-the-technical-requirements-for-connecting-my-on-premises-location-to-azure"></a>Şirket içi konumumu Azure'a bağlamak için teknik gereksinimler nelerdir?

Gereksinimler için [ExpressRoute önkoşullar sayfasına](expressroute-prerequisites.md) bakın.

### <a name="are-connections-to-expressroute-redundant"></a>ExpressRoute bağlantıları gereksiz mi?

Evet. Her ExpressRoute devresi, yüksek kullanılabilirlik sağlamak üzere yapılandırılan gereksiz bir çapraz bağlantı çiftine sahiptir.

### <a name="will-i-lose-connectivity-if-one-of-my-expressroute-links-fail"></a>ExpressRoute bağlantılarımdan biri başarısız olursa bağlantımı kaybeder miyim?

Çapraz bağlantılardan biri başarısız olursa bağlantı kaybetmezsiniz. Ağınızın yükünü desteklemek ve ExpressRoute devrenizin yüksek kullanılabilirliğini sağlamak için yedekli bir bağlantı kullanılabilir. Ayrıca, devre düzeyinde esneklik elde etmek için farklı bir bakış konumunda bir devre oluşturabilirsiniz.

### <a name="how-do-i-implement-redundancy-on-private-peering"></a>Özel bakışlarda fazlalığı nasıl uygularım?

Farklı eşleme konumlarından birden fazla ExpressRoute devresi, tek bir devrenin kullanılamaması durumunda yüksek kullanılabilirlik sağlamak için aynı sanal ağa bağlanabilir. Daha sonra belirli bir devre tercih lehine yerel bağlantı ya [daha yüksek ağırlıklar atayabilirsiniz.](https://docs.microsoft.com/azure/expressroute/expressroute-optimize-routing#solution-assign-a-high-weight-to-local-connection) Müşterilerin tek bir hata noktasını önlemek için en az iki ExpressRoute devresi kurmaları önerilir. 

Buraya [here](https://docs.microsoft.com/azure/expressroute/designing-for-high-availability-with-expressroute) yüksek kullanılabilirlik için tasarım ve [burada](https://docs.microsoft.com/azure/expressroute/designing-for-disaster-recovery-with-expressroute-privatepeering) olağanüstü durum kurtarma için tasarım için bakın.  

### <a name="how-i-do-implement-redundancy-on-microsoft-peering"></a>Microsoft'un eşlemelerinde artıklığı nasıl uygularım?

Müşterilerin Azure Depolama veya Azure SQL gibi Azure kamu hizmetlerine erişmek için Microsoft'u eşlemeleri ve Office 365 için Microsoft'u kullanan müşterilerin farklı bir şekilde birden fazla devre uygulamalarını kullanmaları önerilir tek hata noktalarını önlemek için konumları. Müşteriler her iki devrede de aynı önekin reklamını yapabilir ve şirket içi yolu belirlemek için [AS PATH önonayını](https://docs.microsoft.com/azure/expressroute/expressroute-optimize-routing#solution-use-as-path-prepending) kullanabilir veya farklı öneklerin reklamını yapabilir.

Yüksek kullanılabilirlik için tasarım için [buraya](https://docs.microsoft.com/azure/expressroute/designing-for-high-availability-with-expressroute) bakın.

### <a name="how-do-i-ensure-high-availability-on-a-virtual-network-connected-to-expressroute"></a>ExpressRoute'a bağlı bir sanal ağda yüksek kullanılabilirliği nasıl sağlarım?

Farklı konumlardaki (örneğin, Singapur, Singapur2) ExpressRoute devrelerini sanal ağınıza bağlayarak yüksek kullanılabilirlik elde edebilirsiniz. Bir ExpressRoute devresi çökerse, bağlantı başka bir ExpressRoute devresi üzerinden başarısız olur. Varsayılan olarak, sanal ağınızdan çıkan trafik Eşit Maliyetli Çok Yönlendirme (ECMP) temel alınarak yönlendirilir. Bir devreyi diğerine tercih etmek için Bağlantı Ağırlığını kullanabilirsiniz. Daha fazla bilgi için [ExpressRoute Yönlendirmeyi Optimize Etme'ye](expressroute-optimize-routing.md)bakın.

### <a name="how-do-i-ensure-that-my-traffic-destined-for-azure-public-services-like-azure-storage-and-azure-sql-on-microsoft-peering-or-public-peering-is-preferred-on-the-expressroute-path"></a>Microsoft'un bakış veya ortak bakışlarında Azure Depolama ve Azure SQL gibi Azure Genel hizmetleri için ayrılan trafiğimin ExpressRoute yolunda tercih edilmesini nasıl sağlarım?

Şirket içi Azure'a giden yolun ExpressRoute devrenizde (ler) her zaman tercih edilmesini sağlamak için yönlendiriciniz(ler) üzerinde *Yerel Tercih* özniteliğini uygulamanız gerekir.

BGP yol seçimi ve ortak yönlendirici yapılandırmaları hakkında ek [ayrıntılara bakın.](https://docs.microsoft.com/azure/expressroute/expressroute-optimize-routing#path-selection-on-microsoft-and-public-peerings) 

### <a name="if-im-not-co-located-at-a-cloud-exchange-and-my-service-provider-offers-point-to-point-connection-do-i-need-to-order-two-physical-connections-between-my-on-premises-network-and-microsoft"></a><a name="onep2plink"></a>Bir bulut değişiminde birlikte bulunmuyorsam ve hizmet sağlayıcım noktadan noktaya bağlantı sunuyorsa, şirket içi ağım la Microsoft arasında iki fiziksel bağlantı sipariş etmem gerekir mi?

Servis sağlayıcınız fiziksel bağlantı üzerinden iki Ethernet sanal devresi kurabiliyorsa, yalnızca bir fiziksel bağlantıya ihtiyacınız vardır. Fiziksel bağlantı (örneğin, optik fiber) katman 1 (L1) aygıtında sonlandırılır (resme bakın). İki Ethernet sanal devresi farklı VLAN IP'leri ile etiketlenir, birincil devre için bir, ve ikincil için bir. Bu VLAN ID'ler dış 802.1Q Ethernet üstbilgisindedir. İç 802.1Q Ethernet üstbilgisi (gösterilmez) belirli bir [ExpressRoute yönlendirme etki alanına](expressroute-circuit-peerings.md)eşlenir.

![](./media/expressroute-faqs/expressroute-p2p-ref-arch.png)

### <a name="can-i-extend-one-of-my-vlans-to-azure-using-expressroute"></a>ExpressRoute'u kullanarak V'lerimden birini Azure'a uzatabilir miyim?

Hayır. Azure'daki katman 2 bağlantı uzantılarını destekliyoruz.

### <a name="can-i-have-more-than-one-expressroute-circuit-in-my-subscription"></a>Aboneliğimde birden fazla ExpressRoute devresi olabilir mi?

Evet. Aboneliğinizde birden fazla ExpressRoute devresi olabilir. Varsayılan sınır 10 olarak ayarlanır. Gerekirse sınırı artırmak için Microsoft Destek'e başvurabilirsiniz.

### <a name="can-i-have-expressroute-circuits-from-different-service-providers"></a>Farklı servis sağlayıcılardan ExpressRoute devreleri alabilir miyim?

Evet. Birçok servis sağlayıcıile ExpressRoute devreleri olabilir. Her ExpressRoute devresi yalnızca bir servis sağlayıcısıyla ilişkilidir. 

### <a name="i-see-two-expressroute-peering-locations-in-the-same-metro-for-example-singapore-and-singapore2-which-peering-location-should-i-choose-to-create-my-expressroute-circuit"></a>Ben aynı metro, örneğin, Singapur ve Singapur2 iki ExpressRoute bakan yerleri bakın. ExpressRoute devremi oluşturmak için hangi eşkonumu seçmeliyim?
Servis sağlayıcınız her iki sitede de ExpressRoute sunuyorsa, sağlayıcınızla çalışabilir ve ExpressRoute'u ayarlamak için her iki siteyi de seçebilirsiniz. 

### <a name="can-i-have-multiple-expressroute-circuits-in-the-same-metro-can-i-link-them-to-the-same-virtual-network"></a>Aynı metroda birden fazla ExpressRoute devresi olabilir miyim? Bunları aynı sanal ağa bağlayabilir miyim?

Evet. Aynı veya farklı servis sağlayıcılarına sahip birden çok ExpressRoute devreniz olabilir. Metroda birden çok ExpressRoute eşleme konumu varsa ve devreler farklı eşleme konumlarında oluşturulacaksa, bunları aynı sanal ağa bağlayabilirsiniz. Devreler aynı eşleme konumunda oluşturulursa, aynı sanal ağa en fazla 4 devre bağlayabilirsiniz.

### <a name="how-do-i-connect-my-virtual-networks-to-an-expressroute-circuit"></a>Sanal ağlarımı ExpressRoute devresine nasıl bağlarım?

Temel adımlar şunlardır:

* Bir ExpressRoute devresi kurun ve servis sağlayıcısının etkinleştirmesini sağla.
* Siz veya sağlayıcı, BGP eşleme(ler) yapılandırmanız gerekir.
* Sanal ağı ExpressRoute devresine bağla.

Daha fazla bilgi [için, devre sağlama ve devre durumları için ExpressRoute iş akışlarına](expressroute-workflows.md)bakın.

### <a name="are-there-connectivity-boundaries-for-my-expressroute-circuit"></a>ExpressRoute devrem için bağlantı sınırları var mı?

Evet. [ExpressRoute iş ortakları ve konumları](expressroute-locations.md) makalesi, bir ExpressRoute devresinin bağlantı sınırlarına genel bir bakış sağlar. ExpressRoute devresi için bağlantı tek bir jeopolitik bölge ile sınırlıdır. ExpressRoute premium özelliğini etkinleştirerek bağlantı, jeopolitik bölgeleri kapsayacak şekilde genişletilebilir.

### <a name="can-i-link-to-more-than-one-virtual-network-to-an-expressroute-circuit"></a>Bir ExpressRoute devresine birden fazla sanal ağa bağlanabilir miyim?

Evet. Standart bir ExpressRoute devresi üzerinde en fazla 10 sanal ağ bağlantısına ve [premium ExpressRoute devresi](#expressroute-premium)üzerinde 100'e kadar bağlantınız olabilir. 

### <a name="i-have-multiple-azure-subscriptions-that-contain-virtual-networks-can-i-connect-virtual-networks-that-are-in-separate-subscriptions-to-a-single-expressroute-circuit"></a>Sanal ağlar içeren birden çok Azure aboneliğim var. Ayrı aboneliklerde bulunan sanal ağları tek bir ExpressRoute devresine bağlayabilir miyim?

Evet. Tek bir ExpressRoute devresi kullanarak devre yle veya farklı aboneliklerle aynı abonelikte en fazla 10 sanal ağ bağlayabilirsiniz. Bu sınır, ExpressRoute premium özelliği etkinleştirilerek artırılabilir.

Daha fazla bilgi için bkz: [Birden çok abonelik arasında ExpressRoute devresi paylaşımı.](expressroute-howto-linkvnet-arm.md)

### <a name="i-have-multiple-azure-subscriptions-associated-to-different-azure-active-directory-tenants-or-enterprise-agreement-enrollments-can-i-connect-virtual-networks-that-are-in-separate-tenants-and-enrollments-to-a-single-expressroute-circuit-not-in-the-same-tenant-or-enrollment"></a>Farklı Azure Etkin Dizin kiracıları veya Kurumsal Sözleşme kayıtlarıyla ilişkili birden çok Azure aboneliğim var. Ayrı kiracılarve kayıtlarda bulunan sanal ağları aynı kiracı veya kayıtta olmayan tek bir ExpressRoute devresine bağlayabilir miyim?

Evet. ExpressRoute yetkilendirmeleri, ek yapılandırma gerektirmeden abonelik, kiracı ve kayıt sınırlarını kaplayabilir. 

Daha fazla bilgi için bkz: [Birden çok abonelik arasında ExpressRoute devresi paylaşımı.](expressroute-howto-linkvnet-arm.md)

### <a name="are-virtual-networks-connected-to-the-same-circuit-isolated-from-each-other"></a>Sanal ağlar aynı devreye bağlı mı?

Hayır. Yönlendirme açısından bakıldığında, aynı ExpressRoute devresine bağlı tüm sanal ağlar aynı yönlendirme etki alanının bir parçasıdır ve birbirlerinden izole edilmez. Rota yalıtımına ihtiyacınız varsa, ayrı bir ExpressRoute devresi oluşturmanız gerekir.

### <a name="can-i-have-one-virtual-network-connected-to-more-than-one-expressroute-circuit"></a>Birden fazla ExpressRoute devresine bağlı bir sanal ağım olabilir mi?

Evet. Tek bir sanal ağı, aynı veya farklı eşleme konumlarında en fazla dört ExpressRoute devresine bağlayabilirsiniz. 

### <a name="can-i-access-the-internet-from-my-virtual-networks-connected-to-expressroute-circuits"></a>ExpressRoute devrelerine bağlı sanal ağlarımdan Internet'e erişebilir miyim?

Evet. BGP oturumu aracılığıyla varsayılan rotaların (0.0.0.0/0) veya Internet rota önekleri reklamı yapmadıysanız, ExpressRoute devresine bağlı sanal bir ağdan Internet'e bağlanabilirsiniz.

### <a name="can-i-block-internet-connectivity-to-virtual-networks-connected-to-expressroute-circuits"></a>ExpressRoute devrelerine bağlı sanal ağlara Internet bağlantısını engelleyebilir miyim?

Evet. Sanal ağ içinde dağıtılan sanal makinelere tüm Internet bağlantısını engellemek ve tüm trafiği ExpressRoute devresi üzerinden yönlendirmek için varsayılan rotaların (0.0.0.0/0) reklamını yapabilirsiniz.

Varsayılan yolların reklamını yaparsa, trafiği Microsoft'un bakışları üzerinden sunulan hizmetlere (Azure depolama ve SQL DB gibi) binanıza geri dönmeye zorlarız. Yönlendiricilerinizi, Microsoft'un bakış yolu veya Internet üzerinden Trafiği Azure'a döndürecek şekilde yapılandırmanız gerekir. Hizmet için bir hizmet bitiş noktası etkinleştirdim, hizmet trafiğini binanıza zorlanmıyor. Trafik, Azure omurga ağı içinde kalır. Hizmet bitiş noktaları hakkında daha fazla bilgi edinmek için [Sanal ağ hizmeti bitiş noktalarına](../virtual-network/virtual-network-service-endpoints-overview.md?toc=%2fazure%2fexpressroute%2ftoc.json) bakın

### <a name="can-virtual-networks-linked-to-the-same-expressroute-circuit-talk-to-each-other"></a>Aynı ExpressRoute devresine bağlı sanal ağlar birbirleriyle konuşabilir mi?

Evet. Aynı ExpressRoute devresine bağlı sanal ağlarda dağıtılan sanal makineler birbirleriyle iletişim kurabilir.

### <a name="can-i-use-site-to-site-connectivity-for-virtual-networks-in-conjunction-with-expressroute"></a>ExpressRoute ile birlikte sanal ağlar için siteden siteye bağlantı kullanabilir miyim?

Evet. ExpressRoute, siteden siteye VPN'lerle bir arada bulunabilir. Bkz. [ExpressRoute'u ve siteden siteye birlikte var olan bağlantıları yapılandırın.](expressroute-howto-coexist-resource-manager.md)

### <a name="why-is-there-a-public-ip-address-associated-with-the-expressroute-gateway-on-a-virtual-network"></a>Neden sanal ağda ExpressRoute ağ geçidiyle ilişkili genel bir IP adresi var?

Genel IP adresi yalnızca dahili yönetim için kullanılır ve sanal ağınızın güvenlik pozlamasını oluşturmaz.

### <a name="are-there-limits-on-the-number-of-routes-i-can-advertise"></a>Reklam verebileceğim güzergah sayısında sınırlamalar var mı?

Evet. Özel bakış için 4000'e kadar rota önekleri ve Microsoft'un bakışları için 200'e kadar rota önekleri kabul ediyoruz. ExpressRoute premium özelliğini etkinleştiriyorsanız, bunu özel bakış için 10.000 rotaya yükseltebilirsiniz.

### <a name="are-there-restrictions-on-ip-ranges-i-can-advertise-over-the-bgp-session"></a>BGP oturumunda reklam verebileceğim IP aralıklarında kısıtlamalar var mı?

Microsoft bakan BGP oturumu için özel önekleri (RFC1918) kabul etmeyiz. Hem Microsoft'ta hem de özel eşlemede önek boyutunu (/32'ye kadar) kabul ediyoruz.

### <a name="what-happens-if-i-exceed-the-bgp-limits"></a>BGP limitlerini aşarsam ne olur?

BGP oturumları bırakılır. Önek sayısı sınırın altına indikten sonra sıfırlanacaklar.

### <a name="what-is-the-expressroute-bgp-hold-time-can-it-be-adjusted"></a>ExpressRoute BGP bekleme süresi nedir? Ayarlanabilir mi?

Bekleme süresi 180. Canlı tutma mesajları her 60 saniyede bir gönderilir. Bunlar, Microsoft tarafında değiştirilemeyen sabit ayarlardır. Farklı zamanlayıcıları yapılandırmanız mümkündür ve BGP oturum parametreleri buna göre görüşülecektir.

### <a name="can-i-change-the-bandwidth-of-an-expressroute-circuit"></a>ExpressRoute devresinin bant genişliğini değiştirebilir miyim?

Evet, Azure portalındaki ExpressRoute devrenizin bant genişliğini artırmaya çalışabilir veya PowerShell'i kullanarak. Devrenizin oluşturulduğu fiziksel bağlantı noktasında kapasite varsa, değişikliğiniz başarılı olur. 

Değişikliğiniz başarısız olursa, geçerli bağlantı noktasında yeterli kapasite kalmamış ve daha yüksek bant genişliğine sahip yeni bir ExpressRoute devresi oluşturmanız gerekir veya bu konumda ek kapasite yoktur, bu durumda Bant genişliği. 

Ayrıca, bant genişliği artışını desteklemek için ağlarındaki gazları güncelleştirdiğinden emin olmak için bağlantı sağlayıcınızla da izlemeniz gerekir. Ancak, ExpressRoute devrenizin bant genişliğini azaltamazsınız. Daha düşük bant genişliğine sahip yeni bir ExpressRoute devresi oluşturmanız ve eski devreyi silmeniz gerekir.

### <a name="how-do-i-change-the-bandwidth-of-an-expressroute-circuit"></a>ExpressRoute devresinin bant genişliğini nasıl değiştirebilirim?

REST API veya PowerShell cmdlet kullanarak ExpressRoute devresinin bant genişliğini güncelleyebilirsiniz.

## <a name="expressroute-premium"></a>ExpressRoute premium

### <a name="what-is-expressroute-premium"></a>ExpressRoute premium nedir?

ExpressRoute premium aşağıdaki özelliklerin bir koleksiyondur:

* Yönlendirme tablosu sınırı 4000 rotadan özel izleme için 10.000 rotaya yükseltildi.
* ExpressRoute devresi üzerinde etkinleştirilebilen artan VNet ve ExpressRoute Global Reach bağlantısı sayısı (varsayılan değer 10'dur). Daha fazla bilgi için [ExpressRoute Limits](#limits) tablosuna bakın.
* Office 365'e Bağlantı
* Microsoft çekirdek ağı üzerinden genel bağlantı. Artık bir jeopolitik bölgedeki bir VNet'i başka bir bölgedeki ExpressRoute devresiyle bağlayabilirsiniz.<br>
    **Örnekler:**

    *  Batı Avrupa'da oluşturulan bir VNet'i Silikon Vadisi'nde oluşturulan bir ExpressRoute devresine bağlayabilirsiniz. 
    *  Microsoft'un bakışlarında, diğer jeopolitik bölgelerden gelen önekler, örneğin, Microsoft West'teki SQL Azure'a Silikon Vadisi'ndeki bir devreden bağlanabileceğiniz şekilde duyurulur.


### <a name="how-many-vnets-and-expressroute-global-reach-connections-can-i-enable-on-an-expressroute-circuit-if-i-enabled-expressroute-premium"></a><a name="limits"></a>ExpressRoute premium'u etkinleştirdiysem expressroute devresi üzerinde kaç VNets ve ExpressRoute Global Reach bağlantısı etkinleştirebilirim?

Aşağıdaki tablolar ExpressRoute sınırlarını ve ExpressRoute devresi başına VNets ve ExpressRoute Global Reach bağlantı sayısını gösterir:

[!INCLUDE [ExpressRoute limits](../../includes/expressroute-limits.md)]

### <a name="how-do-i-enable-expressroute-premium"></a>ExpressRoute premium'u nasıl etkinleştirebilirim?

Özellik etkinleştirildiğinde ExpressRoute premium özellikleri etkinleştirilebilir ve devre durumu güncelleştirilerek kapatılabilir. Devre oluşturma zamanında ExpressRoute premium'u etkinleştirebilir veya REST API / PowerShell cmdlet'i arayabilirsiniz.

### <a name="how-do-i-disable-expressroute-premium"></a>ExpressRoute premium'u nasıl devre dışı alarım?

REST API veya PowerShell cmdlet'i arayarak ExpressRoute primini devre dışı bırakabilirsiniz. ExpressRoute primini devre dışı etmeden önce bağlantı gereksinimlerinizi varsayılan sınırları karşılamak için ölçeklendirdiğinizden emin olmalısınız. Kullanımınız varsayılan sınırların ötesine ölçeklenirse, ExpressRoute premium'u devre dışı bırakma isteği başarısız olur.

### <a name="can-i-pick-and-choose-the-features-i-want-from-the-premium-feature-set"></a>Premium özellik kümesinden istediğim özellikleri seçip seçebilir miyim?

Hayır. Özellikleri seçemezsin. ExpressRoute premium'u açtığınızda tüm özellikleri etkinleştiriz.

### <a name="how-much-does-expressroute-premium-cost"></a>ExpressRoute premium ücreti ne kadardır?

Maliyet için [fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/expressroute/) bakın.

### <a name="do-i-pay-for-expressroute-premium-in-addition-to-standard-expressroute-charges"></a>Standart ExpressRoute ücretlerine ek olarak ExpressRoute primi için ödeme yapıyor muyum?

Evet. ExpressRoute premium ücretleri, bağlantı sağlayıcısı tarafından gerekli olan ExpressRoute devre ücretleri ve ücretleri nin üzerine uygulanır.

## <a name="expressroute-local"></a>ExpressRoute Yerel
### <a name="what-is-expressroute-local"></a>ExpressRoute Local nedir?
ExpressRoute Local, Standart SKU ve Premium SKU'ya ek olarak ExpressRoute devresinin bir SKU'sudur. Yerel'in önemli bir özelliği, ExpressRoute'daki yerel bir devrenin aynı metroda veya yakınında yalnızca bir veya iki Azure bölgesine erişmenizi sağlar. Buna karşılık, Standart devre, jeopolitik alandaki tüm Azure bölgelerine ve tüm Azure bölgelerine küresel olarak Premium devre erişimi sağlar. 

### <a name="what-are-the-benefits-of-expressroute-local"></a>ExpressRoute Local'ın avantajları nelerdir?
Standart veya Premium ExpressRoute devreniz için çıkış veri aktarımı ödemeniz gerekirken, ExpressRoute Local devreniz için çıkış veri aktarımı ayrı olarak ödeme yapmazsınız. Başka bir deyişle, ExpressRoute Local'ın fiyatına veri aktarım ücretleri dahildir. ExpressRoute Local, aktarım için çok büyük miktarda veriniz varsa ve verilerinizi istediğiniz Azure bölgelerinin yakınındaki Bir ExpressRoute gözleme konumuna özel bir bağlantı üzerinden getirebiliyorsanız, daha ekonomik bir çözümdür. 

### <a name="what-features-are-available-and-what-are-not-on-expressroute-local"></a>ExpressRoute Local'da hangi özellikler mevcut ve neler yok?
Standart ExpressRoute devresi ile karşılaştırıldığında, Yerel bir devre aşağıdakiler dışında aynı özellik kümesine sahiptir:
* Yukarıda açıklandığı gibi Azure bölgelerine erişim kapsamı
* ExpressRoute Global Reach Yerel'de kullanılamıyor

ExpressRoute Local ayrıca kaynaklarla (örn. devre başına VNet sayısı) Standart ile aynı sınırlara sahiptir. 

### <a name="where-is-expressroute-local-available-and-which-azure-regions-is-each-peering-location-mapped-to"></a>ExpressRoute Local nerede kullanılabilir ve her bir bakan konumu eşlenir?
ExpressRoute Local, bir veya iki Azure bölgesinin yakın olduğu gözarama konumlarında kullanılabilir. Bu eyalette veya eyalette veya ülkede Azure bölgesinin olmadığı bir konumda kullanılamaz. Lütfen [Konumlar sayfasındaki](expressroute-locations-providers.md)tam eşlemelere bakın.  

## <a name="expressroute-for-office-365"></a>Office 365 için ExpressRoute

[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

### <a name="how-do-i-create-an-expressroute-circuit-to-connect-to-office-365-services"></a>Office 365 hizmetlerine bağlanmak için expressroute devresini nasıl oluştururum?

1. Gereksinimleri karşıladığınızdan emin olmak için [ExpressRoute ön koşullar sayfasını](expressroute-prerequisites.md) inceleyin.
2. Bağlantı gereksinimlerinizin karşılandığından emin olmak [için, ExpressRoute iş ortakları ve konumları](expressroute-locations.md) makalesindeki servis sağlayıcıları ve konumların listesini inceleyin.
3. [Office 365 için Ağ planlama ve performans atonlama](https://aka.ms/tune/)gözden geçirerek kapasite gereksinimlerinizi planlayın.
4. Devre sağlama ve devre durumları için bağlantı [ExpressRoute iş akışlarını](expressroute-workflows.md)ayarlamak için iş akışlarında listelenen adımları izleyin.

> [!IMPORTANT]
> Office 365 hizmetlerine bağlantı nızı yapılandırırken ExpressRoute premium eklentisini etkinleştirdiğinizden emin olun.
> 
> 

### <a name="can-my-existing-expressroute-circuits-support-connectivity-to-office-365-services"></a>Mevcut ExpressRoute devrelerim Office 365 hizmetlerine bağlantı sağlayabilir mi?

Evet. Mevcut ExpressRoute devreniz, Office 365 hizmetlerine bağlantı sağlamak üzere yapılandırılabilir. Office 365 hizmetlerine bağlanmak için yeterli kapasiteye sahip olduğunuzu ve premium eklentiyi etkinleştirdiğinizden emin olun. [Office 365 için ağ planlama ve performans atonlama,](https://aka.ms/tune/) bağlantı ihtiyaçlarınızı planlamanıza yardımcı olur. Ayrıca, [bkz.](expressroute-howto-circuit-classic.md)

### <a name="what-office-365-services-can-be-accessed-over-an-expressroute-connection"></a>ExpressRoute bağlantısı üzerinden hangi Office 365 hizmetlerine erişilebilir?

ExpressRoute üzerinden desteklenen hizmetlerin güncel bir listesi için [Office 365 URL'leri ve IP adresi aralıkları](https://aka.ms/o365endpoints) sayfasına bakın.

### <a name="how-much-does-expressroute-for-office-365-services-cost"></a>Office 365 hizmetleri için ExpressRoute'un maliyeti nedir?

Office 365 hizmetlerinin etkinleştirilmesi için premium eklenti gerekir. Maliyetler için [fiyatlandırma ayrıntıları sayfasına](https://azure.microsoft.com/pricing/details/expressroute/) bakın.

### <a name="what-regions-is-expressroute-for-office-365-supported-in"></a>Office 365 için ExpressRoute hangi bölgelerde desteklenir?

Bilgi için [ExpressRoute iş ortakları ve konumlarına](expressroute-locations.md) bakın.

### <a name="can-i-access-office-365-over-the-internet-even-if-expressroute-was-configured-for-my-organization"></a>ExpressRoute kuruluşum için yapılandırıldıysa bile Office 365'e Internet üzerinden erişebilir miyim?

Evet. ExpressRoute ağınız için yapılandırılmış olsa bile, Office 365 hizmet bitiş noktalarına Internet üzerinden ulaşılabilir. Bulunduğunuz yerdeki ağ ExpressRoute üzerinden Office 365 hizmetlerine bağlanacak şekilde yapılandırılmışsa, lütfen kuruluşunuzun ağ ekibine danışın.

### <a name="how-can-i-plan-for-high-availability-for-office-365-network-traffic-on-azure-expressroute"></a>Azure ExpressRoute'daki Office 365 ağ trafiği için yüksek kullanılabilirlik için nasıl plan yapabilirim?
[Azure ExpressRoute ile Yüksek kullanılabilirlik ve başarısızlık](https://aka.ms/erhighavailability) için öneriye bakın

### <a name="can-i-access-office-365-us-government-community-gcc-services-over-an-azure-us-government-expressroute-circuit"></a>Bir Azure ABD Kamu Ekspres Yolu devresi üzerinden Office 365 ABD Kamu Topluluğu (GCC) hizmetlerine erişebilir miyim?

Evet. Office 365 GCC hizmet bitiş noktalarına Azure ABD Devlet Ekspres Yolu üzerinden ulaşılabilir. Ancak, microsoft'a reklam vermek istediğiniz önekleri sağlamak için öncelikle Azure portalında bir destek bileti açmanız gerekir. Destek bileti çözüldükten sonra Office 365 GCC hizmetlerine bağlantınız kurulacaktır. 

## <a name="route-filters-for-microsoft-peering"></a>Microsoft eşleme için rota filtreleri

### <a name="i-am-turning-on-microsoft-peering-for-the-first-time-what-routes-will-i-see"></a>Microsoft'a ilk kez bakmayı açıyorum, hangi yolları göreceğim?

Herhangi bir güzergah göremeyeceksiniz. Önek reklamlar başlatmak için devrenize bir rota filtresi eklemeniz gerekir. Yönergeler [için, Microsoft eşleme için rota filtrelerini yapılandır'a](how-to-routefilter-powershell.md)bakın.

### <a name="i-turned-on-microsoft-peering-and-now-i-am-trying-to-select-exchange-online-but-it-is-giving-me-an-error-that-i-am-not-authorized-to-do-it"></a>Microsoft'a bakış açtım ve şimdi Exchange Online'ı seçmeye çalışıyorum, ancak bunu yapmaya yetkili olmadığım konusunda bana bir hata veriyor.

Rota filtrelerini kullanırken, herhangi bir müşteri Microsoft'un eşleme lerini açabilir. Ancak, Office 365 hizmetlerini tüketmek için Office 365 tarafından yetkialmanız gerekir.

### <a name="i-enabled-microsoft-peering-prior-to-august-1-2017-how-can-i-take-advantage-of-route-filters"></a>Microsoft'un 1 Ağustos 2017'den önce bakmasını sağladım, rota filtrelerinden nasıl yararlanabilirim?

Varolan devreniz Office 365'in öneklerinin reklamını yapmaya devam eder. Aynı Microsoft eşlemesine Azure genel önekleri reklamları eklemek istiyorsanız, bir rota filtresi oluşturabilir, reklamı gereken hizmetleri (ihtiyacınız olan Office 365 hizmeti(ler dahil) seçebilir ve filtreyi Microsoft bakışlarınıza ekleyebilirsiniz. Yönergeler [için, Microsoft eşleme için rota filtrelerini yapılandır'a](how-to-routefilter-powershell.md)bakın.

### <a name="i-have-microsoft-peering-at-one-location-now-i-am-trying-to-enable-it-at-another-location-and-i-am-not-seeing-any-prefixes"></a>Microsoft bir konumda bakıyor, şimdi başka bir konumda etkinleştirmek için çalışıyorum ve herhangi bir önek göremiyorum.

* 1 Ağustos 2017 tarihinden önce yapılandırılan ExpressRoute devrelerinin Microsoft'a bakması, rota filtreleri tanımlanmamış olsa bile, Microsoft'un bakışları aracılığıyla duyurulan tüm hizmet önekleri olacaktır.

* Microsoft' un 1 Ağustos 2017 tarihinde veya sonrasında yapılandırılan ExpressRoute devrelerine bakması, devreye bir rota filtresi iliştirilene kadar herhangi bir önek reklamı yapmayacaktır. Varsayılan olarak hiçbir önek görmezsiniz.

## <a name="expressroute-direct"></a><a name="expressRouteDirect"></a>ExpressRoute Direct

[!INCLUDE [ExpressRoute Direct](../../includes/expressroute-direct-faq-include.md)]

## <a name="global-reach"></a><a name="globalreach"></a>Küresel Erişim

[!INCLUDE [Global Reach](../../includes/expressroute-global-reach-faq-include.md)]
