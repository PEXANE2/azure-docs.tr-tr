---
title: 'Öğretici: Azure Cosmos DB SQL ile sorgulama nasıl yapılır?'
description: 'Öğretici: sorgu PLAYIN kullanarak Azure Cosmos DB SQL sorgularıyla sorgulama hakkında bilgi edinin'
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.custom: tutorial-develop, mvc
ms.topic: tutorial
ms.date: 11/05/2019
ms.reviewer: sngun
ms.openlocfilehash: c206b68495b09988e49f37433ccd5616acd29cb7
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "93337787"
---
# <a name="tutorial-query-azure-cosmos-db-by-using-the-sql-api"></a>Öğretici: SQL API’sini kullanarak Azure Cosmos DB’yi sorgulama
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Azure Cosmos DB [SQL API’si](./introduction.md), SQL kullanılarak belgelerin sorgulanmasını destekler. Bu makalede, örnek bir belge ve iki örnek SQL sorgusu ve sonuçları sağlanmaktadır.

Bu makale aşağıdaki görevleri kapsar: 

> [!div class="checklist"]
> * SQL ile verileri sorgulama

## <a name="sample-document"></a>Örnek belge

Bu makaledeki SQL sorguları aşağıdaki örnek belgeyi kullanır.

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

## <a name="where-can-i-run-sql-queries"></a>SQL sorgularını nerede çalıştırabilirim?

Mevcut örnek veri kümesinde sorgular çalıştıran [Sorgu oyun alanı](https://www.documentdb.com/sql/demo) ve [REST API’si ve SDK’ları](sql-api-sdk-dotnet.md) aracılığıyla, Azure portalındaki Veri Gezgini’ni kullanarak sorgular çalıştırabilirsiniz.

SQL sorguları hakkında daha fazla bilgi için bkz:
* [SQL sorgusu ve SQL sözdizimi](sql-query-getting-started.md)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, bir Azure Cosmos DB hesabınız ve koleksiyonunuz olduğu varsayılır. Bu kaynaklardan hiçbiri yok mu? [5 dakikalık hızlı başlangıcı](create-cosmosdb-resources-portal.md) tamamlayın.

## <a name="example-query-1"></a>Örnek sorgu 1

Yukarıdaki örnek aile belgesi verildiğinde, SQL sorgusu, KIMLIK alanının eşleştiği belgeleri döndürür `WakefieldFamily` . Bu bir `SELECT *` deyimi olduğundan, sorgunun çıkışı eksiksiz JSON belgesidir:

**Sorgu**

```sql
    SELECT * 
    FROM Families f 
    WHERE f.id = "WakefieldFamily"
```

**Sonuçlar**

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

## <a name="example-query-2"></a>Örnek sorgu 2

Sonraki sorgu, ailede KIMLIĞI eşleşen tüm alt öğelerin verilen adlarını döndürür `WakefieldFamily` .

**Sorgu**

```sql
    SELECT c.givenName 
    FROM Families f 
    JOIN c IN f.children 
    WHERE f.id = 'WakefieldFamily'
```

**Sonuçlar**

```
[
    {
        "givenName": "Jesse"
    },
    {
        "givenName": "Lisa"
    }
]
```


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki görevleri tamamladınız:

> [!div class="checklist"]
> * SQL kullanarak sorgulamayı öğrendiniz  

Artık verilerinizi genel olarak nasıl dağıtacağınızı öğrenmek için sonraki öğreticiye ilerleyebilirsiniz.

> [!div class="nextstepaction"]
> [Verilerinizi genel olarak dağıtma](tutorial-global-distribution-sql-api.md)