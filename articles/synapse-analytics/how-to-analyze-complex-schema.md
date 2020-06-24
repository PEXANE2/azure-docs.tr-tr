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
ms.openlocfilehash: b02c3627cea5e441739c77d1882505c6b82489bc
ms.sourcegitcommit: 34eb5e4d303800d3b31b00b361523ccd9eeff0ab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84908211"
---
# <a name="analyze-complex-data-types-in-synapse"></a>SYNAPSE 'de karmaşık veri türlerini çözümleme

Bu makale, **Azure Cosmos DB Için Azure SYNAPSE bağlantısı**'Ndaki Parquet dosyaları ve kapsayıcıları için geçerlidir. Kullanıcıların, diziler veya iç içe yapılar gibi karmaşık şemayla verileri okumak veya dönüştürmek için Spark veya SQL 'i nasıl kullanabileceğini açıklar. Aşağıdaki örnek tek bir belgeyle yapılır, ancak Spark veya SQL ile milyarlarca belgeye kolayca ölçeklendirebilir. Aşağıdaki kod PySpark (Python) kullanır.

## <a name="use-case"></a>Kullanım örneği

Modern veri türleriyle, karmaşık veri türleri genellikle işleme ve veri mühendislerinin bir sınamasını temsil etmek için ortaktır. İç içe şema ve diziler analiz edilirken bazı sorunlar var:
* SQL sorguları yazmak için karmaşık
* İç içe geçmiş sütunların veri türünü yeniden adlandırma/atama zor
* Derin iç içe geçmiş nesnelerde performans sorunlarını giderin

Veri mühendislerinin bu veri türlerini verimli bir şekilde nasıl işleyeceğini ve herkes tarafından kolayca erişilebilir hale getirme hakkında bilgi sahibi olmaları gerekir.

Aşağıdaki örnekte, SYNAPSE Spark, nesneleri veri çerçeveleri aracılığıyla düz bir yapıda okumak ve dönüştürmek için kullanılacaktır. SYNAPSE SQL sunucusuz, doğrudan bu nesneleri sorgulamak ve bu sonuçları normal tablo olarak döndürmek için kullanılır.

## <a name="what-are-arrays-and-nested-structures"></a>Diziler ve iç içe yerleştirilmiş yapılar nelerdir?

Aşağıdaki nesne, uygulama öngörüden geliyor. Bu nesnede, iç içe yapılar, ancak aynı zamanda iç içe yerleştirilmiş yapılar içeren diziler vardır.

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
Bu nesnenin veri çerçevesinin şemasını ( **df**olarak adlandırılır), **df. printschema**komutuyla yazdırırken aşağıdaki gösterimi görüyoruz:

* sarı renk, iç içe yapıyı temsil eder
* yeşil renk, iki öğe içeren bir diziyi temsil eder

