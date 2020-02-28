---
title: Azure Izleyici 'de IIS ve tablo depolaması için blob depolamayı kullanma | Microsoft Docs
description: Azure Izleyici, tablo depolama veya blob depolamaya yazılan IIS günlüklerine tanılama yazan Azure hizmetleri için günlükleri okuyabilir.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/14/2020
ms.openlocfilehash: 44368ab90abd189c6a8a0792494828c87142eb20
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77672404"
---
# <a name="collect-data-from-azure-diagnostics-extension-to-azure-monitor-logs"></a>Azure tanılama uzantılarından Azure Izleyici günlüklerine veri toplama
Azure tanılama uzantısı, Azure [izleyici 'de](agents-overview.md) sanal makineler dahil olmak üzere Azure işlem kaynaklarının Konuk işletim sisteminden izleme verilerini toplayan bir aracıdır. Bu makalede, Azure depolama 'dan Azure Izleyici günlüklerine tanılama uzantısı tarafından toplanan verilerin nasıl toplanacağı açıklanır.

> [!NOTE]
> Azure Izleyici 'deki Log Analytics Aracısı genellikle Konuk işletim sisteminden Azure Izleyici günlüklerine veri toplamak için tercih edilen yöntemdir. Aracıların ayrıntılı bir karşılaştırması için bkz. [Azure izleyici aracılarına genel bakış](agents-overview.md) .

## <a name="supported-data-types"></a>Desteklenen veri türleri
Azure tanılama uzantısı, verileri bir Azure depolama hesabında depolar. Bu verileri toplamak için Azure Izleyici günlüklerinin aşağıdaki konumlarda olması gerekir:

| Günlük türü | Kaynak Türü | Konum |
| --- | --- | --- |
| IIS günlükleri |Virtual Machines <br> Web rolleri <br> Çalışan rolleri |wad-IIS-logfiles (Blob Depolama) |
| Syslog |Virtual Machines |LinuxsyslogVer2v0 (Tablo depolama) |
| Service Fabric çalışma olayları |Service Fabric düğümleri |WADServiceFabricSystemEventTable |
| Service Fabric güvenilir aktör olayları |Service Fabric düğümleri |WADServiceFabricReliableActorEventTable |
| Service Fabric Reliable Services olayları |Service Fabric düğümleri |WADServiceFabricReliableServiceEventTable |
| Windows Olay günlükleri |Service Fabric düğümleri <br> Virtual Machines <br> Web rolleri <br> Çalışan rolleri |WADWindowsEventLogsTable (Tablo depolama) |
| Windows ETW günlükleri |Service Fabric düğümleri <br> Virtual Machines <br> Web rolleri <br> Çalışan rolleri |WADETWEventTable (Tablo depolama) |

## <a name="data-types-not-supported"></a>Veri türleri desteklenmiyor

- Konuk işletim sistemindeki performans verileri
- Azure Web sitelerinden IIS günlükleri


## <a name="enable-azure-diagnostics-extension"></a>Azure tanılama uzantısını etkinleştirme
Tanılama uzantısını yükleme ve yapılandırma hakkında ayrıntılı bilgi için bkz. [Windows Azure tanılama uzantısı 'nı (WAD) yükleme ve yapılandırma](diagnostics-extension-windows-install.md) veya [Linux Tanılama uzantısı 'nı kullanma](../../virtual-machines/extensions/diagnostics-linux.md) . Bu, depolama hesabını belirtmenizi ve Azure Izleyici günlüklerine iletmek istediğiniz verilerin toplanmasını yapılandırmanızı sağlar.


## <a name="collect-logs-from-azure-storage"></a>Azure depolama 'dan günlükleri topla
Azure depolama hesabından tanılama uzantısı verileri toplamayı etkinleştirmek için aşağıdaki yordamı kullanın:

1. Azure portal, **Log Analytics çalışma alanları** ' na gidin ve çalışma alanınızı seçin.
1. Menüdeki **çalışma alanı veri kaynakları** bölümünde **depolama hesapları günlükleri** ' ne tıklayın.
2. **Ekle**' ye tıklayın.
3. Toplanacak verileri içeren **Depolama hesabını** seçin.
4. Toplamak istediğiniz **veri türünü** seçin.
5. Kaynak değeri veri türüne göre otomatik olarak doldurulur.
6. Yapılandırmayı kaydetmek için **Tamam** ' ı tıklatın.
7. Ek veri türleri için tekrarlayın.

Yaklaşık 30 dakika içinde, Log Analytics çalışma alanındaki depolama hesabından verileri görebilirsiniz. Yalnızca yapılandırma uygulandıktan sonra depolama alanına yazılan verileri görürsünüz. Çalışma alanı, depolama hesabından önceden var olan verileri okuyamıyor.

> [!NOTE]
> Portal, kaynağın depolama hesabında mevcut olduğunu veya yeni verilerin yazılmakta olduğunu doğrulamaz.



## <a name="next-steps"></a>Sonraki adımlar

* Desteklenen Azure hizmetleri için [Azure hizmetleri için günlükleri ve ölçümleri toplayın](collect-azure-metrics-logs.md) .
* Verilerle ilgili Öngörüler sağlamak için [çözümleri etkinleştirin](../../azure-monitor/insights/solutions.md) .
* Verileri çözümlemek için [arama sorguları](../../azure-monitor/log-query/log-query-overview.md) ' nı kullanın.
