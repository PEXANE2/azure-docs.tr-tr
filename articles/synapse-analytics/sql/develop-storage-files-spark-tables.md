---
title: Sunucusuz SQL havuzundaki dış tablo tanımlarına yönelik Apache Spark eşitler (Önizleme)
description: Sunucusuz SQL Havuzu (Önizleme) kullanarak Spark tablolarının nasıl sorgulanmasına genel bakış
services: synapse-analytics
author: julieMSFT
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 04/15/2020
ms.author: jrasnick
ms.reviewer: jrasnick
ms.openlocfilehash: ea4e7cd009be8a78faa0dcfab44371a350b6a200
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93315827"
---
# <a name="synchronize-apache-spark-for-azure-synapse-external-table-definitions-in-serverless-sql-pool-preview"></a>Sunucusuz SQL havuzundaki Azure SYNAPSE dış tablo tanımlarına yönelik Apache Spark eşitlemesini (Önizleme)

Sunucusuz SQL Havuzu (Önizleme), Apache Spark meta verileri otomatik olarak eşitleyebilir. Sunucusuz Apache Spark havuzları (Önizleme) içinde bulunan her veritabanı için sunucusuz bir SQL havuzu veritabanı oluşturulacaktır. 

Parquet tabanlı ve Azure depolamada bulunan her Spark dış tablosu için, sunucusuz SQL havuzu veritabanında bir dış tablo oluşturulur. Bu nedenle, Spark havuzlarınızı kapatabilir ve yine de sunucusuz SQL havuzundan Spark dış tablolarını sorgulayabilirsiniz.

Bir tablo Spark 'ta bölümlendiğinde, depolama alanındaki dosyalar klasörlere göre düzenlenir. Sunucusuz SQL havuzu, Bölüm meta verilerini kullanır ve yalnızca sorgunuz için ilgili klasörleri ve dosyaları hedefleyin.

Meta veri eşitleme, Azure SYNAPSE çalışma alanında sağlanan her bir sunucusuz Apache Spark havuzu için otomatik olarak yapılandırılır. Spark dış tablolarını anında sorgulamaya başlayabilirsiniz.

Azure depolama 'da bulunan her Spark Parquet dış tablosu, sunucusuz bir SQL havuzu veritabanına karşılık gelen bir dbo şeması içindeki bir dış tablo ile temsil edilir. 

Spark dış tablo sorguları için, dış [spark_table] hedefleyen bir sorgu çalıştırın. Aşağıdaki örneği çalıştırmadan önce, dosyaların bulunduğu [depolama hesabına doğru erişiminizin](develop-storage-files-storage-access-control.md) olduğundan emin olun.

```sql
SELECT * FROM [db].dbo.[spark_table]
```

> [!NOTE]
> Bir sütun için Add, Drop veya alter Spark External Table komutları sunucusuz SQL havuzundaki dış tabloya yansıtılmayacaktır.

## <a name="apache-spark-data-types-to-sql-data-types-mapping"></a>Veri türlerini SQL veri türleri eşlemesine Apache Spark

| Spark veri türü | SQL veri türü               |
| --------------- | --------------------------- |
| ByteType        | smallint                    |
| Kısa tür      | smallint                    |
| IntegerType     | int                         |
| LongType        | bigint                      |
| FloatType       | real                        |
| DoubleType      | float                       |
| DecimalType     | decimal                     |
| TimestampType   | datetime2                   |
| Tarih türü        | date                        |
| StringType      | varchar(maks.)\*               |
| BinaryType      | ikili                   |
| BooleanType     | bit                         |
| ArrayType       | varchar (max) \* (JSON)\** |
| MapType         | varchar (max) \* (JSON)\** |
| StructType      | varchar (max) \* (JSON)\** |

\* Kullanılan harmanlama Latin1_General_100_BIN2_UTF8.

\** ArrayType, MapType ve StructType, Jdönemleri olarak temsil edilir.



## <a name="next-steps"></a>Sonraki adımlar

Depolama erişim denetimi hakkında daha fazla bilgi edinmek için [depolama Access Control](develop-storage-files-storage-access-control.md) makalesine ilerleyin.
