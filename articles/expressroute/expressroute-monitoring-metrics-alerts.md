---
title: 'Azure ExpressRoute: Izleme, ölçümler ve uyarılar'
description: Azure Izleyici 'yi kullanarak Azure ExpressRoute izleme, ölçümler ve uyarılar hakkında bilgi edinin. Azure 'da tüm ölçümler, uyarı, tanılama günlükleri için bir durdurma.
services: expressroute
author: mialdrid
ms.service: expressroute
ms.topic: how-to
ms.date: 08/25/2020
ms.author: cherylmc
ms.openlocfilehash: 6f3bd5392457f05caa8704e7e1de5b4155b28794
ms.sourcegitcommit: 927dd0e3d44d48b413b446384214f4661f33db04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88871368"
---
# <a name="expressroute-monitoring-metrics-and-alerts"></a>ExpressRoute izleme, ölçümler ve uyarılar

Bu makale, Azure Izleyici kullanarak ExpressRoute izlemeyi, ölçümleri ve uyarıları anlamanıza yardımcı olur. Azure Izleyici, tüm Azure genelinde tüm ölçümler, uyarılar ve tanılama günlükleri için bir dur.
 
>[!NOTE]
>**Klasik ölçümlerin** kullanılması önerilmez.
>

## <a name="expressroute-metrics"></a>ExpressRoute ölçümleri

**Ölçümleri**görüntülemek Için *Azure izleyici* sayfasına gidin ve *ölçümler*' e tıklayın. **ExpressRoute** ölçümlerini görüntülemek Için, *ExpressRoute devreleri*kaynak türüne göre filtreleyin. **Global Reach** ölçümlerini görüntülemek Için, *ExpressRoute devreleri* kaynak türüne göre filtreleyin ve Global Reach etkin bir ExpressRoute bağlantı hattı kaynağı seçin. **ExpressRoute doğrudan** ölçümlerini görüntülemek Için kaynak türünü *ExpressRoute bağlantı noktalarıyla*filtreleyin. 

Ölçüm seçildikten sonra, varsayılan toplama uygulanır. İsteğe bağlı olarak, ölçüyü farklı boyutlara sahip olacak şekilde bölerek bölmeyi uygulayabilirsiniz.

### <a name="available-metrics"></a>Kullanılabilir ölçümler

|**Ölçüm**|**Kategori**|**Boyut (ler)**|**Özellik (ler)**|
| --- | --- | --- | --- |
|ARP kullanılabilirliği|Kullanılabilirlik|<ui><li>Eş (birincil/Ikincil ExpressRoute yönlendirici)</ui></li><ui><li> Eşleme türü (özel/genel/Microsoft)</ui></li>|ExpressRoute|
|BGP kullanılabilirliği|Kullanılabilirlik|<ui><li> Eş (birincil/Ikincil ExpressRoute yönlendirici)</ui></li><ui><li> Eşleme türü</ui></li>|ExpressRoute|
|BitsInPerSecond|Trafik|<ui><li> Eşleme türü (ExpressRoute)</ui></li><ui><li>Bağlantı (ExpressRoute Direct)</ui></li>|<li>ExpressRoute</li><li>ExpressRoute Direct|
|BitsOutPerSecond|Trafik| <ui><li>Eşleme türü (ExpressRoute)</ui></li><ui><li> Bağlantı (ExpressRoute Direct) |<ui><li>ExpressRoute<ui><li>ExpressRoute Direct</ui></li> |
|CPU Kullanımı|Performans| <ui><li>Örnek</ui></li>|ExpressRoute sanal ağ geçidi|
|Paket/saniye|Performans| <ui><li>Örnek</ui></li>|ExpressRoute sanal ağ geçidi|
|GlobalReachBitsInPerSecond|Trafik|<ui><li>Eşlenen devre Skey (hizmet anahtarı)</ui></li>|Global Reach|
|GlobalReachBitsOutPerSecond|Trafik|<ui><li>Eşlenen devre Skey (hizmet anahtarı)</ui></li>|Global Reach|
|AdminState|Fiziksel bağlantı|Bağlantı|ExpressRoute Direct|
|LineProtocol|Fiziksel bağlantı|Bağlantı|ExpressRoute Direct|
|RxLightLevel|Fiziksel bağlantı|<ui><li>Bağlantısının</ui></li><ui><li>Ödül</ui></li>|ExpressRoute Direct|
|TxLightLevel|Fiziksel bağlantı|<ui><li>Bağlantısının</ui></li><ui><li>Ödül</ui></li>|ExpressRoute Direct|
>[!NOTE]
>*GlobalGlobalReachBitsInPerSecond* ve *GlobalGlobalReachBitsOutPerSecond* kullanımı yalnızca en az bir Global Reach bağlantısı kurulduysa görünür olur.
>

