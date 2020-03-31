---
title: Cassandra için Azure Cosmos DB API'sında özet akışı değiştirme
description: Cassandra'nın verilerinizdeki değişiklikleri alması için Azure Cosmos DB API'sinde değişiklik akışını nasıl kullanacağınızı öğrenin.
author: TheovanKraay
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: conceptual
ms.date: 11/25/2019
ms.author: thvankra
ms.openlocfilehash: c2c695608653130b97bf29cc9ce48e2fbb429209
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74694629"
---
# <a name="change-feed-in-the-azure-cosmos-db-api-for-cassandra"></a>Cassandra için Azure Cosmos DB API'sında özet akışı değiştirme

Cassandra için Azure Cosmos DB API'deki özet akışı desteğini [değiştir,](change-feed.md) Cassandra Sorgu Dili'ndeki (CQL) sorgu yüklemleri aracılığıyla kullanılabilir. Bu yüklem koşullarını kullanarak, değişiklik akışı API'sini sorgulayabilirsiniz. Uygulamalar, CQL'de gerekli olduğu gibi birincil anahtarı (bölüm tuşu olarak da bilinir) kullanarak tabloda yapılan değişiklikleri alabilir. Daha sonra sonuçlara dayalı olarak daha fazla işlem yapabilirsiniz. Tablodaki satırdeğişiklikleri, değişiklik zamanları sırasına göre yakalanır ve bölüm anahtarı başına sıralama sırası garanti edilir.

Aşağıdaki örnekte, .NET kullanarak Cassandra API Keyspace tablosundaki tüm satırlarda değişiklik akışı nasıl alınış gösterilmektedir. Yüklem COSMOS_CHANGEFEED_START_TIME() belirli bir başlangıç saatinden (bu durumda geçerli tarih saatinde) değişiklik akışındaki öğeleri sorgulamak için doğrudan CQL içinde kullanılır. Burada tam [örnek](https://docs.microsoft.com/samples/azure-samples/azure-cosmos-db-cassandra-change-feed/cassandra-change-feed/)indirebilirsiniz.

Her yinelemede, sorgu durumu kullanılarak son nokta değişikliklerinde sorgu devam eder. Keyspace'de tabloda sürekli yeni değişiklikler akışı görebiliriz. Eklenen veya güncelleştirilen satırlarda değişiklikler görürsünüz. Cassandra API'deki değişiklik yayınını kullanarak işlemleri silme işlemleri şu anda desteklenmemektedir. 

```C#
    //set initial start time for pulling the change feed
     DateTime timeBegin = DateTime.UtcNow;

    //initialise variable to store the continuation token
    byte[] pageState = null;
    while (true)
    {
        try
        {

            //Return the latest change for all rows in 'user' table    
            IStatement changeFeedQueryStatement = new SimpleStatement(
            $"SELECT * FROM uprofile.user where COSMOS_CHANGEFEED_START_TIME() = '{timeBegin.ToString("yyyy-MM-ddTHH:mm:ss.fffZ", CultureInfo.InvariantCulture)}'");
            if (pageState != null)
            {
                changeFeedQueryStatement = changeFeedQueryStatement.SetPagingState(pageState);
            }
            Console.WriteLine("getting records from change feed at last page state....");
            RowSet rowSet = session.Execute(changeFeedQueryStatement);

            //store the continuation token here
            pageState = rowSet.PagingState;

            List<Row> rowList = rowSet.ToList();
            if (rowList.Count != 0)
            {
                for (int i = 0; i < rowList.Count; i++)
                {
                    string value = rowList[i].GetValue<string>("user_name");
                    int key = rowList[i].GetValue<int>("user_id");
                    // do something with the data - e.g. compute, forward to another event, function, etc.
                    // here, we just print the user name field
                    Console.WriteLine("user_name: " + value);
                }
            }
            else
            {
                Console.WriteLine("zero documents read");
            }
        }
        catch (Exception e)
        {
            Console.WriteLine("Exception " + e);
        }
    }

```

Değişiklikleri birincil anahtarla tek bir satıra almak için, sorguya birincil anahtarı ekleyebilirsiniz. Aşağıdaki örnekte, "user_id = 1" satırıiçin değişikliklerin nasıl izlendiği gösterilmektedir

```C#
    //Return the latest change for all row in 'user' table where user_id = 1
    IStatement changeFeedQueryStatement = new SimpleStatement(
    $"SELECT * FROM uprofile.user where user_id = 1 AND COSMOS_CHANGEFEED_START_TIME() = '{timeBegin.ToString("yyyy-MM-ddTHH:mm:ss.fffZ", CultureInfo.InvariantCulture)}'");

```

## <a name="current-limitations"></a>Geçerli sınırlamalar

Cassandra API ile değişiklik beslemesi kullanırken aşağıdaki sınırlamalar geçerlidir:

* Ekler ve güncelleştirmeler şu anda desteklenir. Silme işlemi henüz desteklenmedi. Geçici çözüm olarak, silinen satırlara yumuşak bir işaretçi ekleyebilirsiniz. Örneğin, satıra "silinmiş" adlı bir alan ekleyin ve "true" olarak ayarlayın.
* Son güncelleştirme, temel SQL API'de olduğu gibi devam etti ve varlık için ara güncelleştirmeler kullanılamıyor.


## <a name="error-handling"></a>Hata işleme

Cassandra API'de değişiklik akışı kullanılarak aşağıdaki hata kodları ve iletiler desteklenir:

* **HTTP hata kodu 429** - Değişiklik akışı hızı sınırlı olduğunda, boş bir sayfa döndürür.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Kaynak Yöneticisi şablonlarını kullanarak Azure Cosmos DB Cassandra API kaynaklarını yönetme](manage-cassandra-with-resource-manager.md)
