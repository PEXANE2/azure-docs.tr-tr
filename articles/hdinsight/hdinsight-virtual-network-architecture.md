---
title: Azure HDInsight sanal ağ mimarisi
description: Azure Sanal Ağı'nda bir HDInsight kümesi oluşturduğunuzda kullanılabilir kaynakları öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/14/2020
ms.openlocfilehash: ad0e0250b32f2bdef4944e6e148be3215f3822f7
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81390216"
---
# <a name="azure-hdinsight-virtual-network-architecture"></a>Azure HDInsight sanal ağ mimarisi

Bu makalede, bir HDInsight kümesini özel bir Azure Sanal Ağına dağıttığınızda mevcut olan kaynaklar açıklanmaktadır. Bu bilgiler, şirket içi kaynakları Azure'daki HDInsight kümenize bağlamanıza yardımcı olur. Azure Sanal Ağlar hakkında daha fazla bilgi [için](../virtual-network/virtual-networks-overview.md)bkz.

## <a name="resource-types-in-azure-hdinsight-clusters"></a>Azure HDInsight kümelerinde kaynak türleri

Azure HDInsight kümelerinde farklı türde sanal makineler veya düğümler vardır. Her düğüm türü sistemin çalışmasında bir rol oynar. Aşağıdaki tabloda bu düğüm türleri ve kümedeki rolleri özetlenir.

| Tür | Açıklama |
| --- | --- |
| Baş düğüm |  Apache Storm dışındaki tüm küme türleri için, baş düğümleri dağıtılmış uygulamanın yürütülmesini yöneten işlemleri barındırAr. Kafa düğümü, küme kaynakları arasında çalıştırmak üzere koordine edilen uygulamaları ssh'e sokabileceğiniz ve yürütebileceğiniz düğümdür. Kafa düğümlerinin sayısı tüm küme türleri için iki olarak sabitlenir. |
| ZooKeeper düğümü | Zookeeper, veri işleme yapan düğümler arasındaki görevleri koordine eder. Ayrıca baş düğümülider seçimi yapar ve hangi baş düğümü belirli bir ana hizmet çalışıyor izler. ZooKeeper düğümlerinin sayısı üç olarak sabitlenir. |
| İşçi düğümü | Veri işleme işlevini destekleyen düğümleri temsil eder. İşlem e-mesuliyetini ölçeklendirmek ve maliyetleri yönetmek için altdüğümler kümeden eklenebilir veya kaldırılabilir. |
| R Server kenar düğümü | R Server kenar düğümü, ssh'e sokabileceğiniz düğümü temsil eder ve daha sonra küme kaynakları arasında çalışacak şekilde koordine edilen uygulamaları çalıştırır. Kenar düğümü küme içindeki veri çözümlemesi ile katılmaz. Bu düğüm aynı zamanda R Studio Server'ı barındırarak tarayıcı kullanarak R uygulamasını çalıştırmanızı sağlar. |
| Bölge düğümü | HBase küme türü için bölge düğümü (Veri Düğümü olarak da adlandırılır) Region Server'ı çalıştırAr. Bölge Sunucuları, HBase tarafından yönetilen verilerin bir kısmını hizmet eve getirir ve yönetir. Bilgi işlem yeteneğini ölçeklendirmek ve maliyetleri yönetmek için bölge düğümleri kümeden eklenebilir veya kaldırılabilir.|
| Nimbus düğümü | Fırtına küme türü için Nimbus düğümü Kafa düğümüne benzer işlevsellik sağlar. Nimbus düğümü, Fırtına topolojilerinin çalışmasını koordine eden Zookeeper aracılığıyla kümedeki diğer düğümlere görevler atar. |
| Süpervizör düğümü | Fırtına küme türü için, denetleyici düğümü işlemi yapmak için Nimbus düğümü tarafından sağlanan yönergeleri yürütür. |

## <a name="resource-naming-conventions"></a>Kaynak adlandırma kuralları

