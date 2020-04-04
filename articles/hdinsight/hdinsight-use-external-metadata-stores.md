---
title: Harici meta veri depolarını kullanma - Azure HDInsight
description: Azure HDInsight kümeleri ile harici meta veri depolarını kullanın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 04/03/2020
ms.openlocfilehash: 0cadf3930008868fe223e6e1024a2d14d17d8131
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80657118"
---
# <a name="use-external-metadata-stores-in-azure-hdinsight"></a>Azure HDInsight’ta dış meta veri depolarını kullanma

HDInsight, harici veri depolarıyla verilerinizin ve meta verilerinizin denetimini ele geçirmenizi sağlar. Bu özellik [Apache Hive metastore](#custom-metastore)için kullanılabilir , [Apache Oozie metastore](#apache-oozie-metastore), ve [Apache Ambari veritabanı](#custom-ambari-db).

HDInsight'taki Apache Hive metastore, Apache Hadoop mimarisinin önemli bir parçasıdır. Metastore merkezi şema deposudur. Metastore, Apache Spark, Interactive Query (LLAP), Presto veya Apache Pig gibi diğer büyük veri erişim araçları tarafından kullanılır. HDInsight, Hive metastore olarak bir Azure SQL Veritabanı kullanır.

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
Bu varsayılan metastore genellikle nispeten basit iş yükleri için kullanılır. Birden çok küme gerektirmeyen ve kümenin yaşam döngüsünün ötesinde korunmuş meta veriler gerektirmeyen iş yükleri.

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

HDInsight kümesi için özel bir Hive metastore'u kurmadan önce varolan bir Azure SQL Veritabanı oluşturun veya varolun.  Daha fazla bilgi için [Bkz. Quickstart: Azure SQL DB'de tek bir veritabanı oluşturun.](https://docs.microsoft.com/azure/sql-database/sql-database-single-database-get-started?tabs=azure-portal)

Azure SQL Veritabanı güvenlik duvarı kurallarını Azure hizmetlerinin ve kaynaklarının sunucuya erişmesine izin verecek şekilde yapılandırın. **Sunucu güvenlik duvarını**ayarla'yı seçerek Azure portalında bu seçeneği etkinleştirin. Ardından, Azure SQL Veritabanı sunucusu veya veritabanı için **bu sunucuya erişmek için Azure hizmetlerine ve kaynaklarına İzin Ver'in** altında **ON'u** seçin. Daha fazla bilgi için IP [güvenlik duvarı kuralı oluştur ve yönetme](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure#use-the-azure-portal-to-manage-server-level-ip-firewall-rules)

![sunucu güvenlik duvarı düğmesini ayarlama](./media/hdinsight-use-external-metadata-stores/configure-azure-sql-database-firewall1.png)

![azure hizmetlerine erişime izin verme](./media/hdinsight-use-external-metadata-stores/configure-azure-sql-database-firewall2.png)

### <a name="select-a-custom-metastore-during-cluster-creation"></a>Küme oluşturma sırasında özel bir metastore seçin

Kümenizi istediğiniz zaman önceden oluşturulmuş bir Azure SQL Veritabanı'na yönlendirebilirsiniz. Portal üzerinden küme oluşturma için, seçenek **Depolama > Metastore ayarlarından**belirtilir.

![HDInsight Hive Metadata Store Azure portalı](./media/hdinsight-use-external-metadata-stores/azure-portal-cluster-storage-metastore.png)

## <a name="hive-metastore-guidelines"></a>Kovan metastore yönergeleri

* Ayrı bilgi işlem kaynakları (çalışan küme) ve meta verileri (meta deposunda depolanan) yardımcı olmak için mümkün olduğunca özel bir metastore kullanın.

* 50 DTU ve 250 GB depolama alanı sağlayan bir S2 katmanı yla başlayın. Bir darboğaz görürseniz, veritabanını ölçeklendirebilirsiniz.

* Ayrı verilere erişmek için birden çok HDInsight kümesini planlıyorsanız, her kümedeki metastore için ayrı bir veritabanı kullanın. Bir metastore'u birden çok HDInsight kümesi nde paylaşıyorsanız, bu kümelerin aynı meta verileri ve altta yatan kullanıcı veri dosyalarını kullandığı anlamına gelir.

* Özel metastore'unuzu düzenli olarak yedekleyin. Azure SQL Veritabanı otomatik olarak yedekleme ler oluşturur, ancak yedekleme bekletme zaman dilimi değişir. Daha fazla bilgi için bkz: [Otomatik SQL Veritabanı yedeklemeleri hakkında bilgi edinin.](../sql-database/sql-database-automated-backups.md)

* Metastore ve HDInsight kümenizi aynı bölgede bulun. Bu yapılandırma en yüksek performansı ve en düşük ağ çıkış ücretlerini sağlar.

* Azure SQL Veritabanı İzleme araçlarını veya Azure Monitor günlüklerini kullanarak metamağazanızı performans ve kullanılabilirlik için izleyin.

* Azure HDInsight'ın yeni ve daha yüksek bir sürümü varolan özel bir metastore veritabanına karşı oluşturulduğunda, sistem metastore şemasını yükseltir. Yükseltme, veritabanını yedeklemeden geri almadan geri alınamaz.

* Birden çok küme arasında bir metastore paylaşıyorsanız, tüm kümelerin aynı HDInsight sürümü olduğundan emin olun. Farklı Hive sürümleri farklı metastore veritabanı şemaları kullanın. Örneğin, Hive 2.1 ve Hive 3.1 sürümlü kümeler arasında bir metastore paylaşamaz.

* HDInsight 4.0'da Spark ve Hive, SparkSQL veya Hive tablolarına erişmek için bağımsız kataloglar kullanır. Spark tarafından oluşturulan bir tablo Spark kataloğunda yaşar. Hive tarafından oluşturulan bir tablo Hive kataloğunda yaşar. Bu davranış, Hive ve Spark'ın ortak kataloğu paylaştığı HDInsight 3.6'dan farklıdır. HDInsight 4.0'da Kovan ve Kıvılcım Tümleştirmesi Kovan Ambar Konektörüne (HWC) dayanır. HWC, Spark ve Hive arasında bir köprü olarak çalışıyor. [Kovan Depo Konektörü hakkında bilgi edinin.](../hdinsight/interactive-query/apache-hive-warehouse-connector.md)

## <a name="apache-oozie-metastore"></a>Apache Oozie metastore

Apache Oozie, Hadoop işlerini yöneten bir iş akışı koordinasyon sistemidir. Oozie Apache MapReduce, Pig, Hive ve diğerleri için Hadoop işleri destekler.  Oozie, iş akışlarıyla ilgili ayrıntıları depolamak için bir metastore kullanır. Oozie kullanırken performansı artırmak için Azure SQL Veritabanı'nı özel bir metastore olarak kullanabilirsiniz. Metastore, kümenizi sildikten sonra Oozie iş verilerine erişim sağlar.

Azure SQL Veritabanı ile Bir Oozie metastore oluşturma yla ilgili talimatlar [için iş akışları için Apache Oozie'yi kullan'a](hdinsight-use-oozie-linux-mac.md)bakın.

## <a name="custom-ambari-db"></a>Özel Ambari DB

HDInsight'ta Apache Ambari ile kendi harici veritabanınızı kullanmak için [Özel Apache Ambari veritabanına](hdinsight-custom-ambari-db.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Hadoop, Apache Spark, Apache Kafka ve daha fazlasıyla HDInsight'ta küme oluşturma](./hdinsight-hadoop-provision-linux-clusters.md)
