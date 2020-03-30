---
title: "Avantajlar: Şirket içinde Apache Hadoop'u Azure HDInsight'a geçirin"
description: Şirket içi Hadoop kümelerini Azure HDInsight'a geçirmenin motivasyonu ve avantajlarını öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: ashishth
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/15/2019
ms.openlocfilehash: 2440b93629416ea73fcf211cbe7bf5a3b72ab2e7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74267321"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---motivation-and-benefits"></a>Şirket içi Apache Hadoop kümelerini Azure HDInsight'a geçirin - motivasyon ve avantajlar

Bu makale, şirket içi Apache Hadoop eko-sistem dağıtımlarını Azure HDInsight'a geçirmek için en iyi uygulamalar la ilgili bir serinin ilkidir. Bu makale serisi, Azure HDInsight'taki Apache Hadoop çözümlerinin tasarımı, dağıtımı ve geçişinden sorumlu kişiler içindir. Bu makalelerden yararlanabilecek roller bulut mimarları, Hadoop yöneticileri ve DevOps mühendisleridir. Yazılım geliştiricileri, veri mühendisleri ve veri bilimciler de bulutta farklı küme türlerinin nasıl çalıştığına ilişkin açıklamalardan yararlanmalıdır.

## <a name="why-to-migrate-to-azure-hdinsight"></a>Azure HDInsight'a neden geçiş yapmak için

Azure HDInsight, Hadoop bileşenlerinin bulut dağıtımıdır. Azure HDInsight, devasa miktarlardaki verileri işlemeyi kolay, hızlı ve uygun maliyetli hale getirir. HDInsight gibi en popüler açık kaynak çerçeveleri içerir:

- Apache Hadoop
- Apache Spark
- LLAP ile Apache Hive
- Apache Kafka
- Apache Storm
- Apache HBase
- R

## <a name="azure-hdinsight-advantages-over-on-premises-hadoop"></a>Azure HDInsight, şirket içi Hadoop'a göre avantajlar

- **Düşük maliyet** - [Maliyetler, isteğe bağlı kümeler oluşturarak](../hdinsight-hadoop-create-linux-clusters-adf.md) ve yalnızca kullandığınız şey için ödeme yaparak azaltılabilir. Ayrılmış bilgi işlem ve depolama, veri hacmini küme boyutundan bağımsız tutarak esneklik sağlar.

- **Otomatik küme oluşturma** - Otomatik küme oluşturma en az kurulum ve yapılandırma gerektirir. Otomasyon isteğe bağlı kümeler için kullanılabilir.

- **Yönetilen donanım ve yapılandırma** - HDInsight kümesine sahip fiziksel donanım veya altyapı konusunda endişelenmenize gerek yoktur. Kümenin yapılandırmasını belirtmeniz ve Azure kümeyi ayarlar.

- **Kolayca ölçeklenebilir** - HDInsight, iş yüklerini yukarı veya aşağı [ölçeklendirmenize](../hdinsight-administer-use-portal-linux.md) olanak tanır. Azure, veri işleme işlerini kesintiye uğratmadan veri yeniden dağıtımı ve iş yükü yeniden dengeleme işlemlerini halledin.

