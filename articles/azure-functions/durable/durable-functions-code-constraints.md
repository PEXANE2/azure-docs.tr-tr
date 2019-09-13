---
title: Dayanıklı Orchestrator kod kısıtlamaları-Azure Işlevleri
description: Azure Dayanıklı İşlevler için düzenleme işlevi yeniden yürütme ve kod kısıtlamaları.
author: cgillum
manager: gwallace
keywords: ''
ms.service: azure-functions
ms.topic: conceptual
ms.date: 08/18/2019
ms.author: azfuncdf
ms.openlocfilehash: 87851a4879760c76950f765d05de4662ecb04bea
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70935774"
---
# <a name="orchestrator-function-code-constraints"></a>Orchestrator işlev kodu kısıtlamaları

Dayanıklı İşlevler, [Azure işlevlerinin](../functions-overview.md) bir uzantısıdır ve durum bilgisi olan uygulamalar oluşturmanıza olanak tanır. Bir işlev uygulaması içindeki diğer dayanıklı işlevlerin yürütülmesini düzenlemek için bir [Orchestrator işlevi](durable-functions-orchestrations.md) kullanabilirsiniz. Orchestrator işlevleri durum bilgisi olan, güvenilir ve uzun süreli çalışır.

## <a name="orchestrator-code-constraints"></a>Orchestrator kod kısıtlamaları

