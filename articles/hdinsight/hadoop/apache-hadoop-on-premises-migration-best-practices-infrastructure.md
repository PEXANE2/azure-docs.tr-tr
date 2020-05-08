---
title: "Altyapı: şirket içi Apache Hadoop Azure HDInsight 'a"
description: Şirket içi Hadoop kümelerini Azure HDInsight 'a geçirmek için en iyi altyapı uygulamalarını öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/06/2019
ms.openlocfilehash: 61d7d2a52f58162d288b1155f9724c7912e451f3
ms.sourcegitcommit: 31236e3de7f1933be246d1bfeb9a517644eacd61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2020
ms.locfileid: "82780105"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight---infrastructure-best-practices"></a>Şirket içi Apache Hadoop kümelerini Azure HDInsight 'a geçirme-altyapı en iyi yöntemleri

Bu makale, Azure HDInsight kümelerinin altyapısını yönetmeye yönelik öneriler sağlar. Şirket içi Apache Hadoop sistemlerini Azure HDInsight 'a geçirmeye yardımcı olmak için en iyi uygulamaları sağlayan bir serinin bir parçasıdır.

## <a name="plan-for-hdinsight-cluster-capacity"></a>HDInsight küme kapasitesi için plan yapma

HDInsight kümesi kapasite planlaması için yapılacak temel seçimler şunlardır:

**Geli**  
Azure bölgesi, kümenin fiziksel olarak sağlandığını belirler. Okuma ve yazma gecikmesini en aza indirmek için, kümenin verilerle aynı bölgede olması gerekir.

**Depolama konumu ve boyutu**  
Varsayılan depolama, kümeyle aynı bölgede olmalıdır.48 düğümlü bir küme için 4 ile 8 arasında depolama hesabı olması önerilir. Toplam depolama alanı zaten yeterli olabilir, ancak her depolama hesabı işlem düğümleri için ek ağ bant genişliği sağlar. Birden çok depolama hesabı olduğunda, ön ek olmadan her depolama hesabı için rastgele bir ad kullanın. Rastgele adlandırma amacı, tüm hesaplarda depolama performans sorunlarını (azaltma) veya genel modlu hataların olasılığını azaltmaktadır. Daha iyi performans için, depolama hesabı başına yalnızca bir kapsayıcı kullanın.

**VM boyutu ve türü (şimdi G serisi destekleniyor)**  
Her küme türü bir dizi düğüm türüne sahiptir ve her düğüm türü, VM boyutu ve türü için özel seçeneklere sahiptir. VM boyutu ve türü, CPU işleme gücü, RAM boyutu ve ağ gecikmesi tarafından belirlenir. Sanal bir iş yükü, her düğüm türü için en iyi VM boyutunu ve türünü tespit etmek üzere kullanılabilir.

**Çalışan düğümlerinin sayısı**  
Çalışan düğümlerinin ilk sayısı, sanal iş yükleri kullanılarak belirlenebilir. Küme, en yüksek yük taleplerini karşılamak için daha fazla çalışan düğümü eklenerek daha sonra ölçeklendirilebilir. Daha sonra ek çalışan düğümleri gerekli olmadığında küme daha sonra yeniden ölçeklendirilebilir.

Daha fazla bilgi için bkz. [HDInsight kümeleri Için kapasite planlaması](../hdinsight-capacity-planning.md).

## <a name="use-recommended-virtual-machine-type-for-cluster"></a>Küme için önerilen sanal makine türünü kullan

Her HDInsight kümesi türü için önerilen sanal makine türlerine yönelik [kümeler için bkz. varsayılan düğüm yapılandırması ve sanal makine boyutları](../hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters) .

## <a name="check-hadoop-components-availability-in-hdinsight"></a>HDInsight 'ta Hadoop bileşenleri kullanılabilirliğini denetleme

Her HDInsight sürümü, bir Hadoop ekonomik sistem bileşenleri kümesinin bulut dağıtımıdır. Tüm HDInsight bileşenleri ve bunların geçerli sürümleri hakkında ayrıntılı bilgi için bkz. [HDInsight bileşen sürümü oluşturma](../hdinsight-component-versioning.md) .

