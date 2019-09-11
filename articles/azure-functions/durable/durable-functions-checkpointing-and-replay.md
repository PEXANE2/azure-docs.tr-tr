---
title: Dayanıklı İşlevler kontrol noktaları ve yeniden yürütme-Azure
description: Azure Işlevleri için Dayanıklı İşlevler uzantısında checkişaret ve yanıtın nasıl çalıştığını öğrenin.
services: functions
author: ggailey777
manager: gwallace
ms.service: azure-functions
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 5d0527de556c25a1d369d7b22c3f62579bc508f0
ms.sourcegitcommit: 97605f3e7ff9b6f74e81f327edd19aefe79135d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70735256"
---
# <a name="checkpoints-and-replay-in-durable-functions-azure-functions"></a>Dayanıklı İşlevler denetim noktaları ve yeniden yürütme (Azure Işlevleri)

Dayanıklı İşlevler anahtar özniteliklerinden biri **güvenilir bir yürütme**. Orchestrator işlevleri ve etkinlik işlevleri bir veri merkezi içindeki farklı VM 'lerde çalışıyor olabilir ve bu VM 'Ler veya temeldeki ağ altyapısı% 100 güvenilir değildir.

Bunun artma Dayanıklı İşlevler, düzenlemeler için güvenilir yürütme sağlar. Bu işlemi, işlev çağrılmasını ve düzenli aralıklarla denetim yürütme geçmişini depolama tablolarına ( [olay](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing)kaynağını belirleme olarak bilinen bir bulut tasarım modelini kullanarak) kullanarak bir depolama kuyruklarını kullanarak gerçekleştirir. Bu geçmiş daha sonra, bir Orchestrator işlevinin bellek içi durumunu otomatik olarak yeniden oluşturmak için yeniden çalınabilir.

## <a name="orchestration-history"></a>Düzenleme geçmişi

Aşağıdaki Orchestrator işlevinizin olduğunu varsayalım:

### <a name="c"></a>C#

```csharp
[FunctionName("E1_HelloSequence")]
public static async Task<List<string>> Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    var outputs = new List<string>();

    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Tokyo"));
    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Seattle"));
    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "London"));

    // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
    return outputs;
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2. x Işlevleri)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const output = [];
    output.push(yield context.df.callActivity("E1_SayHello", "Tokyo"));
    output.push(yield context.df.callActivity("E1_SayHello", "Seattle"));
    output.push(yield context.df.callActivity("E1_SayHello", "London"));

    // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
    return output;
});
```

