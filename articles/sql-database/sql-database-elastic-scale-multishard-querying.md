---
title: Parçalanmış veritabanlarını sorgula
description: Elastik veritabanı istemci kitaplığını kullanarak sorguları kırıklar arasında çalıştırın.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.date: 01/25/2019
ms.openlocfilehash: ae14a9fd8fc8479eac596fb694e12e3e0a9027f5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80067307"
---
# <a name="multi-shard-querying-using-elastic-database-tools"></a>Elastik veritabanı araçlarını kullanarak çok parçalı sorgulama

## <a name="overview"></a>Genel Bakış

[Elastik Veritabanı araçları ile,](sql-database-elastic-scale-introduction.md)kırık veritabanı çözümleri oluşturabilirsiniz. **Çok parçalı sorgulama,** veri toplama/raporlama gibi, birkaç parçaya yayılan bir sorguçalıştırmayı gerektiren görevler için kullanılır. (Bunu, tüm çalışmaları tek bir parça üzerinde gerçekleştiren [veri bağımlı yönlendirmeyle](sql-database-elastic-scale-data-dependent-routing.md)karşılaştırın.)

1. **TryGetRangeShardMap** [(Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager.trygetrangeshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.listshardmap-1)), TryGetRangeShardMap ( Java , [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap)), **TryGetListShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager.trygetlistshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap)) veya **GetShardMap** ( Java , [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap)) yöntemini kullanarak **RangeShardMap** [(Java](/java/api/com.microsoft.azure.elasticdb.shard.map.rangeshardmap), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.shardmanagement.rangeshardmap-1)) veya **ListShardMap** alın.[Java](/java/api/com.microsoft.azure.elasticdb.shard.map.listshardmap)[Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager.shardmapmanager.getshardmap) Bkz. [Bir ShardMapManager Oluşturma](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager) ve [RangeShardMap veya ListShardMap alın.](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap)
2. **MultiShardConnection** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard.multishardconnection), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection)) nesnesi oluşturun.
3. **MultiShardStatement veya MultiShardCommand** [(Java](/java/api/com.microsoft.azure.elasticdb.query.multishard.multishardstatement), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand)) oluşturun.
4. **CommandText özelliğini** [(Java](/java/api/com.microsoft.azure.elasticdb.query.multishard.multishardstatement), [.NET)](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand)T-SQL komutuna ayarlayın.
5. **ExecuteQueryAsync veya ExecuteReader** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard.multishardstatement.executeQueryAsync), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand)) yöntemini arayarak komutu çalıştırın.
6. **Sonuçları MultiShardResultSet veya MultiShardDataReader** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard.multishardresultset), [.NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader)) sınıfını kullanarak görüntüleyin.

## <a name="example"></a>Örnek

Aşağıdaki kod *myShardMap*adlı belirli bir **ShardMap** kullanarak çok parçalı sorgu kullanımını göstermektedir.

```csharp
using (MultiShardConnection conn = new MultiShardConnection(myShardMap.GetShards(), myShardConnectionString))
{
    using (MultiShardCommand cmd = conn.CreateCommand())
    {
        cmd.CommandText = "SELECT c1, c2, c3 FROM ShardedTable";
        cmd.CommandType = CommandType.Text;
        cmd.ExecutionOptions = MultiShardExecutionOptions.IncludeShardNameColumn;
        cmd.ExecutionPolicy = MultiShardExecutionPolicy.PartialResults;

        using (MultiShardDataReader sdr = cmd.ExecuteReader())
        {
            while (sdr.Read())
            {
                var c1Field = sdr.GetString(0);
                var c2Field = sdr.GetFieldValue<int>(1);
                var c3Field = sdr.GetFieldValue<Int64>(2);
            }
        }
    }
}
```

Önemli bir fark çok parçalı bağlantıların inşasıdır. **SqlConnection'ın** tek bir veritabanında çalıştığı durumlarda, **MultiShardConnection** girdiolarak ***bir parça koleksiyonu*** alır. Parça bir haritadan parçaların toplanmasını kalabalıkbir şekilde doldurun. Sorgu daha sonra tek bir genel sonucu birleştirmek için **UNION ALL** semantik kullanarak parçaların koleksiyonunda yürütülür. İsteğe bağlı olarak, satırın kaynağı olan parçanın adı komut üzerindeki **Yürütme Seçenekleri** özelliği kullanılarak çıktıya eklenebilir.

**myShardMap.GetShards()** için arama unutmayın. Bu yöntem, parça eşlemi tüm parçaları alır ve ilgili tüm veritabanları arasında bir sorgu çalıştırmak için kolay bir yol sağlar. Çok parçalı bir sorgu için parçaların toplanması, **myShardMap.GetShards()** çağrısından döndürülen koleksiyon üzerinden bir LINQ sorgusu gerçekleştirerek daha da rafine edilebilir. Kısmi sonuç ilkesiyle birlikte, çok parçalı sorgulamadaki geçerli yetenek, yüzlerce parçaya kadar on ları için iyi çalışacak şekilde tasarlanmıştır.

Çok parçalı sorguile sınırlama şu anda sorgulanır kırıklar ve kırıklar için doğrulama eksikliğidir. Veri bağımlı yönlendirme, belirli bir parçanın sorgu sırasında parçalama eşleminin bir parçası olduğunu doğrularken, çok parçalı sorgular bu denetimi gerçekleştirmez. Bu, parça eşlemi kaldırılmış veritabanlarında çalışan çok parçalı sorgulara neden olabilir.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Çok parçalı sorgular ve bölme birleştirme işlemleri

Çok parçalı sorgular, sorgulanmış veritabanındaki parçaların devam eden bölme birleştirme işlemlerine katılıp katılmadığını doğrulamaz. [(Bkz. Elastik Veritabanı bölme birleştirme aracını kullanarak ölçekleme](sql-database-elastic-scale-overview-split-and-merge.md).) Bu, aynı çok parçalı sorguda birden çok veritabanları için aynı shardlet satırları nın gösterdiği tutarsızlıklara neden olabilir. Bu sınırlamalara dikkat edin ve çok parçalı sorgular gerçekleştirirken devam eden bölme birleştirme işlemlerini ve parça haritasındaki değişiklikleri boşaltmayı düşünün.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]