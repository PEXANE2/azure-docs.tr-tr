---
title: Apache Hadoop, Apache Spark, Apache Kafka ve daha fazlasıyla HDInsight'ta küme oluşturma
description: Bir tarayıcıdan HDInsight, Azure klasik CLI, Azure PowerShell, REST veya SDK'dan HdInsight için Hadoop, Kafka, Spark, HBase, R Server veya Storm kümelerini ayarlayın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017,seodec18
ms.date: 02/12/2020
ms.openlocfilehash: 246ec08e9b4edb33fa49318b68cc4364534282b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80064658"
---
# <a name="set-up-clusters-in-hdinsight-with-apache-hadoop-apache-spark-apache-kafka-and-more"></a>Apache Hadoop, Apache Spark, Apache Kafka ve daha fazlasıyla HDInsight'ta küme oluşturma

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

HDInsight'ta Apache Hadoop, Apache Spark, Apache Kafka, Interactive Query, Apache HBase, ML Services veya Apache Storm'u nasıl ayarlayıp yapılandıracağınızı öğrenin. Ayrıca, kümeleri özelleştirmeyi ve bunları bir etki alanına katılarak güvenlik eklemeyi öğrenin.

Hadoop kümesi, görevlerin dağıtılmış işlenmesi için kullanılan birkaç sanal makineden (düğüm) oluşur. Azure HDInsight, tek tek düğümlerin yükleme ve yapılandırmasının uygulama ayrıntılarını işler, bu nedenle yalnızca genel yapılandırma bilgileri sağlamanız gerekir.

> [!IMPORTANT]  
> HDInsight kümesi faturalandırması küme oluşturulduğunda başlar ve küme silindiğinde sona erer. Fatura dakikalara eşit olarak dağıtıldığından, kullanılmayan kümelerinizi mutlaka silmelisiniz. Kümeyi nasıl [sildiğinizi öğrenin.](hdinsight-delete-cluster.md)

## <a name="cluster-setup-methods"></a>Küme kurulum yöntemleri

Aşağıdaki tablo, bir HDInsight kümesi ayarlamak için kullanabileceğiniz farklı yöntemleri gösterir.

