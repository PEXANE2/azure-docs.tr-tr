---
title: Azure Monitör terminolojisi güncellemeleri | Microsoft Dokümanlar
description: Azure izleme hizmetlerinde yapılan son terminoloji değişikliklerini açıklar.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/08/2019
ms.openlocfilehash: 0c5e1096b0780d2f9e50389f772e63344935012b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79274144"
---
# <a name="azure-monitor-naming-and-terminology-changes"></a>Azure Monitör adlandırma ve terminoloji değişiklikleri
Son zamanlarda Azure Monitor'da önemli değişiklikler yapıldı ve Azure müşterileri için izlemeyi kolaylaştırmak için farklı hizmetler birleştirildi. Bu makalede, Azure Monitor belgelerindeki son ad ve terminoloji değişiklikleri açıklanmaktadır.

## <a name="october-2019---diagnostic-log-to-resource-log"></a>Ekim 2019 - Kaynak Günlüğüne Tanı Günlüğü
"Tanılama günlükleri" gerçekte toplananlarla daha iyi eşleşecek şekilde "kaynak günlükleri" olarak değiştirildi. "Tanı ayarları" terimi aynı kalır.  

## <a name="february-2019---log-analytics-terminology"></a>Şubat 2019 - Log Analytics terminolojisi
Azure Monitor altında farklı hizmetlerin birleştirilmesinden sonra, Azure Monitor hizmetini ve farklı bileşenlerini daha iyi tanımlamak için belgelerimizdeki terminolojiyi değiştirerek bir sonraki adımı atıyoruz. 

### <a name="log-analytics"></a>Log Analytics
Azure Monitor günlük verileri hala bir Log Analytics çalışma alanında depolanır ve yine de aynı Log Analytics hizmeti tarafından toplanır ve analiz edilir, ancak birçok yerde _Log Analytics_ terimini _Azure Monitor günlükleri_olarak değiştiriyoruz. Bu terim Azure Monitor'daki rolünü daha iyi yansıtır ve [Azure Monitor'daki ölçümlerle](platform/data-platform-metrics.md)daha iyi tutarlılık sağlar.

Günlük _analitiği_ terimi artık öncelikle sorgu yazmak ve çalıştırmak ve günlük verilerini analiz etmek için kullanılan Azure portalındaki sayfa için geçerlidir. Metrik verileri çözümlemek için kullanılan Azure portalındaki sayfa olan [metrik](platform/metrics-charts.md)explorer'ın işlevsel eşdeğeridir.

### <a name="log-analytics-workspaces"></a>Log Analytics çalışma alanları
Azure Monitor'da günlük verilerini tutan [çalışma alanları](platform/manage-access.md) hala Log Analytics çalışma alanları olarak adlandırılır. Azure portalındaki **Log Analytics** menüsü Log **Analytics çalışma alanları** olarak yeniden adlandırıldı ve yeni çalışma alanları [oluşturduğunuz](learn/quick-create-workspace.md) ve veri kaynaklarını yapılandırdığınız yerdir. **Azure Monitor'da** günlüklerinizi ve diğer izleme verilerinizi analiz edin ve Çalışma alanınızı **Log Analytics çalışma alanlarında**yapılandırın.

### <a name="management-solutions"></a>Yönetim çözümleri
[Yönetim çözümleri,](insights/solutions.md) işlevselliklerini daha iyi açıklayan _izleme çözümlerine_yeniden adlandırılmıştır.


## <a name="august-2018---consolidation-of-monitoring-services-into-azure-monitor"></a>Ağustos 2018 - İzleme hizmetlerinin Azure Monitör'de konsolidasyonu
Log Analytics ve Application Insights, Azure kaynaklarını ve karma ortamları izlemek için tek bir entegre deneyim sağlamak için Azure Monitor'da birleştirildi. Bu hizmetlerden hiçbir işlevsellik kaldırılmadı ve kullanıcılar herhangi bir özellik kaybı veya ödün vermeden her zaman tamamladıkları senaryoları gerçekleştirebilir.

Bu hizmetlerin her biri için belgeler Azure Monitor için tek bir içerik kümesinde birleştirilmiştir. Bu, okuyucuya, birden çok içerik kümesine başvurmak yerine belirli bir izleme senaryosunun tüm içeriğini tek bir konumda bulmasında yardımcı olur. Birleştirilmiş hizmet geliştikçe, içerik daha entegre hale gelecektir.

Oturum Analizi'nin aracılar ve görünümler gibi bir parçası olarak kabul edilen diğer özellikler de Azure Monitor'un özellikleri olarak yeniden konumlandırıldı. İşlevleri, Azure portalındaki deneyimlerinde olası iyileştirmeler dışında değişmedi.


## <a name="april-2018---retirement-of-operations-management-suite-brand"></a>Nisan 2018 - Operasyon Yönetimi Paketi markasının emekliliği
Operations Management Suite (OMS), lisanslama amacıyla aşağıdaki Azure yönetim hizmetlerinin bir biri miydi:

- Application Insights
- Azure Otomasyonu
- Azure Backup
- Log Analytics
- Site Recovery

[Bu hizmetler için yeni fiyatlandırma sunulmuştur](https://azure.microsoft.com/blog/introducing-a-new-way-to-purchase-azure-monitoring-services/)ve OMS birleştirme artık yeni müşteriler için kullanılamaz. Yukarıda açıklanan Azure Monitor'da konsolidasyon dışında, OMS'nin bir parçası olan hizmetlerin hiçbiri değişmedi. 




## <a name="next-steps"></a>Sonraki adımlar

- Azure [Monitor'un](overview.md) farklı bileşenlerini ve özelliklerini açıklayan genel görünümünü okuyun.
- [OMS portalının geçişi](../log-analytics/log-analytics-oms-portal-transition.md)hakkında bilgi edinin.
