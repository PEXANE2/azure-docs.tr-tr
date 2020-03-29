---
title: 'Azure ExpressRoute: İzleme, Ölçümler ve Uyarılar'
description: Bu sayfa ExpressRoute izleme hakkında bilgi sağlar
services: expressroute
author: mialdrid
ms.service: expressroute
ms.topic: conceptual
ms.date: 08/22/2019
ms.author: cherylmc
ms.openlocfilehash: 268a7e7c94285d3c4fdcb0c5fb91b685c09b58c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75436908"
---
# <a name="expressroute-monitoring-metrics-and-alerts"></a>ExpressRoute izleme, ölçümler ve uyarılar

Bu makale, Azure Monitor kullanarak ExpressRoute izleme, ölçümler ve uyarıları anlamanıza yardımcı olur. Azure Monitor, tüm Azure'daki tüm ölçümler, uyarı, tanılama günlükleri için tek duraktır.
 
>[!NOTE]
>**Klasik Ölçümler'in** kullanılması önerilmez.
>

## <a name="expressroute-metrics"></a>ExpressRoute ölçümleri

**Ölçümleri**görüntülemek için *Azure Monitörü* sayfasına gidin ve *Ölçümler'i*tıklatın. **ExpressRoute** ölçümlerini görüntülemek için Kaynak Türü *ExpressRoute devrelerine*göre filtre uygulayın. Global **Erişim** ölçümlerini görüntülemek için, Resource Type *ExpressRoute devrelerine* göre filtre uygulayın ve Global Reach etkinleştirilmiş bir ExpressRoute devre kaynağı seçin. **ExpressRoute Direct** ölçümlerini görüntülemek için, Kaynak Türünü *ExpressRoute Bağlantı Noktalarına*göre filtreleyin. 

Bir metrik seçildikten sonra varsayılan toplama uygulanır. İsteğe bağlı olarak, farklı boyutlarda metrik gösterecektir bölme uygulayabilirsiniz.

### <a name="available-metrics"></a>Kullanılabilir Ölçümler
|**Ölçüm**|**Kategori**|**Boyut(lar)**|**Özellik(ler)**|
| --- | --- | --- | --- |
|ARP Kullanılabilirliği|Kullanılabilirlik|<ui><li>Eş (Birincil/İkincil ExpressRoute yönlendiricisi)</ui></li><ui><li> Peering Türü (Özel/Genel/Microsoft)</ui></li>|ExpressRoute|
|Bgp Kullanılabilirlik|Kullanılabilirlik|<ui><li> Eş (Birincil/İkincil ExpressRoute yönlendiricisi)</ui></li><ui><li> Peering Türü</ui></li>|ExpressRoute|
|BitsInPerİkinci|Trafik|<ui><li> Peering Türü (ExpressRoute)</ui></li><ui><li>Bağlantı (ExpressRoute Direct)</ui></li>| <li> ExpressRoute</li><li>ExpressRoute Direct|
|BitsOutPerİkinci|Trafik| <ui><li>Peering Türü (ExpressRoute)</ui></li><ui><li> Bağlantı (ExpressRoute Direct) | <ui><li>Ekspres Rota<ui><li>ExpressRoute Direct</ui></li> |
|GlobalReachBitsInPerSecond|Trafik|<ui><li>Peered Circuit Anahtar (Servis Anahtarı)</ui></li>|Küresel Erişim|
|GlobalReachBitsOutPerSecond|Trafik|<ui><li>Peered Circuit Anahtar (Servis Anahtarı)</ui></li>|Küresel Erişim|
|Yönetici Durumu|Fiziksel Bağlantı|Bağlantı|ExpressRoute Direct|
|LineProtokolü|Fiziksel Bağlantı|Bağlantı|ExpressRoute Direct|
|RxLightLevel|Fiziksel Bağlantı|<ui><li>Bağlantı</ui></li><ui><li>Lane</ui></li>|ExpressRoute Direct|
|TxLightLevel|Fiziksel Bağlantı|<ui><li>Bağlantı</ui></li><ui><li>Lane</ui></li>|ExpressRoute Direct|
>[!NOTE]
>*GlobalGlobalReachBitsInPerSecond* ve *GlobalGlobalReachBitsOutPerSecond* kullanarak sadece en az bir Global Reach bağlantısı kurulduğunda görünür olacaktır.
>

