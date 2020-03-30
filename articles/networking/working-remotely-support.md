---
title: Azure ağ hizmetlerini kullanarak uzaktan çalışma
description: Bu sayfa, uzaktan çalışmayı etkinleştirmek için kullanılabilen Azure ağ hizmetlerini nasıl kullanabileceğinizi ve uzaktan çalışan kişi sayısındaki artıştan kaynaklanan trafik sorunlarını nasıl azaltabileceğinizi açıklar.
services: networking
author: rambk
ms.service: virtual-network
ms.topic: article
ms.date: 03/26/2020
ms.author: rambala
ms.openlocfilehash: 78081329b70edff5ed2c728cb2f7c3e562a44e56
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80346281"
---
# <a name="working-remotely-using-azure-networking-services"></a>Azure ağ hizmetlerini kullanarak uzaktan çalışma

>[!NOTE]
> Bu makalede, COVID-19 krizi nedeniyle karşılaşabileceğiniz ağ sorunlarını uzaktan çalışmak ve ağ sorunlarını azaltmak için Azure ağ hizmetleri, Microsoft ağı ve Azure iş ortağı ekosisteminden nasıl yararlanabileceğiniz açıklanmaktadır.

Bu makalede, kuruluşların kullanıcıları için uzaktan erişim ayarlaması veya mevcut çözümlerini en yoğun kullanım dönemlerinde ek kapasiteyle tamamlaması için kullanabileceği seçenekler açıklanmaktadır. Ağ mimarları aşağıdaki zorluklarla karşı karşıyadır:

- Ağ kullanımındaki artışı ele alın.
- Şirket ve müşterilerinin daha fazla çalışanı için güvenilir-güvenli bağlantı sağlayın.
- Dünya çapında uzak konumlara bağlantı sağlayın.

Tüm ağlar (örneğin, özel WAN ve kurumsal çekirdek ağlar) en yüksek uzak çalışan yükünden kaynaklanan tıkanıklıkla karşılaşır. Darboğazlar genellikle sadece ev geniş bant ağları ve şirketlerin şirket içi ağlarıvpn ağ geçitleri bildirilmiştir.

Ağ planlayıcıları, farklı trafik türlerinin farklı ağ işleme önceliklerine ve bazı akıllı yük yeniden yönlendirme/dağıtımına ihtiyaç duyduğunu göz önünde bulundurarak darboğazları hafifletmeye ve ağ tıkanıklığını hafifletmeye yardımcı olabilir. Örneğin, doktor-hasta etkileşiminin gerçek zamanlı tele-medecine trafiği yüksek öneme sahiptir ve gecikme/titremeye duyarlıdır. Oysa, depolama lar arasında aynı trafiğin çoğaltılması gecikme duyarlı değildir. Eski trafik, daha yüksek hizmet kalitesiyle en uygun ağ yolu üzerinden yönlendirilmelidir; oysa daha sonraki trafiği en uygun olmayan rota üzerinden yönlendirmek kabul edilebilir.

>[!NOTE] 
>Bu makalenin sonuna doğru, uzaktan çalışan kullanıcıları desteklemek için farklı Azure ağ özelliklerinden ve ekosistemlerden yararlanan makalelere bağlantılar listelenir.


## <a name="sharing-our-best-practices---azure-network-is-designed-for-elasticity-and-high-availability"></a>En iyi uygulamalarımızı paylaşmak - Azure ağı elastikiyet ve yüksek kullanılabilirlik için tasarlanmıştır

Azure, kaynakların kullanımındaki ani değişikliklere dayanacak şekilde tasarlanmıştır ve en yüksek kullanım dönemlerinde büyük ölçüde yardımcı olabilir. Ayrıca, Microsoft dünyanın en büyük ağlarından birini tutar ve çalışır. Microsoft'un ağı, farklı hata türlerine dayanabilecek yüksek kullanılabilirlik için tasarlanmıştır: tek bir ağ öğesi hatasından tüm bölgenin başarısızlığına kadar.

Microsoft ağı, Skype ve Takımlar için gecikmeye duyarlı multimedya trafiği, CDN, gerçek zamanlı büyük veri analizi, Azure depolama, Bing ve Xbox dahil olmak üzere farklı ağ trafiği türleri için gereksinimleri karşılamak ve en iyi performansı sağlamak üzere tasarlanmıştır. Farklı trafik türleri için en iyi performansı sağlamak için, Microsoft ağı, kaynaklarının kaynağına mümkün olduğunca yakın bir şekilde geçmesi gereken veya geçmek isteyen tüm trafiği çeker.

>[!NOTE] 
>Aşağıda açıklanan Azure ağ özelliklerini kullanmak, daha iyi bir müşteri ağı deneyimi sağlamak için Microsoft küresel ağının trafik çekim davranışından yararlanır. Microsoft ağının trafik çekim davranışı, en yoğun kullanım dönemlerinde tıkanıklık karşılaşabilecek ilk/son mil ağlarından mümkün olan en kısa sürede trafik yükleme kapalı yardımcı olur.
>

## <a name="enable-employees-to-work-remotely"></a>Çalışanların uzaktan çalışmasını sağlama

