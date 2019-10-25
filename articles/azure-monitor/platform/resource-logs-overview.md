---
title: Azure Kaynak günlüklerine genel bakış | Microsoft Docs
description: Azure Kaynak günlükleri için desteklenen Hizmetleri ve olay şemasını anlayın.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: reference
author: rboucher
ms.author: robb
ms.date: 10/22/2019
ms.openlocfilehash: e1bf6a55568671ddb8f6999356cc9be43ce6a728
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72804015"
---
# <a name="azure-resource-logs-overview"></a>Azure Kaynak günlüklerine genel bakış
Azure Kaynak günlükleri, iç işlemlerini tanımlayan Azure kaynakları tarafından yayılan [Platform günlüklerdir](platform-logs-overview.md) . Tüm kaynak günlükleri, ortak bir üst düzey şemayı, her hizmet için her bir hizmetin esnekliğiyle paylaşarak, kendi olayları için benzersiz özellikler sunar.

> [!NOTE]
> Kaynak günlükleri daha önce tanılama günlükleri olarak bilinirdi.

## <a name="collecting-resource-logs"></a>Kaynak günlüklerini toplama
Kaynak günlükleri desteklenen Azure kaynakları tarafından otomatik olarak oluşturulur, ancak bunları bir [Tanılama ayarı](diagnostic-settings.md)kullanarak yapılandırmadığınız müddetçe toplanmaz. Günlükleri aşağıdaki hedeflere iletmek için her bir Azure kaynağı için bir tanılama ayarı oluşturun:

| Hedef | Senaryo |
|:---|:---|:---|
| [Log Analytics çalışma alanı](resource-logs-collect-storage.md) | Günlükleri diğer izleme verileriyle çözümleyin ve günlük sorguları ve günlük uyarıları gibi Azure Izleyici özelliklerinden yararlanın. |
| [Azure depolama alanı](archive-diagnostic-logs.md) | Denetim veya yedekleme için günlükleri arşivle. |
| [Olay Hub 'ı](resource-logs-stream-event-hubs.md) | Günlükleri üçüncü taraf günlüğe kaydetme ve telemetri sistemlerine akışa alma.  |

## <a name="compute-resources"></a>İşlem kaynakları
Kaynak günlükleri, Azure işlem kaynaklarındaki Konuk işletim sistemi düzeyi günlüklerinden farklıdır. İşlem kaynakları, olay günlükleri, syslog ve performans sayaçları gibi veriler de dahil olmak üzere Konuk IŞLETIM sisteminden günlükleri ve ölçümleri toplamak için bir aracı gerektirir. Azure sanal makineler ve [Log Analytics aracılarından](agents-overview.md#log-analytics-agent) günlük verilerinin yönlendirilmesini sağlamak Için [Tanılama uzantısını](agents-overview.md#azure-diagnostic-extension) kullanın ve Azure 'daki sanal makinelerden, diğer bulutlarda ve şirket içinde bir Log Analytics çalışma alanına Günlükler ve ölçümler toplayın. Ayrıntılar için bkz. [Azure izleyici için izleme verileri kaynakları](data-sources.md) .

## <a name="resource-logs-schema"></a>Kaynak günlükleri şeması
Kaynak günlükleri şeması ve kategorileri hakkında daha fazla bilgi için bkz. [kaynak günlüğü şeması](diagnostic-logs-schema.md). 

## <a name="next-steps"></a>Sonraki Adımlar

* Azure 'ı izlemek için kullanabileceğiniz [diğer Azure platformu günlükleri hakkında daha fazla bilgi edinin](platform-logs-overview.md) .
