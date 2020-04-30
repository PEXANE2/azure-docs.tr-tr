---
title: Ölçekte ayıklama, dönüştürme ve yükleme (ETL)-Azure HDInsight
description: Apache Hadoop ile HDInsight 'ta ayıklama, dönüştürme ve yükleme işlemlerinin nasıl kullanıldığını öğrenin.
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,seoapr2020
ms.date: 04/28/2020
ms.openlocfilehash: ee989ccbb2e441256bec71781c538c7761fc7b88
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82232249"
---
# <a name="extract-transform-and-load-etl-at-scale"></a>Ölçeklendirerek ayıklama, dönüştürme ve yükleme (ETL)

Ayıklama, dönüştürme ve yükleme (ETL), verilerin çeşitli kaynaklardan alındığı işlemdir. Standart bir konumda toplanır, temizlendi ve işlenir. Sonunda sorgulanabilecek bir veri deposuna yüklenmiş. Eski ETL, verileri içeri aktarın, yerinde temizler ve sonra ilişkisel bir veri altyapısında depolar. HDInsight ile, çok çeşitli Apache Hadoop ortam bileşenleri ETL 'yi ölçekli olarak destekler.

ETL işleminde HDInsight 'ın kullanılması bu işlem hattı tarafından özetlenebilir:

![HDInsight ETL ölçeğe genel bakış](./media/apache-hadoop-etl-at-scale/hdinsight-etl-at-scale-overview.png)

Aşağıdaki bölümler ETL aşamalarını ve bunlarla ilişkili bileşenleri keşfedebilir.

## <a name="orchestration"></a>Düzenleme

Düzenleme, ETL işlem hattının tüm aşamaları boyunca yayılır. HDInsight 'taki ETL işleri genellikle birbirleriyle birlikte çalışan birkaç farklı ürün içerir.  Verilerin bir bölümünü temizlemek için Hive kullanabilirsiniz, ancak Pig başka bir bölümü temizler.  Azure Data Lake Store Azure SQL veritabanı 'na veri yüklemek için Azure Data Factory kullanabilirsiniz.

Uygun zamanda ilgili işi çalıştırmak için düzenleme gereklidir.

### <a name="apache-oozie"></a>Apache Oozie

Apache Oozie, Hadoop işlerini yöneten bir iş akışı koordinasyon sistemidir. Oozie bir HDInsight kümesi içinde çalışır ve Hadoop yığınına tümleştirilir. Oozie, Apache Hadoop MapReduce, Apache Pig, Apache Hive ve Apache Sqoop için Hadoop işlerini destekler. Oozie, Java programları veya kabuk betikleri gibi bir sisteme özgü işleri zamanlamak için de kullanılabilir.

