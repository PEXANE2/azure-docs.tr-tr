---
title: Azure Cosmos DB 'de SQL sorgusu yürütme
description: Azure Cosmos DB bir SQL sorgusu oluşturmayı ve bunu nasıl çalıştıracağınızı öğrenin. Bu makalede REST API, .NET SDK, JavaScript SDK ve çeşitli diğer SDK 'lar kullanılarak SQL sorgusu oluşturma ve yürütme açıklanır.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: tisande
ms.openlocfilehash: 70eb81b6d13c57a7ebc131244c7aa318cb2b2fd4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74871270"
---
# <a name="azure-cosmos-db-sql-query-execution"></a>SQL sorgu yürütmeyi Azure Cosmos DB

HTTP/HTTPS istekleri yapabilen herhangi bir dil Cosmos DB REST API çağırabilir. Cosmos DB ayrıca .NET, Node.js, JavaScript ve Python programlama dilleri için programlama kitaplıkları sunar. REST API ve kitaplıkları, SQL aracılığıyla sorgulama desteği ve .NET SDK, [LINQ sorgulama](sql-query-linq-to-sql.md)'yı da destekler.

Aşağıdaki örneklerde bir sorgu oluşturma ve Cosmos veritabanı hesabına göre gönderme işlemlerinin nasıl yapılacağı gösterilmektedir.

## <a name="rest-api"></a><a id="REST-API"></a>REST API

Cosmos DB, HTTP üzerinden açık bir yenilenmiş programlama modeli sunar. Kaynak modeli, bir Azure aboneliğinin sağlamasını sağlayan bir veritabanı hesabı altındaki bir kaynak kümesinden oluşur. Veritabanı hesabı, her biri birden çok *kapsayıcı*içerebilen bir *veritabanı*kümesinden oluşur. Bu, *öğeleri*, UDF 'leri ve diğer kaynak türlerini içerir. Her Cosmos DB kaynak, mantıksal ve kararlı bir URI kullanılarak adreslenedir. Bir dizi kaynağa *akış*denir. 

Bu kaynaklarla temel etkileşim modeli,,, `GET` `PUT` `POST` ve `DELETE` Standart yorumlarıyla birlikte HTTP fiilleri üzerinden yapılır. `POST`Yeni bir kaynak oluşturmak, saklı yordamı yürütmek veya Cosmos DB bir sorgu vermek için kullanın. Sorgular, her zaman yan etkileri olmayan salt yazılır işlemlerdir.

Aşağıdaki örneklerde `POST` örnek öğeler için BIR SQL API sorgusu gösterilmektedir. Sorgunun JSON özelliğinde basit bir filtresi vardır `name` . `x-ms-documentdb-isquery`Ve Content-Type: `application/query+json` Headers işlemin bir sorgu olduğunu gösterir. `mysqlapicosmosdb.documents.azure.com:443`Cosmos DB hesabınız IÇIN URI ile değiştirin.

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

Sonraki, daha karmaşık sorgu bir birleşimden birden çok sonuç döndürüyor:

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

Sorgunun sonuçları tek bir sayfaya sığamayacak olursa REST API yanıt üst bilgisi aracılığıyla bir devamlılık belirteci döndürür `x-ms-continuation-token` . İstemciler, üst bilgiyi izleyen sonuçlara dahil ederek sonuçları sayfadan alabilir. Ayrıca, sayı üst bilgisinden sayfa başına sonuç sayısını denetleyebilirsiniz `x-ms-max-item-count` .

Sorgunun COUNT gibi bir toplama işlevi varsa, sorgu sayfası yalnızca bir sonuç sayfası üzerinde kısmen toplanmış bir değer döndürebilir. İstemciler nihai sonuçları oluşturmak için bu sonuçlara ikinci düzey bir toplama işlemi gerçekleştirmelidir. Örneğin, toplam sayısı döndürmek için ayrı sayfalarda döndürülen sayların toplamı.

Sorgular için veri tutarlılığı ilkesini yönetmek üzere `x-ms-consistency-level` üst bilgisini tüm REST API isteklerinde olarak kullanın. Oturum tutarlılığı Ayrıca sorgu isteğindeki en son `x-ms-session-token` tanımlama bilgisi üst bilgisini yankılandırmasını gerektirir. Sorgulanan kapsayıcının dizin oluşturma ilkesi, sorgu sonuçlarının tutarlılığını de etkileyebilir. Kapsayıcılar için varsayılan dizin oluşturma ilkesi ayarları ile, Dizin her zaman öğe içeriğiyle geçerli olur ve sorgu sonuçları, veriler için seçilen tutarlılık ile eşleşir. Daha fazla bilgi için bkz. [Azure Cosmos DB tutarlılık düzeyleri] [tutarlılık-düzeyler].

