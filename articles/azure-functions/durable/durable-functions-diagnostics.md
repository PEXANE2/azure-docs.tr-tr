---
title: Dayanıklı İşlevlerde Tanılama - Azure
description: Azure İşlevler için Dayanıklı İşlevler uzantısı ile ilgili sorunları nasıl tanılayarak tanılamayı öğrenin.
author: cgillum
ms.topic: conceptual
ms.date: 11/02/2019
ms.author: azfuncdf
ms.openlocfilehash: 4cb832f8fe11ac2581e97d9cdcc777eaff702ee9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79278200"
---
# <a name="diagnostics-in-durable-functions-in-azure"></a>Azure'da Dayanıklı İşlevler'de Tanılama

[Dayanıklı İşlevler](durable-functions-overview.md)ile sorunları tanılama için çeşitli seçenekler vardır. Bu seçeneklerden bazıları normal işlevlerle aynıdır ve bazıları da Dayanıklı İşlevler için benzersizdir.

## <a name="application-insights"></a>Application Insights

[Uygulama Öngörüleri,](../../azure-monitor/app/app-insights-overview.md) Azure İşlevleri'nde tanılama ve izleme yapmanın önerilen yoludur. Aynı kalıcı fonksiyonlar için de geçerlidir. İşlev uygulamanızda Uygulama Öngörüleri'nden nasıl yararlanılanıza ilişkin genel bir bakış [için](../functions-monitoring.md)bkz.

Azure İşlerinden Dayanıklı Uzatma, bir orkestrasyonun uçtan uca yürütülmesini izlemenize olanak tanıyan *izleme olayları* da yayır. Bu izleme olayları, Azure portalındaki [Application Insights Analytics](../../azure-monitor/app/analytics.md) aracı kullanılarak bulunabilir ve sorgulanabilir.

### <a name="tracking-data"></a>Verileri izleme

Bir orkestrasyon örneğinin her yaşam döngüsü olayı, Application Insights'taki **izleme** koleksiyonuna bir izleme olayının yazılmasına neden olur. Bu olay, çeşitli alanları içeren bir **özelBoyut** yükü içerir.  Alan adlarının tümü `prop__`.

* **hubName**: Orkestrasyonlarınızın çalıştırıldığı görev merkezinin adı.
* **appName**: Fonksiyon uygulamasının adı. Bu alan, aynı Application Insights örneğini paylaşan birden çok işlevli uygulamanız olduğunda yararlıdır.
* **slotName**: Geçerli işlev uygulamasının çalıştığı [dağıtım yuvası.](../functions-deployment-slots.md) Bu alan, orkestrasyonlarınızı sürümlemek için dağıtım yuvalarından yararlanırken kullanışlıdır.
* **functionName**: Orkestratörün veya etkinlik fonksiyonunun adıdır.
* **functionType**: **Orchestrator** veya **Activity**gibi işlevin türü.
* **instanceId**: Orkestrasyon örneğinin benzersiz kimliği.
* **durum**: Örneğin yaşam döngüsü yürütme durumu. Geçerli değerler şunlardır:
  * **Zamanlanan**: İşlev yürütme için zamanlandı, ancak henüz çalışmaya başlamadı.
  * **Başlangıç**: İşlev çalışmaya başladı, ancak henüz beklemedi veya tamamlanmadı.
  * **Beklenen**: Orkestratör bazı çalışmalar planladı ve tamamlanmasını bekliyor.
  * **Dinleme**: Orkestratör harici bir olay bildirimi dinliyor.
  * **Tamamlandı**: İşlev başarıyla tamamlandı.
  * **Failed**: İşlev bir hata ile başarısız oldu.
* **neden**: İzleme olayıyla ilişkili ek veriler. Örneğin, bir örnek harici olay bildirimi bekliyorsa, bu alan beklediği olayın adını gösterir. Bir işlev başarısız olduysa, bu alan hata ayrıntılarını içerir.
* **isReplay**: İzleme olayının yeniden yürütülüyor olup olmadığını gösteren boolean değeri.
* **extensionVersion**: Dayanıklı Görev uzantısı sürümü. Sürüm bilgileri, uzantıdaki olası hataları rapor ederken özellikle önemli verilerdir. Uzun süren örnekler, çalışırken bir güncelleştirme oluşursa birden çok sürümü bildirebilir.
* **sequenceNumber**: Bir olayın yürütme sıra numarası. Zaman damgası ile birlikte yürütme süresine göre olayların sipariş yardımcı olur. *Örnek çalışırken ana bilgisayar yeniden başlatılırsa bu sayının sıfırlanacağını unutmayın, bu nedenle önce önce zaman damgasına, sonra sequenceNumber'a göre sıralamak önemlidir.*