Kümenizdeki düğümleri ele alırken Tam Nitelikli Alan Adları (FQDNs) kullanın. [Ambari API'yi](hdinsight-hadoop-manage-ambari-rest-api.md)kullanarak kümenizdeki çeşitli düğüm türleri için FQDN'leri alabilirsiniz.

Bu FQDNs şeklinde `<node-type-prefix><instance-number>-<abbreviated-clustername>.<unique-identifier>.cx.internal.cloudapp.net`olacaktır.

Headnodes `<node-type-prefix>` için *hn* olacak, işçi düğümleri için *wn* ve zookeeper düğümleri için *zn.*

Yalnızca ana bilgisayar adına ihtiyacınız varsa, FQDN'nin yalnızca ilk bölümünü kullanın:`<node-type-prefix><instance-number>-<abbreviated-clustername>`

## <a name="basic-virtual-network-resources"></a>Temel sanal ağ kaynakları

Aşağıdaki diyagram, AZURE'da HDInsight düğümlerinin ve ağ kaynaklarının yerleşimini gösterir.

![Azure özel VNET'te oluşturulan HDInsight varlıklarının diyagramı](./media/hdinsight-virtual-network-architecture/hdinsight-vnet-diagram.png)

Azure Sanal Ağı'ndaki varsayılan kaynaklar, önceki tabloda belirtilen küme düğümü türlerini içerir. Ve sanal ağ ve dış ağlar arasındaki iletişimi destekleyen ağ aygıtları.

Aşağıdaki tablo, HDInsight özel bir Azure Sanal Ağı'na dağıtıldığında oluşturulan dokuz küme düğümlerini özetler.

| Kaynak türü | Mevcut sayı | Ayrıntılar |
| --- | --- | --- |
|Baş düğüm | iki |    |
|Zookeeper düğümü | Üç | |
|İşçi düğümü | iki | Bu sayı küme yapılandırması ve ölçekleme bağlı olarak değişebilir. Apache Kafka için en az üç işçi düğümü gereklidir.  |
|Ağ geçidi düğümü | iki | Ağ geçidi düğümleri, Azure'da oluşturulan ancak aboneliğinizde görünmeyen Azure sanal makineleridir. Bu düğümleri yeniden başlatmanız gerekiyorsa desteğe başvurun. |

Mevcut aşağıdaki ağ kaynakları HDInsight ile kullanılan sanal ağ içinde otomatik olarak oluşturulur:

| Ağ kaynağı | Mevcut sayı | Ayrıntılar |
| --- | --- | --- |
|Yük dengeleyici | Üç | |
|Ağ Arabirimleri | Dokuz | Bu değer, her düğümün kendi ağ arabirimi olduğu normal bir kümeyi temel alıntır. Dokuz arabirim içindir: iki baş düğümleri, üç zookeeper düğümleri, iki işçi düğümleri ve önceki tabloda belirtilen iki ağ geçidi düğümleri. |
|Genel IP Adresleri | iki |    |

## <a name="endpoints-for-connecting-to-hdinsight"></a>HDInsight'a bağlanmak için uç noktalar

HDInsight kümenize üç şekilde erişebilirsiniz:

- Sanal ağın dışında bir HTTPS `CLUSTERNAME.azurehdinsight.net`bitiş noktası .
- Headnode doğrudan bağlanmak için bir SSH `CLUSTERNAME-ssh.azurehdinsight.net`bitiş noktası.
- Sanal ağ `CLUSTERNAME-int.azurehdinsight.net`içinde bir HTTPS bitiş noktası. Bu URL'deki " "`-int`" ne dikkat edin. Bu bitiş noktası, bu sanal ağdaki özel bir IP'ye çözülür ve herkese açık internetten erişilemez.

Bu üç uç nokta, her birine bir yük dengeleyicisi atanır.

Ortak IP adresleri, sanal ağ dışından bağlantıya izin veren iki uç noktaya da sağlanır.

1. Bir ortak IP, kümeye internetten `CLUSTERNAME.azurehdinsight.net`bağlanırken kullanmak üzere tam nitelikli alan adı (FQDN) için yük dengeleyicisine atanır.
1. İkinci ortak IP adresi SSH yalnızca etki `CLUSTERNAME-ssh.azurehdinsight.net`alanı adı için kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

- [Özel bitiş noktası olan sanal ağdaki HDInsight kümelerine gelen trafiği güvenli hale](https://azure.microsoft.com/blog/secure-incoming-traffic-to-hdinsight-clusters-in-a-vnet-with-private-endpoint/)
