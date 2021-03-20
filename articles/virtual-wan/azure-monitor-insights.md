---
title: Azure Izleyici öngörülerini kullanarak sanal WAN 'ı izleme
description: Bu makalede, Azure Izleyici öngörülerini kullanarak Azure sanal WAN izleme hakkında bilgi edineceksiniz.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: cherylmc
ms.openlocfilehash: 6ead00a0979d81ef11ac81fb13a1abe31317691d
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "100571320"
---
# <a name="azure-monitor-insights-for-virtual-wan"></a>Sanal WAN için Azure Izleyici öngörüleri

Azure sanal WAN için [Azure Monitor Insights](../azure-monitor/insights/network-insights-overview.md) , kullanıcılara ve operatörlere, bir oto keşfedilen topik Haritası aracılığıyla sunulan BIR sanal WAN 'ın durumunu ve durumunu görüntüleme olanağı sağlar. Haritadaki kaynak durumu ve durum kaplamaları, sanal WAN 'ın genel durumunun anlık görüntüsünü sunar. Sanal WAN portalının kaynak yapılandırma sayfalarına tek bir tıklama erişimi aracılığıyla haritadaki kaynaklara gidebilirsiniz.

Sanal WAN kaynak düzeyi ölçümleri toplanır ve önceden paketlenmiş bir sanal WAN ölçümleri çalışma kitabı aracılığıyla sunulur. Çalışma kitabı, sanal WAN, hub, ağ geçidi ve bağlantı düzeylerindeki ölçümleri gösterir. Bu makale, sanal WAN topolojinizi ve ölçümlerini tek bir yerde görüntülemek için sanal WAN için Azure Izleyici öngörülerini kullanma adımlarında size yol gösterir.

> [!NOTE]
> **Öngörüler** menü seçeneği, **Izleme** altındaki sanal WAN portalında bulunur. Ayrıca, ağlar için Azure Izleyici 'yi kullanarak sanal WAN topolojisi ve ölçüm çalışma kitabına erişebilirsiniz. Daha fazla bilgi için bkz. [ağlar Için Azure izleyici](../azure-monitor/insights/network-insights-overview.md). 
>

## <a name="before-you-begin"></a>Başlamadan önce

Bu makaledeki adımları tamamlayabilmeniz için, bir veya daha fazla hub ile sanal bir WAN olması gerekir. Bir sanal WAN ve bir hub oluşturmak için bu makalelerdeki adımları izleyin:

