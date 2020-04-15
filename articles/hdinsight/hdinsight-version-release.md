---
title: HDInsight 4.0 genel bakış - Azure
description: HDInsight 3.6 ile HDInsight 4.0 özelliklerinin karşılaştırılması, sınırlamalar ve yükseltme önerileri.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: d0fd9999abc4a67ded0f66977e1a3ba5310c87be
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81383040"
---
# <a name="azure-hdinsight-40-overview"></a>Azure HDInsight 4.0 genel bakış

Azure HDInsight, Apache Hadoop ve Apache Spark için kurumsal müşteriler arasında en popüler hizmetlerden biridir. HDInsight 4.0, Apache Hadoop bileşenlerinin bulut dağılımıdır. Bu makalede en güncel Azure HDInsight sürümü hakkında bilgiler verilmekte ve yükseltme yöntemleri anlatılmaktadır.

## <a name="whats-new-in-hdinsight-40"></a>HDInsight 4.0'daki yenilikler nelerdir?

### <a name="apache-hive-30-and-low-latency-analytical-processing"></a>Apache Hive 3.0 ve düşük gecikmeli analitik işleme

Apache Hive düşük gecikmeli analitik işleme (LLAP) kalıcı sorgu sunucuları ve bellek içi önbelleğe kullanır. Bu işlem, uzak bulut depolama veri hızlı SQL sorgu sonuçları sağlar. Hive LLAP, Hive sorgularının parçalarını çalıştıran kalıcı daemons kümesi kullanır. LLAP üzerinde sorgu yürütme LLAP kullanılmayan Hive ile benzerdir ve çalışan görevleri kapsayıcıların değil LLAP daemon'larının içinde çalışır.

Hive LLAP hizmetinin avantajları şunlardır:

* Performans tan ve adaptasyondan ödün vermeden derin SQL analizleri yapma becerisi. Karmaşık birleştirmeler, alt sorgular, pencereleme işlevleri, sıralama, kullanıcı tanımlı işlevler ve karmaşık toplamalar gibi.

* Etkileşimli sorguların verilerin hazırlandığı depolama alanındaki verilerle gerçekleştirilerek analitik işlem için verilerin depolamadan başka bir altyapıya taşınması ihtiyacınız ortadan kaldırma.

* Önbelleğe alma sorgusu sonuçları, önceden hesaplanmış sorgu sonuçlarının yeniden kullanılmasını sağlar. Bu önbellek, sorgu için gereken küme görevlerini çalıştırmak için harcanan zamandan ve kaynaklarından tasarruf sağlar.

### <a name="hive-dynamic-materialized-views"></a>Hive dinamik gerçekleştirilmiş görünümleri

Kovan şimdi dinamik materyalize görünümleri veya ilgili özetlerin önceden hesaplanmasını destekler. Görünümler, veri ambarlarında sorgu işlemeyi hızlandırır. Gerçekleştirilmiş görünümler yerel Hive ortamında depolanabilir ve LLAP hızlandırmasından sorunsuz bir şekilde faydalanabilir.

### <a name="hive-transactional-tables"></a>Hive işlem tabloları

HDI 4.0 Apache Hive 3 içerir. Kovan 3, Kovan ambarında yaşayan işlem tabloları için atomiklik, tutarlılık, izolasyon ve dayanıklılık uyumluluğu gerektirir. ACID uyumluluğuna sahip tablolar ve tablo verileri için erişim ve yönetim Hive tarafından gerçekleştirilir. Oluşturma, alma, güncelleştirme ve silme (CRUD) tablolarındaki veriler Optimize Edilmiş Satır Sütunu (ORC) dosya biçiminde olmalıdır. Yalnızca ekle tabloları tüm dosya biçimlerini destekler.

* ACID v2 hem depolama biçimi hem de yürütme altyapısı alanında performans geliştirmelerine sahiptir.

* ACID, veri güncelleştirmelerine tam destek sunmak için varsayılan olarak etkinleştirilir.

* Geliştirilmiş ACID özellikleri sayesinde satır düzeyinde güncelleştirme ve silme işlemi gerçekleştirebilirsiniz.

* Bu durum Performans açısından ek yük oluşturmaz.

* Gruplandırma gerekli değildir.

* Spark, Hive Warehouse Connector ile Hive ACID tablolarında veri okuma ve yazma işlemleri gerçekleştirebilir.

[Apache Hive 3](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/hive-overview/content/hive_whats_new_in_this_release_hive.html) hakkında daha fazla bilgi edinin.