Azure VPN ağ geçidi hem Siteden Siteye (P2S) hem de Siteden Siteye (S2S) VPN bağlantılarını destekler. Azure VPN ağ geçidini kullanarak çalışanınızın bağlantılarını ölçeklendirebilir ve hem Azure'a dağıtılan kaynaklarınıza hem de şirket içi kaynaklarınıza güvenli bir şekilde erişebilirsiniz. Daha fazla bilgi için, [kullanıcıların uzaktan çalışmasını nasıl etkinleştirin.](https://go.microsoft.com/fwlink/?linkid=2123770) 

Güvenli Sokettünel Protokolü (SSTP) kullanıyorsanız, eşzamanlı bağlantı sayısı 128 ile sınırlıdır. Daha fazla sayıda bağlantı elde etmek için OpenVPN veya IKEv2'ye geçiş yapmanızı öneririz. Daha fazla bilgi için Bkz. [OpenVPN protokolüne geçiş veya SSTP'den IKEv2.](https://go.microsoft.com/fwlink/?linkid=2124112)

Uzak geliştiriciler, Azure'da dağıtılan kaynaklarınıza erişmek için, VM'lerde genel IP'lere gerek kalmadan güvenli kabuk erişimi (RDP veya SSH) elde etmek için VPN bağlantısı yerine Azure Bastion çözümlerini kullanabilir. Daha fazla bilgi için Azure [Bastion'u kullanarak uzaktan Çalış'a](https://go.microsoft.com/fwlink/?linkid=2123939)bakın.

Büyük ölçekli VPN bağlantısını toplamak için, farklı şirket içi küresel konumlardaki kaynaklar arasındaki bağlantıları desteklemek için, farklı bölgesel hub ve konuşan sanal ağlarda ve birden çok ev geniş bant adedinin kullanımını optimize etmek için Azure Virtual WAN'ı kullanabilirsiniz. Daha fazla bilgi için, [ev ihtiyaçları işe hitap etmek için Mücadele bakın? Azure Virtual WAN'ın burada yardımcı olabileceği yer burasıdır.](https://go.microsoft.com/fwlink/?linkid=2123769)

Uzak iş gücünü desteklemenin bir diğer yolu da Azure sanal ağınızda barındırılan ve Azure Güvenlik Duvarı ile güvenli bir Sanal Masaüstü Altyapısı (VDI) dağıtmaktır. Örneğin, Windows Sanal Masaüstü (WVD), Azure'da çalışan bir masaüstü ve uygulama sanallaştırma hizmetidir. Windows Sanal Masaüstü ile, ek ağ geçidi sunucuları çalıştırmaya gerek kalmadan Azure aboneliğinizde ölçeklenebilir ve esnek bir ortam ayarlayabilirsiniz. Sadece sanal ağınızdaki WVD sanal makinelerinden sorumlusunuz. Daha fazla bilgi için Azure [Güvenlik Duvarı uzaktan iş desteğine](../firewall/remote-work-support.md)bakın. 

Azure'da zengin bir eko sistem ortakları kümesi de vardır. İş ortaklarımız Azure'daki Sanal Cihazlar Ağı da VPN bağlantısının ölçeklendirildirine yardımcı olabilir. Daha fazla bilgi için, [uzak çalışma için Ağ Sanal Cihaz (NVA) hususlar](https://go.microsoft.com/fwlink/?linkid=2123771)bakın.

## <a name="extend-employees-connection-to-access-globally-distributed-resources"></a>Çalışanların bağlantısını genel olarak dağıtılan kaynaklara erişmek için genişletme

Aşağıdaki Azure hizmetleri, çalışanların genel olarak dağıtılan kaynaklarınıza erişmesini sağlamanıza yardımcı olabilir. Kaynaklarınız Azure bölgelerinden herhangi birinde, şirket içi ağlarda ve hatta diğer genel veya özel bulutlarda olabilir. 

- **Azure sanal ağ eşleme**: Kaynaklarınızı birden fazla Azure bölgesinde dağıtıyorsanız ve/veya birden çok sanal ağ kullanarak uzaktan çalışan çalışanların bağlantılarını toplarsanız, sanal ağ eşlemi kullanarak birden çok Azure sanal ağı arasında bağlantı kurabilirsiniz. Daha fazla bilgi için [Sanal ağ eşleme'ye][VNet-peer]bakın.

- **Azure VPN tabanlı çözüm**: P2S veya S2S VPN ile Azure'a bağlanan uzak çalışanlarınız için, şirket içi ağlarınız ve Azure VPN ağ geçidi arasında S2S VPN yapılandırarak şirket içi ağlara erişimi etkinleştirebilirsiniz. Daha fazla bilgi için [bkz.][S2S]

- **ExpressRoute**: ExpressRoute özel bakışını kullanarak Azure dağıtımlarınız ile şirket içi altyapınız veya altyapınız arasında ortak konum tesisinde özel bağlantı sağlayabilirsiniz. ExpressRoute, Microsoft'un bakışları aracılığıyla, şirket içi ağınızdan Microsoft'taki ortak uç noktalara erişilmesine de izin verir. ExpressRoute bağlantıları ortak İnternet üzerinden geçmemektedir. Internet üzerinden tipik bağlantılara göre daha düşük ve tutarlı gecikmelerle güvenli bağlantı, güvenilirlik, daha yüksek verim sunar. Daha fazla bilgi için bkz. [ExpressRoute'a genel bakış][ExR]. [ExpressRoute iş ortağı ekosistemimizin][ExR-eco] bir parçası olan mevcut ağ sağlayıcınızdan yararlanmak, Microsoft'a geniş bant genişliği bağlantıları elde etme süresini azaltmaya yardımcı olabilir.  [ExpressRoute Direct'i][ExR-D] kullanarak şirket içi ağınızı Doğrudan Microsoft omurgasına bağlayabilirsiniz. ExpressRoute Direct çift 10 Gbps veya 100 Gbps iki farklı hat hızı seçenekleri sunuyor. 

- **Azure Virtual WAN**: Azure Virtual WAN, VPN bağlantılarınız ve ExpressRoute devreleriniz arasında sorunsuz birlikte çalışabilirlik sağlar. Azure Virtual WAN daha önce de belirttiğimiz gibi, farklı prem global konumlardaki kaynaklar, farklı bölgesel hub ve konuşan sanal ağlar arasındaki her türlü bağlantıyı da destekler

## <a name="scale-customer-connectivity-to-frontend-resources"></a>Müşteri bağlantısını ön uç kaynaklarına ölçeklendirin

Daha fazla kişinin çevrimiçi olduğu zamanlarda, birçok kurumsal web sitesi artan müşteri trafiğiyle karşılaşır. Azure Application Gateway, artırılmış bu ön uç iş yükünü yönetmenize yardımcı olabilir. Daha fazla bilgi için [Uygulama Ağ Geçidi yüksek trafik desteğine](https://go.microsoft.com/fwlink/?linkid=2123940)bakın.

## <a name="microsoft-support-for-multi-cloud-traffic"></a>Çoklu bulut trafiği için Microsoft desteği

Diğer genel bulutlardaki dağıtımlarınız için Microsoft genel bağlantı sağlayabilir. Azure Virtual WAN, VPN veya ExpressRoute bu konuda yardımcı olabilir. Azure'dan diğer bulutlara bağlantı genişletmek için, S2S VPN'i iki bulut arasında yapılandırabilirsiniz. Ayrıca ExpressRoute'u kullanarak Azure'dan diğer genel bulutlara bağlantı oluşturabilirsiniz. Oracle bulutu ExpressRoute iş ortağı ekosisteminin bir parçasıdır. Azure [ve Oracle Cloud Infrastructure arasında doğrudan bir bağlantı kurabilirsiniz.][Az-OCI] ExpressRoute iş ortağı ekosisteminin bir parçası olan çoğu hizmet sağlayıcısı, diğer genel bulutlara özel bağlantı da sunar. Bu hizmet sağlayıcılardan yararlanarak, ExpressRoute aracılığıyla Azure'daki dağıtımlarınız ve diğer bulutlar arasında özel bağlantı kurabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde, kullanıcıları uzaktan çalışacak şekilde ölçeklendirmek için farklı Azure ağ özelliklerinin nasıl kullanılabileceğini açıklayabilirsiniz:

| **Makale** | **Son güncelleştirme** |
| --- | --- |
| [Kullanıcıların uzaktan çalışmasını etkinleştirme](https://go.microsoft.com/fwlink/?linkid=2123770) | 23 Mart 2020 |
| [Ev ihtiyaçları işe hitap etmek için mücadele? Azure Virtual WAN'ın burada yardımcı olabileceği yer burasıdır](https://go.microsoft.com/fwlink/?linkid=2123769) | 23 Mart 2020 |
| [Uygulama Ağ Geçidi yüksek trafik desteği](https://go.microsoft.com/fwlink/?linkid=2123940) | 23 Mart 2020 |
| [Ağ Sanal Cihaz (NVA) uzak çalışma için hususlar](https://go.microsoft.com/fwlink/?linkid=2123771)| 23 Mart 2020 |
| [SSTP'den OpenVPN protokolüne veya IKEv2'ye geçiş](https://go.microsoft.com/fwlink/?linkid=2124112) | 23 Mart 2020 |
| [Azure Bastion'u kullanarak uzaktan çalışma](https://go.microsoft.com/fwlink/?linkid=2123939) | 23 Mart 2020 |
| [Uzak kullanıcıları desteklemek için karma bağlantı oluşturmak için Azure ExpressRoute'u kullanma](https://go.microsoft.com/fwlink/?linkid=2123768) | 23 Mart 2020 |
| [Azure Güvenlik Duvarı uzaktan çalışma desteği](../firewall/remote-work-support.md)|25 Mart 2020|

<!--Link References-->
[VNet-peer]: https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview
[S2S]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal
[ExR]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[ExR-eco]: https://docs.microsoft.com/azure/expressroute/expressroute-locations
[ExR-D]: https://docs.microsoft.com/azure/expressroute/expressroute-erdirect-about
[Az-OCI]: https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/configure-azure-oci-networking
