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
ms.openlocfilehash: 41e31a322a3d771557474fdf5c318960822bcfe1
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81424051"
---
# <a name="query-spark-tables-with-azure-synapse-analytics-using-sql-on-demand-preview"></a>İsteğe bağlı SQL (Önizleme) kullanarak Azure SYNAPSE Analytics ile Spark tabloları sorgulama

İsteğe bağlı SQL (Önizleme), SYNAPSE çalışma alanı (Önizleme) içindeki Spark havuzlarından meta verileri otomatik olarak eşitleyebilir. Spark havuzlarında (Önizleme) bulunan her veritabanı için bir SQL isteğe bağlı veritabanı oluşturulur. Parquet veya CSV tabanlı her Spark tablosu için, SQL isteğe bağlı veritabanında bir dış tablo oluşturulur. Bu nedenle, Spark havuzlarınızı kapatabilir ve hâlâ SQL isteğe bağlı Spark tablolarını sorgulayabilirsiniz.

Bir tablo Spark 'ta bölümlendiğinde, depolama alanındaki dosyalar klasörlere göre düzenlenir. İsteğe bağlı SQL, Bölüm meta verilerini kullanır ve yalnızca sorgunuz için ilgili klasörleri ve dosyaları hedefleyin.

Meta veri eşitleme, Azure SYNAPSE çalışma alanında sağlanan her Spark havuzu için otomatik olarak yapılandırılır. Spark tabloları anında sorgulamaya başlayabilirsiniz.

Her Spark tablosu, bir SQL isteğe bağlı veritabanına karşılık gelen bir dbo şemasında dış tablo ile temsil edilir. Spark tablo sorguları için, dış [spark_table] hedefleyen bir sorgu çalıştırın. Aşağıdaki örneği çalıştırmadan önce, dosyaların bulunduğu [depolama hesabına doğru erişiminizin](develop-storage-files-storage-access-control.md) olduğundan emin olun.

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
| Tarih türü        | date                        |
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
