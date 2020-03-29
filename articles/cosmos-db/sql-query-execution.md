---
title: Azure Cosmos DB'de SQL sorgu yürütmesi
description: Azure Cosmos DB'de nasıl bir SQL sorgusu oluşturup yürütülmeyi öğrenin. Bu makalede, REST API, .Net SDK, JavaScript SDK ve diğer çeşitli SDK'ları kullanarak bir SQL sorgusunun nasıl oluşturulup yürütüleceğimiz açıklanmaktadır.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: tisande
ms.openlocfilehash: 70eb81b6d13c57a7ebc131244c7aa318cb2b2fd4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74871270"
---
# <a name="azure-cosmos-db-sql-query-execution"></a>Azure Cosmos DB SQL sorgu yürütme

HTTP/HTTPS isteklerini yapabilen herhangi bir dil Cosmos DB REST API'yi arayabilir. Cosmos DB ayrıca .NET, Node.js, JavaScript ve Python programlama dilleri için programlama kitaplıkları da sunar. REST API ve kitaplıkları tüm SQL üzerinden sorgu destekleyen ve .NET SDK da [LINQ sorgu destekler.](sql-query-linq-to-sql.md)

Aşağıdaki örnekler, bir sorgunun nasıl oluşturulup cosmos veritabanı hesabına nasıl gönderilenleri gösterir.

## <a name="rest-api"></a><a id="REST-API"></a>REST API

Cosmos DB, HTTP üzerinden açık bir RESTful programlama modeli sunar. Kaynak modeli, azure aboneliğinin geçerli olduğu bir veritabanı hesabı altında bir dizi kaynaktan oluşur. Veritabanı *hesabı,* her biri birden çok *kapsayıcı*içerebilen ve sırayla *öğeler,* UDF'ler ve diğer kaynak türleri içeren bir veritabanı kümesinden oluşur. Her Cosmos DB kaynağı mantıksal ve kararlı URI kullanılarak ele alınabiliyor. Bir kaynak kümesine *besleme*denir. 

Bu kaynaklarla temel etkileşim modeli, standart `GET` `PUT`yorumları `POST`ile `DELETE`HTTP fiilleri , , , ve , aracılığıyladır. Yeni `POST` bir kaynak oluşturmak, depolanan yordamı yürütmek veya Cosmos DB sorgusu vermek için kullanın. Sorgular her zaman hiçbir yan etkisi olmayan salt okunur işlemlerdir.

Aşağıdaki örnekler, `POST` örnek öğelere karşı bir SQL API sorgusu için bir gösteridir. Sorgujson `name` özelliği üzerinde basit bir filtre vardır. Ve `x-ms-documentdb-isquery` İçerik Türü: `application/query+json` üstbilgi, işlemin bir sorgu olduğunu gösterir. Cosmos DB hesabınız için URI ile değiştirin. `mysqlapicosmosdb.documents.azure.com:443`

```json
    POST https://mysqlapicosmosdb.documents.azure.com:443/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {
        "query": "SELECT * FROM Families f WHERE f.id = @familyId",
        "parameters": [
            {"name": "@familyId", "value": "AndersenFamily"}
        ]
    }
```

Sonuçlar:

```json
    HTTP/1.1 200 Ok
    x-ms-activity-id: 8b4678fa-a947-47d3-8dd3-549a40da6eed
    x-ms-item-count: 1
    x-ms-request-charge: 0.32

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "id":"AndersenFamily",
             "lastName":"Andersen",
             "parents":[  
                {  
                   "firstName":"Thomas"
                },
                {  
                   "firstName":"Mary Kay"
                }
             ],
             "children":[  
                {  
                   "firstName":"Henriette Thaulow",
                   "gender":"female",
                   "grade":5,
                   "pets":[  
                      {  
                         "givenName":"Fluffy"
                      }
                   ]
                }
             ],
             "address":{  
                "state":"WA",
                "county":"King",
                "city":"Seattle"
             },
             "_rid":"u1NXANcKogEcAAAAAAAAAA==",
             "_ts":1407691744,
             "_self":"dbs\/u1NXAA==\/colls\/u1NXANcKogE=\/docs\/u1NXANcKogEcAAAAAAAAAA==\/",
             "_etag":"00002b00-0000-0000-0000-53e7abe00000",
             "_attachments":"_attachments\/"
          }
       ],
       "count":1
    }
```

Sonraki, daha karmaşık sorgu bir birleşimden birden çok sonuç döndürür:

```json
    POST https://https://mysqlapicosmosdb.documents.azure.com:443/docs HTTP/1.1
    ...
    x-ms-documentdb-isquery: True
    Content-Type: application/query+json

    {
        "query": "SELECT
                     f.id AS familyName,
                     c.givenName AS childGivenName,
                     c.firstName AS childFirstName,
                     p.givenName AS petName
                  FROM Families f
                  JOIN c IN f.children
                  JOIN p in c.pets",
        "parameters": []
    }
```