## <a name="circuits-metrics"></a>Devre ölçümleri

### <a name="bits-in-and-out---metrics-across-all-peerings"></a>Bit In ve Out - Tüm bakışlarda ölçümler

Belirli bir ExpressRoute devresindeki tüm görünümlerde ölçümleri görüntüleyebilirsiniz.

![devre ölçümleri](./media/expressroute-monitoring-metrics-alerts/ermetricspeering.jpg)

### <a name="bits-in-and-out---metrics-per-peering"></a>Bit Ler Girip Çıkar - Eşleme başına ölçümler

Özel, herkese açık ve Microsoft eşleyen ölçümleri bit/saniye olarak görüntüleyebilirsiniz.

![eşleme başına ölçümler](./media/expressroute-monitoring-metrics-alerts/erpeeringmetrics.jpg) 

### <a name="bgp-availability---split-by-peer"></a>BGP Kullanılabilirliği - Eşe Göre Bölün  

BGP'nin gerçek zamanlı kullanılabilirliğini, eşler ve eşler arasında (Birincil ve İkincil ExpressRoute yönlendiricileri) görebilirsiniz. Bu pano, özel bakış için Birincil BGP oturumunu ve özel bakış için İkinci BGP oturumunu aşağı gösterir. 

![Eş başına BGP kullanılabilirliği](./media/expressroute-monitoring-metrics-alerts/erBgpAvailabilityMetrics.jpg) 

### <a name="arp-availability---split-by-peering"></a>ARP Kullanılabilirliği - Peering tarafından bölün  

