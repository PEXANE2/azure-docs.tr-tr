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
ms.openlocfilehash: 337d36d8f5e7a7b69b98de2f68ff372d4914f905
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82084779"
---
# <a name="what-are-the-apache-hadoop-components-and-versions-available-with-hdinsight"></a>HDInsight ile kullanılabilen Apache Hadoop bileşenleri ve sürümleri nelerdir?

Microsoft Azure HDInsight'taki [Apache Hadoop](https://hadoop.apache.org/) ortamı bileşenleri ve sürümleri ve Kurumsal Güvenlik Paketi hakkında bilgi edinin. Ayrıca, HDInsight'ta Hadoop bileşen sürümlerini nasıl kontrol edebilirsiniz öğrenin.

## <a name="apache-hadoop-components-available-with-different-hdinsight-versions"></a>Apache Hadoop bileşenleri farklı HDInsight sürümleri ile kullanılabilir

Azure HDInsight, herhangi bir zamanda dağıtılabilen birden çok Hadoop küme sürümlerini destekler. 4 Nisan 2017'de Azure HDInsight tarafından kullanılan varsayılan küme sürümü 3,6'dır.

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

HDInsight küme sürümleriyle ilişkili Hadoop ortamı bileşen sürümleri, HDInsight güncellemeleriyle değişebilir. Hadoop bileşenlerini kontrol etmek ve bir küme için hangi sürümlerin kullanıldığını doğrulamak için Ambari REST API'yi kullanın. **GetComponentInformation** komutu hizmet bileşenleri hakkında bilgi alır. Ayrıntılar için [Apache Ambari belgelerine](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)bakın.

### <a name="release-notes"></a>Sürüm notları

HDInsight'ın en son sürümleriyle ilgili ek sürüm notları için [HDInsight sürüm notlarına](hdinsight-release-notes.md) bakın.

## <a name="supported-hdinsight-versions"></a>Desteklenen HDInsight sürümleri

### <a name="support-expiration-and-retirement-for-hdinsight-versions"></a>HDInsight sürümleri için son kullanma ve emekliliği destekleme

**Desteğin sona ermesi,** Microsoft'un artık belirtilen HDInsight sürümü için destek sağlamayacağı anlamına gelir. Ayrıca, küme oluşturma için Azure portalı üzerinden artık kullanılamayalacaktır. Ancak, bu sürümler Azure CLI veya çeşitli SDK'lar kullanılarak oluşturulabilir.

