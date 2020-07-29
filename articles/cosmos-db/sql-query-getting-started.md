---
title: Azure Cosmos DB 'de SQL sorguları ile çalışmaya başlama
description: Azure Cosmos DB verileri sorgulamak için SQL sorgularını nasıl kullanacağınızı öğrenin. Örnek verileri, Azure Cosmos DB bir kapsayıcıya yükleyebilir ve sorgulayabilirsiniz.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/24/2020
ms.author: tisande
ms.openlocfilehash: d292b7cfcda73cb4cd6ac2535c7e27fc675e1030
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87308194"
---
# <a name="getting-started-with-sql-queries"></a>SQL sorgularıyla çalışmaya başlama

SQL API hesapları Azure Cosmos DB, verileri bulmanın iki yolu vardır:

**Nokta okuma** -tek BIR *öğe kimliğinde* ve bölüm anahtarında anahtar/değer araması yapabilirsiniz. *Öğe kimliği* ve bölüm anahtarı birleşimi anahtardır ve öğenin kendisi değerdir. 1 KB 'lik bir belge için, nokta, genellikle 10 MS 'nin altında bir gecikme süresine sahip maliyet 1 [istek birimini](request-units.md) okur. Nokta okuma tek bir öğe döndürür.

**SQL sorguları** -YAPıLANDıRıLMıŞ sorgu DILI (SQL) kullanan sorguları JSON sorgu dili olarak yazarak verileri sorgulayabilirsiniz. Sorgular her zaman en az 2,3 istek birimi maliyetlidir ve genel olarak nokta okumasından daha yüksek ve daha fazla değişken gecikme süresine sahip olur. Sorgular birçok öğe döndürebilir.

Azure Cosmos DB çoğu okuma ağır iş yükü, her iki nokta okuma ve SQL sorgularının birleşimini kullanır. Yalnızca tek bir öğeyi okumanız gerekiyorsa, nokta okumaları sorgular ve sorgulardan daha hızlıdır. İşaret okumalarının veriye erişmek için sorgu altyapısını kullanması gerekmez ve verileri doğrudan okuyabilirler. Tabii ki, tüm iş yüklerinin işaret okumaları kullanarak verileri özel olarak okuyabilmesi mümkün değildir, bu nedenle SQL 'in bir sorgu dili olarak desteklenmesi ve [şema belirsiz dizin oluşturma](index-overview.md) verilerinize daha esnek bir yol sağlar.

Her SDK ile nasıl yapılır noktası okumalarının bazı örnekleri aşağıda verilmiştir:

- [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.container.readitemasync?view=azure-dotnet)
- [Java SDK](https://docs.microsoft.com/java/api/com.azure.cosmos.cosmoscontainer.readitem?view=azure-java-stable#com_azure_cosmos_CosmosContainer__T_readItem_java_lang_String_com_azure_cosmos_models_PartitionKey_com_azure_cosmos_models_CosmosItemRequestOptions_java_lang_Class_T__)
- [Node.js SDK’sı](https://docs.microsoft.com/javascript/api/@azure/cosmos/item?view=azure-node-latest#read-requestoptions-)
- [Python SDK'sı](https://docs.microsoft.com/python/api/azure-cosmos/azure.cosmos.containerproxy?view=azure-python#read-item-item--partition-key--populate-query-metrics-none--post-trigger-include-none----kwargs-)

Bu belge geri kalanında Azure Cosmos DB SQL sorguları yazmaya nasıl başlacağınız gösterilmektedir. SQL sorguları, SDK veya Azure portal aracılığıyla çalıştırılabilir.

## <a name="upload-sample-data"></a>Örnek verileri karşıya yükle

SQL API Cosmos DB hesabınızda adlı bir kapsayıcı oluşturun `Families` . Kapsayıcıda iki basit JSON öğesi oluşturun. Bu veri kümesini kullanarak Azure Cosmos DB sorgu belgelerinde örnek sorguların çoğunu çalıştırabilirsiniz.

### <a name="create-json-items"></a>JSON öğeleri oluşturma

Aşağıdaki kod, aileler hakkında iki basit JSON öğesi oluşturur. Andersen ve Wakefield ailelerinin basit JSON öğeleri, ebeveynler, alt öğeler, adresler ve kayıt bilgilerini içerir. İlk öğede dizeler, sayılar, Boole değerleri, diziler ve iç içe geçmiş özellikler vardır.

```json
{
  "id": "AndersenFamily",
  "lastName": "Andersen",
  "parents": [
     { "firstName": "Thomas" },
     { "firstName": "Mary Kay"}
  ],
  "children": [
     {
         "firstName": "Henriette Thaulow",
         "gender": "female",
         "grade": 5,
         "pets": [{ "givenName": "Fluffy" }]
     }
  ],
  "address": { "state": "WA", "county": "King", "city": "Seattle" },
  "creationDate": 1431620472,
  "isRegistered": true
}
```

İkinci öğe `givenName` `familyName` ve yerine ve kullanır `firstName` `lastName` .

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
         "grade": 8 }
  ],
  "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
  "creationDate": 1431620462,
  "isRegistered": false
}
```

### <a name="query-the-json-items"></a>JSON öğelerini sorgulama

Azure Cosmos DB SQL sorgu dilinin bazı önemli yönlerini anlamak için JSON verilerinde birkaç sorgu deneyin.

Aşağıdaki sorgu, `id` alanın eşleştiği öğeleri döndürür `AndersenFamily` . `SELECT *`Sorgu olduğundan, sorgunun çıktısı tamamlanmış JSON öğesidir. Sözdizimi Seç hakkında daha fazla bilgi için bkz. [select deyimleri](sql-query-select.md).

```sql
    SELECT *
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Sorgu sonuçları şunlardır:

