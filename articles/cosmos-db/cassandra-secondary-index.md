---
title: Azure Cosmos DB Cassandra API hesabında dizin oluşturma
description: Azure Azure Cosmos DB Cassandra API hesabında ikincil dizin oluşturma işleminin nasıl çalıştığını öğrenin.
author: TheovanKraay
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/04/2020
ms.author: thvankra
ms.reviewer: sngun
ms.openlocfilehash: 7de38097acdbfa1f9c9b90f3051c68dec5465b32
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80758033"
---
# <a name="secondary-indexing-in-azure-cosmos-db-cassandra-api"></a>Azure Cosmos DB Cassandra API ikincil dizin oluşturma

Azure Cosmos DB Cassandra API, platformda devralınan dizin oluşturma gücünü göstermek için temel dizin oluşturma altyapısını kullanır. Ancak, çekirdek SQL API 'sinin aksine, Azure Cosmos DB Cassandra API varsayılan olarak tüm öznitelikleri dizine eklemez. Bunun yerine, Apache Cassandra ile aynı şekilde davranan belirli özniteliklerde bir dizin oluşturmak için ikincil dizin oluşturmayı destekler.  

Genel olarak, bölümlenmiş sütunlarda filtre sorgularının yürütülmesi önerilmez. FILTRELEME sözdizimini açık olarak kullanmanız gerekir, bu da iyi gerçekleştiremeyebilir bir işleme neden olur. Azure Cosmos DB, sonuçları almak için bölümler arasında gezindiklerinden, bu tür sorguları düşük kardinalite öznitelikleri üzerinde çalıştırabilirsiniz.

Sık güncellenen bir sütunda dizin oluşturmanız önerilmez. Tabloyu tanımlarken bir dizin oluşturmak, akıllıca olur. Bu, verilerin ve dizinlerin tutarlı bir durumda olmasını sağlar. Mevcut verilerde yeni bir dizin oluşturmanız durumunda şu anda tablo için Dizin ilerleme durumunu izleyemezsiniz. Bu işlemin ilerlemesini izlemeniz gerekiyorsa, bir [destek bileti]( https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)aracılığıyla ilerleme değişikliğini istemeniz gerekir.


> [!NOTE]
> Aşağıdaki nesnelerde ikincil dizin desteklenmiyor:
> - Dondurulmuş koleksiyon türleri, ondalık ve değişken türleri gibi veri türleri.
> - Statik sütunlar
> - Kümeleme anahtarları

## <a name="indexing-example"></a>Dizin oluşturma örneği

İlk olarak, CQL kabuğu isteminde aşağıdaki komutları çalıştırarak örnek bir anahtar alanı ve tablo oluşturun:

```shell
CREATE KEYSPACE sampleks WITH REPLICATION = {'class' : 'SimpleStrategy'};
CREATE TABLE sampleks.t1(user_id int PRIMARY KEY, lastname text) WITH cosmosdb_provisioned_throughput=400;
``` 

Ardından, aşağıdaki komutlarla örnek Kullanıcı verileri ekleyin:

```shell
insert into sampleks.t1(user_id,lastname) values (1, 'nishu');
insert into sampleks.t1(user_id,lastname) values (2, 'vinod');
insert into sampleks.t1(user_id,lastname) values (3, 'bat');
insert into sampleks.t1(user_id,lastname) values (5, 'vivek');
insert into sampleks.t1(user_id,lastname) values (6, 'siddhesh');
insert into sampleks.t1(user_id,lastname) values (7, 'akash');
insert into sampleks.t1(user_id,lastname) values (8, 'Theo');
insert into sampleks.t1(user_id,lastname) values (9, 'jagan');
```

Aşağıdaki ifadeyi yürütmeyi denerseniz, şunu kullanmanızı isteyen bir hata ile karşılaşacaktır `ALLOW FILTERING` : 

```shell
select user_id, lastname from sampleks.t1 where lastname='nishu';
``` 

Cassandra API, önceki bölümde belirtildiği gibi FILTRELEMEYE izin vermeyi desteklese de önerilmez. Bunun yerine, aşağıdaki örnekte gösterildiği gibi bir dizin oluşturmanız gerekir:

```shell
CREATE INDEX ON sampleks.t1 (lastname);
```
"LastName" alanında bir dizin oluşturduktan sonra, artık önceki sorguyu başarıyla çalıştırabilirsiniz. Azure Cosmos DB Cassandra API, bir dizin adı sağlamanız gerekmez. Biçim içeren bir varsayılan dizin `tablename_columnname_idx` kullanılır. Örneğin, ` t1_lastname_idx` Önceki tablonun Dizin adıdır.

## <a name="dropping-the-index"></a>Dizin bırakılıyor 
Dizin adının dizini bırakma ne olduğunu bilmeniz gerekir. `desc schema`Tablonuzun açıklamasını almak için komutunu çalıştırın. Bu komutun çıktısı dizin adını biçiminde içerir `CREATE INDEX tablename_columnname_idx ON keyspacename.tablename(columnname)` . Ardından, aşağıdaki örnekte gösterildiği gibi dizin adını kullanarak dizini bırakabilirsiniz:

```shell
drop index sampleks.t1_lastname_idx;
```

## <a name="next-steps"></a>Sonraki adımlar
* [Otomatik Dizin oluşturmanın](index-overview.md) Azure Cosmos db içinde nasıl çalıştığını öğrenin
* [Azure Cosmos DB Cassandra API'si tarafından desteklenen Apache Cassandra özellikleri](cassandra-support.md)