Uygulama Öngörüleri'ne yayılan verileri izlemenin ayrıntılılığı `logger` `logging` `host.json` dosyanın (Fonksiyonlar 1.x) veya (Fonksiyonlar 2.0) bölümünde yapılandırılabilir.

#### <a name="functions-10"></a>Fonksiyonlar 1.0

```json
{
    "logger": {
        "categoryFilter": {
            "categoryLevels": {
                "Host.Triggers.DurableTask": "Information"
            }
        }
    }
}
```

#### <a name="functions-20"></a>Fonksiyonlar 2.0

```json
{
    "logging": {
        "logLevel": {
          "Host.Triggers.DurableTask": "Information",
        },
    }
}
```

Varsayılan olarak, tüm yeniden oynatmayan izleme olayları yayılır. Veri hacmi ayarlayarak `Host.Triggers.DurableTask` `"Warning"` azaltılabilir veya `"Error"` bu durumda izleme olayları yalnızca istisnai durumlar için yayılan olacaktır.

Ayrıntılı orkestrasyon yeniden oynatma olaylarını yayan `LogReplayEvents` etkinleştirmek `true` için, `host.json` aşağıdaki `durableTask` gibi dosyada ayarlanabilir:

#### <a name="functions-10"></a>Fonksiyonlar 1.0

```json
{
    "durableTask": {
        "logReplayEvents": true
    }
}
```

#### <a name="functions-20"></a>Fonksiyonlar 2.0

```javascript
{
    "extensions": {
        "durableTask": {
            "logReplayEvents": true
        }
    }
}
```

