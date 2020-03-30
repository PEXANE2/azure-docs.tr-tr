---
title: Azure İşlevlerini İzleme
description: İşlev yürütmeyi izlemek için Azure İşlevleriyle Azure Uygulama Öngörülerini nasıl kullanacağınızı öğrenin.
ms.assetid: 501722c3-f2f7-4224-a220-6d59da08a320
ms.topic: conceptual
ms.date: 04/04/2019
ms.openlocfilehash: 08da17f1ef023676aa0c499cf4e7e1bb9687f1c9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80257862"
---
# <a name="monitor-azure-functions"></a>Azure İşlevlerini İzleme

[Azure İşlevleri,](functions-overview.md) işlevleri izlemek için [Azure Application Insights](../azure-monitor/app/app-insights-overview.md) ile yerleşik tümleştirme sunar. Bu makalede, Sistem tarafından oluşturulan günlük dosyalarını Application Insights'a göndermek için Azure İşlevlerini nasıl yapılandırabileceğiniz gösterilmektedir.

Günlük, performans ve hata verileri topladığı için Application Insights'ı kullanmanızı öneririz. Performans anormalliklerini otomatik olarak algılar ve sorunları tanılamanıza ve işlevlerinizin nasıl kullanıldığını anlamanıza yardımcı olacak güçlü analiz araçları içerir. Performansı ve kullanılabilirliği sürekli geliştirmenize yardımcı olmak amacıyla tasarlanmıştır. Yerel işlev uygulaması proje geliştirme sırasında Uygulama Öngörüleri'ni bile kullanabilirsiniz. Daha fazla bilgi için Bkz. [Uygulama Öngörüleri nedir?](../azure-monitor/app/app-insights-overview.md)

