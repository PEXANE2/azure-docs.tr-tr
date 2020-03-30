---
title: Azure Virtual WAN ve güvenli Hub'ı kullanarak Çin ile ara bağlantı kurun
description: Sanal WAN otomatik ölçeklenebilir daldan şubeye bağlantı, kullanılabilir bölgeler ve iş ortakları hakkında bilgi edinin.
services: virtual-wan
author: skishen525
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: sukishen
ms.openlocfilehash: e05aa6e8f656001eb9eb2a3717dde6c16f030e33
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80337145"
---
# <a name="interconnect-with-china-using-azure-virtual-wan-and-secure-hub"></a>Azure Virtual WAN ve Secure Hub'ı kullanarak Çin ile ara bağlantı

Ortak otomotiv, imalat, lojistik endüstrileri veya elçilikler gibi diğer enstitülere bakarken, Çin ile ara bağlantının nasıl geliştirileceklerine dair soru genellikle vardır. Bu geliştirmeler çoğunlukla Office 365, Azure Global Services gibi Bulut Hizmetlerini kullanmak veya Çin'in içindeki şubeleri müşteri omurgasıyla birleştirmek için geçerlidir.

Çoğu durumda, müşteriler yüksek gecikme süreleri, düşük bant genişliği, kararsız bağlantı ve Çin dışında (örneğin, Avrupa veya Amerika Birleşik Devletleri) bağlanan yüksek maliyetler ile mücadele ediyor.

Bu mücadelelerin bir nedeni internetin Çin kısmını koruyan ve Çin trafiğini filtreleyen "Çin'in Büyük Güvenlik Duvarı"dır. Hong Kong ve Makao gibi özel yönetim bölgeleri dışında Çin Anakara'dan Çin dışına çalışan neredeyse tüm trafik, Büyük Güvenlik Duvarı geçer. Hong Kong ve Makao üzerinden çalışan trafik tam güçle Büyük Güvenlik Duvarı isabet etmez, büyük güvenlik duvarının bir alt kümesi tarafından ele alınır.

![Sağlayıcı ara bağlantısı](./media/interconnect-china/provider.png)

Sanal WAN'ı kullanan bir müşteri, Microsoft Bulut Hizmetleri'ne daha uyumlu ve kararlı bir bağlantı ve Çin siber güvenlik yasasını ihlal etmeden kurumsal ağına bağlantı kurabilir.

## <a name="requirements-and-workflow"></a><a name="requirements"></a>Gereksinimler ve iş akışı

Çin siber güvenlik yasasına uygun kalmak istiyorsanız, belirli koşullar alabilmeniz gerekir.

İlk olarak, Çin için bir ICP (Internet İçerik Sağlayıcısı) lisansına sahip bir ağ ve ISS ile birlikte çalışmanız gerekir. Çoğu durumda, aşağıdaki sağlayıcılardan biriyle birlikte olursunuz:

* Çin Telekom Global Ltd
* Çin Mobil Ltd
* Çin Unicom Ltd
* PCCW Global Ltd.
* Hong Kong Telekom Ltd

Sağlayıcıya ve ihtiyaçlarınıza bağlı olarak, çin içindeki şubelerinizi birbirine bağlamak için aşağıdaki ağ bağlantısı hizmetlerinden birini satın almanız gerekir.

* Bir MPLS/IPVPN Ağı 
* Bir Yazılım Tanımlı WAN (SDWAN)
* Özel İnternet Erişimi

Ardından, Pekin veya Şangay'da değil, Microsoft Global Network'e ve Hong Kong'daki Edge Network'e bir ara vermek için bu sağlayıcıyla aynı fikirde olmanız gerekir. Bu durumda, Hong Kong fiziksel bağlantısı ve Çin'e konumu nedeniyle çok önemlidir.

