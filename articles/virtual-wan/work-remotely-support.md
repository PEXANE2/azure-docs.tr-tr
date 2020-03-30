---
title: Azure Sanal WAN ve uzaktan çalışma
description: Bu sayfa, COVID-19 salgını nedeniyle uzaktan çalışmayı etkinleştirmek için Azure Virtual WAN'dan nasıl yararlanabileceğinizi açıklar.
services: virtual-wan
author: reyandap
ms.service: virtual-wan
ms.topic: article
ms.date: 03/22/2020
ms.author: cherylmc
ms.openlocfilehash: ce212b5da90906966025674b58884d0e2f5bb064
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80337136"
---
# <a name="azure-virtual-wan-and-supporting-remote-work"></a>Azure Sanal WAN ve uzaktan çalışmayı destekleme

>[!NOTE]
>Bu makalede, COVID-19 krizi nedeniyle karşılaştığınız ağ sorunlarını uzaktan çalışmak ve ağ sorunlarını azaltmak için Azure Virtual WAN, Azure, Microsoft ağı ve Azure iş ortağı ekosisteminden nasıl yararlanabileceğiniz açıklanmaktadır.
>

Uzak kullanıcılar için bağlantı sağlamak için çabalıyor musunuz?
Aniden, planladığınızın ötesinde bir kullanıcı artışını destekleme ihtiyacı görüyor musunuz?
Kullanıcının evden bağlanmasıve yalnızca buluta erişmekle kalmaması ve şirket içinde erişebilmesi için kullanıcıya ihtiyacınız var mı?
Özel bir WAN'ın arkasındaki kaynaklara ulaşabilmek için uzak kullanıcılarınızın mı gerekir?
Kullanıcıların bölgeler arası bağlantı kurma sına gerek kalmadan bulut içi kaynaklara erişme ihtiyacı var mı?
Bu küresel salgın çevremizdeki eşi görülmemiş değişiklikler yaratırken, Azure Virtual WAN ekibi bağlantı ihtiyaçlarınızı karşılamanız için buradadır.

Azure Virtual WAN, tek bir operasyonel arabirim sağlamak için birçok ağ, güvenlik ve yönlendirme işlevlerini bir araya getiren bir ağ hizmetidir. Bu işlevler şube bağlantısını (SD-WAN veya VPN CPE gibi Sanal WAN İş Ortağı aygıtlarından bağlantı otomasyonu yoluyla), Siteden siteye VPN bağlantısı, Uzaktan Kullanıcı VPN (noktaya noktadan siteye) bağlantısını, Özel (ExpressRoute) bağlantısını, İç bulut bağlantısı (Sanal Ağlar için geçişli bağlantı), VPN ExpressRoute Ara bağlantı, Yönlendirme, Azure güvenlik duvarı, özel bağlantı için şifreleme vb. Virtual WAN kullanmaya başlamak için bu kullanım örneklerinin tümüne sahip olmak zorunda değilsiniz. Tek bir kullanım örneğinden başlayıp ağınızı geliştikçe ayarlayabilirsiniz.

![Sanal WAN diyagramı](./media/virtual-wan-about/virtualwan1.png)

Şimdi uzak kullanıcılardan bahsederek, ağınızı çalışır hale getirmek için neye ihtiyacınız olduğuna bakalım:

## <a name="set-up-remote-user-connectivity"></a><a name="connectivity"></a>Uzak kullanıcı bağlantısını ayarlama

IPsec/IKE (IKEv2) veya OpenVPN bağlantısı üzerinden Azure'daki kaynaklarınıza bağlanabilirsiniz. Bu tür bir bağlantı, uzak kullanıcı için yapılandırılmak üzere bir VPN istemcisi gerektirir. Bu istemci [Azure VPN istemcisi](https://go.microsoft.com/fwlink/?linkid=2117554) veya OpenVPN Istemcisi veya IKEv2'yi destekleyen herhangi bir istemci olabilir. Daha fazla bilgi için [bkz.](virtual-wan-point-to-site-portal.md)

## <a name="connectivity-from-the-remote-user-to-on-premises"></a><a name="remote user connectivity"></a>Uzak kullanıcıdan şirket içi bağlantı

Burada iki seçeneğiniz var:

* Varolan herhangi bir VPN aygıtıyla siteden siteye bağlantı kurun. IPsec VPN aygıtını Azure Virtual WAN hub'ına bağladığınızda, Siteden siteye Kullanıcı VPN 'i (Uzak kullanıcı) ile Siteden siteye VPN arasındaki bağlantı otomatiktir. Şirket içi VPN aygıtınızdan Azure Virtual WAN'a siteden siteye VPN'i nasıl ayarlayabilirsiniz hakkında daha fazla bilgi için Virtual [WAN kullanarak siteden siteye bağlantı oluşturma](virtual-wan-site-to-site-portal.md)'ya bakın.

* ExpressRoute devrenizi Sanal WAN hub'ına bağlayın. Bir ExpressRoute devresi bağlamak için Virtual WAN'da bir ExpressRoute ağ geçidi nin dağıtılması nı gerektirir. Bir tane sini dağıttığınızda, Noktaya Kullanıcı VPN ve ExpressRoute kullanıcısı arasındaki bağlantı otomatiktir. ExpressRoute bağlantısını oluşturmak için [bkz.](virtual-wan-expressroute-portal.md) Azure Virtual WAN'a bağlanmak için varolan bir ExpressRoute devresini kullanabilirsiniz.

## <a name="existing-basic-virtual-wan-customer"></a><a name="basic vWAN"></a>Mevcut temel Sanal WAN müşterisi

Temel Sanal WAN yalnızca Siteden siteye VPN sağlar. Uzak kullanıcıların bağlanabilmesi için sanal WAN'ı Standart Sanal WAN'a yükseltmeniz gerekir. Sanal bir WAN'ı yükseltme adımları için [bkz.](upgrade-virtual-wan.md)

## <a name="additional-information"></a><a name="other considerations"></a>Ek bilgiler

Virtual WAN bölge/konum başına bir hub'ı destekler. Konum bilgileri için [Sanal WAN iş ortakları ve konumları](virtual-wan-locations-partners.md) makalesine bakın. Her hub 10.000'e kadar uzak kullanıcı bağlantısını, 1.000 şube bağlantısını, dört ExpressRoute devresini ve 500'e kadar Sanal Ağ bağlantısını destekler. Uzak kullanıcıları büyütterken, herhangi bir sorunuz varsa, e-posta göndererek azurevirtualwan@microsoft.comyardım istemekten çekinmeyin. Teknik desteğe ihtiyacınız varsa, Azure portalından bir destek bileti açtığınızdan emin olun ve yardım yolda olacaktır.

## <a name="faq"></a><a name="faq"></a>SSS

[!INCLUDE [Virtual WAN FAQ](../../includes/virtual-wan-faq-include.md)]

## <a name="next-steps"></a>Sonraki Adımlar

Virtual WAN hakkında daha fazla bilgi için [Virtual WAN genel bakış](virtual-wan-about.md)