* [Sanal WAN oluşturma](virtual-wan-site-to-site-portal.md#openvwan)
* [Hub oluşturma](virtual-wan-site-to-site-portal.md#hub)

## <a name="view-vwan-topology"></a><a name="topology"></a>VWAN topolojisini görüntüle

**Azure Portal**  >  **sanal WAN**' a gidin. Sol bölmedeki **izleyici** menüsünde Öngörüler ' i **(Önizleme)** seçin. **Öngörüler** görünümü görüntülenir. Sanal WAN bağımlılığı eşlemesini ve üst düzey **ölçüm** mini çalışma kitabını gösterir.

**Şekil 1: > Öngörüler menüsünü Izleme**

:::image type="content" source="./media/azure-monitor-insights/monitor-menu.png" alt-text="Öngörüler (Önizleme) görünümünü gösteren ekran görüntüsü." lightbox="./media/azure-monitor-insights/monitor-menu.png":::

**Öngörüler** görünümünde, oto KEŞFEDILEN sanal WAN kaynaklarını görüntüleyebilirsiniz. Bu kaynaklar arasında hub, ağ geçitleri, güvenlik duvarları, bağlantılar ve bağlı olan sanal ağlar, üçüncü taraf NVA 'lar ve uçtan uca bir sanal WAN 'daki dallar bulunur. Bir örnek için bkz. **Şekil 2**.

Kaynak durumu ve durumu renk kodludur ve haritadaki kaynak simgelerinde yer alır. Hub kapasiteleri ve ağ geçidi kullanımı gibi üst düzey sanal WAN ölçümleri, bir mini çalışma kitabında pencerenin sağ tarafında görünür.

**Şekil 2: Öngörüler görünümü**

:::image type="content" source="./media/azure-monitor-insights/insights-view.png" alt-text="Öngörüler görünümünü gösteren ekran görüntüsü." lightbox="./media/azure-monitor-insights/insights-view.png":::

## <a name="dependency-view"></a><a name="dependency"></a>Bağımlılık görünümü

Sanal WAN için **bağımlılık** görünümü, bir hub ve bağlı bileşen mimarisine göre büyük ölçüde düzenlenmiş tüm sanal WAN kaynaklarının birbirine bağlı görünümünü görselleştirmenize yardımcı olur.

**Şekil 3: VWAN bağımlılığı görünümü**

:::image type="content" source="./media/azure-monitor-insights/dependency-map.png" alt-text="Bağımlılık görünümünü gösteren ekran görüntüsü." lightbox="./media/azure-monitor-insights/dependency-map.png":::

**Bağımlılık** görünümü eşlemesi aşağıdaki kaynakları bağlı bir grafik olarak görüntüler:

* Çeşitli Azure bölgeleri genelinde sanal WAN hub 'ları.
* Hub 'a doğrudan bağlı olan bağlı olan sanal ağlar.
* VPN ve Azure ExpressRoute şube siteleri ve P2S kullanıcıları ilgili ExpressRoute, S2S ve P2S bağlantıları ve sanal ağ geçitleri aracılığıyla her bir hub 'a bağlanır.
* Bir hub 'da dağıtılan (üçüncü taraf güvenlik duvarı proxy 'leri dahil) Azure Güvenlik duvarları (güvenli hub).
* Bağlı olan sanal ağlarda dağıtılan üçüncü taraf NVA 'lar (ağ sanal cihazları).

Bağımlılık eşlemesi, dolaylı olarak bağlı sanal ağları (sanal WAN bağlı sanal ağları ile eşlenmiş sanal ağlar) de görüntüler.

Bağımlılık eşlemesi, her bir kaynağın yapılandırma ayarlarına kolay gezinme sağlar. Örneğin, Merkez Bölgesi ve hub ön eki gibi temel kaynak yapılandırmasını görüntülemek için hub kaynağının üzerine gelebilmeniz gerekir. Hub kaynağının Azure portal sayfasına erişmek için sağ tıklayın.

**Şekil 4: kaynağa özgü bilgilere gitme**

:::image type="content" source="./media/azure-monitor-insights/resource-information.png" alt-text="Kaynağa özgü bilgilere nasıl gidebileceğiniz gösteren ekran görüntüsü.":::

**Bağımlılık** görünümündeki arama ve filtre çubuğu, grafikte arama yapmanın kolay bir yolunu sağlar. Çeşitli filtreler, aramanızı belirli bir yola ve duruma göre daraltmak için yardım sağlar.

**Şekil 5: arama ve filtreleme**

:::image type="content" source="./media/azure-monitor-insights/search-filter.png" alt-text="Arama ve filtre çubuğunu gösteren ekran görüntüsü." lightbox="./media/azure-monitor-insights/search-filter.png":::

## <a name="detailed-metrics"></a><a name="detailed"></a>Ayrıntılı ölçümler

Ayrıntılı **ölçümler** sayfasına erişmek için **ayrıntılı ölçümleri görüntüle** ' yi seçebilirsiniz. **Ölçümler** sayfası, ayrı sekmelerle önceden yapılandırılmış bir panodur. Bu sekmeler, sanal WAN kaynak kapasiteniz, performansı ve sanal WAN düzeyinde ve hub düzeyinde ve tek tek bağlantılar düzeyinde kullanım için Öngörüler sağlar.

**Şekil 6: ayrıntılı ölçüm panosu**

:::image type="content" source="./media/azure-monitor-insights/detailed-metrics.png" alt-text="Ayrıntılı ölçüm panosunu gösteren ekran görüntüsü." lightbox="./media/azure-monitor-insights/detailed-metrics.png":::

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi için bkz. [Azure izleyici 'de ölçümler](../azure-monitor/essentials/data-platform-metrics.md).
* Tüm sanal WAN ölçümleri hakkında tam bir açıklama için bkz. [sanal WAN günlükleri ve ölçümleri](logs-metrics.md).