> [!NOTE]
> Varsayılan olarak, Application Insights telemetrisi, çok sık veri yayan verileri önlemek için Azure İşlevleri çalışma zamanı tarafından örneklenir. Bu, kısa bir süre içinde birçok yaşam döngüsü olayı meydana geldiğinde izleme bilgilerinin kaybolmasına neden olabilir. [Azure İşlevlerini İzleme makalesi,](../functions-monitoring.md#configure-sampling) bu davranışın nasıl yapılandırılabildiğini açıklar.

### <a name="single-instance-query"></a>Tek örnek sorgusu

Aşağıdaki sorgu, [Hello Sequence](durable-functions-sequence.md) işlev in tek bir örneği için geçmiş izleme verilerini gösterir. [Bu Uygulama Insights Sorgu Dili (AIQL)](https://aka.ms/LogAnalyticsLanguageReference)kullanılarak yazılmıştır. Yalnızca *mantıksal* yürütme yolunun gösterilmesi için yeniden yürütmeyi filtreler. Olaylar, aşağıdaki sorguda gösterildiği `timestamp` `sequenceNumber` gibi sıralayarak sıralanabilir:

```AIQL
let targetInstanceId = "ddd1aaa685034059b545eb004b15d4eb";
let start = datetime(2018-03-25T09:20:00);
traces
| where timestamp > start and timestamp < start + 30m
| where customDimensions.Category == "Host.Triggers.DurableTask"
| extend functionName = customDimensions["prop__functionName"]
| extend instanceId = customDimensions["prop__instanceId"]
| extend state = customDimensions["prop__state"]
| extend isReplay = tobool(tolower(customDimensions["prop__isReplay"]))
| extend sequenceNumber = tolong(customDimensions["prop__sequenceNumber"])
| where isReplay != true
| where instanceId == targetInstanceId
| sort by timestamp asc, sequenceNumber asc
| project timestamp, functionName, state, instanceId, sequenceNumber, appName = cloud_RoleName
```

Sonuç, yürütme süresinin artan sırada emrettiği etkinlik işlevleri de dahil olmak üzere, yürütmenin yürütme yolunu gösteren izleme olaylarının listesidir.

![Uygulama Öngörüleri sorgusu](./media/durable-functions-diagnostics/app-insights-single-instance-ordered-query.png)

### <a name="instance-summary-query"></a>Örnek özet sorgusu

Aşağıdaki sorgu, belirli bir zaman aralığında çalıştırılabilen tüm düzenleme örneklerinin durumunu görüntüler.

```AIQL
let start = datetime(2017-09-30T04:30:00);
traces
| where timestamp > start and timestamp < start + 1h
| where customDimensions.Category == "Host.Triggers.DurableTask"
| extend functionName = tostring(customDimensions["prop__functionName"])
| extend instanceId = tostring(customDimensions["prop__instanceId"])
| extend state = tostring(customDimensions["prop__state"])
| extend isReplay = tobool(tolower(customDimensions["prop__isReplay"]))
| extend output = tostring(customDimensions["prop__output"])
| where isReplay != true
| summarize arg_max(timestamp, *) by instanceId
| project timestamp, instanceId, functionName, state, output, appName = cloud_RoleName
| order by timestamp asc
```

Sonuç, örnek kimliklerinin ve bunların geçerli çalışma zamanı durumunun bir listesidir.

![Uygulama Öngörüleri sorgusu](./media/durable-functions-diagnostics/app-insights-single-summary-query.png)

## <a name="logging"></a>Günlüğe Kaydetme

Doğrudan bir orkestratör işlevinden günlükler yazarken orkestratör tekrar davranışını göz önünde bulundurmak önemlidir. Örneğin, aşağıdaki orkestratör işlevini göz önünde bulundurun:

### <a name="precompiled-c"></a>Önceden derlenmiş C #

```csharp
[FunctionName("FunctionChain")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context,
    ILogger log)
{
    log.LogInformation("Calling F1.");
    await context.CallActivityAsync("F1");
    log.LogInformation("Calling F2.");
    await context.CallActivityAsync("F2");
    log.LogInformation("Calling F3");
    await context.CallActivityAsync("F3");
    log.LogInformation("Done!");
}
```

### <a name="c-script"></a>C# Komut Dosyası

```csharp
public static async Task Run(
    IDurableOrchestrationContext context,
    ILogger log)
{
    log.LogInformation("Calling F1.");
    await context.CallActivityAsync("F1");
    log.LogInformation("Calling F2.");
    await context.CallActivityAsync("F2");
    log.LogInformation("Calling F3");
    await context.CallActivityAsync("F3");
    log.LogInformation("Done!");
}
```

### <a name="javascript-functions-20-only"></a>JavaScript (Yalnızca 2.0 Fonksiyonları)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context){
    context.log("Calling F1.");
    yield context.df.callActivity("F1");
    context.log("Calling F2.");
    yield context.df.callActivity("F2");
    context.log("Calling F3.");
    yield context.df.callActivity("F3");
    context.log("Done!");
});
```

Elde edilen günlük verileri aşağıdaki örnek çıktıgibi bir şey görünecektir:

```txt
Calling F1.
Calling F1.
Calling F2.
Calling F1.
Calling F2.
Calling F3.
Calling F1.
Calling F2.
Calling F3.
Done!
```

> [!NOTE]
> Günlükler F1, F2 ve F3'ün çağrıldığını iddia ederken, bu işlevlerin *yalnızca* ilk kez karşılaştıklarında çağrıldığını unutmayın. Yeniden oynatma sırasında meydana gelen sonraki çağrılar atlanır ve çıktılar orkestratör mantığına yeniden çalınır.

Yalnızca yeniden yürütülmeyen yürütmede oturum açmak istiyorsanız, yalnızca `IsReplaying` . `false` Yukarıdaki örneği göz önünde bulundurun, ancak bu sefer tekrar denetimleri ile.

#### <a name="precompiled-c"></a>Önceden derlenmiş C #

```csharp
[FunctionName("FunctionChain")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context,
    ILogger log)
{
    if (!context.IsReplaying) log.LogInformation("Calling F1.");
    await context.CallActivityAsync("F1");
    if (!context.IsReplaying) log.LogInformation("Calling F2.");
    await context.CallActivityAsync("F2");
    if (!context.IsReplaying) log.LogInformation("Calling F3");
    await context.CallActivityAsync("F3");
    log.LogInformation("Done!");
}
```

#### <a name="c"></a>C#

```cs
public static async Task Run(
    IDurableOrchestrationContext context,
    ILogger log)
{
    if (!context.IsReplaying) log.LogInformation("Calling F1.");
    await context.CallActivityAsync("F1");
    if (!context.IsReplaying) log.LogInformation("Calling F2.");
    await context.CallActivityAsync("F2");
    if (!context.IsReplaying) log.LogInformation("Calling F3");
    await context.CallActivityAsync("F3");
    log.LogInformation("Done!");
}
```

#### <a name="javascript-functions-20-only"></a>JavaScript (Yalnızca 2.0 Fonksiyonları)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context){
    if (!context.df.isReplaying) context.log("Calling F1.");
    yield context.df.callActivity("F1");
    if (!context.df.isReplaying) context.log("Calling F2.");
    yield context.df.callActivity("F2");
    if (!context.df.isReplaying) context.log("Calling F3.");
    yield context.df.callActivity("F3");
    context.log("Done!");
});
```

