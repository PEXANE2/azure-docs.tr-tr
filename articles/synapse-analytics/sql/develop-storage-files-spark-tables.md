---
title: İsteğe bağlı SQL kullanarak Spark tablolarını sorgula (önizleme)
description: İsteğe bağlı SQL kullanarak Spark tablolarının nasıl sorgulanacaklarına genel bakış (önizleme)
services: synapse-analytics
author: julieMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: 41e31a322a3d771557474fdf5c318960822bcfe1
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424051"
---
# <a name="query-spark-tables-with-azure-synapse-analytics-using-sql-on-demand-preview"></a>SQL isteğe bağlı (önizleme) kullanarak Azure Synapse Analytics ile Spark tablolarını sorgula

İsteğe bağlı SQL (önizleme), Synapse çalışma alanı (önizleme) içindeki Spark havuzlarından gelen meta verileri otomatik olarak senkronize edebilir. Spark havuzlarında bulunan her veritabanı için isteğe bağlı bir veritabanı oluşturulur (önizleme). Parke veya CSV'yi temel alan her Kıvılcım tablosu için SQL isteğe bağlı veritabanında harici bir tablo oluşturulur. Bu nedenle, Spark havuzlarınızı kapatabilir ve spark tablolarını isteğe bağlı OLARAK SQL'den sorgulayabilirsiniz.

Bir tablo Spark'ta bölümlendiğinde, depolama daki dosyalar klasörlere göre düzenlenir. İsteğe bağlı SQL, bölüm meta verilerini kullanır ve yalnızca sorgunuz için ilgili klasörleri ve dosyaları hedefler.

Meta veri eşitleme, Azure Synapse çalışma alanında sağlanan her Kıvılcım havuzu için otomatik olarak yapılandırılır. Spark tablolarını anında sorgulamaya başlayabilirsiniz.

Her Kıvılcım tablosu, bir SQL isteğe bağlı veritabanına karşılık gelen bir dbo şemasında harici bir tabloyla temsil edilir. Spark tablo sorguları için, dış [spark_table] hedefleyen bir sorgu çalıştırın. Aşağıdaki örneği çalıştırmadan önce, dosyaların bulunduğu [depolama hesabına](develop-storage-files-storage-access-control.md) doğru erişime sahip olduğundan emin olun.

```sql
SELECT * FROM [db].dbo.[spark_table]
```

## <a name="spark-data-types-to-sql-data-types-mapping"></a>SQL veri türleri eşleme için veri türlerini kıvılcım

| Veri türünü kıvılcımla | SQL veri türü               |
| --------------- | --------------------------- |
| ByteType        | smallint                    |
| Kısa Tip       | smallint                    |
| IntegerType     | int                         |
| Uzun Yazı        | bigint                      |
| FloatType       | gerçek                        |
| DoubleType      | float                       |
| Ondalık Yazı     | decimal                     |
| Zaman Damgası Türü   | datetime2                   |
| Datetype        | date                        |
| StringType      | varchar(maks)*               |
| İkili Tip      | Varbinary                   |
| BooleanTipi     | bit                         |
| ArrayType       | varchar(max)* (JSON içine)** |
| MapType         | varchar(max)* (JSON içine)** |
| StructType      | varchar(max)* (JSON içine)** |

\*Kullanılan harmanlama Latin1_General_100_BIN2_UTF8.

** ArrayType, MapType ve StructType JSONs olarak temsil edilir.



## <a name="next-steps"></a>Sonraki adımlar

Depolama erişim denetimi hakkında daha fazla bilgi edinmek için [Depolama Erişim Denetimi](develop-storage-files-storage-access-control.md) makalesine ilerleyin.