Çoğu müşteri, haritaya bakarken Çin'e daha yakın göründüğü için Singapur'u ara bağlantı için kullanmanın en iyi durum olduğunu düşünse de, bu doğru değildir. Ağ fiber haritaları takip ettiğinizde, hemen hemen tüm ağ bağlantıları Pekin, Şangay ve Hong Kong üzerinden gidin. Bu hong kong Çin'e bağlanmak için daha iyi bir yer seçimi yapar.

Sağlayıcıya bağlı olarak, farklı hizmet teklifleri alabilirsiniz. Aşağıdaki tablo, bu makalenin yazıldığı tarihteki bilgilere dayalı olarak sağlayıcılar ve sundukları hizmetin bir örneğini gösterir.

| Hizmet | Sağlayıcı örnekleri |
| --- | --- |
| MPLS/IPVPN Ağı |PCCW, Çin Telekom Global |
|SDWAN| PCCW, Çin Telekom Global|
| Özel İnternet Erişimi | PCCW, Hong Kong Telekom, Çin Mobil, PCCW |

Sağlayıcınızla, Microsoft'un küresel omurgasına ulaşmak için aşağıdaki iki çözümden hangisinin kullanılacağı konusunda anlaşabilirsiniz:

* Hong Kong'da Microsoft Azure ExpressRoute'u alma sonlandırıldı. Bu MPLS / IPVPN kullanımı için durum olacaktır. Şu anda, Hong Kong ExpressRoute ile sadece ICP lisans sağlayıcısı China Telecom Global olduğunu. Ancak, Megaport veya InterCloud gibi Bulut Değişim Sağlayıcılarından yararlanırlarsa diğer sağlayıcılarla da konuşabilirler. Daha fazla bilgi için [ExpressRoute bağlantı sağlayıcılarına](../expressroute/expressroute-locations-providers.md#partners)bakın.

* Doğrudan aşağıdaki Internet Exchange Noktalarından birinde Özel Bir Internet Erişimi kullanma veya özel bir ağ bağlantısı kullanma.

Aşağıdaki liste, Hong Kong'da mümkün olan Internet Exchanges'i gösterir:

* AMS-IX Hong Kong
* BBIX Hong Kong
* Equinix Hong Kong
* HKIX

Bu connect'i kullanırken, Microsoft Hizmetleri için bir sonraki BGP atlamanız Microsoft Özerk Sistem Numarası (AS#) 8075 olmalıdır. Tek bir konum veya SDWAN çözümü kullanıyorsanız, bu bağlantı seçimi olacaktır.

Her iki şekilde de, biz hala Çin Anakara içine ikinci ve düzenli Internet Breakout var öneririz. Bu, kurumsal trafik ile Microsoft 365 ve Azure gibi bulut hizmetlerine olan trafiği ve yasa yla düzenlenmiş Internet trafiğini bölmek içindir.

Çin içinde uyumlu bir ağ mimarisi aşağıdaki örnek gibi görünebilir:

![Birden çok dal](./media/interconnect-china/multi-branch.png)

Bu örnekte, Hong Kong'daki Microsoft Global Network ile ara bağlantıya sahip olarak, artık hizmetleri tüketmek ve Çin dışındaki şubelerinize ve veri merkezinize bağlanmak için [Azure Sanal WAN Global Transit Mimarisi'nden](virtual-wan-global-transit-network-architecture.md) ve Azure güvenli Sanal WAN hub'ı gibi ek hizmetlerden yararlanmaya başlayabilirsiniz.

## <a name="hub-to-hub-communication"></a><a name="hub-to-hub"></a>Hub'dan hub'a iletişim

Bu bölümde, ara bağlantı için Sanal WAN hub'dan hub'a iletişimi kullanıyoruz. Bu senaryoda, Hong Kong'daki bir Sanal WAN hub'ına, tercih ettiğiniz diğer bölgelere, azure kaynaklarına zaten sahip olduğunuz veya bağlanmak istediğiniz bir bölgeye bağlanmak için yeni bir Virtual WAN hub kaynağı oluşturursunuz.

Örnek mimari aşağıdaki örnek gibi görünebilir:

