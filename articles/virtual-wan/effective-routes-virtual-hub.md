---
title: "Sanal hub 'ın geçerli yollarını görüntüleme: Azure sanal WAN | Microsoft Docs"
description: Azure sanal WAN 'da bir sanal hub için geçerli yolları görüntüle
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 06/02/2020
ms.author: cherylmc
ms.openlocfilehash: c7d436f2aecb021a7848ef0455a3f1c834cc38c1
ms.sourcegitcommit: 4ac596f284a239a9b3d8ed42f89ed546290f4128
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84750542"
---
# <a name="view-effective-routes-of-a-virtual-hub"></a>Bir sanal hub 'ın geçerli yollarını görüntüleme

Sanal WAN hub 'ınızın tüm yollarını Azure portal görüntüleyebilirsiniz. Rotaları görüntülemek için sanal hub 'a gidin ve ardından **yönlendirme-> geçerli yolları görüntüle**' yi seçin.

## <a name="understanding-routes"></a><a name="understand"></a>Rotaları anlama

Aşağıdaki örnek sanal WAN yönlendirmesinin nasıl göründüğünü daha iyi anlamanıza yardımcı olabilir.

Bu örnekte, üç hub ile sanal bir WAN sunuyoruz. İlk hub Doğu ABD bölgedeyse, ikinci hub Batı Avrupa bölgedeyse ve üçüncü hub Batı ABD bölgesidir. Sanal bir WAN 'da tüm Hub 'lar birbirine bağlı. Bu örnekte, Doğu ABD ve Batı Avrupa hub 'larının şirket içi dallardan (bağlı bileşenler) ve Azure sanal ağlarının (bağlı bileşenler) bağlantılarının olduğunu varsayacağız.

Ağ sanal gereci (10.4.0.6) ile bir Azure VNet bağlı alanı (10.4.0.0/16) VNet 'e daha fazla eşlenmez (10.5.0.0/16). Merkez yol tablosu hakkında daha fazla bilgi için bu makalenin ilerleyen kısımlarında bulunan [ek bilgilere](#abouthubroute) bakın.

Bu örnekte, Batı Avrupa dalı 1 ' in Doğu ABD hub 'a ve Batı Avrupa hub 'ına bağlı olduğunu da varsaydık. Doğu ABD bir ExpressRoute bağlantı hattı, dalı 2 ' yi Doğu ABD hub 'ına bağlar.

![şema](./media/effective-routes-virtual-hub/diagram.png)

## <a name="view-effective-routes"></a><a name="view"></a>Geçerli yolları görüntüle

