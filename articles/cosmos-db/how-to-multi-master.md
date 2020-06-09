---
title: Azure Cosmos DB çoklu yönetici yapılandırma
description: Azure Cosmos DB 'de farklı SDK 'Ları kullanarak uygulamalarınız için çoklu yönetici yapılandırma hakkında bilgi edinin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: mjbrown
ms.custom: tracking-python
ms.openlocfilehash: 62dedd4cf91143ee7b31b92880135ac6c7953ef9
ms.sourcegitcommit: 964af22b530263bb17fff94fd859321d37745d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84561238"
---
# <a name="configure-multi-master-in-your-applications-that-use-azure-cosmos-db"></a>Azure Cosmos DB kullanan uygulamalarınızda çoklu yönetici yapılandırma

Birden fazla yazma bölgesi etkinken bir hesap oluşturulduktan sonra, Azure Cosmos DB ' de çok yöneticili ve çok girişli özellikleri etkinleştirmek üzere, uygulamanızda, DocumentClient için ConnectionPolicy için iki değişiklik yapmalısınız. ConnectionPolicy içinde UseMultipleWriteLocations öğesini true olarak ayarlayın ve uygulamanın SetCurrentLocation 'a dağıtıldığı bölgenin adını geçirin. Bu işlem, PreferredLocations özelliğini geçirilen konumdan coğrafi yakınlık temelinde doldurur. Yeni bir bölge daha sonra hesaba eklenirse, uygulamanın güncellenmesi veya yeniden dağıtılması gerekmez, daha yakın bölgeyi otomatik olarak algılar ve bölgesel bir olay oluşması gerekir.

> [!Note]
> Başlangıçta tek bir yazma bölgesiyle yapılandırılan Cosmos hesapları, sıfır aşağı doğru bir şekilde birden fazla yazma bölgesine (yani çok yöneticili) yapılandırılabilir. Daha fazla bilgi edinmek için bkz. [birden çok yazma bölgelerini yapılandırma](how-to-manage-database-account.md#configure-multiple-write-regions)

## <a name="net-sdk-v2"></a><a id="netv2"></a>.NET SDK v2

Uygulamanızda çoklu yönetici özelliğini etkinleştirmek için `UseMultipleWriteLocations` olarak ayarlayın `true` . Ayrıca, `SetCurrentLocation` uygulamanın dağıtıldığı bölgeye ve Azure Cosmos DB nerede çoğaltıldığına ayarlanır:

```csharp
ConnectionPolicy policy = new ConnectionPolicy
    {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true
    };
policy.SetCurrentLocation("West US 2");
```

## <a name="net-sdk-v3"></a><a id="netv3"></a>.NET SDK V3

Uygulamanızda çoklu yönetici özelliğini etkinleştirmek için, `ApplicationRegion` uygulamanın dağıtıldığı bölgeye ve Cosmos DB nerede çoğaltılacağı konuma ayarlayın:

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

Uygulamanızda çoklu yönetici özelliğini etkinleştirmek için, ayarlayın `policy.setUsingMultipleWriteLocations(true)` ve `policy.setPreferredLocations` uygulamanın dağıtıldığı bölgeyi ve Cosmos DB nerede çoğaltıldığından ayarlayın:

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

## <a name="nodejs-javascript-and-typescript-sdks"></a><a id="javascript"></a>Node. js, JavaScript ve TypeScript SDK 'Ları

Uygulamanızda çoklu yönetici özelliğini etkinleştirmek için `connectionPolicy.UseMultipleWriteLocations` olarak ayarlayın `true` . Ayrıca, `connectionPolicy.PreferredLocations` uygulamanın dağıtıldığı bölgeye ve Cosmos DB nerede çoğaltıldığına ayarlanır:

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

Uygulamanızda çoklu yönetici özelliğini etkinleştirmek için `connection_policy.UseMultipleWriteLocations` olarak ayarlayın `true` . Ayrıca, `connection_policy.PreferredLocations` uygulamanın dağıtıldığı bölgeye ve Cosmos DB nerede çoğaltıldığına ayarlanır.

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
* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans avantajları](consistency-levels-tradeoffs.md)
* [Sağlanan aktarım hızını küresel olarak ölçeklendirme](scaling-throughput.md)
* [Genel dağıtım: devlet](global-dist-under-the-hood.md)
