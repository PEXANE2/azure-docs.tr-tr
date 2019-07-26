---
title: Apache Hadoop, Apache Spark, Apache Kafka, Apache HBase veya R Server için küme kurulumu-Azure HDInsight
description: HDInsight için Hadoop, Kafka, Spark, HBase, R Server veya fırtınası kümelerini tarayıcıdan, klasik Azure CLı, Azure PowerShell, REST veya SDK 'dan ayarlayın.
keywords: Hadoop kümesi kurulumu, Kafka küme kurulumu, Spark kümesi kurulumu, Hadoop 'da küme nedir?
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017,seodec18
ms.topic: conceptual
ms.date: 07/15/2019
ms.openlocfilehash: 4159eed04ff1a4e81ea36a4d7f06f342a63fe367
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68442020"
---
# <a name="set-up-clusters-in-hdinsight-with-apache-hadoop-apache-spark-apache-kafka-and-more"></a>HDInsight 'ta Apache Hadoop, Apache Spark, Apache Kafka ve daha fazlasını içeren kümeler ayarlayın

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

HDInsight 'ta Apache Hadoop, Apache Spark, Apache Kafka, etkileşimli sorgu, Apache HBase, ML Hizmetleri veya Apache Storm ile küme ayarlamayı ve yapılandırmayı öğrenin. Ayrıca, kümeleri özelleştirmeyi ve bir etki alanına katılarak güvenlik eklemeyi öğrenin.

Hadoop kümesi, görevlerin dağıtılmış işlemesi için kullanılan birkaç sanal makineden (düğümden) oluşur. Azure HDInsight, tek tek düğümlerin yüklenmesi ve yapılandırılmasına ilişkin uygulama ayrıntılarını işler, bu nedenle yalnızca genel yapılandırma bilgilerini sağlamanız gerekir.

> [!IMPORTANT]  
> HDInsight kümesi faturalandırması küme oluşturulduğunda başlar ve küme silindiğinde sona erer. Fatura dakikalara eşit olarak dağıtıldığından, kullanılmayan kümelerinizi mutlaka silmelisiniz. [Bir kümeyi silmeyi öğrenin.](hdinsight-delete-cluster.md)

## <a name="cluster-setup-methods"></a>Küme kurulum yöntemleri
Aşağıdaki tabloda, bir HDInsight kümesi kurmak için kullanabileceğiniz farklı yöntemler gösterilmektedir.

