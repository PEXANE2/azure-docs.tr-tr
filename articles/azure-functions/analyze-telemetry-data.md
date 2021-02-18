---
title: Application Insights 'de Azure Işlevleri telemetrisini çözümleyin
description: Tarafından toplanan ve Azure Application Insights 'de depolanan Azure Işlevleri telemetri verilerini görüntülemeyi ve sorgulamayı öğrenin.
ms.topic: how-to
ms.date: 10/14/2020
ms.custom: contperf-fy21q2
ms.openlocfilehash: 56af6af6f4204a819c06fd97cda357dca20a19b0
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100591181"
---
# <a name="analyze-azure-functions-telemetry-in-application-insights"></a>Application Insights 'de Azure Işlevleri telemetrisini çözümleyin 

Azure Işlevleri, işlev uygulamalarınızı izlemenizi daha iyi hale getirebilmeniz için Application Insights ile tümleşir. Application Insights, uygulamanızın günlüklere yazdığı bilgiler dahil olmak üzere, işlev uygulamanız tarafından oluşturulan telemetri verilerini toplar. Application Insights tümleştirme, genellikle işlev uygulamanız oluşturulduğunda etkinleştirilir. İşlev uygulamanızda izleme anahtarı ayarlanmamışsa, önce [Application Insights tümleştirmesini etkinleştirmeniz](configure-monitoring.md#enable-application-insights-integration)gerekir. 

Varsayılan olarak, işlev uygulamanızdan toplanan veriler Application Insights depolanır. [Azure Portal](https://portal.azure.com), Application Insights telemetri verilerinizin kapsamlı bir görselleştirme kümesini sağlar. Hata günlükleri ve sorgu olayları ve ölçümleri detaylarına gidebilirsiniz. Bu makalede, toplanan verilerinizi görüntüleme ve sorgulama hakkında temel örnekler sağlanmaktadır. İşlev uygulaması verilerinizi Application Insights araştırma hakkında daha fazla bilgi edinmek için bkz. [Application Insights nedir?](../azure-monitor/app/app-insights-overview.md). 

Veri saklama ve potansiyel depolama maliyetleri hakkında daha fazla bilgi için, bkz. [Application Insights veri toplama, bekletme ve depolama](../azure-monitor/app/data-retention-privacy.md).   

## <a name="viewing-telemetry-in-monitor-tab"></a>Izleme sekmesinde Telemetriyi görüntüleme

[Application Insights tümleştirme etkin](configure-monitoring.md#enable-application-insights-integration)olduğunda, telemetri verilerini **izleyici** sekmesinde görüntüleyebilirsiniz.

1. İşlev uygulaması sayfasında, Application Insights yapılandırıldıktan sonra en az bir kez çalışan bir işlev seçin. Ardından, sol bölmeden **izleyici** ' yi seçin. İşlev etkinleştirmeleri listesi görünene kadar düzenli aralıklarla **Yenile** ' yi seçin.

   ![Etkinleştirmeleri listesi](media/functions-monitoring/monitor-tab-ai-invocations.png)

    > [!NOTE]
    > Telemetri istemcisi sunucuya iletilmek üzere verileri toplu olarak işlerken listenin görünmesi beş dakikaya kadar sürebilir. Gecikme [canlı ölçüm akışı](../azure-monitor/app/live-stream.md)uygulanmaz. Bu hizmet, sayfayı yüklediğinizde Işlevler ana bilgisayarına bağlanır, bu nedenle Günlükler doğrudan sayfaya akışlanır.

1. Belirli bir işlev çağrısının günlüklerini görmek için, bu çağrının **Tarih (UTC)** sütunu bağlantısını seçin. Bu çağrının günlüğe kaydetme çıktısı yeni bir sayfada görüntülenir.

   ![Çağırma ayrıntıları](media/functions-monitoring/invocation-details-ai.png)

1. Azure günlüğünde Azure Izleyici günlük verilerini alan sorgunun kaynağını görüntülemek için **Application Insights Içinde Çalıştır '** ı seçin. Aboneliğinizde Azure Log Analytics 'yi ilk kez kullanıyorsanız etkinleştirmeniz istenir.

1. Log Analytics etkinleştirdikten sonra, aşağıdaki sorgu görüntülenir. Sorgu sonuçlarının son 30 güne () sınırlı olduğunu `where timestamp > ago(30d)` ve sonuçların 20 ' den fazla satır () olmadığını görebilirsiniz `take 20` . Buna karşılık, işlevinizin çağırma ayrıntıları listesi, son 30 gün için sınır olmadan olur.

   ![Application Insights Analytics çağırma listesi](media/functions-monitoring/ai-analytics-invocation-list.png)

Daha fazla bilgi için bu makalenin ilerleyen kısımlarında yer alarak [telemetri verilerini sorgulama](#query-telemetry-data) bölümüne bakın.

## <a name="view-telemetry-in-application-insights"></a>Application Insights telemetri görüntüleme

[Azure Portal](https://portal.azure.com)bir işlev uygulamasından Application Insights açmak için:

1. Portalda işlev uygulamanıza gidin.

1. Sol sayfada **Ayarlar** altında **Application Insights** ' ı seçin. 

1. Aboneliğiniz ile Application Insights ilk kez kullanıyorsanız etkinleştirmeniz istenir. Bunu yapmak için **Application Insights aç**' ı seçin ve ardından sonraki sayfada **Uygula** ' yı seçin.

![İşlev uygulamasına genel bakış sayfasından Application Insights açın](media/functions-monitoring/ai-link.png)

Application Insights kullanma hakkında daha fazla bilgi için [Application Insights belgelerine](/azure/application-insights/)bakın. Bu bölümde Application Insights verilerin nasıl görüntüleneceği hakkında bazı örnekler gösterilmektedir. Application Insights zaten hakkında bilginiz varsa [telemetri verilerini yapılandırma ve özelleştirme hakkındaki bölümlere](configure-monitoring.md#configure-log-levels)doğrudan gidebilirsiniz.

![Application Insights Genel Bakış sekmesi](media/functions-monitoring/metrics-explorer.png)

Aşağıdaki Application Insights, işlevinizdeki davranış, performans ve hataları değerlendirirken yararlı olabilir:

| Araştır | Description |
| ---- | ----------- |
| **[Hatalar](../azure-monitor/app/asp-net-exceptions.md)** |  İşlev hatalarıyla ve sunucu özel durumlarına göre grafikler ve uyarılar oluşturun. **Işlem adı** işlev adıdır. Bağımlılıklar için özel telemetri uygulamadığınız takdirde Bağımlılıklardaki arızalar gösterilmez. |
| **[Performans](../azure-monitor/app/performance-counters.md)** | **Bulut rol örnekleri** başına kaynak kullanımını ve aktarım hızını görüntüleyerek performans sorunlarını analiz edin. Bu performans verileri, işlevlerin temeldeki kaynaklarınızın gerisinde olduğu durumlarda hata ayıklama senaryolarında yararlı olabilir. |
| **[Ölçümler](../azure-monitor/essentials/metrics-charts.md)** | Ölçümleri temel alan grafikler ve uyarılar oluşturun. Ölçümler, işlev etkinleştirmeleri, yürütme süresi ve başarı oranları sayısını içerir. |
| **[Canlı ölçümler](../azure-monitor/app/live-stream.md)** | Ölçüm verilerini neredeyse gerçek zamanlı olarak oluşturulan şekilde görüntüleyin. |

## <a name="query-telemetry-data"></a>Telemetri verilerini sorgulama

[Application Insights Analytics](../azure-monitor/logs/log-query-overview.md) , bir veritabanındaki tablo biçimindeki tüm telemetri verilerine erişmenizi sağlar. Analytics verileri ayıklamak, işlemek ve görselleştirmek için bir sorgu dili sağlar. 

Günlüğe kaydedilen olayları incelemek veya sorgulamak için **günlükleri** seçin.

![Analiz örneği](media/functions-monitoring/analytics-traces.png)

Son 30 dakika içinde çalışan başına isteklerin dağılımını gösteren bir sorgu örneği aşağıda verilmiştir.

```kusto
requests
| where timestamp > ago(30m) 
| summarize count() by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
```

Kullanılabilir tablolar, sol taraftaki **şema** sekmesinde gösterilir. İşlev etkinleştirmeleri tarafından oluşturulan verileri aşağıdaki tablolarda bulabilirsiniz:

| Tablo | Açıklama |
| ----- | ----------- |
| **lerin** | Çalışma zamanı, ölçek denetleyicisi ve izleme tarafından işlev kodunuzda oluşturulan Günlükler. |
| **istekler** | Her işlev çağrısı için bir istek. |
| **larý** | Çalışma zamanı tarafından oluşturulan özel durumlar. |
| **customMetrics** | Başarılı ve başarısız çağırma sayısı, başarı oranı ve süre. |
| **customEvents** | Çalışma zamanı tarafından izlenen olaylar, örneğin: bir işlevi tetikleyen HTTP istekleri. |
| **performanceCounters** | İşlevlerin üzerinde çalıştığı sunucuların performansı hakkında bilgiler. |

Diğer tablolar, kullanılabilirlik testleri, istemci ve tarayıcı telemetri içindir. Verilere veri eklemek için özel telemetri uygulayabilirsiniz.

Her tablo içinde IŞLEVLERE özgü verilerden bazıları bir `customDimensions` alandır.  Örneğin, aşağıdaki sorgu günlük düzeyine sahip tüm izlemeleri alır `Error` .

```kusto
traces 
| where customDimensions.LogLevel == "Error"
```

Çalışma zamanı `customDimensions.LogLevel` ve alanlarını sağlar `customDimensions.Category` . İşlev kodunuzda yazdığınız günlüklerde ek alanlar sağlayabilirsiniz. C# ' deki bir örnek için bkz. .NET sınıf kitaplığı geliştirici kılavuzunda [yapılandırılmış günlüğe kaydetme](functions-dotnet-class-library.md#structured-logging) .

## <a name="query-scale-controller-logs"></a>Sorgu ölçek denetleyicisi günlükleri

_Bu özellik önizleme aşamasındadır._

Hem [Ölçek denetleyicisi günlüğünü](configure-monitoring.md#configure-scale-controller-logs) hem de [Application Insights tümleştirmeyi](configure-monitoring.md#enable-application-insights-integration)etkinleştirdikten sonra, Application Insights günlük aramasını, yayılan ölçek denetleyicisi günlüklerini sorgulamak için kullanabilirsiniz. Ölçek denetleyicisi günlükleri, `traces` koleksiyonda **scalecontrollerlogs** kategorisi altında kaydedilir.

Aşağıdaki sorgu, geçerli işlev uygulaması için belirtilen süre içinde tüm ölçek denetleyicisi günlüklerini aramak için kullanılabilir:

```kusto
traces 
| extend CustomDimensions = todynamic(tostring(customDimensions))
| where CustomDimensions.Category == "ScaleControllerLogs"
```

Aşağıdaki sorgu, yalnızca ölçekteki bir değişikliği gösteren günlüklerin nasıl alınacağını göstermek için önceki sorgu üzerinde genişler:

```kusto
traces 
| extend CustomDimensions = todynamic(tostring(customDimensions))
| where CustomDimensions.Category == "ScaleControllerLogs"
| where message == "Instance count changed"
| extend Reason = CustomDimensions.Reason
| extend PreviousInstanceCount = CustomDimensions.PreviousInstanceCount
| extend NewInstanceCount = CustomDimensions.CurrentInstanceCount
```

## <a name="consumption-plan-specific-metrics"></a>Tüketim planına özgü ölçümler

Bir [Tüketim planında](consumption-plan.md)çalışırken, tek bir işlev yürütmenin yürütme *ücreti* *GB saniye* cinsinden ölçülür. Yürütme maliyeti, bellek kullanımını yürütme süresi ile birleştirerek hesaplanır. Daha fazla bilgi edinmek için bkz. [Tüketim planı maliyetlerini tahmin](functions-consumption-costs.md)etme.

Aşağıdaki telemetri sorguları, tüketim planında çalışan işlevlerin maliyetini etkileyen ölçümlere özgüdür.

[!INCLUDE [functions-consumption-metrics-queries](../../includes/functions-consumption-metrics-queries.md)]

## <a name="azure-monitor-metrics"></a>Azure Izleyici ölçümleri

Application Insights tarafından toplanan telemetri verilerine ek olarak, [Azure Izleyici ölçümlerinden](../azure-monitor/platform/data-platform-metrics.md)işlev uygulamasının nasıl çalıştığı hakkında da veri alabilirsiniz. [App Service uygulamalar için kullanılabilen olağan ölçümlerle](../app-service/web-sites-monitor.md#understand-metrics)birlikte, ilgilendiğiniz işlevlere özgü iki ölçüm vardır:

| Metric | Açıklama |
| ---- | ---- |
| **Işlev yürütme sayısı** | İşlev yürütme sayısı, işlev uygulamanızın kaç kez yürütüldüğünü belirtir. Bu, bir işlevin uygulamanızda kaç kez çalışacağını de sağlar. Bu ölçüm Şu anda Linux üzerinde çalışan Premium ve adanmış (App Service) planlar için desteklenmemektedir. |
| **FunctionExecutionUnits** | İşlev yürütme birimleri, yürütme süresi ve bellek kullanımınızın bir birleşimidir.  Bellek verileri şu anda Azure Izleyici aracılığıyla kullanılabilen bir ölçüm değildir. Ancak, uygulamanızın bellek kullanımını iyileştirmek isterseniz, Application Insights tarafından toplanan performans sayacı verilerini kullanabilir. Bu ölçüm Şu anda Linux üzerinde çalışan Premium ve adanmış (App Service) planlar için desteklenmemektedir.|

Application Insights verileri kullanarak bir tüketim planının maliyetlerini hesaplama hakkında daha fazla bilgi edinmek için bkz. [Tüketim planı maliyetlerini tahmin](functions-consumption-costs.md)etme. Ölçümleri görüntülemek için Izleyici Gezginini kullanma hakkında daha fazla bilgi edinmek için bkz. [Azure Ölçüm Gezgini](../azure-monitor/platform/metrics-getting-started.md)kullanmaya başlama.


## <a name="next-steps"></a>Sonraki adımlar

Azure Işlevlerini izleme hakkında daha fazla bilgi edinin:

+ [Azure İşlevlerini İzleme](functions-monitoring.md)
+ [Azure Işlevleri için izlemeyi yapılandırma](configure-monitoring.md)

