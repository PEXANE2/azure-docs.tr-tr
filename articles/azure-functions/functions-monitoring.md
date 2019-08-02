---
title: Azure İşlevlerini İzleme
description: İşlev yürütmeyi izlemek için Azure Application Insights Azure Işlevleri 'ni kullanmayı öğrenin.
services: functions
author: ggailey777
manager: jeconnoc
keywords: azure işlevleri, işlevler, olay işleme, web kancaları, dinamik işlem, sunucusuz mimari
ms.assetid: 501722c3-f2f7-4224-a220-6d59da08a320
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/04/2019
ms.author: glenga
ms.openlocfilehash: cfdc28486cf254c4dd808824ab167489818376ab
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68619603"
---
# <a name="monitor-azure-functions"></a>Azure İşlevlerini İzleme

[Azure işlevleri](functions-overview.md) , işlevleri Izlemek için [Azure Application Insights](../azure-monitor/app/app-insights-overview.md) ile yerleşik tümleştirme sunar. Bu makalede, Azure Işlevlerinin Application Insights sistem tarafından oluşturulan günlük dosyalarını göndermek üzere nasıl yapılandırılacağı gösterilmektedir.

Günlük, performans ve hata verilerini topladığı için Application Insights kullanmanızı öneririz. Performans sorunlarını otomatik olarak algılar ve sorunları tanılamanıza ve işlevlerinizin nasıl kullanıldığını anlamanıza yardımcı olacak güçlü analiz araçları içerir. Performansı ve kullanılabilirliği sürekli geliştirmenize yardımcı olmak amacıyla tasarlanmıştır. Application Insights, yerel işlev uygulama projesi geliştirme sırasında bile kullanabilirsiniz. Daha fazla bilgi için bkz. [Application Insights nedir?](../azure-monitor/app/app-insights-overview.md).

Gerekli Application Insights izleme Azure Işlevleri 'nde yerleşik olduğundan, işlev uygulamanızı bir Application Insights kaynağına bağlamak için tüm ihtiyacınız olan geçerli bir izleme anahtarıdır.

## <a name="application-insights-pricing-and-limits"></a>Fiyatlandırma ve limitleri Application Insights

Işlev uygulamalarıyla Application Insights tümleştirmeyi ücretsiz olarak deneyebilirsiniz. Ücretsiz olarak ne kadar veri işlenebileceğini gösteren günlük bir sınır vardır. Sınama sırasında bu sınıra ulaşırsınız. Günlük sınırınıza yaklaşdığınızda Azure, Portal ve e-posta bildirimleri sağlar. Bu uyarıları kaçırırsanız ve Sınıra ulaşırsanız yeni Günlükler Application Insights sorgularda görünmez. Gereksiz sorun giderme zamanından kaçının limiti göz önünde bulundurun. Daha fazla bilgi için bkz. [Application Insights fiyatlandırma ve veri hacmini yönetme](../azure-monitor/app/pricing.md).

## <a name="enable-application-insights-integration"></a>Application Insights tümleştirmeyi etkinleştir

Bir işlev uygulamasının Application Insights verileri gönderebilmesi için, bir Application Insights kaynağının izleme anahtarını bilmeleri gerekir. Anahtar, **APPINSIGHTS_INSTRUMENTATIONKEY**adlı bir uygulama ayarında olmalıdır.

### <a name="new-function-app-in-the-portal"></a>Portalda yeni işlev uygulaması

[Azure Portal işlev uygulamanızı oluşturduğunuzda](functions-create-first-azure-function.md)Application Insights tümleştirme varsayılan olarak etkinleştirilmiştir. Application Insights kaynak, işlev uygulamanızla aynı ada sahiptir ve aynı bölgede ya da en yakın bölgede oluşturulur.

