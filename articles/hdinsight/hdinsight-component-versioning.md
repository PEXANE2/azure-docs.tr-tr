---
title: Apache Hadoop bileşenleri ve sürümleri-Azure HDInsight
description: Azure HDInsight 'ta Apache Hadoop bileşenleri ve sürümleri hakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017,seoapr2020
ms.date: 05/20/2020
ms.openlocfilehash: 5f3b1fcb573358160707c0b29ad9e1806ea8e24c
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83774297"
---
# <a name="apache-components-and-versions-available-for-azure-hdinsight"></a>Azure HDInsight için kullanılabilen Apache bileşenleri ve sürümleri

Bu makalede, Azure HDInsight ve Kurumsal Güvenlik Paketi [Apache Hadoop](https://hadoop.apache.org/) ortamı bileşenleri ve sürümleri hakkında bilgi edineceksiniz. HDInsight 'ta Hadoop bileşen sürümlerini nasıl denetleyeceğinizi de öğreneceksiniz.

## <a name="apache-components-available-with-different-hdinsight-versions"></a>Farklı HDInsight sürümleriyle kullanılabilen Apache bileşenleri

Azure HDInsight, dilediğiniz zaman dağıtılabilecek birden çok Hadoop küme sürümünü destekler. 4 Nisan 2017 ' de, Azure HDInsight tarafından kullanılan varsayılan küme sürümü 3,6 idi.

HDInsight küme sürümleriyle ilişkili bileşen sürümleri aşağıdaki tabloda listelenmiştir.

> [!NOTE]
> HDInsight hizmeti için varsayılan sürüm bildirimde bulunulmadan değişebilir. Bir sürüm bağımlılığıdır varsa, Azure PowerShell ve Azure klasik CLı ile .NET SDK ile kümelerinizi oluştururken HDInsight sürümünü belirtin.

| Bileşen              | HDInsight 4.0 | HDInsight 3,6 (varsayılan)     |
|------------------------|---------------|-----------------------------|
| Apache Hadoop ve YARN | 3.1.1         | 2.7.3                       |
| Apache Tez             | 0.9.1         | 0.7.0                       |
| Apache Pig             | 0.16.0        | 0.16.0                      |
| Apache Hive            | 3.1.0         | 1.2.1 (2.1.0 Interactive sorgusunda) |
| Apache Tez Hive2       | -             | 0.8.4                       |
| Apache Ranger          | 1.1.0         | 0.7.0                       |
| Apache HBase           | 2.0.2         | 1.1.2                       |
| Apache Sqoop           | 1.4.7         | 1.4.6                       |
| Apache Oozie           | 4.3.1         | 4.2.0                       |
| Apache Zookeeper       | 3.4.6         | 3.4.6                       |
| Apache Storm           | -             | 1.1.0                       |
| Apache Mahout          | -             | 0.9.0 +                      |
| Apache Phoenix         | 5             | 4.7.0                       |
| Apache Spark           | 2,4, 2.3.1 *   | 2.3.0, 2.2.0, 2.1.0         |
| Apache Livy            | 0,5           | 0,4, 0,4, 0,3               |
| Apache Kafka           | 1.1.1, 2,1    | 1,1, 1,0 * (nota bakın.)   |
| Apache ambarı          | 2.7.0         | 2.6.0                       |
| Apache Zeppelin        | 0.8.0         | 0.7.3                       |
| Mono                   | 4.2.1         | 4.2.1                       |

> [!NOTE]
> Kafka sürüm 0,10 desteği, sistem performansı konuları nedeniyle 2019 Mart 'ta zaman aşımına uğradı.

## <a name="check-for-current-apache-component-version-information"></a>Geçerli Apache bileşen sürümü bilgilerini denetle

HDInsight küme sürümleriyle ilişkili Hadoop ortamı bileşen sürümleri, HDInsight güncelleştirmeleriyle değiştirilebilir. Hadoop bileşenlerini denetlemek ve bir küme için hangi sürümlerin kullanıldığını doğrulamak için, ambarı REST API kullanın. **Getcomponentınformation** komutu, hizmet bileşenleriyle ilgili bilgileri alır. Daha fazla bilgi için bkz. [Apache ambarı belgeleri](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

### <a name="release-notes"></a>Sürüm notları

HDInsight 'ın en son sürümlerinde ek sürüm notları için bkz. [HDInsight sürüm notları](hdinsight-release-notes.md).

## <a name="supported-hdinsight-versions"></a>Desteklenen HDInsight sürümleri

### <a name="support-expiration-and-retirement-for-hdinsight-versions"></a>HDInsight sürümleri için kullanım süresi ve kullanımdan kaldırma desteği

**Destek sona erme tarihi** , Microsoft 'un belirli HDInsight sürümü için destek sunmayacağı anlamına gelir. Artık küme oluşturma için Azure portal aracılığıyla kullanılamaz. Bu sürümler yine de Azure CLı veya çeşitli SDK 'lar kullanılarak oluşturulabilir.

**Kullanımdan** kaldırma, HDInsight sürümünün mevcut kümelerinin olduğu gibi çalışmaya devam etmesi anlamına gelir. Bu sürümdeki yeni kümeler CLı ve SDK 'Ları içeren herhangi bir yöntemle oluşturulamaz. El ile ölçekleme ve otomatik ölçekleme gibi diğer denetim düzlemi özellikleri, sürüm kullanımdan kaldırıldıktan sonra da çalışmayabilir. Kullanımdan kaldırılan sürümler için destek kullanılamaz.

Aşağıdaki tablolar HDInsight sürümlerini listeler. Destek süre sonu ve devre dışı bırakma tarihleri bilindiğinde de sağlanır.

### <a name="available-versions"></a>Kullanılabilir sürümler

Bu tabloda, Azure portal ve PowerShell ve .NET SDK gibi diğer dağıtım yöntemleri aracılığıyla bulunan HDInsight sürümleri listelenir.

| HDInsight sürümü | VM İŞLETİM SİSTEMİ | Sürüm tarihi | Destek sona erme tarihi | Emeklilik tarihi | Yüksek kullanılabilirlik |  Azure portal kullanılabilirliği |
| --- | --- | --- | --- | --- | --- | --- |
| HDInsight 4.0 |Ubuntu 16.0.4 LTS |Eylül 24, 2018 | | |Yes |Yes |
| HDInsight 3,6 |Ubuntu 16.0.4 LTS |4 Nisan 2017 | 31 Aralık 2020 |31 Aralık 2020 |Yes |Yes |

Spark 2,1, 2,2 ve Kafka 1,0 desteği 30 Haziran 2020 tarihinde sona erecek.

> [!NOTE]
> Bir sürüm için destek süresi dolduktan sonra, Azure portal aracılığıyla kullanılamayabilir. Küme sürümleri, sürüm kullanımdan kaldırılması tarihine kadar Windows PowerShell [New-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster) komutu ve .NET SDK içindeki **Sürüm** parametresi kullanılarak kullanılabilir olmaya devam eder.

### <a name="retired-versions"></a>Kullanımdan kaldırılan sürümler

Bu tabloda, Azure portal mevcut olmayan HDInsight sürümleri listelenir.

| HDInsight sürümü | HDP sürümü | VM İŞLETİM SİSTEMİ | Sürüm tarihi | Destek sona erme tarihi | Emeklilik tarihi | Yüksek kullanılabilirlik |  Azure portal kullanılabilirliği |
| --- | --- | --- | --- | --- | --- | --- | --- |
| HDInsight 3,5 |HDP 2,5 |Ubuntu 16.0.4 LTS |30 Eylül 2016 |5 Eylül 2017 |28 Haziran 2018 |Yes |No |
| HDInsight 3,4 |HDP 2,4 |Ubuntu 14.0.4 LTS |29 Mart 2016 |29 Aralık 2016 |9 Ocak 2018 |Yes |No |
| HDInsight 3,3 |HDP 2,3 |Windows Server 2012 R2 |2 Aralık 2015 |27 Haziran 2016 |31 Temmuz 2018 |Yes |No |
| HDInsight 3,3 |HDP 2,3 |Ubuntu 14.0.4 LTS |2 Aralık 2015 |27 Haziran 2016 |31 Temmuz 2017 |Yes |No |
| HDInsight 3,2 |HDP 2,2 |Ubuntu 12,04 LTS veya Windows Server 2012 R2 |18 Şubat 2015 |1 Mart 2016 |1 Nisan 2017 |Yes |No |
| HDInsight 3,1 |HDP 2,1 |Windows Server 2012 R2 |24 Haziran 2014 |18 Mayıs 2015 |30 Haziran 2016 |Yes |No |
| HDInsight 3,0 |HDP 2,0 |Windows Server 2012 R2 |11 Şubat 2014 |17 Eylül 2014 |30 Haziran 2015 |Yes |No |
| HDInsight 2,1 |HDP 1,3 |Windows Server 2012 R2 |28 Ekim 2013 |12 Mayıs 2014 |31 Mayıs 2015 |Yes |No |
| HDInsight 1,6 |HDP 1,1 | |28 Ekim 2013 |26 Nisan 2014 |31 Mayıs 2015 |Hayır |Hayır |

> [!NOTE]
> İki baş düğümü olan yüksek oranda kullanılabilir kümeler, HDInsight sürüm 2,1 ve üzeri için varsayılan olarak dağıtılır. HDInsight sürüm 1,6 kümeleri için kullanılamaz.

## <a name="service-level-agreement-for-hdinsight-cluster-versions"></a>HDInsight küme sürümleri için hizmet düzeyi sözleşmesi

Hizmet düzeyi sözleşmesi bir _destek penceresi_olarak tanımlanır. Destek penceresi, HDInsight sürümünün Microsoft Müşteri Hizmetleri ve desteği tarafından desteklendiği zaman dönedir. Sürüm _destek sona erme tarihini_Geçirtiyse, HDInsight kümesi Destek penceresinin dışındadır. HDInsight sürüm X için destek süresi sonu (daha yeni bir X + 1 sürümü kullanılabilir olduktan sonra), daha sonra:

- **Formül 1:** HDInsight kümesi sürüm X 'in serbest bırakıldığı tarihe 180 gün ekleyin.
- **Formül 2:** HDInsight kümesi sürümü X + 1 ' in Azure portal kullanılabilir hale getirilme tarihine 90 gün ekleyin.

_Emeklilik tarihi_ , HDInsight 'ta küme sürümünün oluşturuoluşturuoluşturulamadığı tarihtir. 31 Temmuz 2017 itibariyle, bir HDInsight kümesini kullanımdan kaldırma tarihinden sonra yeniden boyutlandıramazsınız.

## <a name="default-node-configuration-and-virtual-machine-sizes-for-clusters"></a>Kümeler için varsayılan düğüm yapılandırması ve sanal makine boyutları

Kümeniz için seçilecek sanal makine SKU 'Larının hakkında daha fazla bilgi için bkz. [Azure HDInsight küme yapılandırması ayrıntıları](hdinsight-supported-node-configuration.md).

## <a name="next-steps"></a>Sonraki adımlar

- [HDInsight üzerinde Apache Hadoop, Spark ve daha fazlası için küme kurulumu](hdinsight-hadoop-provision-linux-clusters.md)
- [Windows BILGISAYARDAN HDInsight üzerinde Apache Hadoop çalışma](hdinsight-hadoop-windows-tools.md)
- [Azure HDInsight sürümleriyle ilişkili hortonçalışmalar sürüm notları](./hortonworks-release-notes.md)
- [Kurumsal Güvenlik Paketi](./enterprise-security-package.md)
