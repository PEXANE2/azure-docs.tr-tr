---
title: İzleme, Ölçümler ve uyarılar - Azure ExpressRoute | Microsoft Docs
description: Bu sayfa ExpressRoute izleme hakkında bilgi sağlar.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 08/22/2019
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: 92ec03e20fb6e681a0afd14048449ad004ebca0c
ms.sourcegitcommit: 007ee4ac1c64810632754d9db2277663a138f9c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69991452"
---
# <a name="expressroute-monitoring-metrics-and-alerts"></a>ExpressRoute izleme, ölçümler ve uyarılar

Bu makalede izleme, Ölçümler ve uyarılar Azure İzleyicisi'ni kullanarak ExpressRoute anlamanıza yardımcı olur. Azure İzleyici, uyarıları, tüm Azure tanılama günlükleri sorunlarınız için tüm ölçümleri tek sağlanır.
 
>[!NOTE]
>Kullanarak **Klasik ölçüm** önerilmez.
>

## <a name="circuit-metrics"></a>Bağlantı hattı ölçümleri

Gidilecek **ölçümleri**, izlemek istediğiniz bağlantı hattı için ExpressRoute sayfasını tıklatın. Altında **izleme**, görüntüleyebileceğiniz **ölçümleri**. Aşağıda listelenen ölçülerden seçim yapın. Varsayılan toplama uygulanır. İsteğe bağlı olarak, farklı boyutlarda ölçümleri gösterecek şekilde bölme uygulayabilirsiniz.

### <a name="metrics-available"></a>Kullanılabilir ölçümler: 
* **Kullanılabilirlik** 
    * ARP kullanılabilirliği
      * Kullanılabilir Boyutlar:
        * Eş (birincil/Ikincil ExpressRoute yönlendirici)
        * Eşleme türü (özel/genel/Microsoft)
    * BGP kullanılabilirliği
      * Kullanılabilir Boyutlar:
        * Eş (birincil/Ikincil ExpressRoute yönlendirici)
        * Eşleme türü (özel/genel/Microsoft)
* **Ğinden**
    * BitsInPerSecond
      * Kullanılabilir Boyutlar:
        * Eşleme türü (özel/genel/Microsoft)
    * BitsOutPerSecond
      * Kullanılabilir Boyutlar:
        * Eşleme türü (özel/genel/Microsoft)
    * GlobalReachBitsInPerSecond
      * Kullanılabilir Boyutlar:
        * Eşlenen devre Skey (hizmet anahtarı)
    * GlobalReachBitsOutPerSecond
      * Kullanılabilir Boyutlar:
        * Eşlenen devre Skey (hizmet anahtarı)

>[!NOTE]
>*GlobalGlobalReachBitsInPerSecond* ve *GlobalGlobalReachBitsOutPerSecond* kullanımı yalnızca en az bir Global Reach bağlantısı kurulduysa görünür olur.
>

## <a name="bits-in-and-out---metrics-across-all-peerings"></a>Tüm eşlemeler genelinde BITS Içi ve dışı ölçümler

Belirli bir ExpressRoute bağlantı hattındaki tüm eşlemelerde ölçümleri görüntüleyebilirsiniz.

![Bağlantı hattı ölçümleri](./media/expressroute-monitoring-metrics-alerts/ermetricspeering.jpg)

## <a name="bits-in-and-out---metrics-per-peering"></a>Eşleme başına BITS gelen ve giden ölçümler

Özel, genel ve Microsoft bit/saniye içinde eşleme için ölçümleri görüntüleyebilir.

![Eşleme başına ölçümler](./media/expressroute-monitoring-metrics-alerts/erpeeringmetrics.jpg) 

## <a name="bgp-availability---split-by-peer"></a>BGP kullanılabilirliği-eşe göre bölme  

Eşleme ve eşler arasında BGP 'nin gerçek zamanlı kullanılabilirliğini (birincil ve Ikincil ExpressRoute yönlendiricileri) görüntüleyebilirsiniz. Bu panoda, özel eşleme için birincil BGP oturumu ve özel eşleme için Ikinci BGP oturumu gösterilmektedir. 

![Eş başına BGP kullanılabilirliği](./media/expressroute-monitoring-metrics-alerts/erBgpAvailabilityMetrics.jpg) 

## <a name="arp-availability---split-by-peering"></a>ARP kullanılabilirliği-eşlemeye göre bölme  

Eşler arası ve eşlerde [ARP](https://docs.microsoft.com/azure/expressroute/expressroute-troubleshooting-arp-resource-manager) 'nin neredeyse gerçek zamanlı kullanılabilirliğini (birincil ve Ikincil ExpressRoute yönlendiricileri) görüntüleyebilirsiniz. Bu Pano, özel eşleme ARP oturumunu her iki eş arasında gösterir, ancak eşlemeler genelinde Microsoft eşlemesi için tamamlanmıştır. Varsayılan toplama (Ortalama) her iki eş üzerinde de kullanıldı.  

![Her eş için ARP kullanılabilirliği](./media/expressroute-monitoring-metrics-alerts/erArpAvailabilityMetrics.jpg) 

## <a name="expressroute-gateway-connections-in-bitsseconds"></a>ExpressRoute ağ geçidi bağlantıları bit/saniye

![Ağ Geçidi bağlantıları](./media/expressroute-monitoring-metrics-alerts/erconnections.jpg ) 

## <a name="alerts-for-expressroute-gateway-connections"></a>ExpressRoute ağ geçidi bağlantıları için uyarılar

1. Uyarıları yapılandırmak için gidin **Azure İzleyici**, ardından **uyarılar**.

   ![alerts](./media/expressroute-monitoring-metrics-alerts/eralertshowto.jpg)

2. Tıklayın **+ seçin hedef** ve ExpressRoute ağ geçidi bağlantı kaynağı seçin.

   ![hedef]( ./media/expressroute-monitoring-metrics-alerts/alerthowto2.jpg)
3. Uyarı ayrıntılarını tanımlayın.

   ![eylem grubu](./media/expressroute-monitoring-metrics-alerts/alerthowto3.jpg)

4. Tanımlamak ve eylem grubunu ekleyin.

   ![eylem grubu Ekle](./media/expressroute-monitoring-metrics-alerts/actiongroup.png)

## <a name="alerts-based-on-each-peering"></a>Her eşleme bağlı uyarılar

 ![ne](./media/expressroute-monitoring-metrics-alerts/basedpeering.jpg)

## <a name="configure-alerts-for-activity-logs-on-circuits"></a>Bağlantı hatları üzerinde etkinlik günlükleri için uyarıları yapılandırın

İçinde **Uyarı ölçütleri**, seçebileceğiniz **etkinlik günlüğü** sinyal türü ve sinyal seçin.

  ![başka bir](./media/expressroute-monitoring-metrics-alerts/alertshowto6activitylog.jpg)
  
## <a name="next-steps"></a>Sonraki adımlar

ExpressRoute bağlantınızı yapılandırın.
  
  * [Bağlantı hattı oluşturma ve değiştirme](expressroute-howto-circuit-arm.md)
  * [Eşleme yapılandırması oluşturma ve değiştirme](expressroute-howto-routing-arm.md)
  * [ExpressRoute bağlantı hattına bir Sanal Ağ bağlama](expressroute-howto-linkvnet-arm.md)