Gerekli Application Insights enstrümantasyonu Azure İşlevleri'nde yerleşik olduğundan, tek ihtiyacınız olan işlev uygulamanızı bir Application Insights kaynağına bağlamak için geçerli bir enstrümantasyon anahtarıdır. İşlev uygulama kaynağınız Azure'da oluşturulduğunda enstrümantasyon anahtarı uygulama ayarlarınıza eklenmelidir. İşlev uygulamanız zaten bu anahtara sahip değilse, [el ile ayarlayabilirsiniz.](#enable-application-insights-integration)  

## <a name="application-insights-pricing-and-limits"></a>Uygulama Öngörüleri fiyatlandırma ve limitleri

İşlev Uygulamaları ile Application Insights entegrasyonunu ücretsiz olarak deneyebilirsiniz. Ne kadar verinin ücretsiz olarak işlenebileceğinin günlük bir sınırı vardır. Test sırasında bu sınıra ulaşabilirsiniz. Azure, günlük sınırınıza yaklaşırken portal ve e-posta bildirimleri sağlar. Bu uyarıları kaçırır ve sınıra basarsanız, Application Insights sorgularında yeni günlükler görünmez. Gereksiz sorun giderme süresini önlemek için sınıra dikkat edin. Daha fazla bilgi için [Bkz. Uygulama Öngörüleri'nde fiyatlandırmayı ve veri hacmini yönet.](../azure-monitor/app/pricing.md)

İşlev uygulamanızda kullanılabilen Uygulama Öngörüleri özelliklerinin tam listesi [Azure İşlevleri için Uygulama Öngörüleri'nde](../azure-monitor/app/azure-functions-supported-features.md)ayrıntılı olarak açıklanmaktadır.

## <a name="view-telemetry-in-monitor-tab"></a>Monitör sekmesinde telemetriyi görüntüleme

[Application Insights tümleştirmesi etkinken,](#enable-application-insights-integration) **telemetri** verilerini Monitör sekmesinde görüntüleyebilirsiniz.

1. İşlev uygulaması sayfasında, Application Insights yapılandırıldıktan sonra en az bir kez çalışan bir işlev seçin. Ardından **Monitör** sekmesini seçin. İşlev çağrıları listesi görünene kadar düzenli olarak **Yenile'yi** seçin.

   ![Davetlistesi](media/functions-monitoring/monitor-tab-ai-invocations.png)

    > [!NOTE]
    > Telemetri istemcisi sunucuya iletim için verileri toplu hale getirirken listenin görünmesi beş dakika kadar sürebilir. Gecikme, [Canlı Ölçümler Akışı](../azure-monitor/app/live-stream.md)için geçerli değildir. Bu hizmet, sayfayı yüklediğinizde İşlevler ana bilgisayara bağlanır, böylece günlükler doğrudan sayfaya akışı sağlar.

1. Belirli bir işlev çağırmasının günlüklerini görmek için, bu çağırma için **Tarih (UTC)** sütun bağlantısını seçin. Bu çağırmanın günlüğe kaydetme çıktısı yeni bir sayfada görüntülenir.

   ![Çağırma ayrıntıları](media/functions-monitoring/invocation-details-ai.png)

1. Azure Günlüğü'nde Azure Monitörü günlük verilerini alan sorgunun kaynağını görüntülemek için **Uygulama Öngörülerinde Çalıştır** bağlantısını seçin Aboneliğinizde Azure Günlük Analizi'ni ilk kez kullanıyorsanız, bunu etkinleştirmeniz istenir.

1. Bu bağlantıyı seçtiğinizde ve Log Analytic'i etkinleştirmeyi seçtiğinizde. aşağıdaki sorgu görüntülenir. Sorgu sonuçlarının son 30 günle sınırlı olduğunu`where timestamp > ago(30d)`görebilirsiniz . Buna ek olarak, sonuçlar en fazla 20 satır`take 20`(). Buna karşılık, işleviniziçin çağırma ayrıntıları listesi son 30 gün için sınırsızdır.

   ![Uygulama Öngörüleri Analytics çağrı listesi](media/functions-monitoring/ai-analytics-invocation-list.png)

Daha fazla bilgi için, bu makalenin ilerleyen bilgilerine bakın [telemetri bilgilerini sorgula.](#query-telemetry-data)

## <a name="view-telemetry-in-application-insights"></a>Uygulama Öngörülerinde telemetriyi görüntüleyin

Azure portalındaki bir işlev uygulamasından Uygulama Öngörüleri'ni açmak için işlev uygulamasının **Genel Bakış** sayfasına gidin. **Yapılandırılmış özellikler** **altında, Uygulama Öngörüleri'ni**seçin.

![İşlev uygulaması Genel Bakış sayfasından Uygulama Öngörülerini Aç](media/functions-monitoring/ai-link.png)

Uygulama Öngörüleri'nin nasıl kullanılacağı hakkında bilgi için [Uygulama Öngörüleri belgelerine](https://docs.microsoft.com/azure/application-insights/)bakın. Bu bölümde, Uygulama Öngörüleri'nde verilerin nasıl görüntülenenenene ilişkin bazı örnekler gösterilmektedir. Uygulama Öngörüleri'ni zaten biliyorsanız, [telemetri verilerini nasıl yapılandırabileceğiniz ve özelleştirebileceğiniz le ilgili bölümlere](#configure-categories-and-log-levels)doğrudan gidebilirsiniz.

![Uygulama Öngörüleri Genel Bakış sekmesi](media/functions-monitoring/metrics-explorer.png)

Uygulama Öngörüleri'nin aşağıdaki alanları, işlevlerinizdeki davranış, performans ve hataları değerlendirirken yararlı olabilir:

| Araştır | Açıklama |
| ---- | ----------- |
| **[Başarısızlık](../azure-monitor/app/asp-net-exceptions.md)** |  İşlev hatalarına ve sunucu özel durumlarına göre grafikler ve uyarılar oluşturun. **İşlem Adı** işlev adıdır. Bağımlılıklar için özel telemetri uygulamadığınız sürece bağımlılıklarda hatalar gösterilmez. |
| **[Performans](../azure-monitor/app/performance-counters.md)** | Kaynak kullanımını ve **Bulut rol örnekleri**başına iş başına iş başına iş başına iş başına görüntüleyerek performans sorunlarını analiz edin. Bu veriler, işlevlerin temel kaynaklarınızı bozduğu senaryoları hata ayıklama için yararlı olabilir. |
| **[Ölçümler](../azure-monitor/app/metrics-explorer.md)** | Ölçümlere dayalı grafikler ve uyarılar oluşturun. Ölçümler, işlev çağrılarının sayısını, yürütme süresini ve başarı oranlarını içerir. |
| **[Canlı Ölçümler](../azure-monitor/app/live-stream.md)** | Metrik verileri neredeyse gerçek zamanlı olarak oluşturulduğu şekilde görüntüleyin. |

## <a name="query-telemetry-data"></a>Telemetri verilerini sorgula

[Application Insights Analytics,](../azure-monitor/app/analytics.md) veritabanındaki tablolar şeklindeki tüm telemetri verilerine erişmenizi sağlar. Analytics, verileri ayıklamak, işlemek ve görselleştirmek için bir sorgu dili sağlar. 

Günlüğe kaydedilmiş olayları keşfetmek veya sorgulamak için **Günlükler'i** seçin.

![Analitik örneği](media/functions-monitoring/analytics-traces.png)

Burada, son 30 dakika içinde işçi başına istek dağılımını gösteren bir sorgu örneği verilmiştir.

<pre>
requests
| where timestamp > ago(30m) 
| summarize count() by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
</pre>

Kullanılabilir tablolar soldaki **Şema** sekmesinde gösterilir. Aşağıdaki tablolarda işlev çağrıları tarafından oluşturulan verileri bulabilirsiniz:

| Tablo | Açıklama |
| ----- | ----------- |
| **Izler** | Çalışma zamanı ve işlev kodu tarafından oluşturulan günlükler. |
| **istekler** | Her işlev çağırma için bir istek. |
| **Özel durum** | Çalışma zamanı tarafından atılan özel durumlar. |
| **özelMetrikler** | Başarılı ve başarısız çağrıların sayısı, başarı oranı ve süre. |
| **Customevents** | Çalışma zamanı tarafından izlenen olaylar, örneğin: bir işlevi tetikleyen HTTP istekleri. |
| **performansSayaçları** | Işlevlerin çalıştığı sunucuların performansı hakkında bilgi. |

Diğer tablolar kullanılabilirlik testleri ve istemci ve tarayıcı telemetrisi içindir. Bunlara veri eklemek için özel telemetri uygulayabilirsiniz.

Her tabloda, Işlevlere özgü bazı veriler `customDimensions` bir alandadır.  Örneğin, aşağıdaki sorgu günlük düzeyi `Error`olan tüm izleri alır.

<pre>
traces 
| where customDimensions.LogLevel == "Error"
</pre>

Çalışma süresi `customDimensions.LogLevel` ve `customDimensions.Category` alanları sağlar. İşlev kodunuza yazdığınız günlüklerde ek alanlar sağlayabilirsiniz. Bu makalenin ilerleyen larına bakın [Yapılandırılmış günlük.](#structured-logging)

## <a name="configure-categories-and-log-levels"></a>Kategorileri ve günlük düzeylerini yapılandırma

Uygulama Öngörüleri'ni özel bir yapılandırma olmadan kullanabilirsiniz. Varsayılan yapılandırma yüksek hacimli veri neden olabilir. Visual Studio Azure aboneliği kullanıyorsanız, Uygulama Öngörüleri için veri kapağınıza basabilirsiniz. Bu makalenin ilerleyen saatlerinde, işlevlerinizin Uygulama Öngörüleri'ne gönderdiği verileri nasıl yapılandırabileceğinizi ve özelleştirdiğinizi öğrenirsiniz. Bir işlev uygulaması için, günlüğe kaydetme [ana bilgisayar.json] dosyasında yapılandırılır.

### <a name="categories"></a>Kategoriler

Azure İşlevler kaydedicisi her günlük için bir *kategori* içerir. Kategori, günlüğü runtime kodunun veya işlev kodunun hangi bölümünün yazdığını gösterir. Aşağıdaki grafikte, çalışma zamanının oluşturduğu günlüklerin ana kategorileri açıklanmaktadır. 

| Kategori | Açıklama |
| ----- | ----- | 
| Host.Results | Bu günlükler Uygulama Öngörüleri'nde **istek** olarak gösterin. Bir işlevin başarısını veya başarısızlığını gösterirler. Tüm bu günlükler `Information` düzeyinde yazılır. Filtreleederseniz `Warning` veya üzerinde filtre ler ederseniz, bu verilerin hiçbirini görmezsiniz. |
| Host.Toplayıcı | Bu günlükler, [yapılandırılabilir](#configure-the-aggregator) bir süre boyunca işlev çağrılarının sayılarını ve ortalamalarını sağlar. Varsayılan dönem 30 saniye veya 1.000 sonuç,hangisi önce gelirse. Günlükler, Uygulama **Öngörüleri'ndeki özel Metrics** tablosunda mevcuttur. Örnekler, çalıştırma sayısı, başarı oranı ve süredir. Tüm bu günlükler `Information` düzeyinde yazılır. Filtreleederseniz `Warning` veya üzerinde filtre ler ederseniz, bu verilerin hiçbirini görmezsiniz. |

Bunların dışındaki kategorilerin tüm günlükleri Uygulama **Öngörüleri'ndeki izleme** tablosunda mevcuttur.

Kategorilerle `Host` başlayan tüm günlükler, Işlevler çalışma zamanı tarafından yazılır. **İşlev başladı** ve **Fonksiyon** tamamlanan `Host.Executor`günlükleri kategorisi var. Başarılı çalıştırmalar için bu `Information` günlükler düzeydir. Özel durumlar `Error` düzeyinde günlüğe kaydedilir. Çalışma zamanı, örneğin `Warning` düzey günlükleri de oluşturur: zehirli sıraya gönderilen sıra iletileri.

İşlevler çalışma süresi , "Ana Bilgisayar" ile başlayan bir kategoriyle günlükler oluşturur. Sürüm 1.x'te `function started` `function executed`, `function completed` , ve günlükleri kategorisi `Host.Executor`ne var. Sürüm 2.x'ten başlayarak, bu `Function.<YOUR_FUNCTION_NAME>`günlükler kategorisine sahiptir.

İşlev kodunuza günlükler yazarsanız, kategori `Function.<YOUR_FUNCTION_NAME>.User` herhangi bir günlük düzeyidir ve olabilir. Fonksiyonlar çalışma zamanı sürüm 1.x'te `Function`kategori .

### <a name="log-levels"></a>Günlük düzeyleri

Azure İşlevler kaydedicisi, her *günlükte* bir günlük düzeyi de içerir. [LogLevel](/dotnet/api/microsoft.extensions.logging.loglevel) numaralandırmadır ve sonda kodu göreli önemi gösterir:

|LogLevel    |Kod|
|------------|---|
|İzleme       | 0 |
|Hata ayıklama       | 1 |
|Bilgi | 2 |
|Uyarı     | 3 |
|Hata       | 4 |
|Kritik    | 5 |
|None        | 6 |

Günlük `None` düzeyi sonraki bölümde açıklanmıştır. 

### <a name="log-configuration-in-hostjson"></a>Host.json'da günlük yapılandırması

[Host.json] dosyası, bir işlev uygulamasının Uygulama Öngörüleri'ne ne kadar günlüğe kaydetme gönderdiğini yapılandırır. Her kategori için, gönderilecek minimum günlük düzeyini belirtirsiniz. İki örnek vardır: ilk örnek, [2.x sürümünü ve daha sonraki](functions-versions.md#version-2x) Işlevleri çalışma süresini (.NET Core ile) hedefler ve ikinci örnek sürüm 1.x çalışma zamanıdır.

### <a name="version-2x-and-higher"></a>Sürüm 2.x ve üzeri

Sürüm v2.x ve Fonksiyonlar çalışma zamanı sonraki sürümleri [.NET Core günlük filtresi hiyerarşisi](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#log-filtering)kullanın. 

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

### <a name="version-1x"></a>Sürüm 1.x

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

Bu örnekte aşağıdaki kurallar yer alabı:

* Kategori `Host.Results` veya `Function`, yalnızca `Error` seviye ve üzeri olan günlükler için Uygulama Öngörüleri'ne gönderin. Düzey ve `Warning` alt günlükleri yoksayılır.
* Kategorili `Host.Aggregator`günlükler için, tüm günlükleri Application Insights'a gönderin. `Trace` Günlük düzeyi, bazı loggers ne `Verbose`aramak, `Trace` ancak [host.json] dosyasında kullanın aynıdır.
* Diğer tüm günlükler için `Information` yalnızca seviye ve üzerini Application Insights'a gönderin.

[Host.json'daki] kategori değeri, aynı değerle başlayan tüm kategoriler için günlüğe kaydetmeyi denetler. `Host`[host.json] denetimleri `Host.General`için `Host.Executor` `Host.Results`günlük , , , ve benzeri.

[Host.json] aynı dize ile başlayan birden çok kategori içeriyorsa, önce daha uzun kategoriler eşleşir. Düzeyde oturum açmak dışında `Host.Aggregator` çalışma saatinden itibaren her şeyi istediğinizi varsayalım, ancak `Host.Aggregator` `Information` bu düzeyde günlüğe kaydetmek istediğinizi varsayalım: `Error`

### <a name="version-2x-and-later"></a>Sürüm 2.x ve sonrası

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

### <a name="version-1x"></a>Sürüm 1.x 

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

Bir kategorinin tüm günlüklerini bastırmak için `None`günlük düzeyini kullanabilirsiniz. Bu kategoride hiçbir günlük yazMıyor ve üstünde günlük düzeyi yok.

## <a name="configure-the-aggregator"></a>Toplayıcıyı yapılandırma

Önceki bölümde belirtildiği gibi, çalışma zamanı belirli bir süre içinde işlev yürütmeleri hakkındaki verileri toplar. Varsayılan süre 30 saniye veya 1.000 çalıştırmadır (hangisi önce gelirse). Bu ayarı [ana bilgisayar.json] dosyasında yapılandırabilirsiniz.  Bir örneği aşağıda verilmiştir:

```json
{
    "aggregator": {
      "batchSize": 1000,
      "flushTimeout": "00:00:30"
    }
}
```

## <a name="configure-sampling"></a>Örneklemeyi yapılandırma

Application Insights, en yüksek yük olduğu zamanlarda tamamlanan yürütmeler hakkında çok fazla telemetri verisi üretmenizi engelleyen bir [örnekleme](../azure-monitor/app/sampling.md) özelliğine sahiptir. Gelen yürütme hızı belirtilen eşiği aştığında, Application Insights gelen yürütmelerin bazılarını rasgele yok saymaya başlar. Saniyede en fazla yürütme sayısı için varsayılan ayar 20'dir (sürüm 1.x'te beş). Örneklemeyi [host.json'da]yapılandırabilirsiniz.  Bir örneği aşağıda verilmiştir:

### <a name="version-2x-and-later"></a>Sürüm 2.x ve sonrası

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

### <a name="version-1x"></a>Sürüm 1.x 

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
> [Örnekleme](../azure-monitor/app/sampling.md) varsayılan olarak etkinleştirilir. Veri eksik görünüyorsanız, örnekleme ayarlarını belirli izleme senaryonuza uyacak şekilde ayarlamanız gerekebilir.

## <a name="write-logs-in-c-functions"></a>C# işlevlerinde yazma günlükleri

İşlev kodunuza Uygulama Öngörüleri'nde iz olarak görünen günlükler yazabilirsiniz.

### <a name="ilogger"></a>ILogger

Fonksiyonlarınızda `TraceWriter` parametre yerine [Bir ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger) parametresi kullanın. Uygulama Öngörüleri `TraceWriter` kullanılarak oluşturulan günlükler `ILogger` Application Insights'a gidin, ancak [yapılandırılmış günlük oluşturmanızı](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)sağlar.

Bir `ILogger` nesneyle, `Log<level>` günlükleri oluşturmak için [ILogger'da uzantı yöntemlerini](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.loggerextensions#methods) çağırırsınız. Aşağıdaki kod `Information` kategori "Function.<YOUR_FUNCTION_NAME> ile günlükleri yazar. Kullanıcı."

```cs
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger logger)
{
    logger.LogInformation("Request for item with key={itemKey}.", id);
```

### <a name="structured-logging"></a>Yapılandırılmış günlük

Yer tutucuların sırası, adlarını değil, günlük iletisinde hangi parametrelerin kullanılacağını belirler. Aşağıdaki koda sahip olduğunuzu varsayalım:

```csharp
string partitionKey = "partitionKey";
string rowKey = "rowKey";
logger.LogInformation("partitionKey={partitionKey}, rowKey={rowKey}", partitionKey, rowKey);
```

Aynı ileti dizesini tutar ve parametrelerin sırasını tersine çevirirseniz, ortaya çıkan ileti metni değerleri yanlış yerlerde olur.

Yer tutucular, yapılandırılmış günlük işlem yapabilmeniz için bu şekilde işlenir. Application Insights parametre ad değeri çiftleri ve ileti dizesini depolar. Sonuç olarak, ileti bağımsız değişkenleri sorgulayabileceğiniz alanlar haline gelir.

Logger yöntemi aramanız önceki örneğe benziyorsa, `customDimensions.prop__rowKey`alanı sorgulayabilirsiniz. Çalışma `prop__` zamanının eklediği alanlar ile işlev kodunun eklediği alanlar arasında çakıştırış olmadığından emin olmak için önek eklenir.

Ayrıca alana `customDimensions.prop__{OriginalFormat}`başvurarak özgün ileti dizesini sorgulayabilirsiniz.  

Aşağıda, verilerin örnek bir `customDimensions` JSON gösterimi vereb

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

### <a name="custom-metrics-logging"></a>Özel ölçümler günlük

C# komut dosyası işlevlerinde, `LogMetric` Uygulama `ILogger` Öngörüleri'nde özel ölçümler oluşturmak için uzantı yöntemini kullanabilirsiniz. Örnek bir yöntem çağrısı aşağıda veda edebilirsiniz:

```csharp
logger.LogMetric("TestMetric", 1234);
```

Bu kod, .NET `TrackMetric` için Application Insights API'yi kullanarak aramaya alternatiftir.

## <a name="write-logs-in-javascript-functions"></a>JavaScript işlevlerinde yazma günlükleri

Düğüm.js işlevlerinde günlük `context.log` yazmak için kullanın. Yapılandırılmış günlüğe kaydetme etkin değil.

```
context.log('JavaScript HTTP trigger function processed a request.' + context.invocationId);
```

### <a name="custom-metrics-logging"></a>Özel ölçümler günlük

İşlevler çalışma [zamanının 1.x sürümünde](functions-versions.md#creating-1x-apps) çalışırken, Node.js `context.log.metric` işlevleri Uygulama Öngörüleri'nde özel ölçümler oluşturmak için yöntemi kullanabilir. Bu yöntem şu anda sürüm 2.x ve sonraki sürümde desteklenmez. Örnek bir yöntem çağrısı aşağıda veda edebilirsiniz:

```javascript
context.log.metric("TestMetric", 1234);
```

Bu kod, Application `trackMetric` Insights için Düğüm.js SDK'yı kullanarak aramaya alternatiftir.

## <a name="log-custom-telemetry-in-c-functions"></a>C# işlevlerinde özel telemetriyi günlüğe kaydetme

Uygulama Öngörüleri SDK'nın işlevlerinizden Uygulama Öngörüleri'ne özel telemetri verileri göndermek için kullanabileceğiniz, İşlevlere özel bir sürümü vardır: [Microsoft.Azure.WebJobs.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Logging.ApplicationInsights). Bu paketi yüklemek için komut istemi aşağıdaki komutu kullanın:

# <a name="command"></a>[Komut](#tab/cmd)

```cmd
dotnet add package Microsoft.Azure.WebJobs.Logging.ApplicationInsights --version <VERSION>
```

# <a name="powershell"></a>[Powershell](#tab/powershell)

```powershell
Install-Package Microsoft.Azure.WebJobs.Logging.ApplicationInsights -Version <VERSION>
```

---

Bu komutta, `<VERSION>` [microsoft.azure.webjobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs/)yüklü sürümünü destekleyen bu paketin bir sürümü ile değiştirin. 

Aşağıdaki C# örnekleri [özel telemetri API'sini](../azure-monitor/app/api-custom-events-metrics.md)kullanır. Örnek bir .NET sınıf kitaplığı içindir, ancak Application Insights kodu C# komut dosyası için aynıdır.

### <a name="version-2x-and-later"></a>Sürüm 2.x ve sonrası

Sürüm 2.x ve çalışma zamanının sonraki sürümleri, telemetriyi geçerli işlemle otomatik olarak ilişkilendirmek için Application Insights'ta yeni özellikler kullanır. İşlemi `Id` `ParentId`veya `Name` alanları el ile ayarlamaya gerek yoktur.

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

[GetMetric,](../azure-monitor/app/api-custom-events-metrics.md#getmetric) bir metrik oluşturmak için şu anda önerilen API'dir.

### <a name="version-1x"></a>Sürüm 1.x

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

Aramayın `TrackRequest` veya `StartOperation<RequestTelemetry>` işlev çağırması için yinelenen istekleri göreceğiniz için.  İşlevler çalışma süresi istekleri otomatik olarak izler.

Ayarlama. `telemetryClient.Context.Operation.Id` Bu genel ayar, birçok işlev aynı anda çalışırken yanlış korelasyona neden olur. Bunun yerine, yeni bir telemetri örneği oluşturun (`DependencyTelemetry`, `EventTelemetry`) ve özelliğini değiştirin. `Context` Daha sonra telemetri örneğinde `Track` ilgili `TelemetryClient` yönteme geçmek (`TrackDependency()`, , `TrackEvent()`. `TrackMetric()` Bu yöntem, telemetrinin geçerli işlev çağırması için doğru korelasyon ayrıntılarına sahip olmasını sağlar.

## <a name="log-custom-telemetry-in-javascript-functions"></a>JavaScript işlevlerinde özel telemetriyi günlüğe kaydetme

Burada [Uygulama Öngörüleri Düğüm.js SDK](https://github.com/microsoft/applicationinsights-node.js)ile özel telemetri gönderir örnek bir kod snippet:

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

`tagOverrides` Parametre, işlevin `operation_Id` çağırma kimliğini ayarlar. Bu ayar, belirli bir işlev çağırması için otomatik olarak oluşturulan ve özel telemetrinin tümlerini ilişkilendirmenizi sağlar.

## <a name="dependencies"></a>Bağımlılıklar

V2 fonksiyonları HTTP istekleri, ServiceBus, EventHub ve SQL için bağımlılıkları otomatik olarak toplar.

Bağımlılıkları göstermek için özel kod yazabilirsiniz. Örneğin, [C# özel telemetri bölümündeki](#log-custom-telemetry-in-c-functions)örnek koduna bakın. Örnek kod, Application Insights'ta aşağıdaki resme benzeyen bir *uygulama haritasıyla* sonuçlanır:

![Uygulama eşlemesi](./media/functions-monitoring/app-map.png)

## <a name="enable-application-insights-integration"></a>Application Insights tümleştirmesini etkinleştirme

Bir işlev uygulamasının Application Insights'a veri göndermesi için, bir Application Insights kaynağının enstrümantasyon anahtarını bilmesi gerekir. Anahtar **APPINSIGHTS_INSTRUMENTATIONKEY**adlı bir uygulama ayarında olmalıdır.

İşlev uygulamanızı [Azure portalında](functions-create-first-azure-function.md), komut satırından [Azure İşleme Temel Araçları'nı](functions-create-first-azure-function-azure-cli.md)kullanarak veya [Visual Studio Code'u](functions-create-first-function-vs-code.md)kullanarak oluşturduğunuzda, Uygulama Öngörüleri entegrasyonu varsayılan olarak etkinleştirilir. Application Insights kaynağı işlev uygulamanızla aynı ada sahiptir ve aynı bölgede veya en yakın bölgede oluşturulur.

### <a name="new-function-app-in-the-portal"></a>Portalda yeni fonksiyon uygulaması

Oluşturulan Application Insights kaynağını gözden geçirmek için, **Uygulama Öngörüleri** penceresini genişletmek için bu kaynağı seçin. Yeni kaynak **adını** değiştirebilir veya verilerinizi depolamak istediğiniz [Bir Azure coğrafyasında](https://azure.microsoft.com/global-infrastructure/geographies/) farklı bir **Konum** seçebilirsiniz.

![İşlev uygulaması oluştururken Uygulama Öngörülerini etkinleştirme](media/functions-monitoring/enable-ai-new-function-app.png)

**Oluştur'u**seçtiğinizde, uygulama ayarlarında ayarlanan `APPINSIGHTS_INSTRUMENTATIONKEY` işlev uygulamanızla bir Uygulama Öngörüleri kaynağı oluşturulur. Her şey hazır.

<a id="manually-connect-an-app-insights-resource"></a>
### <a name="add-to-an-existing-function-app"></a>Varolan bir işlev uygulamasına ekleme 

[Visual Studio'yu](functions-create-your-first-function-visual-studio.md)kullanarak bir işlev uygulaması oluşturduğunuzda, Application Insights kaynağını oluşturmanız gerekir. Daha sonra işlev uygulamanızda uygulama ayarı olarak bu kaynaktan enstrümantasyon anahtarı ekleyebilirsiniz.

[!INCLUDE [functions-connect-new-app-insights.md](../../includes/functions-connect-new-app-insights.md)]

İşlevlerin ilk sürümleri, artık önerilmez dahili izleme kullanılır. Böyle bir işlev uygulaması için Application Insights tümleştirmesini etkinleştirirken, [yerleşik günlüğe kaydetmeyi](#disable-built-in-logging)de devre dışı bmelisiniz.  

## <a name="report-issues"></a>Sorun bildirme

İşlevlerde Application Insights tümleştirmesiyle ilgili bir sorunu bildirmek veya bir öneri veya istekte bulunmak için [GitHub'da bir sorun oluşturun.](https://github.com/Azure/Azure-Functions/issues/new)

## <a name="streaming-logs"></a>Akış Günlükleri

Bir uygulama geliştirirken, Azure'da çalışırken sık sık günlüklere gerçek zamanlı olarak neler yazıldığını görmek istersiniz.

İşlev yürütmeleriniz tarafından oluşturulan günlük dosyaları akışını görüntülemenin iki yolu vardır.

* **Dahili günlük akışı**: App Service platformu, uygulama günlüğü dosyalarınızın akışını görüntülemenizi sağlar. Bu, [yerel geliştirme](functions-develop-local.md) sırasında işlevlerinizi hata ayıklamave portaldaki **Test** sekmesini kullandığınızda görülen çıktıya eşdeğerdir. Tüm günlük tabanlı bilgiler görüntülenir. Daha fazla bilgi için [Akış günlüklerine](../app-service/troubleshoot-diagnostic-logs.md#stream-logs)bakın. Bu akış yöntemi yalnızca tek bir örneği destekler ve Bir Tüketim planında Linux üzerinde çalışan bir uygulamayla kullanılamaz.

* **Canlı Ölçümler Akışı**: işlev uygulamanız [Application Insights'a bağlandığında,](#enable-application-insights-integration)Günlük verilerini ve diğer ölçümleri Azure portalında Canlı Ölçümler [Akışı'nı](../azure-monitor/app/live-stream.md)kullanarak neredeyse gerçek zamanlı olarak görüntüleyebilirsiniz. Birden çok örnekte veya Bir Tüketim planında Linux üzerinde çalışan işlevleri izlerken bu yöntemi kullanın. Bu yöntem [örneklenmiş verileri](#configure-sampling)kullanır.

Günlük akışları hem portalda hem de çoğu yerel geliştirme ortamlarında görüntülenebilir. 

### <a name="portal"></a>Portal

Portalda her iki günlük akışı türünü de görüntüleyebilirsiniz.

#### <a name="built-in-log-streaming"></a>Yerleşik günlük akışı

Portaldaki akış günlüklerini görüntülemek için işlev uygulamanızdaki **Platform özellikleri** sekmesini seçin. Daha sonra, **İzleme**altında, **Günlük akışı**seçin.

![Portalda akış günlüklerini etkinleştirme](./media/functions-monitoring/enable-streaming-logs-portal.png)

Bu, uygulamanızı günlük akış hizmetine bağlar ve uygulama günlükleri pencerede görüntülenir. **Uygulama günlükleri** ve Web **sunucusu günlükleri**arasında geçiş yapabilirsiniz.  

![Portaldaki akış günlüklerini görüntüleme](./media/functions-monitoring/streaming-logs-window.png)

#### <a name="live-metrics-stream"></a>Canlı Ölçüm Akışı

Uygulamanız için Canlı Ölçümler Akışını görüntülemek için, işlev uygulamanızın **Genel Bakış** sekmesini seçin. Uygulama Öngörüleri etkinleştirildiğinde, **Yapılandırılmış özellikler**altında bir **Uygulama Öngörüleri** bağlantısı görürsünüz. Bu bağlantı sizi uygulamanız için Uygulama Öngörüleri sayfasına götürür.

Uygulama Öngörüleri'nde **Canlı Ölçümler Akışı'nı**seçin. [Örneklenmiş günlük girişleri](#configure-sampling) **Örnek Telemetri**altında görüntülenir.

![Portalda Canlı Ölçümler Akışını Görüntüle](./media/functions-monitoring/live-metrics-stream.png) 

### <a name="visual-studio-code"></a>Visual Studio Code

[!INCLUDE [functions-enable-log-stream-vs-code](../../includes/functions-enable-log-stream-vs-code.md)]

### <a name="core-tools"></a>Çekirdek Araçları

[!INCLUDE [functions-streaming-logs-core-tools](../../includes/functions-streaming-logs-core-tools.md)]

### <a name="azure-cli"></a>Azure CLI

[Azure CLI'yi](/cli/azure/install-azure-cli)kullanarak akış günlüklerini etkinleştirebilirsiniz. Oturum açmak, aboneliğinizi seçmek ve günlük dosyalarını aktarmak için aşağıdaki komutları kullanın:

```azurecli
az login
az account list
az account set --subscription <subscriptionNameOrId>
az webapp log tail --resource-group <RESOURCE_GROUP_NAME> --name <FUNCTION_APP_NAME>
```

### <a name="azure-powershell"></a>Azure PowerShell

[Azure PowerShell'i](/powershell/azure/overview)kullanarak akış günlüklerini etkinleştirebilirsiniz. PowerShell için Azure hesabınızı eklemek, aboneliğinizi seçmek ve günlük dosyalarını akışa aktarmak için aşağıdaki komutları kullanın:

```powershell
Add-AzAccount
Get-AzSubscription
Get-AzSubscription -SubscriptionName "<subscription name>" | Select-AzSubscription
Get-AzWebSiteLog -Name <FUNCTION_APP_NAME> -Tail
```

## <a name="disable-built-in-logging"></a>Yerleşik günlüğe kaydetmeyi devre dışı

Uygulama Öngörüleri'ni etkinleştirdiğinizde, Azure Depolama'yı kullanan yerleşik günlüğe kaydetmeyi devre dışı edin. Dahili günlüğe kaydetme, hafif iş yükleriyle test etmek için yararlıdır, ancak yüksek yük üretim kullanımı için tasarlanmamıştır. Üretim izleme için Application Insights'ı öneririz. Üretimde yerleşik günlüğe kaydetme kullanılıyorsa, Azure Depolama'da azaltma nedeniyle günlük kaydı tamamlanmamış olabilir.

Yerleşik günlüğe kaydetmeyi devre dışı `AzureWebJobsDashboard` kılabilir, uygulama ayarını silin. Azure portalındaki uygulama ayarlarının nasıl silinir hakkında bilgi için, [bir işlev uygulamasını yönetme](functions-how-to-use-azure-function-app-settings.md#settings)bölümüne bakın. **Application settings** Uygulama ayarını silmeden önce, aynı işlev uygulamasındaki varolan işlevlerin Azure Depolama tetikleyicileri veya bağlamaları ayarını kullanmadığından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Uygulama Bilgileri](/azure/application-insights/)
* [ASP.NET Çekirdek günlüğü](/aspnet/core/fundamentals/logging/)

[host.json]: functions-host-json.md
