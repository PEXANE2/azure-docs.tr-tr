---
title: MongoDB için Azure Cosmos DB'nin API'si ile verileri sorgula
description: MongoDB kabuk komutlarını kullanarak Azure Cosmos DB'nin MongoDB api'sinden verileri nasıl sorgulayacağım
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: tutorial
ms.date: 12/03/2019
ms.reviewer: sngun
ms.openlocfilehash: 5b9bc78f6af833d89a3404de0295ddad78ebdf20
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74870148"
---
# <a name="query-data-by-using-azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB'nin API'sini kullanarak verileri sorgula

[Azure Cosmos DB'nin MongoDB için API'si](mongodb-introduction.md) [MongoDB sorgularını](https://docs.mongodb.com/manual/tutorial/query-documents/)destekler. 

Bu makale aşağıdaki görevleri kapsar: 

> [!div class="checklist"]
> * MongoDB kabuğunu kullanarak Cosmos veritabanınızda depolanan verileri sorgulama

Başlamak için bu belgedeki örnekleri kullanabilir ve [Query Azure Cosmos DB with MongoDB shell](https://azure.microsoft.com/resources/videos/query-azure-cosmos-db-data-by-using-the-mongodb-shell/) (Azure Cosmos DB'yi MongoDB kabuğu ile sorgulama) videosunu izleyebilirsiniz.

## <a name="sample-document"></a>Örnek belge

Bu makaledeki sorgular aşağıdaki örnek belgeyi kullanır.

```json
{
  "id": "WakefieldFamily",
  "parents": [
      { "familyName": "Wakefield", "givenName": "Robin" },
      { "familyName": "Miller", "givenName": "Ben" }
  ],
  "children": [
      {
        "familyName": "Merriam", 
        "givenName": "Jesse", 
        "gender": "female", "grade": 1,
        "pets": [
            { "givenName": "Goofy" },
            { "givenName": "Shadow" }
        ]
      },
      { 
        "familyName": "Miller", 
         "givenName": "Lisa", 
         "gender": "female", 
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```
## <a name="example-query-1"></a><a id="examplequery1"></a> Örnek sorgu 1 

Yukarıda verilen örnek aile belgesiyle aşağıdaki sorgu, kimlik alanının `WakefieldFamily` ile eşleştiği belgeleri döndürür.

**Sorgu**
    
    db.families.find({ id: "WakefieldFamily"})

**Sonuç -ları**

    {
    "_id": "ObjectId(\"58f65e1198f3a12c7090e68c\")",
    "id": "WakefieldFamily",
    "parents": [
      {
        "familyName": "Wakefield",
        "givenName": "Robin"
      },
      {
        "familyName": "Miller",
        "givenName": "Ben"
      }
    ],
    "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
          { "givenName": "Goofy" },
          { "givenName": "Shadow" }
        ]
      },
      {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
      }
    ],
    "address": {
      "state": "NY",
      "county": "Manhattan",
      "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
    }

## <a name="example-query-2"></a><a id="examplequery2"></a>Örnek sorgu 2 

Sonraki sorgu, ailedeki tüm çocukları döndürür. 

**Sorgu**
    
    db.families.find( { id: "WakefieldFamily" }, { children: true } )

**Sonuç -ları**

    {
    "_id": "ObjectId("58f65e1198f3a12c7090e68c")",
    "children": [
      {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [
          { "givenName": "Goofy" },
          { "givenName": "Shadow" }
        ]
      },
      {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
      }
    ]
    }


## <a name="example-query-3"></a><a id="examplequery3"></a>Örnek sorgu 3 

Sonraki sorgu, kayıtlı olan tüm aileleri döndürür. 

**Sorgu**
    
    db.families.find( { "isRegistered" : true })
**Sonuçlar** Herhangi bir belge döndürülmez. 

## <a name="example-query-4"></a><a id="examplequery4"></a>Örnek sorgu 4

Sonraki sorgu, kayıtlı olmayan tüm aileleri döndürür. 

**Sorgu**
    
    db.families.find( { "isRegistered" : false })
**Sonuç -ları**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}

## <a name="example-query-5"></a><a id="examplequery5"></a>Örnek sorgu 5

Sonraki sorgu, kayıtlı olmayan ve durumu NY olan tüm aileleri döndürür. 

**Sorgu**
    
     db.families.find( { "isRegistered" : false, "address.state" : "NY" })

**Sonuç -ları**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}


## <a name="example-query-6"></a><a id="examplequery6"></a>Örnek sorgu 6

Sonraki sorgu, çocukları 8. sınıfta olan tüm aileleri döndürür.

**Sorgu**
  
     db.families.find( { children : { $elemMatch: { grade : 8 }} } )

**Sonuç -ları**

     {
    "_id": ObjectId("58f65e1198f3a12c7090e68c"),
    "id": "WakefieldFamily",
    "parents": [{
        "familyName": "Wakefield",
        "givenName": "Robin"
    }, {
        "familyName": "Miller",
        "givenName": "Ben"
    }],
    "children": [{
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1,
        "pets": [{
            "givenName": "Goofy"
        }, {
            "givenName": "Shadow"
        }]
    }, {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }],
    "address": {
        "state": "NY",
        "county": "Manhattan",
        "city": "NY"
    },
    "creationDate": 1431620462,
    "isRegistered": false
}

## <a name="example-query-7"></a><a id="examplequery7"></a>Örnek sorgu 7

Sonraki sorgu, çocuk dizisi boyutu 3 olan tüm aileleri döndürür.

**Sorgu**
  
      db.Family.find( {children: { $size:3} } )

**Sonuç -ları**

İkiden fazla çocuğu olan bir aile olmadığından herhangi bir sonuç döndürülmez. Yalnızca parametre 2 olduğunda bu sorgu başarılı olur ve tam belgeyi döndürür.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdakileri yaptınız:

> [!div class="checklist"]
> * MongoDB için Cosmos DB'nin API'sini kullanarak sorgulama yapmayı öğrendim

Artık verilerinizi genel olarak nasıl dağıtacağınızı öğrenmek için sonraki öğreticiye ilerleyebilirsiniz.

> [!div class="nextstepaction"]
> [Verilerinizi genel olarak dağıtma](tutorial-global-distribution-sql-api.md)

