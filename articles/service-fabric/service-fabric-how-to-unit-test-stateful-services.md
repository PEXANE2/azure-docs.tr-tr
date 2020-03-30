---
title: Devlet hizmetleri için birim testleri geliştirme
description: Devlet hizmetleri için Azure Hizmet Kumaşı'ndaki birim testi ve geliştirme sırasında göz önünde bulundurulması gereken özel hususlar hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 09/04/2018
ms.openlocfilehash: 9c657bd8295d01a4e0fa4e44e969b33946684bfa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75639845"
---
# <a name="create-unit-tests-for-stateful-services"></a>Devlet Hizmetleri için birim testleri oluşturma
Birim testi Hizmet Kumaş stateful hizmetleri mutlaka geleneksel uygulama veya etki alanına özgü birim testi tarafından yakalanmış olmaz yaygın hataları ortaya çıkarır. Devlet hizmetleri için birim testleri geliştirirken, akılda tutulması gereken bazı özel hususlar vardır.

1. Her yineleme uygulama kodunu ancak farklı bağlam altında yürütür. Hizmet üç yineleme kullanıyorsa, hizmet kodu farklı bağlam/rol altında paralel olarak üç düğüm üzerinde yürütülür.
2. Devlet hizmeti içinde depolanan devlet tüm yinelemeler arasında tutarlı olmalıdır. Devlet yöneticisi ve güvenilir koleksiyonlar bu tutarlılığı kutunun dışında sağlayacaktır. Ancak, bellek durumu uygulama kodu tarafından yönetilmesi gerekir.
3. Her yineleme küme üzerinde çalışırken bir noktada rolleri değiştirir. Birincil barındırma düğümü kullanılamaz veya aşırı olur durumda, ikincil bir yineleme birincil olur. Bu Hizmet Kumaş ı için doğal bir davranıştır, bu nedenle hizmetler sonunda farklı bir rol altında yürütülmesi için planlamalıdır.