- **Küresel kullanılabilirlik** - HDInsight, diğer büyük veri analizi tekliflerinden daha fazla [bölgede](https://azure.microsoft.com/regions/services/) kullanılabilir. Azure HDInsight ayrıca temel bağımsız bölgelerde kurumsal ihtiyaçlarınızı karşılamanıza olanak sağlayan Azure Kamu, Çin ve Almanya’da da kullanılabilir.

- **Güvenli ve uyumlu** - HDInsight, Azure Sanal [Ağı](../hdinsight-plan-virtual-network-deployment.md), [şifreleme](../hdinsight-hadoop-create-linux-clusters-with-secure-transfer-storage.md)ve [Azure Active Directory](../domain-joined/hdinsight-security-overview.md)ile tümleştirme ile kurumsal veri varlıklarınızı korumanızı sağlar. HDInsight aynı zamanda en popüler endüstri ve devlet [uyumluluk standartlarını](https://azure.microsoft.com/overview/trusted-cloud)da karşılamaktadır.

- **Basitleştirilmiş sürüm yönetimi** - Azure HDInsight, Hadoop ekosistem bileşenlerinin sürümünü yönetir ve güncel tutar. Yazılım güncelleştirmeleri genellikle şirket içi dağıtımlar için karmaşık bir işlemdir.

- **Bileşenler arasında daha az bağımlılık olan belirli iş yükleri için optimize edilmiş küçük kümeler** - Tipik bir şirket içi Hadoop kurulumu, birçok amahta hizmet eden tek bir küme kullanır. Azure HDInsight ile iş yüküne özel kümeler oluşturulabilir. Belirli iş yükleri için kümeler oluşturmak, büyüyen karmaşıklıkla tek bir kümeyi koruma nın karmaşıklığını ortadan kaldırır.

- **Verimlilik** - Tercih ettiğiniz geliştirme ortamında Hadoop ve Spark için çeşitli araçlar kullanabilirsiniz.

- **Özel araçlar veya üçüncü taraf uygulamalarla genişletilebilirlik** - HDInsight kümeleri yüklü bileşenlerle genişletilebilir ve Azure Market'ten [tek tıklamayla](https://azure.microsoft.com/services/hdinsight/partner-ecosystem/) dağıtımlar kullanılarak diğer büyük veri çözümleriyle de entegre edilebilir.

- **Kolay yönetim, yönetim ve izleme** - Azure HDInsight, tüm kümelerinizi izleyebileceğiniz tek bir arayüz sağlamak için [Azure Monitor günlükleriyle](../hdinsight-hadoop-oms-log-analytics-tutorial.md) tümleşir.

- **Diğer Azure hizmetleriyle tümleştirme** - HDInsight aşağıdakiler gibi diğer popüler Azure hizmetleriyle kolayca entegre edilebilir:

    - Azure Veri Fabrikası (ADF)
    - Azure Blob Depolama
    - Azure Data Lake Storage Gen2
    - Azure Cosmos DB
    - Azure SQL Veritabanı
    - Azure Analysis Services

- **Kendi kendini iyileştiren süreçler ve bileşenler** - HDInsight, kendi izleme altyapısını kullanarak altyapıyı ve açık kaynak bileşenlerini sürekli olarak kontrol eder. Ayrıca, açık kaynak bileşenlerinin ve düğümlerinin kullanılamaması gibi kritik hataları otomatik olarak kurtarır. Herhangi bir OSS bileşeni başarısız olursa Ambari'de uyarılar tetiklenir.

Daha fazla bilgi için Azure [HDInsight ve Apache Hadoop teknoloji yığını nedir makalesine](../hadoop/apache-hadoop-introduction.md)bakın.

## <a name="migration-planning-process"></a>Geçiş planlama süreci

Şirket içi Hadoop kümelerinin Azure HDInsight'a geçişini planlamak için aşağıdaki adımlar önerilir:

1. Mevcut şirket içi dağıtım ve topolojileri anlayın.
2. Geçerli proje kapsamını, zaman çizelgelerini ve takım uzmanlığını anlayın.
3. Azure gereksinimlerini anlayın.
4. En iyi uygulamalara dayalı ayrıntılı bir plan oluşturun.

## <a name="gathering-details-to-prepare-for-a-migration"></a>Göçe hazırlanmak için ayrıntıları toplama

Bu bölümde, ilgili önemli bilgiler toplamaya yardımcı olmak için şablon anketleri sağlandığıdır:

- Şirket içi dağıtım
- Proje ayrıntıları
- Azure gereksinimleri

### <a name="on-premises-deployment-questionnaire"></a>Şirket içi dağıtım anketi

| **Soru** | **Örnek** | **Cevap** |
|---|---|---|
|**Konu**: **Çevre**|||
|Küme Dağılımı sürümü|HDP 2.6.5, CDH 5.7|
|Büyük Veri eko-sistem bileşenleri|HDFS, İplik, Kovan, LLAP, Impala, Kudu, HBase, Kıvılcım, MapReduce, Kafka, Zookeeper, Solr, Sqoop, Oozie, Ranger, Atlas, Şahin, Zeppelin, R|
|Küme türleri|Hadoop, Kıvılcım, Confluent Kafka, Fırtına, Solr|
|Küme sayısı|4|
|Ana düğüm sayısı|2|
|İşçi düğümlerinin sayısı|100|
|Kenar düğümlerinin sayısı| 5|
|Toplam Disk alanı|100 TB|
|Ana Düğüm yapılandırması|m/y, işlemci, disk, vb.|
|Veri Düğümleri yapılandırması|m/y, işlemci, disk, vb.|
|Kenar Düğümleri yapılandırması|m/y, işlemci, disk, vb.|
|HDFS Şifreleme?|Evet|
|Yüksek Kullanılabilirlik|HDFS HA, Metastore HA|
|Olağanüstü Durum Kurtarma / Yedekleme|Yedek küme mi?|  
|Küme'ye bağımlı sistemler|SQL Server, Teradata, Güç BI, MongoDB|
|Üçüncü taraf tümleştirmeleri|Tableau, GridGain, Qubole, Informatica, Splunk|
|**Konu**: **Güvenlik**|||
|Çevre güvenliği|Güvenlik Duvarları|
|Küme kimlik doğrulama & yetkilendirme|Active Directory, Ambari, Cloudera Yöneticisi, Kimlik doğrulama yok|
|HDFS Erişim Kontrolü|  Manuel, ssh kullanıcıları|
|Kovan kimlik doğrulama & yetkilendirme|Nöbetçi, LDAP, Kerberos, Ranger ile AD|
|Denetim|Ambari, Cloudera Navigator, Korucu|
|İzleme|Grafit, toplanan, statsd, Telegraf, InfluxDB|
|Uyarı|Kapacitor, Prometheus, Datadog|
|Veri Saklama süresi| 3 yıl, 5 yıl|
|Küme Yöneticileri|Tek Yönetici, Birden Çok Yönetici|

### <a name="project-details-questionnaire"></a>Proje ayrıntıları anketi

|**Soru**|**Örnek**|**Cevap**|
|---|---|---|
|**Konu**: **İş Yükleri ve Sıklık**|||
|Mapİşleri azaltın|10 iş - günde iki kez||
|Kovan işleri|100 iş -- her saat||
|Toplu işleri kıvılcımla|50 iş -- her 15 dakikada bir||
|Kıvılcım Akış işleri|5 iş - her 3 dakikada bir||
|Yapılandırılmış Akış işleri|5 iş -- her dakika||
|ML Model eğitim işleri|2 iş - haftada bir kez||
|Programlama Dilleri|Python, Scala, Java||
|Betik Oluşturma|Kabuk, Piton||
|**Konu**: **Veri**|||
|Veri kaynakları|Düz dosyalar, Json, Kafka, RDBMS||
|Veri orkestrasyonu|Oozie iş akışları, Hava Akımı||
|Bellek aramalarında|Apaçi Ignite, Redis||
|Veri hedefleri|HDFS, RDBMS, Kafka, MPP ||
|**Konu**: **Meta verileri**|||
|Kovan DB tipi|Mysql, Postgres||
|Kovan metastore sayısı|2||
|Kovan tablolarının sayısı|100||
|Ranger politikalarının sayısı|20||
|Oozie iş akışlarının sayısı|100||
|**Konu**: **Ölçek**|||
|Çoğaltma dahil veri hacmi|100 TB||
|Günlük yutma hacmi|50 GB||
|Veri büyüme hızı|Yılda %10||
|Küme Düğümleri büyüme hızı|Yılda %5
|**Konu**: **Küme kullanımı**|||
|Kullanılan ortalama CPU %|%60||
|Ortalama Bellek % kullanılan|%75||
|Kullanılan disk alanı|%75||
|Ortalama Ağ % kullanılan|%25
|**Konu**: **Personel**|||
|Yönetici Sayısı|2||
|Geliştirici Sayısı|10||
|Son kullanıcı sayısı|100||
|Beceri|Hadoop, Kıvılcım||
|Geçiş çabaları için kullanılabilir kaynak sayısı|2||
|**Konu**: **Sınırlamalar**|||
|Geçerli sınırlamalar|Gecikme oranı yüksek||
|Güncel zorluklar|Eşzamanlılık sorunu||

### <a name="azure-requirements-questionnaire"></a>Azure gereksinimleri anketi

|**Konu**: **Altyapı** |||
|---|---|---|
|**Soru**|**Örnek**|**Cevap**|
| Tercih Edilen Bölge|ABD Doğu||
|VNet tercih?|Evet||
|HA / DR Gerekli?|Evet||
|Diğer bulut hizmetleriyle entegrasyon mu?|ADF, CosmosDB||
|**Konu**: **Veri Hareketi**  |||
|İlk yük tercihi|DistCp, Veri kutusu, ADF, WANDisco||
|Veri aktarım deltası|DistCp, AzCopy||
|Devam eden artımlı veri aktarımı|DistCp, Sqoop||
|**Konu**: **İzleme & Uyarı** |||
|Azure İzleme & Uyarı Vs Tümleştir üçüncü taraf izlemeyi kullanma|Azure İzleme & Uyarı'yı kullanma||
|**Konu**: **Güvenlik tercihleri** |||
|Özel ve korumalı veri boru hattı?|Evet||
|Etki Alanı Birleştirme kümesi (ESP)?|     Evet||
|Şirket Içi REKLAM Senkronizasyonu'ndan Buluta Mı?|     Evet||
|Eşitlenebilen AD kullanıcı sayısı?|          100||
|Parolaları buluta eşitleme de tamam mı?|    Evet||
|Bulut yalnızca Kullanıcılar mı?|                 Evet||
|MFA gerekli?|                       Hayır|| 
|Veri yetkilendirme gereksinimleri?|  Evet||
|Rol Tabanlı Erişim Kontrolü?|        Evet||
|Denetim gerekli mi?|                  Evet||
|Veri şifreleme sayılsın mı?|          Evet||
|Aktarımda veri şifreleme?|       Evet||
|**Konu**: **Yeniden Mimarlık tercihleri** |||
|Tek küme vs Belirli küme türleri|Belirli küme türleri||
|Colocated Depolama Vs Uzak Depolama?|Uzak Depolama||
|Veri uzaktan depolandıkça daha küçük küme boyutu mu?|Daha küçük küme boyutu||
|Tek bir büyük küme yerine birden çok küçük küme mi kullanalım?|Birden çok küçük küme kullanın||
|Uzak bir metastore mu kullanıyorsun?|Evet||
|Metamağazaları farklı kümeler arasında mı paylaşın?|Evet||
|İş yüklerini dekonstrükte etmek mi?|Kovan işlerini Kıvılcım işleri ile değiştirme||
|Veri düzenleme için ADF'yi mi kullanıyorsun?|Hayır||

## <a name="next-steps"></a>Sonraki adımlar

Bu serinin sonraki makaleyi okuyun:

- [Azure HDInsight Hadoop geçişi için şirket içi mimari en iyi uygulamalar](apache-hadoop-on-premises-migration-best-practices-architecture.md)
