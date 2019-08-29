---
title: Dayanıklı İşlevler tanılama-Azure
description: Azure Işlevleri için Dayanıklı İşlevler uzantısıyla ilgili sorunları tanılamayı öğrenin.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 2cc60ee2c73aa6858f68d6b13a895a0188bb5735
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70098129"
---
# <a name="diagnostics-in-durable-functions-in-azure"></a>Azure 'da Dayanıklı İşlevler tanılama

[Dayanıklı işlevler](durable-functions-overview.md)sorunları tanılamaya yönelik çeşitli seçenekler vardır. Bu seçeneklerden bazıları normal işlevlerle aynıdır ve bazıları da Dayanıklı İşlevler için benzersizdir.

## <a name="application-insights"></a>Application Insights

[Application Insights](../../azure-monitor/app/app-insights-overview.md) , Azure işlevlerinde tanılama ve izleme yapmak için önerilen yoldur. Aynı Dayanıklı İşlevler için de geçerlidir. İşlev uygulamanızda Application Insights nasıl yararlanacağınızı gösteren bir genel bakış için bkz. [Azure Işlevlerini izleme](../functions-monitoring.md).

Azure Işlevleri dayanıklı uzantısı, bir Orchestration 'un uçtan uca yürütülmesini izlemenize imkan tanıyan *olayları izleme* özelliği de yayar. Bunlar, Azure portal [Application Insights Analytics](../../azure-monitor/app/analytics.md) Aracı kullanılarak bulunabilir ve sorgulanır.

### <a name="tracking-data"></a>İzleme verileri

Bir Orchestration örneğinin her yaşam döngüsü olayı, bir izleme olayının Application Insights İziz koleksiyonuna yazılmasına neden olur. Bu olay, birkaç alan içeren bir **Customdimensions** yükü içerir.  Alan adlarının hepsi ile `prop__`sona erer.

