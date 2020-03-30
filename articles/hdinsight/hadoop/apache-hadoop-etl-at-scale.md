---
title: Ölçekte ayıklama, dönüştürme ve yükleme (ETL) - Azure HDInsight
description: Apache Hadoop ile HDInsight'ta ayıklama, dönüştürme ve yükün nasıl kullanıldığını öğrenin.
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 03/03/2020
ms.openlocfilehash: f4be3343f090c4d31ccb85eba8e99f22a3b1fcae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79529484"
---
# <a name="extract-transform-and-load-etl-at-scale"></a>Ölçekte ayıklama, dönüştürme ve yükleme (ETL)

Ayıklama, dönüştürme ve yükleme (ETL), verilerin çeşitli kaynaklardan elde edildiği, standart bir konumda toplandığı, temizlendiği ve işlendiği ve sonuçta sorgulanabileceği bir veri deposuna yüklendiği işlemdir. Eski ETL, veri alma işlemlerini işler, yerinde temizler ve sonra ilişkisel bir veri motorunda saklar. HDInsight ile çok çeşitli Apache Hadoop ekosistem bileşenleri, ETL'nin ölçekte performans ını destekler.

HDInsight'ın ETL işleminde kullanımı şu şekilde özetlenebilir:

![HDInsight ETL ölçekli genel bakış](./media/apache-hadoop-etl-at-scale/hdinsight-etl-at-scale-overview.png)

Aşağıdaki bölümlerde ETL aşamalarının her biri ve bunların ilişkili bileşenleri incelenir.

## <a name="orchestration"></a>Orkestrasyon

Orkestrasyon, ETL boru hattının tüm aşamalarına yayılır. HDInsight'taki ETL işleri genellikle birbiriyle birlikte çalışan birkaç farklı ürünü içerir.  Pig başka bir kısmını temizlerken, verilerin bir kısmını temizlemek için Hive'ı kullanabilirsiniz.  Azure Veri Gölü Deposu'ndan Azure SQL Veritabanına veri yüklemek için Azure Veri Fabrikası'nı kullanabilirsiniz.

Uygun işi uygun zamanda çalıştırmak için orkestrasyon gereklidir.

### <a name="apache-oozie"></a>Apaçi Oozie

Apache Oozie, Hadoop işlerini yöneten bir iş akışı koordinasyon sistemidir. Oozie bir HDInsight kümesi içinde çalışır ve Hadoop yığını ile entegre edilmiştir. Oozie Apache Hadoop MapReduce, Apache Pig, Apache Hive ve Apache Sqoop için Hadoop işleri destekler. Oozie, Java programları veya kabuk komut dosyaları gibi bir sisteme özgü işleri zamanlamak için de kullanılabilir.

Daha fazla bilgi için, [hdinsight'ta bir iş akışını tanımlamak ve çalıştırmak için Apache Oozie'yi](../hdinsight-use-oozie-linux-mac.md) kullanın ve hdInsight'ta bir iş akışını çalıştırmak için uçtan uca bir boru hattı nı çalıştırmak için Oozie'nin nasıl kullanılacağını gösteren derin bir dalış için [bkz.](../hdinsight-operationalize-data-pipeline.md)

### <a name="azure-data-factory"></a>Azure Data Factory

Azure Veri Fabrikası, hizmet olarak platform şeklinde düzenleme özellikleri sağlar. Veri hareketini ve veri dönüşümlerini düzenlemek ve otomatikleştirmek için bulutta veri tabanlı iş akışları oluşturmanıza olanak tanıyan bulut tabanlı bir veri tümleştirme hizmetidir.

Azure Veri Fabrikası'nı kullanarak şunları yapabilirsiniz:

1. Birbirinden farklı veri depolarından veri yutabilecek veri tabanlı iş akışları (ardışık işler olarak adlandırılır) oluşturun ve zamanlayın.
2. Azure HDInsight Hadoop, Spark, Azure Veri Gölü Analitiği, Azure Toplu İş ve Azure Machine Learning gibi bilgi işlem hizmetlerini kullanarak verileri işleyip dönüştürün.
3. Çıktı verilerini iş zekası (BI) uygulamalarının kullanması için Azure SQL Veri Ambarı gibi veri depolarında yayımlayabilirsiniz.

