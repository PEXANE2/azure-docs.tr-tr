---
title: Birden çok ISS bağlantısı üzerinde Azure yol seçimi
titleSuffix: Azure Virtual WAN
description: Azure yol seçimi ve sanal WAN hakkında bilgi edinin
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: article
ms.date: 06/01/2020
ms.author: cherylmc
ms.openlocfilehash: 73249b2c66c7e064aa5c07ae1b78d819f4f44c23
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84300361"
---
# <a name="azure-virtual-wan-logs-and-metrics"></a>Azure sanal WAN günlükleri ve ölçümleri

Azure Izleyici 'yi kullanarak Azure sanal WAN izleyebilirsiniz. Sanal WAN, tek bir işlemsel arabirim sağlamak için birçok ağ, güvenlik ve yönlendirme işlevlerini birlikte getiren bir ağ hizmetidir. Sanal WAN VPN Gateway 'leri, ExpressRoute ağ geçitleri ve Azure Güvenlik Duvarı, Azure Izleyici aracılığıyla günlüğe kaydetme ve ölçümlere sahiptir. Azure Güvenlik duvarı bilgileri için bkz. [Azure Güvenlik Duvarı günlükleri ve ölçümleri](../firewall/logs-and-metrics.md).

Bu makalede Portal aracılığıyla kullanılabilen ölçümler ve Tanılamalar ele alınmaktadır. Ölçümler hafif ve neredeyse gerçek zamanlı senaryoların yanı sıra uyarı ve hızlı sorun algılama için yararlı hale getirebilirsiniz.

## <a name="metrics"></a>Ölçümler

Azure Izleyici ölçümleri, belirli bir zamanda sistemin bazı yönlerini tanımlayan sayısal değerlerdir. Ölçümler her dakika toplanır ve sıklıkla örneklenebilir olduğundan uyarma için faydalıdır. Bir uyarı görece basit mantık ile hızlı bir şekilde tetiklenebilir.

### <a name="site-to-site-vpn-gateways"></a>Siteden siteye VPN ağ geçitleri

Azure siteden siteye VPN ağ geçitleri için aşağıdaki ölçümler mevcuttur:

* **Ağ geçidi bant genişliği** – bir ağ geçidinin ortalama siteden siteye toplam bant genişliği (saniye başına bayt).
* **Tünel bant genişliği** – saniye başına bayt cinsinden bir tünelin ortalama bant genişliği.
* **Tünel çıkış baytları** – bir tünelin giden bayt sayısı. 
* **Tünel çıkış paketleri** – bir tünelin giden paket sayısı. 
* **Tünel çıkışları eşleşmeyen paket bırakma** – giden paket bırakma sayısı, bir tünelin trafik seçiciyle uyuşmuyor. 
* **Tünel girişi baytları** – bir tünelin gelen bayt sayısı. 
* **Tünel giriş paketi** – bir tünelin gelen paket sayısı. 
* **Tünel girişi uyumsuz paket bırakma** -gelen paket bırakma sayısı, bir tünelin trafik seçiciyle uyuşmuyor. 

### <a name="point-to-site-vpn-gateways"></a>Noktadan siteye VPN ağ geçitleri

Azure Noktadan siteye VPN ağ geçitleri için aşağıdaki ölçümler mevcuttur:

* **Gateway P2S band genişliği** – bir ağ geçidinin ortalama Noktadan siteye toplam bant genişliği (saniye başına bayt).
* **P2S bağlantı sayısı** – bir ağ geçidinin Noktadan siteye bağlantı sayısı.

### <a name="azure-expressroute-gateways"></a>Azure ExpressRoute ağ geçitleri

Azure ExpressRoute ağ geçitleri için aşağıdaki ölçümler mevcuttur:

* **Bitsinpersecond** – BITS giriş/saniye.
* **Bitsoutpersecond** – bit, saniye başına Azure 'a kurtarıldı.

### <a name="view-gateway-metrics"></a><a name="metrics-steps"></a>Ağ Geçidi ölçümlerini görüntüle

Aşağıdaki adımlar ölçümleri bulmanıza ve görüntülemenize yardımcı olur:

1. Portalda, ağ geçidine sahip sanal hub 'a gidin.

2. Siteden siteye ağ geçidini bulmak için ExpressRoute, bir ExpressRoute ağ geçidini bulmak için **ExpressRoute** veya bir noktadan siteye ağ geçidini bulmak IÇIN **Kullanıcı VPN (siteye işaret)** **seçeneğini belirleyin.** Sayfasında, ağ geçidi bilgilerini görebilirsiniz. Bu bilgileri kopyalayın. Daha sonra, Azure Izleyici 'yi kullanarak tanılamayı görüntülemek için kullanacaksınız.

3. **Ölçümler**’i seçin.

   :::image type="content" source="./media/logs-metrics/metrics.png" alt-text="ölçümler":::

4. **Ölçümler** sayfasında, ilgilendiğiniz ölçümleri görebilirsiniz.

   :::image type="content" source="./media/logs-metrics/metrics-page.png" alt-text="ölçümler sayfası":::