| İle oluşturulan kümeler | Web tarayıcısı | Komut satırı | REST API | SDK | 
| --- |:---:|:---:|:---:|:---:|
| [Azure portal](hdinsight-hadoop-create-linux-clusters-portal.md) |✔ |&nbsp; |&nbsp; |&nbsp; |
| [Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) |✔ |✔ |✔ |✔ |
| [Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [cURL](hdinsight-hadoop-create-linux-clusters-curl-rest.md) |&nbsp; |✔ |✔ |&nbsp; |
| [.NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) |&nbsp; |&nbsp; |&nbsp; |✔ |
| [Azure Resource Manager şablonları](hdinsight-hadoop-create-linux-clusters-arm-templates.md) |&nbsp; |✔ |&nbsp; |&nbsp; |

## <a name="quick-create-basic-cluster-setup"></a>Hızlı oluştur: Temel küme kurulumu
Bu makalede, *hızlı oluşturma* veya *özel*' i kullanarak bir HDInsight kümesi oluşturabileceğiniz [Azure Portal](https://portal.azure.com)kurulum işlemi adım adım açıklanmaktadır. 

![HDInsight oluşturma seçenekleri özel hızlı oluşturma](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-creation-options.png)

Temel küme kurulumu yapmak için ekrandaki yönergeleri izleyin. Ayrıntılar aşağıda verilmiştir:

* [Kaynak grubu adı](#resource-group-name)
* [Küme türleri ve yapılandırma](#cluster-types) 
* [Küme adı](#cluster-name)
* [Küme oturum açma ve SSH Kullanıcı adı](#cluster-login-and-ssh-username)
* [Location](#location)

## <a name="resource-group-name"></a>Kaynak grubu adı

[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) , Azure Kaynak grubu olarak adlandırılan, uygulamanızdaki kaynaklarla bir grup olarak çalışmanıza yardımcı olur. Uygulamanıza yönelik tüm kaynakları tek bir eşgüdümlü işlemle dağıtabilir, güncelleştirebilir, izleyebilir veya silebilirsiniz.

## <a name="cluster-types"></a>Küme türleri ve yapılandırma
Azure HDInsight Şu anda, her biri belirli işlevleri sağlamak üzere bir bileşen kümesi olan aşağıdaki küme türlerini sağlamaktadır.

> [!IMPORTANT]  
> HDInsight kümeleri, her biri tek bir iş yükü veya teknoloji için çeşitli türlerde kullanılabilir. Bir kümede fırtınası ve HBase gibi birden çok türü birleştiren bir küme oluşturmak için desteklenen bir yöntem yoktur. Çözümünüz birden çok HDInsight kümesi türüne yayılan teknolojiler gerektiriyorsa, bir [Azure sanal ağı](https://docs.microsoft.com/azure/virtual-network) gerekli küme türlerini bağlanabilir. 

| Küme türü | İşlevi |
| --- | --- |
| [Hadoop](hadoop/apache-hadoop-introduction.md) |Depolanan verilerin toplu sorgu ve Analizi |
| [HBase](hbase/apache-hbase-overview.md) |Büyük miktarlarda çok sayıda şemaya daha, NoSQL verisi işleme |
| [Etkileşimli sorgu](./interactive-query/apache-interactive-query-get-started.md) |Etkileşimli ve daha hızlı Hive sorguları için bellek içi önbelleğe alma |
| [Kafka](kafka/apache-kafka-introduction.md) | Gerçek zamanlı akış veri işlem hatları ve uygulamaları oluşturmak için kullanılabilen bir dağıtılmış akış platformu |
| [ML Hizmetleri](r-server/r-server-overview.md) |Çeşitli büyük veri istatistikleri, tahmine dayalı modelleme ve makine öğrenimi özellikleri |
| [Spark](spark/apache-spark-overview.md) |Bellek içi işleme, etkileşimli sorgular, mikro-Batch akış işleme |
| [Ina](storm/apache-storm-overview.md) |Gerçek zamanlı olay işleme |


### <a name="hdinsight-version"></a>HDInsight sürümü
Bu küme için HDInsight sürümünü seçin. Daha fazla bilgi için bkz. [desteklenen HDInsight sürümleri](hdinsight-component-versioning.md#supported-hdinsight-versions).

## <a name="cluster-name"></a>Küme adı

HDInsight kümesi adları aşağıdaki kısıtlamalara sahiptir:
- İzin verilen karakterler: a-z, 0-9, A-Z 
- En fazla uzunluk: 59
- Ayrılmış adlar: uygulamalar
- Benzersiz olmalıdır
- İlk 6 karakter bir sanal ağ içinde benzersiz olmalıdır

## <a name="cluster-login-and-ssh-username"></a>Küme oturum açma ve SSH Kullanıcı adı
HDInsight kümeleri ile, küme oluşturma sırasında iki kullanıcı hesabını yapılandırabilirsiniz:

* HTTP kullanıcısı: Varsayılan kullanıcı adı *admin* şeklindedir. Azure portal temel yapılandırmayı kullanır. Bazen "küme kullanıcısı" olarak adlandırılır.
* SSH kullanıcısı: Kümeye SSH aracılığıyla bağlanmak için kullanılır. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

HTTP Kullanıcı adı aşağıdaki kısıtlamalara sahiptir:
- İzin verilen özel karakterler: _ ve @ 
- Karakterlere izin verilmiyor: #;. "',\/: '! *? $ (){}[] < > | &--= +% ~ ^ boşluk
- En fazla uzunluk: 20

SSH Kullanıcı adı aşağıdaki kısıtlamalara sahiptir:
- İzin verilen özel karakterler: _ ve @ 
- Karakterlere izin verilmiyor: #;. "',\/: '! *? $ (){}[] < > | &--= +% ~ ^ boşluk
- En fazla uzunluk: 64
- Ayrılmış adlar: Hadoop, kullanıcılar, Oozie, Hive, mapred, ambarı-qa, Zookeeper, tez, Ise, Sqoop, Yarn, hcat, AMS, HBase, fırtınası, yönetici, yönetici, Kullanıcı, Kullanıcı1, test, kullanıcı2, test1, User3, admin1, 1, 123, a, ACTUser, adm, Admin2, ASPNET, Backup, Console, David, Konuk, John, Owner, root, Server, SQL, support, SUPPORT_388945a0, sys, test2, test3, User4, user5, Spark

Kurumsal güvenlik paketi, HDInsight 'ı Active Directory ve Apache Ranger ile tümleştirmenize olanak tanır. Kurumsal güvenlik paketi kullanılarak birden çok Kullanıcı oluşturulabilir.

## <a name="location"></a>Kümeler ve depolama için konum (bölgeler)

Küme konumunu açıkça belirtmeniz gerekmez: Küme, varsayılan depolama alanı ile aynı konumdadır. Desteklenen bölgelerin listesi için [HDInsight fiyatlandırması](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409)üzerindeki **bölge** açılan listesine tıklayın.

## <a name="storage-endpoints-for-clusters"></a>Kümeler için depolama uç noktaları

Hadoop 'un Şirket içi yüklemesi kümedeki depolama için Hadoop Dağıtılmış Dosya Sistemi (bir) kullanıyor olsa da bulutta, kümeye bağlı depolama uç noktaları kullanılır. HDInsight kümeleri [Azure Storage 'da](hdinsight-hadoop-use-blob-storage.md) [Azure Data Lake Storage](hdinsight-hadoop-use-data-lake-store.md) veya blob kullanır. Azure Storage veya Data Lake Storage kullanımı, hesaplama için kullanılan HDInsight kümelerini verilerinizi korurken güvenli bir şekilde silebileceğiniz anlamına gelir. 

> [!WARNING]  
> HDInsight kümesinden farklı bir konumda ek depolama hesabı kullanılması desteklenmez.

Yapılandırma sırasında, varsayılan depolama uç noktası için bir Azure depolama hesabının veya Data Lake Storage blob kapsayıcısını belirtirsiniz. Varsayılan depolama, uygulama ve sistem günlükleri içerir. İsteğe bağlı olarak, ek bağlı Azure depolama hesapları ve kümenin erişebileceği Data Lake Storage hesapları belirtebilirsiniz. HDInsight kümesi ve bağımlı depolama hesapları aynı Azure konumunda olmalıdır.

![Küme depolama ayarları: ,, Uyumlu depolama uç noktaları](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-cluster-creation-storage.png)

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]


### <a name="optional-metastores"></a>İsteğe bağlı meta depolar
İsteğe bağlı Hive veya Apache Oozie metastores oluşturabilirsiniz. Ancak, tüm küme türleri metastores 'yi desteklemez ve Azure SQL veri ambarı, metastores ile uyumlu değildir. 

Daha fazla bilgi için bkz. [Azure HDInsight 'ta dış meta veri depoları kullanma](./hdinsight-use-external-metadata-stores.md).

> [!IMPORTANT]  
> Özel bir meta veri deposu oluşturduğunuzda, veritabanı adında tireler, tireler veya boşluk kullanmayın. Bu, küme oluşturma işleminin başarısız olmasına neden olabilir.

### <a name="use-hiveoozie-metastore"></a>Hive meta veri deposu

Bir HDInsight kümesini sildikten sonra Hive tablolarınızı sürdürmek isterseniz, özel bir meta veri deposu kullanın. Daha sonra, meta veri deposunu başka bir HDInsight kümesine ekleyebilirsiniz.

Bir HDInsight küme sürümü için oluşturulan An HDInsight meta veri, farklı HDInsight küme sürümleri arasında paylaşılamaz. HDInsight sürümlerinin listesi için bkz. [desteklenen HDInsight sürümleri](hdinsight-component-versioning.md#supported-hdinsight-versions).

### <a name="oozie-metastore"></a>Oozie meta veri deposu

Oozie kullanırken performansı artırmak için özel bir meta veri deposu kullanın. Bir meta veri deposu, kümenizi sildikten sonra Oozie iş verilerine erişim de sağlayabilir. 

> [!IMPORTANT]  
> Özel bir Oozie meta veri deposunu yeniden kullanamazsınız. Özel bir Oozie meta veri deposu kullanmak için, HDInsight kümesini oluştururken boş bir Azure SQL veritabanı sağlamanız gerekir.


## <a name="custom-cluster-setup"></a>Özel küme kurulumu
Özel küme kurulumu hızlı oluşturma ayarlarında oluşturulur ve aşağıdaki seçenekleri ekler:
- [Kurumsal güvenlik paketi](#enterprise-security-package)
- [HDInsight uygulamaları](#install-hdinsight-applications-on-clusters)
- [Küme boyutu](#configure-cluster-size)
- [Betik eylemleri](#advanced-settings-script-actions)
- [Sanal ağ](#advanced-settings-extend-clusters-with-a-virtual-network)
 
## <a name="enterprise-security-package"></a>Kurumsal güvenlik paketi

Hadoop, Spark, HBase, Kafka ve etkileşimli sorgu kümesi türleri için **Kurumsal güvenlik paketi**etkinleştirmeyi seçebilirsiniz. Bu paket, Apache Ranger kullanarak ve Azure Active Directory tümleştirerek daha güvenli bir küme kurulumuna sahip olmak için seçenek sağlar. Daha fazla bilgi için bkz. [Azure HDInsight 'ta Kurumsal güvenliğe genel bakış](./domain-joined/hdinsight-security-overview.md).

![HDInsight oluşturma seçenekleri kurumsal güvenlik paketini seçin](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-creation-enterprise-security-package.png)

Etki alanına katılmış HDInsight kümesi oluşturma hakkında daha fazla bilgi için bkz. [etki alanına katılmış HDInsight Sandbox ortamı oluşturma](./domain-joined/apache-domain-joined-configure.md). 

## <a name="install-hdinsight-applications-on-clusters"></a>Kümelere HDInsight uygulamaları yükleme

HDInsight uygulaması kullanıcıların Linux tabanlı HDInsight kümesine yükleyebileceği bir uygulamadır. Microsoft, üçüncü taraflar tarafından sunulan veya kendi kendinize Geliştirdiğiniz uygulamaları kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure HDInsight 'ta üçüncü taraf Apache Hadoop uygulamaları yüklemeyi](hdinsight-apps-install-applications.md).

HDInsight uygulamalarının çoğu boş bir kenar düğümüne yüklenir.  Boş bir Edge düğümü, aynı istemci araçları yüklü ve baş düğümde olarak yapılandırılmış bir Linux sanal makinedir. Küme erişimi, istemci uygulamalarınızı test etmek ve istemci uygulamalarınızı barındırmak için kenar düğümünü kullanabilirsiniz. Daha fazla bilgi için bkz. [HDInsight 'ta boş kenar düğümlerini kullanma](hdinsight-apps-use-edge-node.md).

## <a name="configure-cluster-size"></a>Küme boyutunu Yapılandır

Küme mevcut olduğu sürece düğüm kullanımı için faturalandırılırsınız. Faturalandırma, küme oluşturulduğunda başlar ve küme silindiğinde duraklar. Kümeler serbest olarak ayrılamaz veya beklemeye alınamaz.

### <a name="number-of-nodes-for-each-cluster-type"></a>Her küme türü için düğüm sayısı
Her küme türünün kendi düğüm sayısı, düğüm terminolojisi ve varsayılan VM boyutu vardır. Aşağıdaki tabloda, her düğüm türü için düğümlerin sayısı parantez içinde bulunur.

| Type | Düğümler | Diyagram |
| --- | --- | --- |
| Hadoop |Baş düğüm (2), çalışan düğümü (1 +) |![HDInsight Hadoop küme düğümleri](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-hadoop-cluster-type-nodes.png) |
| HBase |Baş sunucu (2), bölge sunucusu (1 +), ana/ZooKeeper düğüm (3) |![HDInsight HBase küme düğümleri](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-hbase-cluster-type-setup.png) |
| Storm |Nimbus node (2), gözetmen sunucusu (1 +), ZooKeeper node (3) |![HDInsight fırtınası küme düğümleri](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-storm-cluster-type-setup.png) |
| Spark |Baş düğüm (2), çalışan düğümü (1 +), ZooKeeper node (3) (a1 ZooKeeper VM boyutu için ücretsiz) |![HDInsight Spark küme düğümleri](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-spark-cluster-type-setup.png) |

Daha fazla bilgi için bkz. "HDInsight 'ta Hadoop bileşenleri ve sürümleri nelerdir?" içindeki [kümeler Için varsayılan düğüm yapılandırması ve sanal makine boyutları](hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters) .

HDInsight kümelerinin maliyeti düğüm sayısına ve düğümlerin sanal makine boyutlarına göre belirlenir. 

Farklı küme türlerinde farklı düğüm türleri, düğüm sayıları ve düğüm boyutları vardır:
* Hadoop kümesi türü varsayılanı: 
    * İki *baş düğüm*  
    * Dört *çalışan düğümü*
* Fırtınası kümesi türü varsayılanı: 
    * İki *Nimbus düğümü*
    * Üç *ZooKeeper düğümü*
    * Dört *Gözetmen düğümü* 

HDInsight 'ı yeni deneiyorsanız, bir çalışan düğümü kullanmanızı öneririz. HDInsight fiyatlandırması hakkında daha fazla bilgi için bkz. [HDInsight fiyatlandırması](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

> [!NOTE]  
> Küme boyutu sınırı, Azure abonelikleri arasında farklılık gösterir. Limiti artırmak için [Azure Faturalandırma desteği](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) 'ne başvurun.

Kümeyi yapılandırmak için Azure portal kullandığınızda, düğüm boyutu **düğüm fiyatlandırma katmanları** dikey penceresinde kullanılabilir. Portalda, farklı düğüm boyutlarıyla ilişkili maliyeti de görebilirsiniz. 

![HDInsight VM düğüm boyutları](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-node-sizes.png)

### <a name="virtual-machine-sizes"></a>Sanal makine boyutları 
Kümeleri dağıtırken, dağıtmayı planladığınız çözüme göre işlem kaynakları ' nı seçin. HDInsight kümeleri için aşağıdaki VM 'Ler kullanılır:
* A ve D1-4 Serisi VM 'Ler: [Genel amaçlı Linux VM boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general)
* D11-14 serisi VM: [Bellek için iyileştirilmiş Linux VM boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory)

Farklı SDK 'Ları kullanarak bir küme oluştururken veya Azure PowerShell kullanırken VM boyutunu belirtmek için hangi değeri kullanmanız gerektiğini öğrenmek için bkz. [HDInsight kümeleri için kullanılacak VM boyutları](../cloud-services/cloud-services-sizes-specs.md#size-tables). Bu bağlantılı makalede, tabloların **Boyut** sütunundaki değeri kullanın.

> [!IMPORTANT]  
> Bir kümede 32 ' den fazla çalışan düğümüne ihtiyacınız varsa, en az 8 çekirdek ve 14 GB RAM içeren bir baş düğüm boyutu seçmeniz gerekir.

Daha fazla bilgi için [sanal makine boyutları](../virtual-machines/windows/sizes.md). Çeşitli boyutlardaki fiyatlar hakkında daha fazla bilgi için bkz. [HDInsight fiyatlandırması](https://azure.microsoft.com/pricing/details/hdinsight).   

## <a name="advanced-settings-script-actions"></a>Gelişmiş ayarlar: Betik eylemleri

Oluşturma sırasında betikleri kullanarak ek bileşenler yükleyebilir veya küme yapılandırmasını özelleştirebilirsiniz. Bu tür betikler, Azure portal, HDInsight Windows PowerShell cmdlet 'leri veya HDInsight .NET SDK 'dan kullanılabilen bir yapılandırma seçeneği olan **betik eylemi**aracılığıyla çağrılır. Daha fazla bilgi için bkz. [betik eylemini kullanarak HDInsight kümesini özelleştirme](hdinsight-hadoop-customize-cluster-linux.md).

Apache Mahout ve basamaklı gibi bazı yerel Java bileşenleri, küme üzerinde Java Arşivi (JAR) dosyaları olarak çalıştırılabilir. Bu JAR dosyaları Azure depolama 'ya dağıtılabilir ve Hadoop iş gönderim mekanizmalarıyla HDInsight kümelerine gönderilebilir. Daha fazla bilgi için bkz. [Apache Hadoop işleri programlı olarak gönderme](hadoop/submit-apache-hadoop-jobs-programmatically.md).

> [!NOTE]  
> JAR dosyalarını HDInsight kümelerine dağıtmaya veya HDInsight kümelerinde JAR dosyalarını çağırmaya sorunlarınız varsa [Microsoft desteği](https://azure.microsoft.com/support/options/)başvurun.
>
> HDInsight, HDInsight tarafından desteklenmez ve Microsoft Desteği için uygun değildir. Desteklenen bileşenlerin listesi için bkz. [HDInsight tarafından sunulan küme sürümlerindeki](hdinsight-component-versioning.md)yenilikler.

Bazen, oluşturma işlemi sırasında aşağıdaki yapılandırma dosyalarını yapılandırmak isteyebilirsiniz:

* clusterIdentity.xml
* Core-site. xml
* Gateway. xml
* HBase-env. xml
* HBase-site. xml
* hdfs-site.xml
* Hive-env. xml
* Hive-site. xml
* mapred-site
* oozie-site.xml
* Oozie-env. xml
* storm-site.xml
* tez-site.xml
* webhcat-site. xml
* Yarn-site. xml

Daha fazla bilgi için bkz. [önyükleme kullanarak HDInsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-bootstrap.md).

## <a name="advanced-settings-extend-clusters-with-a-virtual-network"></a>Gelişmiş ayarlar: Kümeleri bir sanal ağ ile genişletme
Çözümünüz birden çok HDInsight kümesi türüne yayılan teknolojiler gerektiriyorsa, bir [Azure sanal ağı](https://docs.microsoft.com/azure/virtual-network) gerekli küme türlerini bağlanabilir. Bu yapılandırma, kümelerin ve bunlara dağıttığınız tüm kodların birbirleriyle doğrudan iletişim kurmasına olanak tanır.

HDInsight ile Azure sanal ağını kullanma hakkında daha fazla bilgi için bkz. [HDInsight için bir sanal ağ planlayın](hdinsight-plan-virtual-network-deployment.md).

Bir Azure sanal ağı içinde iki küme türü kullanmanın bir örneği için, bkz. [Apache Kafka Apache Spark yapısal akış kullanma](hdinsight-apache-kafka-spark-structured-streaming.md). Sanal ağ için belirli yapılandırma gereksinimleri dahil olmak üzere bir sanal ağla HDInsight kullanma hakkında daha fazla bilgi için bkz. [HDInsight için bir sanal ağ planlayın](hdinsight-plan-virtual-network-deployment.md).


## <a name="next-steps"></a>Sonraki adımlar

- [HDInsight nedir, Apache Hadoop ekosistemi ve Hadoop kümeleri nedir?](hadoop/apache-hadoop-introduction.md)
- [HDInsight 'ta Apache Hadoop kullanmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md)
- [Windows BILGISAYARDAN HDInsight üzerinde Apache Hadoop çalışma](hdinsight-hadoop-windows-tools.md)
