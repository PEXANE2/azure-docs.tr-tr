---
title: "Mimari: Şirket içi Apache Hadoop'dan Azure HDInsight'a"
description: Şirket içi Hadoop kümelerini Azure HDInsight'a geçirmek için mimarinin en iyi uygulamalarını öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: ashishth
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/06/2019
ms.openlocfilehash: 2d0d5bb871612bc5e16a26eb49808c39661ffb50
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75934682"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---architecture-best-practices"></a>Şirket içi Apache Hadoop kümelerini Azure HDInsight'a geçirin - mimari en iyi uygulamalar

Bu makalede, Azure HDInsight sistemlerinin mimarisi için öneriler verebilmiştir. Şirket içi Apache Hadoop sistemlerini Azure HDInsight'a geçirmede yardımcı olmak için en iyi uygulamaları sağlayan bir serinin parçasıdır.

## <a name="use-multiple-workload-optimized-clusters"></a>Birden çok iş yükü için en iyi duruma getirilmiş kümeler kullanma

Birçok şirket içi Apache Hadoop dağıtımı, birçok iş yükünü destekleyen tek bir büyük kümeden oluşur. Bu tek küme karmaşık olabilir ve her şeyin birlikte çalışmasını sağlamak için tek tek hizmetleriçin ödün ler gerektirebilir. Şirket içi Hadoop kümelerini Azure HDInsight'a geçirmek için yaklaşım değişikliği gerekir.

Azure HDInsight kümeleri belirli bir bilgi işlem kullanımı türü için tasarlanmıştır. Depolama birden çok küme arasında paylaşılabildiği için, farklı işlerin gereksinimlerini karşılamak için birden çok iş yükü için optimize edilmiş işlem kümeleri oluşturmak mümkündür. Her küme türü, belirli bir iş yükü için en uygun yapılandırmaya sahiptir. Aşağıdaki tabloda HDInsight'ta desteklenen küme türleri ve ilgili iş yükleri listelenir.

|İş yükü|HDInsight Küme türü|
|---|---|
|Toplu işleme (ETL / ELT)|Hadoop, Kıvılcım|
|Veri ambarlama|Hadoop, Kıvılcım, İnteraktif Sorgu|
|IoT / Akış|Kafka, Fırtına, Kıvılcım|
|NoSQL İşlemel işleme|HBase|
|Bellek önbelleğe alma ile etkileşimli ve daha hızlı sorgular|Interactive Query|
|Veri Bilimi|ML Hizmetleri, Kıvılcım|

Aşağıdaki tablo, bir HDInsight kümesi oluşturmak için kullanılabilecek farklı yöntemleri gösterir.