Portalda ' etkin yolları görüntüle ' seçeneğini belirlediğinizde, Doğu ABD hub ['ına ait merkez yolu tablosunda](#routetable) gösterilen çıktıyı üretir.

Bunu perspektife koymak için, ilk satır Doğu ABD hub 'ın, VPN *sonraki atlama türü* bağlantısı (' sonraki atlama ' VPN Gateway örnek0 IP 10.1.0.6, Instance1 IP 10.1.0.7) nedeniyle 10.20.1.0/24 (dal 1) yolunu öğrenmiş olduğunu gösterir. *Yol* kaynağı kaynak kimliğine işaret eder. *As yolu* , Branch 1 Için as yolunu gösterir.

### <a name="hub-route-table"></a><a name="routetable"></a>Merkez yol tablosu

"Yol olarak" görüntülemek için tablonun altındaki kaydırma çubuğunu kullanın.

| **Ön ek** |  **Sonraki atlama türü** | **Sonraki atlama** |  **Yol kaynağı** |**AS yolu** |
| ---        | ---                | ---          | ---               | ---         |
| 10.20.1.0/24|VPN |10.1.0.6, 10.1.0.7| /Subscriptions/ `<sub>` /ResourceGroups/ `<rg>` /providers/Microsoft.Network/vpnGateways/343a19aa6ac74e4d81f05ccccf1536cf-eastus-GW| 20000|
|10.21.1.0/24 |ExpressRoute|10.1.0.10, 10.1.0.11|/Subscriptions/ `<sub>` /ResourceGroups/ `<rg>` /providers/Microsoft.Network/expressRouteGateways/4444a6ac74e4d85555-eastus-GW|21000|
|10.23.1.0/24| VPN |10.1.0.6, 10.1.0.7|/Subscriptions/ `<sub>` /ResourceGroups/ `<rg>` /providers/Microsoft.Network/vpnGateways/343a19aa6ac74e4d81f05ccccf1536cf-eastus-GW|23000|
|10.4.0.0/16|Sanal ağ bağlantısı| Bağlantı üzerine |  |  |
|10.5.0.0/16| IP Adresi| 10.4.0.6|/Subscriptions/ `<sub>` /ResourceGroups/ `<rg>` /providers/Microsoft.Network/virtualhubs/easthub_1/routeTables/table_1| |
|0.0.0.0/0| IP Adresi| `<Azure Firewall IP>` |/Subscriptions/ `<sub>` /ResourceGroups/ `<rg>` /providers/Microsoft.Network/virtualhubs/easthub_1/routeTables/table_1| |
|10.22.1.0/16| Uzak Merkez|10.8.0.6, 10.8.0.7|/Subscriptions/ `<sub>` /ResourceGroups/ `<rg>` /providers/Microsoft.Network/virtualhubs/westhub_| 4848-22000 |
|10.9.0.0/16| Uzak Merkez|  Bağlantı üzerine |/Subscriptions/ `<sub>` /ResourceGroups/ `<rg>` /providers/Microsoft.Network/virtualhubs/westhub_1| |

>[!NOTE]
> Doğu ABD ve Batı Avrupa hub 'ları örnek topolojide birbirleriyle iletişim kurmadığında, öğrenilen yol (10.9.0.0/16) mevcut olmaz. Hub 'lar yalnızca doğrudan kendisine bağlı olan ağları duyurur.
>

## <a name="additional-information"></a><a name="additional"></a>Ek bilgi

### <a name="about-the-hub-route-table"></a><a name="abouthubroute"></a>Merkez yol tablosu hakkında

Sanal hub yolu oluşturabilir ve yolu sanal hub yolu tablosuna uygulayabilirsiniz. Sanal hub rota tablosuna birden fazla rota uygulayabilirsiniz. Bu, hedef VNet için bir IP adresi (genellikle bir bağlı olan VNet 'teki ağ sanal gereci (NVA)) yoluyla bir yol ayarlamanıza olanak sağlar. NVA 'lar hakkında daha fazla bilgi için bkz. [bir sanal hub 'dan NVA 'ya trafiği yönlendirme](virtual-wan-route-table-portal.md). Bu yolların etkin yol tablosunda gösterilmeyeceğini lütfen unutmayın. Etkin yol tablosu, yalnızca yerel ve uzak hub 'ların ve bağlı sanal ağ adres alanının ve BGP aracılığıyla öğrenilen yolların öneklerini içerir.

### <a name="about-default-route-00000"></a><a name="aboutdefaultroute"></a>Varsayılan yol hakkında (0.0.0.0/0)

Bir sanal hub, bağlantıda ' Enabled ' olduğunda, bir sanal ağa, siteden siteye VPN 'e ve bir ExpressRoute bağlantısına, öğrenilen bir varsayılan yolu yaymaya olanak tanır. Bu bayrak, bir sanal ağ bağlantısını, bir VPN bağlantısını veya bir ExpressRoute bağlantısını düzenlediğinizde görülebilir. Hub VNet, ExpressRoute ve VPN bağlantılarında varsayılan olarak ' Enableınternetsecurity ' her zaman false 'tur.

Varsayılan yol, sanal WAN hub 'ında değil. Hub 'da bir güvenlik duvarı dağıtımının bir sonucu olarak veya başka bir bağlı sitede Zorlamalı tünel etkinse, varsayılan yol sanal WAN hub tarafından zaten öğrenilmiş ise yayılır.

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için bkz. [Sanal WAN'a Genel Bakış](virtual-wan-about.md).
