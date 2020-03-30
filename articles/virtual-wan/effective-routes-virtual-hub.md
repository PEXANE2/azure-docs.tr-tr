---
title: "Sanal hub'ın etkili rotalarını görüntüleyin: Azure Virtual WAN | Microsoft Dokümanlar"
description: Azure Virtual WAN'da sanal bir hub için etkili rotalar
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 10/18/2019
ms.author: cherylmc
ms.openlocfilehash: 1173da81736661048d1e4e12d9919bc2aadf73ee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73515856"
---
# <a name="view-effective-routes-of-a-virtual-hub"></a>Sanal hub'ın etkili rotalarını görüntüleme

Sanal WAN hub'ınızın tüm rotalarını Azure portalında görüntüleyebilirsiniz. Rotaları görüntülemek için sanal hub'a gidin ve ardından **Yönlendirme -> Etkili Rotaları**Görüntüleyin'i seçin.

## <a name="understanding-routes"></a><a name="understand"></a>Yolları anlama

Aşağıdaki örnek, Sanal WAN yönlendirmesinin nasıl göründüğünü daha iyi anlamanıza yardımcı olabilir.

Bu örnekte, üç hub'ı olan sanal bir WAN'imiz vardır. İlk merkez Doğu ABD bölgesinde, ikinci merkez Batı Avrupa bölgesinde, üçüncü merkez ise Batı ABD bölgesindedir. Sanal BIR WAN'da tüm hub'lar birbirine bağlıdır. Bu örnekte, Doğu ABD ve Batı Avrupa hub'larının şirket içi şubelerden (spokes) ve Azure sanal ağlarından (sözcüler) bağlantıları olduğunu varsayacağız.

Bir Ağ Sanal Cihazı (10.4.0.0.6) içeren bir Azure VNet konuşması (10.4.0.0/16) vnet'e (10.5.0.0/16) daha fazla bakar. Hub rota tablosu hakkında daha fazla bilgi için bu makalede daha sonra [ek bilgilere](#abouthubroute) bakın.

Bu örnekte, Batı Avrupa Şube 1'in indoğu ABD merkezinin yanı sıra Batı Avrupa merkezine de bağlı olduğunu varsayıyoruz. Doğu ABD'deki bir ExpressRoute devresi, Şube 2'yi Doğu ABD merkezine bağlar.

![şema](./media/effective-routes-virtual-hub/diagram.png)

## <a name="view-effective-routes"></a><a name="view"></a>Etkili rotaları görüntüleme

Portalda 'Etkili Rotaları Görüntüle'yi seçtiğinizde, Doğu ABD Hub hub'ı için [Hub rota tablosunda](#routetable) gösterilen çıktıyı üretir.

Bunu bir perspektife koymak için, ilk satır Doğu ABD hub VPN *Sonraki atlama türü* bağlantısı ('Sonraki hop' VPN Gateway Instance0 IP 10.1.0.6, Instance1 IP 10.1.0.7) nedeniyle 10.20.1.0/24 (Şube 1) rota öğrendim anlamına gelir. *Rota Kaynağı* kaynak kimliğine işaret ediyor. *AS Yolu,* Şube 1 için AS Yolunu gösterir.

### <a name="hub-route-table"></a><a name="routetable"></a>Hub rota tablosu

"AS Yolu"nu görüntülemek için tablonun altındaki kaydırma çubuğunu kullanın.

| **Ön ek** |  **Sonraki atlama türü** | **Sonraki atlama** |  **Rota Kökeni** |**AS Yolu** |
| ---        | ---                | ---          | ---               | ---         |
| 10.20.1.0/24|VPN |10.1.0.6, 10.1.0.7| /subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/vpnGateways/343a19aa6ac74e4d81f05ccccf1536cf-eastus-gw| 20000|
|10.21.1.0/24 |ExpressRoute|10.1.0.10, 10.1.0.11|/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/expressRouteGateways/4444a6ac74e4d85555-eastus-gw|21000|
|10.23.1.0/24| VPN |10.1.0.6, 10.1.0.7|/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/vpnGateways/343a19aa6ac74e4d81f05ccccf1536cf-eastus-gw|23000|
|10.4.0.0/16|Sanal Ağ Bağlantısı| Bağlantı üzerine |  |  |
|10.5.0.0/16| IP Adresi| 10.4.0.6|/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/virtualHubs/easthub_1/routeTables/table_1| |
|0.0.0.0/0| IP Adresi| `<Azure Firewall IP>` |/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/virtualHubs/easthub_1/routeTables/table_1| |
|10.22.1.0/16| Uzak Hub|10.8.0.6, 10.8.0.7|/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/virtualHubs/westhub_| 4848-22000 |
|10.9.0.0/16| Uzak Hub|  Bağlantı üzerine |/subscriptions/`<sub>`/resourceGroups/`<rg>`/providers/Microsoft.Network/virtualHubs/westhub_1| |

>[!NOTE]
> Örneğin topolojide Doğu ABD ve Batı Avrupa merkezleri birbirleriyle iletişim kurmasa, öğrenilen rota (10.9.0.0/16) olmazdı. Hub'lar yalnızca doğrudan kendilerine bağlı ağların reklamını yapıyor.
>

## <a name="additional-information"></a><a name="additional"></a>Ek bilgiler

### <a name="about-the-hub-route-table"></a><a name="abouthubroute"></a>Hub rota tablosu hakkında

Sanal hub rotası oluşturabilir ve rotayı sanal hub rota tablosuna uygulayabilirsiniz. Sanal hub rota tablosuna birden fazla rota uygulayabilirsiniz. Bu, bir IP adresi (genellikle bir vnet'teki Network Virtual Appliance (NVA) üzerinden hedef VNet için bir rota belirlemenize olanak tanır. NV'ler hakkında daha fazla bilgi [için, sanal hub'dan NVA'ya giden Rota trafiğine](virtual-wan-route-table-portal.md)bakın.

### <a name="about-default-route-00000"></a><a name="aboutdefaultroute"></a>Varsayılan rota hakkında (0.0.0.0/0)

Sanal hub, öğrenilen varsayılan bir rotayı sanal ağa, siteden siteye VPN'e ve bayrak bağlantıda 'Etkinleştirilmiş' sa ExpressRoute bağlantısına yayma yeteneğine sahiptir. Sanal ağ bağlantısını, VPN bağlantısını veya ExpressRoute bağlantısını siz de edindiğinizde bu bayrak görünür. Hub VNet, ExpressRoute ve VPN bağlantılarında varsayılan olarak 'InternetSecurity'yi etkinleştirme' her zaman yanlıştır.

Varsayılan rota sanal WAN hub'ında kaynaklanmaz. Hub'a bir güvenlik duvarı nın dağıtılması sonucunda sanal WAN hub'ı tarafından zaten öğrenilmişse veya bağlı başka bir site tünel imal etmeyi zorlamışsa varsayılan rota yayılır.

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için bkz. [Sanal WAN'a Genel Bakış](virtual-wan-about.md).