Azure Veri Fabrikası hakkında daha fazla bilgi için [belgelere](../../data-factory/introduction.md)bakın.

## <a name="ingest-file-storage-and-result-storage"></a>Dosya depolama ve sonuç depolama yı sindirin

Kaynak veri dosyaları genellikle Azure Depolama veya Azure Veri Gölü Depolama'daki bir konuma yüklenir. Dosyalar herhangi bir biçimde olabilir, ancak genellikle CSV'ler gibi düz dosyalardır.

### <a name="azure-storage"></a>Azure Storage

[Azure Depolama'nın](https://azure.microsoft.com/services/storage/blobs/) belirli ölçeklenebilirlik hedefleri vardır. Daha fazla bilgi için [Blob depolama için Ölçeklenebilirlik ve performans hedeflerine](../../storage/blobs/scalability-targets.md)bakın. Çoğu analitik düğüm için Azure Depolama, birçok küçük dosyayla uğraşırken en iyi şekilde ölçeklendirin.  Azure Depolama, dosyalar ne kadar sayıda veya ne kadar büyük olursa olsun (sınırlarınız içinde olduğunuz sürece) aynı performansı garanti eder.  Bu, ister verilerin bir alt kümesini ister tüm verileri kullanıyor olun, terabaytlarca veri depolayabilir ve yine de tutarlı performans elde edebilirsiniz.

Azure Depolama'da birkaç farklı türde blob vardır.  Bir *ek blob* web günlükleri veya sensör verileri depolamak için harika bir seçenektir.  

Birden çok blobs onlara erişimi ölçeklendirmek için birçok sunucu arasında dağıtılabilir, ancak tek bir blob yalnızca tek bir sunucu tarafından sunulabilir. Blobs mantıksal blob kapsayıcılarda gruplandırılabilir iken, bu gruplandırma hiçbir bölümleme etkileri vardır.

Azure Depolama da blob depolama için bir WebHDFS API katmanı vardır.  HDInsight'taki tüm hizmetler, söz gelimi, bu hizmetlerin Hadoop Distributed Files System 'i (HDFS) nasıl kullanacağına benzer şekilde, veri temizleme ve veri işleme için Azure Blob Depolama'daki dosyalara erişebilir.

Veriler genellikle PowerShell, Azure Depolama SDK veya AZCopy kullanılarak Azure Depolama'ya yutulrülebilir.

### <a name="azure-data-lake-storage"></a>Azure Data Lake Storage

Azure Veri Gölü Depolama (ADLS), HDFS ile uyumlu analitik veriler için yönetilen, hiper ölçekli bir depodur.  ADLS, HDFS'ye benzer bir tasarım paradigması kullanır ve toplam kapasite ve tek tek dosyaların boyutu açısından sınırsız ölçeklenebilirlik sunar. ADLS büyük dosyalarla çalışırken çok iyidir, çünkü büyük bir dosya birden çok düğüm arasında depolanabilir.  ADLS'deki verilerin bölümlemi perde arkasında yapılır.  Yüzlerce terabaytlık veriyi verimli bir şekilde okuyup yazan binlerce eşzamanlı uygulayıcıyla analitik işleri çalıştırmak için büyük bir iş elde emkişi elde emkişi elde emkişi.

Veriler genellikle Azure Veri Fabrikası, ADLS SDK'lar, AdlCopy Hizmeti, Apache DistCp veya Apache Sqoop kullanılarak ADLS'ye yutululur.  Bu hizmetlerden hangisinin kullanılacağı büyük ölçüde verilerin nerede olduğuna bağlıdır.  Veriler şu anda varolan bir Hadoop kümesindeyse, Apache DistCp, AdlCopy Service veya Azure Veri Fabrikası'nı kullanabilirsiniz.  Azure Blob Depolama'daysa, Azure Veri Gölü Depolama .NET SDK, Azure PowerShell veya Azure Veri Fabrikası'nı kullanabilirsiniz.

ADLS, Azure Event Hub veya Apache Storm kullanarak etkinlik alımı için de optimize edilebiyi optimize edede yadeder.

#### <a name="considerations-for-both-storage-options"></a>Her iki depolama seçeneği için de dikkat edilecek noktalar

Terabayt aralığında veri kümeleri yüklemek için, ağ gecikmesi önemli bir sorun olabilir, özellikle de veriler şirket içi bir konumdan geliyorsa.  Bu gibi durumlarda, aşağıdaki seçenekleri kullanabilirsiniz:

* Azure ExpressRoute: Azure ExpressRoute, Azure veri merkezleri ve şirket içi altyapınız arasında özel bağlantılar oluşturmanıza olanak tanır. Bu bağlantılar, büyük miktarda veri aktarmak için güvenilir bir seçenek sağlar. Daha fazla bilgi için Azure [ExpressRoute belgelerine](../../expressroute/expressroute-introduction.md)bakın.

* "Çevrimdışı" veri yüklemesi. Verilerinizle birlikte sabit disk sürücülerini bir Azure veri merkezine aktarmak için [Azure İçe Alma/Dışa Aktarma hizmetini](../../storage/common/storage-import-export-service.md) kullanabilirsiniz. Verileriniz ilk olarak Azure Depolama Blobs'a yüklenir. Daha sonra Azure Depolama lekelerinden Veri Gölü Depolamasına veri kopyalamak için [Azure Veri Fabrikası'nı](../../data-factory/connector-azure-data-lake-store.md) veya [AdlCopy](../../data-lake-store/data-lake-store-copy-data-azure-storage-blob.md) aracını kullanabilirsiniz.