Bu makalede, [Hizmet Kumaşı'ndaki Birim test hizmeti nin](service-fabric-concepts-unit-testing.md) okunduğu varsayar.

## <a name="the-servicefabricmocks-library"></a>ServiceFabric.Mocks kütüphane
Sürüm 3.3.0 itibariyle [ServiceFabric.Mocks,](https://www.nuget.org/packages/ServiceFabric.Mocks/) hem kopyaların orkestrasyonhem de devlet yönetimiyle alay etmek için bir API sağlar. Bu örneklerde kullanılacaktır.

[Nuget](https://www.nuget.org/packages/ServiceFabric.Mocks/)
[GitHub](https://github.com/loekd/ServiceFabric.Mocks)

*ServiceFabric.Mocks Microsoft'a ait değildir veya Microsoft tarafından korunmaz. Ancak, bu şu anda birim devlet hizmetleri sınama için Microsoft önerilen kitaplık.*

## <a name="set-up-the-mock-orchestration-and-state"></a>Sahte orkestrasyon ve devlet kurmak
Bir testin düzenleme bölümünün bir parçası olarak, sahte bir yineleme kümesi ve durum yöneticisi oluşturulur. Yineleme kümesi daha sonra her yineleme için sınanan hizmetin bir örneğini oluştururken sahip olur. Ayrıca gibi `OnChangeRole` ve yürütme yaşam döngüsü `RunAsync`olaylara sahip olacak. Sahte devlet müdürü, devlet müdürü nezdinde gerçekleştirilen her türlü operasyonun gerçek devlet müdürünün yapacağı gibi yürütülmesini ve tutulmasını sağlayacaktır.

1. Test edilen hizmeti anında sağlayacak bir hizmet fabrikası temsilcisi oluşturun. Bu, genellikle bir Service Fabric hizmeti veya aktörü `Program.cs` için bulunan servis fabrikası geri araması ile aynı veya aynı olmalıdır. Bu, aşağıdaki imzayı izlemelidir:
   ```csharp
   MyStatefulService CreateMyStatefulService(StatefulServiceContext context, IReliableStateManagerReplica2 stateManager)
   ```
2. Sınıfın bir `MockReliableStateManager` örneğini oluşturun. Bu, devlet müdürü ile tüm etkileşimleri alay edecek.
3. Sınanan `MockStatefulServiceReplicaSet<TStatefulService>` hizmetin türü nerede `TStatefulService` olduğuna dair bir örnek oluşturun. Bunun için adım #1 oluşturulan temsilci ve eyalet yöneticisi nin #2
4. Yineleme Kümesine Yinelemeler ekleyin. Rolü (Birincil, ActiveSecondary, IdleSecondary gibi) ve yinelemenin kimliğini belirtin
   > Çoğaltma lı titretilenini tutun! Bunlar büyük olasılıkla hareket sırasında kullanılacak ve bir birim testinin bölümlerini ileri serecek.

```csharp
//service factory to instruct how to create the service instance
var serviceFactory = (StatefulServiceContext context, IReliableStateManagerReplica2 stateManager) => new MyStatefulService(context, stateManager);
//instantiate a new mock state manager
var stateManager = new MockReliableStateManager();
//instantiate a new replica set with the service factory and state manager
var replicaSet = new MockStatefulServiceReplicaSet<MyStatefulService>(CreateStatefulService, stateManager);
//add a new Primary replica with id 1
await replicaSet.AddReplicaAsync(ReplicaRole.Primary, 1);
//add a new ActiveSecondary replica with id 2
await replicaSet.AddReplicaAsync(ReplicaRole.ActiveSecondary, 2);
//add a second ActiveSecondary replica with id 3
await replicaSet.AddReplicaAsync(ReplicaRole.ActiveSecondary, 3);
```

## <a name="execute-service-requests"></a>Hizmet isteklerini yürütme
Hizmet istekleri, kolaylık özellikleri ve aramaları kullanılarak belirli bir yinelemede yürütülebilir.
```csharp
const string stateName = "test";
var payload = new Payload(StatePayload);

//execute a request on the primary replica using
await replicaSet.Primary.ServiceInstance.InsertAsync(stateName, payload);

//execute a request against replica with id 2
await replicaSet[2].ServiceInstance.InsertAsync(stateName, payload);

//execute a request against one of the active secondary replicas
await replicaSet.FirstActiveSecondary.InsertAsync(stateName, payload);
```

## <a name="execute-a-service-move"></a>Hizmet hareketini yürütme
Sahte çoğaltma kümesi, farklı hizmet hareketlerini tetiklemek için çeşitli kolaylık yöntemlerini ortaya çıkarır.
```csharp
//promote the first active secondary to primary
replicaSet.PromoteNewReplicaToPrimaryAsync();
//promote the secondary with replica id 4 to primary
replicaSet.PromoteNewReplicaToPrimaryAsync(4);

//promote the first idle secondary to an active secondary
PromoteIdleSecondaryToActiveSecondaryAsync();
//promote idle secondary with replica id 4 to active secondary
PromoteIdleSecondaryToActiveSecondaryAsync(4);

//add a new replica with randomly assigned replica id and promote it to primary
PromoteNewReplicaToPrimaryAsync()
//add a new replica with replica id 4 and promote it to primary
PromoteNewReplicaToPrimaryAsync(4)
```

## <a name="putting-it-all-together"></a>Hepsini bir araya getirme
Aşağıdaki test, üç düğüm yineleme kümesi ayarlama ve veri bir rol değişikliği sonra ikincil kullanılabilir olup olmadığını doğrulayan gösterir. Bu, bu sorunun yakalayabileceği tipik `InsertAsync` bir sorun, sırasında eklenen verilerin bellekte `CommitAsync`bir şeye veya çalışmadan güvenilir bir koleksiyona kaydedildiğidir. Her iki durumda da, ikincil birincil ile senkronize olacaktır. Bu, hizmet hareketettikten sonra tutarsız yanıtlara yol açar.

```csharp
[TestMethod]
public async Task TestServiceState_InMemoryState_PromoteActiveSecondary()
{
    var stateManager = new MockReliableStateManager();
    var replicaSet = new MockStatefulServiceReplicaSet<MyStatefulService>(CreateStatefulService, stateManager);
    await replicaSet.AddReplicaAsync(ReplicaRole.Primary, 1);
    await replicaSet.AddReplicaAsync(ReplicaRole.ActiveSecondary, 2);
    await replicaSet.AddReplicaAsync(ReplicaRole.ActiveSecondary, 3);

    const string stateName = "test";
    var payload = new Payload(StatePayload);

    //insert data
    await replicaSet.Primary.ServiceInstance.InsertAsync(stateName, payload);
    //promote one of the secondaries to primary
    await replicaSet.PromoteActiveSecondaryToPrimaryAsync(2);
    //get data
    var payloads = (await replicaSet.Primary.ServiceInstance.GetPayloadsAsync()).ToList();

    //data should match what was inserted against the primary
    Assert.IsTrue(payloads.Count == 1);
    Assert.IsTrue(payloads[0].Content == payload.Content);

    //verify the data was saved against the reliable dictionary
    var dictionary = await StateManager.GetOrAddAsync<IReliableDictionary<string, Payload>>(MyStatefulService.StateManagerDictionaryKey);
    using(var tx = StateManager.CreateTransaction())
    {
        var payload = await dictionary.TryGetValue(stateName);
        Assert.IsTrue(payload.HasValue);
        Assert.IsTrue(payload.Value.Content == payload.Content);
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
[Hizmet-servis iletişimini](service-fabric-testability-scenarios-service-communication.md) nasıl test edebilirsiniz ve [kontrollü kaosu kullanarak hataları nasıl simüle edebilirsiniz.](service-fabric-controlled-chaos.md)