## <a name="circuits-metrics"></a>Devreler ölçümleri

### <a name="bits-in-and-out---metrics-across-all-peerings"></a>Tüm eşlemeler genelinde BITS Içi ve dışı ölçümler

Belirli bir ExpressRoute bağlantı hattındaki tüm eşlemelerde ölçümleri görüntüleyebilirsiniz.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/ermetricspeering.jpg" alt-text="devre ölçümleri":::

### <a name="bits-in-and-out---metrics-per-peering"></a>Eşleme başına BITS gelen ve giden ölçümler

Özel, genel ve Microsoft eşlemesi için ölçümleri bit/saniye cinsinden görüntüleyebilirsiniz.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/erpeeringmetrics.jpg" alt-text="eşleme başına ölçümler":::

### <a name="bgp-availability---split-by-peer"></a>BGP kullanılabilirliği-eşe göre bölme  

Eşleme ve eşler arasında BGP 'nin gerçek zamanlı kullanılabilirliğini (birincil ve Ikincil ExpressRoute yönlendiricileri) görüntüleyebilirsiniz. Bu panoda, özel eşleme için birincil BGP oturumu ve özel eşleme için Ikinci BGP oturumu gösterilmektedir. 

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/erBgpAvailabilityMetrics.jpg" alt-text="Eş başına BGP kullanılabilirliği":::

### <a name="arp-availability---split-by-peering"></a>ARP kullanılabilirliği-eşlemeye göre bölme  

