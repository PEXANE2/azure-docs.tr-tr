---
title: Azure Monitor'daki etkinlikler için IIS için blob depolama ve tablo depolama yı kullanma | Microsoft Dokümanlar
description: Azure Monitor, tablo depolamasına tanılama yazan Azure hizmetlerinin günlüklerini veya blob depolamaya yazılmış IIS günlüklerini okuyabilir.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/14/2020
ms.openlocfilehash: 44368ab90abd189c6a8a0792494828c87142eb20
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77672404"
---
# <a name="collect-data-from-azure-diagnostics-extension-to-azure-monitor-logs"></a>Azure Tanılama uzantısından Azure Monitör Günlükleri'ne veri toplama
Azure tanılama uzantısı, [Azure Monitor'da](agents-overview.md) sanal makineler de dahil olmak üzere Azure bilgi işlem kaynaklarının konuk işletim sisteminden izleme verileri toplayan bir aracıdır. Bu makalede, tanılama uzantısı tarafından toplanan verilerin Azure Depolama'dan Azure Monitör Günlüklerine nasıl toplandığı açıklanmaktadır.

> [!NOTE]
> Azure Monitor'daki Log Analytics aracısı genellikle konuk işletim sisteminden Azure Monitör Günlükleri'ne veri toplamak için tercih edilen yöntemdir. Aracıların ayrıntılı bir karşılaştırması için [Azure Monitor aracılarının genel görünümüne](agents-overview.md) bakın.

## <a name="supported-data-types"></a>Desteklenen veri türleri
Azure tanılama uzantısı verileri bir Azure Depolama hesabında depolar. Azure Monitör Günlükleri'nin bu verileri toplaması için aşağıdaki konumlarda olması gerekir:

| Günlük Türü | Kaynak Türü | Konum |
| --- | --- | --- |
| IIS günlükleri |Virtual Machines <br> Web rolleri <br> İşçi rolleri |wad-iis-logfiles (Blob Depolama) |
| Syslog |Virtual Machines |LinuxsyslogVer2v0 (Tablo Depolama) |
| Servis Kumaşı Operasyonel Etkinlikler |Servis Kumaş düğümleri |WADServiceFabricSystemEventTable |
| Hizmet Kumaş Güvenilir Aktör Etkinlikler |Servis Kumaş düğümleri |WADServiceFabricReliableActorEventTable |
| Servis Kumaşı Güvenilir Servis Etkinlikleri |Servis Kumaş düğümleri |WADServiceFabricReliableServiceEventTable |
| Windows Olay günlükleri |Servis Kumaş düğümleri <br> Virtual Machines <br> Web rolleri <br> İşçi rolleri |WADWindowsEventLogsTable (Tablo Depolama) |
| Windows ETW günlükleri |Servis Kumaş düğümleri <br> Virtual Machines <br> Web rolleri <br> İşçi rolleri |WADETWEventTable (Tablo Depolama) |

## <a name="data-types-not-supported"></a>Desteklenmeyen veri türleri

- Konuk işletim sisteminden performans verileri
- Azure web sitelerinden IIS günlükleri


## <a name="enable-azure-diagnostics-extension"></a>Azure tanılama uzantısını etkinleştirme
Tanılama uzantısını yükleme ve yapılandırma hakkındaki ayrıntılar için [ölçümleri ve günlükleri izlemek için](../../virtual-machines/extensions/diagnostics-linux.md) [Windows Azure tanılama uzantısını (WAD)](diagnostics-extension-windows-install.md) yükle ve yapılandırın. Bu, depolama hesabını belirtmenizi ve Azure Monitor Günlükleri'ne iletmek istediğiniz verilerin toplanmasını yapılandırmanızı sağlar.


## <a name="collect-logs-from-azure-storage"></a>Azure Depolama'dan günlükleri toplama
Bir Azure Depolama hesabından tanılama uzantısı verilerinin toplanmasını etkinleştirmek için aşağıdaki yordamı kullanın:

1. Azure **portalında, Analytics Çalışma Alanlarını Günlüğe** Kaydet'e gidin ve çalışma alanınızı seçin.
1. Menünün Çalışma Alanı **Veri Kaynakları** bölümünde Depolama **hesapları günlüklerini** tıklatın.
2. **Ekle'yi**tıklatın.
3. Toplanacak verileri içeren **Depolama hesabını** seçin.
4. Toplamak istediğiniz **Veri Türünü** seçin.
5. Kaynak değeri, veri türüne göre otomatik olarak doldurulur.
6. Yapılandırmayı kaydetmek için **Tamam'ı** tıklatın.
7. Ek veri türleri için yineleyin.

Yaklaşık 30 dakika içinde, Log Analytics çalışma alanında depolama hesabından verileri görebilirsiniz. Yalnızca yapılandırma uygulandıktan sonra depolamaya yazılan verileri görürsünüz. Çalışma alanı, depolama hesabından önceden varolan verileri okumaz.

> [!NOTE]
> Portal, kaynağın depolama hesabında var olduğunu veya yeni verilerin yazıldığını doğrulamaz.



## <a name="next-steps"></a>Sonraki adımlar

* Desteklenen Azure hizmetleri [için Azure hizmetleri için günlükleri ve ölçümleri toplayın.](collect-azure-metrics-logs.md)
* Verilere ilişkin bilgi sağlamak için [Çözümleri etkinleştirin.](../../azure-monitor/insights/solutions.md)
* Verileri çözümlemek için [arama sorgularını kullanın.](../../azure-monitor/log-query/log-query-overview.md)
