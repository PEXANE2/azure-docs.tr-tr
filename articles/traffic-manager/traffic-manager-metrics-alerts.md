---
title: Azure Trafik Yöneticisi'nde Ölçümler ve Uyarılar
description: Bu makalede, Azure'daki Trafik Yöneticisi için kullanılabilen ölçümleri ve uyarıları öğrenin.
services: traffic-manager
author: rohinkoul
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/11/2018
ms.author: rohink
ms.openlocfilehash: 521e6ac605d187c0f95545611a17a86cfda6e1dd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76938597"
---
# <a name="traffic-manager-metrics-and-alerts"></a>Trafik Yöneticisi ölçümleri ve uyarıları

Trafik Yöneticisi, birden çok yönlendirme yöntemi ve uç nokta izleme seçeneği içeren DNS tabanlı yük dengeleme sağlar. Bu makalede, müşteriler tarafından kullanılabilen ölçümler ve ilişkili uyarılar açıklanmaktadır. 

## <a name="metrics-available-in-traffic-manager"></a>Trafik Yöneticisi'nde bulunan ölçümler 

Traffic Manager, müşterilerin Trafik yöneticisinin kullanımını ve bu profilaltındaki uç noktalarının durumunu anlamak için kullanabilecekleri profil bazında aşağıdaki ölçümleri sağlar.  

### <a name="queries-by-endpoint-returned"></a>Uç noktaya göre sorgular döndürülür
Trafik Yöneticisi profilinin belirli bir dönemde işlediği sorgu sayısını görüntülemek için [bu ölçütü](../azure-monitor/platform/metrics-supported.md) kullanın. Trafik Yöneticisi'nden gelen sorgu yanıtlarında bir bitiş noktasının kaç kez döndürüldedildiğini anlamanıza yardımcı olan bitiş noktası düzeyinde ki aynı bilgileri de görüntüleyebilirsiniz.

Aşağıdaki örnekte Şekil 1, Trafik Yöneticisi profilinin döndürdgösterdiği tüm sorgu yanıtlarını görüntüler. 

  
![Tüm sorguların toplu görünümü](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-queries-aggregate-view.png)

*Şekil 1: Tüm sorgularda toplu görünüm*
  
Şekil 2 aynı bilgileri görüntüler, ancak uç noktalara bölünür. Sonuç olarak, belirli bir bitiş noktasının döndürüldildiği sorgu yanıtlarının hacmini görebilirsiniz.

![Trafik Yöneticisi ölçümleri - bitiş noktası başına sorgu hacminin bölünmüş görünümü](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-query-volume-per-endpoint.png)

*Şekil 2: Bitiş noktası başına gösterilen sorgu hacmi döndürülürken görünümü bölme*

## <a name="endpoint-status-by-endpoint"></a>Bitiş noktasına göre bitiş noktası durumu
Profildeki uç noktaların sistem durumu durumunu anlamak için [bu ölçüyü](../azure-monitor/platform/metrics-supported.md#microsoftnetworktrafficmanagerprofiles) kullanın. İki değer alır:
 - bitiş noktası dolduysa **1** kullanın.
 - bitiş noktası aşağıdaysa **0** kullanın.

Bu metrik, tüm ölçümlerin durumunu gösteren toplam bir değer olarak gösterilebilir (Şekil 3), veya belirli uç noktaların durumunu göstermek için bölünebilir (Şekil 4'e bakınız). Eski, toplama düzeyi **Avg**olarak seçilirse, bu ölçümün değeri tüm uç noktaların durumunun aritmetik ortalamasıdır. Örneğin, bir profilin iki uç noktası varsa ve yalnızca bir tanesi sağlıklıysa, bu ölçümün Şekil 3'te gösterildiği gibi **0,50** değeri vardır. 


![Trafik Yöneticisi ölçümleri - uç nokta durumunun bileşik görünümü](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-composite-view.png)

*Şekil 3: Uç nokta durum ölçümünün bileşik görünümü – "Avg" toplama seçildi*


![Trafik Yöneticisi ölçümleri - bitiş noktası durumunun bölünmüş görünümü](./media/traffic-manager-metrics-alerts/traffic-manager-metrics-endpoint-status-split-view.png)

*Şekil 4: Uç nokta durum ölçümlerinin bölünmüş görünümü*

Bu ölçümleri Azure [Monitor hizmetiportalı,](../azure-monitor/platform/metrics-supported.md) [REST API,](https://docs.microsoft.com/rest/api/monitor/) [Azure CLI](https://docs.microsoft.com/cli/azure/monitor)ve [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.applicationinsights)aracılığıyla veya Traffic Manager'ın portal deneyiminin ölçümler bölümünden kullanabilirsiniz.

## <a name="alerts-on-traffic-manager-metrics"></a>Trafik Yöneticisi ölçümlerinde uyarılar
Azure Monitor, Traffic Manager ölçümlerini işlemenin ve görüntülemenin yanı sıra müşterilerin bu ölçümlerle ilişkili uyarıları yapılandırmasına ve almasını sağlar. Bir uyarının oluşması için bu ölçümlerde hangi koşulların karşılanması gerektiğini, bu koşulların ne sıklıkta izlenmesi gerektiğini ve uyarıların size nasıl gönderilmesi gerektiğini seçebilirsiniz. Daha fazla bilgi için Azure [Monitor uyarıları belgelerine](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
- [Azure Monitör hizmeti](../azure-monitor/platform/metrics-supported.md) hakkında daha fazla bilgi edinin
- [Azure Monitörü'ü kullanarak grafik oluşturmayı](../azure-monitor/platform/metrics-getting-started.md#create-your-first-metric-chart) öğrenin
