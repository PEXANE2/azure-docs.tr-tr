---
title: Azure Cosmos DB çoklu yönetici yapılandırma
description: Azure Cosmos DB 'de farklı SDK 'Ları kullanarak uygulamalarınız için çoklu yönetici yapılandırma hakkında bilgi edinin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: mjbrown
ms.openlocfilehash: 654baed649093add2aa62f4ba81bf6ce7c3e0df5
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74873650"
---
# <a name="configure-multi-master-in-your-applications-that-use-azure-cosmos-db"></a>Azure Cosmos DB kullanan uygulamalarınızda çoklu yönetici yapılandırma

Birden fazla yazma bölgesi etkinken bir hesap oluşturulduktan sonra, Azure Cosmos DB ' de çok yöneticili ve çok girişli özellikleri etkinleştirmek üzere, uygulamanızda, DocumentClient için ConnectionPolicy için iki değişiklik yapmalısınız. ConnectionPolicy içinde UseMultipleWriteLocations öğesini true olarak ayarlayın ve uygulamanın SetCurrentLocation 'a dağıtıldığı bölgenin adını geçirin. Bu işlem, PreferredLocations özelliğini geçirilen konumdan coğrafi yakınlık temelinde doldurur. Yeni bir bölge daha sonra hesaba eklenirse, uygulamanın güncellenmesi veya yeniden dağıtılması gerekmez, daha yakın bölgeyi otomatik olarak algılar ve bölgesel bir olay oluşması gerekir.

> [!Note]
> Başlangıçta tek bir yazma bölgesiyle yapılandırılan Cosmos hesapları, sıfır aşağı doğru bir şekilde birden fazla yazma bölgesine (yani çok yöneticili) yapılandırılabilir. Daha fazla bilgi edinmek için bkz. [birden çok yazma bölgelerini yapılandırma](how-to-manage-database-account.md#configure-multiple-write-regions)

## <a id="netv2"></a>.NET SDK v2

Uygulamanızda çoklu yönetici özelliğini etkinleştirmek için `UseMultipleWriteLocations` `true`ayarlayın. Ayrıca, `SetCurrentLocation` uygulamanın dağıtıldığı bölgeye ve Azure Cosmos DB nerede çoğaltıldığından ayarlayın:

```csharp
ConnectionPolicy policy = new ConnectionPolicy
    {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true
    };
policy.SetCurrentLocation("West US 2");
```

## <a id="netv3"></a>.NET SDK V3

Uygulamanızda çoklu yönetici özelliğini etkinleştirmek için `ApplicationRegion` uygulamanın dağıtıldığı bölgeye ve Cosmos DB nerede çoğaltıldığından ayarlayın:

```csharp
CosmosClient cosmosClient = new CosmosClient(
    "<connection-string-from-portal>", 
    new CosmosClientOptions()
    {
        ApplicationRegion = Regions.WestUS2,
    });
```

İsteğe bağlı olarak, `CosmosClientBuilder` ve `WithApplicationRegion` kullanarak aynı sonucu elde edebilirsiniz:

```csharp
CosmosClientBuilder cosmosClientBuilder = new CosmosClientBuilder("<connection-string-from-portal>")
            .WithApplicationRegion(Regions.WestUS2);
CosmosClient client = cosmosClientBuilder.Build();
```

## <a id="java"></a>Java Async SDK’sı

Uygulamanızda çoklu yönetici özelliğini etkinleştirmek için `policy.setUsingMultipleWriteLocations(true)` ayarlayın ve `policy.setPreferredLocations` uygulamanın dağıtıldığı bölgeye ve Cosmos DB nerede çoğaltılacağı konuma ayarlayın:

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

## <a id="javascript"></a>Node. js, JavaScript ve TypeScript SDK 'Ları

Uygulamanızda çoklu yönetici özelliğini etkinleştirmek için `connectionPolicy.UseMultipleWriteLocations` `true`ayarlayın. Ayrıca, `connectionPolicy.PreferredLocations` uygulamanın dağıtıldığı bölgeye ve Cosmos DB nerede çoğaltıldığından ayarlayın:

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

## <a id="python"></a>Python SDK’sı

Uygulamanızda çoklu yönetici özelliğini etkinleştirmek için `connection_policy.UseMultipleWriteLocations` `true`ayarlayın. Ayrıca, `connection_policy.PreferredLocations` uygulamanın dağıtıldığı bölgeye ve Cosmos DB nerede çoğaltıldığından ayarlayın.

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.UseMultipleWriteLocations = True
connection_policy.PreferredLocations = [region]

client = cosmos_client.CosmosClient(self.account_endpoint, {
                                    'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Session)
```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makaleleri okuyun:

* [Azure Cosmos DB tutarlılığı yönetmek için oturum belirteçlerini kullanın](how-to-manage-consistency.md#utilize-session-tokens)
* [Azure Cosmos DB çakışma türleri ve çözüm ilkeleri](conflict-resolution-policies.md)
* [Azure Cosmos DB yüksek kullanılabilirlik](high-availability.md)
* [Azure Cosmos DB 'deki tutarlılık düzeyleri](consistency-levels.md)
* [Azure Cosmos DB doğru tutarlılık düzeyini seçin](consistency-levels-choosing.md)
* [Azure Cosmos DB tutarlılık, kullanılabilirlik ve performans avantajları](consistency-levels-tradeoffs.md)
* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri](consistency-levels-tradeoffs.md)
* [Sağlanan aktarım hızı genel olarak ölçeklendiriliyor](scaling-throughput.md)
* [Genel dağıtım: devlet](global-dist-under-the-hood.md)
