---
title: Azure Izleyici öngörülerini kullanarak Azure sanal WAN 'ı izleme
description: Azure Izleyici öngörülerini kullanarak sanal WAN izleme hakkında bilgi edinin
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 07/22/2020
ms.author: cherylmc
ms.openlocfilehash: 8553a809173d955a21e6730de35c70de5b69e81b
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87136352"
---
# <a name="azure-monitor-insights-for-virtual-wan-preview"></a>Sanal WAN için Azure Izleyici öngörüleri (Önizleme)

Sanal WAN için [Azure Izleyici öngörüleri](../azure-monitor/insights/network-insights-overview.md) , kullanıcılara ve operatörlere, bir oto keşfedilen topik Haritası aracılığıyla sunulan sanal WAN 'ın durumunu ve durumunu görüntüleme olanağı sağlar. Kaynak durumu ve durumu, sanal WAN 'ın genel durumunun anlık görüntüsünü sağlamak için haritada yer alır. Kaynak gezintisi, sanal WAN portalının kaynak yapılandırma sayfalarına tek bir tıklama erişimi aracılığıyla haritada etkinleştirilir.

Sanal WAN kaynak düzeyi ölçümleri toplanır ve sanal WAN, hub, ağ geçidi ve bağlantı düzeylerindeki ölçümleri gösteren, önceden paketlenmiş bir sanal WAN ölçümleri çalışma kitabı aracılığıyla sunulur. Bu makale, sanal WAN topolojinizi ve ölçümlerini tek bir yerde görüntülemek için sanal WAN için Azure Izleyici öngörülerini kullanma adımlarında size yol gösterir.

> [!NOTE]
> Öngörüler menüsü, bu önizlemede kullanıma sunulmaya yönelik bir işlemdir.
>

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımlarda, bir veya daha fazla hub ile sanal bir WAN dağıtmış olduğunuz varsayılır. Yeni bir sanal WAN ve yeni bir hub oluşturmak için aşağıdaki makalelerdeki adımları kullanın:

* [Sanal WAN oluşturma](virtual-wan-site-to-site-portal.md#openvwan)
* [Hub oluşturma](virtual-wan-site-to-site-portal.md#hub)

## <a name="view-vwan-topology"></a><a name="topology"></a>VWAN topolojisini görüntüle

**Azure Portal->sanal WAN**'da, sol taraftaki **izleyici** menüsünden Öngörüler ' i **(Önizleme)** seçin. Bu, sanal WAN bağımlılığı eşlemesini ve üst düzey ölçüm mini çalışma kitabını görüntüleyen **Öngörüler görünümünü**getirir.

**Şekil 1: Izleme-Öngörüler menüsü**

:::image type="content" source="./media/azure-monitor-insights/monitor-menu.png" alt-text="rakam" lightbox="./media/azure-monitor-insights/monitor-menu.png":::

**Öngörüler** görünümünde, **Şekil 2**' de gösterildiği gibi, hub 'lar, ağ geçitleri, güvenlik duvarları, bağlantılar ve bağlı bileşen VNET 'leri, 3. taraf NVA 'lar ve dallar gibi farklı sanal WAN kaynaklarını BIR uçtan uca sanal WAN 'da görüntüleyebilirsiniz.

**Kaynak durumu** ve **durumu** renk kodludur ve haritadaki kaynak simgelerinde yer alır. Hub kapasiteleri ve ağ geçidi kullanımı gibi sanal WAN üst düzey ölçümleri, bir mini çalışma kitabı aracılığıyla sağda görüntülenir.

**Şekil 2: Öngörüler görünümü**

:::image type="content" source="./media/azure-monitor-insights/insights-view.png" alt-text="rakam" lightbox="./media/azure-monitor-insights/insights-view.png":::

## <a name="dependency-view"></a><a name="dependency"></a>Bağımlılık görünümü

Sanal WAN için **bağımlılık** görünümü, bir hub ve bağlı bileşen mimarisine göre büyük ölçüde düzenlenmiş tüm sanal WAN kaynaklarının birbirine bağlı görünümünü görselleştirmenize yardımcı olur.

**Şekil 3: VWAN bağımlılığı görünümü**

:::image type="content" source="./media/azure-monitor-insights/dependency-map.png" alt-text="Bağımlılık eşlemesi" lightbox="./media/azure-monitor-insights/dependency-map.png":::

Bağımlılık görünümü eşlemesi aşağıdaki kaynakları bağlı bir grafik olarak görüntüler:

* Çeşitli Azure bölgeleri genelinde sanal WAN hub 'ları.
* Hub 'a doğrudan bağlı olan bağlı olan sanal ağlar.
* VPN ve ExpressRoute şube siteleri ve P2S kullanıcıları ilgili ExpressRoute, S2S ve P2S bağlantıları ve sanal ağ geçitleri aracılığıyla her bir hub 'a bağlanır.
* Bir hub 'da dağıtılan (üçüncü taraf güvenlik duvarı proxy 'leri dahil) Azure Güvenlik duvarları (güvenli hub).
* bağlı bir sanal ağa dağıtılan üçüncü taraf NVA (ağ sanal cihazları).

Bağımlılık eşlemesi Ayrıca, dolaylı olarak bağlı VNET 'leri (sanal WAN ana ağı VNET 'leri ile eşlenmiş VNet) görüntüler.

Bağımlılık eşlemesi, her bir kaynağın yapılandırma ayarlarına kolay gezinme sağlar. Örneğin, Merkez Bölgesi ve Merkez ön eki gibi temel kaynak yapılandırmasını görüntülemek için hub kaynağının üzerine gelebilmeniz gerekir. Hub kaynağının Azure portal sayfasına erişmek için sağ tıklayın.

**Şekil 4: kaynağa özgü bilgilere gitme**

:::image type="content" source="./media/azure-monitor-insights/resource-information.png" alt-text="Kaynak bilgileri":::

Bağımlılık görünümündeki arama ve filtre çubuğu, grafikte arama yapmanın kolay bir yolunu sağlar. Farklı filtreler, aramanızı belirli bir yola ve duruma göre daraltmak için yardım sağlar.

**Şekil 5: arama ve filtreleme**

:::image type="content" source="./media/azure-monitor-insights/search-filter.png" alt-text="Arama ve filtre çubuğu" lightbox="./media/azure-monitor-insights/search-filter.png":::

## <a name="detailed-metrics"></a><a name="detailed"></a>Ayrıntılı ölçümler

Ayrıntılı **ölçümler** sayfasına erişmek için **ayrıntılı ölçümleri görüntüle** ' yi seçebilirsiniz. Ölçümler sayfası, sanal WAN kaynak kapasiteniz, performansı ve kullanımı için sanal WAN düzeyinde, hub düzeyinde ve tek bağlantılardan yararlı Öngörüler sağlayan ayrı sekmelerle önceden yapılandırılmış bir panodur.

**Şekil 6: ayrıntılı ölçüm panosu**

:::image type="content" source="./media/azure-monitor-insights/detailed-metrics.png" alt-text="ayrıntılı ölçümler" lightbox="./media/azure-monitor-insights/detailed-metrics.png":::

## <a name="next-steps"></a>Sonraki adımlar

* Azure Izleyici 'de ölçümler hakkında daha fazla bilgi edinmek için bkz. [Azure izleyici 'de ölçümler](../azure-monitor/platform/data-platform-metrics.md).
* Tüm sanal WAN ölçümleri hakkında tam bir açıklama için bkz. [sanal WAN günlükleri ve ölçümleri](logs-metrics.md).
