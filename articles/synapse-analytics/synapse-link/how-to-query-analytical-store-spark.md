---
title: Apache Spark ile analitik depo (Önizleme) Sorgula Azure Cosmos DB
description: Azure SYNAPSE Analytics için Apache Spark ile analitik Azure Cosmos DB sorgulama
services: synapse-analytics
author: ArnoMicrosoft
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: synapse-link
ms.date: 05/06/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: a7ee04c922e4373414dc27ed2b7c98be605280e5
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87089116"
---
# <a name="query-azure-cosmos-db-analytical-store-preview-with-apache-spark-for-azure-synapse-analytics"></a>Azure SYNAPSE Analytics için Apache Spark analitik mağaza (Önizleme) Azure Cosmos DB sorgulama

Bu makalede, SYNAPSE hareketlerinden analitik depoyla nasıl etkileşime girebileceği hakkında örnekler verilmektedir. Hareketleri görmek için kapsayıcıya sağ tıklamanız gerekir. Hareketler sayesinde hızlı bir şekilde kod oluşturabilir ve ihtiyaçlarınıza göre düzenleyebilirsiniz. Hareketler ayrıca tek tıklamayla verileri keşfetmek için de idealdir.

## <a name="load-to-dataframe"></a>DataFrame 'e yükle

Bu adımda, bir Spark veri çerçevesinde Azure Cosmos DB analitik deposundan verileri okuyacaksınız. ***SD***adlı veri çerçevesinin 10 satırını görüntüler. Verileriniz dataframe 'e girdikten sonra ek analiz gerçekleştirebilirsiniz.

Bu işlem, işlem deposunu etkilemez.

```python
# To select a preferred list of regions in a multi-region Azure Cosmos DB account, add .option("spark.cosmos.preferredRegions", "<Region1>,<Region2>")

df = spark.read.format("cosmos.olap")\
    .option("spark.synapse.linkedService", "INFERRED")\
    .option("spark.cosmos.container", "INFERRED")\
    .load()

df.show(10)
```

**Scala** 'daki eşdeğer kod hareketi aşağıdaki kod olacaktır:
```java
// To select a preferred list of regions in a multi-region Azure Cosmos DB account, add option("spark.cosmos.preferredRegions", "<Region1>,<Region2>")

val df_olap = spark.read.format("cosmos.olap").
    option("spark.synapse.linkedService", "pySparkSamplesDb").
    option("spark.cosmos.container", "trafficSourceColl").
    load()
```

## <a name="create-spark-table"></a>Spark tablosu oluşturma

Bu hareket sırasında, seçtiğiniz kapsayıcıyı işaret eden bir Spark tablosu oluşturacaksınız. Bu işlem herhangi bir veri hareketine tabi değildir. Bu tabloyu silmeye karar verirseniz, temel alınan kapsayıcı (ve ilgili analitik depo) etkilenmez. 

Bu senaryo, üçüncü taraf araçlarla tabloları yeniden kullanmak ve çalışma zamanına ilişkin verilere erişilebilirlik sağlamak için uygundur.

```sql
%%sql
-- To select a preferred list of regions in a multi-region Azure Cosmos DB account, add spark.cosmos.preferredRegions '<Region1>,<Region2>' in the config options

create table call_center using cosmos.olap options (
    spark.synapse.linkedService 'INFERRED',
    spark.cosmos.container 'INFERRED'
)
```

## <a name="write-dataframe-to-container"></a>Veri çerçevesini kapsayıcıya yaz

Bu hareket halinde, bir kapsayıcıya bir veri çerçevesi yazacak. Bu işlem, işlem performansını etkiler ve Istek birimlerini tüketir. Azure Cosmos DB işlem performansı kullanmak yazma işlemleri için idealdir. **Veri çerçevesini** , geri yazmak istediğiniz dataframe ile değiştirdiğinizden emin olun.

```python
# Write a Spark DataFrame into an Azure Cosmos DB container
# To select a preferred list of regions in a multi-region Azure Cosmos DB account, add .option("spark.cosmos.preferredRegions", "<Region1>,<Region2>")


YOURDATAFRAME.write.format("cosmos.oltp")\
    .option("spark.synapse.linkedService", "INFERRED")\
    .option("spark.cosmos.container", "INFERRED")\
    .option("spark.cosmos.write.upsertEnabled", "true")\
    .mode('append')\
    .save()
```

