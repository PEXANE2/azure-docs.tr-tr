---
title: Apache Hadoop bileşenleri ve sürümleri - Azure HDInsight
description: Azure HDInsight'ta Apache Hadoop bileşenlerini ve sürümlerini öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 02/26/2020
ms.openlocfilehash: 2321918e9eae63a71d136753657bd7259862c2d1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79272662"
---
# <a name="what-are-the-apache-hadoop-components-and-versions-available-with-hdinsight"></a>HDInsight ile kullanılabilen Apache Hadoop bileşenleri ve sürümleri nelerdir?

Microsoft Azure HDInsight'taki [Apache Hadoop](https://hadoop.apache.org/) ekosistem bileşenleri ve sürümlerinin yanı sıra Kurumsal Güvenlik Paketi hakkında bilgi edinin. Ayrıca, HDInsight'ta Hadoop bileşen sürümlerini nasıl kontrol edebilirsiniz öğrenin.

## <a name="apache-hadoop-components-available-with-different-hdinsight-versions"></a>Apache Hadoop bileşenleri farklı HDInsight sürümleri ile kullanılabilir

Azure HDInsight, herhangi bir zamanda dağıtılabilen birden çok Hadoop küme sürümlerini destekler. 4 Nisan 2017 itibariyle Azure HDInsight tarafından kullanılan varsayılan küme sürümü 3,6'dır.

HDInsight küme sürümleriyle ilişkili bileşen sürümleri aşağıdaki tabloda listelenmiştir:

> [!NOTE]  
> HDInsight hizmetiiçin varsayılan sürüm önceden haber vermeden değişebilir. Sürüm bağımlılığınız varsa, .NET SDK ile .NET SDK ve Azure Classic CLI ile kümelerinizi oluştururken HDInsight sürümünü belirtin.

| Bileşen              | HDInsight 4.0 | HDInsight 3.6 (Varsayılan)     |
|------------------------|---------------|-----------------------------|
| Apaçi Hadoop ve İplik | 3.1.1         | 2.7.3                       |
| Apaçi Tez             | 0.9.1         | 0.7.0                       |
| Apaçi Domuz             | 0.16.0        | 0.16.0                      |
| Apaçi Kovanı            | 3.1.0         | 2.1.0 (ESP dışı kümeler), 1.2.1 (ESP kümeleri)                |
| Apaçi Tez Hive2       | -             | 0.8.4                       |
| Apaçi Ranger          | 1.1.0         | 0.7.0                       |
| Apache HBase           | 2.0.2         | 1.1.2                       |
| Apaçi Sqoop           | 1.4.7         | 1.4.6                       |
| Apaçi Oozie           | 4.3.1         | 4.2.0                       |
| Apaçi Hayvanat Bahçesi Bekçisi       | 3.4.6         | 3.4.6                       |
| Apache Storm           | -             | 1.1.0                       |
| Apaçi Mahout          | -             | 0.9.0+                      |
| Apaçi Anka Kuşu         | 5             | 4.7.0                       |
| Apache Spark           | 2.3.1, 2.4    | 2.3.0, 2.2.0, 2.1.0         |
| Apaçi Livy            | 0,5           | 0.4, 0.4, 0.3               |
| Apache Kafka           | 1.1.1, 2.1    | 1.1, 1.0 * (Aşağıdaki Nota bakın) |
| Apaçi Ambari          | 2.7.0         | 2.6.0                       |
| Apaçi Zeppelin        | 0.8.0         | 0.7.3                       |
| Mono                   | 4.2.1         | 4.2.1                       |

> [!NOTE]
> Sistem performansı yla ilgili değerlendirmeler nedeniyle Kafka sürüm 0.10 desteği Mart 2019'da sona erdi.

## <a name="check-for-current-hadoop-component-version-information"></a>Geçerli Hadoop bileşen sürüm bilgilerini denetleyin

