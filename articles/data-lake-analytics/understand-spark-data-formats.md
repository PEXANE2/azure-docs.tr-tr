---
title: Azure Veri Gölü Analytics U-SQL geliştiricileri için Apache Spark veri biçimlerini anlayın.
description: Bu makalede, U_SQL geliştiricilerin U-SQL ve Spark veri biçimleri arasındaki farklılıkları anlamalarına yardımcı olmak için Apache Spark kavramları açıklanmaktadır.
author: guyhay
ms.author: guyhay
ms.reviewer: jasonh
ms.service: data-lake-analytics
ms.topic: conceptual
ms.custom: understand-apache-spark-data-formats
ms.date: 01/31/2019
ms.openlocfilehash: 36f39503ca32f1ee4b422ae7b1cf9abf48716f07
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73648447"
---
# <a name="understand-differences-between-u-sql-and-spark-data-formats"></a>U-SQL ve Spark veri biçimleri arasındaki farkları anlama

[Azure Veri Tuğlaları](../azure-databricks/what-is-azure-databricks.md) veya Azure [HDInsight Spark'ı](../hdinsight/spark/apache-spark-overview.md)kullanmak istiyorsanız, verilerinizi [Azure Veri Gölü Depolama Gen1'den Azure](../data-lake-store/data-lake-store-overview.md) Veri Gölü Depolama [Gen2'ye](../storage/blobs/data-lake-storage-introduction.md)geçirmenizi öneririz.

Dosyalarınızı taşımanın yanı sıra, Verilerinizi Spark'ın erişebileceği U-SQL tablolarında depolanan hale getirmek de istersiniz.

## <a name="move-data-stored-in-azure-data-lake-storage-gen1-files"></a>Azure Veri Gölü Depolama Gen1 dosyalarında depolanan verileri taşıma

Dosyalarda depolanan veriler çeşitli şekillerde taşınabilir:

- Verileri [Azure Veri Gölü Depolama Gen1](../data-lake-store/data-lake-store-overview.md) hesabındaki Verileri Azure [Veri Gölü Depolama Gen2](../storage/blobs/data-lake-storage-introduction.md) hesabına kopyalamak için bir [Azure Veri Fabrikası](../data-factory/introduction.md) ardışık hattı yazın.
- [Azure Veri Gölü Depolama Gen1](../data-lake-store/data-lake-store-overview.md) hesabındaki verileri okuyan ve Azure Veri Gölü Depolama [Gen2](../storage/blobs/data-lake-storage-introduction.md) hesabına yazan bir Spark işi yazın. Kullanım durumunuza bağlı olarak, orijinal dosya biçimini korumanız gerekmiyorsa Parke gibi farklı bir biçimde yazmak isteyebilirsiniz.

Makaleyi gözden geçirmenizi öneririz [Büyük veri analizi çözümlerinizi Azure Veri Gölü Depolama Gen1'den Azure Veri Gölü Depolama Gen2'ye yükseltin](../storage/blobs/data-lake-storage-upgrade.md)

## <a name="move-data-stored-in-u-sql-tables"></a>U-SQL tablolarında depolanan verileri taşıma

U-SQL tabloları Spark tarafından anlaşılmaz. U-SQL tablolarında depolanan verileriniz varsa, tablo verilerini ayıklayan ve Spark'ın anladığı biçimde kaydeden bir U-SQL işi çalıştırın. En uygun biçim, Hive metastore'un klasör düzenini izleyen bir parke dosyası kümesi oluşturmaktır.

Çıktı, yerleşik Parke çıkışı ile U-SQL'de ve bölüm klasörlerini oluşturmak için dosya kümeleriyle dinamik çıktı bölümleme kullanılarak elde edilebilir. [Her zamankinden daha fazla dosya işleme ve Parke kullanmak](https://blogs.msdn.microsoft.com/azuredatalake/2018/06/11/process-more-files-than-ever-and-use-parquet-with-azure-data-lake-analytics) nasıl bu Tür Kıvılcım sarf verileri oluşturmak için bir örnek sağlar.

Bu dönüşümden sonra, [Azure Veri Gölü Depolama Gen1 dosyalarında depolanan verileri taşı](#move-data-stored-in-azure-data-lake-storage-gen1-files)bölümünde özetlenen verileri kopyalarsınız.

## <a name="caveats"></a>Uyarılar

- Veri semantik dosyaları kopyalama sırasında, kopya bayt düzeyinde oluşur. Bu nedenle, aynı veriler [Azure Veri Gölü Depolama Gen2](../storage/blobs/data-lake-storage-introduction.md) hesabında da görünmelidir. Ancak, Spark bazı karakterleri farklı yorumlayabilir. Örneğin, bir CSV dosyasındaki satır delimiter için farklı bir varsayılan kullanabilir.
    Ayrıca, yazılı verileri (tablolardan) kopyalıyorsanız, Parke ve Kıvılcım, yazılan bazı değerler (örneğin, float) için farklı duyarlık ve ölçeklere sahip olabilir ve null değerlerini farklı şekilde işleyebilir. Örneğin, U-SQL'de null değerler için C# semantiktiği, Spark'ın ise null değerler için üç değerli bir mantığı vardır.

- Veri organizasyonu (bölümleme) U-SQL tabloları iki düzey bölümleme sağlar. Dış düzey`PARTITIONED BY`( ) değer ve haritalar çoğunlukla hive / Kıvılcım bölümleme düzeni klasör hiyerarşileri kullanarak. Null değerlerinin doğru klasöre eşlendirildiğından emin olmanız gerekir. U-SQL'deki iç düzey (`DISTRIBUTED BY`) 4 dağıtım şeması sunar: yuvarlak robin, aralık, karma ve doğrudan karma.
    Kovan/Kıvılcım tabloları, U-SQL'den farklı bir karma işlev kullanarak yalnızca değer bölümleme veya karma bölümleme desteği destekler. U-SQL tablo verilerinizi çıktığınızda, büyük olasılıkla yalnızca Spark'ın değer bölümlemesini eşleyebilir ve son Kıvılcım sorgularınıza bağlı olarak veri düzeninizi daha fazla ayarı yapmanız gerekebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [U-SQL geliştiricileri için Kıvılcım kod kavramlarını anlama](understand-spark-code-concepts.md)
- [Azure Veri Gölü Depolama Gen1'den büyük veri analizi çözümlerinizi Azure Veri Gölü Depolama Gen2'ye yükseltin](../storage/blobs/data-lake-storage-upgrade.md)
- [Apache Spark için .NET](https://docs.microsoft.com/dotnet/spark/what-is-apache-spark-dotnet)
- [Azure Veri Fabrikası'ndaki Spark etkinliğini kullanarak verileri dönüştürme](../data-factory/transform-data-using-spark.md)
- [Azure Veri Fabrikası'ndaki Hadoop Hive etkinliğini kullanarak verileri dönüştürme](../data-factory/transform-data-using-hadoop-hive.md)
- [Azure HDInsight’ta Apache Spark nedir?](../hdinsight/spark/apache-spark-overview.md)
