---
title: "Altyapı: Şirket içi Apache Hadoop'dan Azure HDInsight'a"
description: Şirket içi Hadoop kümelerini Azure HDInsight'a geçirmek için altyapının en iyi uygulamalarını öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/06/2019
ms.openlocfilehash: d7ee8ae121e3cbb9760a87c95d12109a9b05e0c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74951522"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---infrastructure-best-practices"></a>Şirket içi Apache Hadoop kümelerini Azure HDInsight'a geçirin - altyapı en iyi uygulamaları

Bu makalede, Azure HDInsight kümelerinin altyapısını yönetmek için öneriler verebisi vardır. Şirket içi Apache Hadoop sistemlerini Azure HDInsight'a geçirmede yardımcı olmak için en iyi uygulamaları sağlayan bir serinin parçasıdır.

## <a name="plan-for-hdinsight-cluster-capacity"></a>HDInsight küme kapasitesi için plan yapma

HDInsight küme kapasite planlaması için yapılacılar:

**Bölge**  
Azure bölgesi kümenin fiziksel olarak nerede sağlandığıbelirlenir belirler. Okuma ve yazma ların gecikmesini en aza indirmek için küme, verilerle aynı Bölgede olmalıdır.

**Depolama konumu ve boyutu**  
Varsayılan depolama kümeyle aynı Bölgede olmalıdır.48 düğümlü bir küme için 4 ila 8 depolama hesabı olması önerilir. Zaten yeterli toplam depolama alanı olsa da, her depolama hesabı bilgi işlem düğümleri için ek ağ bant genişliği sağlar. Birden çok depolama hesabı olduğunda, önek olmadan her depolama hesabı için rasgele bir ad kullanın. Rasgele adlandırmanın amacı, depolama darboğazları (azaltma) veya tüm hesaplarda ortak mod hataları olasılığını azaltmaktır. Daha iyi performans için depolama hesabı başına yalnızca bir kapsayıcı kullanın.

**VM boyutu ve türü (şimdi G-serisi destekler)**  
Her küme türünde bir düğüm türü kümesi vardır ve her düğüm türü, VM boyutu ve türü için belirli seçeneklere sahiptir. VM boyutu ve türü CPU işleme gücü, RAM boyutu ve ağ gecikmesi ile belirlenir. Her düğüm türü için en uygun VM boyutunu ve türünü belirlemek için benzetimli iş yükü kullanılabilir.

**İşçi düğümlerinin sayısı**  
İlk işçi düğüm sayısı benzetilen iş yükleri kullanılarak belirlenebilir. Küme daha sonra en yüksek yük taleplerini karşılamak için daha fazla altdüğüm ekleyerek ölçeklendirilebilir. Ek alt düğümler gerekli olmadığında küme daha sonra geri ölçeklenebilir.

Daha fazla bilgi için [HDInsight kümeleri için kapasite planlaması makalesine](../hdinsight-capacity-planning.md)bakın.

## <a name="use-recommended-virtual-machine-type-for-cluster"></a>Küme için önerilen sanal makine türünü kullanma

Her HDInsight kümesi türü için önerilen sanal makine türleri için [kümeler için Varsayılan düğüm yapılandırması ve sanal makine boyutlarına](../hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters) bakın.

## <a name="check-hadoop-components-availability-in-hdinsight"></a>HDInsight'ta Hadoop bileşenlerinin kullanılabilirliğini kontrol edin