Her `await` (C#) veya `yield` (JavaScript) ifadesinde, dayanıklı görev çerçevesi işlevin yürütme durumunu tablo depolaması olarak kontrol noktaları. Bu durum, *düzenleme geçmişi*olarak adlandırılan şeydir.

## <a name="history-table"></a>Geçmiş tablosu

Genellikle, dayanıklı görev çerçevesi her denetim noktasında aşağıdakileri yapar:

1. Yürütme geçmişini Azure depolama tablolarına kaydeder.
2. Orchestrator 'ın çağırmak istediği işlevlere yönelik iletileri sıraya alır.
3. Orchestrator &mdash; için iletileri, dayanıklı Zamanlayıcı iletileri gibi kuyruğa alır.

Kontrol noktası tamamlandıktan sonra Orchestrator işlevi, kendisi için daha fazla iş tamamlanana kadar bellekten kaldırılabilir.

> [!NOTE]
> Azure depolama, verileri tablo depolama ve kuyruklara kaydetme arasında herhangi bir işlem garantisi sağlamaz. Dayanıklı İşlevler depolama sağlayıcısı, sorunları işlemek için *nihai tutarlılık* düzenlerini kullanır. Bu desenler, bir denetim noktasının ortasında kilitlenme veya bağlantı kaybı olursa hiçbir veri kaybolmamasını önler.

Tamamlandıktan sonra, daha önce gösterilen işlevin geçmişi, Azure Tablo Depolaması 'nda aşağıdakine benzer bir şekilde görünür (çizim amaçları için kısaltılmış):

| PartitionKey (InstanceId)                     | EventType             | Timestamp               | Girdi | Name             | Sonuç                                                    | Durum |
|----------------------------------|-----------------------|----------|--------------------------|-------|------------------|-----------------------------------------------------------|
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:32.362 Z |       |                  |                                                           |                     |
| eaee885b | ExecutionStarted      | 2017-05-05T18:45:28.852 Z | null  | E1_HelloSequence |                                                           |                     |
| eaee885b | Taskzamanlandı         | 2017-05-05T18:45:32.670 Z |       | E1_SayHello      |                                                           |                     |
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:32.670 Z |       |                  |                                                           |                     |
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:34.232 Z |       |                  |                                                           |                     |
| eaee885b | TaskCompleted         | 2017-05-05T18:45:34.201 Z |       |                  | "" "Merhaba Tokyo!" ""                                        |                     |
| eaee885b | Taskzamanlandı         | 2017-05-05T18:45:34.435 Z |       | E1_SayHello      |                                                           |                     |
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:34.435 Z |       |                  |                                                           |                     |
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:34.857 Z |       |                  |                                                           |                     |
| eaee885b | TaskCompleted         | 2017-05-05T18:45:34.763 Z |       |                  | "" "Merhaba Seattle!" ""                                      |                     |
| eaee885b | Taskzamanlandı         | 2017-05-05T18:45:34.857 Z |       | E1_SayHello      |                                                           |                     |
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:34.857 Z |       |                  |                                                           |                     |
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:35.032 Z |       |                  |                                                           |                     |
| eaee885b | TaskCompleted         | 2017-05-05T18:45:34.919 Z |       |                  | "" "Merhaba Londra!" ""                                       |                     |
| eaee885b | ExecutionCompleted    | 2017-05-05T18:45:35.044 Z |       |                  | "[" "Merhaba Tokyo!" "," "Merhaba Seattle!" "," "Merhaba Londra!" "]" | Tamamlandı           |
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:35.044 Z |       |                  |                                                           |                     |

Sütun değerlerinde birkaç Not:

* **Partitionkey**: Düzenleme örnek KIMLIĞINI içerir.
* **EventType**: Olayın türünü temsil eder. Aşağıdaki türlerden biri olabilir:
  * **Orchestrationstarted**: Orchestrator işlevi bir await 'den devam ettirildi veya ilk kez çalışıyor. Sütun, CurrentUtcDateTime API 'si için belirleyici değeri doldurmak için kullanılır. [](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) `Timestamp`
  * **Executionstarted**: Orchestrator işlevi ilk kez yürütülmeye başladı. Bu olay `Input` sütundaki işlev girişini de içerir.
  * **Taskzamanlandı**: Etkinlik işlevi zamanlandı. Etkinlik işlevinin adı `Name` sütununda yakalanır.
  * **TaskCompleted**: Etkinlik işlevi tamamlandı. İşlevin `Result` sonucu sütununda.
  * **Timercreated**: Dayanıklı bir Zamanlayıcı oluşturuldu. `FireAt` Sütun, zamanlayıcının süresinin dolacağı zamanlanmış UTC saatini içerir.
  * **Timertetiklendi**: Dayanıklı bir Zamanlayıcı tetiklendi.
  * **Eventraıof**: Orchestration örneğine bir dış olay gönderildi. Sütun, olayın adını yakalar `Input` ve sütun, olayın yükünü yakalar. `Name`
  * **Orchestratorcompleted**: Orchestrator işlevi bekletildi.
  * **Continueasnew**: Orchestrator işlevi tamamlandı ve yeni bir durumla birlikte başlatıldı. `Result` Sütunu, yeniden başlatılan örnekte girdi olarak kullanılan değerini içerir.
  * **Executioncompleted**: Orchestrator işlevi tamamlamaya çalıştı (veya başarısız). İşlevin çıkışları veya hata ayrıntıları `Result` sütununda depolanır.
* **Zaman damgası**: Geçmiş olayının UTC zaman damgası.
* **Ad**: Çağrılan işlevin adı.
* **Giriş**: İşlevin JSON biçimli girişi.
* **Sonuç**: İşlevin çıkışı; diğer bir deyişle, dönüş değeridir.

> [!WARNING]
> Hata ayıklama aracı olarak yararlı olsa da, bu tablo üzerinde hiçbir bağımlılığı olmaz. Dayanıklı İşlevler uzantısı geliştikçe değişiklik gösterebilir.

İşlev bir `await` (C#) veya `yield` (JavaScript) ' den her başlatıldığında, dayanıklı görev çerçevesi Orchestrator işlevini sıfırdan yeniden çalıştırır. Her yeniden çalıştırıldığında, geçerli zaman uyumsuz işlemin yapılıp yapılmayacağını belirlemede yürütme geçmişine bakar.  İşlem gerçekleştiyse, çerçeve bu işlemin çıkışını hemen yeniden yürütür ve Next `await` (C#) ya `yield` da (JavaScript) ' e gider. Bu işlem, tüm geçmiş yeniden yürütülene kadar devam eder, bu noktada Orchestrator işlevindeki tüm yerel değişkenler önceki değerlerine geri yüklenir.

## <a name="orchestrator-code-constraints"></a>Orchestrator kod kısıtlamaları

Yeniden yürütme davranışı, bir Orchestrator işlevinde yazılabilen kod türü üzerinde kısıtlamalar oluşturur:

* Orchestrator kodu **belirleyici**olmalıdır. Bu, birden çok kez yeniden yürütülür ve her seferinde aynı sonucu üretmelidir. Örneğin, geçerli tarih/saat almak, rastgele sayılar almak, rastgele GUID 'Ler oluşturmak veya uzak uç noktalara çağrı yapmak için doğrudan çağrı yoktur.

  Orchestrator kodunun geçerli tarih/saati alması gerekiyorsa, yeniden yürütme için güvenli olan [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) (.net) veya `currentUtcDateTime` (JavaScript) API 'sini kullanması gerekir.

  Orchestrator kodunun rastgele bir GUID oluşturması gerekiyorsa, yeniden yürütme için güvenli olan [NewGuid](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_NewGuid) (.net) veya `newGuid` (JavaScript) API 'sini kullanmanız gerekir.

   Bu özel durumlar dışında, etkinlik işlevlerinde belirleyici olmayan işlemler yapılmalıdır. Bu, diğer giriş veya çıkış bağlamalarıyla herhangi bir etkileşimi içerir. Bu, ilk yürütme sırasında belirleyici olmayan tüm değerlerin bir kez oluşturulmasını ve yürütme geçmişine kaydedilmesini sağlar. Sonraki yürütmeler daha sonra kaydedilen değeri otomatik olarak kullanacaktır.

* Orchestrator kodu **engellenmemiş**olmalıdır. Örneğin, bu, hiçbir g/ç ve `Thread.Sleep` (.net) veya eşdeğer API çağrısı olmayan anlamına gelir.

  Bir Orchestrator 'ın geciktirilmesi gerekiyorsa [CreateTimer](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CreateTimer_) (.net) veya `createTimer` (JavaScript) API 'sini kullanabilir.

* Orchestrator kodu, [durableorchestrationcontext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) API 'si veya `context.df` nesnenin API 'si dışında **herhangi bir zaman uyumsuz işlem başlatmalıdır** . Örneğin `Task.Run`, Hayır veya`HttpClient.SendAsync`.net veya`setTimeout()` JavaScript içinde`setInterval()` . `Task.Delay` Dayanıklı görev çerçevesi, Orchestrator kodunu tek bir iş parçacığında yürütür ve diğer zaman uyumsuz API 'Ler tarafından zamanlanabilecek diğer iş parçacıklarıyla etkileşime giremezsiniz. Bunun gerçekleşmesi, `InvalidOperationException` özel durum oluşturulması gerekir.

> [!NOTE]
> [Durableorchestrationclient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) API 'si, bir Orchestrator işlevinde izin verilmeyen ve yalnızca Orchestrator olmayan işlevlerde kullanılabilen zaman uyumsuz g/ç işlemini gerçekleştirir.

* Orchestrator kodunda **sonsuz döngüler kaçınılmalıdır** . Sürekli görev çerçevesi Orchestration işlevi ilerledikçe yürütme geçmişini kaydettiğinden, sonsuz bir döngü bir Orchestrator örneğinin bellek tükenmesine neden olabilir. Sonsuz döngü senaryolarında, işlev yürütmeyi yeniden başlatmak ve önceki yürütme geçmişini atmak için [continueasnew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_) (.net) veya `continueAsNew` (JavaScript) gibi API 'leri kullanın.

* JavaScript Orchestrator işlevleri `async`olamaz. Zaman uyumlu Oluşturucu işlevleri olarak bildirilmelidir.

Bu kısıtlamalar ilk başta göz katabilir, ancak uygulamada izlenmesi zor değildir. Dayanıklı görev çerçevesi yukarıdaki kuralların ihlallerini algılamaya çalışır ve bir `NonDeterministicOrchestrationException`oluşturur. Ancak, bu algılama davranışı en iyi çabadır ve buna bağlı olmanız gerekmez.

> [!NOTE]
> Bu kuralların tümü yalnızca `orchestrationTrigger` bağlama tarafından tetiklenen işlevler için geçerlidir. `activityTrigger` Bağlama tarafından tetiklenen etkinlik işlevleri ve `orchestrationClient` bağlamayı kullanan işlevler bu tür sınırlamalara sahip değildir.

## <a name="durable-tasks-net"></a>Dayanıklı görevler (.NET)

> [!NOTE]
> Bu bölümde, dayanıklı görev çerçevesinin iç uygulama ayrıntıları açıklanmaktadır. Bu bilgileri bilmeden Dayanıklı İşlevler kullanabilirsiniz. Yalnızca yeniden yürütme davranışını anlamanıza yardımcı olmak için tasarlanmıştır.

Orchestrator işlevlerinde güvenli bir şekilde beklebilen görevler bazen *dayanıklı görevler*olarak adlandırılır. Bunlar, dayanıklı görev çerçevesi tarafından oluşturulan ve yönetilen görevlerdir. , `CallActivityAsync` ,`WaitForExternalEvent`Ve tarafından`CreateTimer`döndürülen görevler örnektir.

Bu *dayanıklı görevler* , bir `TaskCompletionSource` nesne listesi kullanılarak dahili olarak yönetilir. Yeniden yürütme sırasında, bu görevler Orchestrator Code Execution 'in bir parçası olarak oluşturulur ve dağıtıcı ilgili geçmiş olaylarını numaralandırdığından tamamlanır. Tüm geçmiş yeniden yürütülene kadar tek bir iş parçacığı kullanılarak bu işlem zaman uyumlu olarak gerçekleştirilir. Geçmişin sonuna kadar tamamlanmayan dayanıklı görevler, uygun eylemlere sahiptir. Örneğin, bir ileti bir etkinlik işlevini çağırmak üzere sıraya alınmış olabilir.

Burada açıklanan yürütme davranışı, Orchestrator işlev kodunun neden asla `await` dayanıklı olmayan bir görev olması gerektiğini anlamanıza yardımcı olmalıdır: Dağıtıcı iş parçacığı işlemin tamamlanmasını bekleyemez ve bu görev tarafından geri çağırma işlemi, izlemeyi bozabilir Orchestrator işlevinin durumu. Bunu engellemeye çalışmak için bazı çalışma zamanı denetimleri yerinde.

Dayanıklı görev çerçevesinin Orchestrator işlevlerini nasıl yürüttüğünü hakkında daha fazla bilgi isterseniz, yapılacak en iyi şey [GitHub 'Daki dayanıklı görev kaynak koduna](https://github.com/Azure/durabletask)danışmanız gerekir. Özellikle, bkz. [TaskOrchestrationExecutor.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationExecutor.cs) and [TaskOrchestrationContext.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationContext.cs)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örnek yönetimi hakkında bilgi edinin](durable-functions-instance-management.md)