Dayanıklı Fonksiyonlar 2.0'dan başlayarak,.NET orkestratör işlevleri, yeniden oynatma sırasında günlük deyimlerini otomatik olarak filtreleyen bir `ILogger` işlev de oluşturma seçeneğine sahiptir. Bu otomatik filtreleme `IDurableOrchestrationContext.CreateReplaySafeLogger(ILogger)` API kullanılarak yapılır.

```csharp
[FunctionName("FunctionChain")]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context,
    ILogger log)
{
    log = context.CreateReplaySafeLogger(log);
    log.LogInformation("Calling F1.");
    await context.CallActivityAsync("F1");
    log.LogInformation("Calling F2.");
    await context.CallActivityAsync("F2");
    log.LogInformation("Calling F3");
    await context.CallActivityAsync("F3");
    log.LogInformation("Done!");
}
```

Daha önce belirtilen değişikliklerle, günlük çıktısı aşağıdaki gibidir:

```txt
Calling F1.
Calling F2.
Calling F3.
Done!
```

> [!NOTE]
> Önceki C# örnekleri Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x `DurableOrchestrationContext` `IDurableOrchestrationContext`için, 'yi yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

## <a name="custom-status"></a>Özel Durum

Özel orkestrasyon durumu, orkestratör işleviniz için özel bir durum değeri ayarlamanızı sağlar. Bu durum HTTP durum sorgusu API `IDurableOrchestrationClient.GetStatusAsync` veya API üzerinden sağlanır. Özel orkestrasyon durumu, orkestratör işlevleri için daha zengin izleme sağlar. Örneğin, orkestratör işlev kodu, `IDurableOrchestrationContext.SetCustomStatus` uzun süren bir işlem için ilerlemeyi güncelleştirmek için çağrılar içerebilir. Web sayfası veya başka bir dış sistem gibi bir istemci, daha zengin ilerleme bilgileri için HTTP durum sorgusu API'lerini düzenli olarak sorgulayabilir. Bir örnek `IDurableOrchestrationContext.SetCustomStatus` kullanılarak aşağıda verilmiştir:

### <a name="precompiled-c"></a>Önceden derlenmiş C #

```csharp
[FunctionName("SetStatusTest")]
public static async Task SetStatusTest([OrchestrationTrigger] IDurableOrchestrationContext context)
{
    // ...do work...

    // update the status of the orchestration with some arbitrary data
    var customStatus = new { completionPercentage = 90.0, status = "Updating database records" };
    context.SetCustomStatus(customStatus);

    // ...do more work...
}
```

> [!NOTE]
> Önceki C# örneği Dayanıklı Fonksiyonlar 2.x içindir. Dayanıklı Fonksiyonlar 1.x `DurableOrchestrationContext` `IDurableOrchestrationContext`için, 'yi yerine kullanmalısınız. Sürümler arasındaki farklar hakkında daha fazla bilgi için [Dayanıklı Işlevler sürümleri](durable-functions-versions.md) makalesine bakın.

### <a name="javascript-functions-20-only"></a>JavaScript (Yalnızca 2.0 Fonksiyonları)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    // ...do work...

    // update the status of the orchestration with some arbitrary data
    const customStatus = { completionPercentage: 90.0, status: "Updating database records", };
    context.df.setCustomStatus(customStatus);

    // ...do more work...
});
```

Orkestrasyon çalışırken, dış istemciler bu özel durumu getirebilir:

```http
GET /admin/extensions/DurableTaskExtension/instances/instance123