## <a name="diagnostic-logs"></a><a name="diagnostic"></a>Tanılama günlükleri

### <a name="site-to-site-vpn-gateways"></a>Siteden siteye VPN ağ geçitleri

Azure siteden siteye VPN ağ geçitleri için aşağıdaki Tanılamalar mevcuttur:

* **Ağ geçidi tanılama günlükleri** – sistem durumu, yapılandırma, hizmet güncelleştirmeleri ve ek tanılama gibi ağ geçidine özgü Tanılamalar.
* **Tünel tanılama günlükleri** – Bunlar, siteden siteye IPSec tüneli, anlaşmalı SAS, bağlantı kesme nedenleri ve ek Tanılamalar için bağlanma ve bağlantı kesme olayları gibi IPSec tünelinde ilgili günlüklerdir.
* **Tanılama günlüklerini yönlendirme** : Bunlar, statik YOLLARıN, BGP 'nin, yol güncelleştirmelerinin yanı sıra ek Tanılamalar için olaylarla ilgili günlüklerdir.
* **Ike tanılama günlükleri** – IPSec bağlantıları için IKE 'ye özgü Tanılamalar.

### <a name="point-to-site-vpn-gateways"></a>Noktadan siteye VPN ağ geçitleri

Azure Noktadan siteye VPN ağ geçitleri için aşağıdaki Tanılamalar mevcuttur:

* **Ağ geçidi tanılama günlükleri** – sistem durumu, yapılandırma, hizmet güncelleştirmeleri ve diğer Tanılamalar gibi ağ geçidine özgü Tanılamalar.
* **Ike tanılama günlükleri** – IPSec bağlantıları için IKE 'ye özgü Tanılamalar.
* **P2S tanılama günlükleri** – Bunlar, kullanıcı VPN (noktadan sıteye) P2S yapılandırma ve istemci olaylardır. Bunlara istemci bağlantısı/bağlantıyı kes, VPN istemci adresi ayırma ve diğer Tanılamalar de dahildir.

### <a name="view-diagnostic-logs"></a><a name="diagnostic-steps"></a>Tanılama günlüklerini görüntüle

Aşağıdaki adımlar tanılamayı bulmanıza ve görüntülemenize yardımcı olur:

1. Portalda sanal WAN kaynağınız ' ne gidin. Portalda sanal WAN sayfasının **genel bakış** bölümünde, görünüm ve kaynak grubu bilgilerini almak Için **temel parçalar** ' ı seçin. Kaynak grubu bilgilerini kopyalayın.

   :::image type="content" source="./media/logs-metrics/3.png" alt-text="ölçümler sayfası":::

2. Izleme bölümünde kaynak grubuna gidin. **Tanılama ayarları**' nı seçin ve kaynak bilgilerini girin. Bu makalenin önceki bölümlerinde yer alan [ağ geçidi ölçümlerini görüntüle](#metrics-steps) bölümünde 2. adımda kopyaladığınız kaynak bilgileri.

   :::image type="content" source="./media/logs-metrics/4.png" alt-text="ölçümler sayfası":::

3. Sonuçlar sayfasında **+ Tanılama ayarı Ekle**' yi seçin ve ardından bir seçenek belirleyin. Log Analytics, bir olay hub 'ına ya da yalnızca bir depolama hesabına arşivleme ' ye gönderme seçeneğini belirleyebilirsiniz.

   :::image type="content" source="./media/logs-metrics/5.png" alt-text="ölçümler sayfası":::

### <a name="log-analytics-sample-query"></a><a name="sample-query"></a>Örnek sorgu Log Analytics

Günlükler **Azure Log Analytics çalışma alanında**bulunur. Log Analytics bir sorgu ayarlayabilirsiniz. Aşağıdaki örnek, siteden siteye yol tanılamayı elde etmek için bir sorgu içerir.

```AzureDiagnostics | where Category == "RouteDiagnosticLog"```

Aşağıdaki değerleri, **= =** öğesinden sonra gerektiği şekilde değiştirin.

* "GatewayDiagnosticLog"
* "Ikediagnosticlog"
* "P2SDiagnosticLog"
* "Tüneldiagnosticlog"
* "RouteDiagnosticLog"

## <a name="activity-logs"></a><a name="activity-logs"></a>Etkinlik günlükleri

**Etkinlik günlüğü** girdileri varsayılan olarak toplanır ve Azure Portal görüntülenebilir. Azure aboneliğinize gönderilen tüm işlemleri görüntülemek için Azure etkinlik günlüklerini (eski adıyla *İşlemsel Günlükler* ve *Denetim günlükleri*olarak bilinir) kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Güvenlik Duvarı günlüklerini ve ölçümlerini izlemeyi öğrenmek için bkz. [öğretici: Azure Güvenlik Duvarı günlüklerini izleme](../firewall/tutorial-diagnostics.md).
* Azure Izleyici 'de ölçümler hakkında daha fazla bilgi edinmek için bkz. [Azure izleyici 'de ölçümler](../azure-monitor/platform/data-platform-metrics.md).