| Kümeler ile oluşturulan | Web tarayıcısı | Komut satırı | REST API | SDK |
| --- |:---:|:---:|:---:|:---:|
| [Azure portalında](hdinsight-hadoop-create-linux-clusters-portal.md) |✔ |&nbsp; |&nbsp; |&nbsp; |
| [Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) |✔ |✔ |✔ |✔ |
| [Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [cURL](hdinsight-hadoop-create-linux-clusters-curl-rest.md) |&nbsp; |✔ |✔ |&nbsp; |
| [Azure Kaynak Yöneticisi şablonları](hdinsight-hadoop-create-linux-clusters-arm-templates.md) |&nbsp; |✔ |&nbsp; |&nbsp; |

Bu makale, bir HDInsight kümesi oluşturabileceğiniz [Azure portalındaki](https://portal.azure.com)kurulumda size yol açar.

## <a name="basics"></a>Temel Bilgiler

![hdinsight özel hızlı seçenekler oluşturmak](./media/hdinsight-hadoop-provision-linux-clusters/azure-portal-cluster-basics-blank-fs.png)

### <a name="project-details"></a>Proje ayrıntıları

[Azure Kaynak Yöneticisi,](../azure-resource-manager/management/overview.md) uygulamanızdaki kaynaklarla bir grup olarak çalışmanıza yardımcı olur ve bu kaynak [grubu](../azure-resource-manager/management/overview.md#resource-groups)olarak adlandırılır. Tek bir eşgüdümlü işlemle uygulamanız için tüm kaynakları dağıtabilir, güncelleyebilir, izleyebilir veya silebilirsiniz.

### <a name="cluster-details"></a>Küme ayrıntıları

#### <a name="cluster-name"></a>Küme adı

HDInsight küme adları aşağıdaki kısıtlamalara sahiptir:

* İzin verilen karakterler: a-z, 0-9, A-Z
* Maksimum uzunluk: 59
* Ayrılmış adlar: uygulamalar
* Küme adlandırma kapsamı tüm Azure için, tüm abonelikler içindir. Yani küme adı dünya çapında benzersiz olmalıdır.
* İlk altı karakter sanal ağ içinde benzersiz olmalıdır

#### <a name="region"></a>Bölge

Küme konumunu açıkça belirtmeniz gerekmez: Küme varsayılan depolama alanıyla aynı konumdadır. Desteklenen bölgelerin listesi [için, HDInsight fiyatlandırması](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409)için **Bölge** açılır listesini seçin.

#### <a name="cluster-type"></a>Küme türü

Azure HDInsight şu anda, her biri belirli işlevleri sağlamak için bir dizi bileşene sahip aşağıdaki küme türlerini sağlar.

> [!IMPORTANT]  
> HDInsight kümeleri, her biri tek bir iş yükü veya teknoloji için çeşitli türlerde kullanılabilir. Bir kümede Fırtına ve HBase gibi birden çok türü birleştiren bir küme oluşturmak için desteklenen bir yöntem yoktur. Çözümünüz birden çok HDInsight küme türüne yayılmış teknolojiler gerektiriyorsa, bir [Azure sanal ağı](https://docs.microsoft.com/azure/virtual-network) gerekli küme türlerini bağlayabilir.

| Küme türü | İşlev |
| --- | --- |
| [Hadoop](hadoop/apache-hadoop-introduction.md) |Toplu sorgu ve depolanan verilerin analizi |
| [HBase](hbase/apache-hbase-overview.md) |Büyük miktarda şemasız, NoSQL verisi için işleme |
| [Etkileşimli Sorgu](./interactive-query/apache-interactive-query-get-started.md) |Etkileşimli ve daha hızlı Hive sorguları için bellek içi önbelleğe alma |
| [Kafka](kafka/apache-kafka-introduction.md) | Gerçek zamanlı veri aktaran veri ardışık hatları ve uygulamaları oluşturmak için kullanılabilecek dağıtılmış bir akış platformu |
| [ML Services](r-server/r-server-overview.md) |Çeşitli büyük veri istatistikleri, tahmine dayalı modelleme ve makine öğrenimi yetenekleri |
| [Spark](spark/apache-spark-overview.md) |Bellek içi işleme, etkileşimli sorgular, mikro toplu akış işleme |
| [Fırtına](storm/apache-storm-overview.md) |Gerçek zamanlı olay işleme |

#### <a name="version"></a>Sürüm

Bu küme için HDInsight sürümünü seçin. Daha fazla bilgi için desteklenen [HDInsight sürümlerine](hdinsight-component-versioning.md#supported-hdinsight-versions)bakın.

### <a name="cluster-credentials"></a>Küme kimlik bilgileri

HDInsight kümeleri ile küme oluşturma sırasında iki kullanıcı hesabını yapılandırabilirsiniz:

* Küme giriş kullanıcı adı: Varsayılan kullanıcı adı *yöneticidir.* Azure portalındaki temel yapılandırmayı kullanır. Bazen buna "Küme kullanıcısı" veya "HTTP kullanıcısı" denir.
* Secure Shell (SSH) kullanıcı adı: Kümeye SSH üzerinden bağlanmak için kullanılır. Daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

HTTP kullanıcı adı aşağıdaki kısıtlamalara sahiptir:

* İzin verilen özel `_` karakterler: ve`@`
* İzin verilmeyen karakterler: #;."',\/:'!*?$(){}[]<>|&--=+%~^boşluk
* Maksimum uzunluk: 20

SSH kullanıcı adı aşağıdaki kısıtlamalara sahiptir:

* İzin verilen özel`_` karakterler: ve`@`
* İzin verilmeyen karakterler: #;."',\/:'!*?$(){}[]<>|&--=+%~^boşluk
* Maksimum uzunluk: 64
* Ayrılmış adlar: hadoop, kullanıcılar, oozie, kovan, mapred, ambari-qa, zookeeper, tez, hdfs, sqoop, ipn, hcat, ams, hbase, fırtına, yönetici, yönetici, kullanıcı, user1, test, user2, test1, user3, admin1, 1, 123, a, actuser, adm, admin2, aspnet, yedekleme, konsol, david misafir, john, sahibi, kök, sunucu, sql, destek, support_388945a0, sys, test2, test3, user4, user5, kıvılcım

## <a name="storage"></a>Depolama

![Küme depolama ayarları: HDFS uyumlu uç noktalar](./media/hdinsight-hadoop-provision-linux-clusters/azure-portal-cluster-storage.png)

Hadoop'un şirket içi kurulumu kümede depolama için Hadoop Dağıtılmış Dosya Sistemi'ni (HDFS) kullansa da, bulutta kümeye bağlı depolama uç noktalarını kullanırsınız. Bulut depolamayı kullanmak, verilerinizi korurken hesaplama için kullanılan HDInsight kümelerini güvenle silebileceğiniz anlamına gelir.

HDInsight kümeleri aşağıdaki depolama seçeneklerini kullanabilir:

* Azure Data Lake Storage Gen2
* Azure Data Lake Storage Gen1
* Azure Depolama Genel Amaçlı v2
* Azure Depolama Genel Amaçlı v1
* Azure Depolama Bloğu blob **(yalnızca ikincil depolama alanı olarak desteklenir)**

HDInsight ile depolama seçenekleri hakkında daha fazla bilgi için Azure [HDInsight kümeleriyle kullanım için depolama seçeneklerini karşılaştır'a](hdinsight-hadoop-compare-storage-options.md)bakın.

> [!WARNING]  
> HDInsight kümesinden farklı bir konumda ek bir depolama hesabı nın kullanılması desteklenmez.

Yapılandırma sırasında, varsayılan depolama bitiş noktası için bir Azure Depolama hesabının veya Veri Gölü Depolamasının blob kapsayıcısını belirtirsiniz. Varsayılan depolama uygulama ve sistem günlükleri içerir. İsteğe bağlı olarak, kümenin erişebileceği ek bağlantılı Azure Depolama hesapları ve Veri Gölü Depolama hesapları belirtebilirsiniz. HDInsight kümesi ve bağımlı depolama hesapları aynı Azure konumunda olmalıdır.

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

### <a name="metastore-settings"></a>Metastore ayarları

İsteğe bağlı Hive veya Apache Oozie metastores oluşturabilirsiniz. Ancak, tüm küme türleri meta depoları desteklemez ve Azure SQL Veri Ambarı meta store'larla uyumlu değildir.

Daha fazla bilgi için bkz: [Azure HDInsight'ta harici meta veri depolarını kullan.](./hdinsight-use-external-metadata-stores.md)

> [!IMPORTANT]  
> Özel bir metastore oluşturduğunuzda, veritabanı adında tire, tire veya boşluk kullanmayın. Bu küme oluşturma işlemi başarısız neden olabilir.

#### <a name="sql-database-for-hive"></a>Hive için SQL veritabanı

Bir HDInsight kümesini sildikten sonra Kovan tablolarınızı korumak istiyorsanız, özel bir metastore kullanın. Daha sonra metastore'u başka bir HDInsight kümesine ekleyebilirsiniz.

Bir HDInsight küme sürümü için oluşturulan bir HDInsight metastore'u farklı HDInsight küme sürümlerinde paylaşılamaz. HDInsight sürümlerinin listesi için [desteklenen HDInsight sürümlerine](hdinsight-component-versioning.md#supported-hdinsight-versions)bakın.

#### <a name="sql-database-for-oozie"></a>Oozie için SQL veritabanı

Oozie kullanırken performansı artırmak için özel bir metastore kullanın. Bir metastore, kümenizi sildikten sonra Oozie iş verilerine de erişim sağlayabilir.

#### <a name="sql-database-for-ambari"></a>Ambari için SQL veritabanı

Ambari, HDInsight kümelerini izlemek, yapılandırma değişiklikleri yapmak ve küme yönetimi bilgilerinin yanı sıra iş geçmişini depolamak için kullanılır. Özel Ambari DB özelliği, yönettiğiniz harici bir veritabanında ambari'yi yeni bir küme ve kurulum adabına dağıtmanızı sağlar. Daha fazla bilgi için [Bkz. Özel Ambari DB.](./hdinsight-custom-ambari-db.md)

> [!IMPORTANT]  
> Özel bir Oozie metastore'u yeniden kullanamazsınız. Özel bir Oozie metastore kullanmak için, HDInsight kümesini oluştururken boş bir Azure SQL Veritabanı sağlamanız gerekir.

## <a name="security--networking"></a>Güvenlik + ağ

![hdinsight oluşturma seçenekleri kurumsal güvenlik paketini seçin](./media/hdinsight-hadoop-provision-linux-clusters/azure-portal-cluster-security-networking.png)

### <a name="enterprise-security-package"></a>Kurumsal güvenlik paketi

Hadoop, Spark, HBase, Kafka ve İnteraktif Sorgu küme türleri için **Kurumsal Güvenlik Paketini**etkinleştirmeyi seçebilirsiniz. Bu paket, Apache Ranger'ı kullanarak ve Azure Active Directory ile tümleştirerek daha güvenli bir küme kurulumu seçeneği sunar. Daha fazla bilgi için Azure [HDInsight'ta kurumsal güvenliğe genel bakış bölümüne](./domain-joined/hdinsight-security-overview.md)bakın.

Kurumsal güvenlik paketi, HDInsight'ı Active Directory ve Apache Ranger ile entegre etmenizi sağlar. Kurumsal güvenlik paketi kullanılarak birden çok kullanıcı oluşturulabilir.

Etki alanı yla birleştirilmiş HDInsight kümesi oluşturma hakkında daha fazla bilgi için [bkz.](./domain-joined/apache-domain-joined-configure.md)

### <a name="tls"></a>TLS

Daha fazla bilgi için Taşıma [Katmanı Güvenliği'ne](./hdinsight-plan-virtual-network-deployment.md#transport-layer-security) bakın

### <a name="virtual-network"></a>Sanal ağ

Çözümünüz birden çok HDInsight küme türüne yayılmış teknolojiler gerektiriyorsa, bir [Azure sanal ağı](https://docs.microsoft.com/azure/virtual-network) gerekli küme türlerini bağlayabilir. Bu yapılandırma kümelerin ve onlara dağıttığınız tüm kodların birbirleriyle doğrudan iletişim kurmasını sağlar.

HDInsight içeren bir Azure sanal ağı kullanma hakkında daha fazla bilgi [için HDInsight için sanal ağ planı'na](hdinsight-plan-virtual-network-deployment.md)bakın.

Bir Azure sanal ağında iki küme türü kullanma örneği [için](hdinsight-apache-kafka-spark-structured-streaming.md)bkz. Sanal ağ için belirli yapılandırma gereksinimleri de dahil olmak üzere SANAL ağda HDInsight kullanma hakkında daha fazla bilgi [için HDInsight için sanal ağ planı'na](hdinsight-plan-virtual-network-deployment.md)bakın.

### <a name="disk-encryption-setting"></a>Disk şifreleme ayarı

Daha fazla bilgi için [Müşteri tarafından yönetilen anahtar disk şifrelemesi'ne](./disk-encryption.md)bakın.

### <a name="kafka-rest-proxy"></a>Kafka REST ara sunucusu

Bu ayar yalnızca Kafka kümesi türü için kullanılabilir. Daha fazla bilgi için REST [proxy'si kullanma'ya](./kafka/rest-proxy.md)bakın.

### <a name="identity"></a>Kimlik

Daha fazla bilgi için Azure [HDInsight'ta Yönetilen kimlikler'e](./hdinsight-managed-identities.md)bakın.

## <a name="configuration--pricing"></a>Yapılandırma + fiyatlandırma

![HDInsight düğüm boyutunu seçin](./media/hdinsight-hadoop-provision-linux-clusters/azure-portal-cluster-configuration.png)

Küme var olduğu sürece düğüm kullanımı için faturalandırılırsınız. Bir küme oluşturulduğunda faturalandırma başlar ve küme silindiğinde durur. Kümeler ayrılamaz veya beklemeye alamaz.

### <a name="node-configuration"></a>Düğüm yapılandırması

Her küme türünün kendi düğüm sayısı, düğümler için terminoloji ve varsayılan VM boyutu vardır. Aşağıdaki tabloda, her düğüm türü için düğüm sayısı parantez içindedir.

| Tür | Düğümler | Diyagram |
| --- | --- | --- |
| Hadoop |Baş düğümü (2), İşçi düğümü (1+) |![HDInsight Hadoop küme düğümleri](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-hadoop-cluster-type-nodes.png) |
| HBase |Baş sunucu (2), bölge sunucusu (1+), ana/ZooKeeper düğümü (3) |![HDInsight HBase küme tipi kurulumu](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-hbase-cluster-type-setup.png) |
| Storm |Nimbus düğümü (2), süpervizör sunucusu (1+), ZooKeeper düğümü (3) |![HDInsight fırtına kümesi türü kurulumu](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-storm-cluster-type-setup.png) |
| Spark |Baş düğümü (2), İşçi düğümü (1+), ZooKeeper düğümü (3) (A1 ZooKeeper VM boyutu için ücretsiz) |![HDInsight kıvılcım küme türü kurulumu](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-spark-cluster-type-setup.png) |

Daha fazla bilgi için " HDInsight'taki Hadoop bileşenleri ve sürümleri nedir?" dizindeki [kümeler için Varsayılan düğüm yapılandırması](hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters) ve sanal makine boyutlarına bakın.

HDInsight kümelerinin maliyeti düğüm sayısına ve düğümler için sanal makine boyutlarına göre belirlenir.

Farklı küme türlerinin farklı düğüm türleri, düğüm sayıları ve düğüm boyutları vardır:
* Hadoop küme türü varsayılan:
    * İki *baş düğümleri*  
    * Dört *İşçi düğümü*
* Fırtına kümesi türü varsayılan:
    * İki *Nimbus düğümü*
    * Üç *ZooKeeper düğümleri*
    * Dört *süpervizör düğümü*

HDInsight'ı deniyorsanız, tek bir İşçi düğümkullanmanızı öneririz. HDInsight fiyatlandırması hakkında daha fazla bilgi için [HDInsight](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409)fiyatlandırması'na bakın.

> [!NOTE]  
> Küme boyutu sınırı Azure abonelikleri arasında değişir. Sınırı artırmak için [Azure faturalandırma desteğine](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request) başvurun.

Kümeyi yapılandırmak için Azure portalını kullandığınızda, düğüm boyutu **Yapılandırma + fiyatlandırma** sekmesinden kullanılabilir. Portalda, farklı düğüm boyutlarıyla ilişkili maliyeti de görebilirsiniz.

### <a name="virtual-machine-sizes"></a>Sanal makine boyutları

Kümeleri dağıttığınızda, dağıtmayı planladığınız çözüme göre hesaplama kaynaklarını seçin. Aşağıdaki VM'ler HDInsight kümeleri için kullanılır:

* A ve D1-4 serisi [VM'ler: Genel amaçlı Linux VM boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general)
* D11-14 serisi VM: [Bellek için optimize edilmiş Linux VM boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory)

Farklı SDK'ları kullanırken veya Azure PowerShell kullanırken bir küme oluştururken VM boyutunu belirtmek için hangi değeri kullanmanız gerektiğini öğrenmek [için HDInsight kümeleri için kullanılacak VM boyutlarını](../cloud-services/cloud-services-sizes-specs.md#size-tables)görün. Bu bağlantılı makaleden, tabloların **Boyut** sütunundaki değeri kullanın.

> [!IMPORTANT]  
> Bir kümede 32'den fazla İşçi düğümüne ihtiyacınız varsa, en az 8 çekirdek ve 14 GB RAM içeren bir baş düğümü boyutu seçmeniz gerekir.

Daha fazla bilgi [için sanal makineler için Boyutlar'a](../virtual-machines/windows/sizes.md)bakın. Çeşitli boyutların fiyatlandırması hakkında daha fazla bilgi için [HDInsight fiyatlandırması](https://azure.microsoft.com/pricing/details/hdinsight)bölümüne bakın.

### <a name="add-application"></a>Uygulama ekleme

HDInsight uygulaması kullanıcıların Linux tabanlı HDInsight kümesine yükleyebileceği bir uygulamadır. Microsoft, üçüncü taraflar tarafından sağlanan veya kendinizi geliştirdiğiniz uygulamaları kullanabilirsiniz. Daha fazla bilgi için Azure [HDInsight'ta üçüncü taraf Apache Hadoop uygulamalarını yükleyin'](hdinsight-apps-install-applications.md)e bakın.

HDInsight uygulamalarının çoğu boş bir kenar düğümüne yüklenir.  Boş kenar düğümü, kafa düğümünde olduğu gibi aynı istemci araçlarıyüklü ve yapılandırılan bir Linux sanal makinesidir. Kümeye erişmek, istemci uygulamalarınızı test etmek ve istemci uygulamalarınızı barındırmak için kenar düğümlerini kullanabilirsiniz. Daha fazla bilgi için bkz: [HDInsight'ta boş kenar düğümlerini kullan.](hdinsight-apps-use-edge-node.md)

### <a name="script-actions"></a>Komut dosyası eylemleri

Oluşturma sırasında komut dosyalarını kullanarak ek bileşenler yükleyebilir veya küme yapılandırması özelleştirebilirsiniz. Bu tür komut dosyaları, Azure portalı, HDInsight Windows PowerShell cmdlets veya HDInsight .NET SDK'dan kullanılabilecek bir yapılandırma seçeneği olan **Script Action**aracılığıyla çağrılır. Daha fazla bilgi için, [Komut Dosyası Eylemi'ni kullanarak HDInsight kümesini Özelleştir'e](hdinsight-hadoop-customize-cluster-linux.md)bakın.

Apache Mahout ve Basamaklı gibi bazı yerel Java bileşenleri kümede Java Archive (JAR) dosyaları olarak çalıştırılabilir. Bu JAR dosyaları Azure Depolama'ya dağıtılabilir ve Hadoop iş gönderme mekanizmalarıyla HDInsight kümelerine gönderilebilir. Daha fazla bilgi için bkz: [Apache Hadoop işlerini programlı bir şekilde gönder.](hadoop/submit-apache-hadoop-jobs-programmatically.md)

> [!NOTE]  
> JAR dosyalarını HDInsight kümelerine dağıtma veya HDInsight kümelerinde JAR dosyalarını arama sorunlarınız varsa, [Microsoft Destek'e](https://azure.microsoft.com/support/options/)başvurun.
>
> Basamaklı HDInsight tarafından desteklenmez ve Microsoft Desteği için uygun değildir. Desteklenen bileşenlerin listeleri için [HDInsight tarafından sağlanan küme sürümlerinde yeniliklere](hdinsight-component-versioning.md)bakın.

Bazen, oluşturma işlemi sırasında aşağıdaki yapılandırma dosyalarını yapılandırmak istersiniz:

* clusterIdentity.xml
* core-site.xml
* ağ geçidi.xml
* hbase-env.xml
* hbase-site.xml
* hdfs-site.xml
* kovan-env.xml
* kovan-site.xml
* mapred-site
* oozie-site.xml
* oozie-env.xml
* fırtına-site.xml
* tez-site.xml
* webhcat-site.xml
* iplik-site.xml

Daha fazla bilgi [için, Bootstrap kullanarak HDInsight kümelerini özelleştir'e](hdinsight-hadoop-customize-cluster-bootstrap.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight ile küme oluşturma hatalarını giderme](./hadoop/hdinsight-troubleshoot-cluster-creation-fails.md)
* [HDInsight, Apache Hadoop ekosistemi ve Hadoop kümeleri nedir?](hadoop/apache-hadoop-introduction.md)
* [HDInsight'ta Apache Hadoop'u kullanmaya başlayın](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Windows PC'den HDInsight'ta Apache Hadoop'ta çalışın](hdinsight-hadoop-windows-tools.md)