Eşler arası ve eşlerde [ARP](https://docs.microsoft.com/azure/expressroute/expressroute-troubleshooting-arp-resource-manager) 'nin neredeyse gerçek zamanlı kullanılabilirliğini (birincil ve Ikincil ExpressRoute yönlendiricileri) görüntüleyebilirsiniz. Bu Pano, özel eşleme ARP oturumunu her iki eş arasında gösterir, ancak eşlemeler genelinde Microsoft eşlemesi için tamamlanmıştır. Varsayılan toplama (Ortalama) her iki eş üzerinde de kullanıldı.  

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/erArpAvailabilityMetrics.jpg" alt-text="Her eş için ARP kullanılabilirliği":::

## <a name="expressroute-direct-metrics"></a>ExpressRoute doğrudan ölçümleri

### <a name="admin-state---split-by-link"></a>Yönetici durumu-bağlantıya göre Böl

ExpressRoute doğrudan bağlantı noktası çiftinin her bir bağlantısı için yönetici durumunu görüntüleyebilirsiniz.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/adminstate-per-link.jpg" alt-text="ER doğrudan yönetici durumu":::

### <a name="bits-in-per-second---split-by-link"></a>Saniyedeki bit/saniye-bağlantıya göre Böl

Her iki ExpressRoute doğrudan bağlantı noktası çiftinin bağlantılarında, saniye başına bit sayısını görüntüleyebilirsiniz.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/bits-in-per-second-per-link.jpg" alt-text="Saniyedeki doğrudan bit/saniye":::

### <a name="bits-out-per-second---split-by-link"></a>Saniye başına BITS-bağlantıya göre bölme

Ayrıca, ExpressRoute doğrudan bağlantı noktası çiftinin her iki bağlantı için de saniyede BITS görüntüleyebilirsiniz.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/bits-out-per-second-per-link.jpg" alt-text="ER saniye başına doğrudan BITS":::

### <a name="line-protocol---split-by-link"></a>Satır Protokolü-bağlantıya göre Böl

Her ExpressRoute doğrudan bağlantı noktası çiftinin bağlantı genelinde hat protokolünü görüntüleyebilirsiniz.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/line-protocol-per-link.jpg" alt-text="ER doğrudan hat Protokolü":::

### <a name="rx-light-level---split-by-link"></a>RX hafif düzeyi-bağlantıya göre bölme

Her bağlantı noktası için RX ışık düzeyini (ExpressRoute doğrudan bağlantı noktasının **aldığı**ışık düzeyi) görüntüleyebilirsiniz. Sağlıklı RX hafif düzeyler genellikle-10 ila 0 dBm aralığında yer almalıdır

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/rxlight-level-per-link.jpg" alt-text="ER doğrudan Line RX hafif düzeyi":::

### <a name="tx-light-level---split-by-link"></a>TX hafif düzeyi-bağlantıya göre Böl

Her bağlantı noktası için, TX ışık düzeyini (ExpressRoute doğrudan bağlantı noktasının **Iletmekte**olan ışık düzeyi) görüntüleyebilirsiniz. Sağlıklı TX hafif düzeyleri genellikle-10 ila 0 dBm aralığında yer almalıdır

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/txlight-level-per-link.jpg" alt-text="ER doğrudan hat TX hafif düzeyi":::

## <a name="expressroute-virtual-network-gateway-metrics"></a>ExpressRoute sanal ağ geçidi ölçümleri

### <a name="cpu-utilization---split-instance"></a>CPU kullanımı-bölünmüş örnek

Ağ Geçidi örneklerinin CPU kullanımını görüntüleyebilirsiniz.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/cpu-split.jpg" alt-text="CPU bölünmüş":::

### <a name="packets-per-second---split-by-instance"></a>Saniyedeki paket sayısı-örneğe göre Böl

Ağ geçidine geçiş yaparken paket sayısını görüntüleyebilirsiniz.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/pps-split.jpg" alt-text="Saniyedeki paket sayısı-bölünmüş":::

## <a name="expressroute-gateway-connections-in-bitsseconds"></a>Bit/saniye cinsinden ExpressRoute ağ geçidi bağlantıları

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/erconnections.jpg" alt-text="Ağ Geçidi bağlantıları":::

## <a name="alerts-for-expressroute-gateway-connections"></a>ExpressRoute ağ geçidi bağlantıları için uyarılar

1. Uyarıları yapılandırmak için **Azure İzleyicisi**' ne gidin ve ardından **Uyarılar**' ı seçin.

   :::image type="content" source="./media/expressroute-monitoring-metrics-alerts/eralertshowto.jpg" alt-text="uyarılar":::
2. **+ Hedef Seç** ' e tıklayın ve ExpressRoute ağ geçidi bağlantı kaynağını seçin.

   :::image type="content" source="./media/expressroute-monitoring-metrics-alerts/alerthowto2.jpg" alt-text="hedef":::
3. Uyarı ayrıntılarını tanımlayın.

   :::image type="content" source="./media/expressroute-monitoring-metrics-alerts/alerthowto3.jpg" alt-text="eylem grubu":::
4. Eylem grubunu tanımlayın ve ekleyin.

   :::image type="content" source="./media/expressroute-monitoring-metrics-alerts/actiongroup.png" alt-text="eylem grubu Ekle":::

## <a name="alerts-based-on-each-peering"></a>Her eşlemeye göre uyarılar

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/basedpeering.jpg" alt-text="Her eşleme":::

## <a name="configure-alerts-for-activity-logs-on-circuits"></a>Devrelere ilişkin etkinlik günlüklerine yönelik uyarıları yapılandırın

**Uyarı ölçütlerinde**, sinyal türü Için **etkinlik günlüğü** ' nü seçebilir ve sinyali seçebilirsiniz.

:::image type="content" source="./media/expressroute-monitoring-metrics-alerts/alertshowto6activitylog.jpg" alt-text="etkinlik günlükleri":::
  
## <a name="next-steps"></a>Sonraki adımlar

ExpressRoute bağlantınızı yapılandırın.
  
* [Bağlantı hattı oluşturma ve değiştirme](expressroute-howto-circuit-arm.md)
* [Eşleme yapılandırması oluşturma ve değiştirme](expressroute-howto-routing-arm.md)
* [ExpressRoute bağlantı hattına bir Sanal Ağ bağlama](expressroute-howto-linkvnet-arm.md)
