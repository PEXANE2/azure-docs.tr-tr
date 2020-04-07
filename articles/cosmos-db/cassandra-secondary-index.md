---
title: Azure Cosmos DB Cassandra API hesabında dizin oluşturma
description: Azure Azure Cosmos DB Cassandra API hesabında ikincil dizin oluşturmanın nasıl çalıştığını öğrenin.
author: TheovanKraay
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/04/2020
ms.author: thvankra
ms.reviewer: sngun
ms.openlocfilehash: 7de38097acdbfa1f9c9b90f3051c68dec5465b32
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80758033"
---
# <a name="secondary-indexing-in-azure-cosmos-db-cassandra-api"></a>Azure Cosmos DB Cassandra API'de ikincil dizin oluşturma

Azure Cosmos DB'deki Cassandra API, platformun doğasında bulunan dizin oluşturma gücünü ortaya çıkarmak için temel dizin oluşturma altyapısından yararlanır. Ancak, temel SQL API'sının aksine, Azure Cosmos DB'deki Cassandra API varsayılan olarak tüm öznitelikleri dizine almaz. Bunun yerine, Apache Cassandra ile aynı şekilde davranan belirli öznitelikler üzerinde bir dizin oluşturmak için ikincil dizin oluşturmayı destekler.  

Genel olarak, bölümlenmemiş sütunlarda filtre sorguları yürütmek için tavsiye edilmez. Allow FILTERING sözdizimini açıkça kullanmanız gerekir, bu da iyi performans göstermeyebilecek bir işlemle sonuçlanır. Azure Cosmos DB'de, sonuçları almak için bölümler arasında silik çıkış yaptıkları için bu tür sorguları düşük ciddiyet özellikleriyle çalıştırabilirsiniz.

Sık güncelleştirilen bir sütunda dizin oluşturman tavsiye edilmez. Tabloyu tanımlarken bir dizin oluşturmak ihtiyatlı dır. Bu, verilerin ve dizinlerin tutarlı bir durumda olmasını sağlar. Varolan verilerde yeni bir dizin oluşturursanız, şu anda tablo için dizin ilerleme değişikliğini izleyebilirsiniz. Bu işlem için ilerlemeyi izlemeniz gerekiyorsa, [bir destek bileti]( https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)aracılığıyla ilerleme değişikliğini talep etseniz.


> [!NOTE]
> İkincil dizin aşağıdaki nesnelerde desteklenmez:
> - dondurulmuş toplama türleri, ondalık ve varyant türleri gibi veri türleri.
> - Statik sütunlar
> - Kümeleme tuşları

## <a name="indexing-example"></a>Dizin oluşturma örneği

İlk olarak, CQL kabuk istemi üzerinde aşağıdaki komutları çalıştırarak örnek bir keyspace ve tablo oluşturun:

```shell
CREATE KEYSPACE sampleks WITH REPLICATION = {'class' : 'SimpleStrategy'};
CREATE TABLE sampleks.t1(user_id int PRIMARY KEY, lastname text) WITH cosmosdb_provisioned_throughput=400;
``` 

Ardından, örnek kullanıcı verilerini aşağıdaki komutlarla ekleyin:

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

Aşağıdaki ifadeyi yürütmeyi denerseniz, kullanmanızı `ALLOW FILTERING`isteyen bir hatayla karşınıza çıkacaktır: 

```shell
select user_id, lastname from sampleks.t1 where lastname='nishu';
``` 

Cassandra API, önceki bölümde belirtildiği gibi, ALLOW FILTERING'i desteklese de, önerilmez. Bunun yerine aşağıdaki örnekte gösterildiği gibi bir dizin oluşturmanız gerekir:

```shell
CREATE INDEX ON sampleks.t1 (lastname);
```
"Soyadı" alanında bir dizin oluşturduktan sonra, artık önceki sorguyu başarıyla çalıştırabilirsiniz. Azure Cosmos DB'deki Cassandra API ile dizin adı sağlamanız gerekmez. Biçimli `tablename_columnname_idx` varsayılan dizin kullanılır. Örneğin, ` t1_lastname_idx` önceki tablonun dizin adıdır.

## <a name="dropping-the-index"></a>Dizini düşürme 
Dizin bırakmak için dizin adının ne olduğunu bilmeniz gerekir. Tablonuzun açıklamasını almak için `desc schema` komutu çalıştırın. Bu komutun çıktısı, dizin `CREATE INDEX tablename_columnname_idx ON keyspacename.tablename(columnname)`adını biçimde içerir. Daha sonra dizin adını aşağıdaki örnekte gösterildiği gibi dizin bırakmak için kullanabilirsiniz:

```shell
drop index sampleks.t1_lastname_idx;
```

## <a name="next-steps"></a>Sonraki adımlar
* Azure Cosmos DB'de [otomatik dizin oluşturmanın](index-overview.md) nasıl çalıştığını öğrenin
* [Azure Cosmos DB Cassandra API'si tarafından desteklenen Apache Cassandra özellikleri](cassandra-support.md)
