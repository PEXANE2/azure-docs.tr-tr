---
title: Azure Veri Gölü Analytics U-SQL geliştiricileri için Apache Spark'ı anlayın.
description: Bu makalede, U-SQL geliştiricileri arasındaki farklara yardımcı olmak için Apache Spark kavramları açıklanmaktadır.
author: guyhay
ms.author: guyhay
ms.reviewer: jasonh
ms.service: data-lake-analytics
ms.topic: conceptual
ms.custom: understand-apache-spark-for-usql-developers
ms.date: 10/15/2019
ms.openlocfilehash: 594e1055c4c063e4e151fefa3e183e6e799c90b5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73648434"
---
# <a name="understand-apache-spark-for-u-sql-developers"></a>U-SQL geliştiricileri için Apache Spark'ı anlama

Microsoft, [Azure Databricks](../azure-databricks/what-is-azure-databricks.md) ve Azure [HDInsight](../hdinsight/hdinsight-overview.md) gibi çeşitli Analytics hizmetlerinin yanı sıra Azure Veri Gölü Analitiği'ni de destekler. Geliştiricilerden, analitik boru hatları oluştururken açık kaynak çözümlerine karşı net bir tercihleri olduğunu duyuyoruz. U-SQL geliştiricilerinin Apache Spark'ı anlamalarına ve U-SQL komut dosyalarınızı Apache Spark'a nasıl dönüştüreceğinize yardımcı olmak için bu kılavuzu oluşturduk.

Atabileceğiniz birkaç adım ve birkaç alternatif içerir.

## <a name="steps-to-transform-u-sql-to-apache-spark"></a>U-SQL'i Apache Spark'a dönüştürme adımları

1. İş orkestrasyon boru hatlarınızı dönüştürün.

   [Azure Veri](../data-factory/introduction.md) Fabrikası'nı Azure Veri Gölü Analizi komut dosyalarınızı düzenlemek için kullanıyorsanız, bunları yeni Spark programlarını düzenlemek için ayarlamanız gerekir.
2. U-SQL ve Spark'ın verileri yönetme şekli arasındaki farkları anlama

   Verilerinizi [Azure Veri Gölü Depolama Gen1'den](../data-lake-store/data-lake-store-overview.md) Azure Veri Gölü Depolama [Gen2'ye](../storage/blobs/data-lake-storage-introduction.md)taşımak istiyorsanız, hem dosya verilerini hem de katalog tutulan verileri kopyalamanız gerekir. Azure Veri Gölü Analitiği'nin yalnızca Azure Veri Gölü Depolama Gen1'i desteklediğini unutmayın. Bkz. [Spark veri biçimlerini anlayın](understand-spark-data-formats.md)
3. U-SQL komut dosyalarınızı Spark'a dönüştürün

   U-SQL komut dosyalarınızı dönüştürmeden önce bir analiz hizmeti seçmeniz gerekir. Kullanılabilir işlem hizmetlerinden bazıları şunlardır:
      - [Azure Veri Fabrikası Veri Akışı](../data-factory/concepts-data-flow-overview.md) Veri akışlarının eşlemi, veri mühendislerinin kod yazmadan grafiksel veri dönüştürme mantığı geliştirmelerine olanak tanıyan görsel olarak tasarlanmış veri dönüşümleridir. Karmaşık kullanıcı kodunu yürütmek için uygun olmasa da, geleneksel SQL benzeri veri akışı dönüşümlerini kolayca temsil edebilirler
      - [Azure HDInsight Kovanı](../hdinsight/hadoop/apache-hadoop-using-apache-hive-as-an-etl-tool.md) HDInsight'taki Apache Hive Ayıklama, Dönüştürme ve Yükleme (ETL) işlemleri için uygundur. Bu, U-SQL komut dosyalarınızı Apache Hive'a çevireceğiniz anlamına gelir.
      - [Azure HDInsight Spark](../hdinsight/spark/apache-spark-overview.md) veya [Azure Databricks](../azure-databricks/what-is-azure-databricks.md) gibi Apache Spark Motorları Bu, U-SQL komut dosyalarınızı Spark'a çevireceğiniz anlamına gelir. Daha fazla bilgi için [bkz.](understand-spark-data-formats.md)

> [!CAUTION]
> Hem [Azure Databricks](../azure-databricks/what-is-azure-databricks.md) hem de [Azure HDInsight Spark,](../hdinsight/spark/apache-spark-overview.md) Azure Data Lake Analytics gibi sunucusuz işler değil, küme hizmetleridir. Uygun maliyet/performans oranını elde etmek için kümeleri nasıl sağacağınızı ve maliyetlerinizi en aza indirmek için yaşam ömürlerini nasıl yöneteceğiniz göz önünde bulundurmanız gerekir. Bu hizmetler .NET'te yazılı kullanıcı koduyla farklı performans özelliklerine sahiptir, bu nedenle paketleyiciler yazmanız veya kodunuzu desteklenen bir dilde yeniden yazmanız gerekir. Daha fazla bilgi için [Bkz. Spark veri biçimlerini anlayın](understand-spark-data-formats.md), [U-SQL geliştiricileri için Apache Spark kod kavramlarını anlayın](understand-spark-code-concepts.md), [.Net for Apache Spark](https://dotnet.microsoft.com/apps/data/spark)

## <a name="next-steps"></a>Sonraki adımlar

- [U-SQL geliştiricileri için Spark veri biçimlerini anlama](understand-spark-data-formats.md)
- [U-SQL geliştiricileri için Kıvılcım kod kavramlarını anlama](understand-spark-code-concepts.md)
- [Azure Veri Gölü Depolama Gen1'den büyük veri analizi çözümlerinizi Azure Veri Gölü Depolama Gen2'ye yükseltin](../storage/blobs/data-lake-storage-upgrade.md)
- [Apache Spark için .NET](https://docs.microsoft.com/dotnet/spark/what-is-apache-spark-dotnet)
- [Azure Veri Fabrikası'ndaki Hadoop Hive etkinliğini kullanarak verileri dönüştürme](../data-factory/transform-data-using-hadoop-hive.md)
- [Azure Veri Fabrikası'ndaki Spark etkinliğini kullanarak verileri dönüştürme](../data-factory/transform-data-using-spark.md)
- [Azure HDInsight’ta Apache Spark nedir?](../hdinsight/spark/apache-spark-overview.md)
