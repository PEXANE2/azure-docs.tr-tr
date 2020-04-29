---
title: Azure sanal WAN ve güvenli hub kullanarak Çin ile bağlantı
description: Sanal WAN otomatik ölçeklenebilir dalı-dal bağlantısı, kullanılabilir bölgeler ve iş ortakları hakkında bilgi edinin.
services: virtual-wan
author: skishen525
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: sukishen
ms.openlocfilehash: d086484ece6faf95dccffb1e29da8a0e906b1da4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80985635"
---
# <a name="interconnect-with-china-using-azure-virtual-wan-and-secure-hub"></a>Azure sanal WAN ve güvenli hub kullanarak Çin ile bağlantı

Ortak bir oto, üretim, lojistik sektörlere veya emeltes gibi diğer Kurumsal Sektörlere baktığınızda, genellikle Çin ile iç bağlantı geliştirme hakkında soru vardır. Bu geliştirmeler çoğunlukla Office 365, Azure küresel hizmetler veya bir müşteri omurgası ile Çin 'in içindeki bağlantı dalları gibi Cloud Services kullanmak için geçerlidir.

Çoğu durumda, müşteriler yüksek gecikme süresi, düşük bant genişliği, kararsız bağlantı ve Çin dışında (örneğin, Avrupa veya Birleşik Devletler) yüksek maliyetlerle bağlantı altına alınır.

Bu bir nedenden dolayı, Internet 'in Çince bölümünü koruyan ve trafiği Çin 'e filtreleyen "Çin güvenlik duvarı 'nın" harika güvenlik duvarıdır. Çin Mainland ile Çin dışında çalışan neredeyse tüm trafik, Hong Kong ve Macau gibi özel yönetim bölgeleri haricinde harika güvenlik duvarını geçirir. Hong Kong ve Macau aracılığıyla çalışan trafik, eksiksiz güvenlik duvarında harika güvenlik duvarının bir alt kümesi tarafından işlenir.

![Sağlayıcı bağlantısı](./media/interconnect-china/provider.png)

Bir müşteri, sanal WAN 'ı kullanarak, Microsoft Bulut hizmetlere daha fazla performans ve kararlı bir bağlantı kurabilir ve bu, Çince siber güvenlik yasasını bozmadan kurumsal ağıyla bağlantı kurabilir.

## <a name="requirements-and-workflow"></a><a name="requirements"></a>Gereksinimler ve iş akışı

Çince siber güvenlik yasaları ile uyumlu kalmak istiyorsanız belirli koşullar kümesini karşılamanız gerekir.

İlk olarak, Çin için bir ıCP (Internet Içerik sağlayıcısı) lisansına sahip bir ağ ve ISS ile birlikte çalışmanız gerekir. Çoğu durumda aşağıdaki sağlayıcılardan birini kullanacaksınız:

* Çin telekomünikasyon küresel Ltd.
* Çin Mobile Ltd.
* Çin Unicom Ltd.
* PCCW Global Ltd.
* Hong Kong Telem Ltd.

Sağlayıcıya ve gereksinimlerinize bağlı olarak, şimdi Çin 'deki Dallarınızı bağlamak için aşağıdaki ağ bağlantısı hizmetlerinden birini satın almanız gerekir.

* MPLS/IPVPN ağı 
* Yazılım tanımlı WAN (SDWAN)
* Adanmış Internet erişimi

Daha sonra, Microsoft Global Network 'e ve uç Ağa gelen Hong Kong ' ye (PIN veya Shanghai değil) izin vermek için bu sağlayıcıyı kabul etmeniz gerekir. Bu durumda, fiziksel bağlantısı ve Çin konumu nedeniyle Hong Kong çok önemlidir.

Çoğu müşteri, haritada arama yaparken Çin 'e en iyi şekilde bakacağından, bu durum doğru değildir. Ağ Fiber haritalarını izlediğinizde, neredeyse tüm ağ bağlantıları PI, Shanghai ve Hong Kong aracılığıyla gider. Bu, Hong Kong 'yi Çin 'e bağlamak için daha iyi bir konum seçeneği sunar.

