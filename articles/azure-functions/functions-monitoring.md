---
title: Azure İşlevlerini İzleme
description: İşlev yürütmeyi izlemek için Azure Application Insights Azure Işlevleri 'ni kullanmayı öğrenin.
ms.assetid: 501722c3-f2f7-4224-a220-6d59da08a320
ms.topic: conceptual
ms.date: 04/04/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: 578e1580bdaafb1b309a7af44353602cc31cb5a5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85207016"
---
# <a name="monitor-azure-functions"></a>Azure İşlevlerini İzleme

[Azure işlevleri](functions-overview.md) , işlevleri Izlemek için [Azure Application Insights](../azure-monitor/app/app-insights-overview.md) ile yerleşik tümleştirme sunar. Bu makalede, Azure Işlevlerinin Application Insights sistem tarafından oluşturulan günlük dosyalarını göndermek üzere nasıl yapılandırılacağı gösterilmektedir.

Günlük, performans ve hata verilerini topladığı için Application Insights kullanmanızı öneririz. Performans sorunlarını otomatik olarak algılar ve sorunları tanılamanıza ve işlevlerinizin nasıl kullanıldığını anlamanıza yardımcı olacak güçlü analiz araçları içerir. Performansı ve kullanılabilirliği sürekli geliştirmenize yardımcı olmak amacıyla tasarlanmıştır. Application Insights, yerel işlev uygulama projesi geliştirme sırasında bile kullanabilirsiniz. Daha fazla bilgi için bkz. [Application Insights nedir?](../azure-monitor/app/app-insights-overview.md).

