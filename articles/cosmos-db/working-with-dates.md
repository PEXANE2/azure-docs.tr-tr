---
title: Azure Cosmos DB'de tarihlerle çalışma
description: Azure Cosmos DB 'de DataTime nesnelerini depolamayı, dizinlemeyi ve sorgulamayı öğrenin
ms.service: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.topic: conceptual
ms.date: 03/03/2020
ms.openlocfilehash: 92fa35fbe8e5eef4dbdc8b6c47a9055affd449a5
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78273196"
---
# <a name="working-with-dates-in-azure-cosmos-db"></a>Azure cosmos DB'de tarihlerle çalışma

Azure Cosmos DB, yerel bir [JSON](https://www.json.org) veri modeli aracılığıyla şema esnekliği ve zengin dizin oluşturma sağlar. Veritabanları, kapsayıcılar, belgeler ve saklı yordamlar da dahil olmak üzere tüm Azure Cosmos DB kaynaklarını modellenir ve JSON belgeleri olarak depolanır. Şu taşınabilir için bir gereklilik olarak JSON (ve Azure Cosmos DB) temel türleri yalnızca küçük bir kümesini destekler: dize, sayı, Boole, dizi, nesne ve Null. Ancak, JSON esnektir ve geliştiriciler ve çerçeveleri kullanarak bu temelleri ve nesne veya dizi oluşturmayı daha karmaşık türleri temsil etmek izin verin.

Temel türlerin yanı sıra, birçok uygulamanın tarih ve zaman damgalarını temsil etmesi için tarih saat türüne ihtiyacı vardır. Bu makalede nasıl geliştiriciler depolamak, alabilir ve .NET SDK kullanarak Azure Cosmos DB tarihleri sorgu açıklanır.

## <a name="storing-datetimes"></a>Tarih/saat depolama

Azure Cosmos DB dize, sayı, Boolean, null, dizi, nesne gibi JSON türlerini destekler. Bir tarih saat türünü doğrudan desteklemez. Şu anda Azure Cosmos DB tarihlerin yerelleştirmesini desteklemez. Bu nedenle, DateTimes 'ı dizeler olarak depolamanız gerekir. Azure Cosmos DB DateTime dizeleri için önerilen biçim ISO 8601 UTC standardını izleyen `YYYY-MM-DDThh:mm:ss.sssZ`. Tüm tarihleri UTC olarak Azure Cosmos DB depolamanız önerilir. Tarih dizelerini bu biçime dönüştürmek, zaman sözcüıgrafik tarihleri sıralamasına izin verir. UTC dışı tarihler depolanıyorsa, mantığın istemci tarafında işlenmesi gerekir. Yerel bir tarih/saati UTC 'ye dönüştürmek için, fark JSON içinde bir özellik olarak bilinmeli/depolanmalıdır ve istemci UTC Tarih saat değerini hesaplamak için de sapmayı kullanabilir.

Çoğu uygulama, aşağıdaki nedenlerden dolayı DateTime için varsayılan dize gösterimi kullanabilirsiniz:

* Dizeleri karşılaştırma ve dizelere dönüştürülür, DateTime değerlerini göreli sıralamasını korunur.
* Bu yaklaşım, herhangi bir özel kod veya öznitelikleri için JSON dönüştürme gerektirmez.
* JSON içinde depolanmış olarak tarihleri insan tarafından okunabilir.
* Bu yaklaşım, hızlı sorgu performansı için Azure Cosmos DB'nin dizini avantajlarından yararlanabilirsiniz.

Örneğin, aşağıdaki kod parçacığı, .NET SDK kullanarak bir belge olarak `ShipDate` ve `OrderDate` iki DateTime özelliği içeren bir `Order` nesnesi depolar:

```csharp
    public class Order
    {
        [JsonProperty(PropertyName="id")]
        public string Id { get; set; }
        public DateTime OrderDate { get; set; }
        public DateTime ShipDate { get; set; }
        public double Total { get; set; }
    }

    await container.CreateItemAsync(
        new Order
        {
            Id = "09152014101",
            OrderDate = DateTime.UtcNow.AddDays(-30),
            ShipDate = DateTime.UtcNow.AddDays(-14), 
            Total = 113.39
        });
```

Bu belge, Azure Cosmos DB'de aşağıdaki şekilde depolanır:

```json
    {
        "id": "09152014101",
        "OrderDate": "2014-09-15T23:14:25.7251173Z",
        "ShipDate": "2014-09-30T23:14:25.7251173Z",
        "Total": 113.39
    }
```  

Alternatif olarak, tarih/saat olarak Unix zaman damgası, diğer bir deyişle, 1 Ocak 1970'ten beri geçen saniyelerin sayısını gösteren bir sayı olarak depolayabilirsiniz. Azure Cosmos DB iç zaman damgası (`_ts`) özelliği bu yaklaşımı izler. [Unixdatetimeconverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) sınıfını, DateTimeS 'ı sayı olarak seri hale getirmek için kullanabilirsiniz.

## <a name="querying-datetimes-in-linq"></a>LINQ tarih saatleri sorgulama

LINQ ile Azure Cosmos DB'de depolanan verileri sorgulama, SQL .NET SDK'sını otomatik olarak destekler. Örneğin, aşağıdaki kod parçacığında son üç gün içinde gönderilen siparişlere filtre uygulayan bir LINQ sorgusu gösterilmektedir:

```csharp
    IQueryable<Order> orders = container.GetItemLinqQueryable<Order>(allowSynchronousQueryExecution: true).Where(o => o.ShipDate >= DateTime.UtcNow.AddDays(-3));
```

Aşağıdaki SQL bildirimine çevrilmiş ve Azure Cosmos DB yürütüldü:

```sql
    SELECT * FROM root WHERE (root["ShipDate"] >= "2016-12-18T21:55:03.45569Z")
```

Azure Cosmos DB SQL sorgu dili ve LINQ sağlayıcısı hakkında daha fazla bilgi için bkz. [LINQ 'da Cosmos DB sorgulama](sql-query-linq-to-sql.md).

## <a name="indexing-datetimes-for-range-queries"></a>Tarih saat aralık sorguları için dizin oluşturma

Sorgular DateTime değerleriyle ortaktır. Bu sorguları verimli bir şekilde yürütmek için sorgunun filtresindeki herhangi bir özelliklerde tanımlanmış bir dizininiz olmalıdır.

[Azure Cosmos DB Dizin oluşturma ilkelerinde](index-policy.md)dizin oluşturma ilkelerini yapılandırma hakkında daha fazla bilgi edinebilirsiniz. 

## <a name="next-steps"></a>Sonraki Adımlar

* [GitHub 'Da kod örneklerini](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples) indirme ve çalıştırma
* [SQL sorguları](sql-query-getting-started.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB Dizin oluşturma ilkeleri](index-policy.md) hakkında daha fazla bilgi edinin