Sonuçlar: 

```json
    HTTP/1.1 200 Ok
    x-ms-activity-id: 568f34e3-5695-44d3-9b7d-62f8b83e509d
    x-ms-item-count: 1
    x-ms-request-charge: 7.84

    {  
       "_rid":"u1NXANcKogE=",
       "Documents":[  
          {  
             "familyName":"AndersenFamily",
             "childFirstName":"Henriette Thaulow",
             "petName":"Fluffy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Goofy"
          },
          {  
             "familyName":"WakefieldFamily",
             "childGivenName":"Jesse",
             "petName":"Shadow"
          }
       ],
       "count":3
    }
```

Bir sorgunun sonuçları tek bir sayfaya sığamazsa, REST API yanıt `x-ms-continuation-token` üstbilgisi aracılığıyla bir devam belirteci döndürür. İstemciler sonraki sonuçlara üstbilgi ekleyerek sonuçları paginate edebilirsiniz. Ayrıca, sayı üstbilgisi `x-ms-max-item-count` aracılığıyla sayfa başına sonuç sayısını da denetleyebilirsiniz.

Bir sorguda COUNT gibi bir toplama işlevi varsa, sorgu sayfası yalnızca bir sonuç sayfası üzerinde kısmen birleştirilmiş bir değer döndürebilir. İstemciler, nihai sonuçları elde etmek için bu sonuçlar üzerinde ikinci düzey bir toplama gerçekleştirmelidir. Örneğin, toplam sayımı döndürmek için tek tek sayfalarda döndürülen sayımların toplamı.

Sorgular için veri tutarlılığı ilkesini `x-ms-consistency-level` yönetmek için üstbilginin tüm REST API isteklerinde olduğu gibi kullanın. Oturum tutarlılığı, sorgu isteğindeki en son `x-ms-session-token` çerez üstbilgisinin yankılanması da gerektirir. Sorgulanmış kapsayıcının dizin oluşturma ilkesi de sorgu sonuçlarının tutarlılığını etkileyebilir. Kapsayıcılar için varsayılan dizin dizini ayarlarıyla, dizin her zaman madde içeriğiyle geçerlidir ve sorgu sonuçları veriler için seçilen tutarlılıkla eşleşir. Daha fazla bilgi için bkz: [Azure Cosmos DB tutarlılık düzeyleri][tutarlılık düzeyleri].

Kapsayıcıdaki yapılandırılan dizin oluşturma ilkesi belirtilen sorguyu desteklenemezse, Azure Cosmos DB sunucusu 400 "Hatalı İstek" döndürür. Bu hata iletisi, dizin oluşturmanın dışında açıkça dışlanmış yolları olan sorgular için döndürür. Dizin `x-ms-documentdb-query-enable-scan` kullanılabilir olmadığında sorgunun bir tayini gerçekleştirmesine izin vermek için üstbilgi belirtebilirsiniz.

Üstbilgiyi `true`' ye ayarlayarak sorgu `x-ms-documentdb-populatequerymetrics` yürütmesi hakkında ayrıntılı ölçümler alabilirsiniz Daha fazla bilgi için [Azure Cosmos DB için SQL sorgu ölçümlerine](sql-api-query-metrics.md)bakın.

## <a name="c-net-sdk"></a>C# (.NET SDK)

.NET SDK hem LINQ hem de SQL sorgusunu destekler. Aşağıdaki örnekte, .NET ile önceki filtre sorgusunun nasıl gerçekleştirililiz gösterilmektedir:

```csharp
    foreach (var family in client.CreateDocumentQuery(containerLink,
        "SELECT * FROM Families f WHERE f.id = \"AndersenFamily\""))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    SqlQuerySpec query = new SqlQuerySpec("SELECT * FROM Families f WHERE f.id = @familyId");
    query.Parameters = new SqlParameterCollection();
    query.Parameters.Add(new SqlParameter("@familyId", "AndersenFamily"));

    foreach (var family in client.CreateDocumentQuery(containerLink, query))
    {
        Console.WriteLine("\tRead {0} from parameterized SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery(containerLink)
        where f.Id == "AndersenFamily"
        select f))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in client.CreateDocumentQuery(containerLink)
        .Where(f => f.Id == "AndersenFamily")
        .Select(f => f))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }
```

Aşağıdaki örnek, her öğe içinde eşitlik için iki özelliği karşılaştırır ve anonim projeksiyonlar kullanır.