Sağlayıcıya bağlı olarak, farklı hizmet teklifleri alabilirsiniz. Aşağıdaki tabloda, bu makalede yazıldığı sırada bilgiler temel alınarak, sağlayıcıların ve sundukları hizmetin bir örneği gösterilmektedir.

| Hizmet | Sağlayıcı örnekleri |
| --- | --- |
| MPLS/IPVPN ağı |PCCW, Çin telekomünikasyon küresel |
|SDWAN| PCCW, Çin telekomünikasyon küresel|
| Adanmış Internet erişimi | PCCW, Hong Kong Telem, Çin mobil|

Sağlayıcınızda, Microsoft Global omurgaya ulaşmak için aşağıdaki iki çözümün ne kadar kullanılacağını kabul edebilirsiniz:

* Microsoft Azure ExpressRoute, Hong Kong ' de sonlandırıldı. Bu, MPLS/IPVPN kullanımı için büyük bir durumdur. Şu anda yalnızca Hong Kong için ExpressRoute 'a sahip tek ıCP lisans sağlayıcısı Çin Telekodır. Ancak, Megaport veya Intercloud gibi bulut Exchange sağlayıcılarından yararlanıyorsa diğer sağlayıcılarla de iletişim kurabilir. Daha fazla bilgi için bkz. [ExpressRoute bağlantı sağlayıcıları](../expressroute/expressroute-locations-providers.md#partners).

* Aşağıdaki Internet Exchange noktalarından biri veya özel bir ağ bağlantısı kullanarak, adanmış bir Internet erişimini doğrudan kullanma.

Aşağıdaki listede, Hong Kong 'da olası Internet değişimleri gösterilmektedir:

* AMS-IX Hong Kong
* BBIX Hong Kong
* Equinx Hong Kong
* HKIX

Bu Connect kullanılırken, Microsoft hizmetlerine yönelik bir sonraki BGP atlayağınızın Microsoft otonom sistem numarası (AS #) 8075 olması gerekir. Tek bir konum veya SDWAN çözümü kullanıyorsanız, bu bağlantı seçimi olacaktır.

Her iki durumda da, Çince ana kara 'nin ikinci ve normal bir Internet ayırıcıya sahip olmasını öneririz. Bu, kurumsal trafik arasındaki trafiği Microsoft 365 ve Azure gibi bulut hizmetleri ve yasalar tarafından düzenlenen Internet trafiği arasında bölecektir.

Çin 'de uyumlu bir ağ mimarisi aşağıdaki örnekteki gibi görünebilir:

![Birden çok dal](./media/interconnect-china/multi-branch.png)

Bu örnekte, Microsoft Global Ağa gelen Hong Kong ile Interconnect 'e sahip olmak için artık [Azure sanal WAN küresel aktarım mimarisinden](virtual-wan-global-transit-network-architecture.md) ve Azure GÜVENLI sanal WAN hub 'ı gibi ek hizmetlerden yararlanarak, Çin ve veri merkezinize yönelik hizmetleri ve bağlantı merkezini kullanabilirsiniz.

## <a name="hub-to-hub-communication"></a><a name="hub-to-hub"></a>Hub-hub arası iletişim

Bu bölümde, Interconnect 'e yönelik sanal WAN hub-hub iletişimini kullanırız. Bu senaryoda, Hong Kong ' de bir sanal WAN hub 'ına, tercih ettiğiniz diğer bölgelere, Azure kaynaklarına sahip olduğunuz bir bölgeye veya bağlanmak istediğiniz yere bağlanmak üzere yeni bir sanal WAN hub kaynağı oluşturacaksınız.

Örnek bir mimari aşağıdaki örnekteki gibi görünebilir:

![Örnek WAN](./media/interconnect-china/sample.png)

Bu örnekte, Çin dalları VPN veya MPLS bağlantıları kullanarak Azure bulutu Çin ve birbirlerine bağlanır. Küresel hizmetlere bağlanması gereken dallar, doğrudan Hong Kong 'a bağlı olan MPLS veya Internet tabanlı Hizmetleri kullanır. ExpressRoute 'u Hong Kong 'da ve diğer bölgede kullanmak istiyorsanız ExpressRoute [Global Reach](../expressroute/expressroute-global-reach.md) her Iki ExpressRoute devresine de bağlamak üzere yapılandırmanız gerekir.

ExpressRoute Global Reach bazı bölgelerde kullanılamıyor. Örneğin, Brezilya veya Hindistan ile bağlantı oluşturmanız gerekiyorsa, yönlendirme hizmetleri sağlamak için [bulut Exchange sağlayıcılarından](../expressroute/expressroute-locations.md#connectivity-through-exchange-providers) yararlanın.

Aşağıdaki şekilde, bu senaryoya ilişkin örneklerin her ikisi de gösterilmektedir.

![Global Reach](./media/interconnect-china/global.png)

## <a name="secure-internet-breakout-for-office-365"></a><a name="secure"></a>Office 365 için güvenli Internet kırılımı

Diğer bir deyişle, ağ güvenliği, Çin ve sanal WAN tarafından sağlanan omurga bileşeni ve müşteri omurgası arasında giriş noktası için günlüğe kaydetme işleminin yanı sıra bir noktadır. Çoğu durumda, Microsoft Edge ağına doğrudan ulaşmak ve bununla birlikte Microsoft 365 hizmetleri için kullanılan Azure ön kapılarını sağlamak üzere Hong Kong 'da Internet 'e dikkat etmeniz gerekir.

Sanal WAN ile her iki senaryo için de [Azure sanal WAN güvenli hub 'dan](../firewall-manager/secured-virtual-hub.md)yararlanabilirsiniz. Azure Güvenlik Duvarı Yöneticisi 'ni kullanarak, düzenli bir sanal WAN hub 'ını güvenli bir hub 'a değiştirebilir ve ardından bu hub 'da bir Azure Güvenlik Duvarı dağıtabilir ve yönetebilirsiniz.

Aşağıdaki şekilde bu senaryonun bir örneği gösterilmektedir:

![Web ve Microsoft Hizmetleri trafiği için Internet kırılımı](./media/interconnect-china/internet.png)

## <a name="architecture-and-traffic-flows"></a><a name="traffic"></a>Mimari ve trafik akışları

Hong Kong bağlantısı ile ilgili seçiminize bağlı olarak, genel mimari biraz değişebilir. Bu bölümde, VPN veya SDWAN ve/veya ExpressRoute ile farklı birleşimlerle kullanılabilir üç mimari gösterilmektedir.

Bu seçeneklerin tümü, Hong Kong 'da doğrudan M365 bağlantısı için Azure sanal WAN güvenli hub 'ını kullanır. Bu mimariler Ayrıca [office 365 çoklu coğrafi bölge](https://docs.microsoft.com/office365/enterprise/office-365-multi-geo) uyumluluk gereksinimlerini destekler ve bu trafiği sonraki Office 365 ön kapısının yakınında tutar. Sonuç olarak, Çin 'den Microsoft 365 kullanımı için de bir iyileştirmedir.

Azure sanal WAN 'ı Internet bağlantılarıyla birlikte kullanırken, her bağlantı [Microsoft Azure eşleme Hizmetleri (haritalar)](https://docs.microsoft.com/azure/peering-service/about)gibi ek hizmetlerden yararlanabilir. HARITALAR, üçüncü taraf Internet servis sağlayıcılarından gelen Microsoft Global ağı 'na gelen trafiği iyileştirmek için oluşturulmuştur.

### <a name="option-1-sdwan-or-vpn"></a><a name="option-1"></a>Seçenek 1: SDWAN veya VPN

Bu bölümde, SDWAN veya VPN 'yi Hong Kong ve diğer dallara kullanan bir tasarım açıklanmaktadır. Bu seçenek, sanal WAN omurgasını her iki sitesinde de saf Internet bağlantısı kullanırken kullanım ve trafik akışını gösterir. Bu durumda bağlantı, adanmış Internet erişimi veya bir ıCP sağlayıcısı SDWAN çözümü kullanılarak Hong Kong 'ye getirilir. Diğer dallar da saf Internet veya SDWAN çözümlerini kullanıyor.

![Çin-Hong Kong trafiği](./media/interconnect-china/china-traffic.png)

Bu mimaride, her site, VPN ve Azure sanal WAN kullanılarak Microsoft küresel ağa bağlanır. Siteler ve Hong Kong arasındaki trafik, Microsoft ağı Trough iletilir ve yalnızca son mil üzerinde normal Internet bağlantısı kullanır.

### <a name="option-2-expressroute-and-sdwan-or-vpn"></a><a name="option-2"></a>Seçenek 2: ExpressRoute ve SDWAN veya VPN

Bu bölümde, Hong Kong ve diğer dallardaki VPN/SDWAN dallarıyla ExpressRoute kullanan bir tasarım açıklanmaktadır. Bu seçenek, Hong Kong ve SDWAN veya VPN aracılığıyla bağlı diğer dallarda sonlandırılan ve ExpressRoute kullanımını gösterir. Hong Kong 'daki ExpressRoute Şu anda [hızlı rota Iş ortakları](../expressroute/expressroute-locations-providers.md#global-commercial-azure)listesinde bulabileceğiniz kısa bir sağlayıcı listesiyle sınırlıdır.

![Çin-Hong Kong trafik ExpressRoute](./media/interconnect-china/expressroute.png)

Ayrıca, Çin 'den ExpressRoute 'u (örneğin, Güney Kore veya Japonya) sonlandırma seçenekleri de vardır. Ancak, uyumluluk, düzenleme ve gecikme süresi, Hong Kong Şu anda en iyi seçimdir.

### <a name="option-3-expressroute-only"></a><a name="option-3"></a>Seçenek 3: yalnızca ExpressRoute

Bu bölümde, ExpressRoute 'un Hong Kong ve diğer dallar için kullanıldığı bir tasarım açıklanmaktadır. Bu seçenekte her iki uçta de ExpressRoute kullanılarak bağlantı gösterilmektedir. Burada, diğerinin farklı bir trafik akışı vardır. Microsoft 365 trafiği, Azure sanal WAN güvenli hub 'ına ve buradan Microsoft Edge ağı ve Internet 'e akacaktır.

Birbirine bağlı dallara veya bunlardan, Çin 'deki konumlara giden trafik, Bu mimaride farklı bir yaklaşıma uyar. Şu anda sanal WAN, ExpressRoute ulaşım 'e ExpressRoute 'ı desteklemiyor. Trafik, sanal WAN hub 'ını geçirmeden ExpressRoute Global Reach veya üçüncü taraf Interconnect 'ten yararlanır. Doğrudan bir Microsoft Enterprise Edge (MSEE) ' den diğerine akacaktır.

![ExpressRoute Global Reach hakkında](./media/interconnect-china/expressroute-virtual.png)

Şu anda ExpressRoute Global Reach her ülkede kullanılamaz, ancak Azure sanal WAN kullanarak bir çözüm yapılandırabilirsiniz.

Örneğin, Microsoft eşlemesiyle bir ExpressRoute yapılandırabilir ve Azure sanal WAN ile bu eşleme aracılığıyla bir VPN tüneli bağlayabilirsiniz. Artık, Megaport bulutu gibi Global Reach ve 3. taraf sağlayıcı ve hizmet olmadan VPN ve ExpressRoute arasında geçişi etkinleştirdiniz.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure sanal WAN ile küresel aktarım ağı mimarisi](virtual-wan-global-transit-network-architecture.md)

* [Sanal WAN hub 'ı oluşturma](virtual-wan-site-to-site-portal.md)

* [Sanal WAN güvenli hub 'ı yapılandırma](../firewall-manager/secure-cloud-network.md)

* [Azure eşleme hizmeti önizlemesine genel bakış](https://docs.microsoft.com/azure/peering-service/about)