Her HDInsight sürümü, hadoop eko-sistem bileşenleri kümesinin bulut dağılımıdır. Tüm HDInsight bileşenleri ve mevcut sürümleri hakkında ayrıntılar için [HDInsight Bileşen Sürümü'ne](../hdinsight-component-versioning.md) bakın.

HdInsight'taki Hadoop bileşenlerini ve sürümlerini kontrol etmek için Apache Ambari UI veya Ambari REST API'yi de kullanabilirsiniz.

Şirket içi kümelerde bulunan ancak HDInsight kümelerinin bir parçası olmayan uygulamalar veya bileşenler, HDInsight kümesiyle aynı VNet'te kenar düğümüne veya VM'ye eklenebilir. Azure HDInsight'ta bulunmayan bir üçüncü taraf Hadoop uygulaması, HDInsight kümesindeki "Uygulamalar" seçeneği kullanılarak yüklenebilir. Özel Hadoop uygulamaları HDInsight kümesine "komut dosyası eylemleri" kullanılarak yüklenebilir. Aşağıdaki tabloda, yaygın uygulamalardan bazıları ve HDInsight tümleştirme seçenekleri listeleneb::

|**Uygulama**|**Tümleştirme**
|---|---|
|Hava akımı|IaaS veya HDInsight kenar düğümü
|Alluxio|IaaS  
|Arcadia|IaaS 
|Atlas|Yok (Sadece HDP)
|Datameer|HDInsight kenar düğümü
|Datastax (Cassandra)|IaaS (CosmosDB Azure'da bir alternatif)
|Veri Torrent|IaaS 
|Drill|IaaS 
|Tutuştur|IaaS
|Jethro|IaaS 
|Mapador|IaaS 
|Mongo|IaaS (CosmosDB Azure'da bir alternatif)
|NiFi|IaaS 
|Presto|IaaS veya HDInsight kenar düğümü
|Piton 2|PaaS 
|Python 3|PaaS 
|R|PaaS 
|Sas|IaaS 
|Vertica|IaaS (SQLDW Azure'da bir alternatif)
|Tableau|IaaS 
|Su Hattı|HDInsight kenar düğümü
|Akış Setleri|HDInsight kenarı 
|Palantir|IaaS 
|Yelken noktası|Iaas 

Daha fazla bilgi için, farklı HDInsight sürümleri ile kullanılabilir makale [Apache Hadoop bileşenleri](../hdinsight-component-versioning.md#apache-hadoop-components-available-with-different-hdinsight-versions) bakın

## <a name="customize-hdinsight-clusters-using-script-actions"></a>Komut dosyası eylemlerini kullanarak HDInsight kümelerini özelleştirme

**HDInsight, komut dosyası eylemi**adı verilen küme yapılandırma yöntemini sağlar. Komut dosyası eylemi, HDInsight kümesindeki düğümlerde çalışan ve ek bileşenler yüklemek ve yapılandırma ayarlarını değiştirmek için kullanılabilen Bash komut dosyasıdır.

Komut dosyası eylemleri, HDInsight kümesinden erişilebilen bir URI'de depolanmalıdır. Küme oluşturma sırasında veya sonrasında kullanılabilir ler ve yalnızca belirli düğüm türlerinde çalışmak üzere kısıtlanabilirler.

Komut dosyası bir kez kalıcı veya yürütülebilir. Kalıcı komut dosyaları, ölçekleme işlemleri yoluyla kümeye eklenen yeni alt düğümleri özelleştirmek için kullanılır. Kalıcı bir komut dosyası, ölçekleme işlemleri gerçekleştiğinde kafa düğümü gibi başka bir düğüm türüne de değişiklikler uygulayabilir.

HDInsight, aşağıdaki bileşenleri HDInsight kümelerine yüklemek için önceden yazılmış komut dosyaları sağlar:

- Azure Depolama hesabı ekleme
- Ton Yükle
- Presto'ya Yükleyin
- Solr'u Yükle
- Giraph'ı Yükleyin
- Ön yükleme Hive kitaplıkları
- Mono’yu yükleme veya güncelleştirme

> [!Note]  
> HDInsight, komut dosyası eylemleri kullanılarak yüklenen özel hadoop bileşenleri veya bileşenleri için doğrudan destek sağlamaz.

Komut dosyası eylemleri, HDInsight uygulaması olarak Azure Marketi'nde de yayımlanabilir.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [HDInsight'a üçüncü taraf Apache Hadoop Uygulamalarını yükleme](../hdinsight-apps-install-applications.md)
- [Komut dosyası eylemlerini kullanarak HDInsight kümelerini özelleştirme](../hdinsight-hadoop-customize-cluster-linux.md)
- [Azure Marketi'nde bir HDInsight uygulaması yayınlama](../hdinsight-apps-publish-applications.md)

## <a name="customize-hdinsight-configs-using-bootstrap"></a>Bootstrap kullanarak HDInsight configs özelleştirin

Gibi config dosyalarında `core-site.xml`configs değişiklikler `hive-site.xml` `oozie-env.xml` ve Bootstrap kullanılarak yapılabilir. Aşağıdaki komut dosyası Powershell [AZ modülü](https://docs.microsoft.com/powershell/azure/new-azureps-module-az) cmdlet [New-AzHDInsightClusterConfig](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster)kullanarak bir örnektir:

```powershell
# hive-site.xml configuration
$hiveConfigValues = @{"hive.metastore.client.socket.timeout"="90"}

$config = New—AzHDInsightClusterConfig '
    | Set—AzHDInsightDefaultStorage
    —StorageAccountName "$defaultStorageAccountName.blob. core.windows.net" `
    —StorageAccountKey "defaultStorageAccountKey " `
    | Add—AzHDInsightConfigValues `
        —HiveSite $hiveConfigValues

New—AzHDInsightCluster `
    —ResourceGroupName $existingResourceGroupName `
    —Cluster-Name $clusterName `
    —Location $location `
    —ClusterSizeInNodes $clusterSizeInNodes `
    —ClusterType Hadoop `
    —OSType Linux `
    —Version "3.6" `
    —HttpCredential $httpCredential `
    —Config $config
```

Daha fazla bilgi için, [Bootstrap kullanarak HDInsight kümelerini özelleştir' makalesine](../hdinsight-hadoop-customize-cluster-bootstrap.md)bakın.  Ayrıca bakınız, [Apache Ambari REST API'yi kullanarak HDInsight kümelerini yönetin.](../hdinsight-hadoop-manage-ambari-rest-api.md)

## <a name="access-client-tools-from-hdinsight-hadoop-cluster-edge-nodes"></a>HDInsight Hadoop küme kenar düğümlerinden istemci araçlarına erişin

Boş kenar düğümü, kafa düğümlerinde olduğu gibi aynı istemci araçlarının yüklenmiş ve yapılandırılan, ancak Hadoop hizmetlerinin çalışmadığı bir Linux sanal makinesidir. Kenar düğümü aşağıdaki amaçlar için kullanılabilir:

- kümeye erişim
- istemci uygulamalarını test etme
- müşteri uygulamalarını barındırma

Kenar düğümleri Azure portalı üzerinden oluşturulabilir ve silinebilir ve küme oluşturma sırasında veya sonrasında kullanılabilir. Kenar düğümü oluşturulduktan sonra, SSH kullanarak kenar düğümüne bağlanabilir ve HDInsight'ta Hadoop kümesine erişmek için istemci araçlarını çalıştırabilirsiniz. Kenar düğümü ssh bitiş `<EdgeNodeName>.<ClusterName>-ssh.azurehdinsight.net:22`noktası .


Daha fazla bilgi için, [HDInsight'taki Apache Hadoop kümelerinde boş kenar düğümleri kullanın makalesine](../hdinsight-apps-use-edge-node.md)bakın.


## <a name="use-scale-up-and-scale-down-feature-of-clusters"></a>Kümelerin ölçeklendirme ve küçültme özelliğini kullanma

HDInsight, kümelerinizdeki işçi düğümlerinin sayısını ölçeklendirme ve küçültme seçeneği sunarak esneklik sağlar. Bu özellik, bir kümeyi saatler sonra veya hafta sonları küçültmenize ve yoğun iş talepleri sırasında genişletmenize olanak tanır. Daha fazla bilgi için bkz.

* [HDInsight kümelerini ölçeklendirin.](../hdinsight-scaling-best-practices.md)
* [Kümeleri ölçeklendir.](../hdinsight-administer-use-portal-linux.md#scale-clusters)

## <a name="use-hdinsight-with-azure-virtual-network"></a>Azure Sanal Ağı ile HDInsight'ı kullanma

Azure Sanal Ağlar, Azure Sanal Makineleri gibi Azure kaynaklarının ağ trafiğini filtreleyerek ve yönlendirme yaparak birbirleriyle, internetle ve şirket içi ağlarla güvenli bir şekilde iletişim kurmasını sağlar.

HDInsight ile Azure Sanal Ağı'nı kullanmak aşağıdaki senaryoları sağlar:

- HDInsight'a doğrudan şirket içi bir ağdan bağlanma.
- HDInsight'ı bir Azure Sanal ağındaki veri depolarına bağlama.
- İnternet üzerinden herkese açık olmayan Hadoop hizmetlerine doğrudan erişme. Örneğin, Kafka API'leri veya HBase Java API'si.

HDInsight, yeni veya mevcut bir Azure Sanal Ağına eklenebilir. HDInsight varolan bir Sanal Ağa ekleniyorsa, Azure veri merkezindeki [birkaç IP adresine](../hdinsight-management-ip-addresses.md) sınırsız erişim sağlamak için varolan ağ güvenlik gruplarının ve kullanıcı tanımlı yolların güncellenmesi gerekir. Ayrıca, HDInsight hizmetleri tarafından kullanılan [bağlantı noktalarına](../hdinsight-plan-virtual-network-deployment.md#hdinsight-ports)gelen trafiği engellemeyin.

> [!Note]  
> HDInsight şu anda zorunlu tünel kazmayı desteklemiyor. Zorunlu tünel oluşturma, giden Internet trafiğini denetim ve günlüğe kaydetme aygıtına zorlayan bir alt ağ ayarıdır. HDInsight'ı bir alt ağa yüklemeden önce zorunlu tünel oluşturmayı kaldırın veya HDInsight için yeni bir alt ağ oluşturun. HDInsight ayrıca giden ağ bağlantısının kısıtlanmasına da destek vermez.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure sanal ağlara genel bakış](../../virtual-network/virtual-networks-overview.md)
- [Azure Sanal Ağını kullanarak Azure HDInsight kapsamını genişletme](../hdinsight-plan-virtual-network-deployment.md)

## <a name="securely-connect-to-azure-services-with-azure-virtual-network-service-endpoints"></a>Azure Sanal Ağ hizmeti bitiş noktalarıyla Azure hizmetlerine güvenli bir şekilde bağlanma

HDInsight, Azure Blob Depolama, Azure Veri Gölü Depolama Gen2, Cosmos DB ve SQL veritabanlarına güvenli bir şekilde bağlanmanızı sağlayan [sanal ağ hizmeti uç noktalarını](../../virtual-network/virtual-network-service-endpoints-overview.md)destekler. Azure HDInsight için bir Hizmet Bitiş Noktası etkinleştirerek, trafik Azure veri merkezi nin içinden güvenli bir rota üzerinden akar. Ağ katmanındaki bu gelişmiş güvenlik düzeyiyle, büyük veri depolama hesaplarını belirtilen Sanal Ağlara (VNETs) kilitleyebilir ve bu verilere erişmek ve işlemek için HDInsight kümelerini sorunsuz bir şekilde kullanabilirsiniz.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Sanal ağ hizmet uç noktaları](../../virtual-network/virtual-network-service-endpoints-overview.md)
- [HDInsight güvenliğini hizmet bitiş noktalarıyla geliştirin](https://azure.microsoft.com/blog/enhance-hdinsight-security-with-service-endpoints/)

## <a name="connect-hdinsight-to-the-on-premises-network"></a>HDInsight'ı şirket içi ağa bağlayın

HDInsight, Azure Sanal Ağlar ve VPN ağ geçidi kullanılarak şirket içi ağa bağlanabilir. Bağlantı kurmak için aşağıdaki adımlar kullanılabilir:

- HDInsight'ı şirket içi ağa bağlantısı olan bir Azure Sanal Ağı'nda kullanın.
- Sanal ağ ve şirket içi ağ arasında DNS ad çözümlemesi yapılandırın.
- Ağ trafiğini denetlemek için ağ güvenlik gruplarını veya kullanıcı tanımlı yolları (UDR) yapılandırın.

Daha fazla bilgi için [HDInsight'ı şirket içi ağınıza bağlayın makalesine](../connect-on-premises-network.md) bakın

## <a name="next-steps"></a>Sonraki adımlar

Bu serinin bir sonraki makalesini okuyun: [Azure HDInsight Hadoop geçişi için şirket içi depolama en iyi uygulamaları.](apache-hadoop-on-premises-migration-best-practices-storage.md)