```json
    [{
        "id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
           { "firstName": "Thomas" },
           { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "Seattle" },
        "creationDate": 1431620472,
        "isRegistered": true
    }]
```

Aşağıdaki sorgu JSON çıkışını farklı bir şekle yeniden biçimlendirir. Sorgu, `Family` Seçili iki alana sahip yeni BIR JSON nesnesi `Name` ve `City` Adres şehri durum ile aynı olduğunda. "NY, NY" Bu durumda eşleşir.

```sql
    SELECT {"Name":f.id, "City":f.address.city} AS Family
    FROM Families f
    WHERE f.address.city = f.address.state
```

Sorgu sonuçları şunlardır:

```json
    [{
        "Family": {
            "Name": "WakefieldFamily",
            "City": "NY"
        }
    }]
```

Aşağıdaki sorgu, aile içindeki alt öğelerin verilen tüm adlarını ( `id` `WakefieldFamily` şehre göre sıralanmış olan) döndürür.

```sql
    SELECT c.givenName
    FROM Families f
    JOIN c IN f.children
    WHERE f.id = 'WakefieldFamily'
    ORDER BY f.address.city ASC
```

Sonuçlar:

```json
    [
      { "givenName": "Jesse" },
      { "givenName": "Lisa"}
    ]
```

## <a name="remarks"></a>Açıklamalar

Yukarıdaki örneklerde Cosmos DB sorgu dilinin çeşitli yönleri gösterilmektedir:  

* SQL API 'SI JSON değerleri üzerinde çalıştığından, satırlar ve sütunlar yerine Ağaç şekilli varlıklarla ilgilenir. Ağaç düğümlerine, `Node1.Node2.Node3…..Nodem` ANSI SQL ' in iki parçalı başvurusuna benzer şekilde herhangi bir rastgele derinlikte başvurabilirsiniz `<table>.<column>` .

* Sorgu dili şeless verilerle çalıştığından, tür sistemi dinamik olarak bağlanmalıdır. Aynı ifade farklı öğelerde farklı türler sağlayabilir. Bir sorgunun sonucu geçerli bir JSON değeridir, ancak sabit bir şema olması garanti edilmez.  

* Azure Cosmos DB yalnızca katı JSON öğelerini destekler. Tür sistemi ve ifadeleri yalnızca JSON türleriyle uğraşmak üzere kısıtlanmıştır. Daha fazla bilgi için bkz. [JSON belirtimi](https://www.json.org/).  

* Cosmos kapsayıcısı, JSON öğelerinin şema içermeyen bir koleksiyonudur. Kapsayıcı öğeleri içindeki ve içindeki ilişkiler, birincil anahtar ve yabancı anahtar ilişkilerine göre değil, kapsama göre örtük olarak yakalanır. Bu özellik, bu makalenin ilerleyen kısımlarında açıklanan öğe içi birleşimler için önemlidir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB giriş](introduction.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [SELECT yan tümcesi](sql-query-select.md)
