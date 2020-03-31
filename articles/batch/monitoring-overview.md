---
title: Azure Toplu İşi İzle | Microsoft Dokümanlar
description: Azure Toplu İş için Azure izleme hizmetleri, ölçümler, tanılama günlükleri ve diğer izleme özellikleri hakkında bilgi edinin.
services: batch
author: LauraBrenner
manager: evansma
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.workload: na
ms.date: 04/05/2018
ms.author: labrenne
ms.openlocfilehash: d251229c522bd4d6daca894513eaae14d244d8a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77025869"
---
# <a name="monitor-batch-solutions"></a>Batch çözümlerini izleme

Azure ve Toplu İşlem hizmeti, Toplu İşlem çözümlerinizi izlemek için çeşitli hizmetler, araçlar ve API'ler sağlar. Bu genel bakış makalesi, gereksinimlerinize uygun bir izleme yaklaşımı seçmenize yardımcı olur.

Azure kaynaklarını izlemek için kullanılabilen Azure bileşenlerine [ve](../monitoring-and-diagnostics/monitoring-overview.md)hizmetlerine genel bakış için bkz.

## <a name="subscription-level-monitoring"></a>Abonelik düzeyinde izleme

Toplu Iş hesaplarını içeren abonelik düzeyinde, [Azure etkinlik günlüğü](../azure-monitor/platform/platform-logs-overview.md) çeşitli [kategorilerde](../azure-monitor/platform/activity-log-view.md#categories-in-the-activity-log)operasyonel olay verileri toplar.

Özellikle Toplu İşlem hesapları için, etkinlik günlüğü hesap oluşturma, silme ve anahtar yönetimiyle ilgili olayları toplar.

Etkinlik günlüğünüzden olayları almanın bir yolu Azure portalını kullanmaktır. **Tüm hizmetler** > **Etkinlik Günlüğü'ne**tıklayın. Veya Azure CLI, PowerShell cmdlets veya Azure Monitor REST API'sini kullanarak olayları sorgula. Ayrıca etkinlik günlüğünü dışa aktarabilir veya [etkinlik günlüğü uyarılarını](../monitoring-and-diagnostics/monitoring-activity-log-alerts-new-experience.md)yapılandırabilirsiniz.

## <a name="batch-account-level-monitoring"></a>Toplu hesap düzeyi izleme

[Azure Monitor](../azure-monitor/overview.md)özelliklerini kullanarak her Toplu İş hesabını izleyin. Azure Monitor, [havuzlar,](../azure-monitor/platform/data-platform-metrics.md) işler ve görevler gibi Toplu Iş hesabı düzeyinde kapsama giren kaynaklar için ölçümler ve isteğe bağlı [tanılama günlükleri](../azure-monitor/platform/platform-logs-overview.md) toplar. Toplu Iş hesabınızdaki etkinlikleri izlemek ve sorunları tanılamak için bu verileri el ile veya programlı olarak toplayın ve tüketin. Ayrıntılar için, [tanılama değerlendirme ve izleme için Toplu iş ölçümleri, uyarılar ve günlüklere](batch-diagnostics.md)bakın.
 
> [!NOTE]
> Ölçümler, ek yapılandırma olmadan Toplu İşlem hesabınızda varsayılan olarak kullanılabilir ve 30 günlük işlem geçmişi vardır. Toplu Iş hesabı için tanısal günlüğe kaydetmeyi etkinleştirmeniz gerekir ve tanılama günlüğü verilerini depolamak veya işlemek için ek maliyetlere maruz kalabilirsiniz. 

## <a name="batch-resource-monitoring"></a>Toplu kaynak izleme

Toplu İşlem uygulamalarınızda, işler, görevler, düğümler ve havuzlar da dahil olmak üzere kaynaklarınızın durumunu izlemek veya sorgulamak için Toplu İş API'lerini kullanın. Örnek:

* [Görevleri sayve düğümleri duruma göre hesaplama](batch-get-resource-counts.md)
* [Toplu iş kaynaklarını verimli bir şekilde listelemek için sorgular oluşturma](batch-efficient-list-queries.md)
* [Görev bağımlılıkları oluşturma](batch-task-dependencies.md)
* İş [yöneticisi görevini](/rest/api/batchservice/job/add#jobmanagertask) kullanma
* Görev [durumunu](/rest/api/batchservice/task/list#taskstate) izleme
* Düğüm [durumunu](/rest/api/batchservice/computenode/list#computenodestate) izleme
* Havuz [durumunu](/rest/api/batchservice/pool/get#poolstate) izleme
* [Hesaptaki havuz kullanımını](/rest/api/batchservice/pool/listusagemetrics) izleme
* [Havuz düğümlerini duruma göre say](/rest/api/batchservice/account/listpoolnodecounts)

## <a name="vm-performance-counters-and-application-monitoring"></a>VM performans sayaçları ve uygulama izleme

* [Application Insights,](../azure-monitor/app/app-insights-overview.md) Toplu Iş ve görevlerinizin kullanılabilirliğini, performansını ve kullanımını programlı bir şekilde izlemek için kullanabileceğiniz bir Azure hizmetidir. İşlem düğümlerinden (VM'lerden) performans sayaçlarını ve VM'lerden görevler için özel bilgileri kolayca alın. 

  Örneğin, [Bkz. Uygulama Öngörüleri ve](monitor-application-insights.md) beraberindeki kod [örneği](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ApplicationInsights)ile bir Toplu İşlem .NET uygulamasını izleyin ve hata ayıklayın.

  > [!NOTE]
  > Application Insights'ı kullanmak için ek ücrete tabi olabilirsiniz. Fiyatlandırma [seçeneklerine](https://azure.microsoft.com/pricing/details/application-insights/)bakın. 
  >

* [Toplu İşlem Gezgini,](https://github.com/Azure/BatchExplorer) Azure Toplu İş uygulamaları nın oluşturulmasına, hata ayıklanmasına ve izlenmesine yardımcı olan ücretsiz, zengin özellikli, bağımsız bir istemci aracıdır. Mac, Linux veya Windows için [yükleme paketi](https://azure.github.io/BatchExplorer/) indirebilirsiniz. Toplu İşlem çözümünüzü, Toplu İş Explorer'da VM performans sayaçları gibi [Application Insights verilerini görüntülemek](https://github.com/Azure/batch-insights) için yapılandırın.


## <a name="next-steps"></a>Sonraki adımlar

* Batch çözümleri oluşturmak için kullanılabilen [Batch API’leri ve araçları](batch-apis-tools.md) hakkında bilgi alın.
* Toplu İşlem ile [tanısal günlüğe kaydetme](batch-diagnostics.md) hakkında daha fazla bilgi edinin.