|Araç|Tarayıcı tabanlı|Komut Satırı|REST API|SDK|
|---|---|---|---|---|
|[Azure portalında](../hdinsight-hadoop-create-linux-clusters-portal.md)|X||||
|[Azure Data Factory](../hdinsight-hadoop-create-linux-clusters-adf.md)|X|X|X|X|
|[Azure CLI (ver 1.0)](../hdinsight-hadoop-create-linux-clusters-azure-cli.md)||X|||
|[Azure PowerShell](../hdinsight-hadoop-create-linux-clusters-azure-powershell.md)||X|||
|[cURL](../hdinsight-hadoop-create-linux-clusters-curl-rest.md)||X|X||
|[.NET SDK](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight?view=azure-dotnet)||||X|
|[Python SDK'sı](https://docs.microsoft.com/python/api/overview/azure/hdinsight?view=azure-python)||||X|
|[Java SDK](https://docs.microsoft.com/java/api/overview/azure/hdinsight?view=azure-java-stable)||||X|
|[Azure Kaynak Yöneticisi şablonları](../hdinsight-hadoop-create-linux-clusters-arm-templates.md)||X|||

Daha fazla bilgi için [HDInsight'ta Cluster türü makalesine](../hadoop/apache-hadoop-introduction.md)bakın.

## <a name="use-transient-on-demand-clusters"></a>Geçici isteğe bağlı kümeleri kullanma

HDInsight kümeleri uzun süre kullanılmadan kalabilir. HdInsight, kaynak maliyetlerinden tasarruf etmeye yardımcı olmak için, iş yükü başarıyla tamamlandıktan sonra silinebilen isteğe bağlı geçici kümeleri destekler.

Bir kümeyi sildiğinizde, ilişkili depolama hesabı ve dış meta veriler kaldırılmaz. Küme daha sonra aynı depolama hesapları ve meta-depolar kullanılarak yeniden oluşturulabilir.

Azure Veri Fabrikası, isteğe bağlı HDInsight kümelerinin oluşturulmasını planlamak için kullanılabilir. Daha fazla bilgi için, [Azure Veri Fabrikası'nı kullanarak HDInsight'ta isteğe bağlı Apache Hadoop kümeleri oluştur](../hdinsight-hadoop-create-linux-clusters-adf.md)makalesine bakın.

## <a name="decouple-storage-from-compute"></a>Depolamayı işlemden ayırma

Tipik şirket içi Hadoop dağıtımları, veri depolama ve veri işleme için aynı makine kümesini kullanır. Birlikte bulundukları için, hesaplama ve depolama birlikte ölçeklendirilmelidir.

HDInsight kümelerinde depolamanın bilgi işlemle birlikte konumlanması gerekmez ve Azure depolama, Azure Veri Gölü Depolama veya her ikisinde de bulunabilir. Depolamayı bilgisayardan ayırmanın aşağıdaki avantajları vardır:

- Kümeler arasında veri paylaşımı.
- Veriler kümeye bağlı olmadığından geçici kümelerin kullanımı.
- Daha düşük depolama maliyeti.
- Depolamayı ölçekleme ve ayrı ayrı hesaplama.
- Bölgeler arasında veri çoğaltma.

İşlem kümeleri, işlem ve depolamayı ayırmanın performans maliyetini azaltmak için Azure bölgesindeki depolama hesabı kaynaklarına yakın oluşturulur. Yüksek hızlı ağlar, bilgi işlem düğümlerinin Azure depolama içindeki verilere erişmesini verimli hale getirir.

## <a name="use-external-metadata-stores"></a>Dış meta veri depolarını kullanma

HDInsight kümeleri ile çalışan iki ana metastore vardır: [Apache Hive](https://hive.apache.org/) ve [Apache Oozie](https://oozie.apache.org/). Kovan metastore Hadoop, Spark, LLAP, Presto ve Apache Pig dahil olmak üzere veri işleme motorları tarafından kullanılabilecek merkezi şema deposudur. Oozie metastore, planlama ve devam eden ve hadoop işlerini tamamlayan ların durumu hakkında ayrıntıları saklar.

HDInsight, Hive ve Oozie metastore'ları için Azure SQL Veritabanı'nı kullanır. HDInsight kümelerinde bir metastore kurmanın iki yolu vardır:

1. Varsayılan metastore

    - Ek ücret yok.
    - Küme silindiğinde Metastore silinir.
    - Metastore farklı kümeler arasında paylaşılamaz.
    - Beş DTU sınırı olan temel Azure SQL DB'yi kullanır.

1. Özel dış metastore

    - Metastore olarak harici bir Azure SQL Veritabanı belirtin.
    - Hive schema Oozie iş ayrıntıları da dahil olmak üzere meta verileri kaybetmeden kümeler oluşturulabilir ve silinebilir.
    - Tek metastore db kümeleri farklı türleri ile paylaşılabilir.
    - Metastore gerektiği gibi ölçeklendirilebilir.
    - Daha fazla bilgi için bkz: [Azure HDInsight'ta harici meta veri depolarını kullan.](../hdinsight-use-external-metadata-stores.md)

## <a name="best-practices-for-hive-metastore"></a>Hive Metastore için en iyi uygulamalar

Bazı HDInsight Hive metastore en iyi uygulamaları aşağıdaki gibidir:

- Bilgi işlem kaynaklarını ve meta verileri ayırmak için özel bir dış metastore kullanın.
- 50 DTU ve 250 GB depolama alanı sağlayan S2 katmanı Azure SQL örneğiyle başlayın. Bir darboğaz görürseniz, veritabanını ölçeklendirebilirsiniz.
- Bir HDInsight küme sürümü için oluşturulan metastore'u farklı bir sürüm kümeleriyle paylaşmayın. Farklı Hive sürümleri farklı şemalar kullanın. Örneğin, bir metastore hem Hive 1.2 hem de Hive 2.1 kümeleriyle paylaşılamaz.
- Özel metastore'u düzenli olarak yedekleyin.
- Metastore ve HDInsight kümesini aynı bölgede tutun.
- Azure portalı veya Azure Monitor günlükleri gibi Azure SQL Veritabanı İzleme araçlarını kullanarak performans ve kullanılabilirlik için metastore'u izleyin.
- Tablolar `ANALYZE TABLE` ve sütunlar için istatistik oluşturmak için gereken komutu çalıştırın. Örneğin, `ANALYZE TABLE [table_name] COMPUTE STATISTICS`.

## <a name="best-practices-for-different-workloads"></a>Farklı iş yükleri için en iyi uygulamalar

- Llap gelişmiş yanıt süresi ile etkileşimli Hive sorguları için [LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) kümesini kullanmayı düşünün, Hive 2.0 sorguların bellek önbelleğe alınmasına izin veren yeni bir özelliktir. LLAP hive sorguları çok daha hızlı yapar, [bazı durumlarda Hive 1.x daha hızlı 26x](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).
- Hive işleri yerine Spark işleri kullanmayı düşünün.
- IMPALA tabanlı sorguları LLAP sorgularıyla değiştirmeyi düşünün.
- MapReduce işlerini Spark işleri ile değiştirmeyi düşünün.
- Spark Structured Streaming işlerini kullanarak düşük gecikmeli Spark toplu işlerini değiştirmeyi düşünün.
- Veri düzenleme için Azure Veri Fabrikası (ADF) 2.0'ı kullanmayı düşünün.
- Küme Yönetimi için Ambari'yi düşünün.
- Komut dosyalarını işlemek için veri depolamayı şirket içi HDFS'den WASB veya ADLS veya ADFS'ye değiştirin.
- Hive tablolarve denetim ranger RBAC kullanarak düşünün.
- MongoDB veya Cassandra yerine CosmosDB kullanmayı düşünün.

## <a name="next-steps"></a>Sonraki adımlar

Bu serinin sonraki makaleyi okuyun:

- [Azure HDInsight Hadoop geçişi için şirket içi altyapı en iyi uygulamaları](apache-hadoop-on-premises-migration-best-practices-infrastructure.md)