**Scala** 'daki eşdeğer kod hareketi aşağıdaki kod olacaktır:
```java
// To select a preferred list of regions in a multi-region Azure Cosmos DB account, add option("spark.cosmos.preferredRegions", "<Region1>,<Region2>")

import org.apache.spark.sql.SaveMode

df.write.format("cosmos.oltp").
    option("spark.synapse.linkedService", "pySparkSamplesDb").
    option("spark.cosmos.container", "trafficSourceColl"). 
    option("spark.cosmos.write.upsertEnabled", "true").
    mode(SaveMode.Overwrite).
    save()
```

## <a name="load-streaming-dataframe-from-container"></a>Kapsayıcıdan akış veri çerçevesini yükleme
Bu hareket halinde, bir kapsayıcıdan veri çerçevesine veri yüklemek için Spark akış özelliğini kullanacaksınız. Veriler, çalışma alanına bağladığınız birincil Data Lake hesabında (ve dosya sisteminde) depolanır. 

*/Localreadcheckpointfolder* klasörü oluşturulmadıysa, otomatik olarak oluşturulur. Bu işlem, Azure Cosmos DB işlem performansını etkiler.

```python
# To select a preferred list of regions in a multi-region Azure Cosmos DB account, add .option("spark.cosmos.preferredRegions", "<Region1>,<Region2>")

dfStream = spark.readStream\
    .format("cosmos.oltp")\
    .option("spark.synapse.linkedService", "INFERRED")\
    .option("spark.cosmos.container", "INFERRED")\
    .option("spark.cosmos.changeFeed.readEnabled", "true")\
    .option("spark.cosmos.changeFeed.startFromTheBeginning", "true")\
    .option("spark.cosmos.changeFeed.checkpointLocation", "/localReadCheckpointFolder")\
    .option("spark.cosmos.changeFeed.queryName", "streamQuery")\
    .load()
```

**Scala** 'daki eşdeğer kod hareketi aşağıdaki kod olacaktır:
```java
// To select a preferred list of regions in a multi-region Azure Cosmos DB account, add .option("spark.cosmos.preferredRegions", "<Region1>,<Region2>")

val dfStream = spark.readStream.
    format("cosmos.oltp").
    option("spark.synapse.linkedService", "pySparkSamplesDb").
    option("spark.cosmos.container", "trafficSourceColl").
    option("spark.cosmos.changeFeed.readEnabled", "true").
    option("spark.cosmos.changeFeed.startFromTheBeginning", "true").
    option("spark.cosmos.changeFeed.checkpointLocation", "/localReadCheckpointFolder").
    option("spark.cosmos.changeFeed.queryName", "streamTestRevin2").
    load()
```

## <a name="write-streaming-dataframe-to-container"></a>Akış veri çerçevesini kapsayıcıya yaz
Bu hareket halinde, seçtiğiniz Azure Cosmos DB kapsayıcısına bir akış veri çerçevesi yazacaksınız. */Localreadcheckpointfolder* klasörü oluşturulmadıysa, otomatik olarak oluşturulur. Bu işlem, Azure Cosmos DB işlem performansını etkiler.

```python
# To select a preferred list of regions in a multi-region Azure Cosmos DB account, add .option("spark.cosmos.preferredRegions", "<Region1>,<Region2>")

streamQuery = dfStream\
        .writeStream\
        .format("cosmos.oltp")\
        .outputMode("append")\
        .option("checkpointLocation", "/localWriteCheckpointFolder")\
        .option("spark.synapse.linkedService", "INFERRED")\
        .option("spark.cosmos.container", "trafficSourceColl_sink")\
        .option("spark.cosmos.connection.mode", "gateway")\
        .start()

streamQuery.awaitTermination()
```

**Scala** 'daki eşdeğer kod hareketi aşağıdaki kod olacaktır:
```java
// To select a preferred list of regions in a multi-region Azure Cosmos DB account, add .option("spark.cosmos.preferredRegions", "<Region1>,<Region2>")

val query = dfStream.
            writeStream.
            format("cosmos.oltp").
            outputMode("append").
            option("checkpointLocation", "/localWriteCheckpointFolder").
            option("spark.synapse.linkedService", "pySparkSamplesDb").
            option("spark.cosmos.container", "test2").
            option("spark.cosmos.connection.mode", "gateway").
            start()

query.awaitTermination()
```
## <a name="next-steps"></a>Sonraki adımlar

* [SYNAPSE ve Azure Cosmos DB arasında nelerin desteklendiğini öğrenin](./concept-synapse-link-cosmos-db-support.md)
* [Azure Cosmos DB için SYNAPSE bağlantısına Bağlan](../quickstart-connect-synapse-link-cosmos-db.md)