Ayrıca, HDInsight 'taki Hadoop bileşenlerini ve sürümlerini denetlemek için Apache ambarı Kullanıcı arabirimi veya ambarı REST API de kullanabilirsiniz.

Şirket içi kümelerde bulunan ancak HDInsight kümelerinin bir parçası olmayan uygulamalar veya bileşenler, bir kenar düğümüne veya HDInsight kümesiyle aynı VNet 'teki bir sanal makineye eklenebilir. Azure HDInsight üzerinde kullanılamayan bir üçüncü taraf Hadoop uygulaması, HDInsight kümesindeki "uygulamalar" seçeneği kullanılarak yüklenebilir. Özel Hadoop uygulamaları, HDInsight kümesine "betik eylemleri" kullanılarak yüklenebilir. Aşağıdaki tabloda bazı ortak uygulamalar ve bunların HDInsight tümleştirme seçenekleri listelenmektedir:

|**Uygulama**|**Tümleştirme**
|---|---|
|Airflow|IaaS veya HDInsight Edge düğümü
|Alluxıo|IaaS  
|Arcadia|IaaS 
|Atlas|Hiçbiri (yalnızca HDP)
|Datameer|HDInsight Edge düğümü
|DataStax (Cassandra)|IaaS (CosmosDB Azure üzerinde alternatif)
|Veri Toruya|IaaS 
|Drill|IaaS 
|Ignite|IaaS
|Jetro dili|IaaS 
|Mapaçi|IaaS 
|Mongo|IaaS (CosmosDB Azure üzerinde alternatif)
|NiFi|IaaS 
|Presto|IaaS veya HDInsight Edge düğümü
|Python 2|PaaS 
|Python 3|PaaS 
|R|PaaS 
|'LARıNıN|IaaS 
|Vertica|IaaS (Azure 'da SQLDW bir alternatif)
|Tableau|IaaS 
|Su çizgisi|HDInsight Edge düğümü
|StreamSets|HDInsight Edge 
|Palanyapıştır|IaaS 
|Sailpoint|IaaS 

Daha fazla bilgi için, [farklı HDInsight sürümleriyle kullanılabilen bileşenler Apache Hadoop](../hdinsight-component-versioning.md#apache-hadoop-components-available-with-different-hdinsight-versions) makalesine bakın

## <a name="customize-hdinsight-clusters-using-script-actions"></a>Betik eylemlerini kullanarak HDInsight kümelerini özelleştirme

HDInsight, bir **betik eylemi**olarak adlandırılan bir küme yapılandırması yöntemi sağlar. Bir betik eylemi, HDInsight kümesindeki düğümlerde çalıştırılan ve ek bileşenleri yüklemek ve yapılandırma ayarlarını değiştirmek için kullanılabilir olan Bash betiğiyle çalışır.

Betik eylemleri, HDInsight kümesinden erişilebilen bir URI üzerinde depolanmalıdır. Bunlar, küme oluşturma sırasında veya sonrasında kullanılabilir ve yalnızca belirli düğüm türlerinde çalışacak şekilde kısıtlanabilir.

Betik kalıcı olabilir veya bir kez çalıştırılabilir. Kalıcı betikler, ölçek işlemleri aracılığıyla kümeye eklenen yeni çalışan düğümlerini özelleştirmek için kullanılır. Kalıcı bir betik, ölçeklendirme işlemleri gerçekleştiğinde baş düğüm gibi başka bir düğüm türüne değişiklikler de uygulayabilir.

HDInsight, HDInsight kümelerine aşağıdaki bileşenleri yüklemek için önceden yazılmış betikler sağlar:

- Azure depolama hesabı ekleme
- Ton 'yi yükler
- Presto 'A Install
- Solr 'yi yükler
- Giraph 'yi yükler
- Hive kitaplıklarını önceden yükle
- Mono’yu yükleme veya güncelleştirme

> [!Note]  
> HDInsight, özel Hadoop bileşenleri veya betik eylemleri kullanılarak yüklenen bileşenler için doğrudan destek sağlamaz.

Betik eylemleri, Azure Marketi 'Nde de HDInsight uygulaması olarak yayımlanabilir.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [HDInsight 'a üçüncü taraf Apache Hadoop uygulamalarını yükler](../hdinsight-apps-install-applications.md)
- [Betik eylemlerini kullanarak HDInsight kümelerini özelleştirme](../hdinsight-hadoop-customize-cluster-linux.md)
- [Azure Marketi 'nde HDInsight uygulaması yayımlama](../hdinsight-apps-publish-applications.md)

## <a name="customize-hdinsight-configs-using-bootstrap"></a>Önyükleme kullanarak HDInsight yapılandırmalarını özelleştirme

, Gibi yapılandırma dosyalarındaki kurulumunun değişiklikleri ve `oozie-env.xml` önyükleme kullanılarak yapılabilir. `hive-site.xml` `core-site.xml` Aşağıdaki betik, PowerShell [az Module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az) cmdlet [New-AzHDInsightClusterConfig](https://docs.microsoft.com/powershell/module/az.hdinsight/new-azhdinsightcluster)kullanarak bir örnektir:

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

Daha fazla bilgi için bkz. [önyükleme kullanarak HDInsight kümelerini özelleştirme](../hdinsight-hadoop-customize-cluster-bootstrap.md)makalesi.  Ayrıca bkz. [Apache ambarı REST API kullanarak HDInsight kümelerini yönetme](../hdinsight-hadoop-manage-ambari-rest-api.md).

## <a name="access-client-tools-from-hdinsight-hadoop-cluster-edge-nodes"></a>HDInsight Hadoop kümesi kenar düğümlerinden istemci araçlarına erişme

Boş bir Edge düğümü, aynı istemci araçları yüklü ve baş düğümlerde olarak yapılandırılmış, ancak hiçbir Hadoop hizmeti çalıştırmayan bir Linux sanal makinedir. Edge düğümü aşağıdaki amaçlarla kullanılabilir:

- kümeye erişme
- istemci uygulamalarını test etme
- istemci uygulamaları barındırma

Edge düğümleri Azure portal ile oluşturulup silinebilir ve küme oluşturma sırasında veya sonrasında kullanılabilir. Edge düğümü oluşturulduktan sonra, SSH kullanarak kenar düğümüne bağlanabilir ve HDInsight 'ta Hadoop kümesine erişmek için istemci araçlarını çalıştırabilirsiniz. Edge düğümü SSH uç noktası `<EdgeNodeName>.<ClusterName>-ssh.azurehdinsight.net:22`.


Daha fazla bilgi için bkz. [HDInsight 'ta Apache Hadoop kümelerinde boş kenar düğümlerini kullanma](../hdinsight-apps-use-edge-node.md).


## <a name="use-scale-up-and-scale-down-feature-of-clusters"></a>Kümelerin ölçek genişletme ve genişleme özelliğini kullanma

HDInsight, kümelerinizdeki çalışan düğümlerinin sayısını artırma ve azaltma seçeneği sunarak esneklik sağlar. Bu özellik, bir kümeyi saat veya hafta sonları ile küçültmenize ve yoğun iş talepleri sırasında genişletmenize olanak sağlar. Daha fazla bilgi için bkz.

* [HDInsight kümelerini ölçeklendirin](../hdinsight-scaling-best-practices.md).
* [Kümeleri ölçeklendirin](../hdinsight-administer-use-portal-linux.md#scale-clusters).

## <a name="use-hdinsight-with-azure-virtual-network"></a>Azure sanal ağı ile HDInsight kullanma

Azure sanal ağları, ağ trafiğini filtreleyerek ve yönlendirerek, Azure sanal makineleri gibi Azure kaynaklarının birbirleriyle, internet ve şirket içi ağlarla güvenli bir şekilde iletişim kurmasını sağlar.

Azure sanal ağını HDInsight ile kullanmak aşağıdaki senaryoları sunar:

- HDInsight 'a doğrudan şirket içi ağdan bağlanma.
- HDInsight 'ı bir Azure sanal ağındaki veri depolarına bağlama.
- Internet üzerinden genel kullanıma açık olmayan Hadoop hizmetlerine doğrudan erişme. Örneğin, Kafka API 'Leri veya HBase Java API 'SI.

HDInsight, yeni veya mevcut bir Azure sanal ağına eklenebilir. HDInsight var olan bir sanal ağa eklenirse, mevcut ağ güvenlik gruplarının ve Kullanıcı tanımlı yolların, Azure veri merkezi 'nde [bırkaç IP adresine](../hdinsight-management-ip-addresses.md) sınırsız erişime izin verecek şekilde güncelleştirilmeleri gerekir. Ayrıca, HDInsight Hizmetleri tarafından kullanılan [bağlantı noktalarında](../control-network-traffic.md#required-ports)trafiği engellemediğinizden emin olun.

> [!Note]  
> HDInsight Şu anda zorlamalı tüneli desteklemiyor. Zorlamalı tünel, gelen Internet trafiğini İnceleme ve günlüğe kaydetme amacıyla bir cihaza zorlayan bir alt ağ ayarıdır. HDInsight 'ı bir alt ağa yüklemeden önce zorlamalı tüneli kaldırın veya HDInsight için yeni bir alt ağ oluşturun. HDInsight, giden ağ bağlantısının kısıtlanması de desteklemez.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure sanal ağları-genel bakış](../../virtual-network/virtual-networks-overview.md)
- [Azure Sanal Ağını kullanarak Azure HDInsight kapsamını genişletme](../hdinsight-plan-virtual-network-deployment.md)

## <a name="securely-connect-to-azure-services-with-azure-virtual-network-service-endpoints"></a>Azure sanal ağ hizmet uç noktaları ile Azure hizmetlerine güvenli bir şekilde bağlanma

HDInsight, Azure Blob depolama, Azure Data Lake Storage 2., Cosmos DB ve SQL veritabanlarına güvenli bir şekilde bağlanmanıza olanak tanıyan [sanal ağ hizmet uç noktalarını](../../virtual-network/virtual-network-service-endpoints-overview.md)destekler. Azure HDInsight için bir hizmet uç noktası etkinleştirerek trafik, Azure veri merkezi içinden güvenli bir yol üzerinden akar. Ağ katmanında bu gelişmiş güvenlik düzeyiyle, büyük veri depolama hesaplarını belirtilen sanal ağlarına (VNet 'ler) kilitleyebilir ve bu verilere erişmek ve onları işlemek için HDInsight kümelerini sorunsuz bir şekilde kullanmaya devam edebilirsiniz.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Sanal ağ hizmet uç noktaları](../../virtual-network/virtual-network-service-endpoints-overview.md)
- [Hizmet uç noktaları ile HDInsight güvenliğini geliştirme](https://azure.microsoft.com/blog/enhance-hdinsight-security-with-service-endpoints/)

## <a name="connect-hdinsight-to-the-on-premises-network"></a>HDInsight 'ı şirket içi ağa bağlama

HDInsight, Azure sanal ağları ve bir VPN ağ geçidi kullanarak şirket içi ağa bağlanabilir. Aşağıdaki adımlar bağlantı kurmak için kullanılabilir:

- Şirket içi ağa bağlantısı olan bir Azure sanal ağında HDInsight kullanın.
- Sanal ağ ile şirket içi ağ arasında DNS ad çözümlemesini yapılandırın.
- Ağ trafiğini denetlemek için ağ güvenlik gruplarını veya Kullanıcı tanımlı yolları (UDR) yapılandırın.

Daha fazla bilgi için bkz. [HDInsight 'ı şirket içi ağınıza bağlama](../connect-on-premises-network.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu serideki bir sonraki makaleyi okuyun: [geçiş Azure HDInsight Hadoop için En Iyi depolama, şirket içi uygulamalar](apache-hadoop-on-premises-migration-best-practices-storage.md).
