---
title: İsteğe bağlı SQL kullanarak Spark tablolarını sorgulama (Önizleme)
description: İsteğe bağlı SQL (Önizleme) kullanarak Spark tablolarının nasıl sorgulanmasına genel bakış
services: synapse-analytics
author: julieMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: 210e0b240eefd2dd3f8d1ac45c781959e47ab893
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83198486"
---
# <a name="query-spark-tables-with-azure-synapse-analytics-using-sql-on-demand-preview"></a>İsteğe bağlı SQL (Önizleme) kullanarak Azure SYNAPSE Analytics ile Spark tabloları sorgulama

İsteğe bağlı SQL (Önizleme), SYNAPSE çalışma alanı (Önizleme) içindeki Spark havuzlarından meta verileri otomatik olarak eşitleyebilir. Spark havuzlarında (Önizleme) bulunan her veritabanı için bir SQL isteğe bağlı veritabanı oluşturulur. Parquet tabanlı ve Azure depolamada bulunan her Spark dış tablosu için, SQL isteğe bağlı veritabanında bir dış tablo oluşturulur. Bu nedenle Spark havuzlarınızı kapatabilir ve yine de SQL isteğe bağlı olarak Spark dış tablolarını sorgulayabilirsiniz.

Bir tablo Spark 'ta bölümlendiğinde, depolama alanındaki dosyalar klasörlere göre düzenlenir. İsteğe bağlı SQL, Bölüm meta verilerini kullanır ve yalnızca sorgunuz için ilgili klasörleri ve dosyaları hedefleyin.

Meta veri eşitleme, Azure SYNAPSE çalışma alanında sağlanan her Spark havuzu için otomatik olarak yapılandırılır. Spark dış tablolarını anında sorgulamaya başlayabilirsiniz.

Azure depolama 'da bulunan her Spark Parquet dış tablosu, bir SQL isteğe bağlı veritabanına karşılık gelen bir dbo şemasında dış tablo ile temsil edilir. Spark dış tablo sorguları için, dış [spark_table] hedefleyen bir sorgu çalıştırın. Aşağıdaki örneği çalıştırmadan önce, dosyaların bulunduğu [depolama hesabına doğru erişiminizin](develop-storage-files-storage-access-control.md) olduğundan emin olun.

```sql
SELECT * FROM [db].dbo.[spark_table]
```

## <a name="spark-data-types-to-sql-data-types-mapping"></a>SQL veri türleri eşlemesinde Spark veri türleri

| Spark veri türü | SQL veri türü               |
| --------------- | --------------------------- |
| ByteType        | smallint                    |
| ShortType       | smallint                    |
| IntegerType     | int                         |
| LongType        | bigint                      |
| FloatType       | real                        |
| DoubleType      | float                       |
| DecimalType     | decimal                     |
| TimestampType   | datetime2                   |
| Tarih türü        | tarih                        |
| StringType      | varchar (max) *               |
| BinaryType      | ikili                   |
| BooleanType     | bit                         |
| ArrayType       | varchar (max) * (JSON 'a) * * |
| MapType         | varchar (max) * (JSON 'a) * * |
| StructType      | varchar (max) * (JSON 'a) * * |

\*Kullanılan harmanlama Latin1_General_100_BIN2_UTF8.

* * ArrayType, MapType ve StructType, Jdönemleri olarak temsil edilir.



## <a name="next-steps"></a>Sonraki adımlar

Depolama erişim denetimi hakkında daha fazla bilgi edinmek için [depolama Access Control](develop-storage-files-storage-access-control.md) makalesine ilerleyin.
