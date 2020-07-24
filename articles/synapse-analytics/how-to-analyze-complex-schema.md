---
title: Şemayı diziler ve iç içe yapılar ile analiz etme
description: Apache Spark ve SQL ile dizileri ve iç içe yapıları çözümleme
services: synapse-analytics
author: ArnoMicrosoft
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: ''
ms.date: 06/15/2020
ms.author: acomet
ms.reviewer: jrasnick
ms.openlocfilehash: ad6761466cc958235557609e929e641a0311ee43
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86999208"
---
# <a name="analyze-complex-data-types-in-azure-synapse-analytics"></a>Azure SYNAPSE Analytics 'te karmaşık veri türlerini çözümleme

Bu makale, [Azure Cosmos DB Için SYNAPSE link](.\synapse-link\how-to-connect-synapse-link-cosmos-db.md)Içindeki Parquet dosyaları ve kapsayıcıları için geçerlidir. Kullanıcıların, diziler veya iç içe yapılar gibi karmaşık şemalarla veri okumak veya dönüştürmek için Spark veya SQL 'i nasıl kullanabileceğini açıklar. Aşağıdaki örnek tek bir belgeyle tamamlandı, ancak Spark veya SQL ile milyarlarca belgeye kolayca ölçeklendirebilirler. Bu makalede yer alan kod PySpark (Python) kullanır.

## <a name="use-case"></a>Kullanım Örneği

Karmaşık veri türleri giderek yaygındır ve iç içe geçmiş şemayı analiz etme ve diziler zaman alıcı ve karmaşık SQL sorguları dahil olmak üzere, veri mühendislerinin bir sınamasını temsil etmektedir. Ayrıca, iç içe geçmiş sütunlar veri türünü yeniden adlandırmak veya dönüştürmek zor olabilir. Ayrıca, derin iç içe geçmiş nesnelerle çalışırken performans sorunları ortaya çıkar.

Veri mühendislerinin karmaşık veri türlerini verimli bir şekilde nasıl işleyeceğini ve herkesin kolayca erişilebilir hale getirme işlemini anlaması gerekir.

Aşağıdaki örnekte, SYNAPSE Spark, nesneleri veri çerçeveleri aracılığıyla düz bir yapıya okumak ve dönüştürmek için kullanılır. SYNAPSE SQL sunucusuz, bu nesneleri doğrudan sorgulamak ve bu sonuçları normal tablo olarak döndürmek için kullanılır.

## <a name="what-are-arrays-and-nested-structures"></a>Diziler ve iç içe yerleştirilmiş yapılar nelerdir?

Aşağıdaki nesne, [uygulama öngörüden](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview)geliyor. Bu nesnede, iç içe yapılar içeren iç içe yapılar ve diziler vardır.

```json
{
    "id": "66532691-ab20-11ea-8b1d-936b3ec64e54",
    "context": {
        "data": {
            "eventTime": "2020-06-10T13:43:34.553Z",
            "samplingRate": "100.0",
            "isSynthetic": "false"
        },
        "session": {
            "isFirst": "false",
            "id": "38619c14-7a23-4687-8268-95862c5326b1"
        },
        "custom": {
            "dimensions": [
                {
                    "customerInfo": {
                        "ProfileType": "ExpertUser",
                        "RoomName": "",
                        "CustomerName": "diamond",
                        "UserName": "XXXX@yahoo.com"
                    }
                },
                {
                    "customerInfo": {
                        "ProfileType": "Novice",
                        "RoomName": "",
                        "CustomerName": "topaz",
                        "UserName": "XXXX@outlook.com"
                    }
                }
            ]
        }
    }
}
```

### <a name="schema-example-of-arrays-and-nested-structures"></a>Diziler ve iç içe yapılar için şema örneği
Komutuyla nesnenin veri çerçevesinin ( **df**olarak adlandırılır) şemasını yazdırırken `df.printschema` aşağıdaki gösterimi görüyoruz:

* Sarı renk, iç içe yapıyı temsil eder
* Yeşil renk, iki öğe içeren bir diziyi temsil eder