Oluşturulan Application Insights kaynağını gözden geçirmek için, **Application Insights** penceresini genişletmek üzere seçin. **Yeni kaynak adını** değiştirebilir veya verilerinizi depolamak istediğiniz [Azure Coğrafya](https://azure.microsoft.com/global-infrastructure/geographies/) 'da farklı bir **konum** seçebilirsiniz.

![İşlev uygulaması oluştururken Application Insights etkinleştirme](media/functions-monitoring/enable-ai-new-function-app.png)

**Oluştur**' u seçtiğinizde, işlev `APPINSIGHTS_INSTRUMENTATIONKEY` uygulamanız ile uygulama ayarlarında ayarlanmış olan bir Application Insights kaynak oluşturulur. Her şey başlamaya hazırlanıyor.

<a id="manually-connect-an-app-insights-resource"></a>
### <a name="add-to-an-existing-function-app"></a>Mevcut bir işlev uygulamasına ekleme 

[Azure CLI](functions-create-first-azure-function-azure-cli.md), [Visual Studio](functions-create-your-first-function-visual-studio.md)veya [Visual Studio Code](functions-create-first-function-vs-code.md)kullanarak bir işlev uygulaması oluşturduğunuzda, Application Insights kaynağını oluşturmanız gerekir. Daha sonra bu kaynaktaki izleme anahtarını, işlev uygulamanızda bir uygulama ayarı olarak ekleyebilirsiniz.

[!INCLUDE [functions-connect-new-app-insights.md](../../includes/functions-connect-new-app-insights.md)]

Işlevlerin erken sürümleri, artık önerilmeyen yerleşik izleme kullanır. Böyle bir işlev uygulaması için Application Insights tümleştirmesinin etkinleştirilmesinde, [yerleşik günlüğü de devre dışı bırakmanız](#disable-built-in-logging)gerekir.  

## <a name="view-telemetry-in-monitor-tab"></a>Izleyici sekmesinde Telemetriyi görüntüleme

[Application Insights tümleştirme etkin](#enable-application-insights-integration)olduğunda, telemetri verilerini **izleyici** sekmesinde görüntüleyebilirsiniz.

1. İşlev uygulaması sayfasında, Application Insights yapılandırıldıktan sonra en az bir kez çalışan bir işlev seçin. Ardından **izleyici** sekmesini seçin.

   ![Izleme sekmesini seçin](media/functions-monitoring/monitor-tab.png)

1. İşlev etkinleştirmeleri listesi görünene kadar düzenli aralıklarla **Yenile** ' yi seçin.

   Telemetri istemcisi sunucuya iletilmek üzere verileri toplu olarak işlerken listenin görünmesi beş dakikaya kadar sürebilir. (Gecikme [canlı ölçüm akışı](../azure-monitor/app/live-stream.md)uygulanmaz. Bu hizmet, sayfayı yüklediğinizde Işlevler ana bilgisayarına bağlanır, bu nedenle Günlükler doğrudan sayfaya akışlanır.)

   ![Etkinleştirmeleri listesi](media/functions-monitoring/monitor-tab-ai-invocations.png)

1. Belirli bir işlev çağrısının günlüklerini görmek için, bu çağrının **Tarih** sütunu bağlantısını seçin.

   ![Çağırma ayrıntıları bağlantısı](media/functions-monitoring/invocation-details-link-ai.png)

   Bu çağrının günlüğe kaydetme çıktısı yeni bir sayfada görüntülenir.

   ![Çağırma ayrıntıları](media/functions-monitoring/invocation-details-ai.png)

Her iki sayfanın da verileri alan Application Insights Analytics sorgusuna **Application Insights bir Çalıştır** bağlantısı olduğunu görebilirsiniz.

![Application Insights'ta çalıştır](media/functions-monitoring/run-in-ai.png)

Aşağıdaki sorgu görüntülenir. Çağrı listesinin son 30 güne sınırlı olduğunu görebilirsiniz. Listede en fazla 20 satır (`where timestamp > ago(30d) | take 20`) gösterilir. Çağırma ayrıntıları listesi, son 30 gün için sınır yoktur.

![Application Insights Analytics çağırma listesi](media/functions-monitoring/ai-analytics-invocation-list.png)

Daha fazla bilgi için bu makalenin ilerleyen kısımlarında yer alarak [telemetri verilerini sorgulama](#query-telemetry-data) bölümüne bakın.

## <a name="view-telemetry-in-application-insights"></a>Application Insights telemetri görüntüleme

Azure portal bir işlev uygulamasından Application Insights açmak için, işlev uygulamasının **genel bakış** sayfasına gidin. **Yapılandırılan Özellikler**altında **Application Insights**' yi seçin.

![İşlev uygulamasına genel bakış sayfasından Application Insights açın](media/functions-monitoring/ai-link.png)

Application Insights kullanma hakkında daha fazla bilgi için [Application Insights belgelerine](https://docs.microsoft.com/azure/application-insights/)bakın. Bu bölümde Application Insights verilerin nasıl görüntüleneceği hakkında bazı örnekler gösterilmektedir. Application Insights zaten hakkında bilginiz varsa [telemetri verilerini yapılandırma ve özelleştirme hakkındaki bölümlere](#configure-categories-and-log-levels)doğrudan gidebilirsiniz.

![Application Insights Genel Bakış sekmesi](media/functions-monitoring/metrics-explorer.png)

Aşağıdaki Application Insights, işlevinizdeki davranış, performans ve hataları değerlendirirken yararlı olabilir:

| Tab | Açıklama |
| ---- | ----------- |
| **[Kesil](../azure-monitor/app/asp-net-exceptions.md)** |  İşlev hatalarıyla ve sunucu özel durumlarına göre grafikler ve uyarılar oluşturun. **Işlem adı** işlev adıdır. Bağımlılıklar için özel telemetri uygulamadığınız takdirde Bağımlılıklardaki arızalar gösterilmez. |
| **[Mının](../azure-monitor/app/performance-counters.md)** | Performans sorunlarını analiz edin. |
| **Sunucular** | Sunucu başına kaynak kullanımını ve aktarım hızını görüntüleyin. Bu veriler, işlevlerin temeldeki kaynaklarınızın gerisinde bulunduğu hata ayıklama senaryolarında yararlı olabilir. Sunucular, **bulut rolü örnekleri**olarak adlandırılır. |
| **[Ölçümler](../azure-monitor/app/metrics-explorer.md)** | Ölçümleri temel alan grafikler ve uyarılar oluşturun. Ölçümler, işlev etkinleştirmeleri, yürütme süresi ve başarı oranları sayısını içerir. |
| **[Canlı Ölçüm Akışı](../azure-monitor/app/live-stream.md)** | Gerçek zamanlı olarak oluşturulduğu sırada ölçüm verilerini görüntüleyin. |

## <a name="query-telemetry-data"></a>Telemetri verilerini sorgulama

[Application Insights Analytics](../azure-monitor/app/analytics.md) , bir veritabanındaki tablo biçimindeki tüm telemetri verilerine erişmenizi sağlar. Analytics verileri ayıklamak, işlemek ve görselleştirmek için bir sorgu dili sağlar.

![Analiz Seç](media/functions-monitoring/select-analytics.png)

![Analiz örneği](media/functions-monitoring/analytics-traces.png)

Son 30 dakika içinde çalışan başına isteklerin dağılımını gösteren bir sorgu örneği aşağıda verilmiştir.

```
requests
| where timestamp > ago(30m) 
| summarize count() by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
```

Kullanılabilir tablolar, sol taraftaki **şema** sekmesinde gösterilir. İşlev etkinleştirmeleri tarafından oluşturulan verileri aşağıdaki tablolarda bulabilirsiniz:

| Tablo | Açıklama |
| ----- | ----------- |
| **lerin** | Çalışma zamanı ve işlev kodu tarafından oluşturulan Günlükler. |
| **istekleri** | Her işlev çağrısı için bir istek. |
| **larý** | Çalışma zamanı tarafından oluşturulan özel durumlar. |
| **customMetrics** | Başarılı ve başarısız çağırma sayısı, başarı oranı ve süre. |
| **customEvents** | Çalışma zamanı tarafından izlenen olaylar, örneğin: Bir işlevi tetikleyen HTTP istekleri. |
| **performanceCounters** | İşlevlerin üzerinde çalıştığı sunucuların performansı hakkında bilgiler. |

Diğer tablolar, kullanılabilirlik testleri, istemci ve tarayıcı telemetri içindir. Verilere veri eklemek için özel telemetri uygulayabilirsiniz.

Her tablo içinde işlevlere özgü verilerden bazıları bir `customDimensions` alandır.  Örneğin, aşağıdaki sorgu günlük düzeyine `Error`sahip tüm izlemeleri alır.

```
traces 
| where customDimensions.LogLevel == "Error"
```

Çalışma zamanı `customDimensions.LogLevel` ve `customDimensions.Category` alanlarını sağlar. İşlev kodunuzda yazdığınız günlüklerde ek alanlar sağlayabilirsiniz. Bu makalenin sonraki kısımlarında bulunan [yapılandırılmış günlüğe](#structured-logging) bakın.

## <a name="configure-categories-and-log-levels"></a>Kategorileri ve günlük düzeylerini yapılandırma

Application Insights, özel yapılandırma olmadan kullanabilirsiniz. Varsayılan yapılandırma, yüksek hacimde veri oluşmasına neden olabilir. Visual Studio Azure aboneliği kullanıyorsanız, Application Insights için veri üst sınırına ulaşırsınız. Bu makalenin ilerleyen kısımlarında, işlevlerinizin Application Insights gönderileceği verileri yapılandırmayı ve özelleştirmeyi öğreneceksiniz. Bir işlev uygulaması için, Logging [Host. JSON] dosyasında yapılandırılır.

### <a name="categories"></a>Categories

Azure Işlevleri günlükçüsü, her günlük için bir *Kategori* içerir. Kategori, çalışma zamanı kodunun veya işlev kodunuzun günlüğü yazanın hangi kısmının olduğunu gösterir. 

Işlevler çalışma zamanı, "Host" ile başlayan bir kategoriye sahip Günlükler oluşturur. `function started`Sürüm 1. x `function executed`içinde,, ve `function completed` günlükleri kategorisi `Host.Executor`vardır. 2\. x sürümünden itibaren bu günlüklerde kategori `Function.<YOUR_FUNCTION_NAME>`vardır.

İşlev kodunuzda Günlükler yazarsanız, kategori işlevler çalışma zamanının sürüm 1. `Function` x ' de bulunur. Sürüm 2. x içinde kategori `Function.<YOUR_FUNCTION_NAME>.User`.

### <a name="log-levels"></a>Günlük düzeyleri

Azure Işlevleri günlükçüsü, her günlük için bir *günlük düzeyi* de içerir. [LogLevel](/dotnet/api/microsoft.extensions.logging.loglevel) bir sabit listesi ve tamsayı kodu göreli önemi gösterir:

|LogLevel    |Kod|
|------------|---|
|İzleme       | 0 |
|Hata ayıklama       | 1\. |
|Information | 2 |
|Uyarı     | 3 |
|Hata       | 4 |
|Kritik    | 5 |
|Yok.        | 6 |

Günlük düzeyi `None` sonraki bölümde açıklanmaktadır. 

### <a name="log-configuration-in-hostjson"></a>Host. JSON içinde günlük yapılandırması

[Host. JSON] dosyası, bir işlev uygulamasının Application Insights ne kadar günlüğe göndereceğini yapılandırır. Her kategori için, gönderileceği en düşük günlük düzeyini belirtirsiniz. İki örnek vardır: ilk örnek, [sürüm 2. x çalışma zamanı](functions-versions.md#version-2x) (.NET Core) işlevlerini hedefler ve ikinci örnek sürüm 1. x çalışma zamanı içindir.

### <a name="version-2x"></a>Sürüm 2. x

V2. x çalışma zamanı [.NET Core günlük filtresi hiyerarşisini](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#log-filtering)kullanır. 

```json
{
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "default": "Information",
      "Host.Results": "Error",
      "Function": "Error",
      "Host.Aggregator": "Trace"
    }
  }
}
```

### <a name="version-1x"></a>Sürüm 1. x

```json
{
  "logger": {
    "categoryFilter": {
      "defaultLevel": "Information",
      "categoryLevels": {
        "Host.Results": "Error",
        "Function": "Error",
        "Host.Aggregator": "Trace"
      }
    }
  }
}
```

Bu örnek aşağıdaki kuralları ayarlar:

* Kategorisi `Host.Results` olan Günlükler veya `Function`Application Insights için yalnızca `Error` bir düzey ve yukarıya gönder. Düzey ve `Warning` alt için Günlükler yoksayıldı.
* Kategorisi `Host.Aggregator`olan Günlükler için tüm günlükleri Application Insights gönderin. Günlük düzeyi, `Verbose`bazı günlükçülerin çağrı yaptığı, ancak [Host. JSON] dosyasında `Trace` kullanılan şeydir. `Trace`
* Tüm diğer Günlükler için, Application Insights için `Information` yalnızca düzeyini ve üstünü gönderin.

[Host. JSON] dosyasındaki kategori değeri aynı değerle başlayan tüm kategoriler için günlük kaydını denetler. `Host`[Host. JSON] ' da, `Host.General` `Host.Executor` `Host.Results`,, vb. için günlüğe kaydetmeyi denetler.

[Host. JSON] aynı dizeyle başlayan birden çok kategori içeriyorsa, daha uzun olanlar önce eşleştirilir. Çalışma `Host.Aggregator` zamanından itibaren `Error` oturum açma haricinde, ancak `Host.Aggregator` şu `Information` düzeyde günlüğe kaydetmek istediğiniz her şeyi istediğinizi varsayalım:

### <a name="version-2x"></a>Sürüm 2. x 

```json
{
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "default": "Information",
      "Host": "Error",
      "Function": "Error",
      "Host.Aggregator": "Information"
    }
  }
}
```

### <a name="version-1x"></a>Sürüm 1. x 

```json
{
  "logger": {
    "categoryFilter": {
      "defaultLevel": "Information",
      "categoryLevels": {
        "Host": "Error",
        "Function": "Error",
        "Host.Aggregator": "Information"
      }
    }
  }
}
```

Bir kategorinin tüm günlüklerini gizlemek için günlük düzeyi `None`' ni kullanabilirsiniz. Bu kategoriyle hiçbir günlük yazılmadı ve üzerinde günlük düzeyi yok.

Aşağıdaki bölümlerde, çalışma zamanının oluşturduğu günlüklerin ana kategorileri açıklanır. 

### <a name="category-hostresults"></a>Kategori konak. Results

Bu Günlükler Application Insights ' de "istekler" olarak gösterilir. Bir işlevin başarısını veya başarısızlığını gösterir.

![İstek grafiği](media/functions-monitoring/requests-chart.png)

Tüm bu Günlükler `Information` düzeyinde yazılır. `Warning` Veya üzerine filtre yaparsanız, bu verilerden herhangi birini görmezsiniz.

### <a name="category-hostaggregator"></a>Kategori ana bilgisayar. toplayıcısı

Bu Günlükler, [yapılandırılabilir](#configure-the-aggregator) bir süre boyunca işlev çağırma sayısının sayısını ve ortalamasını sağlar. Varsayılan süre 30 saniye veya 1.000 sonuçdur, hangisi önce gelir. 

Günlükler, Application Insights 'daki **Customölçümler** tablosunda bulunabilir. Çalıştırma sayısı, başarı oranı ve süre örnekleri verilebilir.

![Customölçümler sorgusu](media/functions-monitoring/custom-metrics-query.png)

Tüm bu Günlükler `Information` düzeyinde yazılır. `Warning` Veya üzerine filtre yaparsanız, bu verilerden herhangi birini görmezsiniz.

### <a name="other-categories"></a>Diğer Kategoriler

Önceden listelenmiş olanlar dışındaki kategoriler için tüm Günlükler, Application Insights İziz tablosunda mevcuttur .

![izleme sorgusu](media/functions-monitoring/analytics-traces.png)

İle `Host` başlayan kategorilerin bulunduğu tüm Günlükler işlevler çalışma zamanı tarafından yazılır. "Işlev başlatıldı" ve "Işlev tamamlandı" günlüklerinin kategorisi `Host.Executor`vardır. Başarılı çalıştırmalar için bu Günlükler düzeydir `Information` . Özel durumlar `Error` düzeyinde günlüğe kaydedilir. Çalışma zamanı ayrıca düzey `Warning` günlükleri oluşturur, örneğin, zarar kuyruğuna gönderilen kuyruk iletileri.

İşlev kodunuz tarafından yazılan günlüklerde kategori `Function` vardır ve herhangi bir günlük düzeyi olabilir.

## <a name="configure-the-aggregator"></a>Toplayıcısı yapılandırma

Önceki bölümde belirtildiği gibi, çalışma zamanı bir süre boyunca işlev yürütmeleri hakkındaki verileri toplar. Varsayılan süre 30 saniye veya 1.000 çalışma olur ve hangisi önce gelir. Bu ayarı, [Host. JSON] dosyasında yapılandırabilirsiniz.  Bir örneği aşağıda verilmiştir:

```json
{
    "aggregator": {
      "batchSize": 1000,
      "flushTimeout": "00:00:30"
    }
}
```

## <a name="configure-sampling"></a>Örnekleme yapılandırma

Application Insights, yoğun yük saatlerinde tamamlanan yürütmeler üzerinde çok fazla telemetri verisi üretmenin bir [örnekleme](../azure-monitor/app/sampling.md) özelliğine sahiptir. Gelen yürütmeler oranı belirtilen eşiği aştığında Application Insights, gelen yürütmelerin bazılarını rastgele yok saymaya başlar. Saniyedeki en fazla yürütme sayısı için varsayılan ayar 20 ' dir (sürüm 1. x içinde beş). [Host. JSON]içinde örnekleme yapılandırabilirsiniz.  Bir örneği aşağıda verilmiştir:

### <a name="version-2x"></a>Sürüm 2. x 

```json
{
  "logging": {
    "applicationInsights": {
      "samplingSettings": {
        "isEnabled": true,
        "maxTelemetryItemsPerSecond" : 20
      }
    }
  }
}
```

### <a name="version-1x"></a>Sürüm 1. x 

```json
{
  "applicationInsights": {
    "sampling": {
      "isEnabled": true,
      "maxTelemetryItemsPerSecond" : 5
    }
  }
}
```

> [!NOTE]
> [Örnekleme](../azure-monitor/app/sampling.md) varsayılan olarak etkindir. Eksik veri olduğunu görürseniz, örnekleme ayarlarını belirli izleme senaryonuza uyacak şekilde ayarlamanız gerekebilir.

## <a name="write-logs-in-c-functions"></a>Günlükleri C# işlevlerde yazma

İşlev kodunuzda, Application Insights izlemeler olarak görünen Günlükler yazabilirsiniz.

### <a name="ilogger"></a>ILogger

İşlevinizdeki bir `TraceWriter` parametre yerine bir [ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger) parametresi kullanın. Kullanılarak `TraceWriter` oluşturulan Günlükler Application Insights git, ancak `ILogger` [yapılandırılmış günlüğe kaydetme](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)yapmanızı sağlar.

Bir `ILogger` nesnesi ile, günlük oluşturmak `Log<level>` için [ILogger üzerinde uzantı yöntemleri](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.loggerextensions#methods) çağırın. Aşağıdaki kod, " `Information` Function" kategorisine sahip günlükleri yazar.

```cs
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger logger)
{
    logger.LogInformation("Request for item with key={itemKey}.", id);
```

### <a name="structured-logging"></a>Yapılandırılmış günlüğe kaydetme

Adları değil, yer tutucular sırası, günlük iletisinde kullanılan parametreleri belirler. Aşağıdaki koda sahip olduğunuzu varsayalım:

```csharp
string partitionKey = "partitionKey";
string rowKey = "rowKey";
logger.LogInformation("partitionKey={partitionKey}, rowKey={rowKey}", partitionKey, rowKey);
```

Aynı ileti dizesini tutar ve parametrelerin sırasını ters tutarsanız, sonuçta elde edilen ileti metninde yanlış yerlerde değerler olacaktır.

Yer tutucular bu şekilde işlenir, böylece yapılandırılmış günlüğe kaydetme yapabilirsiniz. Application Insights, ad-değer çiftlerini ve ileti dizesini depolayan parametre. Sonuç olarak, ileti bağımsız değişkenlerinin sorgulayabilmeniz için alanlar haline gelir.

Günlükçü yöntemi çağrın bir önceki örneğe benzei durumunda, alanı `customDimensions.prop__rowKey`sorgulayabilirsiniz. Çalışma zamanı eklemeleri ve işlev kodunuzun eklediği alanlar arasında çakışma olmadığından emin olmak için önekeklenir.`prop__`

Ayrıca, alanına `customDimensions.prop__{OriginalFormat}`başvurarak özgün ileti dizesinde sorgulama yapabilirsiniz.  

Aşağıda, `customDimensions` verilerin JSON örnek bir gösterimi verilmiştir:

```json
{
  customDimensions: {
    "prop__{OriginalFormat}":"C# Queue trigger function processed: {message}",
    "Category":"Function",
    "LogLevel":"Information",
    "prop__message":"c9519cbf-b1e6-4b9b-bf24-cb7d10b1bb89"
  }
}
```

### <a name="custom-metrics-logging"></a>Özel Ölçüm günlüğü

C# Betik işlevlerinde, Application Insights içinde özel ölçümler oluşturmak `LogMetric` `ILogger` için genişletme yöntemini kullanabilirsiniz. Örnek bir yöntem çağrısı aşağıda verilmiştir:

```csharp
logger.LogMetric("TestMetric", 1234);
```

Bu kod, .NET için Application Insights API `TrackMetric` 'si kullanılarak çağırmanın bir alternatifidir.

## <a name="write-logs-in-javascript-functions"></a>JavaScript işlevlerinde yazma günlükleri

Node. js işlevlerinde, günlükleri yazmak `context.log` için kullanın. Yapılandırılmış günlüğe kaydetme etkin değil.

```
context.log('JavaScript HTTP trigger function processed a request.' + context.invocationId);
```

### <a name="custom-metrics-logging"></a>Özel Ölçüm günlüğü

Functions çalışma zamanının [1. x sürümünde](functions-versions.md#creating-1x-apps) çalışırken Node. js işlevleri Application Insights içinde özel ölçümler oluşturmak için `context.log.metric` yöntemini kullanabilir. Bu yöntem şu anda sürüm 2. x içinde desteklenmiyor. Örnek bir yöntem çağrısı aşağıda verilmiştir:

```javascript
context.log.metric("TestMetric", 1234);
```

Bu kod, Application Insights için Node. `trackMetric` js SDK 'sını kullanarak çağırmak için bir alternatiftir.

## <a name="log-custom-telemetry-in-c-functions"></a>C# İşlevlerde özel telemetri günlüğe kaydet

[Microsoft. ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) NuGet paketini, Application Insights özel telemetri verileri göndermek için kullanabilirsiniz. Aşağıdaki C# örnek, [özel telemetri API](../azure-monitor/app/api-custom-events-metrics.md)'sini kullanır. Örnek, bir .NET sınıf kitaplığı içindir, ancak Application Insights kodu C# betiğe yöneliktir.

### <a name="version-2x"></a>Sürüm 2. x

Sürüm 2. x çalışma zamanı, telemetrinin geçerli işlemle otomatik olarak ilişkilendirilmesi için Application Insights yeni özellikleri kullanır. İşlem `Id`, `ParentId`veya alanlarınıelileayarlamanızgerekmez.`Name`

```cs
using System;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Extensions.Logging;

namespace functionapp0915
{
    public class HttpTrigger2
    {
        private readonly TelemetryClient telemetryClient;

        /// Using dependency injection will guarantee that you use the same configuration for telemetry collected automatically and manually.
        public HttpTrigger2(TelemetryConfiguration telemetryConfiguration)
        {
            this.telemetryClient = new TelemetryClient(telemetryConfiguration);
        }

        [FunctionName("HttpTrigger2")]
        public Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", Route = null)]
            HttpRequest req, ExecutionContext context, ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            DateTime start = DateTime.UtcNow;

            // Parse query parameter
            string name = req.Query
                .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
                .Value;

            // Track an Event
            var evt = new EventTelemetry("Function called");
            evt.Context.User.Id = name;
            this.telemetryClient.TrackEvent(evt);

            // Track a Metric
            var metric = new MetricTelemetry("Test Metric", DateTime.Now.Millisecond);
            metric.Context.User.Id = name;
            this.telemetryClient.TrackMetric(metric);

            // Track a Dependency
            var dependency = new DependencyTelemetry
            {
                Name = "GET api/planets/1/",
                Target = "swapi.co",
                Data = "https://swapi.co/api/planets/1/",
                Timestamp = start,
                Duration = DateTime.UtcNow - start,
                Success = true
            };
            dependency.Context.User.Id = name;
            this.telemetryClient.TrackDependency(dependency);

            return Task.FromResult<IActionResult>(new OkResult());
        }
    }
}
```

### <a name="version-1x"></a>Sürüm 1. x

```cs
using System;
using System.Net;
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.Azure.WebJobs;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Extensions.Logging;
using System.Linq;

namespace functionapp0915
{
    public static class HttpTrigger2
    {
        private static string key = TelemetryConfiguration.Active.InstrumentationKey = 
            System.Environment.GetEnvironmentVariable(
                "APPINSIGHTS_INSTRUMENTATIONKEY", EnvironmentVariableTarget.Process);

        private static TelemetryClient telemetryClient = 
            new TelemetryClient() { InstrumentationKey = key };

        [FunctionName("HttpTrigger2")]
        public static async Task<HttpResponseMessage> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
            HttpRequestMessage req, ExecutionContext context, ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            DateTime start = DateTime.UtcNow;

            // Parse query parameter
            string name = req.GetQueryNameValuePairs()
                .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
                .Value;

            // Get request body
            dynamic data = await req.Content.ReadAsAsync<object>();

            // Set name to query string or body data
            name = name ?? data?.name;
         
            // Track an Event
            var evt = new EventTelemetry("Function called");
            UpdateTelemetryContext(evt.Context, context, name);
            telemetryClient.TrackEvent(evt);
            
            // Track a Metric
            var metric = new MetricTelemetry("Test Metric", DateTime.Now.Millisecond);
            UpdateTelemetryContext(metric.Context, context, name);
            telemetryClient.TrackMetric(metric);
            
            // Track a Dependency
            var dependency = new DependencyTelemetry
                {
                    Name = "GET api/planets/1/",
                    Target = "swapi.co",
                    Data = "https://swapi.co/api/planets/1/",
                    Timestamp = start,
                    Duration = DateTime.UtcNow - start,
                    Success = true
                };
            UpdateTelemetryContext(dependency.Context, context, name);
            telemetryClient.TrackDependency(dependency);
        }
        
        // Correlate all telemetry with the current Function invocation
        private static void UpdateTelemetryContext(TelemetryContext context, ExecutionContext functionContext, string userName)
        {
            context.Operation.Id = functionContext.InvocationId.ToString();
            context.Operation.ParentId = functionContext.InvocationId.ToString();
            context.Operation.Name = functionContext.FunctionName;
            context.User.Id = userName;
        }
    }    
}
```

Bir işlev `TrackRequest` çağrısı `StartOperation<RequestTelemetry>` için yinelenen istekler göreceğiniz için veya çağrısı yapmayın.  Işlevler çalışma zamanı istekleri otomatik olarak izler.

Ayarlanmadı `telemetryClient.Context.Operation.Id`. Aynı anda çok sayıda işlev çalışırken bu genel ayar yanlış bağıntı oluşmasına neden olur. Bunun yerine, yeni bir telemetri örneği (`DependencyTelemetry`, `EventTelemetry`) oluşturun ve `Context` özelliğini değiştirin. `Track` Ardından Telemetri örneğini ( `TelemetryClient` `TrackDependency()`, )üzerindeilgiliyöntemegeçirin.`TrackEvent()` Bu yöntem, telemetrinin geçerli işlev çağırma için doğru bağıntı ayrıntılarına sahip olmasını sağlar.

## <a name="log-custom-telemetry-in-javascript-functions"></a>JavaScript işlevlerinde özel telemetrisi günlüğe kaydet

[Application Insights Node. js SDK 'sı](https://github.com/microsoft/applicationinsights-node.js)ile özel telemetri gönderen örnek bir kod parçacığı aşağıda verilmiştir:

```javascript
const appInsights = require("applicationinsights");
appInsights.setup();
const client = appInsights.defaultClient;

module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    client.trackEvent({name: "my custom event", tagOverrides:{"ai.operation.id": context.invocationId}, properties: {customProperty2: "custom property value"}});
    client.trackException({exception: new Error("handled exceptions can be logged with this method"), tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackMetric({name: "custom metric", value: 3, tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackTrace({message: "trace message", tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackDependency({target:"http://dbname", name:"select customers proc", data:"SELECT * FROM Customers", duration:231, resultCode:0, success: true, dependencyTypeName: "ZSQL", tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackRequest({name:"GET /customers", url:"http://myserver/customers", duration:309, resultCode:200, success:true, tagOverrides:{"ai.operation.id": context.invocationId}});

    context.done();
};
```

`tagOverrides` Parametresi ,`operation_Id` öğesini işlevin çağırma kimliğine ayarlar. Bu ayar, belirli bir işlev çağrısı için otomatik olarak oluşturulan ve özel telemetrinin tümünü ilişkilendirmenizi sağlar.

## <a name="dependencies"></a>Bağımlılıkları

İşlev v2, HTTP istekleri, ServiceBus ve SQL bağımlılıklarını otomatik olarak toplar.

Bağımlılıkları göstermek için özel kod yazabilirsiniz. Örnekler için [ C# özel telemetri bölümünde](#log-custom-telemetry-in-c-functions)örnek koda bakın. Örnek kod, aşağıdaki görüntüde olduğu gibi Application Insights bir *uygulama Haritası* ile sonuçlanır:

![Uygulama haritası](./media/functions-monitoring/app-map.png)

## <a name="report-issues"></a>Sorunları raporla

Işlevlerde Application Insights tümleştirmeyle ilgili bir sorun bildirmek veya bir öneri veya istek yapmak için [GitHub 'da bir sorun oluşturun](https://github.com/Azure/Azure-Functions/issues/new).

## <a name="streaming-logs"></a>Akış Günlükleri

Bir uygulama geliştirirken genellikle günlük bilgileri neredeyse gerçek zamanlı olarak görmeniz yararlı olur. İşlevleriniz tarafından oluşturulan günlük dosyalarının akışını, Azure portal veya yerel bilgisayarınızdaki bir komut satırı oturumunda görüntüleyebilirsiniz.

Bu, [yerel geliştirme](functions-develop-local.md)sırasında işlevlerinizi hata ayıkladığınızda görülen çıkışın eşdeğeridir. Daha fazla bilgi için bkz. [günlüklerin akışı](../app-service/troubleshoot-diagnostic-logs.md#streamlogs).

> [!NOTE]
> Akış günlükleri, Işlevlerin yalnızca tek bir örneğini destekler. İşleviniz birden çok örneğe ölçeklenirse, diğer örneklerden gelen veriler günlük akışında gösterilmez. Application Insights [canlı ölçüm akışı](../azure-monitor/app/live-stream.md) birden çok örnek destekliyordu. Ayrıca, neredeyse gerçek zamanlı olarak, Akış Analizi [örneklenmiş verileri](#configure-sampling)de temel alır.

### <a name="portal"></a>Portal

Portalda akış günlüklerini görüntülemek için, işlev uygulamanızda **platform özellikleri** sekmesini seçin. Ardından, **izleme**altında **günlük akışı**' nı seçin.

![Portalda akış günlüklerini etkinleştirme](./media/functions-monitoring/enable-streaming-logs-portal.png)

Bu, uygulamanızı günlük akış hizmetine bağlar ve uygulama günlükleri pencerede görüntülenir. **Uygulama günlükleri** ve **Web sunucusu günlükleri**arasında geçiş yapabilirsiniz.  

![Portalda akış günlüklerini görüntüleme](./media/functions-monitoring/streaming-logs-window.png)

### <a name="visual-studio-code"></a>Visual Studio Code

[!INCLUDE [functions-enable-log-stream-vs-code](../../includes/functions-enable-log-stream-vs-code.md)]

### <a name="azure-cli"></a>Azure CLI

[Azure CLI](/cli/azure/install-azure-cli)kullanarak akış günlüklerini etkinleştirebilirsiniz. Oturum açmak için aşağıdaki komutları kullanın, aboneliğinizi seçin ve günlük dosyalarını akışla açın:

```azurecli
az login
az account list
az account set --subscription <subscriptionNameOrId>
az webapp log tail --resource-group <RESOURCE_GROUP_NAME> --name <FUNCTION_APP_NAME>
```

### <a name="azure-powershell"></a>Azure PowerShell

Akış günlüklerini, [Azure PowerShell](/powershell/azure/overview)kullanarak etkinleştirebilirsiniz. PowerShell için aşağıdaki komutları kullanarak Azure hesabınızı ekleyin, aboneliğinizi seçin ve günlük dosyalarını akışla açın:

```powershell
Add-AzAccount
Get-AzSubscription
Get-AzSubscription -SubscriptionName "<subscription name>" | Select-AzSubscription
Get-AzWebSiteLog -Name <FUNCTION_APP_NAME> -Tail
```

## <a name="disable-built-in-logging"></a>Yerleşik günlüğe kaydetmeyi devre dışı bırak

Application Insights etkinleştirdiğinizde, Azure Storage kullanan yerleşik günlüğe kaydetmeyi devre dışı bırakın. Yerleşik günlük kaydı, hafif iş yükleri ile test etmek için kullanışlıdır, ancak yüksek yük üretim kullanımı için tasarlanmamıştır. Üretim izleme için Application Insights önerilir. Üretim ortamında yerleşik günlük kullanılıyorsa, Azure Storage üzerinde azaltma nedeniyle günlüğe kaydetme kaydı tamamlanmamış olabilir.

Yerleşik günlüğe kaydetmeyi devre dışı bırakmak için `AzureWebJobsDashboard` uygulama ayarını silin. Azure portal uygulama ayarlarının nasıl silineceği hakkında daha fazla bilgi için, [bir işlev uygulamasını yönetme](functions-how-to-use-azure-function-app-settings.md#settings)konusunun **uygulama ayarları** bölümüne bakın. Uygulama ayarını silmeden önce, aynı işlev uygulamasındaki mevcut bir işlev olmadığından emin olun, Azure depolama Tetikleyicileri veya bağlamaları için ayarı kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Application Insights](/azure/application-insights/)
* [Günlüğe kaydetme ASP.NET Core](/aspnet/core/fundamentals/logging/)

[Host. JSON]: functions-host-json.md
