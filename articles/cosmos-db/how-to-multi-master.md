---
title: Azure Cosmos DB'de çoklu master nasıl yapılandırılabilen
description: Azure Cosmos DB'de farklı SDK'lar kullanarak uygulamalarınız için çok yapılı yapılandırmayı öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: mjbrown
ms.openlocfilehash: 654baed649093add2aa62f4ba81bf6ce7c3e0df5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74873650"
---
# <a name="configure-multi-master-in-your-applications-that-use-azure-cosmos-db"></a>Azure Cosmos DB kullanan uygulamalarınızda çoklu master yapılandırın

Birden çok yazma bölgesi etkinleştirilmiş bir hesap oluşturulduktan sonra, Azure Cosmos DB'deki çok açılı ve çoklu homing özelliklerini etkinleştirmek için DocumentClient için ConnectionPolicy'ye yaptığınız uygulamada iki değişiklik yapmalısınız. ConnectionPolicy içinde, UseMultipleWriteLocations'ı doğru olarak ayarlayın ve uygulamanın SetCurrentLocation'a dağıtıldığı bölgenin adını geçirin. Bu, geçirilen konumdan coğrafi yakınlığı temel alan PreferredLocations özelliğini dolduracaktır. Hesaba daha sonra yeni bir bölge eklenirse, uygulamanın güncelleştirilmesi veya yeniden dağıtılması gerekmez, yakın bölgeyi otomatik olarak algılar ve bölgesel bir olay meydana geldiğinde otomatik olarak bu bölgeye otomatik olarak yerleştirilir.

> [!Note]
> Başlangıçta tek yazma bölgesiyle yapılandırılan Cosmos hesapları, sıfır kapalı kalma süresiyle birden çok yazma bölgesine (örn. çok analık) yapılandırılabilir. Daha fazla bilgi edinmek [için, birden çok yazma bölgesini yapılandırma](how-to-manage-database-account.md#configure-multiple-write-regions)

## <a name="net-sdk-v2"></a><a id="netv2"></a>.NET SDK v2

Uygulamanızda çoklu master'ı etkinleştirmek `true`için, '' olarak ayarlayın. `UseMultipleWriteLocations` Ayrıca, `SetCurrentLocation` uygulamanın dağıtıldığı ve Azure Cosmos DB'nin çoğaltıldığı bölgeye de ayarlanır:

```csharp
ConnectionPolicy policy = new ConnectionPolicy
    {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true
    };
policy.SetCurrentLocation("West US 2");
```

## <a name="net-sdk-v3"></a><a id="netv3"></a>.NET SDK v3

Uygulamanızda çoklu master sağlamak için, uygulamanın dağıtıldığı ve Cosmos DB'nin çoğaltıldığı bölgeye ayarlayın: `ApplicationRegion`

```csharp
CosmosClient cosmosClient = new CosmosClient(
    "<connection-string-from-portal>", 
    new CosmosClientOptions()
    {
        ApplicationRegion = Regions.WestUS2,
    });
```

İsteğe bağlı olarak, `CosmosClientBuilder` `WithApplicationRegion` aynı sonucu elde etmek için ve kullanabilirsiniz:

```csharp
CosmosClientBuilder cosmosClientBuilder = new CosmosClientBuilder("<connection-string-from-portal>")
            .WithApplicationRegion(Regions.WestUS2);
CosmosClient client = cosmosClientBuilder.Build();
```

## <a name="java-async-sdk"></a><a id="java"></a>Java Async SDK’sı

Uygulamanızda çoklu master sağlamak için, uygulamanın dağıtıldığı ve Cosmos DB'nin çoğaltıldığı bölgeye ayarlayın `policy.setUsingMultipleWriteLocations(true)` ve ayarlayın: `policy.setPreferredLocations`

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setUsingMultipleWriteLocations(true);
policy.setPreferredLocations(Collections.singletonList(region));

AsyncDocumentClient client =
    new AsyncDocumentClient.Builder()
        .withMasterKeyOrResourceToken(this.accountKey)
        .withServiceEndpoint(this.accountEndpoint)
        .withConsistencyLevel(ConsistencyLevel.Eventual)
        .withConnectionPolicy(policy).build();
```

## <a name="nodejs-javascript-and-typescript-sdks"></a><a id="javascript"></a>Düğüm.js, JavaScript ve TypeScript SDK'lar

Uygulamanızda çoklu master'ı etkinleştirmek `true`için, '' olarak ayarlayın. `connectionPolicy.UseMultipleWriteLocations` Ayrıca, `connectionPolicy.PreferredLocations` uygulamanın dağıtıldığı ve Cosmos DB'nin çoğaltıldığı bölgeye de ayarlanır:

```javascript
const connectionPolicy: ConnectionPolicy = new ConnectionPolicy();
connectionPolicy.UseMultipleWriteLocations = true;
connectionPolicy.PreferredLocations = [region];

const client = new CosmosClient({
  endpoint: config.endpoint,
  auth: { masterKey: config.key },
  connectionPolicy,
  consistencyLevel: ConsistencyLevel.Eventual
});
```

## <a name="python-sdk"></a><a id="python"></a>Python SDK'sı

Uygulamanızda çoklu master'ı etkinleştirmek `true`için, '' olarak ayarlayın. `connection_policy.UseMultipleWriteLocations` Ayrıca, `connection_policy.PreferredLocations` uygulamanın dağıtıldığı ve Cosmos DB'nin çoğaltıldığı bölgeye ayarlayın.

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.UseMultipleWriteLocations = True
connection_policy.PreferredLocations = [region]

client = cosmos_client.CosmosClient(self.account_endpoint, {
                                    'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Session)
```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleleri okuyun:

* [Azure Cosmos DB'de tutarlılığı yönetmek için oturum belirteçlerini kullanma](how-to-manage-consistency.md#utilize-session-tokens)
* [Azure Cosmos DB'de çakışma türleri ve çözüm ilkeleri](conflict-resolution-policies.md)
* [Azure Cosmos DB'de yüksek kullanılabilirlik](high-availability.md)
* [Azure Cosmos DB'de tutarlılık düzeyleri](consistency-levels.md)
* [Azure Cosmos DB'de doğru tutarlılık düzeyini seçin](consistency-levels-choosing.md)
* [Azure Cosmos DB'de tutarlılık, kullanılabilirlik ve performans dengeleri](consistency-levels-tradeoffs.md)
* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans dengeleri](consistency-levels-tradeoffs.md)
* [Sağlanan aktarım hızını küresel olarak ölçeklendirme](scaling-throughput.md)
* [Küresel dağılım: Başlık altında](global-dist-under-the-hood.md)
