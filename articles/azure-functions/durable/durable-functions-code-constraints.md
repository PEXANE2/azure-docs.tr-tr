---
title: Dayanıklı Orchestrator kod kısıtlamaları-Azure Işlevleri
description: Azure Dayanıklı İşlevler için düzenleme işlevi yeniden yürütme ve kod kısıtlamaları.
author: cgillum
ms.topic: conceptual
ms.date: 11/02/2019
ms.author: azfuncdf
ms.openlocfilehash: 4ed604302ca187ad4953e865d68dc73030a37c02
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77562148"
---
# <a name="orchestrator-function-code-constraints"></a>Orchestrator işlev kodu kısıtlamaları

Dayanıklı İşlevler, [Azure işlevlerinin](../functions-overview.md) bir uzantısıdır ve durum bilgisi olan uygulamalar oluşturmanıza olanak tanır. Bir işlev uygulaması içindeki diğer dayanıklı işlevlerin yürütülmesini düzenlemek için bir [Orchestrator işlevi](durable-functions-orchestrations.md) kullanabilirsiniz. Orchestrator işlevleri durum bilgisi olan, güvenilir ve uzun süreli çalışır.

## <a name="orchestrator-code-constraints"></a>Düzenleyici kodu kısıtlamaları

Orchestrator işlevleri, güvenilir bir yürütme sağlamak ve yerel değişken durumunu korumak için [olay](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing) kaynağını kullanır. Orchestrator kodunun yeniden [yürütme davranışı](durable-functions-orchestrations.md#reliability) , bir Orchestrator işlevinde yazabileceğiniz kod türü üzerinde kısıtlamalar oluşturur. Örneğin, Orchestrator işlevleri *belirleyici*olmalıdır: bir Orchestrator işlevi birden çok kez yeniden yürütülür ve her seferinde aynı sonucu üretmelidir.

### <a name="using-deterministic-apis"></a>Belirleyici API 'Leri kullanma

Bu bölümde, kodunuzun belirleyici olmasını sağlamaya yardımcı olacak bazı basit yönergeler sunulmaktadır.

Orchestrator işlevleri, hedef dillerinde API 'leri çağırabilir. Ancak, Orchestrator işlevlerinin yalnızca belirleyici API 'Leri çağırması önemlidir. *Belirleyici BIR API* , her zaman aynı girişe verilen değeri, ne zaman veya ne sıklıkta çağrdığını BELIRTEN bir API 'dir.

Aşağıdaki tabloda, oluşmaması gereken API 'Ler örnekleri gösterilmektedir çünkü belirleyici *değildir* . Bu kısıtlamalar yalnızca Orchestrator işlevleri için geçerlidir. Diğer işlev türlerinde bu tür kısıtlamalar yoktur.

| API kategorisi | Neden | Geçici çözüm |
| ------------ | ------ | ---------- |
| Tarihler ve saatler  | Döndürülen değer her yeniden yürütme için farklı olduğundan geçerli tarih veya saati döndüren API 'Ler belirleyici değildir. | `CurrentUtcDateTime` .NET veya `currentUtcDateTime` API içindeki API 'yi, yeniden yürütme için güvenli olan JavaScript 'te kullanın. |
| GUID 'Ler ve UUID 'ler  | Oluşturulan değer her yeniden yürütme için farklı olduğundan rastgele bir GUID veya UUID döndüren API 'Ler belirleyici değildir. | Rastgele `NewGuid` GUID 'leri güvenle `newGuid` oluşturmak için .NET veya JavaScript içinde kullanın. |
| Rastgele sayılar | Oluşturulan değer her yeniden yürütme için farklı olduğundan rastgele sayılar döndüren API 'Ler belirleyici değildir. | Bir düzenleme için rastgele sayılar döndürmek üzere bir etkinlik işlevi kullanın. Etkinlik işlevlerinin dönüş değerleri her zaman yeniden yürütme için güvenlidir. |
| Bağlamalar | Giriş ve çıkış bağlamaları genellikle g/ç ve belirleyici olmayan bir şekilde yapılır. Orchestrator işlevinin [düzenleme istemcisi](durable-functions-bindings.md#orchestration-client) ve [varlık istemci](durable-functions-bindings.md#entity-client) bağlamaları bile doğrudan kullanılması gerekir. | İstemci veya etkinlik işlevleri içindeki giriş ve çıkış bağlamalarını kullanın. |
| Ağ | Ağ çağrıları dış sistemler içerir ve belirleyici değildir. | Ağ çağrıları yapmak için etkinlik işlevlerini kullanın. Orchestrator işlevinizden bir HTTP çağrısı yapmanız gerekiyorsa, [DAYANıKLı HTTP API 'lerini](durable-functions-http-features.md#consuming-http-apis)de kullanabilirsiniz. |
| API 'Leri engelleme | .NET ve benzer `Thread.Sleep` API 'lerde olduğu gibi API 'leri engellemek, Orchestrator işlevleri için performans ve ölçeklendirme sorunlarına neden olabilir ve kaçınılmalıdır. Azure Işlevleri tüketim planında, bu, hatta gereksiz çalışma zamanı ücretlerine neden olabilir. | Kullanılabilir olduklarında API 'Leri engellemek için alternatifleri kullanın. Örneğin, düzenleme yürütmesindeki gecikmeleri tanıtmak için kullanın `CreateTimer` . [Sürekli süreölçer](durable-functions-timers.md) gecikmeleri, bir Orchestrator işlevinin yürütme zamanına doğru sayılmaz. |
| Zaman uyumsuz API 'Ler | Orchestrator kodu, `IDurableOrchestrationContext` API veya `context.df` nesnenin API 'si dışında herhangi bir zaman uyumsuz işlem başlatmalıdır. Örneğin, `Task.Run`, `Task.Delay`, `HttpClient.SendAsync` ve ' ı .NET veya `setTimeout` JavaScript `setInterval` 'te kullanamazsınız. Dayanıklı görev çerçevesi, tek bir iş parçacığında Orchestrator kodunu çalıştırır. Diğer zaman uyumsuz API 'Ler tarafından çağrılabilen diğer iş parçacıklarıyla etkileşime giremeyebilir. | Orchestrator işlevi yalnızca dayanıklı zaman uyumsuz çağrılar sağlamalıdır. Etkinlik işlevleri, başka bir zaman uyumsuz API çağrısı yapmalıdır. |
| Zaman uyumsuz JavaScript işlevleri | Node. js çalışma zamanı zaman uyumsuz `async` işlevlerin belirleyici olduğunu garanti etmez, JavaScript Orchestrator işlevlerini bildiremezsiniz. | JavaScript Orchestrator işlevlerini zaman uyumlu Oluşturucu işlevleri olarak bildirin. |
| İş parçacığı API 'Leri | Dayanıklı görev çerçevesi, Orchestrator kodunu tek bir iş parçacığında çalıştırır ve diğer iş parçacıklarıyla etkileşime giremeyen bir işlem olamaz. Orchestration yürütmesinin yeni iş parçacıklarını tanıtma, belirleyici olmayan yürütme veya kilitlenmeyle sonuçlanabilir. | Orchestrator işlevleri, iş parçacığı API 'Lerini neredeyse asla kullanmaz. Örneğin, .NET 'te kullanmaktan kaçının `ConfigureAwait(continueOnCapturedContext: false)`; Bu, görev devamlılıklarının Orchestrator işlevinin orijinalde `SynchronizationContext`çalıştırılmasını sağlar. Bu tür API 'Ler gerekliyse, kullanımlarını yalnızca etkinlik işlevlerine sınırlayın. |
| Statik değişkenler | Orchestrator işlevlerinde sabit olmayan statik değişkenler kullanmaktan kaçının çünkü değerleri zaman içinde değişebilir, bu durum belirleyici olmayan çalışma zamanı davranışına neden olur. | Sabitleri kullanın veya statik değişkenlerin kullanımını etkinlik işlevleriyle sınırlandırın. |
| Ortam değişkenleri | Orchestrator işlevlerinde ortam değişkenlerini kullanmayın. Değerleri zaman içinde değişebilir ve belirleyici olmayan çalışma zamanı davranışına neden olabilir. | Ortam değişkenlerine yalnızca istemci işlevleri veya etkinlik işlevleri içinden başvurulmalıdır. |
| Sonsuz döngüler | Orchestrator işlevlerinde sonsuz döngüleri önleyin. Sürekli görev çerçevesi Orchestration işlevi ilerledikçe yürütme geçmişini kaydettiğinden, sonsuz bir döngü bir Orchestrator örneğinin bellek tükenmesine neden olabilir. | Sonsuz döngü senaryolarında, işlev yürütmeyi yeniden başlatmak `ContinueAsNew` ve önceki yürütme `continueAsNew` geçmişini atmak için .NET veya JavaScript 'te gibi API 'leri kullanın. |

Bu kısıtlamaları uygulamak ilk olarak zor görünebilir, ancak uygulamada izlenmesi kolay bir işlemdir.

Dayanıklı görev çerçevesi, önceki kuralların ihlallerini algılamaya çalışır. Bir ihlal bulursa Framework bir **NonDeterministicOrchestrationException** özel durumu oluşturur. Ancak, bu algılama davranışı tüm ihlalleri yakalamaz ve buna bağlı kalmazsınız.

## <a name="versioning"></a>Sürüm Oluşturma

Sürekli bir düzenleme, günler, aylar, yıllar [veya hatta önemli](durable-functions-eternal-orchestrations.md)bir şekilde çalışabilir. Tamamlanmamış düzenlemeleri etkileyen Dayanıklı İşlevler uygulamalarda yapılan tüm kod güncelleştirmeleri, düzenleme yeniden yürütme davranışını bozabilir. Bu nedenle, kod üzerinde güncelleştirme yaparken dikkatli bir şekilde planlamanız önemlidir. Kodunuzun sürümünün nasıl kullanılacağına ilişkin daha ayrıntılı bir açıklama için [sürüm oluşturma makalesine](durable-functions-versioning.md)bakın.

## <a name="durable-tasks"></a>Dayanıklı görevler

> [!NOTE]
> Bu bölümde, dayanıklı görev çerçevesinin iç uygulama ayrıntıları açıklanmaktadır. Bu bilgileri bilmeden dayanıklı işlevleri kullanabilirsiniz. Yalnızca yeniden yürütme davranışını anlamanıza yardımcı olmak için tasarlanmıştır.

Orchestrator işlevlerinde güvenle beklemeleri gereken görevler bazen *dayanıklı görevler*olarak adlandırılır. Dayanıklı görev çerçevesi bu görevleri oluşturur ve yönetir. .NET Orchestrator işlevlerinde **CallActivityAsync**, **WaitForExternalEvent**ve **CreateTimer** tarafından döndürülen görevler örnektir.

Bu dayanıklı görevler, .NET 'teki bir `TaskCompletionSource` nesne listesi tarafından dahili olarak yönetilir. Yeniden yürütme sırasında, bu görevler Orchestrator Code Execution 'ın bir parçası olarak oluşturulur. Dağıtıcı karşılık gelen geçmiş olaylarını numaralandırdığından tamamlanırlar.

Görevler, tüm geçmiş yeniden yürütülene kadar tek bir iş parçacığı kullanılarak eşzamanlı olarak yürütülür. Geçmişi yeniden yürütme işlemi tarafından bitmeyen dayanıklı görevler, uygun eylemlere sahiptir. Örneğin, bir ileti bir etkinlik işlevini çağırmak üzere sıraya alınmış olabilir.

Bu bölümün çalışma zamanı davranışının açıklaması, bir Orchestrator işlevinin neden veya `await` `yield` dayanıklı olmayan bir görevde nasıl kullanamadığına yardımcı olmalıdır. İki neden vardır: Dağıtıcı iş parçacığı görevin bitmesini bekleyemez ve bu görev tarafından yapılan geri çağırma işlemi Orchestrator işlevinin izleme durumunu bozabilir. Bu ihlallerin algılanmasına yardımcı olmak için bazı çalışma zamanı denetimleri yerinde.

Dayanıklı görev çerçevesinin Orchestrator işlevlerini nasıl çalıştırdığı hakkında daha fazla bilgi edinmek için [GitHub 'Daki dayanıklı görev kaynak koduna](https://github.com/Azure/durabletask)başvurun. Özellikle, bkz. [TaskOrchestrationExecutor.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationExecutor.cs) and [TaskOrchestrationContext.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationContext.cs).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Alt düzenlemeleri çağırmayı öğrenin](durable-functions-sub-orchestrations.md)

> [!div class="nextstepaction"]
> [Sürüm oluşturmayı nasıl ele alabileceğinizi öğrenin](durable-functions-versioning.md)