```csharp
    foreach (var family in client.CreateDocumentQuery(containerLink,
        @"SELECT {""Name"": f.id, ""City"":f.address.city} AS Family
        FROM Families f
        WHERE f.address.city = f.address.state"))
    {
        Console.WriteLine("\tRead {0} from SQL", family);
    }

    foreach (var family in (
        from f in client.CreateDocumentQuery<Family>(containerLink)
        where f.address.city == f.address.state
        select new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ query", family);
    }

    foreach (var family in
        client.CreateDocumentQuery<Family>(containerLink)
        .Where(f => f.address.city == f.address.state)
        .Select(f => new { Name = f.Id, City = f.address.city }))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", family);
    }
```

Sonraki örnek, LINQ `SelectMany`ile ifade edilen birleştirmeleri gösterir.

```csharp
    foreach (var pet in client.CreateDocumentQuery(containerLink,
          @"SELECT p
            FROM Families f
                 JOIN c IN f.children
                 JOIN p in c.pets
            WHERE p.givenName = ""Shadow"""))
    {
        Console.WriteLine("\tRead {0} from SQL", pet);
    }

    // Equivalent in Lambda expressions:
    foreach (var pet in
        client.CreateDocumentQuery<Family>(containerLink)
        .SelectMany(f => f.children)
        .SelectMany(c => c.pets)
        .Where(p => p.givenName == "Shadow"))
    {
        Console.WriteLine("\tRead {0} from LINQ lambda", pet);
    }
```

.NET istemcisi, önceki örnekte gösterildiği gibi, `foreach` bloklarda sorgu sonuçlarının tüm sayfalarında otomatik olarak yinelenir. [REST API](#REST-API) bölümünde tanıtılan sorgu seçenekleri de `FeedOptions` `FeedResponse` `CreateDocumentQuery` .NET SDK'da, yöntemdeki ve sınıfları kullanarak kullanılabilir. `MaxItemCount` Ayarı kullanarak sayfa sayısını kontrol edebilirsiniz.

Ayrıca, `IDocumentQueryable` `IQueryable` nesneyi kullanarak oluşturarak `ResponseContinuationToken` sayfalamayı açıkça denetleyebilir, ardından değerleri `RequestContinuationToken` `FeedOptions`okuyarak ve 'deki gibi geri geçirerek. Sorgu yapılandırılan dizin ilkesi tarafından desteklenmediği zaman taramaları etkinleştirmek için ayarlayabilirsiniz. `EnableScanInQuery` Azure Cosmos DB `PartitionKey` bunu sorgu metninden otomatik olarak çıkarabilse de, bölümlenmiş kapsayıcılar için sorguyu tek bir bölüme karşı çalıştırmak için kullanabilirsiniz. Sorguları `EnableCrossPartitionQuery` birden çok bölüme karşı çalıştırmak için kullanabilirsiniz.

Sorguları olan daha fazla .NET örneği için GitHub'daki [Azure Cosmos DB .NET örneklerine](https://github.com/Azure/azure-cosmos-dotnet-v3) bakın.

## <a name="javascript-server-side-api"></a><a id="JavaScript-server-side-API"></a>JavaScript sunucu tarafı API

Azure Cosmos DB, depolanan yordamları ve tetikleyicileri kullanarak [JavaScript tabanlı uygulama](stored-procedures-triggers-udfs.md) mantığını doğrudan kapsayıcılara uygulamak için bir programlama modeli sağlar. Kapsayıcı düzeyinde kayıtlı JavaScript mantığı daha sonra ortam ACID işlemleri sarılmış, verilen kapsayıcının öğeleri veritabanı işlemleri verebilir.

Aşağıdaki örnek, javascript `queryDocuments` sunucu API'sinde depolanan yordamlar ve tetikleyiciler için sorgu yapmak için nasıl kullanılacağını gösterir:

```javascript
    function findName(givenName, familyName) {
        var context = getContext();
        var containerManager = context.getCollection();
        var containerLink = containerManager.getSelfLink()

        // create a new item.
        containerManager.createDocument(containerLink,
            { givenName: givenName, familyName: familyName },
            function (err, documentCreated) {
                if (err) throw new Error(err.message);

                // filter items by familyName
                var filterQuery = "SELECT * from root r WHERE r.familyName = 'Wakefield'";
                containerManager.queryDocuments(containerLink,
                    filterQuery,
                    function (err, matchingDocuments) {
                        if (err) throw new Error(err.message);
    context.getResponse().setBody(matchingDocuments.length);

                        // Replace the familyName for all items that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].familyName = "Robin Wakefield";
                            // we don't need to execute a callback because they are in parallel
                            containerManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);
                        }
                    })
            });
    }
```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB'ye Giriş](introduction.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Azure Cosmos DB tutarlılık düzeyleri](consistency-levels.md)