Kapsayıcıda yapılandırılan Dizin oluşturma ilkesi belirtilen sorguyu destekleyemiyorum, Azure Cosmos DB sunucusu 400 "bozuk Istek" döndürür. Bu hata iletisi, dizin oluşturma işleminden açıkça dışlanan yolların bulunduğu sorgular için döndürür. `x-ms-documentdb-query-enable-scan`Bir dizin kullanılamadığında sorgunun tarama yapmasına izin vermek için üst bilgiyi belirtebilirsiniz.

Üstbilgiyi olarak ayarlayarak sorgu yürütme hakkında ayrıntılı ölçümler edinebilirsiniz `x-ms-documentdb-populatequerymetrics` `true` . Daha fazla bilgi için bkz. [Azure Cosmos DB Için SQL sorgu ölçümleri](sql-api-query-metrics.md).

## <a name="c-net-sdk"></a>C# (.NET SDK)

.NET SDK, hem LINQ hem de SQL sorgulaması destekler. Aşağıdaki örnek, önceki filtre sorgusunun .NET ile nasıl gerçekleştirileceğini göstermektedir:

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

Aşağıdaki örnek, her öğe içinde eşitlik için iki özelliği karşılaştırır ve anonim tahminleri kullanır.

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

Sonraki örnekte, LINQ aracılığıyla ifade edilen birleşimler gösterilmektedir `SelectMany` .

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

.NET istemcisi, `foreach` Önceki örnekte gösterildiği gibi bloklardaki sorgu sonuçlarının tüm sayfalarında otomatik olarak yinelenir. [REST API](#REST-API) bölümünde tanıtılan sorgu seçenekleri, .NET SDK 'da, `FeedOptions` yöntemindeki ve sınıfları kullanılarak da kullanılabilir `FeedResponse` `CreateDocumentQuery` . Ayarını kullanarak sayfa sayısını kontrol edebilirsiniz `MaxItemCount` .

Ayrıca, `IDocumentQueryable` `IQueryable` nesnesini kullanarak `ResponseContinuationToken` ve sonra değerleri okuyarak ve sonra yeniden geçirerek, `RequestContinuationToken` `FeedOptions` sayfalama denetimini açıkça denetleyebilirsiniz. `EnableScanInQuery`Sorgu, yapılandırılmış dizin oluşturma ilkesi tarafından desteklenmiyorsa taramayı etkinleştir olarak ayarlayabilirsiniz. Bölümlenmiş kapsayıcılar için, `PartitionKey` sorguyu tek bir bölüme karşı çalıştırmak için kullanabilirsiniz, ancak Azure Cosmos db sorgu metinden otomatik olarak ayıklayabilir. `EnableCrossPartitionQuery`Birden çok bölüme karşı sorgu çalıştırmak için ' i kullanabilirsiniz.

Sorgularla daha fazla .NET örneği için bkz. GitHub 'da [.net örneklerine Azure Cosmos DB](https://github.com/Azure/azure-cosmos-dotnet-v3) .

## <a name="javascript-server-side-api"></a><a id="JavaScript-server-side-API"></a>JavaScript sunucu tarafı API 'SI

Azure Cosmos DB, saklı yordamları ve Tetikleyicileri kullanarak doğrudan kapsayıcılarda [JavaScript tabanlı uygulama mantığını yürütmeye](stored-procedures-triggers-udfs.md) yönelik bir programlama modeli sağlar. Kapsayıcı düzeyinde kayıtlı JavaScript mantığı, daha sonra çevresel ACID işlemlerine kaydırılmış olan belirtilen kapsayıcının öğeleri üzerinde veritabanı işlemleri verebilir.

Aşağıdaki örnek, `queryDocuments` saklı yordamların ve tetikleyicilerin içinden sorgu yapmak Için JavaScript sunucu API 'sinde nasıl kullanılacağını gösterir:

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

- [Azure Cosmos DB giriş](introduction.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Azure Cosmos DB tutarlılık düzeyleri](consistency-levels.md)