![Örnek WAN](./media/interconnect-china/sample.png)

Bu örnekte, Çin şubeleri VPN veya MPLS bağlantılarını kullanarak Azure Bulut Çin'e ve birbirlerine bağlanır. Global Hizmetlere bağlanması gereken şubeler, doğrudan Hong Kong'a bağlı MPLS veya Internet tabanlı hizmetleri kullanır. ExpressRoute'u Hong Kong'da ve diğer bölgede kullanmak istiyorsanız, [ExpressRoute Global Reach'i](../expressroute/expressroute-global-reach.md) her iki ExpressRoute Devresi'ni de birbirine bağlamak için yapılandırmanız gerekir.

ExpressRoute Global Reach bazı bölgelerde kullanılamaz. Örneğin, Brezilya veya Hindistan ile ara bağlantı kurmanız gerekiyorsa, yönlendirme hizmetlerini sağlamak için [Bulut Değişimi Sağlayıcılarından](../expressroute/expressroute-locations.md#connectivity-through-exchange-providers) yararlanmanız gerekir.

Aşağıdaki şekil, bu senaryo için her iki örneği de gösterir.

![Küresel Erişim](./media/interconnect-china/global.png)

## <a name="secure-internet-breakout-for-office-365"></a><a name="secure"></a>Office 365 için güvenli Internet koparması

Başka bir husus ağ güvenliği yanı sıra Çin ve Virtual WAN kurulan omurga bileşeni ve müşteri omurgası arasındaki giriş noktası için günlük olduğunu. Çoğu durumda, Doğrudan Microsoft Edge Network'e ve bununla birlikte Microsoft 365 Hizmetleri için kullanılan Azure Ön Kapı Sunucularına ulaşmak için Hong Kong'da Internet'e ara verme ihtiyacı vardır.

Virtual WAN ile her iki senaryoda da [Azure Virtual WAN güvenli hub'ından](../firewall-manager/secured-virtual-hub.md)yararlanırsınız. Azure Güvenlik Duvarı Yöneticisi'ni kullanarak, normal bir Sanal WAN hub'ı güvenli bir hub olarak değiştirebilir ve ardından bu hub içinde bir Azure Güvenlik Duvarı dağıtAbilir ve yönetebilirsiniz.

Aşağıdaki şekil bu senaryonun bir örneğini gösterir:

![Web ve Microsoft hizmetleri trafiği için Internet koparma](./media/interconnect-china/internet.png)

## <a name="architecture-and-traffic-flows"></a><a name="traffic"></a>Mimari ve trafik akışları

Hong Kong bağlantısı ile ilgili seçtiğiniz bağlı olarak, genel mimari biraz değişebilir. Bu bölümde VPN veya SDWAN ve/veya ExpressRoute ile farklı kombinasyonlarda üç kullanılabilir mimari gösterilmektedir.

Tüm bu seçenekler, Hong Kong'daki doğrudan M365 bağlantısı için Azure Virtual WAN güvenli hub'ını kullanır. Bu mimariler ayrıca Office [365 Multi-Geo](https://docs.microsoft.com/office365/enterprise/office-365-multi-geo) için uyumluluk gereksinimlerini destekler ve bu trafiği sonraki Office 365 Ön Kapı konumuna yakın tutar. Sonuç olarak, Aynı zamanda Microsoft 365 Çin dışında kullanımı için bir gelişmedir.

Azure Virtual WAN'ı Internet bağlantılarıyla birlikte kullanırken, her bağlantı [Microsoft Azure Peering Services (MAPS)](https://docs.microsoft.com/azure/peering-service/about)gibi ek hizmetlerden yararlanabilir. MAPS, Microsoft Global Network'e gelen trafiği 3.

### <a name="option-1-sdwan-or-vpn"></a><a name="option-1"></a>Seçenek 1: SDWAN veya VPN

Bu bölümde, Hong Kong ve diğer şubelere SDWAN veya VPN kullanan bir tasarım anlatılmaktadır. Bu seçenek, Virtual WAN omurgasının her iki sitesinde de saf Internet bağlantısı kullanırken kullanımı ve trafik akışını gösterir. Bu durumda, bağlantı özel Internet erişimi veya bir ICP sağlayıcısı SDWAN çözümü kullanılarak Hong Kong'a getirilir. Diğer dallar da saf Internet veya SDWAN Çözümleri kullanıyor.

![Çin'den Hong Kong trafiğine](./media/interconnect-china/china-traffic.png)

Bu mimaride, her site VPN ve Azure Virtual WAN kullanılarak Microsoft Global Network'e bağlanır. Siteler ve Hong Kong arasındaki trafik Microsoft Ağı'na aktarılır ve yalnızca son milde düzenli Internet bağlantısı kullanır.

### <a name="option-2-expressroute-and-sdwan-or-vpn"></a><a name="option-2"></a>Seçenek 2: ExpressRoute ve SDWAN veya VPN

Bu bölümde, Hong Kong'daki ExpressRoute ve VPN/SDWAN Şubeleri olan diğer Şubeler kullanan bir tasarım açıkılmaktadır. Bu seçenek, Hong Kong'da ve SDWAN veya VPN üzerinden bağlanan diğer şubelerde ve ExpressRoute'un kullanımını ve ExpressRoute'un kullanımını gösterir. Hong Kong'daki ExpressRoute şu anda [Express Route Partners](../expressroute/expressroute-locations-providers.md#global-commercial-azure)listesinde bulabileceğiniz kısa bir Sağlayıcı listesiyle sınırlıdır.

![Çin Hong Kong trafik ExpressRoute için](./media/interconnect-china/expressroute.png)

Örneğin Güney Kore veya Japonya'da Çin'den ExpressRoute'u sonlandırma seçenekleri de vardır. Ancak, uyumluluk, düzenleme ve gecikme göz önüne alındığında, Hong Kong şu anda en iyi seçimdir.

### <a name="option-3-expressroute-only"></a><a name="option-3"></a>Seçenek 3: Yalnızca ExpressRoute

Bu bölümde ExpressRoute'un Hong Kong ve diğer Şubeler için kullanıldığı bir tasarım açıklanır. Bu seçenek, her iki uçta expressroute kullanarak ara bağlantı gösterir. Burada diğerinden farklı bir trafik akışı var. Microsoft 365 trafiği Azure sanal WAN güvenli hub'ına ve oradan da Microsoft Edge Network ve Internet'e akacaktır.

Birbirine bağlı şubelere ya da onlardan Çin'deki yerlere giden trafik, bu mimari içinde farklı bir yaklaşım izleyecek. Şu anda sanal WAN ExpressRoute ExpressRoute transit desteklemiyor. Trafik, Virtual WAN Hub'ı geçmeden ExpressRoute Global Reach veya 3. Doğrudan bir Microsoft Enterprise Edge (MSEE) diğerine akar.

![ExpressRoute Global Reach hakkında](./media/interconnect-china/expressroute-virtual.png)

Şu anda ExpressRoute Global Reach her ülkede kullanılamıyor, ancak Azure Virtual WAN kullanarak bir çözüm yapılandırabilirsiniz.

Örneğin, Microsoft Peering ile bir ExpressRoute yapılandırabilir ve Azure Virtual WAN'a bu bakışla bir VPN tüneli bağlayabilirsiniz. Şimdi, yine, Global Reach ve Megaport Cloud gibi 3.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Virtual WAN ile Global Transit ağ mimarisi](virtual-wan-global-transit-network-architecture.md)

* [Sanal WAN hub'ı oluşturma](virtual-wan-site-to-site-portal.md)

* [Sanal WAN güvenli hub'ı yapılandırma](../firewall-manager/secure-cloud-network.md)

* [Azure Bakma Hizmeti Önizleme Genel Bakış](https://docs.microsoft.com/azure/peering-service/about)