### <a name="azure-sql-data-warehouse"></a>Azure SQL Veri Ambarı

Azure SQL DW, gelecekteki analizler için temizlenmiş ve hazırlanmış sonuçları depolamak için mükemmel bir seçimdir.  Azure HDInsight, bu hizmetleri Azure SQL DW için gerçekleştirmek için kullanılabilir.

Azure SQL Veri Ambarı (SQL DW), analitik iş yükleri için optimize edilmiş ilişkisel bir veritabanı deposudur.  Azure SQL DW, bölümlenmiş tablolara dayalı ölçekler.  Tablolar birden çok düğüme bölünebilir.  Oluşturma sırasında Azure SQL DW düğümleri seçilir.  Olaydan sonra ölçeklendirebilirler, ancak bu veri hareketi gerektiren etkin bir işlemdir. Daha fazla bilgi için [bkz.](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md)

### <a name="apache-hbase"></a>Apache HBase

Apache HBase, Azure HDInsight'ta kullanılabilen önemli bir değer deposudur.  Apache HBase, Hadoop’ta oluşturulan ve Google BigTable’a göre modellenen açık kaynaklı bir NoSQL veritabanıdır. HBase, sütun aileleri tarafından düzenlenen şemasız bir veritabanında büyük miktarda yapılandırılmamış ve yarı yapılandırılmış veri için performant rasgele erişim ve güçlü tutarlılık sağlar.

Veriler bir tablonun satırlarında depolanır ve satır içindeki veriler sütun ailesi tarafından gruplandırılır. HBase, kullanılmadan önce sütunların ya da bunlarda depolanan veri türünün tanımlanmasına gerek duyulmayan, şemasız bir veritabanıdır. Açık kaynak kodu, binlerce düğümdeki petabaytlarca verileri işlemek için doğrusal olarak ölçeklendirir. HBase, Hadoop ekosistemindeki dağıtılmış uygulamalar tarafından sağlanan veri artıklığı, toplu işleme ve diğer özelliklere güvenebilir.

HBase sensör ve günlük verileri için gelecekteki analizler için mükemmel bir hedeftir.

HBase ölçeklenebilirliği HDInsight kümesindeki düğüm sayısına bağlıdır.

### <a name="azure-sql-database-and-azure-database"></a>Azure SQL Veritabanı ve Azure Veritabanı

Azure, hizmet olarak platform olarak üç farklı ilişkisel veritabanları (PAAS) sunar.

