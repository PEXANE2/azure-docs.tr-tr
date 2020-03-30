---
title: Azure'da izleme çözümleri envanteri | Microsoft Dokümanlar
description: Azure Monitor'daki izleme çözümleri, belirli bir sorun alanı etrafında döndürülen ölçümler sağlayan bir mantık, görselleştirme ve veri toplama kuralları koleksiyonudur.  Bu makalede, Microsoft'un kullanabileceği izleme çözümlerinin bir listesi ve bunların yöntemi ve veri toplama sıklığı hakkında ayrıntılar yer almaktadır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/26/2018
ms.openlocfilehash: 7b88d957bce45bf518fc77584f1691de8010459a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77663139"
---
# <a name="inventory-and-data-collection-details-for-monitoring-solutions-in-azure"></a>Azure'da çözümleri izlemek için envanter ve veri toplama ayrıntıları
[İzleme çözümleri,](solutions.md) belirli bir uygulamanın veya hizmetin işleyişi hakkında ek bilgiler sağlamak için Azure'daki hizmetlerden yararlanır. İzleme çözümleri genellikle günlük verilerini toplar ve toplanan verileri çözümlemek için sorgular ve görünümler sağlar. Kullandığınız tüm uygulamalar ve hizmetler için Azure Monitor'a izleme çözümleri ekleyebilirsiniz. Bunlar genellikle hiçbir ücret ödemeden kullanılabilir, ancak kullanım ücretleri çağırabilecek verileri toplar.

Bu makalede, ayrıntılı belgelerine bağlantılar içeren Microsoft'un kullanabileceği [montioring çözümlerinin](solutions.md) bir listesi yer almaktadır.  Ayrıca Azure Monitor'a veri toplama yöntemi ve sıklığı hakkında da bilgi sağlar.  Bu makaledeki bilgileri, mevcut farklı çözümleri tanımlamak ve farklı izleme çözümleri için veri akışını ve bağlantı gereksinimlerini anlamak için kullanabilirsiniz.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="list-of-monitoring-solutions"></a>İzleme çözümleri listesi

