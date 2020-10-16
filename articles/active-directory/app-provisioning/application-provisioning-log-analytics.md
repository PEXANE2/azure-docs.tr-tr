---
title: Sağlamanın Azure Active Directory Azure Izleyici günlükleri ile nasıl tümleştirildiğini anlayın.
description: Sağlamanın Azure Active Directory Azure Izleyici günlükleri ile nasıl tümleştirildiğini anlayın.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-provisioning
ms.topic: conceptual
ms.workload: identity
ms.date: 10/12/2020
ms.author: kenwith
ms.reviewer: arvinh,luleon
ms.openlocfilehash: 4e14bd6365ce53d98d6e0b0d1f2601ff3b3e59b4
ms.sourcegitcommit: 83610f637914f09d2a87b98ae7a6ae92122a02f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91997314"
---
# <a name="understand-how-provisioning-integrates-with-azure-monitor-logs"></a>Sağlamanın Azure Izleyici günlükleriyle nasıl tümleştirildiğini anlama

Sağlama, Azure Izleyici günlükleri ve Log Analytics ile tümleşir. Azure Izleme ile, pano olarak da bilinen çalışma kitaplarını oluşturma, 30 ila gün için sağlama günlüklerini depolama ve özel sorgular ve uyarılar oluşturma gibi işlemleri yapabilirsiniz. Bu makalede, sağlama günlüklerinin Azure Izleyici günlükleriyle nasıl tümleştirileceği açıklanmaktadır. Sağlama günlüklerinin genel olarak nasıl çalıştığı hakkında daha fazla bilgi edinmek için bkz. [sağlama günlükleri](../reports-monitoring/concept-provisioning-logs.md).

## <a name="enabling-provisioning-logs"></a>Sağlama günlüklerinin etkinleştirilmesi