Orchestrator işlevleri, güvenilir bir yürütme sağlamak ve yerel değişken durumunu korumak için [olay](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing) kaynağını kullanır. Orchestrator kodunun yeniden [yürütme davranışı](durable-functions-orchestrations.md#reliability) , bir Orchestrator işlevinde yazabileceğiniz kod türü üzerinde kısıtlamalar oluşturur. Örneğin, Orchestrator kodu *belirleyici*olmalıdır.  Orchestrator işlevleri birden çok kez yeniden yürütülür ve her seferinde aynı sonucu üretmelidir.

Aşağıdaki bölümlerde, kodunuzun belirleyici olmasını sağlamaya yönelik bazı basit yönergeler sağlanmaktadır.

### <a name="using-deterministic-apis"></a>Belirleyici API 'Leri kullanma

Orchestrator işlevleri, hedef dillerinde istedikleri API 'leri çağırmak için ücretsizdir. Ancak, Orchestrator işlevlerinin yalnızca *belirleyici* API 'leri çağırması önemlidir. *Belirleyici BIR API* , her zaman aynı girişe verilen aynı değere sahip olan ve ne zaman ve ne sıklıkta çağrdığını BELIRTEN bir API 'dir.

Aşağıda *, belirleyici olmadıklarından* kaçınılması gereken API 'ler örnekleri verilmiştir. Bu kısıtlamalar yalnızca Orchestrator işlevleri için geçerlidir. Diğer işlev türlerinde bu tür kısıtlamalar yoktur.

| API kategorisi | Reason | Geçici Çözüm |
| ------------ | ------ | ---------- |
| Tarih/saat  | _Geçerli_ tarih veya saati döndüren API 'ler, her yeniden yürütme için farklı olacak şekilde değer döndürmediği için belirleyici değildir. | Yeniden yürütme için güvenli olan [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) ( `currentUtcDateTime` .net) veya (JavaScript) API 'sini kullanın. |
| GUID 'Ler/UUID 'ler  | Oluşturulan değer her yeniden yürütme için farklı olacağı için _rastgele_ bir GUID/UUID döndüren API 'ler belirleyici değildir. | Rastgele GUID 'leri güvenle oluşturmak için [NEWGUID](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_NewGuid) (.net) veya `newGuid` (JavaScript) kullanın. |
| Rastgele sayılar | Oluşturulan değer her yeniden yürütme için farklı olacağı için rastgele sayılar döndüren API 'Ler belirleyici değildir. | Bir düzenleme için rastgele sayılar döndürmek üzere bir etkinlik işlevi kullanın. Etkinlik işlevlerinin dönüş değerleri her zaman yeniden yürütme için güvenlidir. |
| Bağlamalar | Giriş ve çıkış bağlamaları genellikle g/ç ve belirleyici olmayan bir şekilde yapılır. [Orchestration istemcisi](durable-functions-bindings.md#orchestration-client) ve [varlık istemci](durable-functions-bindings.md#entity-client) bağlamaları bile doğrudan bir Orchestrator işlevi tarafından kullanılmamalıdır. | İstemci veya etkinlik işlevleri içindeki giriş ve çıkış bağlamalarını kullanın. |
| Ağ | Ağ çağrıları dış sistemler içerir ve belirleyici değildir. | Ağ çağrıları yapmak için etkinlik işlevlerini kullanın. Orchestrator işlevinizden bir HTTP çağrısı yapmanız gerekiyorsa, [DAYANıKLı HTTP API 'lerini](durable-functions-http-features.md#consuming-http-apis)kullanma seçeneğiniz de vardır. |
| API 'Leri engelleme | `Thread.Sleep` (.Net) veya diğer benzer API 'ler gibi API 'leri engellemek, Orchestrator işlevleri için performans ve ölçeklendirme sorunlarına neden olabilir ve kaçınılmalıdır. Azure Işlevleri tüketim planında, bu, hatta gereksiz yürütme süresi ücretlerine neden olabilir. | Düzenleme yürütmesindeki gecikmeleri tanıtmak gibi `CreateTimer` , kullanılabilir olduğunda API 'leri engellemek için alternatifleri kullanın. [Sürekli süreölçer](durable-functions-timers.md) gecikmeleri, bir Orchestrator işlevinin yürütme zamanına doğru sayılmaz. |
| Zaman uyumsuz API 'Ler | Orchestrator kodu, [durableorchestrationcontext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) API 'si veya `context.df` nesnenin API 'si dışında **herhangi bir zaman uyumsuz işlem başlatmalıdır** . Örneğin `Task.Run`, Hayır veya`HttpClient.SendAsync`.net veya`setTimeout()` JavaScript içinde`setInterval()` . `Task.Delay` Dayanıklı görev çerçevesi, Orchestrator kodunu tek bir iş parçacığında yürütür ve diğer zaman uyumsuz API 'Ler tarafından zamanlanabilecek diğer iş parçacıklarıyla etkileşime giremezsiniz. | Yalnızca *dayanıklı* zaman uyumsuz çağrılar bir Orchestrator işlevi tarafından yapılmalıdır. Diğer zaman uyumsuz API çağrılarının etkinlik işlevlerinden yapılması gerekir. |
| Zaman uyumsuz JavaScript işlevleri | Node. js çalışma zamanının `async` zaman uyumsuz işlevlerin belirleyici olduğunu garanti etmez, JavaScript Orchestrator işlevleri olamaz. | JavaScript Orchestrator işlevleri, zaman uyumlu Oluşturucu işlevleri olarak bildirilmelidir. |
| İş parçacığı API 'Leri | Dayanıklı görev çerçevesi, Orchestrator kodunu tek bir iş parçacığında yürütür ve diğer iş parçacıklarıyla etkileşime giremezsiniz. Orchestration yürütmesinin yeni iş parçacıklarını tanıtma, belirleyici olmayan yürütme veya kilitlenmeyle sonuçlanabilir. | İş parçacığı API 'Leri Orchestrator işlevlerinde neredeyse asla kullanılmamalıdır. Bunlar gerekliyse, etkinlik işlevleriyle sınırlı olmaları gerekir. |
| Statik değişkenler | Sabit olmayan statik değişkenler Orchestrator işlevlerinde kaçınılmalıdır, çünkü değerleri zaman içinde değişebilir, belirleyici olmayan yürütme davranışına neden olur. | Sabitleri kullanın veya statik değişkenlerin kullanımını etkinlik işlevlerine sınırlayın. |
| Ortam değişkenleri | Orchestrator işlevlerinde ortam değişkenlerini kullanmayın. Değerleri zaman içinde değişebilir ve belirleyici olmayan yürütme davranışına neden olur. | Ortam değişkenlerine yalnızca istemci işlevleri veya etkinlik işlevleri içinden başvurulmalıdır. |
| Sonsuz döngüler | Orchestrator işlevlerinde sonsuz döngüler kaçınılmalıdır. Sürekli görev çerçevesi, düzenleme işlevi ilerledikçe yürütme geçmişini kaydeder. bu nedenle, sonsuz bir döngü bir Orchestrator örneğinin bellek tükenmesine neden olabilir. | Sonsuz döngü senaryolarında, işlev yürütmeyi yeniden başlatmak ve önceki yürütme geçmişini atmak için [continueasnew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_) (.net) veya `continueAsNew` (JavaScript) gibi API 'leri kullanın. |

Bu kısıtlamalar ilk başta göz katabilir, ancak uygulamada izlenmesi zor değildir. Dayanıklı görev çerçevesi yukarıdaki kuralların ihlallerini algılamaya çalışır ve bir `NonDeterministicOrchestrationException`oluşturur. Ancak, bu algılama davranışı en iyi çabadır ve buna bağlı olmanız gerekmez.

## <a name="versioning"></a>Sürüm oluşturma

Sürekli bir düzenleme, günler, aylar, yıllar [veya hatta önemli](durable-functions-eternal-orchestrations.md)bir şekilde çalışabilir. Henüz tamamlanmış düzenlemeleri etkileyen Dayanıklı İşlevler uygulamalarda yapılan herhangi bir kod güncelleştirmesi, yeniden yürütme davranışlarını bozabilir. Bu nedenle, kodda güncelleştirme yaparken dikkatli bir şekilde planlamanız önemlidir. Kodunuzun sürümünün nasıl kullanılacağına ilişkin daha ayrıntılı bir açıklama için [sürüm oluşturma](durable-functions-versioning.md) makalesine bakın.

## <a name="durable-tasks"></a>Dayanıklı görevler

> [!NOTE]
> Bu bölümde, dayanıklı görev çerçevesinin iç uygulama ayrıntıları açıklanmaktadır. Bu bilgileri bilmeden Dayanıklı İşlevler kullanabilirsiniz. Yalnızca yeniden yürütme davranışını anlamanıza yardımcı olmak için tasarlanmıştır.

Orchestrator işlevlerinde güvenli bir şekilde beklebilen görevler bazen *dayanıklı görevler*olarak adlandırılır. Bu görevler, dayanıklı görev çerçevesi tarafından oluşturulur ve yönetilir. .Net Orchestrator işlevlerinde, `CallActivityAsync` `WaitForExternalEvent`, ve `CreateTimer` tarafından döndürülen görevler örnektir.

Bu *dayanıklı görevler* , .net 'teki bir `TaskCompletionSource` nesne listesi kullanılarak dahili olarak yönetilir. Yeniden yürütme sırasında, bu görevler Orchestrator Code Execution 'in bir parçası olarak oluşturulur ve dağıtıcı ilgili geçmiş olaylarını numaralandırdığından tamamlanır. Tüm geçmiş yeniden yürütülene kadar yürütme tek bir iş parçacığı kullanılarak eşzamanlı olarak gerçekleştirilir. Geçmişin sonuna kadar tamamlanmayan dayanıklı görevler, uygun eylemlere sahiptir. Örneğin, bir ileti bir etkinlik işlevini çağırmak üzere sıraya alınmış olabilir.

Burada açıklanan yürütme davranışı, Orchestrator işlev kodunun neden asla `await` ne de `yield` dayanıklı olmayan bir görev olması gerektiğini anlamanıza yardımcı olmalıdır: Dağıtıcı iş parçacığı işlemin tamamlanmasını bekleyemez ve bu görev tarafından geri çağırma olasılığı olabilir Orchestrator işlevinin izleme durumu bozuk. Bu ihlalleri algılamaya çalışmak için bazı çalışma zamanı denetimleri yerinde.

Dayanıklı görev çerçevesinin Orchestrator işlevlerini nasıl yürüttüğünü hakkında daha fazla bilgi isterseniz, yapılacak en iyi şey [GitHub 'Daki dayanıklı görev kaynak koduna](https://github.com/Azure/durabletask)danışmanız gerekir. Özellikle, bkz. [TaskOrchestrationExecutor.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationExecutor.cs) and [TaskOrchestrationContext.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationContext.cs)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Alt düzenlemeleri çağırmayı öğrenin](durable-functions-sub-orchestrations.md)

> [!div class="nextstepaction"]
> [Sürüm oluşturmayı nasıl ele alabileceğinizi öğrenin](durable-functions-versioning.md)