[![Şema kaynağı](./media/how-to-complex-schema/schema-origin.png)](./media/how-to-complex-schema/schema-origin.png#lightbox)

_rid, _ts ve _etag, belge Azure Cosmos DB işlemsel depoya alındığı için sisteme eklenmiştir.

Yukarıdaki veri çerçevesi yalnızca 5 sütun ve 1 satır için sayılır. Dönüşümden sonra, seçkin veri çerçevesinin tablolu biçimde 13 sütunu ve 2 satırı olacaktır.

## <a name="flatten-nested-structures-and-explode-arrays-with-apache-spark"></a>Apache Spark ile iç içe yapıları düzleştirme ve dizileri açın

SYNAPSE Spark ile, iç içe yapıları sütunlar ve dizi öğelerine birden çok satıra dönüştürmek kolaydır. Aşağıdaki adımlar herkes tarafından kendi uygulamaları için kullanılabilir.

[![Spark dönüştürmeleri adımları](./media/how-to-complex-schema/spark-transfo-steps.png)](./media/how-to-complex-schema/spark-transfo-steps.png#lightbox)

**1. adım**: iç içe şemayı düzleştirmek için bir işlev tanımladık. Bu işlev, değişiklik yapılmadan kullanılabilir. Bir Pyspark not defterinde bu işlevle bir hücre oluşturun:

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

**2. adım**: bir **veri çerçevesinin iç** içe şemasını yeni bir veri çerçevesine düzleştirmek için işlevini kullanın **df_flat**:

```python
from pyspark.sql.types import StringType, StructField, StructType
df_flat = flatten_df(df)
display(df_flat.limit(10))
```

Görüntüleme işlevi 10 sütun ve 1 satır döndürmelidir. Dizi ve iç içe yerleştirilmiş öğeleri hala orada.

**3. adım**: artık veri çerçevesindeki dizi **context_custom_dimensions** **df_flat** yeni bir veri çerçevesi **df_flat_explode**dönüştürüyoruz. Aşağıdaki kodda, hangi sütunu seçtireceğiz de tanımlayacağız:

```python
from pyspark.sql.functions import explode
from pyspark.sql.functions import flatten
from pyspark.sql.functions import arrays_zip
df_flat_explode = df_flat.select("_rid","_ts","id","_etag",explode(df_flat.context_custom_dimensions),"context_session_isFirst","context_session_id","context_data_eventTime","context_data_samplingRate","context_data_isSynthetic")\
.select("_rid","_ts","id","_etag","col.*","context_session_isFirst","context_session_id","context_data_eventTime","context_data_samplingRate","context_data_isSynthetic")
display(df_flat_explode.limit(10))

```

Görüntüleme işlevi şu sonucu döndürmelidir: 10 sütun ve 2 satır. Sonraki adım, 1. adımda tanımlanan işlevle iç içe şemaları düzleştirmeniz olur.

**4. adım**: **df_flat_explode** veri çerçevesinin iç içe şemasını, **df_flat_explode_flat**yeni bir veri çerçevesine düzleştirmek için kullanın:
```python
df_flat_explode_flat = flatten_df(df_flat_explode)
display(df_flat_explode_flat.limit(10))
```

Görüntüleme işlevi 13 sütun ve 2 satır göstermelidir:

Veri çerçevesinin printSchema işlevi df_flat_explode_flat aşağıdaki sonucu döndürür:

[![Şema son](./media/how-to-complex-schema/schema-final.png)](./media/how-to-complex-schema/schema-final.png#lightbox)

## <a name="read-arrays-and-nested-structures-directly-with-sql-serverless"></a>Dizileri ve iç içe yapıları doğrudan SQL sunucusuz ile okuyun

SQL sunucusuz ile bu tür nesneler üzerinde görünümler ve tablolar oluşturmak mümkündür.

İlki, verilerin nasıl depolandığına bağlı olarak, kullanıcıların aşağıdaki taksonomiyi kullanması gerekir. Her şey büyük harf kullanım örneğine özeldir:

| Döndür              | FORMAT |
| -------------------- | --- |
| 'https://ACCOUNTNAME.dfs.core.windows.net/FILESYSTEM/PATH/FINENAME.parquet' |' Parquet ' (ADLSg2)|
| N'endpoint = https://ACCOUNTNAME.documents-staging.windows-ppe.net:443/ ; Account = ACCOUNTNAME; Database = DatabaseName; Collection = COLLECTIONNAME; Region = REGIONTOQUERY, Secret = ' yoursecret ' |' CosmosDB ' (SYNAPSE bağlantısı)|



**SQL sunucusuz** , Azure Cosmos DB ve AAD geçişi Için Azure SYNAPSE bağlantısı bağlantılı hizmetini destekleyecektir. Bu özellik şu anda SYNAPSE bağlantısı için geçitli önizleme aşamasındadır.

Aşağıdan değiştirin:
* Bağlandığınız veri kaynağının bağlantı dizesi tarafından ' toplu
* Kaynağa bağlanmak için kullandığınız biçime göre ' YUKARıYA YAZıN '

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

İki farklı türde işlem yapılır:
* Aşağıdaki kod satırı, iç içe geçmiş öğeye başvuran contextdataeventTime adlı sütunu tanımlar: Context. Data. eventTime
```sql
contextdataeventTime varchar(50) '$.context.data.eventTime'
```

Bu satır, iç içe öğesine başvuran contextdataeventTime adlı sütunu tanımlar: bağlam>verileri>eventTime

* **Çapraz uygulama** , dizi altındaki her öğe için yeni satırlar oluşturmak için kullanılır ve ardından, iç içe geçmiş her nesneyi ilk madde işaretine benzer şekilde tanımlar: 
```sql
cross apply openjson (contextcustomdimensions) 
with ( ProfileType varchar(50) '$.customerInfo.ProfileType', 
```

Dizide 4 iç içe yapı içeren 5 öğe varsa, SQL sunucusuz 5 satır ve 4 sütun döndürür.

SQL sunucusuz, yerinde sorgulama yapabilir, diziyi 2 satır içinde eşleyebilir ve tüm iç içe yapıları sütunlar halinde görüntüler.

## <a name="next-steps"></a>Sonraki adımlar

* [Spark ile Azure Cosmos DB için Azure SYNAPSE bağlantısını sorgulama hakkında bilgi edinin](./synapse-link/how-to-query-analytical-store-spark.md)
* [Sorgu Parquet iç içe türler](./sql/query-parquet-nested-types.md) 