### <a name="apache-spark"></a>Apache Spark

Apache Spark, güncelleştirilebilir tabloları ve ACID işlemlerini Hive Warehouse Connector ile alır. Hive Warehouse Connector, tam işlevlere erişmek için Hive işlem tablolarını Spark'ta dış tablo olarak kaydetmenize izin verir. Önceki sürümler yalnızca tablo bölümü değiştirmeyi destekliyordu. Kovan Ambarı Bağlayıcısı, Akış Veri Çerçevelerini de destekler.  Bu işlem, Spark'tan işlemsel ve akışlı Kovan tablolarına okur ve yazar.

Spark yürütücüleri doğrudan Hive LLAP daemon'larına bağlanarak verileri işlemsel bir şekilde alabilir ve bu sayede verilerin denetimi Hive'da kalır.

HDInsight 4.0'da Apache Spark şu senaryoları destekler:

* Raporlama için kullanılan işlem tablosunda makine öğrenmesi modeli eğitimi çalıştırma.
* ACID işlemlerini kullanarak Spark ML'den Hive tablosuna güvenli bir şekilde sütun ekleme.
* Hive akış tablosundaki değişiklik akışında bir Spark akış işi çalıştırma.
* Doğrudan bir Spark Yapılandırılmış Akış işinden ORC dosyası oluşturma.

Artık yanlışlıkla Doğrudan Spark'tan Hive işlem tablolarına erişmeye çalışma konusunda endişelenmenize gerek yok. Tutarsız sonuçlar, yinelenen veriler veya veri bozulması ile sonuçlanır. HDInsight 4.0'da, Kıvılcım tabloları ve Hive tabloları ayrı Metastore'larda saklanır. Hive Data Warehouse Connector ile Hive işlem tablolarını açıkça Spark dış tabloları olarak kaydedebilirsiniz.

[Apache Spark](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/spark-overview/content/analyzing_data_with_apache_spark.html) hakkında daha fazla bilgi edinin.

### <a name="apache-oozie"></a>Apaçi Oozie

HDI 4.0 sürümünde bulunan Apache Oozie 4.3.1'de aşağıdaki değişiklikler yapılmıştır:

* Oozie artık Hive eylemlerini çalıştırmaz. Hive CLI kaldırılmış ve yerine BeeLine getirilmiştir.

* **job.properties** dosyanıza bir hariç tutma deseni ekleyerek istenmeyen bağımlılıkları paylaşma kitaplığından hariç tutabilirsiniz.

[Apache Oozie](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/release-notes/content/patch_oozie.html) hakkında daha fazla bilgi edinin.

## <a name="how-to-upgrade-to-hdinsight-40"></a>HDInsight 4.0'a yükseltme

Üretim ortamında en son sürümü uygulamadan önce bileşenlerinizi iyice test edin. HDInsight 4.0 yükseltme işlemine başlamak için kullanılabilir. HDInsight 3.6, kazara aksilikleri önlemek için varsayılan seçenektir.

HDInsight'ın önceki sürümlerinden HDInsight 4.0'a kadar desteklenen bir yükseltme yolu yoktur. Metastore ve blob veri biçimleri değiştiğinden, 4.0 önceki sürümlerle uyumlu değildir. Yeni HDInsight 4.0 ortamınızı mevcut üretim ortamınızdan ayrı tutmanız önemlidir. HDInsight 4.0'ı mevcut ortamınıza dağıtırsanız, Metastore'unuzun kalıcı olarak yükseltilir.  

## <a name="limitations"></a>Sınırlamalar

* HDInsight 4.0, Apache Hive için MapReduce'ı desteklemez. Onun yerine Apaçi Tez'i kullan. [Apache Tez](https://tez.apache.org/) hakkında daha fazla bilgi edinin.
* HDInsight 4.0, Apache Storm'u desteklemiyor.
* Kovan Görünümü artık HDInsight 4.0'da kullanılamıyor.
* Apache Zeppelin'deki Shell yorumlayıcısı Spark ve Etkileşimli Sorgu kümelerinde desteklenmez.
* Spark-LLAP kümesinde LLAP özelliğini *devre dışı* bırakamazsınız. Yalnızca LLAP'yi kapatabilirsiniz.
* Azure Veri Gölü Depolama Gen2, Bir Kıvılcım kümesinde Jupyter dizüstü bilgisayarları kaydedemez.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight Belgeleri](index.yml)
* [Yayın Notları](hdinsight-release-notes.md)