Gerekli Application Insights izleme Azure Işlevleri 'nde yerleşik olduğundan, işlev uygulamanızı bir Application Insights kaynağına bağlamak için tüm ihtiyacınız olan geçerli bir izleme anahtarıdır. İşlev uygulaması kaynağınız Azure 'da oluşturulduğunda, izleme anahtarı uygulama ayarlarınıza eklenmelidir. İşlev uygulamanız zaten bu anahtara sahip değilse, [el ile ayarlayabilirsiniz](#enable-application-insights-integration).  

## <a name="application-insights-pricing-and-limits"></a>Fiyatlandırma ve limitleri Application Insights

Azure Işlevleri ile Application Insights tümleştirmeyi ücretsiz olarak deneyebilirsiniz. Ücretsiz olarak ne kadar veri işlenebileceğini gösteren günlük bir sınır vardır. Sınama sırasında bu sınıra ulaşırsınız. Günlük sınırınıza yaklaşdığınızda Azure, Portal ve e-posta bildirimleri sağlar. Bu uyarıları kaçırırsanız ve Sınıra ulaşırsanız yeni Günlükler Application Insights sorgularda görünmez. Gereksiz sorun giderme zamanından kaçının limiti göz önünde bulundurun. Daha fazla bilgi için bkz. [Application Insights fiyatlandırma ve veri hacmini yönetme](../azure-monitor/app/pricing.md).

> [!IMPORTANT]
> Application Insights, yoğun yük saatlerinde tamamlanan yürütmeler üzerinde çok fazla telemetri verisi üretmenin bir [örnekleme](../azure-monitor/app/sampling.md) özelliğine sahiptir. Örnekleme varsayılan olarak etkindir. Eksik veri olduğunu görürseniz, örnekleme ayarlarını belirli izleme senaryonuza uyacak şekilde ayarlamanız gerekebilir. Daha fazla bilgi için bkz. [örneklemesi yapılandırma](#configure-sampling).

İşlev uygulamanız için kullanılabilen Application Insights özelliklerinin tam listesi, [Azure işlevleri tarafından desteklenen özellikler için Application Insights](../azure-monitor/app/azure-functions-supported-features.md)ayrıntılı olarak açıklanmıştır.

## <a name="view-telemetry-in-monitor-tab"></a>Izleyici sekmesinde Telemetriyi görüntüleme

[Application Insights tümleştirme etkin](#enable-application-insights-integration)olduğunda, telemetri verilerini **izleyici** sekmesinde görüntüleyebilirsiniz.

1. İşlev uygulaması sayfasında, Application Insights yapılandırıldıktan sonra en az bir kez çalışan bir işlev seçin. Ardından, sol bölmeden **izleyici** ' yi seçin. İşlev etkinleştirmeleri listesi görünene kadar düzenli aralıklarla **Yenile** ' yi seçin.

   ![Etkinleştirmeleri listesi](media/functions-monitoring/monitor-tab-ai-invocations.png)

    > [!NOTE]
    > Telemetri istemcisi sunucuya iletilmek üzere verileri toplu olarak işlerken listenin görünmesi beş dakikaya kadar sürebilir. Gecikme [canlı ölçüm akışı](../azure-monitor/app/live-stream.md)uygulanmaz. Bu hizmet, sayfayı yüklediğinizde Işlevler ana bilgisayarına bağlanır, bu nedenle Günlükler doğrudan sayfaya akışlanır.

1. Belirli bir işlev çağrısının günlüklerini görmek için, bu çağrının **Tarih (UTC)** sütunu bağlantısını seçin. Bu çağrının günlüğe kaydetme çıktısı yeni bir sayfada görüntülenir.

   ![Çağırma ayrıntıları](media/functions-monitoring/invocation-details-ai.png)

1. Azure günlüğünde Azure Izleyici günlük verilerini alan sorgunun kaynağını görüntülemek için **Application Insights Içinde Çalıştır '** ı seçin. Aboneliğinizde Azure Log Analytics 'yi ilk kez kullanıyorsanız etkinleştirmeniz istenir.

1. Log Analytics etkinleştirdikten sonra, aşağıdaki sorgu görüntülenir. Sorgu sonuçlarının son 30 güne () sınırlı olduğunu görebilirsiniz `where timestamp > ago(30d)` . Ayrıca, sonuçlar 20 ' den fazla satır ( `take 20` ) göstermez. Buna karşılık, işlevinizin çağırma ayrıntıları listesi, son 30 gün için sınır olmadan olur.

   ![Application Insights Analytics çağırma listesi](media/functions-monitoring/ai-analytics-invocation-list.png)

Daha fazla bilgi için bu makalenin ilerleyen kısımlarında yer alarak [telemetri verilerini sorgulama](#query-telemetry-data) bölümüne bakın.

## <a name="view-telemetry-in-application-insights"></a>Application Insights telemetri görüntüleme

Azure portal bir işlev uygulamasından Application Insights açmak için sol taraftaki sayfada **Ayarlar** ' ın altında **Application Insights** ' yı seçin. Aboneliğiniz ile Application Insights ilk kez kullanıyorsanız, etkinleştirmeniz istenir: **Application Insights aç**' ı seçin ve ardından sonraki sayfada **Uygula** ' yı seçin.

![İşlev uygulamasına genel bakış sayfasından Application Insights açın](media/functions-monitoring/ai-link.png)

Application Insights kullanma hakkında daha fazla bilgi için [Application Insights belgelerine](https://docs.microsoft.com/azure/application-insights/)bakın. Bu bölümde Application Insights verilerin nasıl görüntüleneceği hakkında bazı örnekler gösterilmektedir. Application Insights zaten hakkında bilginiz varsa [telemetri verilerini yapılandırma ve özelleştirme hakkındaki bölümlere](#configure-categories-and-log-levels)doğrudan gidebilirsiniz.

![Application Insights Genel Bakış sekmesi](media/functions-monitoring/metrics-explorer.png)

Aşağıdaki Application Insights, işlevinizdeki davranış, performans ve hataları değerlendirirken yararlı olabilir:

| Araştır | Açıklama |
| ---- | ----------- |
| **[Hatalar](../azure-monitor/app/asp-net-exceptions.md)** |  İşlev hatalarıyla ve sunucu özel durumlarına göre grafikler ve uyarılar oluşturun. **Işlem adı** işlev adıdır. Bağımlılıklar için özel telemetri uygulamadığınız takdirde Bağımlılıklardaki arızalar gösterilmez. |
| **[Performans](../azure-monitor/app/performance-counters.md)** | **Bulut rol örnekleri**başına kaynak kullanımını ve aktarım hızını görüntüleyerek performans sorunlarını analiz edin. Bu veriler, işlevlerin temeldeki kaynaklarınızın gerisinde bulunduğu hata ayıklama senaryolarında yararlı olabilir. |
| **[Ölçümler](../azure-monitor/app/metrics-explorer.md)** | Ölçümleri temel alan grafikler ve uyarılar oluşturun. Ölçümler, işlev etkinleştirmeleri, yürütme süresi ve başarı oranları sayısını içerir. |
| **[Canlı ölçümler](../azure-monitor/app/live-stream.md)** | Ölçüm verilerini neredeyse gerçek zamanlı olarak oluşturulan şekilde görüntüleyin. |

## <a name="query-telemetry-data"></a>Telemetri verilerini sorgulama

[Application Insights Analytics](../azure-monitor/app/analytics.md) , bir veritabanındaki tablo biçimindeki tüm telemetri verilerine erişmenizi sağlar. Analytics verileri ayıklamak, işlemek ve görselleştirmek için bir sorgu dili sağlar. 

Günlüğe kaydedilen olayları incelemek veya sorgulamak için **günlükleri** seçin.

![Analiz örneği](media/functions-monitoring/analytics-traces.png)

Son 30 dakika içinde çalışan başına isteklerin dağılımını gösteren bir sorgu örneği aşağıda verilmiştir.

<pre>
requests
| where timestamp > ago(30m) 
| summarize count() by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
</pre>

Kullanılabilir tablolar, sol taraftaki **şema** sekmesinde gösterilir. İşlev etkinleştirmeleri tarafından oluşturulan verileri aşağıdaki tablolarda bulabilirsiniz:

| Tablo | Açıklama |
| ----- | ----------- |
| **lerin** | Çalışma zamanı ve işlev kodu tarafından oluşturulan Günlükler. |
| **istekler** | Her işlev çağrısı için bir istek. |
| **larý** | Çalışma zamanı tarafından oluşturulan özel durumlar. |
| **customMetrics** | Başarılı ve başarısız çağırma sayısı, başarı oranı ve süre. |
| **customEvents** | Çalışma zamanı tarafından izlenen olaylar, örneğin: bir işlevi tetikleyen HTTP istekleri. |
| **performanceCounters** | İşlevlerin üzerinde çalıştığı sunucuların performansı hakkında bilgiler. |

Diğer tablolar, kullanılabilirlik testleri, istemci ve tarayıcı telemetri içindir. Verilere veri eklemek için özel telemetri uygulayabilirsiniz.

Her tablo içinde IŞLEVLERE özgü verilerden bazıları bir `customDimensions` alandır.  Örneğin, aşağıdaki sorgu günlük düzeyine sahip tüm izlemeleri alır `Error` .

<pre>
traces 
| where customDimensions.LogLevel == "Error"
</pre>

Çalışma zamanı `customDimensions.LogLevel` ve alanlarını sağlar `customDimensions.Category` . İşlev kodunuzda yazdığınız günlüklerde ek alanlar sağlayabilirsiniz. Bu makalenin sonraki kısımlarında bulunan [yapılandırılmış günlüğe](#structured-logging) bakın.

## <a name="configure-categories-and-log-levels"></a>Kategorileri ve günlük düzeylerini yapılandırma

Application Insights, özel yapılandırma olmadan kullanabilirsiniz. Varsayılan yapılandırma, yüksek hacimde veri oluşmasına neden olabilir. Visual Studio Azure aboneliği kullanıyorsanız, Application Insights için veri üst sınırına ulaşırsınız. Bu makalenin ilerleyen kısımlarında, işlevlerinizin Application Insights gönderileceği verileri yapılandırmayı ve özelleştirmeyi öğreneceksiniz. Bir işlev uygulaması için günlüğe kaydetme, dosyadaki [host.js] yapılandırılır.

### <a name="categories"></a>Kategoriler

Azure Işlevleri günlükçüsü, her günlük için bir *Kategori* içerir. Kategori, çalışma zamanı kodunun veya işlev kodunuzun günlüğü yazanın hangi kısmının olduğunu gösterir. Aşağıdaki grafikte, çalışma zamanının oluşturduğu günlüklerin ana kategorileri açıklanmaktadır. 

| Kategori | Açıklama |
| ----- | ----- | 
| Host.Results | Bu Günlükler Application Insights **istek** olarak gösterir. Bir işlevin başarısını veya başarısızlığını gösterir. Tüm bu Günlükler `Information` düzeyinde yazılır. Veya üzerine filtre yaparsanız `Warning` , bu verilerden herhangi birini görmezsiniz. |
| Host. toplayıcısı | Bu Günlükler, [yapılandırılabilir](#configure-the-aggregator) bir süre boyunca işlev çağırma sayısının sayısını ve ortalamasını sağlar. Varsayılan süre 30 saniye veya 1.000 sonuçdur, hangisi önce gelir. Günlükler, Application Insights 'daki **Customölçümler** tablosunda bulunabilir. Çalıştırma sayısı, başarı oranı ve süre örnekleri verilebilir. Tüm bu Günlükler `Information` düzeyinde yazılır. Veya üzerine filtre yaparsanız `Warning` , bu verilerden herhangi birini görmezsiniz. |

Bunlar dışındaki kategoriler için tüm Günlükler Application Insights içindeki **izlemeler** tablosunda kullanılabilir.

İle başlayan kategorilerin bulunduğu tüm Günlükler `Host` işlevler çalışma zamanı tarafından yazılır. **Başlatılan işlev** ve **işlev tamamlanan** günlüklerde kategori var `Host.Executor` . Başarılı çalıştırmalar için bu Günlükler `Information` düzeydir. Özel durumlar düzeyinde günlüğe kaydedilir `Error` . Çalışma zamanı ayrıca `Warning` düzey günlükleri oluşturur, örneğin, zarar kuyruğuna gönderilen kuyruk iletileri.

Işlevler çalışma zamanı, "Host" ile başlayan bir kategoriye sahip Günlükler oluşturur. Sürüm 1. x içinde,, `function started` `function executed` ve `function completed` günlükleri kategorisi vardır `Host.Executor` . 2. x sürümünden itibaren bu günlüklerde kategori vardır `Function.<YOUR_FUNCTION_NAME>` .

İşlev kodunuzda Günlükler yazarsanız, kategori olur `Function.<YOUR_FUNCTION_NAME>.User` ve herhangi bir günlük düzeyi olabilir. Işlevler çalışma zamanının 1. x sürümünde kategori olur `Function` .

### <a name="log-levels"></a>Günlük düzeyleri

Azure Işlevleri günlükçüsü, her günlük için bir *günlük düzeyi* de içerir. [LogLevel](/dotnet/api/microsoft.extensions.logging.loglevel) bir sabit listesi ve tamsayı kodu göreli önemi gösterir:

|LogLevel    |Kod|
|------------|---|
|İzleme       | 0 |
|Hata ayıklama       | 1 |
|Bilgi | 2 |
|Uyarı     | 3 |
|Hata       | 4 |
|Kritik    | 5 |
|Hiçbiri        | 6 |

Günlük düzeyi `None` sonraki bölümde açıklanmaktadır. 

### <a name="log-configuration-in-hostjson"></a>host.js'de günlük yapılandırması

Dosya [host.js] , bir işlev uygulamasının Application Insights ne kadar günlüğe göndereceğini yapılandırır. Her kategori için, gönderileceği en düşük günlük düzeyini belirtirsiniz. İki örnek vardır: ilk örnek, [sürüm 2. x ve sonraki](functions-versions.md#version-2x) sürümlerini işlevler çalışma zamanının (.NET Core ile) hedefliyor ve ikinci örnek sürüm 1. x çalışma zamanı içindir.

### <a name="version-2x-and-higher"></a>Sürüm 2. x ve üzeri

Işlev çalışma zamanının Sürüm V2. x ve sonraki sürümlerinde [.NET Core günlük kaydı filtresi hiyerarşisi](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#log-filtering)kullanılır. 

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

* Kategorisi olan Günlükler `Host.Results` veya `Function` Application Insights için yalnızca bir `Error` düzey ve yukarıya gönder. Düzey ve alt için Günlükler `Warning` yoksayıldı.
* Kategorisi olan Günlükler için `Host.Aggregator` tüm günlükleri Application Insights gönderin. `Trace`Günlük düzeyi, bazı günlükçülerin çağrı yaptığı `Verbose` , ancak `Trace` dosyada [host.js] kullanılan şeydir.
* Tüm diğer Günlükler için, `Information` Application Insights için yalnızca düzeyini ve üstünü gönderin.

' Deki kategori değeri, aynı değer ile başlayan tüm kategoriler için günlük kaydı denetimlerinde [host.js] . `Host`,,, vb. için denetim günlük [host.js] `Host.General` `Host.Executor` `Host.Results` .

[host.json] , aynı dizeyle başlayan birden çok kategori içeriyorsa, daha uzun olanlar önce eşleştirilir. Çalışma zamanından itibaren `Host.Aggregator` oturum açma haricinde `Error` , ancak şu `Host.Aggregator` düzeyde günlüğe kaydetmek istediğiniz her şeyi istediğinizi varsayalım `Information` :

### <a name="version-2x-and-later"></a>Sürüm 2. x ve üzeri

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

Bir kategorinin tüm günlüklerini gizlemek için günlük düzeyi ' ni kullanabilirsiniz `None` . Bu kategoriyle hiçbir günlük yazılmadı ve üzerinde günlük düzeyi yok.

## <a name="configure-the-aggregator"></a>Toplayıcısı yapılandırma

Önceki bölümde belirtildiği gibi, çalışma zamanı bir süre boyunca işlev yürütmeleri hakkındaki verileri toplar. Varsayılan süre 30 saniye veya 1.000 çalışma olur ve hangisi önce gelir. Bu ayarı, [host.jsdosya üzerinde] yapılandırabilirsiniz.  İşte bir örnek:

```json
{
    "aggregator": {
      "batchSize": 1000,
      "flushTimeout": "00:00:30"
    }
}
```

## <a name="configure-sampling"></a>Örnekleme yapılandırma

Application Insights, yoğun yük saatlerinde tamamlanan yürütmeler üzerinde çok fazla telemetri verisi üretmenin bir [örnekleme](../azure-monitor/app/sampling.md) özelliğine sahiptir. Gelen yürütmeler oranı belirtilen eşiği aştığında Application Insights, gelen yürütmelerin bazılarını rastgele yok saymaya başlar. Saniyedeki en fazla yürütme sayısı için varsayılan ayar 20 ' dir (sürüm 1. x içinde beş). [Üzerindehost.js](https://docs.microsoft.com/azure/azure-functions/functions-host-json#applicationinsights)örnekleme yapılandırabilirsiniz.  İşte bir örnek:

### <a name="version-2x-and-later"></a>Sürüm 2. x ve üzeri

```json
{
  "logging": {
    "applicationInsights": {
      "samplingSettings": {
        "isEnabled": true,
        "maxTelemetryItemsPerSecond" : 20,
        "excludedTypes": "Request"
      }
    }
  }
}
```

2. x sürümünde belirli telemetri türlerini örnekleme dışında bırakabilirsiniz. Yukarıdaki örnekte, türündeki veriler `Request` örnekleme dışında tutulur. Bu, diğer telemetri türleri örneklemeye tabi kaldığı sürece *Tüm* işlev yürütmelerinin (istekler) günlüğe kaydedilmesini sağlar.

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

## <a name="write-logs-in-c-functions"></a>C# işlevlerinde yazma günlükleri

İşlev kodunuzda, Application Insights izlemeler olarak görünen Günlükler yazabilirsiniz.

### <a name="ilogger"></a>ILogger

İşlevinizdeki bir parametre yerine bir [ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger) parametresi kullanın `TraceWriter` . Kullanılarak oluşturulan Günlükler `TraceWriter` Application Insights git, ancak `ILogger` [yapılandırılmış günlüğe kaydetme](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)yapmanızı sağlar.

Bir `ILogger` nesnesi ile, `Log<level>` günlük oluşturmak Için [ILogger üzerinde uzantı yöntemleri](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.loggerextensions#methods) çağırın. Aşağıdaki kod, `Information` "function. <YOUR_FUNCTION_NAME> kategorisi ile günlükleri yazar. Kullanıcı. "

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

Günlükçü yöntemi çağrın bir önceki örneğe benzei durumunda, alanı sorgulayabilirsiniz `customDimensions.prop__rowKey` . `prop__`Çalışma zamanı eklemeleri ve işlev kodunuzun eklediği alanlar arasında çakışma olmadığından emin olmak için ön ek eklenir.

Ayrıca, alanına başvurarak özgün ileti dizesinde sorgulama yapabilirsiniz `customDimensions.prop__{OriginalFormat}` .  

Aşağıda, verilerin JSON örnek bir gösterimi verilmiştir `customDimensions` :

```json
{
  "customDimensions": {
    "prop__{OriginalFormat}":"C# Queue trigger function processed: {message}",
    "Category":"Function",
    "LogLevel":"Information",
    "prop__message":"c9519cbf-b1e6-4b9b-bf24-cb7d10b1bb89"
  }
}
```

### <a name="custom-metrics-logging"></a>Özel Ölçüm günlüğü

C# betik işlevlerinde, `LogMetric` `ILogger` Application Insights ' de özel ölçümler oluşturmak için genişletme yöntemini kullanabilirsiniz. Örnek bir yöntem çağrısı aşağıda verilmiştir:

```csharp
logger.LogMetric("TestMetric", 1234);
```

Bu kod, `TrackMetric` .NET için Application Insights API 'si kullanılarak çağırmanın bir alternatifidir.

## <a name="write-logs-in-javascript-functions"></a>JavaScript işlevlerinde yazma günlükleri

Node.js işlevlerde `context.log` günlükleri yazmak için kullanın. Yapılandırılmış günlüğe kaydetme etkin değil.

```
context.log('JavaScript HTTP trigger function processed a request.' + context.invocationId);
```

### <a name="custom-metrics-logging"></a>Özel Ölçüm günlüğü

Işlevler çalışma zamanının [1. x sürümünde](functions-versions.md#creating-1x-apps) çalışırken, Node.js işlevleri `context.log.metric` Application Insights içinde özel ölçümler oluşturmak için yöntemini kullanabilir. Bu yöntem şu anda sürüm 2. x ve üzeri sürümlerde desteklenmiyor. Örnek bir yöntem çağrısı aşağıda verilmiştir:

```javascript
context.log.metric("TestMetric", 1234);
```

Bu kod, `trackMetric` Application Insights için Node.js SDK kullanılarak çağırmanın bir alternatifidir.

## <a name="log-custom-telemetry-in-c-functions"></a>C# işlevlerinde özel telemetriyi günlüğe kaydetme

Application Insights SDK 'nın İşlevlerinizden Özel telemetri verilerini Application Insights: [Microsoft. Azure. WebJobs. Logging. ApplicationInsights](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Logging.ApplicationInsights)'a göndermek Için kullanabileceğiniz işlevlere özgü bir sürümü vardır. Bu paketi yüklemek için komut isteminde aşağıdaki komutu kullanın:

# <a name="command"></a>[Komut](#tab/cmd)

```cmd
dotnet add package Microsoft.Azure.WebJobs.Logging.ApplicationInsights --version <VERSION>
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
Install-Package Microsoft.Azure.WebJobs.Logging.ApplicationInsights -Version <VERSION>
```

---

Bu komutta, ' nin `<VERSION>` yüklü [Microsoft. Azure. WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs/)sürümünüzü destekleyen bir sürümü ile değiştirin. 

Aşağıdaki C# örnekleri [özel TELEMETRI API](../azure-monitor/app/api-custom-events-metrics.md)'sini kullanır. Örnek bir .NET sınıf kitaplığı içindir, ancak Application Insights kodu C# betiği için aynıdır.

### <a name="version-2x-and-later"></a>Sürüm 2. x ve üzeri

Çalışma zamanının sürüm 2. x ve sonraki sürümleri, geçerli işlemle Telemetriyi otomatik olarak ilişkilendirmek için Application Insights yeni özellikleri kullanır. İşlem `Id` , veya alanlarını el ile ayarlamanız gerekmez `ParentId` `Name` .

```cs
using System;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.ApplicationInsights.Extensibility;
using System.Linq;

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

[GetMetric](../azure-monitor/app/api-custom-events-metrics.md#getmetric) , bir ölçüm oluşturmak için şu anda önerilen API 'dir.

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

`TrackRequest` `StartOperation<RequestTelemetry>` Bir işlev çağrısı için yinelenen istekler göreceğiniz için veya çağrısı yapmayın.  Işlevler çalışma zamanı istekleri otomatik olarak izler.

Ayarlanmadı `telemetryClient.Context.Operation.Id` . Aynı anda çok sayıda işlev çalışırken bu genel ayar yanlış bağıntı oluşmasına neden olur. Bunun yerine, yeni bir telemetri örneği ( `DependencyTelemetry` , `EventTelemetry` ) oluşturun ve `Context` özelliğini değiştirin. Ardından Telemetri örneğini `Track` `TelemetryClient` ( `TrackDependency()` , `TrackEvent()` ,) üzerinde ilgili yönteme geçirin `TrackMetric()` . Bu yöntem, telemetrinin geçerli işlev çağırma için doğru bağıntı ayrıntılarına sahip olmasını sağlar.

## <a name="log-custom-telemetry-in-javascript-functions"></a>JavaScript işlevlerinde özel telemetriyi günlüğe kaydetme

[Application Insights Node.js SDK](https://github.com/microsoft/applicationinsights-node.js)ile özel telemetri gönderen örnek kod parçacıkları aşağıda verilmiştir:

### <a name="version-2x-and-later"></a>Sürüm 2. x ve üzeri

```javascript
const appInsights = require("applicationinsights");
appInsights.setup();
const client = appInsights.defaultClient;

module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    // Use this with 'tagOverrides' to correlate custom telemetry to the parent function invocation.
    var operationIdOverride = {"ai.operation.id":context.traceContext.traceparent};

    client.trackEvent({name: "my custom event", tagOverrides:operationIdOverride, properties: {customProperty2: "custom property value"}});
    client.trackException({exception: new Error("handled exceptions can be logged with this method"), tagOverrides:operationIdOverride);
    client.trackMetric({name: "custom metric", value: 3, tagOverrides:operationIdOverride});
    client.trackTrace({message: "trace message", tagOverrides:operationIdOverride});
    client.trackDependency({target:"http://dbname", name:"select customers proc", data:"SELECT * FROM Customers", duration:231, resultCode:0, success: true, dependencyTypeName: "ZSQL", tagOverrides:operationIdOverride});
    client.trackRequest({name:"GET /customers", url:"http://myserver/customers", duration:309, resultCode:200, success:true, tagOverrides:operationIdOverride});

    context.done();
};
```

### <a name="version-1x"></a>Sürüm 1. x

```javascript
const appInsights = require("applicationinsights");
appInsights.setup();
const client = appInsights.defaultClient;

module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    // Use this with 'tagOverrides' to correlate custom telemetry to the parent function invocation.
    var operationIdOverride = {"ai.operation.id":context.operationId};

    client.trackEvent({name: "my custom event", tagOverrides:operationIdOverride, properties: {customProperty2: "custom property value"}});
    client.trackException({exception: new Error("handled exceptions can be logged with this method"), tagOverrides:operationIdOverride);
    client.trackMetric({name: "custom metric", value: 3, tagOverrides:operationIdOverride});
    client.trackTrace({message: "trace message", tagOverrides:operationIdOverride});
    client.trackDependency({target:"http://dbname", name:"select customers proc", data:"SELECT * FROM Customers", duration:231, resultCode:0, success: true, dependencyTypeName: "ZSQL", tagOverrides:operationIdOverride});
    client.trackRequest({name:"GET /customers", url:"http://myserver/customers", duration:309, resultCode:200, success:true, tagOverrides:operationIdOverride});

    context.done();
};
```

`tagOverrides`Parametresi, öğesini `operation_Id` IŞLEVIN çağırma kimliğine ayarlar. Bu ayar, belirli bir işlev çağrısı için otomatik olarak oluşturulan ve özel telemetrinin tümünü ilişkilendirmenizi sağlar.

## <a name="dependencies"></a>Bağımlılıklar

İşlev v2, HTTP istekleri, ServiceBus, EventHub ve SQL bağımlılıklarını otomatik olarak toplar.

Bağımlılıkları göstermek için özel kod yazabilirsiniz. Örnekler için [C# özel telemetri bölümünde](#log-custom-telemetry-in-c-functions)örnek koda bakın. Örnek kod, aşağıdaki görüntüde olduğu gibi Application Insights bir *uygulama Haritası* ile sonuçlanır:

![Uygulama eşlemesi](./media/functions-monitoring/app-map.png)

## <a name="enable-application-insights-integration"></a>Application Insights tümleştirmesini etkinleştirme

Bir işlev uygulamasının Application Insights verileri gönderebilmesi için, bir Application Insights kaynağının izleme anahtarını bilmeleri gerekir. Anahtar, **APPINSIGHTS_INSTRUMENTATIONKEY**adlı bir uygulama ayarında olmalıdır.

İşlev uygulamanızı [Azure Portal](functions-create-first-azure-function.md)oluşturduğunuzda, komut satırından [Azure Functions Core Tools](functions-create-first-azure-function-azure-cli.md)veya [Visual Studio Code](functions-create-first-function-vs-code.md)kullanarak Application Insights tümleştirme varsayılan olarak etkinleştirilir. Application Insights kaynak, işlev uygulamanızla aynı ada sahiptir ve aynı bölgede ya da en yakın bölgede oluşturulur.

### <a name="new-function-app-in-the-portal"></a>Portalda yeni işlev uygulaması

Oluşturulan Application Insights kaynağını gözden geçirmek için, **Application Insights** penceresini genişletmek üzere seçin. **Yeni kaynak adını** değiştirebilir veya verilerinizi depolamak istediğiniz [Azure Coğrafya](https://azure.microsoft.com/global-infrastructure/geographies/) 'da farklı bir **konum** seçebilirsiniz.

![İşlev uygulaması oluştururken Application Insights etkinleştirme](media/functions-monitoring/enable-ai-new-function-app.png)

**Oluştur**' u seçtiğinizde, işlev uygulamanız ile uygulama ayarlarında ayarlanmış olan bir Application Insights kaynak oluşturulur `APPINSIGHTS_INSTRUMENTATIONKEY` . Her şey başlamaya hazırlanıyor.

<a id="manually-connect-an-app-insights-resource"></a>
### <a name="add-to-an-existing-function-app"></a>Mevcut bir işlev uygulamasına ekleme 

[Visual Studio](functions-create-your-first-function-visual-studio.md)kullanarak bir işlev uygulaması oluşturduğunuzda, Application Insights kaynağını oluşturmanız gerekir. Daha sonra bu kaynaktaki izleme anahtarını, işlev uygulamanızda bir [uygulama ayarı](functions-how-to-use-azure-function-app-settings.md#settings) olarak ekleyebilirsiniz.

[!INCLUDE [functions-connect-new-app-insights.md](../../includes/functions-connect-new-app-insights.md)]

Işlevlerin erken sürümleri, artık önerilmeyen yerleşik izleme kullanır. Böyle bir işlev uygulaması için Application Insights tümleştirmesinin etkinleştirilmesinde, [yerleşik günlüğü de devre dışı bırakmanız](#disable-built-in-logging)gerekir.  

## <a name="report-issues"></a>Sorun bildirme

Işlevlerde Application Insights tümleştirmeyle ilgili bir sorun bildirmek veya bir öneri veya istek yapmak için [GitHub 'da bir sorun oluşturun](https://github.com/Azure/Azure-Functions/issues/new).

## <a name="streaming-logs"></a>Akış günlükleri

Bir uygulama geliştirirken genellikle Azure 'da çalışırken neredeyse gerçek zamanlı olarak günlüklere ne yazıldığını görmek istersiniz.

İşlev yürütmeleri tarafından oluşturulan günlük dosyalarının akışını görüntülemenin iki yolu vardır.

* **Yerleşik günlük akışı**: App Service platformu, uygulama günlüğü dosyalarınızın akışını görüntülemenize olanak sağlar. Bu, [yerel geliştirme](functions-develop-local.md) sırasında işlevlerinizi hata ayıkladığınızda ve portalda **Test** sekmesini kullandığınızda görülen çıkış ile eşdeğerdir. Günlük tabanlı tüm bilgiler görüntülenir. Daha fazla bilgi için bkz. [akış günlükleri](../app-service/troubleshoot-diagnostic-logs.md#stream-logs). Bu akış yöntemi yalnızca tek bir örneği destekler ve bir tüketim planında Linux üzerinde çalışan bir uygulamayla birlikte kullanılamaz.

* **Canlı ölçüm akışı**: işlev uygulamanız [Application Insights bağlandığı](#enable-application-insights-integration)zaman, Azure Portal [canlı ölçüm akışı](../azure-monitor/app/live-stream.md)kullanarak günlük verilerini ve diğer ölçümleri neredeyse gerçek zamanlı olarak görebilirsiniz. Bir tüketim planında birden çok örnek veya Linux üzerinde çalışan işlevleri izlerken bu yöntemi kullanın. Bu yöntem [örneklenmiş verileri](#configure-sampling)kullanır.

Günlük akışları hem portalda hem de birçok yerel geliştirme ortamında görüntülenebilir. 

### <a name="portal"></a>Portal

Portalda her iki tür günlük akışı görüntüleyebilirsiniz.

#### <a name="built-in-log-streaming"></a>Yerleşik günlük akışı

Portalda akış günlüklerini görüntülemek için, işlev uygulamanızda **platform özellikleri** sekmesini seçin. Ardından, **izleme**altında **günlük akışı**' nı seçin.

![Portalda akış günlüklerini etkinleştirme](./media/functions-monitoring/enable-streaming-logs-portal.png)

Bu, uygulamanızı günlük akış hizmetine bağlar ve uygulama günlükleri pencerede görüntülenir. **Uygulama günlükleri** ve **Web sunucusu günlükleri**arasında geçiş yapabilirsiniz.  

![Portalda akış günlüklerini görüntüleme](./media/functions-monitoring/streaming-logs-window.png)

#### <a name="live-metrics-stream"></a>Canlı Ölçüm Akışı

Uygulamanızın Canlı Ölçüm Akışı görüntülemek için, işlev uygulamanızın **genel bakış** sekmesini seçin. Application Insights etkinleştirdiğinizde, **yapılandırılmış özellikler**altında bir **Application Insights** bağlantısı görürsünüz. Bu bağlantı sizi uygulamanızın Application Insights sayfasına götürür.

Application Insights ' de **canlı ölçüm akışı**' ı seçin. [Örneklenir günlük girişleri](#configure-sampling) **örnek telemetri**altında görüntülenir.

![Portalda Canlı Ölçüm Akışı görüntüleme](./media/functions-monitoring/live-metrics-stream.png) 

### <a name="visual-studio-code"></a>Visual Studio Code

[!INCLUDE [functions-enable-log-stream-vs-code](../../includes/functions-enable-log-stream-vs-code.md)]

### <a name="core-tools"></a>Temel araçlar

[!INCLUDE [functions-streaming-logs-core-tools](../../includes/functions-streaming-logs-core-tools.md)]

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

## <a name="scale-controller-logs"></a>Denetleyici günlüklerini ölçeklendirme

[Azure işlevleri ölçek denetleyicisi](./functions-scale.md#runtime-scaling) , uygulamanızı çalıştıran işlev ana bilgisayar örneklerini izler ve işlev konak örneklerinin ne zaman ekleneceğini veya kaldırılacağını gösteren kararlar verir. Ölçek denetleyicisinin uygulamanızda yaptığı kararların anlaşılması gerekiyorsa, günlükleri Application Insights veya blob depolama alanına yaymak üzere yapılandırabilirsiniz.

> [!WARNING]
> Bu özellik önizlemede. Bu özelliği süresiz olarak bırakmanız önerilmez. bunun yerine, topladığı bilgilere ihtiyacınız olduğunda onu etkinleştirmeniz ve sonra devre dışı bırakmanız gerekir.

Bu özelliği etkinleştirmek için adlı yeni bir uygulama ayarı ekleyin `SCALE_CONTROLLER_LOGGING_ENABLED` . Bu ayarın değeri şu biçimde olmalıdır `{Destination}:{Verbosity}` :
* `{Destination}`gönderilecek günlüklerin hedefini belirtir ve ya da olmalıdır `AppInsights` `Blob` .
* `{Verbosity}`istediğiniz günlüğe kaydetme düzeyini belirtir ve, veya ' den biri olmalıdır `None` `Warning` `Verbose` .

Örneğin, ölçek denetleyicisinden Application Insights için ayrıntılı bilgileri günlüğe kaydetmek için değerini kullanın `AppInsights:Verbose` .

> [!NOTE]
> `AppInsights`Hedef türünü etkinleştirirseniz, [işlev uygulamanız için Application Insights](#enable-application-insights-integration)yapılandırmanıza emin olmanız gerekir.

Hedefi olarak ayarlarsanız `Blob` , Günlükler `azure-functions-scale-controller` uygulama ayarında ayarlanan depolama hesabı içinde adlı bir blob kapsayıcısında oluşturulur `AzureWebJobsStorage` .

Ayrıntı düzeyini olarak ayarlarsanız `Verbose` , ölçek denetleyicisi çalışan sayısında her değişiklik için bir neden, Ayrıca ölçek denetleyicinin kararına katılan Tetikleyiciler hakkındaki bilgileri günlüğe kaydeder. Örneğin, günlüklerde tetikleyici uyarıları ve ölçek denetleyicisi çalıştırılmadan önce ve sonra Tetikleyiciler tarafından kullanılan karmaları dahil edilir.

Ölçek denetleyicisi günlüğünü devre dışı bırakmak için, değerini olarak ayarlayın `{Verbosity}` `None` veya `SCALE_CONTROLLER_LOGGING_ENABLED` uygulama ayarını kaldırın.

## <a name="disable-built-in-logging"></a>Yerleşik günlüğe kaydetmeyi devre dışı bırak

Application Insights etkinleştirdiğinizde, Azure Storage kullanan yerleşik günlüğe kaydetmeyi devre dışı bırakın. Yerleşik günlük kaydı, hafif iş yükleri ile test etmek için kullanışlıdır, ancak yüksek yük üretim kullanımı için tasarlanmamıştır. Üretim izleme için Application Insights önerilir. Üretim ortamında yerleşik günlük kullanılıyorsa, Azure Storage üzerinde azaltma nedeniyle günlüğe kaydetme kaydı tamamlanmamış olabilir.

Yerleşik günlüğe kaydetmeyi devre dışı bırakmak için `AzureWebJobsDashboard` uygulama ayarını silin. Azure portal uygulama ayarlarının nasıl silineceği hakkında daha fazla bilgi için, [bir işlev uygulamasını yönetme](functions-how-to-use-azure-function-app-settings.md#settings)konusunun **uygulama ayarları** bölümüne bakın. Uygulama ayarını silmeden önce, aynı işlev uygulamasındaki mevcut bir işlev olmadığından emin olun, Azure depolama Tetikleyicileri veya bağlamaları için ayarı kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Uygulama Bilgileri](/azure/application-insights/)
* [Günlüğe kaydetme ASP.NET Core](/aspnet/core/fundamentals/logging/)

[Üzerindehost.js]: functions-host-json.md
