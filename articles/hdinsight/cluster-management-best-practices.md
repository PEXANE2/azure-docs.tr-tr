---
title: Küme yönetimi en iyi uygulamaları - Azure HDInsight
description: HDInsight kümelerini yönetmek için en iyi uygulamaları öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 3010c1d597907e7d5c7f82c8b42721dc1f934f6f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74782027"
---
# <a name="hdinsight-cluster-management-best-practices"></a>HDInsight küme yönetimi en iyi uygulamaları

HDInsight kümelerini yönetmek için en iyi uygulamaları öğrenin.

## <a name="how-do-i-create-hdinsight-clusters"></a>HDInsight kümelerini nasıl oluştururum?

| Seçenek | Belgeler |
|---|---|
| Azure Data Factory | [Azure Veri Fabrikası'nı kullanarak HDInsight'ta isteğe bağlı Apache Hadoop kümeleri oluşturun](./hdinsight-hadoop-create-linux-clusters-adf.md) |
| Özel Kaynak Yöneticisi şablonu | [Kaynak Yöneticisi şablonlarını kullanarak HDInsight'ta Apache Hadoop kümeleri oluşturma](./hdinsight-hadoop-create-linux-clusters-arm-templates.md) |
| Hızlı başlangıç şablonları | [HDInsight Quickstart şablonları](https://azure.microsoft.com/resources/templates/?term=hdinsight) |
| Azure örneklerinden | [HDInsight Azure örnekleri](https://docs.microsoft.com/samples/browse/?products=azure-hdinsight) |
| Azure portalında | [Azure portalını kullanarak HDInsight'ta Linux tabanlı kümeler oluşturun](./spark/apache-spark-intellij-tool-plugin.md) |
| Azure CLI | [Azure CLI'yi kullanarak HDInsight kümeleri oluşturma](./hdinsight-hadoop-create-linux-clusters-azure-cli.md) |
| Azure PowerShell | [Azure PowerShell'i kullanarak HDInsight'ta Linux tabanlı kümeler oluşturun](./hdinsight-hadoop-create-linux-clusters-azure-powershell.md) |
| cURL | [Azure REST API'sini kullanarak Apache Hadoop kümeleri oluşturma](./hdinsight-hadoop-create-linux-clusters-curl-rest.md) |
| SDK'lar (.NET, Python, Java) | [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight?view=azure-dotnet), [Python](https://docs.microsoft.com/python/api/overview/azure/hdinsight?view=azure-python), [Java](https://docs.microsoft.com/java/api/overview/azure/hdinsight?view=azure-java-stable), [Git](https://docs.microsoft.com/azure/hdinsight/hdinsight-go-sdk-overview) |

> [!Note]
> Bir küme oluşturuyorve daha önce oluşturulmuş bir kümeden küme adını yeniden kullanıyorsanız, kümenizi oluşturmadan önce önceki küme silme işleminin tamamlanmasını bekleyin.

## <a name="how-do-i-customize-hdinsight-clusters"></a>HDInsight kümelerini nasıl özelleştirebilirim?

| Seçenek | Belgeler |
|---|---|
| Komut dosyası eylemleri | [Komut dosyası eylemlerini kullanarak Azure HDInsight kümelerini özelleştirme](./hdinsight-hadoop-customize-cluster-linux.md) |
| Bootstrap | [Bootstrap kullanarak HDInsight kümelerini özelleştirin](./hdinsight-hadoop-customize-cluster-bootstrap.md) |
| Dış metastore'lar | [Azure HDInsight’ta dış meta veri depolarını kullanma](./hdinsight-use-external-metadata-stores.md) |
| Özel Ambari DB | [ÖZEL Ambari DB ile HDInsight kümelerini ayarlama](./hdinsight-custom-ambari-db.md) |

## <a name="what-are-some-errors-i-might-face-when-creating-clusters"></a>Kümeoluştururken karşılaşabileceğim bazı hatalar nelerdir?

| Hata | Daha fazla bilgi |
|---|---|
| Kota yok | Her bölgede aboneliğinizde oluşturabileceğiniz kota sayısı için kotalar vardır. Daha fazla bilgi için kapasite [planlamasına bakın: kotalar.](./hdinsight-capacity-planning.md) |
| Başka IP adresi yok | Her VNet'in sınırlı sayıda IP adresi vardır. Bir HDInsight kümesi oluşturduğunuzda, her düğüm (zookeeper ve ağ geçidi düğümleri dahil) bu ayrılan IP adreslerinden bazılarını kullanır. Tüm IP adresleri kullanımda olduğunda, bu hatayla karşılaşacaksınız.  |
| Ağ güvenliği grubu (NSG) kuralları HDInsight kaynak sağlayıcılarıyla iletişime izin vermez | HDInsight kümenize gelen trafiği denetlemek için NSG'leri veya kullanıcı tanımlı yolları (ÜDR) kullanıyorsanız, kümenizin kritik Azure sistem durumu ve yönetim hizmetleriyle iletişim kurabileceğinden emin olmalısınız. Daha fazla bilgi için [Azure HDInsight için Ağ güvenlik grubu (NSG) hizmet etiketlerine](./hdinsight-service-tags.md) bakın |
| Küme adının yeniden kullanımı | Daha önce kullandığınız bir küme adı kullandığınızda, kümeyi yeniden oluşturmadan önce X dakika sayısını beklemeniz gerekir. Aksi takdirde, kaynağın zaten var olduğunu belirten bir ileti görürsünüz. |

## <a name="how-do-i-manage-running-hdinsight-clusters"></a>HDInsight kümelerini çalıştırmayı nasıl yönetirim?

| Seçenek | Belgeler |
|---|---|
| Otomatik Ölçeklendirme | [Azure HDInsight kümelerini otomatik olarak ölçeklendirin](./hdinsight-autoscale-clusters.md) |
| El ile ölçeklendirme | [Azure HDInsight kümelerini ölçeklendirin](./hdinsight-scaling-best-practices.md) |
| Ambari ile İzleme| [Azure HDInsight'ta küme performansını izleme](./hdinsight-key-scenarios-to-monitor.md) |
| Azure Monitor günlükleriyle izleme | [Azure İzleyici günlüklerini kullanarak HDInsight kümelerini izleme](./hdinsight-hadoop-oms-log-analytics-tutorial.md) |

## <a name="how-do-i-check-on-deleted-hdinsight-clusters"></a>Silinen HDInsight kümelerini nasıl kontrol edebilirim?

### <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

Silinen kümeleri izlemek için Azure Monitor günlükleriyle aşağıdaki sorguyu kullanabilirsiniz.

```loganalytics
AzureActivity
| where ResourceProvider == "Microsoft.HDInsight" and (OperationName == "Create or Update Cluster" or OperationName == "Delete Cluster") and ActivityStatus == "Succeeded"
```

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight kümeleri için kapasite planlaması](./hdinsight-capacity-planning.md)
* [Azure HDInsight için varsayılan ve önerilen düğüm yapılandırmaları nelerdir?](./hdinsight-supported-node-configuration.md)