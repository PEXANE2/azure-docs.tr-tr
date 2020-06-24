---
title: Birden çok ISS bağlantısı üzerinde Azure yol seçimi
titleSuffix: Azure Virtual WAN
description: Azure yol seçimi ve sanal WAN hakkında bilgi edinin
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 05/28/2020
ms.author: cherylmc
ms.openlocfilehash: a83a050b65f5673a86dd07ec13842f6009d2c8dc
ms.sourcegitcommit: 4ac596f284a239a9b3d8ed42f89ed546290f4128
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84753349"
---
# <a name="azure-path-selection-across-multiple-isp-links"></a>Birden çok ISS bağlantısı üzerinde Azure yol seçimi

Azure sanal WAN, bir kullanıcıya bir VPN sitesine bağlantı bilgilerini ekleme özelliği sunar. VPN/SD-WAN cihaz çözümünün, çeşitli bağlantılar genelinde trafiği Azure 'a dönüştürmek için dala özgü ilkeleri programleyebildiği senaryolara olanak tanır. Bu, **Azure yol seçimi**olarak adlandırılır.

## <a name="architecture"></a>Mimari

Azure yol seçiminin nasıl çalıştığını anlamak için, bir sanal WAN VPN sitesi ve siteden siteye bağlantı örneğini kullanalım.

VPN sitesi, şirket içi SD-WAN/VPN cihazını genel IP, cihaz modeli ve ad gibi bilgilerle temsil eder. Gerçek şirket içi VPN sitesi, sanal WAN VPN site bilgilerine de dahil edilebilir birden çok ISS bağlantısına sahip olabilir. Bu, Azure 'daki bağlantı bilgilerini görüntülemenize olanak sağlar.

Bir sanal WAN 'ın VPN 'e gelen siteden siteye IPSec bağlantısı, sanal bir hub içindeki VPN Gateway örneklerinde sonlanır. Siteden siteye bağlantı VPN sitesi ile Azure VPN ağ geçidi arasındaki bağlantıyı temsil eder. Bir veya daha fazla bağlantı bağlantısından oluşur. Her bağlantı bağlantısı, her tünele Azure sanal WAN VPN Gateway 'in benzersiz bir örneğinde sonlanarak iki tünelden oluşur. Siteden siteye bağlantı için en fazla dört bağlantı bağlantısı kurulabilir. Bu, siteden siteye bağlantı içinde sekiz tünelye sahip olmanızı olanaklı kılar. Azure, tek bir sanal WAN VPN ağ geçidinin içinde sonlandırıp en fazla 2000 tüneli destekler.

:::image type="content" source="./media/path-selection-multiple-links/multi-link-site.png" alt-text="Çoklu bağlantı diyagramı":::

Bu şekilde, Azure sanal WAN 'a bağlanan bir sitede birden çok bağlantı gösterilmektedir. Bu diyagramda:

* Şirket içi dalda (VPN/SD-WAN cihazı) iki ISS bağlantısı vardır. Her ISP bağlantısı bağlantı bağlantısına karşılık gelir.

* Şirket içi müşteri-yönetici VPN/SD-WAN cihazının IKEv1 veya Ikev2 IPSec 'i desteklediğini kabul edilir.

* Her bir Azure siteden siteye sanal WAN bağlantısı, kendi içindeki bağlantı bağlantılarından oluşur. Bir bağlantı en fazla dört bağlantı bağlantısını destekler. Azure, sanal WAN bağlantısı için bir bağlantı birimi ücreti ücretlendirir. Bağlantı bağlantıları için ücret alınmaz.

* Her bağlantı bağlantısı, sanal WAN VPN ağ geçidinin iki farklı örneğinde sonlanabilir iki IPsec tünelinden oluşur. Ağ geçitleri, dayanıklılık için etkin-etkin ağ geçitleri olarak ayarlanır. Her bağlantı bağlantısının benzersiz bir IP adresi ve BGP eşleme IP 'si olması gerekir. Diyagramda tünel 0, örnek 0 ' da sonlandırılabilir ve tünel 1 örnek 1 ' de sonlanabilir.

* Yol seçimi sağlayan dal cihazları, Azure 'a yönelik birden çok bağlantı üzerinde trafik sağlamak için dal yönetimi çözümünde uygun ilkeyi etkinleştirebilir. Örneğin, ISS 1 bağlantısı daha yüksek öncelikli trafik için kullanılabilir ve ISS 2 bağlantısı yedekleme olarak kullanılabilir.

* Sanal HUB VPN 'nin tüm sonlandırma tünellerinin genelinde ECMP (eşit maliyetli çoklu yol yönlendirmesi) kullandığını unutmamak önemlidir.

## <a name="next-steps"></a>Sonraki adımlar

Bkz. [Azure SSS](virtual-wan-faq.md).