Azure Izleme ve Log Analytics hakkında zaten bilgi sahibi olmanız gerekir. Aksi takdirde, hakkında bilgi edinmek için üzerine atlayın ve ardından uygulama sağlama günlükleri hakkında bilgi edinmek için geri dönün. Azure Izleme hakkında daha fazla bilgi edinmek için bkz. [Azure izleyiciye genel bakış](../../azure-monitor/overview.md). Azure Izleyici günlükleri ve Log Analytics hakkında daha fazla bilgi edinmek için bkz. [Azure izleyici 'de günlük sorgularına genel bakış](../../azure-monitor/log-query/log-query-overview.md).

Azure Izleme üzerinde yapılandırıldıktan sonra, uygulama sağlama için günlükleri etkinleştirebilirsiniz. Seçeneği **Tanılama ayarları** sayfasında bulunur.

:::image type="content" source="media/application-provisioning-log-analytics/diagnostic-settings.png" alt-text="Erişim Tanılama ayarları" lightbox="media/application-provisioning-log-analytics/diagnostic-settings.png":::

:::image type="content" source="media/application-provisioning-log-analytics/enable-log-analytics.png" alt-text="Erişim Tanılama ayarları" lightbox="media/application-provisioning-log-analytics/enable-log-analytics.png":::

> [!NOTE]
> Yeni bir çalışma alanı sağladıysanız, günlüğe Günlükler gönderebilmeniz için biraz zaman alabilir. Aboneliğin *Microsoft. Insights* 'ı kullanmak için kayıtlı olmadığını belirten bir hata alırsanız, birkaç dakika sonra tekrar kontrol edin.
 
## <a name="understanding-the-data"></a>Verileri anlama
Kaynak görüntüleyicilerinin gönderdiği temel alınan veri akışı neredeyse aynıdır. Azure Izleyici günlükleri Azure portal UI ve Azure API 'SI ile neredeyse aynı akışı alır. Aşağıdaki tabloda özetlenen günlük alanlarında yalnızca birkaç **fark** vardır. Bu alanlar hakkında daha fazla bilgi edinmek için bkz. [provisioningObjectSummary listeleme](https://docs.microsoft.com/graph/api/provisioningobjectsummary-list?view=graph-rest-beta&tabs=http&preserve-view=true).

|Azure İzleyici günlükleri   |Azure portal Kullanıcı arabirimi   |Azure API 'SI |
|----------|-----------|------------|
|errorDescription |reason |resultDescription |
|durum |resultType |resultType |
|activityDateTime |TimeGenerated |TimeGenerated |


## <a name="azure-monitor-workbooks"></a>Azure Izleyici çalışma kitapları

Azure Izleyici çalışma kitapları, veri analizi için esnek bir tuval sağlar. Ayrıca, Azure portal içinde zengin görsel raporların oluşturulmasını sağlar. Daha fazla bilgi için bkz. [Azure Izleyici çalışma kitaplarına genel bakış](../../azure-monitor/platform/workbooks-overview.md).

Uygulama sağlama, önceden oluşturulmuş bir çalışma kitapları kümesiyle gelir. Bunları çalışma kitapları sayfasında bulabilirsiniz. Verileri görüntülemek için tüm filtrelerin (timeRange, JobId, appName) doldurulduğundan emin olmanız gerekir. Ayrıca, bir uygulamayı sağladığınızdan emin olmanız gerekir, aksi takdirde günlüklerde hiç veri olmayacaktır.

:::image type="content" source="media/application-provisioning-log-analytics/workbooks.png" alt-text="Erişim Tanılama ayarları" lightbox="media/application-provisioning-log-analytics/workbooks.png":::

:::image type="content" source="media/application-provisioning-log-analytics/report.png" alt-text="Erişim Tanılama ayarları" lightbox="media/application-provisioning-log-analytics/report.png":::

## <a name="custom-queries"></a>Özel sorgular

Özel sorgular oluşturabilir ve verileri Azure panolarında gösterebilirsiniz. Hakkında bilgi edinmek için bkz. [Log Analytics veri panoları oluşturma ve paylaşma](../../azure-monitor/log-query/get-started-queries.md). Ayrıca, [Azure izleyici 'de günlük sorgularına genel bakış](../../azure-monitor/log-query/log-query-overview.md)' ı kullanıma aldığınızdan emin olun.

Uygulama sağlamayı kullanmaya başlamak için bazı örnekler aşağıda verilmiştir.

Kaynak sistemdeki KIMLIĞINI temel alarak bir kullanıcıya ait günlükleri sorgulayın:
```kusto
AADProvisioningLogs
| extend SourceIdentity = parse_json(SourceIdentity)
| where tostring(SourceIdentity.Id) == "49a4974bb-5011-415d-b9b8-78caa7024f9a"
```

Hata kodu başına özetleme sayısı:
```kusto
AADProvisioningLogs
| summarize count() by ErrorCode = ResultSignature
```

Her gün olay sayısını eyleme göre özetle:
```kusto
AADProvisioningLogs
| where TimeGenerated > ago(7d)
| summarize count() by Action, bin(TimeGenerated, 1d)
```

100 olay ve proje anahtarı özellikleri al:
```kusto
AADProvisioningLogs
| extend SourceIdentity = parse_json(SourceIdentity)
| extend TargetIdentity = parse_json(TargetIdentity)
| extend ServicePrincipal = parse_json(ServicePrincipal)
| where tostring(SourceIdentity.identityType) == "Group"
| project tostring(ServicePrincipal.Id), tostring(ServicePrincipal.Name), ModifiedProperties, JobId, Id, CycleId, ChangeId, Action, SourceIdentity.identityType, SourceIdentity.details, TargetIdentity.identityType, TargetIdentity.details, ProvisioningSteps
|take 100
```

## <a name="custom-alerts"></a>Özel uyarılar

Azure Izleyici, sağlama ile ilgili önemli olaylar hakkında bildirim alabilmeniz için özel uyarılar yapılandırmanıza olanak tanır. Örneğin, hatalarda ani artışlar hakkında bir uyarı almak isteyebilirsiniz. Veya belki de devre dışı bırakır veya siler. Uyarı almak isteyebileceğiniz diğer bir örnek, bir şeyin yanlış olduğunu belirten herhangi bir sağlama olmamasıdır.

Uyarılar hakkında daha fazla bilgi edinmek için bkz. [Azure Izleyici uyarıları ile olaylara yanıt verme](../../azure-monitor/learn/tutorial-response.md).

Hatalarda ani bir artış olduğunda uyarır. JobId öğesini uygulamanızın iş kimliği ile değiştirin.

:::image type="content" source="media/application-provisioning-log-analytics/alert1.png" alt-text="Erişim Tanılama ayarları" lightbox="media/application-provisioning-log-analytics/alert1.png":::

Sağlama hizmetinin çalışmayı durdurmasına neden olan bir sorun olabilir. Belirli bir zaman aralığı boyunca hiçbir sağlama olayı olmadığını algılamak için aşağıdaki uyarıyı kullanın.

:::image type="content" source="media/application-provisioning-log-analytics/alert2.png" alt-text="Erişim Tanılama ayarları" lightbox="media/application-provisioning-log-analytics/alert2.png":::

Devre dışı bırakıldığında veya silindiğinde uyarır.

:::image type="content" source="media/application-provisioning-log-analytics/alert3.png" alt-text="Erişim Tanılama ayarları" lightbox="media/application-provisioning-log-analytics/alert3.png":::


## <a name="community-contributions"></a>Topluluk katkıları

Uygulama sağlama sorgularına ve panolarına yönelik açık kaynaklı ve topluluk tabanlı bir yaklaşım sunuyoruz. Başkalarının yararlı bulabileceğini düşündüğünüz bir sorgu, uyarı veya çalışma kitabı oluşturduysanız, [AzureMonitorCommunity GitHub](https://github.com/microsoft/AzureMonitorCommunity)deposunda yayımlamayı unutmayın. Ardından bağlantı içeren bir e-posta gönderin. Başkalarının da avantajlarından faydalanabilmesi için gözden geçireceğiz ve bu hizmette yayımlayacağız. Bizimle iletişime geçin provisioningfeedback@microsoft.com .

## <a name="next-steps"></a>Sonraki adımlar

- [Log Analytics](../reports-monitoring/howto-analyze-activity-logs-log-analytics.md)
- [Azure Izleyici günlüklerinde sorguları kullanmaya başlama](../../azure-monitor/log-query/get-started-queries.md)
- [Azure portal uyarı grupları oluşturma ve yönetme](../../azure-monitor/platform/action-groups.md)
- [Azure Active Directory için Log Analytics görünümlerini yükleyip kullanın](../reports-monitoring/howto-install-use-log-analytics-views.md)
- [Sağlama günlükleri API 'SI](https://docs.microsoft.com/graph/api/resources/provisioningobjectsummary?view=graph-rest-beta.md&preserve-view=true)
