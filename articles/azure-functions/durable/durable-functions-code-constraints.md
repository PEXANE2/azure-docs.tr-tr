---
title: Dayanıklı orkestratör kodu kısıtlamaları - Azure Fonksiyonları
description: Azure Dayanıklı İşlevler için düzenleme işlevi yeniden oynatma ve kod kısıtlamaları.
author: cgillum
ms.topic: conceptual
ms.date: 11/02/2019
ms.author: azfuncdf
ms.openlocfilehash: 4ed604302ca187ad4953e865d68dc73030a37c02
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77562148"
---
# <a name="orchestrator-function-code-constraints"></a>Orchestrator işlev kodu kısıtlamaları

Dayanıklı Işlevler, [azure işlevlerinin](../functions-overview.md) bir uzantısıdır ve özel uygulamalar oluşturmanıza olanak tanır. Bir işlev uygulaması içindeki diğer dayanıklı işlevlerin yürütülmesini düzenlemek için bir [orkestratör işlevi](durable-functions-orchestrations.md) kullanabilirsiniz. Orchestrator işlevleri durum lu, güvenilir ve potansiyel olarak uzun sürelidir.

## <a name="orchestrator-code-constraints"></a>Düzenleyici kodu kısıtlamaları

Orchestrator işlevleri, güvenilir yürütmeyi sağlamak ve yerel değişken durumunu korumak için [olay kaynağını](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing) kullanır. Orchestrator kodunun [yeniden oynatma davranışı,](durable-functions-orchestrations.md#reliability) bir orkestratör işlevinde yazabileceğiniz kod türünde kısıtlamalar oluşturur. Örneğin, orkestratör işlevleri *deterministik*olmalıdır: bir orkestratör işlevi birden çok kez yeniden çalınacak ve her seferinde aynı sonucu üretmelidir.

### <a name="using-deterministic-apis"></a>Deterministik API'leri kullanma

Bu bölümde, kodunuzu deterministic olduğundan emin olmak için bazı basit yönergeler sağlar.

Orchestrator işlevleri hedef dillerindeki herhangi bir API'yi arayabilir. Ancak, orkestratör işlevlerinin yalnızca deterministik API'leri çağırması önemlidir. *Deterministik API,* ne zaman veya ne sıklıkta çağrıldığına bakılmaksızın, aynı girdi yi verilen değeri her zaman döndüren bir API'dir.

Aşağıdaki tabloda, deterministic *olmadığı* için kaçınmanız gereken API örnekleri gösterilmektedir. Bu kısıtlamalar yalnızca orkestratör işlevleri için geçerlidir. Diğer işlev türlerinin bu tür kısıtlamaları yoktur.

| API kategorisi | Neden | Geçici çözüm |
| ------------ | ------ | ---------- |
| Tarihler ve saatler  | Döndürülen değer her tekrar için farklı olduğundan, geçerli tarih veya saati döndüren API'ler belirsizdir. | .NET'teki`CurrentUtcDateTime` API'yi `currentUtcDateTime` veya yeniden oynatma kullanabilirsiniz. |
| GUIDs ve UUIDs  | Rasgele bir GUID veya UUID döndüren API'ler, oluşturulan değer her tekrar için farklı olduğundan belirsizdir. | Rastgele `NewGuid` GUID'leri güvenli bir şekilde oluşturmak için .NET veya `newGuid` JavaScript'te kullanın. |
| Rastgele sayılar | Oluşturulan değer her tekrar için farklı olduğundan rasgele sayılar döndüren API'ler nondeterministic vardır. | Rasgele sayıları bir orkestrasyon'a döndürmek için bir etkinlik işlevi kullanın. Etkinlik işlevlerinin dönüş değerleri yeniden oynatma için her zaman güvenlidir. |
| Bağlamalar | Giriş ve çıkış bağlayıcıları genellikle G/Ç yapar ve nondeterministic vardır. Bir orkestratör işlevi, [orkestrasyon istemcisi](durable-functions-bindings.md#orchestration-client) ve [varlık istemcisi](durable-functions-bindings.md#entity-client) bağlamalarını bile doğrudan kullanmamalıdır. | İstemci veya etkinlik işlevleri içinde giriş ve çıkış bağlamaları kullanın. |
| Ağ | Ağ aramaları dış sistemleri içerir ve belirsizdir. | Ağ aramaları yapmak için etkinlik işlevlerini kullanın. Orkestratör işlevinizden bir HTTP çağrısı yapmanız gerekiyorsa, [dayanıklı HTTP API'lerini](durable-functions-http-features.md#consuming-http-apis)de kullanabilirsiniz. |
| API'leri engelleme | .NET ve `Thread.Sleep` benzeri API'lerde olduğu gibi API'leri engellemek, orkestratör işlevleri için performans ve ölçek sorunlarına neden olabilir ve kaçınılmalıdır. Azure İşlevler Tüketimi planında, gereksiz çalışma zamanı ücretlerine bile neden olabilirler. | Kullanılabilir olduklarında API'leri engellemeye alternatifler kullanın. Örneğin, orkestrasyon yürütmegecikmelertanıtmak için kullanın. `CreateTimer` [Dayanıklı zamanlayıcı](durable-functions-timers.md) gecikmeleri bir orkestratör işlevinin yürütme süresine dahil değildir. |
| Async API'leri | Orchestrator kodu, API veya nesnenin `IDurableOrchestrationContext` `context.df` API'sini kullanmadışında hiçbir zaman bir async işlemi başlatmamalıdır. Örneğin, .NET `Task.Run`veya `Task.Delay` `HttpClient.SendAsync` `setTimeout` `setInterval` JavaScript'te kullanamazsınız. Dayanıklı Görev Çerçevesi, tek bir iş parçacığı üzerinde orchestrator kodu çalışır. Diğer async API'ler tarafından çağrılabilecek diğer iş parçacıklarıyla etkileşim eleşemez. | Bir orkestratör işlevi yalnızca dayanıklı async aramaları yapmalıdır. Etkinlik işlevleri diğer async API aramaları yapmalıdır. |
| Async JavaScript işlevleri | JavaScript orchestrator işlevlerini `async` düğüm.js çalışma süresi asenkron işlevlerinin belirleyici olduğunu garanti etmediği nden dolayı bildiremezsiniz. | JavaScript orchestrator işlevlerini senkron jeneratör işlevleri olarak bildirin. |
| İş parçacığı API'leri | Dayanıklı Görev Çerçevesi, tek bir iş parçacığı üzerinde orchestrator kodu çalıştırıyor ve diğer iş parçacıklarıyla etkileşim kuramıyor. Bir orkestrasyonun yürütülmesine yeni iş parçacıkları getirilmesi, belirsiz yürütmeye veya kilitlenmelere neden olabilir. | Orchestrator işlevleri hemen hemen hiçbir zaman iş parçacığı API'leri kullanmamalıdır. Örneğin, .NET'te; `ConfigureAwait(continueOnCapturedContext: false)` bu, orkestratör işlevinin orijinalinde `SynchronizationContext`çalışan görev devamlarını sağlar. Bu tür API'ler gerekiyorsa, bunların kullanımını yalnızca etkinlik işlevleriyle sınırlayın. |
| Statik değişkenler | Orchestrator işlevlerinde sabit olmayan statik değişkenler kullanmaktan kaçının, çünkü değerleri zaman içinde değişebilir ve bu da belirsiz çalışma zamanı davranışına neden olabilir. | Sabitleri kullanın veya statik değişkenlerin kullanımını etkinlik işlevleriyle sınırlandırın. |
| Ortam değişkenleri | Orkestratör işlevlerinde ortam değişkenlerini kullanmayın. Değerleri zaman içinde değişebilir ve bu da belirsiz çalışma zamanı davranışıyla sonuçlanır. | Ortam değişkenleri yalnızca istemci işlevleri veya etkinlik işlevleri içinden başvurulmalıdır. |
| Sonsuz döngüler | Orkestratör işlevlerinde sonsuz döngülerden kaçının. Dayanıklı Görev Çerçevesi yürütme işlevinin ilerlemesiyle yürütme geçmişini kaydettiğinden, sonsuz döngü bir orkestratör örneğinin belleği tükenmesine neden olabilir. | Sonsuz döngü senaryoları için, `ContinueAsNew` işlev yürütmeyi `continueAsNew` yeniden başlatmak ve önceki yürütme geçmişini atmak için .NET veya JavaScript'teki gibi API'leri kullanın. |

Bu kısıtlamaları uygulamak ilk başta zor gibi görünse de, pratikte takip etmesi kolaydır.

Dayanıklı Görev Çerçevesi, önceki kuralların ihlallerini algılamaya çalışır. Bir ihlal bulursa, çerçeve bir **NonDeterministicOrchestrationException özel durum** atar. Ancak, bu algılama davranışı tüm ihlalleri yakalamaz ve buna bağlı olmamalıdır.

## <a name="versioning"></a>Sürüm Oluşturma

Dayanıklı bir orkestrasyon gün, ay, yıl, hatta [sonsuza kadar](durable-functions-eternal-orchestrations.md)sürekli çalışabilir. Bitmemiş orkestrasyonları etkileyen Dayanıklı İşlevler uygulamalarında yapılan tüm kod güncelleştirmeleri, orkestrasyonların yeniden oynatma davranışını bozabilir. Bu nedenle kod güncellemeleri yaparken dikkatli bir şekilde planlamak önemlidir. Kodunuzu nasıl sürüm edinsağlayacağınızı daha ayrıntılı bir açıklama için [sürüm makalesine](durable-functions-versioning.md)bakın.

## <a name="durable-tasks"></a>Dayanıklı görevler

> [!NOTE]
> Bu bölümde, Dayanıklı Görev Çerçevesi'nin iç uygulama ayrıntıları açıklanmaktadır. Bu bilgileri bilmeden dayanıklı işlevleri kullanabilirsiniz. Yalnızca yeniden oynatma davranışını anlamanıza yardımcı olmak için tasarlanmıştır.

Orkestratör işlevlerinde güvenle beklenebilen görevler bazen *dayanıklı görevler*olarak adlandırılır. Dayanıklı Görev Çerçevesi bu görevleri oluşturur ve yönetir. Örnekler **CallActivityAsync**tarafından döndürülen görevler , **WaitForExternalEvent**, ve **CreateTimer** .NET orchestrator fonksiyonları.

Bu dayanıklı görevler dahili olarak `TaskCompletionSource` .NET'teki nesnelerin listesi tarafından yönetilir. Yeniden oynatma sırasında, bu görevler orchestrator kodu yürütmenin bir parçası olarak oluşturulur. Sevk memuru ilgili tarih olaylarını sıralarken onlar da bitti.

Tüm geçmiş yeniden oynatıldıncaya kadar görevler tek bir iş parçacığı kullanılarak eşzamanlı olarak yürütülür. Geçmişin sonunda tamamlanamayan dayanıklı görevler, uygun eylemlericra edilmiştir. Örneğin, bir ileti bir etkinlik işlevini çağırmak için sıraya eklenmiş olabilir.

Bu bölümün çalışma zamanı davranışı açıklaması, bir orkestratör işlevinin neden `await` kullanılamadığını veya `yield` dayanılmaz bir görevde neden kullanılamadığını anlamanıza yardımcı olur. İki nedeni vardır: sevk iş parçacığı görevin tamamlanmasını bekleyemez ve bu görevin herhangi bir geri araması, orkestratör işlevinin izleme durumunu bozabilir. Bu ihlalleri algılamaya yardımcı olmak için bazı çalışma zamanı denetimleri yapılır.

Dayanıklı Görev Çerçevesi'nin orkestratör işlevlerini nasıl yürüttüğü hakkında daha fazla bilgi edinmek için [GitHub'daki Dayanıklı Görev kaynak koduna](https://github.com/Azure/durabletask)başvurun. Özellikle, [TaskOrchestrationExecutor.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationExecutor.cs) ve [TaskOrchestrationContext.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationContext.cs)bakın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Alt orkestrasyonları nasıl çağırılamayı öğrenin](durable-functions-sub-orchestrations.md)

> [!div class="nextstepaction"]
> [Sürüm le nasıl işleyeceğinizi öğrenin](durable-functions-versioning.md)
