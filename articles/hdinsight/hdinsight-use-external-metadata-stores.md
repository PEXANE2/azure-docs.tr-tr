---
title: Harici meta veri depolarını kullanma - Azure HDInsight
description: Azure HDInsight kümeleri ve en iyi uygulamalarla harici meta veri depolarını kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/02/2020
ms.openlocfilehash: edb2d256d3e5d98c52dbdff1162e0e030ebe2be3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79272168"
---
# <a name="use-external-metadata-stores-in-azure-hdinsight"></a>Azure HDInsight’ta dış meta veri depolarını kullanma

HDInsight, önemli meta veri çözümlerini ve yönetim veritabanlarını harici veri depolarına dağıtarak verilerinizin ve meta verilerinizin denetimini ele geçirmenizi sağlar. Bu özellik şu anda [Apache Hive metastore,](#custom-metastore) [Apache Oozie metastore](#apache-oozie-metastore) ve [Apache Ambari veritabanı](#custom-ambari-db)için kullanılabilir.

HDInsight'taki Apache Hive metastore, Apache Hadoop mimarisinin önemli bir parçasıdır. Metastore, Apache Spark, Interactive Query (LLAP), Presto veya Apache Pig gibi diğer büyük veri erişim araçları tarafından kullanılabilen merkezi şema deposudur. HDInsight, Hive metastore olarak bir Azure SQL Veritabanı kullanır.

![HDInsight Hive Metadata Store Mimarisi](./media/hdinsight-use-external-metadata-stores/metadata-store-architecture.png)

HDInsight kümeleriniz için bir metastore ayarlamanın iki yolu vardır:

* [Varsayılan metastore](#default-metastore)
* [Özel metastore](#custom-metastore)

## <a name="default-metastore"></a>Varsayılan metastore

Varsayılan olarak, HDInsight her küme türünde bir metastore oluşturur. Bunun yerine özel bir metastore belirtebilirsiniz. Varsayılan metastore aşağıdaki hususları içerir:

* Ek ücret yok. HDInsight, size ek bir ücret ödemeden her küme türünde bir metastore oluşturur.

* Her varsayılan metastore küme yaşam döngüsünün bir parçasıdır. Bir kümeyi sildiğinizde, ilgili metastore ve meta veriler de silinir.

* Varsayılan metastore'u diğer kümelerle paylaşamaz.

* Varsayılan metastore, beş DTU (veritabanı işlem birimi) sınırı olan temel Azure SQL DB'yi kullanır.
Bu varsayılan metastore genellikle birden çok küme gerektirmeyen ve kümenin yaşam döngüsünün ötesinde korunmuş meta veriler gerektirmeyen görece basit iş yükleri için kullanılır.

## <a name="custom-metastore"></a>Özel metastore

HDInsight, üretim kümeleri için önerilen özel meta store'ları da destekler:

* Metastore olarak kendi Azure SQL Veritabanınızı belirtirsiniz.

* Metastore'un yaşam döngüsü kümelerin yaşam döngüsüne bağlı değildir, böylece meta verileri kaybetmeden kümeler oluşturabilir ve silebilirsiniz. Hive şemalarınız gibi meta veriler, HDInsight kümesini silip yeniden oluşturduktan sonra bile devam eder.

* Özel bir metastore, bu metamağazaya birden çok küme ve küme türü eklemenize olanak tanır. Örneğin, TEK bir metastore HDInsight'ta Etkileşimli Sorgu, Hive ve Spark kümeleri arasında paylaşılabilir.

* Seçtiğiniz performans düzeyine göre bir metastore (Azure SQL DB) maliyetini ödler.

* Gerektiğinde metastore'u ölçeklendirebilirsiniz.

* Küme ve dış metastore aynı bölgede barındırılmalıdır.

![HDInsight Hive Metadata Store Kullanım Örneği](./media/hdinsight-use-external-metadata-stores/metadata-store-use-case.png)

### <a name="create-and-config-azure-sql-database-for-the-custom-metastore"></a>Özel metastore için Azure SQL Veritabanı oluşturma ve config

HDInsight kümesi için özel bir Hive metastore'u kurmadan önce varolan bir Azure SQL Veritabanı oluşturmanız veya sahip olmanız gerekir.  Daha fazla bilgi için [Bkz. Quickstart: Azure SQL DB'de tek bir veritabanı oluşturun.](https://docs.microsoft.com/azure/sql-database/sql-database-single-database-get-started?tabs=azure-portal)

HDInsight kümenizin bağlı Azure SQL Veritabanı'na erişebilmesini sağlamak için Azure hizmetlerinin ve kaynaklarının sunucuya erişmesine izin verecek şekilde Azure SQL Veritabanı güvenlik duvarı kurallarını yapılandırın.

Bu seçeneği Azure portalında **Sunucu Güvenlik Duvarını Ayarla'yı**tıklatarak ve Azure hizmetlerine ve kaynaklarına izin vererek Azure SQL Veritabanı sunucusu veya veritabanı için **bu sunucuya erişmeye** hazırlayarak etkinleştirebilirsiniz. **ON** Daha fazla bilgi için IP [güvenlik duvarı kuralı oluştur ve yönetme](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure#use-the-azure-portal-to-manage-server-level-ip-firewall-rules)

![sunucu güvenlik duvarı düğmesini ayarlama](./media/hdinsight-use-external-metadata-stores/configure-azure-sql-database-firewall1.png)

![azure hizmetlerine erişime izin verme](./media/hdinsight-use-external-metadata-stores/configure-azure-sql-database-firewall2.png)

### <a name="select-a-custom-metastore-during-cluster-creation"></a>Küme oluşturma sırasında özel bir metastore seçin

Küme oluşturma sırasında kümenizi önceden oluşturulmuş bir Azure SQL Veritabanı'na yönlendirebilir veya küme oluşturulduktan sonra SQL Veritabanını yapılandırabilirsiniz. Bu seçenek, Azure portalından yeni bir Hadoop, Spark veya etkileşimli Hive kümesi oluştururken **Depolama > Metastore ayarlarıyla** belirtilir.

![HDInsight Hive Metadata Store Azure portalı](./media/hdinsight-use-external-metadata-stores/azure-portal-cluster-storage-metastore.png)

## <a name="hive-metastore-best-practices"></a>Kovan metastore en iyi uygulamalar

Burada bazı genel HDInsight Hive metastore en iyi uygulamaları şunlardır:

* Ayrı bilgi işlem kaynakları (çalışan küme) ve meta verileri (meta deposunda depolanan) yardımcı olmak için mümkün olduğunca özel bir metastore kullanın.

* 50 DTU ve 250 GB depolama alanı sağlayan bir S2 katmanı yla başlayın. Bir darboğaz görürseniz, veritabanını ölçeklendirebilirsiniz.

* Ayrı verilere erişmek için birden çok HDInsight kümesini planlıyorsanız, her kümedeki metastore için ayrı bir veritabanı kullanın. Bir metastore'u birden çok HDInsight kümesi nde paylaşıyorsanız, bu kümelerin aynı meta verileri ve altta yatan kullanıcı veri dosyalarını kullandığı anlamına gelir.

* Özel metastore'unuzu düzenli olarak yedekleyin. Azure SQL Veritabanı otomatik olarak yedekleme ler oluşturur, ancak yedekleme bekletme zaman dilimi değişir. Daha fazla bilgi için bkz: [Otomatik SQL Veritabanı yedeklemeleri hakkında bilgi edinin.](../sql-database/sql-database-automated-backups.md)

* En yüksek performans ve en düşük ağ çıkış ücretleri için metastore ve HDInsight kümenizi aynı bölgede bulun.

* Azure portalı veya Azure Monitor günlükleri gibi Azure SQL Veritabanı İzleme araçlarını kullanarak metamağazanızı performans ve kullanılabilirlik açısından izleyin.

* Azure HDInsight'ın yeni ve daha yüksek bir sürümü varolan bir metastore veritabanına karşı oluşturulduğunda, sistem veritabanını yedeklemeden geri döndürülemez olan metastore şemasını yükseltir.

* Birden çok küme arasında bir metastore paylaşıyorsanız, tüm kümelerin aynı HDInsight sürümü olduğundan emin olun. Farklı Hive sürümleri farklı metastore veritabanı şemaları kullanın. Örneğin, Hive 2.1 ve Hive 3.1 sürümlü kümeler arasında bir metastore paylaşamaz.

* HDInsight 4.0'da Spark ve Hive, SparkSQL veya Hive tablolarına erişmek için bağımsız kataloglar kullanır. Spark tarafından oluşturulan bir tablo Spark kataloğunda bulunur. Hive tarafından oluşturulan bir tablo Hive kataloğunda bulunur. Bu, Hive ve Spark'ın ortak kataloğu paylaştığı HDInsight 3.6'dan farklıdır. HDInsight 4.0'da Kovan ve Kıvılcım Tümleştirmesi Kovan Ambar Konektörüne (HWC) dayanır. HWC, Spark ve Hive arasında bir köprü olarak çalışıyor. [Kovan Depo Konektörü hakkında bilgi edinin.](../hdinsight/interactive-query/apache-hive-warehouse-connector.md)

## <a name="apache-oozie-metastore"></a>Apache Oozie metastore

Apache Oozie, Hadoop işlerini yöneten bir iş akışı koordinasyon sistemidir.  Oozie Apache MapReduce, Pig, Hive ve diğerleri için Hadoop işleri destekler.  Oozie, geçerli ve tamamlanan iş akışlarıyla ilgili ayrıntıları depolamak için bir metastore kullanır. Oozie kullanırken performansı artırmak için Azure SQL Veritabanı'nı özel bir metastore olarak kullanabilirsiniz. Metastore, kümenizi sildikten sonra Oozie iş verilerine de erişim sağlayabilir.

Azure SQL Veritabanı ile Bir Oozie metastore oluşturma yla ilgili talimatlar [için iş akışları için Apache Oozie'yi kullan'a](hdinsight-use-oozie-linux-mac.md)bakın.

## <a name="custom-ambari-db"></a>Özel Ambari DB

HDInsight'ta Apache Ambari ile kendi harici veritabanınızı kullanmak için [Özel Apache Ambari veritabanına](hdinsight-custom-ambari-db.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Hadoop, Apache Spark, Apache Kafka ve daha fazlasıyla HDInsight'ta küme oluşturma](./hdinsight-hadoop-provision-linux-clusters.md)
