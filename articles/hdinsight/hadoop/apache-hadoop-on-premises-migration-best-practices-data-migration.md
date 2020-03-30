---
title: "Veri geçişi: Şirket içi Apache Hadoop'dan Azure HDInsight'a"
description: Şirket içi Hadoop kümelerini Azure HDInsight'a geçirmek için veri geçişi nin en iyi uygulamalarını öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: ashishth
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/22/2019
ms.openlocfilehash: 41112359408497d84243ed9bb06f396acf008dc5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74666010"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---data-migration-best-practices"></a>Şirket içi Apache Hadoop kümelerini Azure HDInsight'a geçirin - veri geçişi en iyi uygulamalar

Bu makalede, Azure HDInsight'a veri geçişi için öneriler sunulur. Şirket içi Apache Hadoop sistemlerini Azure HDInsight'a geçirmede yardımcı olmak için en iyi uygulamaları sağlayan bir serinin parçasıdır.

## <a name="migrate-on-premises-data-to-azure"></a>Şirket içi verileri Azure'a geçirin

Verileri şirket içinde Azure ortamına geçirmek için iki ana seçenek vardır:

* TLS ile ağ üzerinden veri aktarımı
    * Internet üzerinden - Azure Depolama Gezgini, AzCopy, Azure Powershell ve Azure CLI gibi çeşitli araçlardan herhangi birini kullanarak verileri normal bir internet bağlantısı üzerinden Azure depolamaalanına aktarabilirsiniz. Daha fazla bilgi için bkz: [Azure Depolama'ya veri taşıma ve](../../storage/common/storage-moving-data.md)

    * Express Route - ExpressRoute, Microsoft veri merkezleri ve binalarınızda veya bir birlikte konumlandırma tesisinde bulunan altyapı arasında özel bağlantılar oluşturmanıza olanak tanıyan bir Azure hizmetidir. ExpressRoute bağlantıları genel Internet üzerinden gitmez ve Internet üzerinden tipik bağlantılara göre daha düşük gecikme süreleri ile daha yüksek güvenlik, güvenilirlik ve hız sunar. Daha fazla bilgi için [expressroute devresi oluştur ve değiştir'](../../expressroute/expressroute-howto-circuit-portal-resource-manager.md)e bakın.

    * Veri Kutusu çevrimiçi veri aktarımı - Data Box Edge ve Data Box Gateway, siteniz le Azure arasındaki verileri yönetmek için ağ depolama ağ geçidi görevi yapan çevrimiçi veri aktarım ürünleridir. Şirket içi bir ağ aygıtı olan Data Box Edge, verileri Azure'a ve Azure'dan aktarAn ve verileri işlemek için yapay zeka (AI) özellikli kenar bilgi işlemini kullanır. Data Box Gateway, depolama ağ geçidi özelliklerine sahip sanal bir cihazdır. Daha fazla bilgi için Azure [Veri Kutusu Belgeleri - Çevrimiçi Aktarım'a](https://docs.microsoft.com/azure/databox-online/)bakın.

* Gönderi verileri Çevrimdışı

    Data Box çevrimdışı veri aktarımı - Veri Kutusu, Veri Kutusu Diski ve Veri Kutusu Ağır aygıtları, ağ bir seçenek olmadığında büyük miktarda veriyi Azure'a aktarmanıza yardımcı olur. Bu çevrimdışı veri aktarım aygıtları kuruluşunuz ve Azure veri merkezi arasında sevk edilir. Verilerinizin aktarım sırasında korunmasına yardımcı olmak için AES şifrelemesi kullanırlar ve verilerinizi cihazdan silmek için yükleme sonrası temizleme işleminden geçerler. Veri Kutusu çevrimdışı aktarım aygıtları hakkında daha fazla bilgi için [Azure Veri Kutusu Belgeleri - Çevrimdışı Aktarım'a](https://docs.microsoft.com/azure/databox/)bakın. Hadoop kümelerinin geçişi hakkında daha fazla bilgi için, [şirket içi bir HDFS deposundan Azure Depolama'ya geçiş yapmak için Azure Veri Kutusu'yu kullan'a](../../storage/blobs/data-lake-storage-migrate-on-premises-hdfs-cluster.md)bakın.

Aşağıdaki tablo, veri hacmine ve ağ bant genişliğine bağlı olarak yaklaşık veri aktarım süresine sahiptir. Veri geçişinin üç haftadan uzun sürmesi bekleniyorsa bir Veri kutusu kullanın.

|Veri Qty|Ağ Bant Genişliği||||
|---|---|---|---|---|
|| **45 Mbps (T3)**|**100 Mbps**|**1 Gbps**|**10 Gbps**|
|1 TB|2 gün|1 gün| 2 saat|14 dakika|
|10 TB|22 gün|10 gün|1 gün|2 saat|
|35 TB|76 gün|34 gün|3 gün|8 saat|
|80 TB|173 gün|78 gün|8 gün|19 saat|
|100 TB|216 gün|97 gün|10 gün|1 gün|
|200 TB|1 yıl|194 gün|19 gün|2 gün|
|500 TB|3 yıl|1 yıl|49 gün|5 gün|
|1 PB|6 yıl|3 yıl|97 gün|10 gün|
|2 PB|12 yıl|5 yıl|194 gün|19 gün|

Apache Hadoop DistCp, Azure Veri Fabrikası ve AzureCp gibi Azure'a özgü araçlar ağ üzerinden veri aktarmak için kullanılabilir. Üçüncü taraf aracı WANDisco da aynı amaç için kullanılabilir. Apache Kafka Mirrormaker ve Apache Sqoop, şirket içi azure depolama sistemlerine sürekli veri aktarımı için kullanılabilir.

## <a name="performance-considerations-when-using-apache-hadoop-distcp"></a>Apache Hadoop DistCp kullanırken performans hususları

DistCp, verileri aktarmak, hataları işlemek ve bu hatalardan kurtarmak için MapReduce Map işini kullanan bir Apache projesidir. Her Harita görevine kaynak dosyaların listesini atar. Harita görevi daha sonra atanan tüm dosyaları hedefe kopyalar. DistCp performansını artırabilir çeşitli teknikler vardır.

### <a name="increase-the-number-of-mappers"></a>Mappers sayısını artırmak

DistCp, her birinin kabaca aynı sayıda bayt kopyalaması için eşlemi görevleri oluşturmaya çalışır. Varsayılan olarak, DistCp işleri 20 mappers kullanın. Distcp için daha fazla Mappers kullanmak (komut satırında 'm' parametresi ile) veri aktarım işlemi sırasında paralelliği artırır ve veri aktarım süresini azaltır. Ancak, Mappers sayısını artırırken göz önünde bulundurulması gereken iki şey vardır:

* DistCp'nin en düşük parçalı lık tek bir dosyadır. Kaynak dosya sayısından daha fazla Mappers bir dizi belirtilmesi yardımcı olmaz ve kullanılabilir küme kaynakları atık.

* Mappers sayısını belirlemek için kümeüzerinde kullanılabilir İplik bellek düşünün. Her Harita görevi bir İplik konteyner olarak başlatılır. Kümede başka ağır iş yüklerinin çalışmadığını varsayarsak, Mappers sayısı aşağıdaki formülle belirlenebilir: m \* = (her işçi düğümü için işçi düğümleri iplik belleği sayısı) / İplik kapsayıcı boyutu. Ancak, diğer uygulamalar bellek kullanıyorsa, DistCp işleri için İplik belleği yalnızca bir bölümünü kullanmayı seçin.

### <a name="use-more-than-one-distcp-job"></a>Birden fazla DistCp işi kullanma

Taşınacak veri kümesinin boyutu 1 TB'den büyükse, birden fazla DIsCp işi kullanın. Birden fazla iş kullanmak hataların etkisini sınırlar. Herhangi bir iş başarısız olursa, tüm işler yerine yalnızca belirli bir işi yeniden başlatmanız gerekir.

### <a name="consider-splitting-files"></a>Dosyaları bölmeyi düşünün

Az sayıda büyük dosya varsa, daha fazla Mappers ile daha fazla potansiyel eşzamanlılık elde etmek için bunları 256 MB dosya parçalarına bölmeyi düşünün.

### <a name="use-the-strategy-command-line-parameter"></a>'Strateji' komut satırı parametresini kullanma

Komut `strategy = dynamic` satırında parametre kullanmayı düşünün. Parametrenin `strategy` varsayılan değeri, `uniform size`bu durumda her harita nın kabaca aynı sayıda bayt kopyaladığı değerdir. Bu parametre `dynamic`değiştirildiğinde, listeleme dosyası birkaç "yığın dosyasına" bölünür. Yığın dosyalarının sayısı, eşlem sayısının bir katıdır. Her harita görevi, yığın dosyalarından birine atanır. Bir yığındaki tüm yollar işlendikten sonra, geçerli öbek silinir ve yeni bir yığın elde edilir. İşlem, başka parça bulunana kadar devam eder. Bu "dinamik" yaklaşım, daha hızlı harita görevlerinin daha yavaş olanlardan daha fazla yol tüketmesine olanak sağlayarak Genel olarak DistCp işini hızlandırıyor.

### <a name="increase-the-number-of-threads"></a>İş parçacığı sayısını artırma

Parametreyi `-numListstatusThreads` artırmanın performansı artırıp artırmayacamaya bakın. Bu parametre, dosya listeleme oluşturmak için kullanılacak iş parçacığı sayısını denetler ve 40 maksimum değerdir.

### <a name="use-the-output-committer-algorithm"></a>Çıktı committer algoritmasını kullanma

Parametreyi `-Dmapreduce.fileoutputcommitter.algorithm.version=2` geçirmenin DistCp performansını artırıp artırmayaca bak. Bu çıktı committer algoritması hedefe çıkış dosyaları yazma etrafında optimizasyonlar vardır. Aşağıdaki komut, farklı parametrelerin kullanımını gösteren bir örnektir:

```bash
hadoop distcp -Dmapreduce.fileoutputcommitter.algorithm.version=2 -numListstatusThreads 30 -m 100 -strategy dynamic hdfs://nn1:8020/foo/bar wasb://<container_name>@<storage_account_name>.blob.core.windows.net/foo/
```

## <a name="metadata-migration"></a>Meta veri geçişi

### <a name="apache-hive"></a>Apaçi Kovanı

Kovan metadeposu komut dosyaları kullanılarak veya DB Çoğaltma kullanılarak geçirilebilir.

#### <a name="hive-metastore-migration-using-scripts"></a>Komut dosyalarını kullanarak kovan metastore geçişi

1. Tesislerinde Hive metastore gelen Hive DDLs oluşturun. Bu adım bir [sarmalayıcı bash komut dosyası](https://github.com/hdinsight/hdinsight.github.io/blob/master/hive/hive-export-import-metastore.md)kullanılarak yapılabilir.
1. HDFS url'sini WASB/ADLS/ABFS URL'leri ile değiştirmek için oluşturulan DDL'yi edin.
1. GÜNCELLEŞTIRILMIŞ DDL'yi HDInsight kümesinden metastore'da çalıştırın.
1. Hive metastore sürümünün şirket içi ve bulut arasında uyumlu olduğundan emin olun.

#### <a name="hive-metastore-migration-using-db-replication"></a>DB çoğaltma kullanarak kovan metastore geçiş

- Şirket içi Hive metastore DB ve HDInsight metastore DB arasında Veritabanı Çoğaltma'yı ayarlayın.
- ÖRNEĞIN, HDFS url'sini WASB/ADLS/ABFS url'leri ile değiştirmek için "Hive MetaTool"u kullanın:

    ```bash
    ./hive --service metatool -updateLocation hdfs://nn1:8020/ wasb://<container_name>@<storage_account_name>.blob.core.windows.net/
    ```

### <a name="apache-ranger"></a>Apaçi Ranger

- Şirket içi Ranger politikalarını xml dosyalarına aktarın.
- XSLT gibi bir aracı kullanarak şirket içi hdfs tabanlı yolları WASB/ADLS'e dönüştürün.
- Politikaları HDInsight'ta çalışan Ranger'a aktarın.

## <a name="next-steps"></a>Sonraki adımlar

Bu serinin sonraki makaleyi okuyun:

- [Güvenlik ve DevOps, Azure HDInsight Hadoop geçişi için şirket içi için en iyi uygulamalar](apache-hadoop-on-premises-migration-best-practices-security-devops.md)
