---
title: Ölçümler ve uyarılar - Azure DNS
description: Bu öğrenme yolu ile Azure DNS ölçümleri ve uyarılarıyla başlayın.
services: dns
documentationcenter: na
author: rohinkoul
manager: kumudD
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/17/2018
ms.author: rohink
ms.openlocfilehash: 42acbc0d32b3ce5de4befcf112b68f611ad70542
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76937451"
---
# <a name="azure-dns-metrics-and-alerts"></a>Azure DNS ölçümleri ve uyarıları
Azure DNS, Microsoft Azure altyapısını kullanarak ad çözümlemesi sağlayan DNS etki alanları için bir barındırma hizmetidir. Bu makalede, Azure DNS hizmetiiçin ölçümler ve uyarılar açıklanmaktadır.

## <a name="azure-dns-metrics"></a>Azure DNS ölçümleri

Azure DNS, müşterilerin hizmette barındırılan DNS bölgelerinin belirli yönlerini izlemelerini sağlamak için ölçümler sağlar. Ayrıca, Azure DNS ölçümleri ile, ilgi koşullarına göre uyarıları yapılandırabilir ve alabilirsiniz. Ölçümler [Azure Monitor hizmeti](../azure-monitor/index.yml)aracılığıyla sağlanır. Azure DNS, DNS bölgeleriniz için Azure Monitor üzerinden aşağıdaki ölçümleri sağlar:

-   SorguHacmi
-   Kayıt Seti Sayısı
-   RecordSetCapacityKullanımation

[Bu ölçümlerin tanımını](../azure-monitor/platform/metrics-supported.md#microsoftnetworkdnszones) Azure Monitor dokümantasyon sayfasında da görebilirsiniz.
>[!NOTE]
> Şu anda, bu ölçümler yalnızca Azure DNS'de barındırılan Genel DNS bölgeleri için kullanılabilir. Azure DNS'de barındırılan Özel Bölgeler varsa, bu ölçümler bu bölgeler için veri sağlamaz. Ayrıca, ölçümler ve uyarı özelliği yalnızca Azure Genel bulutunda desteklenir. Egemen bulutlar için destek daha sonra takip edecektir. 

Ölçümleri görebileceğiniz en ayrıntılı öğe bir DNS bölgesidir. Şu anda bir bölge içindeki tek tek kaynak kayıtlarıiçin ölçümleri göremezsiniz.

### <a name="query-volume"></a>Sorgu hacmi

Azure DNS'deki *Sorgu Hacmi* ölçüsü, DNS bölgeniz için Azure DNS tarafından alınan DNS sorgularının (sorgu trafiği) hacmini gösterir. Ölçü birimi Count ve toplama bir süre içinde alınan tüm sorguların toplamıdır. 

Bu ölçütü görüntülemek için Azure portalındaki Monitör sekmesinden Ölçümler (önizleme) explorer deneyimini seçin. Kaynak açılır tarafından DNS bölgenizi seçin, Sorgu Hacmi metrikünü seçin ve Toplama olarak Toplam'ı seçin. Aşağıda ekran görüntüsü bir örnek gösterir.  Metrics Explorer deneyimi ve grafik hakkında daha fazla bilgi için Azure [Monitör Ölçümleri Gezgini'ne](../azure-monitor/platform/metrics-charts.md)bakın.

![Sorgu hacmi](./media/dns-alerts-metrics/dns-metrics-query-volume.png)

*Şekil: Azure DNS Sorgu Birim ölçümleri*

### <a name="record-set-count"></a>Kayıt Kümesi Sayısı
*Kayıt Kümesi Sayısı* ölçümü, DNS bölgeniz için Azure DNS'deki Kayıt Kümesi sayısını gösterir. Bölgenizde tanımlanan tüm Kayıt Kümeleri sayılır. Ölçüm birimi Count ve toplama tüm Recordsets Maksimum. Bu ölçütü görüntülemek için Azure portalındaki **Monitör** sekmesinden **Ölçümler (önizleme)** explorer deneyimini seçin. **Kaynak** açılır noktasından DNS bölgenizi seçin, **Kayıt Kümesi Sayısı** metrikünü seçin ve ardından **Toplama**olarak **Max'i** seçin. Metrics Explorer deneyimi ve grafik hakkında daha fazla bilgi için Azure [Monitör Ölçümleri Gezgini'ne](../azure-monitor/platform/metrics-charts.md)bakın. 

![Kayıt Kümesi Sayısı](./media/dns-alerts-metrics/dns-metrics-record-set-count.png)

*Şekil: Azure DNS Kayıt Kümesi Sayısı ölçümleri*


### <a name="record-set-capacity-utilization"></a>Kayıt Seti Kapasite Kullanımı
Azure DNS'deki *Kayıt Kümesi Kapasite Kullanımı* ölçümü, Bir DNS Bölgesi için Recordset kapasitenizin kullanım yüzdesini gösterir. Azure DNS'deki her DNS bölgesi, Bölge için izin verilen maksimum Kayıt Kümesi sayısını tanımlayan bir Kayıt Kümesi sınırına tabidir (bkz. [DNS sınırları).](dns-zones-records.md#limits) Bu nedenle, bu metrik, Recordset sınırına ne kadar yakın olduğunuzu gösterir. Örneğin, DNS bölgeniz için yapılandırılmış 500 Kayıt Kümesi varsa ve bölge varsayılan Recordset 5000 sınırına sahipse, RecordSetCapacityUtilization ölçümü %10 değerini gösterir (500'ü 5000'e bölerek elde edilir). Ölçüm birimi **Yüzde** ve **Toplama** türü **Maksimumdur.** Bu ölçütü görüntülemek için Azure portalındaki Monitör sekmesinden Ölçümler (önizleme) explorer deneyimini seçin. Kaynak açılır noktasından DNS bölgenizi seçin, Kayıt Kümesi Kapasite Kullanımı metrikünü seçin ve Toplama olarak Max'i seçin. Aşağıda ekran görüntüsü bir örnek gösterir. Metrics Explorer deneyimi ve grafik hakkında daha fazla bilgi için Azure [Monitör Ölçümleri Gezgini'ne](../azure-monitor/platform/metrics-charts.md)bakın. 

![Kayıt Kümesi Sayısı](./media/dns-alerts-metrics/dns-metrics-record-set-capacity-uitlization.png)

*Şekil: Azure DNS Kayıt Kümesi Kapasite Kullanımı ölçümleri*

## <a name="alerts-in-azure-dns"></a>Azure DNS'deki uyarılar
Azure Monitor, kullanılabilir metrik değerlere karşı uyarı verme olanağı sağlar. DNS ölçümleri yeni Uyarı yapılandırma deneyiminde kullanılabilir. Azure Monitor uyarıları [belgelerinde](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md)ayrıntılı olarak açıklandığı gibi, kaynak olarak DNS Bölgesi'ni seçebilir, Metrik sinyal türünü seçebilir ve uyarı mantığını ve **Dönem** ve **Sıklık**gibi diğer parametreleri yapılandırabilirsiniz. Uyarı koşulunun ne zaman karşılandığı ve uyarının seçilen eylemler aracılığıyla teslim edileceği bir [Eylem Grubu](../azure-monitor/platform/action-groups.md) daha da tanımlayabilirsiniz. Azure Monitor ölçümleri için uyarı yı yapılandırma hakkında daha fazla bilgi için Azure [Monitor kullanarak uyarı oluştur, görüntüleme ve yönetme](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md)bilgisine bakın. 

## <a name="next-steps"></a>Sonraki adımlar
- [Azure DNS](dns-overview.md)hakkında daha fazla bilgi edinin.
