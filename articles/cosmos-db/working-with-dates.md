---
title: Azure Cosmos DB'de tarihlerle çalışma
description: Azure Cosmos DB'de DataTime nesnelerini nasıl depolayacak, dizine ekleyip sorgulayın
ms.service: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.topic: conceptual
ms.date: 03/03/2020
ms.openlocfilehash: 92fa35fbe8e5eef4dbdc8b6c47a9055affd449a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273196"
---
# <a name="working-with-dates-in-azure-cosmos-db"></a>Azure Cosmos DB'de Tarihlerle Çalışma

Azure Cosmos DB, yerel [bir JSON](https://www.json.org) veri modeli aracılığıyla şema esnekliği ve zengin dizin oluşturma sağlar. Veritabanları, kapsayıcılar, belgeler ve depolanan yordamlar da dahil olmak üzere tüm Azure Cosmos DB kaynakları modellenir ve JSON belgeleri olarak depolanır. Taşınabilir olmanın bir gereksinimi olarak, JSON (ve Azure Cosmos DB) yalnızca küçük bir temel türü kümesini destekler: String, Number, Boolean, Array, Object ve Null. Ancak, JSON esnek ve geliştiriciler ve çerçeveler bu ilkel kullanarak daha karmaşık türleri temsil etmek ve nesne veya dizi olarak bunları oluşturmak için izin verir.

Temel türlere ek olarak, birçok uygulamatarihleri ve zaman damgalarını temsil etmek için DateTime türüne ihtiyaç duyar. Bu makalede, geliştiricilerin .NET SDK'yı kullanarak Azure Cosmos DB'de tarihleri nasıl depolayabildiği, alabildiği ve sorgulayabildiği açıklanmaktadır.

## <a name="storing-datetimes"></a>DateTimes'ı Depolama

Azure Cosmos DB gibi JSON türlerini destekler - dize, sayı, boolean, null, dizi, nesne. Doğrudan bir DateTime türünü desteklemez. Şu anda Azure Cosmos DB tarihlerin yerelleştirilmesini desteklemez. Bu nedenle, DateTimes'ı dizeleri olarak depolamanız gerekir. Azure Cosmos DB'deki DateTime dizeleri `YYYY-MM-DDThh:mm:ss.sssZ` için önerilen biçim, ISO 8601 UTC standardını izler. Tüm tarihleri Azure Cosmos DB'de UTC olarak depolamak önerilir. Tarih dizeleri'ni bu biçime dönüştürmek, tarihleri lexicographically sıralamaya olanak sağlar. UTC dışı tarihler depolanırsa, mantık istemci tarafında işlenmelidir. Yerel bir DateTime'ı UTC'ye dönüştürmek için, ofsetin JSON'da bir özellik olarak bilinmesi/depolanmış olması gerekir ve istemci mahsup ı UTC DateTime değerini hesaplamak için ofset kullanabilir.

Çoğu uygulama, DateTime için varsayılan dize gösterimini aşağıdaki nedenlerle kullanabilir:

* Dizeleri karşılaştırılabilir ve DateTime değerlerinin göreli sırası dizeleri dönüştürüldüğünde korunur.
* Bu yaklaşım, JSON dönüştürme için herhangi bir özel kod veya öznitelikleri gerektirmez.
* JSON'da depolanan tarihler okunabilir.
* Bu yaklaşım, hızlı sorgu performansı için Azure Cosmos DB dizininden yararlanabilir.

Örneğin, aşağıdaki parçacık iki DateTime `Order` özelliği içeren bir `ShipDate` nesneyi ve `OrderDate` .NET SDK'yı kullanan bir belge olarak depolar:

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

Bu belge Azure Cosmos DB'de aşağıdaki gibi depolanır:

```json
    {
        "id": "09152014101",
        "OrderDate": "2014-09-15T23:14:25.7251173Z",
        "ShipDate": "2014-09-30T23:14:25.7251173Z",
        "Total": 113.39
    }
```  

Alternatif olarak, DateTimes'ı Unix zaman damgası olarak, yani 1 Ocak 1970'ten bu yana geçen saniye sayısını temsil eden bir sayı olarak depolayabilirsiniz. Azure Cosmos DB'nin dahili Timestamp (`_ts`) özelliği bu yaklaşımı izler. DateTimes'ı sayı olarak serihale getirmek için [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) sınıfını kullanabilirsiniz.

## <a name="querying-datetimes-in-linq"></a>LINQ'da DateTimes'ı Sorgulama

SQL .NET SDK, LINQ aracılığıyla Azure Cosmos DB'de depolanan verilerin sorgulanmasını otomatik olarak destekler. Örneğin, aşağıdaki parçacık, son üç gün içinde gönderilen siparişleri filtreleyen bir LINQ sorgusunu gösterir:

```csharp
    IQueryable<Order> orders = container.GetItemLinqQueryable<Order>(allowSynchronousQueryExecution: true).Where(o => o.ShipDate >= DateTime.UtcNow.AddDays(-3));
```

Aşağıdaki SQL deyimine çevrilmiş ve Azure Cosmos DB'de yürütülmüştür:

```sql
    SELECT * FROM root WHERE (root["ShipDate"] >= "2016-12-18T21:55:03.45569Z")
```

Azure Cosmos DB'nin SQL sorgu dili ve [LINQ'daki Cosmos DB'yi Sorgulama'da](sql-query-linq-to-sql.md)LINQ sağlayıcısı hakkında daha fazla bilgi edinebilirsiniz.

## <a name="indexing-datetimes-for-range-queries"></a>Aralık sorguları için DateTimes'ı dizine alma

Sorgular DateTime değerleri ile ortaktır. Bu sorguları verimli bir şekilde yürütmek için, sorgunun filtresindeki tüm özelliklerde bir dizin tanımlanmış olması gerekir.

[Azure Cosmos DB Dizin Oluşturma İlkeleri'nde](index-policy.md)dizin oluşturma ilkelerini yapılandırma hakkında daha fazla bilgi edinebilirsiniz. 

## <a name="next-steps"></a>Sonraki Adımlar

* [Kod örneklerini GitHub'da](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples) indirin ve çalıştırın
* [SQL sorguları](sql-query-getting-started.md) hakkında daha fazla bilgi edinin
* [Azure Cosmos DB DB Dizin Oluşturma İlkeleri](index-policy.md) hakkında daha fazla bilgi edinin