HDInsight küme sürümleriyle ilişkili Hadoop ekosistem bileşeni sürümleri, HDInsight güncellemeleriyle değişebilir. Hadoop bileşenlerini kontrol etmek ve bir küme için hangi sürümlerin kullanıldığını doğrulamak için Ambari REST API'yi kullanın. **GetComponentInformation** komutu hizmet bileşenleri hakkında bilgi alır. Ayrıntılar için [Apache Ambari belgelerine](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)bakın.

### <a name="release-notes"></a>Sürüm notları

HDInsight'ın en son sürümleriyle ilgili ek sürüm notları için [HDInsight sürüm notlarına](hdinsight-release-notes.md) bakın.

## <a name="supported-hdinsight-versions"></a>Desteklenen HDInsight sürümleri

### <a name="support-expiration-and-retirement-for-hdinsight-versions"></a>HDInsight sürümleri için son kullanma ve emekliliği destekleme

**Desteğin sona ermesi,** Microsoft'un artık belirtilen HDInsight sürümü için destek sağlamayacağı ve küme oluşturma için Azure portalı üzerinden artık kullanılabilir olmayacağı anlamına gelir. Ancak, bu sürümler Azure CLI veya çeşitli SDK'lar kullanılarak oluşturulabilir. 

HDInsight sürümünün **emekliye ayrılması,** varolan kümelerin olduğu gibi çalışmaya devam edeceği anlamına gelir. Ancak, bu sürümün yeni kümeleri hiçbir yolla oluşturulamaz (CLI ve SDK'lar dahil). Diğer kontrol düzlemi özellikleri (manuel ölçekleme ve Otomatik ölçekleme gibi) sürüm emekliliğinden sonra da çalışmayabilir. Destek, kullanımdan kaldırılan sürümler için kullanılamaz.

Aşağıdaki tablolarDA HDInsight sürümleri listeleneb.r Destek süresi ve emeklilik tarihleri de bilindiği nde sağlanır.

### <a name="available-versions"></a>Kullanılabilir sürümler

Aşağıdaki tabloda, Azure portalında kullanılabilen HDInsight sürümlerinin yanı sıra PowerShell ve .NET SDK gibi diğer dağıtım yöntemleri listelenmektedir.

| HDInsight sürümü | VM İŞLETİM SİSTEMİ | Sürüm tarihi | Destek son kullanma tarihi | Emeklilik tarihi | Yüksek kullanılabilirlik |  Azure portalında kullanılabilirlik |
| --- | --- | --- | --- | --- | --- | --- |
| HDInsight 4.0 |Ubuntu 16.0.4 LTS |Eylül 24, 2018 | | |Evet |Evet |
| HDInsight 3.6 |Ubuntu 16.0.4 LTS |4 Nisan 2017 | 31 Aralık 2020 |31 Aralık 2020 |Evet |Evet |

Kıvılcım 2.1, 2.2 & Kafka 1.0 desteği 30 Haziran 2020 tarihinde sona erecek.

> [!NOTE]  
> Bir sürüm için destek süresi dolduktan sonra, Microsoft Azure portalı üzerinden kullanılamayabilir. Ancak, küme sürümleri, sürüm `Version` emeklilik tarihine kadar Windows PowerShell [New-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) komutu ve .NET SDK'daki parametreyi kullanarak kullanılabilir olmaya devam eder.

### <a name="retired-versions"></a>Kullanımdan kaldırılan sürümler

Aşağıdaki tabloda, Azure portalında **bulunmayan** HDInsight sürümleri listelenmektedir.

| HDInsight sürümü | HDP versiyonu | VM İŞLETİM SİSTEMİ | Sürüm tarihi | Destek son kullanma tarihi | Emeklilik tarihi | Yüksek kullanılabilirlik |  Azure portalında kullanılabilirlik |
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 3.5 |HDP 2.5 |Ubuntu 16.0.4 LTS |Eylül 30, 2016 |Eylül 5, 2017 |28 Haziran 2018 |Evet |Hayır |
| HDInsight 3.4 |HDP 2.4 |Ubuntu 14.0.4 LTS |29 Mart 2016 |29 Aralık 2016 |9 Ocak 2018 |Evet |Hayır |
| HDInsight 3.3 |HDP 2.3 |Windows Server 2012 R2 |2 Aralık 2015 |27 Haziran 2016 |31 Temmuz 2018 |Evet |Hayır |
| HDInsight 3.3 |HDP 2.3 |Ubuntu 14.0.4 LTS |2 Aralık 2015 |27 Haziran 2016 |Temmuz 31, 2017 |Evet |Hayır |
| HDInsight 3.2 |HDP 2.2 |Ubuntu 12.04 LTS veya Windows Server 2012 R2 |Şubat 18, 2015 |1 Mart 2016 |1 Nisan 2017 |Evet |Hayır |
| HDInsight 3.1 |HDP 2.1 |Windows Server 2012 R2 |Haziran 24, 2014 |Mayıs 18, 2015 |Haziran 30, 2016 |Evet |Hayır |
| HDInsight 3.0 |HDP 2.0 |Windows Server 2012 R2 |Şubat 11, 2014 |Eylül 17, 2014 |30 Haziran 2015 |Evet |Hayır |
| HDInsight 2.1 |HDP 1.3 |Windows Server 2012 R2 |Ekim 28, 2013 |Mayıs 12, 2014 |Mayıs 31, 2015 |Evet |Hayır |
| HDInsight 1.6 |HDP 1.1 | |Ekim 28, 2013 |Nisan 26, 2014 |Mayıs 31, 2015 |Hayır |Hayır |

> [!NOTE]  
> HDInsight sürüm 2.1 ve sonraki sürümler için varsayılan olarak iki baş düğümü olan yüksek kullanılabilir kümeler dağıtılır. HDInsight sürüm 1.6 kümeleri için kullanılamazlar.

## <a name="enterprise-security-package-for-hdinsight"></a>HDInsight için Kurumsal Güvenlik Paketi

Kurumsal Güvenlik, küme oluşturma iş akışının bir parçası olarak HDInsight kümenize ekleyebileceğiniz isteğe bağlı bir pakettir. Kurumsal Güvenlik Paketi şunları destekler:

- Kimlik doğrulama için Active Directory ile tümleştirme.

    Geçmişte, yalnızca yerel bir yönetici kullanıcısı ve yerel bir SSH kullanıcısıyla HDInsight kümeleri oluşturabilirsiniz. Yerel yönetici kullanıcı tüm dosyalara, klasörlere, tablolara ve sütunlara erişebilir.  Kurumsal Güvenlik Paketi ile, HDInsight kümelerini şirket içi Active Directory, Azure Active Directory Domain Services veya Active Directory on IaaS'ı içeren kendi Active Dizininizle entegre ederek rol tabanlı erişim denetimini etkinleştirebilirsiniz sanal makine. Kümedeki etki alanı yöneticisi, kullanıcılara kümeye erişmek için kendi kurumsal (etki alanı) kullanıcı adını ve parolasını kullanma izni verebilir.

    Daha fazla bilgi için bkz.

    - [Etki alanı birleştirilmiş HDInsight kümeleri ile Apache Hadoop güvenliğine giriş](./domain-joined/hdinsight-security-overview.md)
    - [HDInsight'ta Azure etki alanına katılan Apache Hadoop kümelerini planlayın](./domain-joined/apache-domain-joined-architecture.md)
    - [Etki alanına katılmış korumalı alan ortamını yapılandırma](./domain-joined/apache-domain-joined-configure.md)
    - [Azure Active Directory Etki Alanı Hizmetlerini kullanarak Etki Alanı birleştirilmiş HDInsight kümelerini yapılandırma](./domain-joined/apache-domain-joined-configure-using-azure-adds.md)

- Veri yetkilendirmesi

  - Hive, Spark SQL ve İplik Kuyrukları için yetkilendirme için Apache Ranger ile entegrasyon.
  - Dosya ve klasörler üzerinde erişim denetimini ayarlayabilirsiniz.

    Daha fazla bilgi için bkz.

  - [Etki Alanına katılan HDInsight'ta Apache Hive ilkelerini yapılandırma](./domain-joined/apache-domain-joined-run-hive.md)

- Erişilenleri ve yapılandırılan ilkeleri izlemek için denetim günlüklerini görüntüleyin.

### <a name="supported-cluster-types"></a>Desteklenen küme türleri

Şu anda yalnızca aşağıdaki küme türleri Kurumsal Güvenlik Paketini destekler:

- Hadoop (yalnızca HDInsight 3.6)
- Spark
- Kafka
- HBase
- Interactive Query

### <a name="support-for-azure-data-lake-storage"></a>Azure Veri Gölü Depolama desteği

Kurumsal Güvenlik Paketi, Azure Veri Gölü Depolama'nın hem birincil depolama alanı hem de eklenti depolama alanı olarak kullanılmasını destekler.

### <a name="pricing-and-service-level-agreement"></a>Fiyatlandırma ve hizmet düzeyi sözleşmesi

Kurumsal Güvenlik Paketi için fiyatlandırma ve SLA hakkında daha fazla bilgi için [HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/)fiyatlandırması'na bakın.

## <a name="service-level-agreement-for-hdinsight-cluster-versions"></a>HDInsight küme sürümleri için hizmet düzeyi sözleşmesi

Hizmet düzeyi sözleşmesi (SLA) bir _destek penceresi_açısından tanımlanır. Destek penceresi, HDInsight küme sürümünün Microsoft Müşteri Hizmetleri ve Desteği tarafından desteklenme zamanıdır. Sürümde geçmiş bir _destek son kullanma tarihi_ varsa, HDInsight kümesi destek penceresinin dışındadır. Belirli bir HDInsight sürümü X için destek bitiş tarihi (daha yeni bir X+1 sürümü kullanılabilir hale geldiğinden sonra) aşağıdakilerin daha sonraki olarak hesaplanır:  

- Formül 1: HDInsight küme sürümü X'in yayımlandığı tarihe 180 gün ekleyin.
- Formül 2: HDInsight küme sürümü X+1'in Azure portalında kullanıma sunulduğu tarihe 90 gün ekleyin.

_Emeklilik tarihi,_ küme sürümünün HDInsight'ta oluşturulamayacağı tarihtir. 31 Temmuz 2017'den itibaren, bir HDInsight kümesini emeklilik tarihinden sonra yeniden boyutlandıramaz.

## <a name="hortonworks-release-notes-associated-with-hdinsight-versions"></a>Hortonworks HDInsight sürümleri ile ilişkili sürüm notları

Bölüm, HORTonworks Veri Platformu dağıtımları ve HDInsight ile kullanılan Apache bileşenleri için sürüm notları için bağlantılar sağlar.
* HDInsight küme sürümü 4.0 [Hortonworks Veri Platformu 3.0](https://docs.hortonworks.com/HDPDocuments/HDP3/HDP-3.0.0/release-notes/content/relnotes.html) dayalı bir Hadoop dağıtım kullanır
* HDInsight küme sürüm 3.6 [Hortonworks Veri Platformu 2.6](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.0/bk_release-notes/content/ch_relnotes.html)dayalı bir Hadoop dağıtım kullanır.
* HDInsight küme sürüm 3.5 [Hortonworks Veri Platformu 2.5](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.5.0/bk_release-notes/content/ch_relnotes_v250.html)dayalı bir Hadoop dağıtım kullanır. HDInsight küme sürümü 3.5, Azure portalında oluşturulan _varsayılan_ Hadoop kümesidir.
* HDInsight küme sürüm 3.4 [Hortonworks Veri Platformu 2.4](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.4.0/bk_HDP_RelNotes/content/ch_relnotes_v240.html)dayalı bir Hadoop dağıtım kullanır.
* HDInsight küme sürüm 3.3 [Hortonworks Veri Platformu 2.3](https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.3.0/bk_HDP_RelNotes/content/ch_relnotes_v230.html)dayalı bir Hadoop dağıtım kullanır.

  * [Apache Storm sürüm notları](https://storm.apache.org/2015/11/05/storm0100-released.html) Apache web sitesinde mevcuttur.
  * [Apache Hive sürüm notları](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12332384&styleName=Text&projectId=12310843) Apache web sitesinde mevcuttur.
* HDInsight küme sürüm 3.2 [Hortonworks Veri Platformu 2.2][hdp-2-2]dayalı bir Hadoop dağıtım kullanır.

  * Belirli Apache bileşenleri için sürüm notları aşağıdaki gibi mevcuttur: [Kovan 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310843&version=12326450), [Domuz 0.14](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310730&version=12326954), [HBase 0.98.4](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310753&version=12326810), [Phoenix 4.2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12315120&version=12327581), [M/R 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310941&version=12327180), [HDFS 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310942&version=12327181), [İpLik 2.6](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12313722&version=12327197), [Ortak](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12310240&version=12327179), [Tez 0.5.2](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314426&version=12328742), [Ambari 2.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12312020&version=12327486), [Fırtına 0.9.3](https://issues.apache.org/jira/secure/ReleaseNote.jspa?projectId=12314820&version=12327112), ve [Oozie 4.1.0](https://issues.apache.org/jira/secure/ReleaseNote.jspa?version=12324960&projectId=12311620).
* HDInsight küme sürüm 3.1 [Hortonworks Veri Platformu 2.1.7][hdp-2-1-7]dayalı bir Hadoop dağıtım kullanır. 7 Kasım 2014'ten önce oluşturulan HDInsight 3.1 kümeleri [Hortonworks Veri Platformu 2.1.1'e][hdp-2-1-1]dayanmaktadır.
* HDInsight küme sürüm 3.0 [Hortonworks Veri Platformu 2.0][hdp-2-0-8]dayalı bir Hadoop dağıtım kullanır.
* HDInsight küme sürüm 2.1 [Hortonworks Veri Platformu 1.3][hdp-1-3-0]dayalı bir Hadoop dağıtım kullanır.
* HDInsight küme sürüm 1.6 [Hortonworks Veri Platformu 1.1][hdp-1-1-0]dayalı bir Hadoop dağıtım kullanır.

## <a name="default-node-configuration-and-virtual-machine-sizes-for-clusters"></a>Kümeler için varsayılan düğüm yapılandırması ve sanal makine boyutları

Kümeniz için hangi sanal makine SUS'larının seçilen hakkında daha fazla bilgi için [Azure HDInsight küme yapılandırma ayrıntılarına](hdinsight-supported-node-configuration.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [HDInsight'ta Apache Hadoop, Spark ve daha fazlası için küme kurulumu](hdinsight-hadoop-provision-linux-clusters.md)
- [Windows PC'den HDInsight'ta Apache Hadoop'ta çalışın](hdinsight-hadoop-windows-tools.md)

[hdp-2-2]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.2.9/bk_HDP_RelNotes/content/ch_relnotes_v229.html

[hdp-2-1-7]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.7-Win/bk_releasenotes_HDP-Win/content/ch_relnotes-HDP-2.1.7.html

[hdp-2-1-1]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.1/bk_releasenotes_hdp_2.1/content/ch_relnotes-hdp-2.1.1.html

[hdp-2-0-8]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.8.0/bk_releasenotes_hdp_2.0/content/ch_relnotes-hdp2.0.8.0.html

[hdp-1-3-0]: https://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.3.0_1.html

[hdp-1-1-0]: https://docs.hortonworks.com/HDPDocuments/HDP1/HDP-1.3.0/bk_releasenotes_hdp_1.x/content/ch_relnotes-hdp1.1.1.16_1.html