Daha fazla bilgi için bkz. [HDInsight 'ta bir iş akışını tanımlamak ve çalıştırmak için Apache Hadoop Ile Apache Oozie kullanma](../hdinsight-use-oozie-linux-mac.md). Ayrıca bkz. [veri](../hdinsight-operationalize-data-pipeline.md)işlem hattını işleme.

### <a name="azure-data-factory"></a>Azure Data Factory

Azure Data Factory, bir hizmet olarak platform biçiminde düzenleme özellikleri sağlar. Bulutta veri odaklı iş akışları oluşturmanıza olanak tanıyan, bulut tabanlı bir veri tümleştirme hizmetidir. Veri hareketini ve veri dönüştürmeyi düzenlemek ve otomatikleştirmek için iş akışları.

Azure Data Factory kullanarak şunları yapabilirsiniz:

1. Farklı veri depolarından veri alan veri odaklı iş akışları (işlem hatları olarak adlandırılır) oluşturun ve zamanlayın.
2. Azure HDInsight Hadoop gibi işlem hizmetlerini kullanarak verileri işleyin ve dönüştürün. Veya Spark, Azure Data Lake Analytics, Azure Batch ve Azure Machine Learning.
3. Çıktı verilerini iş zekası (BI) uygulamalarının kullanması için Azure SQL Veri Ambarı gibi veri depolarında yayımlayabilirsiniz.

Azure Data Factory hakkında daha fazla bilgi için [belgelerine](../../data-factory/introduction.md)bakın.

## <a name="ingest-file-storage-and-result-storage"></a>Alma dosya depolama ve sonuç depolama

Kaynak veri dosyaları genellikle Azure Storage veya Azure Data Lake Storage bir konuma yüklenir. Dosyalar herhangi bir biçimde olabilir, ancak genellikle CSV 'ler gibi düz dosyalardır.

### <a name="azure-storage"></a>Azure Storage

Azure depolama 'nın belirli uyarlanabilir hedefleri vardır. Bkz. [BLOB depolama Için ölçeklenebilirlik ve performans hedefleri](../../storage/blobs/scalability-targets.md). Birçok analitik düğüm için Azure depolama, çok daha küçük dosyalarla ilgilenirken en iyi şekilde ölçeklendirilir.  Azure depolama, dosyaların ne kadar büyük olduğuna ve ne kadar büyük bir şekilde (limitlerinizin içinde olduğunuz sürece) aynı performansı garanti eder.  Bu garanti, terabayt depolayabilmeniz ve verilerin bir alt kümesini ya da tüm verilerin bir alt kümesini kullanıyor olup olmadığınız anlamına gelir.

Azure depolama 'nın birçok farklı blob türü vardır.  *Ekleme blobu* , web günlüklerinin veya algılayıcı verilerinin depolanması için harika bir seçenektir.  

Birden çok blob, bunlara erişimi ölçeklendirmek için birçok sunucu arasında dağıtılabilir. Ancak tek bir blob yalnızca tek bir sunucu tarafından sunulabilir. Blob 'lar blob kapsayıcılarında mantıksal olarak gruplandırılabildiği halde, bu gruplamanın hiçbir bölümleme etkileri yoktur.

Azure depolama 'nın Ayrıca blob depolaması için bir Webbir API katmanı vardır.  HDInsight 'taki tüm hizmetler, veri temizleme ve veri işleme için Azure Blob depolama alanındaki dosyalara erişebilir. Bu hizmetlerin Hadoop Dağıtılmış dosya sistemi 'ni (ISE) kullanmasına benzer.

Veriler genellikle PowerShell, Azure Storage SDK veya AZCopy kullanılarak Azure depolama 'ya alınır.

### <a name="azure-data-lake-storage"></a>Azure Data Lake Storage

Azure Data Lake Storage (ADLS), yönetilen, hiper ölçekli bir depodur. , Bir analiz verileri için, ".  ADLS,,, bir tasarım paradigması ile benzer. ADLS, toplam kapasite ve tek dosyaların boyutu için sınırsız uyumluluk sunar. Büyük bir dosya birden çok düğüm arasında depolanabileceği için, ADLS, büyük dosyalarla çalışırken iyidir.  ADLS içindeki bölümlendirme verileri arka planda yapılır.  Yüzlerce terabaytlarca veriyi verimli bir şekilde okuyan ve yazan binlerce eşzamanlı yürütme sayesinde analitik işleri çalıştırmak için büyük ölçüde üretilen iş kazanın.

Veriler genellikle Azure Data Factory kullanılarak ADLS 'ye alınır. Ya da ADLS SDK 'Ları, AdlCopy hizmeti, Apache DistCp veya Apache Sqoop.  Bu hizmetlerden hangisinin büyük ölçüde kullanılacağı, verilerin nerede olduğuna bağlıdır.  Veriler şu anda mevcut bir Hadoop kümenizle birlikte Apache DistCp, AdlCopy hizmeti veya Azure Data Factory kullanabilirsiniz.  Azure Blob depolama 'daki veriler için Azure Data Lake Storage .NET SDK, Azure PowerShell veya Azure Data Factory kullanabilirsiniz.

ADLS, Azure Olay Hub 'ı veya Apache Storm kullanılarak olay alımı için de iyileştirilmiştir.

#### <a name="considerations-for-both-storage-options"></a>Depolama seçeneklerine ilişkin konular

Veri kümelerini terabayt aralığında karşıya yüklemek için, özellikle de veriler şirket içi bir konumdan geliyorsa, ağ gecikmesi önemli bir sorun olabilir.  Böyle durumlarda, aşağıdaki seçenekleri kullanabilirsiniz:

* Azure ExpressRoute: Azure ExpressRoute, Azure veri merkezleri ile şirket içi altyapınız arasında özel bağlantılar oluşturmanızı sağlar. Bu bağlantılar, büyük miktarlarda veri aktarmaya yönelik güvenilir bir seçenek sağlar. Daha fazla bilgi için bkz. [Azure ExpressRoute belgeleri](../../expressroute/expressroute-introduction.md).

* Verilerin "çevrimdışı" yüklenmesi. Azure [içeri/dışarı aktarma hizmeti](../../storage/common/storage-import-export-service.md) 'ni kullanarak verilerinize bir Azure veri merkezine sabit disk sürücüleri gönderebilirsiniz. Verileriniz ilk olarak Azure Storage Bloblarına yüklenir. Daha sonra, Azure depolama Bloblarındaki verileri Data Lake Storage kopyalamak için Azure Data Factory veya AdlCopy aracını kullanabilirsiniz.

### <a name="azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı

Azure SQL DW, hazırlanan sonuçları depolamak için harika bir seçimdir.  Azure HDInsight, Azure SQL DW için bu hizmetleri yapmak üzere kullanılabilir.

Azure SQL veri ambarı (SQL DW), analitik iş yükleri için iyileştirilmiş bir ilişkisel veritabanı deposudur.  Azure SQL DW, bölümlenmiş tablolara göre ölçeklendirilir.  Tablolar birden çok düğüm arasında bölümlenebilir.  Azure SQL DW düğümleri oluşturma sırasında seçilir.  Olgu sonrasında ölçeklendirebilir, ancak bu, veri taşıma gerektirebilecek etkin bir işlemdir. Daha fazla bilgi için bkz. [SQL veri ambarı-Işlem yönetme](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md).

### <a name="apache-hbase"></a>Apache HBase

Apache HBase, Azure HDInsight 'ta kullanılabilen bir anahtar-değer deposudur.  Apache HBase, Hadoop’ta oluşturulan ve Google BigTable’a göre modellenen açık kaynaklı bir NoSQL veritabanıdır. HBase, büyük miktarlarda yapılandırılmamış ve yarı oluşturulmuş veriler için yüksek düzeyde rastgele erişim ve güçlü tutarlılık sağlar. Sütun ailelerine göre düzenlenmiş, şemaya daha seyrek bir veritabanındaki veriler.

Veriler bir tablonun satırlarında depolanır ve satır içindeki veriler sütun ailesi tarafından gruplandırılır. HBase, şesız bir veritabanıdır. Bunlara depolanan sütunların ve veri türlerinin kullanılmadan önce tanımlanması gerekmez. Açık kaynak kodu, binlerce düğümdeki petabaytlarca verileri işlemek için doğrusal olarak ölçeklendirir. HBase, Hadoop ortamında dağıtılmış uygulamalar tarafından sunulan veri yedekliliği, toplu işleme ve diğer özellikleri kullanabilir.

HBase, gelecekteki analize yönelik algılayıcı ve günlük verileri için mükemmel bir hedefdir.

HBase uyumluluk, HDInsight kümesindeki düğüm sayısına bağlıdır.

### <a name="azure-sql-database-and-azure-database"></a>Azure SQL veritabanı ve Azure veritabanı

Azure, hizmet olarak platform (PAAS) olarak üç farklı ilişkisel veritabanı sunmaktadır.

* [Azure SQL veritabanı](../../sql-database/sql-database-technical-overview.md) Microsoft SQL Server uygulamasıdır. Performans hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı 'Nda ayarlama performansı](../../sql-database/sql-database-performance-guidance.md).
* [MySQL Için Azure veritabanı](../../mysql/overview.md) , Oracle MySQL 'in bir uygulamasıdır.
* [PostgreSQL Için Azure veritabanı](../../postgresql/quickstart-create-server-database-portal.md) , PostgreSQL 'in bir uygulamasıdır.

Bu ürünlerin ölçeği, daha fazla CPU ve bellek ekleyerek ölçeklendiği anlamına gelir.  Daha iyi g/ç performansı için ürünlerle Premium diskler kullanmayı da tercih edebilirsiniz.

## <a name="azure-analysis-services"></a>Azure Analysis Services

Azure Analysis Services (AAS), karar destek ve iş analizi 'nde kullanılan bir analitik veri altyapısıdır. AAS, iş raporları ve Power BI gibi istemci uygulamaları için analitik verileri sağlar. Ayrıca, Excel, Raporlama Hizmetleri raporları ve diğer veri görselleştirme araçları.

Analiz küpleri, her bir küpe ait katmanları değiştirerek ölçeklendirebilir.  Daha fazla bilgi için bkz. [Azure Analysis Services fiyatlandırması](https://azure.microsoft.com/pricing/details/analysis-services/).

## <a name="extract-and-load"></a>Ayıkla ve yükle

Veriler Azure 'da mevcut olduğunda, bunları ayıklamak ve diğer ürünlere yüklemek için birçok hizmet kullanabilirsiniz.  HDInsight, Sqoop ve flome 'yi destekler.

### <a name="apache-sqoop"></a>Apache Sqoop

Apache Sqoop, yapılandırılmış, yarı yapılandırılmış ve yapılandırılmamış veri kaynakları arasında verileri verimli bir şekilde aktarmak için tasarlanan bir araçtır.

Sqoop, paralel işlem ve hataya dayanıklılık sağlamak üzere verileri içeri ve dışarı aktarmak için MapReduce kullanır.

### <a name="apache-flume"></a>Apache akıcı

`Apache Flume`, büyük miktarlarda günlük verilerini verimli bir şekilde toplamak, toplamak ve taşımak için dağıtılmış, güvenilir ve kullanılabilir bir hizmettir. Akışkan akışı, akış veri akışlarını temel alan esnek bir mimariye sahiptir. Akıcı bir güvenilirlik mekanizmalarına ve birçok yük devretme ve kurtarma mekanizmalarına sahip sağlam ve hataya dayanıklı bir akışdır. Akışkan, çevrimiçi analitik uygulamaya izin veren basit bir Genişletilebilir veri modeli kullanır.

Apache akışkan, Azure HDInsight ile kullanılamaz.  Şirket içi Hadoop yüklemesi, verileri Azure depolama Bloblarına veya Azure Data Lake Storage göndermek için akıcı bir şekilde kullanabilir.  Daha fazla bilgi için bkz. [HDInsight Ile Apache Flobana kullanımı](https://web.archive.org/web/20190217104751/https://blogs.msdn.microsoft.com/bigdatasupport/2014/03/18/using-apache-flume-with-hdinsight/).

## <a name="transform"></a>Dönüşüm

Seçilen konumda veriler varsa, bunu temizlemeniz, birleştirmeniz veya belirli bir kullanım deseninin hazırlanması gerekir.  Hive, Pig ve Spark SQL, bu tür bir çalışma için iyi seçimlerdir.  Bunlar HDInsight 'ta desteklenirler.

## <a name="next-steps"></a>Sonraki adımlar

* [ETL aracı olarak Apache Hive kullanma](apache-hadoop-using-apache-hive-as-an-etl-tool.md)
* [Azure HDInsight kümeleri ile Azure Data Lake Storage 2. kullanma](../hdinsight-hadoop-use-data-lake-storage-gen2.md)
* [Verileri Azure SQL veritabanından Apache Hive tablosuna taşıma](./apache-hadoop-use-sqoop-mac-linux.md)