* [Azure SQL Veritabanı,](../../sql-database/sql-database-technical-overview.md) Microsoft SQL Server'ın bir uygulamasıdır. Performans hakkında daha fazla bilgi için Azure [SQL Veritabanı'nda Tuning Performance'a](../../sql-database/sql-database-performance-guidance.md)bakın.
* [MySQL için Azure Veritabanı](../../mysql/overview.md) Oracle MySQL'in bir uygulamasıdır.
* [PostgreSQL için Azure Veritabanı, PostgreSQL'in](../../postgresql/quickstart-create-server-database-portal.md) bir uygulamasıdır.

Bu ürünler ölçeklendirilir, bu da daha fazla CPU ve bellek ekleyerek ölçeklendirildikleri anlamına gelir.  Ayrıca, daha iyi G/Ç performansı için ürünlerle birlikte premium diskler kullanmayı da seçebilirsiniz.

## <a name="azure-analysis-services"></a>Azure Analysis Services

Azure Analiz Hizmetleri (AAS), Power BI, Excel, Reporting Services raporları ve diğer veriler gibi iş raporları ve istemci uygulamaları için analitik verileri sağlayan karar desteği ve iş analitiğinde kullanılan analitik bir veri motorudur. görselleştirme araçları.

Analiz küpleri, her bir küp için katmanları değiştirerek ölçeklendirilebilir.  Daha fazla bilgi için Azure [Analiz Hizmetleri Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/analysis-services/)bakın.

## <a name="extract-and-load"></a>Ayıklama ve Yük

Veriler Azure'da var olduğunda, verileri ayıklamak ve diğer ürünlere yüklemek için birçok hizmeti kullanabilirsiniz.  HDInsight, Sqoop ve Flume'u destekler.

### <a name="apache-sqoop"></a>Apaçi Sqoop

Apache Sqoop, yapılandırılmış, yarı yapılandırılmış ve yapılandırılmamış veri kaynakları arasında veri aktarımı için tasarlanmış bir araçtır.

Sqoop, paralel çalışma ve hata toleransı sağlamak için verileri içe ve dışa aktarmak için MapReduce'i kullanır.

### <a name="apache-flume"></a>Apaçi Flume

Apache Flume, büyük miktarda günlük verisinin verimli bir şekilde toplanması, toplanması ve taşınması için dağıtılmış, güvenilir ve kullanılabilir bir hizmettir. Flume, veri akışıakışına dayalı basit ve esnek bir mimariye sahiptir. Flume sağlam ve arıza yada dayanıklı, dayanıklı güvenilirlik mekanizmaları ve birçok arıza ve kurtarma mekanizması ile. Flume, çevrimiçi analitik uygulamaya olanak tanıyan basit bir genişletilebilir veri modeli kullanır.

Apache Flume, Azure HDInsight ile kullanılamaz.  Şirket içi Hadoop yüklemesi, Azure Depolama Blobs veya Azure Veri Gölü Depolamasına veri göndermek için Flume'yi kullanabilir.  Daha fazla bilgi için [HDInsight ile Apache Flume kullanma](https://web.archive.org/web/20190217104751/https://blogs.msdn.microsoft.com/bigdatasupport/2014/03/18/using-apache-flume-with-hdinsight/)'ya bakın.

## <a name="transform"></a>Dönüşüm

Veriler seçilen konumda var olduğunda, verileri temizlemeniz, birleştirmeniz veya belirli bir kullanım deseni için hazırlamanız gerekir.  Hive, Pig ve Spark SQL bu tür işler için iyi seçeneklerdir.  Hepsi HDInsight'ta desteklenir.

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Hive'ı ETL Aracı Olarak Kullanma](apache-hadoop-using-apache-hive-as-an-etl-tool.md)
* [Azure HDInsight kümeleriyle Azure Veri Gölü Depolama Gen2'yi kullanma](../hdinsight-hadoop-use-data-lake-storage-gen2.md)
* [Verileri Azure SQL Veritabanından Apache Hive tablosuna taşıma](./apache-hadoop-use-sqoop-mac-linux.md)