* **Hubname**: Düzenleyiclerinizin çalıştığı görev merkezinin adı.
* **appname**: İşlev uygulamasının adı. Aynı Application Insights örneğini paylaşan birden çok işlevli uygulamanız olduğunda bu faydalıdır.
* **slotname**: Geçerli işlev uygulamasının çalıştığı [dağıtım yuvası](https://blogs.msdn.microsoft.com/appserviceteam/2017/06/13/deployment-slots-preview-for-azure-functions/) . Bu, ayarlarınızı sürümüne dağıtmak için dağıtım yuvalarından yararlandığınızda yararlı olur.
* **fonksiyonadı**: Orchestrator veya Activity işlevinin adı.
* **FunctionType**: **Orchestrator** veya **Activity**gibi işlevin türü.
* **InstanceId**: Orchestration örneğinin benzersiz KIMLIĞI.
* **durum**: Örneğin yaşam döngüsü yürütme durumu. Geçerli değerler şunlardır:
  * **Zamanlanan**: İşlev yürütme için zamanlandı ancak henüz çalışmaya başlamadı.
  * **Başlangıç**: İşlev çalışmaya başladı ancak henüz beklememiş veya tamamlanmamış.
  * **Beklenen**: Orchestrator bazı çalışmaları zamanladı ve tamamlanmasını bekliyor.
  * **Dinleme**: Orchestrator bir dış olay bildirimini dinliyor.
  * **Tamamlandı**: İşlev başarıyla tamamlandı.
  * **Başarısız oldu**: İşlev bir hata vererek başarısız oldu.
* **neden**: İzleme olayı ile ilişkili ek veriler. Örneğin, bir örnek bir dış olay bildirimi bekliyorsa Bu alan, beklediği olayın adını gösterir. Bir işlev başarısız olduysa, bu hata ayrıntılarını içerir.
* **ısreplay**: İzleme olayının yeniden yürütülmüş yürütme için olup olmadığını gösteren Boole değeri.
* **Extensionversion**: Dayanıklı görev uzantısının sürümü. Bu, uzantıdaki olası hataları bildirirken özellikle önemli bir veri olabilir. Uzun süre çalışan örnekler, çalışırken bir güncelleştirme gerçekleşirse birden çok sürümü bildirebilir.
* **sequenceNumber**: Bir olay için yürütme sıra numarası. Zaman damgasıyla birlikte, olayları yürütme zamanına göre sipariş etmeye yardımcı olur. *Örnek çalışırken konağın yeniden başlatılması durumunda bu sayının sıfıra sıfırlandığını unutmayın, bu nedenle her zaman zaman damgasına göre sıralamak önemlidir, sonra sequenceNumber.*

Application Insights yayılan izleme verilerinin ayrıntı düzeyi, `logger` `host.json` dosyanın (işlevler 1. x) veya `logging` (işlevler 2. x) bölümünde yapılandırılabilir.

#### <a name="functions-1x"></a>İşlevler 1.x

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

#### <a name="functions-2x"></a>İşlevler 2.x

```json
{
    "logging": {
        "logLevel": {
          "Host.Triggers.DurableTask": "Information",
        },
    }
}
```

Varsayılan olarak, tüm yeniden yürütme olmayan izleme olayları yayınlanır. Verilerin hacmi, bu durum izleme olaylarının yalnızca olağanüstü `Host.Triggers.DurableTask` durumlar `"Warning"` için `"Error"` yayınlandığı, veya olarak ayarlanarak azaltılabilir.

Ayrıntılı düzenleme yeniden yürütme olaylarını yaymayı etkinleştirmek için, aşağıda `LogReplayEvents` gösterildiği `durableTask` gibi, `host.json` dosyasında `true` öğesine ayarlanabilir:

#### <a name="functions-1x"></a>İşlevler 1.x

```json
{
    "durableTask": {
        "logReplayEvents": true
    }
}
```

#### <a name="functions-2x"></a>İşlevler 2.x

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
> Application Insights telemetri, verileri çok sık yaymamak için Azure Işlevleri çalışma zamanı tarafından örneklenir. Bu, kısa bir süre içinde birçok yaşam döngüsü olayı gerçekleştiğinde izleme bilgilerinin kaybolmasına neden olabilir. [Azure Işlevleri izleme makalesinde](../functions-monitoring.md#configure-sampling) , bu davranışın nasıl yapılandırılacağı açıklanmaktadır.

### <a name="single-instance-query"></a>Tek örnekli sorgu

Aşağıdaki sorgu, bir [Merhaba sıra](durable-functions-sequence.md) işlevi düzenleme işlevinin tek bir örneği için geçmiş izleme verilerini gösterir. [Application Insights sorgu dili (AIQL)](https://aka.ms/LogAnalyticsLanguageReference)kullanılarak yazılmıştır. Yalnızca *mantıksal* yürütme yolunun gösterilmesi için yeniden yürütme yürütmesini filtreler. Olaylar, aşağıdaki sorguda gösterildiği gibi `timestamp` ve `sequenceNumber` arasında sıralama yaparak sıralanabilir:

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

Sonuç, yürütme süresi tarafından artan düzende sıralanan etkinlik işlevleri dahil olmak üzere Orchestration yürütme yolunu gösteren izleme olaylarının bir listesidir.

![Application Insights sorgu](./media/durable-functions-diagnostics/app-insights-single-instance-ordered-query.png)

### <a name="instance-summary-query"></a>Örnek Özet sorgusu

Aşağıdaki sorgu, belirli bir zaman aralığında çalıştırılan tüm düzenleme örneklerinin durumunu görüntüler.

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

Sonuç, örnek kimliklerinin ve bunların geçerli çalışma zamanı durumlarının bir listesidir.

![Application Insights sorgu](./media/durable-functions-diagnostics/app-insights-single-summary-query.png)

## <a name="logging"></a>Günlüğe Kaydetme

Doğrudan bir Orchestrator işlevinden Günlükler yazarken Orchestrator yeniden yürütme davranışının aklınızda tutulması önemlidir. Örneğin, aşağıdaki Orchestrator işlevini göz önünde bulundurun:

### <a name="c"></a>C#

```cs
public static async Task Run(
    DurableOrchestrationContext context,
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

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

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

Elde edilen günlük verileri aşağıdakine benzer şekilde görünür:

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
> Günlük, F1, F2 ve F3 çağrısı yaparken, bu işlevlerin yalnızca ilk kez karşılaştığı zaman adlandırıldığını unutmayın . Yeniden yürütme sırasında gerçekleşen sonraki çağrılar atlanır ve çıktılar Orchestrator mantığına yeniden yürütülür.

Yalnızca `IsReplaying` yeniden denenmesiz yürütme oturumu açmak istiyorsanız, yalnızca ise, `false`günlüğe kaydedilecek koşullu bir ifade yazabilirsiniz. Yukarıdaki örneği, ancak bu kez yeniden yürütme denetimlerini göz önünde bulundurun.

#### <a name="c"></a>C#

```cs
public static async Task Run(
    DurableOrchestrationContext context,
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

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

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

Bu değişiklik ile günlük çıktısı aşağıdaki gibidir:

```txt
Calling F1.
Calling F2.
Calling F3.
Done!
```

## <a name="custom-status"></a>Özel durum

Özel düzenleme durumu, Orchestrator işleviniz için özel bir durum değeri ayarlamanıza olanak sağlar. Bu durum http durumu sorgu API 'si veya `DurableOrchestrationClient.GetStatusAsync` API 'si ile sağlanır. Özel düzenleme durumu Orchestrator işlevleri için daha zengin izleme imkanı sunar. Örneğin, Orchestrator işlev kodu uzun süre çalışan bir `DurableOrchestrationContext.SetCustomStatus` işlemin ilerlemesini güncelleştirme çağrılarını içerebilir. Web sayfası veya diğer dış sistem gibi bir istemci, daha zengin ilerleme bilgileri için HTTP durum sorgusu API 'Lerini düzenli aralıklarla sorgulayabilir. Aşağıdaki bir örnek `DurableOrchestrationContext.SetCustomStatus` aşağıda verilmiştir:

### <a name="c"></a>C#

```csharp
public static async Task SetStatusTest([OrchestrationTrigger] DurableOrchestrationContext context)
{
    // ...do work...

    // update the status of the orchestration with some arbitrary data
    var customStatus = new { completionPercentage = 90.0, status = "Updating database records" };
    context.SetCustomStatus(customStatus);

    // ...do more work...
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

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

Düzenleme çalışırken, dış istemciler şu özel durumu getirebilir:

```http
GET /admin/extensions/DurableTaskExtension/instances/instance123

```

İstemciler aşağıdaki yanıtı alır:

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
> Özel durum yükü, bir Azure Tablo depolama sütununa sığamayacak olması gerektiğinden, 16 KB 'lık UTF-16 JSON metniyle sınırlandırılmıştır. Daha büyük yük gerekiyorsa dış depolama alanını kullanabilirsiniz.

## <a name="debugging"></a>Hata Ayıklama

Azure Işlevleri, işlev kodu doğrudan hata ayıklamayı destekler ve aynı destek, Azure 'da veya yerel olarak çalıştırılmasının yanı sıra Dayanıklı İşlevler ileri taşır. Ancak, hata ayıklarken farkında olacak birkaç davranış vardır:

* Yeniden **Yürüt**: Yeni girişler alındığında Orchestrator işlevleri düzenli olarak yeniden tekrarlanır. Bu, bir Orchestrator işlevinin tek bir *mantıksal* yürütmesi, özellikle işlev kodunda daha önce ayarlandıysa, aynı kesme noktasına birden çok kez ulaşabilme anlamına gelir.
* **Await**: `await` Her karşılaşıldığında, kalıcı görev çerçevesi dağıtıcısına denetim verir. Bu belirli `await` bir kez karşılaşılırsa, ilişkili görev *hiçbir zaman sürdürülmez* . Görev hiçbir şekilde devam ettirilmediği için, await (Visual Studio 'da F10) *üzerinde* Adımlama gerçekten mümkün değildir. Yalnızca bir görev yeniden yürütüldüğünde, üzerinde adımlamayı yapın.
* **Mesajlaşma zaman aşımları**: Dayanıklı İşlevler, hem Orchestrator işlevlerinin hem de etkinlik işlevlerinin yürütülmesini sağlamak için sıra iletilerini kullanır. Çoklu VM ortamında, uzun bir süre için hata ayıklamanın kesilmesi, başka bir VM 'nin iletiyi seçmesini ve yinelenen yürütmeye neden olabilir. Bu davranış düzenli sıra tetikleyicisi işlevleri için de bulunur, ancak kuyruklar bir uygulama ayrıntısı olduğundan bu bağlamda işaret etmek önemlidir.

> [!TIP]
> Kesme noktaları ayarlarken, yalnızca yeniden yürütme dışı yürütmeyi bölmek istiyorsanız, yalnızca, `IsReplaying` `false`varsa kesen bir koşullu kesme noktası ayarlayabilirsiniz.

## <a name="storage"></a>Depolama

Varsayılan olarak, Dayanıklı İşlevler durumu Azure Storage 'da depolar. Bu, [Microsoft Azure Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)gibi araçları kullanarak, düzenleyiclarınızın durumunu inceleyebilmeniz anlamına gelir.

![Azure Depolama Gezgini ekran görüntüsü](./media/durable-functions-diagnostics/storage-explorer.png)

Bu, bir düzenleme için tam olarak hangi durum olduğunu görtiğinden hata ayıklama için yararlıdır. Kuyruklarda bulunan iletiler, hangi çalışmanın beklendiğini (veya bazı durumlarda takılı olduğunu) öğrenmek için de incelenebilir.

> [!WARNING]
> Tablo depolamada yürütme geçmişini görmek uygun olsa da, bu tablo üzerinde herhangi bir bağımlılık yapmaktan kaçının. Dayanıklı İşlevler uzantısı geliştikçe değişiklik gösterebilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dayanıklı zamanlayıcıları nasıl kullanacağınızı öğrenin](durable-functions-timers.md)