```

İstemciler aşağıdaki yanıtı alırsınız:

```http
{
  "runtimeStatus": "Running",
  "input": null,
  "customStatus": { "completionPercentage": 90.0, "status": "Updating database records" },
  "output": null,
  "createdTime": "2017-10-06T18:30:24Z",
  "lastUpdatedTime": "2017-10-06T19:40:30Z"
}
```

> [!WARNING]
> Özel durum yükü, Bir Azure Tablo Depolama sütununa sığabilmesi gerektiğinden UTF-16 JSON metninin 16 KB'si ile sınırlıdır. Daha büyük taşıma kapasitesine ihtiyacınız varsa harici depolama alanı kullanabilirsiniz.

## <a name="debugging"></a>Hata Ayıklama

Azure İşlevler hata ayıklama işlev kodunu doğrudan destekler ve aynı destek, ister Azure'da ister yerel olarak çalışır, Dayanıklı Işlevler'e taşınır. Ancak, hata ayıklama yaparken dikkat edilmesi gereken birkaç davranış vardır:

* **Replay**: Orchestrator fonksiyonları yeni girişler geldiğinde düzenli olarak [yeniden oynatılır.](durable-functions-orchestrations.md#reliability) Bu davranış, bir orchestrator işlevinin tek bir *mantıksal* yürütme, özellikle işlev kodunda erken ayarlanmışsa, aynı kesme noktasına birden çok kez çarpmayla sonuçlanabilir anlamına gelir.
* **Bekle**: Bir `await` orkestratör işlevinde bir şeyle karşılaşıldığında, denetimi Dayanıklı Görev Çerçevesi göndericisine geri verir. Belirli `await` bir sorunla ilk kez karşılaşıldıysa, ilişkili görev *hiçbir zaman* sürdürülmez. Görev hiçbir zaman devam etmediğinden, bekleme (Visual Studio'da F10) *üzerine* adım atmak mümkün değildir. Üzerine basma yalnızca bir görev yeniden yürütülüyorsa çalışır.
* **İleti zaman zaman ları**: Dayanıklı Işlevler, düzenleyicinin, etkinliğin ve varlık işlevlerinin yürütülmesini sağlamak için sıra iletilerini dahili olarak kullanır. Çok VM ortamında, uzun süre hata ayıklama içine kırma başka bir VM iletialmak için neden olabilir, yinelenen yürütme ile sonuçlanan. Bu davranış, düzenli sıra tetikleyici işlevleri için de vardır, ancak kuyruklar bir uygulama ayrıntısı olduğundan bu bağlamda işaret etmek önemlidir.
* **Durdurma ve başlatma**: Kalıcı işlevlerde iletiler hata ayıklama oturumları arasında devam eder. Hata ayıklamayı durdurur ve dayanıklı bir işlev yürütülürken yerel ana bilgisayar işlemini sonlandırırsanız, bu işlev gelecekteki bir hata ayıklama oturumunda otomatik olarak yeniden çalıştırılabilir. Bu davranış, beklenmiyorsa kafa karıştırıcı olabilir. Hata ayıklama oturumları arasındaki [iç depolama kuyruklarından](durable-functions-perf-and-scale.md#internal-queue-triggers) tüm iletileri temizlemek, bu davranışı önlemek için bir tekniktir.

> [!TIP]
> Orchestrator işlevlerinde kesme noktaları ayarlarken, yalnızca yeniden yürütülmeyemede kesmeyapmak istiyorsanız, yalnızca `IsReplaying` . `false`

## <a name="storage"></a>Depolama

Varsayılan olarak, Dayanıklı Işlevler depoları Azure Depolama'da durumu belirtir. Bu davranış, [Microsoft Azure Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)gibi araçları kullanarak orkestrasyonlarınızın durumunu inceleyebileceğiniz anlamına gelir.

![Azure Depolama Gezgini ekran görüntüsü](./media/durable-functions-diagnostics/storage-explorer.png)

Bu hata ayıklama için yararlıdır, çünkü bir orkestrasyonun tam olarak hangi durumda olabileceğini görürsünüz. Kuyruktaki iletiler, hangi işin beklemede olduğunu (veya bazı durumlarda takılıp kaldığını) öğrenmek için de incelenebilir.

> [!WARNING]
> Yürütme geçmişini tablo depolamada görmek uygun olsa da, bu tabloya herhangi bir bağımlılık uygulamaktan kaçının. Dayanıklı Fonksiyonlar uzantısı geliştikçe değişebilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure İşlevleri'nde izleme hakkında daha fazla bilgi edinin](../functions-monitoring.md)