[![Şema kaynağı](./media/how-to-complex-schema/schema-origin.png)](./media/how-to-complex-schema/schema-origin.png#lightbox)

**_rid**, **_ts**ve **_etag** , belge Azure Cosmos DB işlemsel depoya alındığı için sisteme eklenmiştir.

Yukarıdaki veri çerçevesi yalnızca 5 sütun ve 1 satır için sayılır. Dönüşümden sonra, seçkin veri çerçevesinin tablolu biçimde 13 sütunu ve 2 satırı olacaktır.

## <a name="flatten-nested-structures-and-explode-arrays-with-apache-spark"></a>Apache Spark ile iç içe yapıları düzleştirme ve dizileri açın

SYNAPSE Spark ile, iç içe yapıları sütunlar ve dizi öğelerine birden çok satıra dönüştürmek kolaydır. Aşağıdaki adımlar uygulama için kullanılabilir.

[![Spark dönüştürmeleri adımları](./media/how-to-complex-schema/spark-transform-steps.png)](./media/how-to-complex-schema/spark-transform-steps.png#lightbox)

**1. adım**: iç içe şemayı düzleştirmek için bir işlev tanımladık. Bu işlev, değişiklik yapılmadan kullanılabilir. [Pyspark not defterinde](quickstart-apache-spark-notebook.md) aşağıdaki işlevle bir hücre oluşturun:

```python
from pyspark.sql.functions import col

def flatten_df(nested_df):
    stack = [((), nested_df)]
    columns = []

    while len(stack) > 0:
        parents, df = stack.pop()

        flat_cols = [
            col(".".join(parents + (c[0],))).alias("_".join(parents + (c[0],)))
            for c in df.dtypes
            if c[1][:6] != "struct"
        ]

        nested_cols = [
            c[0]
            for c in df.dtypes
            if c[1][:6] == "struct"
        ]

        columns.extend(flat_cols)

        for nested_col in nested_cols:
            projected_df = df.select(nested_col + ".*")
            stack.append((parents + (nested_col,), projected_df))

    return nested_df.select(columns)
```

**2. adım**: veri çerçevesinin (**df**) iç içe şemasını yeni bir veri çerçevesine düzleştirmek için işlevini kullanın `df_flat` :

```python
from pyspark.sql.types import StringType, StructField, StructType
df_flat = flatten_df(df)
display(df_flat.limit(10))
```

Görüntüleme işlevi 10 sütun ve 1 satır döndürmelidir. Dizi ve iç içe yerleştirilmiş öğeleri hala orada.

**3. adım**: `context_custom_dimensions` veri çerçevesindeki diziyi `df_flat` Yeni bir veri çerçevesine dönüştürme `df_flat_explode` . Aşağıdaki kodda seçilecek sütunu da tanımlayacağız:

```python
from pyspark.sql.functions import explode
from pyspark.sql.functions import flatten
from pyspark.sql.functions import arrays_zip
df_flat_explode = df_flat.select("_rid","_ts","id","_etag",explode(df_flat.context_custom_dimensions),"context_session_isFirst","context_session_id","context_data_eventTime","context_data_samplingRate","context_data_isSynthetic")\
.select("_rid","_ts","id","_etag","col.*","context_session_isFirst","context_session_id","context_data_eventTime","context_data_samplingRate","context_data_isSynthetic")
display(df_flat_explode.limit(10))

```

Görüntüleme işlevi 10 sütun ve 2 satır döndürmelidir. Sonraki adım, 1. adımda tanımlanan işlevle iç içe şemaları düzleştirmeniz olur.

**4. adım**: veri çerçevesinin iç içe şemasını `df_flat_explode` Yeni bir veri çerçevesine düzleştirmek için işlevini kullanın `df_flat_explode_flat` :
```python
df_flat_explode_flat = flatten_df(df_flat_explode)
display(df_flat_explode_flat.limit(10))
```

Görüntüleme işlevi 13 sütun ve 2 satır göstermelidir.

`printSchema`Veri çerçevesinin işlevi `df_flat_explode_flat` aşağıdaki sonucu verir:

[![Şema son](./media/how-to-complex-schema/schema-final.png)](./media/how-to-complex-schema/schema-final.png#lightbox)

## <a name="read-arrays-and-nested-structures-directly-with-sql-serverless"></a>Dizileri ve iç içe yapıları doğrudan SQL sunucusuz ile okuyun

SQL sunucusuz ile bu tür nesneler üzerinde görünümleri ve tabloları sorgulama ve oluşturma olasılığı vardır.

Birincisi, verilerin nasıl depolandığına bağlı olarak, kullanıcılar aşağıdaki taksonomiyi kullanmalıdır. BÜYÜK harfle gösterilen her şey kullanım örneğine özeldir:

| Döndür              | FORMAT |
| -------------------- | --- |
| 'https://ACCOUNTNAME.dfs.core.windows.net/FILESYSTEM/PATH/FINENAME.parquet' |' Parquet ' (ADLSg2)|
| N'endpoint = https://ACCOUNTNAME.documents-staging.windows-ppe.net:443/ ; Account = ACCOUNTNAME; Database = DatabaseName; Collection = COLLECTIONNAME; Region = REGIONTOQUERY, Secret = ' yoursecret ' |' CosmosDB ' (SYNAPSE bağlantısı)|


> [!NOTE]
> SQL sunucusuz, Azure Cosmos ve AAD PASSTHROUGH için SYNAPSE bağlantısı için bağlı hizmeti destekleyecektir. Bu özellik şu anda SYNAPSE bağlantısı için geçitli önizleme aşamasındadır.

Her bir alanı aşağıdaki gibi değiştirin:
* ' Toplu bir sürüm ' = bağlandığınız veri kaynağının bağlantı dizesi
* ' YUKARıDAKI tür ' = kaynağa bağlanmak için kullandığınız biçim

```sql
select *
FROM
openrowset(
BULK 'YOUR BULK ABOVE',
            FORMAT='YOUR TYPE ABOVE'
)
with (id varchar(50),
        contextdataeventTime varchar(50) '$.context.data.eventTime',
        contextdatasamplingRate varchar(50) '$.context.data.samplingRate',
        contextdataisSynthetic varchar(50) '$.context.data.isSynthetic',
        contextsessionisFirst varchar(50) '$.context.session.isFirst',
        contextsessionid varchar(50) '$.context.session.id',
        contextcustomdimensions varchar(max) '$.context.custom.dimensions'
) as q 
cross apply openjson (contextcustomdimensions) 
with ( ProfileType varchar(50) '$.customerInfo.ProfileType',
            RoomName varchar(50) '$.customerInfo.RoomName',
            CustomerName varchar(50) '$.customerInfo.CustomerName',
            UserName varchar(50) '$.customerInfo.UserName'
    )
```

İki farklı türde işlem vardır:

İlk işlem türü, iç içe geçmiş öğeye başvuran adlı sütunu tanımlayan aşağıdaki kod satırında belirtilir `contextdataeventTime` : Context. Data. eventTime 
```sql
contextdataeventTime varchar(50) '$.context.data.eventTime'
```

Bu satır, iç içe öğesine başvuran contextdataeventTime adlı sütunu tanımlar: bağlam>verileri>eventTime

İkinci işlem türü, `cross apply` dizi altındaki her öğe için yeni satırlar oluşturmak için kullanır ve ardından, iç içe geçmiş her nesneyi ilk madde işaretine benzer şekilde tanımlar: 
```sql
cross apply openjson (contextcustomdimensions) 
with ( ProfileType varchar(50) '$.customerInfo.ProfileType', 
```

Dizide 4 iç içe yapı içeren 5 öğe varsa, SQL sunucusuz 5 satır ve 4 sütun döndürür. SQL sunucusuz, yerinde sorgulama yapabilir, diziyi 2 satır içinde eşleyebilir ve tüm iç içe yapıları sütunlar halinde görüntüler.

## <a name="next-steps"></a>Sonraki adımlar

* [Spark ile Azure Cosmos DB için SYNAPSE bağlantısını sorgulama hakkında bilgi edinin](./synapse-link/how-to-query-analytical-store-spark.md)
* [Sorgu Parquet iç içe türler](./sql/query-parquet-nested-types.md) 