HDInsight sürümünün **emekliye ayrılması,** varolan kümelerin olduğu gibi çalışmaya devam edeceği anlamına gelir. Ancak, bu sürümün yeni kümeleri hiçbir yolla oluşturulamaz (CLI ve SDK'lar dahil). Diğer kontrol düzlemi özellikleri (manuel ölçekleme ve Otomatik ölçekleme gibi) sürüm emekliliğinden sonra da çalışmayabilir. Destek, kullanımdan kaldırılan sürümler için kullanılamaz.

Aşağıdaki tablolarDA HDInsight sürümleri listeleneb.r Destek süresi ve emeklilik tarihleri de bilindiği nde sağlanır.

### <a name="available-versions"></a>Kullanılabilir sürümler

Aşağıdaki tabloda, Azure portalında bulunan HDInsight sürümleri ve PowerShell ve .NET SDK gibi diğer dağıtım yöntemleri listelenmektedir.

| HDInsight sürümü | VM İŞLETİM SİSTEMİ | Sürüm tarihi | Destek son kullanma tarihi | Emeklilik tarihi | Yüksek kullanılabilirlik |  Azure portalında kullanılabilirlik |
| --- | --- | --- | --- | --- | --- | --- |
| HDInsight 4.0 |Ubuntu 16.0.4 LTS |Eylül 24, 2018 | | |Evet |Evet |
| HDInsight 3.6 |Ubuntu 16.0.4 LTS |4 Nisan 2017 | 31 Aralık 2020 |31 Aralık 2020 |Evet |Evet |

Kıvılcım 2.1, 2.2 & Kafka 1.0 desteği 30 Haziran 2020 tarihinde sona erecek.

> [!NOTE]  
> Bir sürüm için destek süresi dolduktan sonra, Microsoft Azure portalı üzerinden kullanılamayabilir. Ancak, küme sürümleri, sürüm `Version` emeklilik tarihine kadar Windows PowerShell [New-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) komutu ve .NET SDK'daki parametreyi kullanarak kullanılabilir olmaya devam eder.

### <a name="retired-versions"></a>Kullanımdan kaldırılan sürümler

Aşağıdaki tabloda, AZURE portalında **bulunmayan** HDInsight sürümleri listelenmektedir.

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

    Geçmişte, yerel yönetici kullanıcısı ve yerel SSH kullanıcısıyla HDInsight kümeleri oluşturdunuz. Yerel yönetici kullanıcı tüm dosyalara, klasörlere, tablolara ve sütunlara erişebilir.  Kurumsal Güvenlik Paketi ile HDInsight'ı Active Dizininizle bütünleştirerek rol tabanlı erişim denetimini etkinleştirebilirsiniz. Şirket içi Active Directory, Azure Active Directory Etki Alanı Hizmetleri içerir. Veya IaaS sanal makinede Active Directory. Kümedeki etki alanı yöneticisi, kullanıcılara kendi kurumsal (etki alanı) kullanıcı adı ve parolalarını kullanma izni verebilir.

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

### <a name="pricing-and-service-level-agreement-sla"></a>Fiyatlandırma ve hizmet düzeyi sözleşmesi (SLA)

Kurumsal Güvenlik Paketi için fiyatlandırma ve SLA hakkında daha fazla bilgi için [HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/)fiyatlandırması'na bakın.

## <a name="service-level-agreement-for-hdinsight-cluster-versions"></a>HDInsight küme sürümleri için hizmet düzeyi sözleşmesi

Hizmet düzeyi sözleşmesi (SLA) bir _destek penceresi_olarak tanımlanır. Destek penceresi, HDInsight sürümünün desteklediği zaman `Microsoft Customer Service and Support`dilimidir. Sürümde geçmiş bir _destek son kullanma tarihi_varsa, HDInsight kümesi destek penceresinin dışındadır. HDInsight sürüm X için destek sona ermesi (daha yeni bir X+1 sürümü kullanılabilir hale geldiğinden sonra) daha sonraki sürümdür:  

- Formül 1: HDInsight küme sürümü X'in yayımlandığı tarihe 180 gün ekleyin.
- Formül 2: HDInsight küme sürümü X+1'in Azure portalında kullanıma sunulduğu tarihe 90 gün ekleyin.

_Emeklilik tarihi,_ küme sürümünün HDInsight'ta oluşturulamayacağı tarihtir. 31 Temmuz 2017'den itibaren, bir HDInsight kümesini emeklilik tarihinden sonra yeniden boyutlandıramaz.

## <a name="default-node-configuration-and-virtual-machine-sizes-for-clusters"></a>Kümeler için varsayılan düğüm yapılandırması ve sanal makine boyutları

Kümeniz için hangi sanal makine SUS'larının seçilen hakkında daha fazla bilgi için [Azure HDInsight küme yapılandırma ayrıntılarına](hdinsight-supported-node-configuration.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [HDInsight'ta Apache Hadoop, Spark ve daha fazlası için küme kurulumu](hdinsight-hadoop-provision-linux-clusters.md)
- [Windows PC'den HDInsight'ta Apache Hadoop'ta çalışın](hdinsight-hadoop-windows-tools.md)
- [Hortonworks, Azure HDInsight sürümleriyle ilişkili sürüm notları](./hortonworks-release-notes.md)