[ARP'nin](https://docs.microsoft.com/azure/expressroute/expressroute-troubleshooting-arp-resource-manager) gerçek zamanlı kullanılabilirliğini, eşler ve eşler (Birincil ve İkincil ExpressRoute yönlendiricileri) arasında görebilirsiniz. Bu pano, Her iki eş arasında Özel Eşleme ARP oturumunu gösterir, ancak Microsoft'un eşlemeler arasında baklaması için aşağı tamamlanmıştır. Varsayılan toplama (Ortalama) her iki eş arasında kullanılmıştır.  

![Eş başına ARP kullanılabilirliği](./media/expressroute-monitoring-metrics-alerts/erArpAvailabilityMetrics.jpg) 

## <a name="expressroute-direct-metrics"></a>ExpressRoute Doğrudan Ölçümleri

### <a name="admin-state---split-by-link"></a>Admin State - Linke Göre Böl
ExpressRoute Direct bağlantı noktası çiftinin her bağlantısı için yönetici durumunu görüntüleyebilirsiniz.

![er doğrudan yönetici durum](./media/expressroute-monitoring-metrics-alerts/adminstate-per-link.jpg)

### <a name="bits-in-per-second---split-by-link"></a>Saniyede Bitler - Bağlantıya Göre Bölün
ExpressRoute Direct bağlantı noktası çiftinin her iki bağlantısında da bitleri saniyede görüntüleyebilirsiniz. 

![er saniyede doğrudan bit](./media/expressroute-monitoring-metrics-alerts/bits-in-per-second-per-link.jpg)

### <a name="bits-out-per-second---split-by-link"></a>Bitleri Saniyede Dışarı - Bağlantıya göre bölme
Ayrıca, ExpressRoute Direct bağlantı noktası çiftinin her iki bağlantıda da bitleri saniyede görüntüleyebilirsiniz. 

![er saniyede doğrudan bit](./media/expressroute-monitoring-metrics-alerts/bits-out-per-second-per-link.jpg)

### <a name="line-protocol---split-by-link"></a>Hat Protokolü - Bağlantıya göre bölme
ExpressRoute Direct bağlantı noktası çiftinin her bağlantısında hat protokolünü görüntüleyebilirsiniz.

![er doğrudan hat protokolü](./media/expressroute-monitoring-metrics-alerts/line-protocol-per-link.jpg)

### <a name="rx-light-level---split-by-link"></a>Rx Işık Seviyesi - Bağlantıya göre bölme
Her bağlantı noktası için Rx ışık düzeyini (ExpressRoute Direct bağlantı noktasının **aldığı**ışık düzeyini) görüntüleyebilirsiniz. Sağlıklı Rx ışık seviyeleri genellikle -10 ila 0 dBm aralığında

![er direkt hat Rx Işık Seviyesi](./media/expressroute-monitoring-metrics-alerts/rxlight-level-per-link.jpg)

### <a name="tx-light-level---split-by-link"></a>Tx Işık Seviyesi - Bağlantıya göre bölme
Her bağlantı noktası için Tx ışık düzeyini (ExpressRoute Direct bağlantı noktasının **aktardığı**ışık düzeyini) görüntüleyebilirsiniz. Sağlıklı Tx ışık seviyeleri genellikle -10-0 dBm aralığında

![er direkt hat Rx Işık Seviyesi](./media/expressroute-monitoring-metrics-alerts/txlight-level-per-link.jpg)

## <a name="expressroute-gateway-connections-in-bitsseconds"></a>Bit/saniye cinsinden ExpressRoute ağ geçidi bağlantıları

![ağ geçidi bağlantıları](./media/expressroute-monitoring-metrics-alerts/erconnections.jpg )

## <a name="alerts-for-expressroute-gateway-connections"></a>ExpressRoute ağ geçidi bağlantıları için uyarılar

1. Uyarıları yapılandırmak için **Azure Monitor'a**gidin ve **ardından Uyarılar'ı**tıklatın.

   ![alerts](./media/expressroute-monitoring-metrics-alerts/eralertshowto.jpg)

2. **+Hedef'i seçin** ve ExpressRoute ağ geçidi bağlantı kaynağını seçin.

   ![Hedef]( ./media/expressroute-monitoring-metrics-alerts/alerthowto2.jpg)
3. Uyarı ayrıntılarını tanımlayın.

   ![eylem grubu](./media/expressroute-monitoring-metrics-alerts/alerthowto3.jpg)

4. Eylem grubunu tanımlayın ve ekleyin.

   ![eylem grubu ekleme](./media/expressroute-monitoring-metrics-alerts/actiongroup.png)

## <a name="alerts-based-on-each-peering"></a>Her bir eşleme dayalı uyarılar

 ![Ne](./media/expressroute-monitoring-metrics-alerts/basedpeering.jpg)

## <a name="configure-alerts-for-activity-logs-on-circuits"></a>Devreler üzerindeki etkinlik günlükleri için uyarıları yapılandırma

Uyarı **Ölçütleri'nde,** Sinyal Türü için **Etkinlik Günlüğü'nü** seçebilir ve Sinyal'i seçebilirsiniz.

  ![Başka](./media/expressroute-monitoring-metrics-alerts/alertshowto6activitylog.jpg)
  
## <a name="next-steps"></a>Sonraki adımlar

ExpressRoute bağlantınızı yapılandırın.
  
  * [Bağlantı hattı oluşturma ve değiştirme](expressroute-howto-circuit-arm.md)
  * [Eşleme yapılandırması oluşturma ve değiştirme](expressroute-howto-routing-arm.md)
  * [ExpressRoute bağlantı hattına bir Sanal Ağ bağlama](expressroute-howto-linkvnet-arm.md)