Aşağıdaki tabloda, Microsoft tarafından sağlanan [Azure'daki izleme çözümleri](solutions.md) listelenmektedir. Sütundaki bir giriş, çözümün bu yöntemi kullanarak Azure Monitor'da veri topladığı anlamına gelir.  Bir çözümde sütun yoksa, başka bir Azure hizmetinden doğrudan Azure Monitor'a yazar. Daha fazla bilgi için her biri için ayrıntılı belgelere bağlantıyı izleyin.

Sütunların açıklamaları aşağıdaki gibidir:

- **Microsoft izleme aracısı** - SCOM'dan yönetim paketini çalıştırmak ve Azure'dan izleme çözümleri çalıştırmak için Windows ve Linux'ta kullanılan aracı. Bu yapılandırmada aracı, bir İşlem Yöneticisi yönetim grubuna bağlanmadan doğrudan Azure Monitor'a bağlanır. 
- **Operations Manager** - Microsoft izleme aracısı olarak özdeş aracı. Bu yapılandırmada, Azure Monitor'a bağlı [bir Operasyon Yöneticisi yönetim grubuna bağlanır.](../platform/om-agents.md) 
-  **Azure Depolama** - Solution, bir Azure depolama hesabından veri toplar. 
- **Operasyon Yöneticisi gerekli mi?** - İzleme çözümü tarafından veri toplanması için bağlı bir Operasyon Yöneticisi yönetim grubu gereklidir. 
- **Yönetim grubu aracılığıyla gönderilen Operasyon Yöneticisi aracı verileri** - Aracı bir [SCOM yönetim grubuna bağlıysa,](../platform/om-agents.md)veriler yönetim sunucusundan Azure Monitor'a gönderilir. Bu durumda, aracının doğrudan Azure Monitor'a bağlanması gerekmez. Bu kutu seçili değilse, aracı bir SCOM yönetim grubuna bağlı olsa bile veriler aracıdan doğrudan Azure Monitor'a gönderilir. [Log Analytics ağ geçidi](../platform/gateway.md)üzerinden Azure Monitor ile iletişim kurabilmesi gerekir.
- **Toplama sıklığı** - Verilerin izleme çözümü tarafından toplanma sıklığını belirtir. 



| **İzleme çözümü** | **Platform** | **Microsoft izleme aracısı** | **Operations Manager aracısı** | **Azure depolama** | **Operasyon Yöneticisi gerekli mi?** | **Yönetim grubu aracılığıyla gönderilen Operasyon Yöneticisi aracı verileri** | **Toplama sıklığı** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [Etkinlik Günlüğü analitiği](../platform/activity-log-collect.md) | Azure | | | | | | bildirimde |
| [AD Değerlendirmesi](ad-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 gün |
| [AD Çoğaltma Durumu](ad-replication-status.md) |Windows |&#8226; |&#8226; | | |&#8226; |5 gün |
| [Ajan Sağlık](solution-agenthealth.md) | Windows ve Linux | &#8226; | &#8226; | | | &#8226; | 1 dakika |
| [Uyarı Yönetimi](../platform/alert-management-solution.md) (Nagios) |Linux |&#8226; | | | | |varışta |
| [Uyarı Yönetimi](../platform/alert-management-solution.md) (Zabbix) |Linux |&#8226; | | | | |1 dakika |
| [Uyarı Yönetimi](../platform/alert-management-solution.md) (Operasyon Yöneticisi) |Windows | |&#8226; | |&#8226; |&#8226; |3 dakika |
| [Azure Site Kurtarma](../../site-recovery/site-recovery-overview.md) | Azure | | | | | | yok |
| [Uygulama Öngörüleri Bağlayıcısı (Amortismana Uğradı)](../platform/app-insights-connector.md) | Azure | | | |  |  | bildirimde |
| [Otomasyon Hibrid İşçi](../../automation/automation-hybrid-runbook-worker.md) | Windows | &#8226; | &#8226; |  |  |  | yok |
| [Azure Uygulama Ağ Geçidi Analitiği](azure-networking-analytics.md) | Azure |  |  |  |  |  | bildirimde |
| **İzleme çözümü** | **Platform** | **Microsoft izleme aracısı** | **Operations Manager aracısı** | **Azure depolama** | **Operasyon Yöneticisi gerekli mi?** | **Yönetim grubu aracılığıyla gönderilen Operasyon Yöneticisi aracı verileri** | **Toplama sıklığı** |
| [Azure Ağ Güvenliği Grubu Analizi (Amortismana Uğradı)](azure-networking-analytics.md) | Azure |  |  |  |  |  | bildirimde |
| [Azure SQL Analytics (Önizleme)](azure-sql.md) | Windows | | | | | | 1 dakika |
| [Yedekleme](https://azure.microsoft.com/resources/templates/101-backup-oms-monitoring/) | Azure |  |  |  |  |  | bildirimde |
| [Kapasite ve Performans (Önizleme)](capacity-performance.md) |Windows |&#8226; |&#8226; | | |&#8226; |varışta |
| [Değişiklik İzleme](../../automation/change-tracking.md) |Windows |&#8226; |&#8226; | | |&#8226; |[Değişir](../../automation/change-tracking.md#change-tracking-data-collection-details) |
| [Değişiklik İzleme](../../automation/change-tracking.md) |Linux |&#8226; | | | | |[Değişir](../../automation/change-tracking.md#change-tracking-data-collection-details) |
| [Kapsayıcılar](containers.md) | Windows ve Linux | &#8226; | &#8226; |  |  |  | 3 dakika |
| [Anahtar Kasası Analizi](azure-key-vault.md) |Windows | | | | | |bildirimde |
| [Kötü Amaçlı Yazılım Değerlendirmesi](../../security-center/security-center-install-endpoint-protection.md) |Windows |&#8226; |&#8226; | | |&#8226; |Saatlik |
| [Ağ Performansı İzleyicisi](network-performance-monitor.md) | Windows | &#8226; | &#8226; |  |  |  | TCP her 5 saniyede bir el sıkışıyor, veriler her 3 dakikada bir gönderildi |
| [Office 365 Analytics (Önizleme)](solution-office-365.md) |Windows | | | | | |bildirimde |
| **İzleme çözümü** | **Platform** | **Microsoft izleme aracısı** | **Operations Manager aracısı** | **Azure depolama** | **Operasyon Yöneticisi gerekli mi?** | **Yönetim grubu aracılığıyla gönderilen Operasyon Yöneticisi aracı verileri** | **Toplama sıklığı** |
| [Service Fabric Analizi](../../service-fabric/service-fabric-diagnostics-oms-setup.md) |Windows | | |&#8226; | | |5 dakika |
| [Hizmet Eşlemesi](service-map.md) | Windows ve Linux | &#8226; | &#8226; |  |  |  | 15 saniye |
| [SQL Değerlendirmesi](sql-assessment.md) |Windows |&#8226; |&#8226; | | |&#8226; |7 gün |
| [SurfaceHub](surface-hubs.md) |Windows |&#8226; | | | | |varışta |
| [Sistem Merkezi Operasyon Yöneticisi Değerlendirmesi (Önizleme)](scom-assessment.md) | Windows | &#8226; | &#8226; |  |  | &#8226; | yedi gün |
| [Güncelleme Yönetimi](../../automation/automation-update-management.md) | Windows |&#8226; |&#8226; | | |&#8226; |günde en az 2 kez ve bir güncelleştirme yükledikten sonra 15 dakika |
| [Yükseltme Hazırlığı](https://docs.microsoft.com/windows/deployment/upgrade/upgrade-readiness-get-started) | Windows | &#8226; |  |  |  |  | 2 gün |
| [VMware İzleme (Amortismana Uğradı)](vmware.md) | Linux | &#8226; |  |  |  |  | 3 dakika |
| [Tel Veri 2.0 (Önizleme)](wire-data.md) |Windows (2012 R2 / 8.1 veya sonrası) |&#8226; |&#8226; | | | | 1 dakika |




## <a name="next-steps"></a>Sonraki adımlar
* İzleme çözümlerini nasıl [yükleyip kullanacağınızı](solutions.md)öğrenin.
* İzleme çözümleri yle toplanan verileri çözümlemek için [sorguoluşturmayı](../log-query/log-query-overview.md) öğrenin.
