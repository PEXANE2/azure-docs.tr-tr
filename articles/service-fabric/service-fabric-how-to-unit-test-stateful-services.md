---
title: Durum bilgisi olan hizmetler için birim testleri geliştirme
description: Durum bilgisi olan hizmetler için Azure Service Fabric birim testi ve geliştirme sırasında göz önünde bulundurmanız gereken özel noktalar hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 09/04/2018
ms.openlocfilehash: 9c657bd8295d01a4e0fa4e44e969b33946684bfa
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75639845"
---
# <a name="create-unit-tests-for-stateful-services"></a>Durum bilgisi olan hizmetler için birim testleri oluşturma
Durum bilgisi olan hizmetler Service Fabric birim testi, geleneksel uygulama veya etki alanına özgü birim testi tarafından yakalanmak zorunda olmayan yaygın hataları açığa kaldırır. Durum bilgisi olan hizmetler için birim testleri geliştirirken göz önünde tutulması gereken bazı özel noktalar vardır.

1. Her çoğaltma uygulama kodunu yürütür, ancak farklı bir bağlam altında. Hizmet üç çoğaltma kullanıyorsa, hizmet kodu, farklı bağlam/rol altında paralel olarak üç düğüm üzerinde yürütüyordur.
2. Durum bilgisi olan hizmet içinde depolanan durum tüm çoğaltmalar arasında tutarlı olmalıdır. Durum Yöneticisi ve güvenilir koleksiyonlar, bu tutarlılığı kullanıma hazır olarak sağlayacaktır. Ancak, bellek içi durumunun uygulama kodu tarafından yönetilmesi gerekir.
3. Her çoğaltma, küme üzerinde çalışırken bazı bir noktada rolleri değiştirecek. Birincil bir çoğaltma, birincili barındıran düğümün kullanılamaz veya aşırı yüklü hale gelmesini durumunda birincil olur. Bu, Service Fabric için doğal bir davranıştır ve bu nedenle hizmetler, sonunda farklı bir rol altında yürütmeyi planlıyor olmalıdır.

Bu makalede, [Service Fabric ' de durum bilgisi olmayan hizmetlerin](service-fabric-concepts-unit-testing.md) okunduğunu olduğu varsayılır.

## <a name="the-servicefabricmocks-library"></a>ServiceFabric. Moaltını kitaplığı
Sürüm 3.3.0 itibariyle [Servicefabric. Moze](https://www.nuget.org/packages/ServiceFabric.Mocks/) , çoğaltmaları ve durum yönetimini düzenleme IÇIN bir API sağlar. Bu örneklerde kullanılacaktır.

[NuGet](https://www.nuget.org/packages/ServiceFabric.Mocks/) 
 [GitHub](https://github.com/loekd/ServiceFabric.Mocks)

*ServiceFabric. Moaltını, Microsoft tarafından sahip değil veya korunmaz. Ancak, şu anda birim testi durum bilgisi olmayan hizmetler için Microsoft tarafından önerilen bir kitaplıktır.*

## <a name="set-up-the-mock-orchestration-and-state"></a>Sahte düzenleme ve durumunu ayarlama
Bir testin düzenleme bölümünün parçası olarak, bir sahte çoğaltma kümesi ve durum Yöneticisi oluşturulur. Çoğaltma kümesi daha sonra, her çoğaltma için Sınanan hizmetin bir örneğini oluşturur. Ayrıca, ve gibi yaşam döngüsü olaylarını yürütmektedir `OnChangeRole` `RunAsync` . Sahte durum Yöneticisi, durum yöneticisine karşı gerçekleştirilen tüm işlemlerin çalışır durumda olduğundan emin olur ve gerçek durum Yöneticisi olarak saklanır.

1. Sınanan hizmeti örneklendirilecek bir Service Factory temsilcisi oluşturun. Bu, genellikle `Program.cs` bir Service Fabric hizmeti veya aktör için içinde bulunan hizmet fabrikası geri çağırması ile benzerdir veya aynı olmalıdır. Bu, aşağıdaki imzayı izlemelidir:
   ```csharp
   MyStatefulService CreateMyStatefulService(StatefulServiceContext context, IReliableStateManagerReplica2 stateManager)
   ```
2. Sınıfının bir örneğini oluşturun `MockReliableStateManager` . Bu, tüm etkileşimleri durum yöneticisiyle birlikte kullanacaktır.
3. Öğesinin bir örneğini `MockStatefulServiceReplicaSet<TStatefulService>` `TStatefulService` , test edilmekte olan hizmetin türünün bir örneğini oluşturun. Bu, #1 adımında oluşturulan temsilcinin ve durum Yöneticisi 'nin #2 ' de oluşturulmasını gerektirir
4. Çoğaltma kümesine çoğaltmalar ekleyin. Rolü (örneğin, birincil, ActiveSecondary, ıdde Ikincil) ve çoğaltmanın KIMLIĞINI belirtin
   > Çoğaltma kimliklerinde tutun! Bu, büyük olasılıkla bir birim testinin hareket ve onaylama bölümlerinde kullanılır.

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

## <a name="execute-service-requests"></a>Hizmet isteklerini Yürüt
Hizmet istekleri, kullanışlı özellikler ve aramalar kullanılarak belirli bir çoğaltmada yürütülebilir.
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

## <a name="execute-a-service-move"></a>Hizmet taşımayı yürütme
Sahte çoğaltma kümesi, farklı türlerde hizmet hareketlerinin tetiklenmesi için çeşitli kullanışlı yöntemler sunar.
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
Aşağıdaki test, üç düğümlü bir çoğaltma kümesi ayarlamayı ve bir rol değişikliğinden sonra verilerin ikincil tarafından kullanılabilir olduğunu doğrulamayı gösterir. Normal bir sorun, sırasında eklenen verilerin `InsertAsync` belleğe veya güvenilir bir koleksiyona çalıştırmadan bir şeye kaydedilmesidir `CommitAsync` . Her iki durumda da ikincil, birincil ile eşitlenmemiş olur. Bu, hizmet taşındıktan sonra tutarsız yanıtlara yol açabilir.

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
[Hizmetten hizmete iletişimi](service-fabric-testability-scenarios-service-communication.md) test etmeyi ve [denetlenen Chaos kullanarak hataların benzetimini](service-fabric-controlled-chaos.md)yapmayı öğrenin.
