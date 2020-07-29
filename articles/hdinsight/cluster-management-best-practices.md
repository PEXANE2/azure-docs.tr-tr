---
title: Küme yönetimi en iyi uygulamalar-Azure HDInsight
description: HDInsight kümelerini yönetmeye yönelik en iyi yöntemleri öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 3010c1d597907e7d5c7f82c8b42721dc1f934f6f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74782027"
---
# <a name="hdinsight-cluster-management-best-practices"></a>HDInsight kümesi Yönetimi en iyi uygulamaları

HDInsight kümelerini yönetmeye yönelik en iyi yöntemleri öğrenin.

## <a name="how-do-i-create-hdinsight-clusters"></a>HDInsight kümeleri oluşturmak Nasıl yaparım??

| Seçenek | Belgeler |
|---|---|
| Azure Data Factory | [Azure Data Factory kullanarak HDInsight 'ta isteğe bağlı Apache Hadoop kümeleri oluşturma](./hdinsight-hadoop-create-linux-clusters-adf.md) |
| Özel Kaynak Yöneticisi şablonu | [Kaynak Yöneticisi şablonları kullanarak HDInsight 'ta Apache Hadoop kümeleri oluşturma](./hdinsight-hadoop-create-linux-clusters-arm-templates.md) |
| Hızlı başlangıç şablonları | [HDInsight hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/?term=hdinsight) |
| Azure örneklerinden | [HDInsight Azure örnekleri](https://docs.microsoft.com/samples/browse/?products=azure-hdinsight) |
| Azure portal | [Azure portal kullanarak HDInsight 'ta Linux tabanlı kümeler oluşturun](./spark/apache-spark-intellij-tool-plugin.md) |
| Azure CLI | [Azure CLı kullanarak HDInsight kümeleri oluşturma](./hdinsight-hadoop-create-linux-clusters-azure-cli.md) |
| Azure PowerShell | [Azure PowerShell kullanarak HDInsight 'ta Linux tabanlı kümeler oluşturma](./hdinsight-hadoop-create-linux-clusters-azure-powershell.md) |
| cURL | [Azure REST API kullanarak Apache Hadoop kümeleri oluşturma](./hdinsight-hadoop-create-linux-clusters-curl-rest.md) |
| SDK 'lar (.NET, Python, Java) | [.Net](https://docs.microsoft.com/dotnet/api/overview/azure/hdinsight?view=azure-dotnet), [Python](https://docs.microsoft.com/python/api/overview/azure/hdinsight?view=azure-python), [Java](https://docs.microsoft.com/java/api/overview/azure/hdinsight?view=azure-java-stable), [Go](https://docs.microsoft.com/azure/hdinsight/hdinsight-go-sdk-overview) |

> [!Note]
> Küme oluşturuyorsanız ve daha önce oluşturulmuş bir kümeden küme adını yeniden kullanacaksanız, kümenizi oluşturmadan önce önceki küme silme işleminin tamamlanmasını bekleyin.

## <a name="how-do-i-customize-hdinsight-clusters"></a>HDInsight kümelerini özelleştirmek Nasıl yaparım??

| Seçenek | Belgeler |
|---|---|
| Betik eylemleri | [Betik eylemlerini kullanarak Azure HDInsight kümelerini özelleştirme](./hdinsight-hadoop-customize-cluster-linux.md) |
| Bootstrap | [Önyükleme kullanarak HDInsight kümelerini özelleştirme](./hdinsight-hadoop-customize-cluster-bootstrap.md) |
| Dış meta veri | [Azure HDInsight’ta dış meta veri depolarını kullanma](./hdinsight-use-external-metadata-stores.md) |
| Özel Ambari DB | [HDInsight kümelerini özel bir ambarı DB ile ayarlama](./hdinsight-custom-ambari-db.md) |

## <a name="what-are-some-errors-i-might-face-when-creating-clusters"></a>Küme oluştururken yüz yüze olabilecek bazı hatalar nelerdir?

| Hata | Daha fazla bilgi |
|---|---|
| Kota yok | Her bölgede aboneliğinizde oluşturabileceğiniz kota sayısı için kotalar vardır. Daha fazla bilgi için bkz. [Kapasite planlama: kotalar](./hdinsight-capacity-planning.md). |
| Daha fazla IP adresi yok | Her VNet 'in sınırlı sayıda IP adresi vardır. Bir HDInsight kümesi oluşturduğunuzda, her düğüm (Zookeeper ve Gateway düğümleri dahil) Bu ayrılmış IP adreslerinden bazılarını kullanır. Tüm IP adresleri kullanımda olduğunda bu hatayla karşılaşırsınız.  |
| Ağ güvenlik grubu (NSG) kuralları HDInsight kaynak sağlayıcılarıyla iletişime izin vermez | HDInsight kümenize gelen trafiği denetlemek için NSG 'ler veya Kullanıcı tanımlı yollar (UDRs) kullanıyorsanız, kümenizin kritik Azure sistem durumu ve yönetim hizmetleriyle iletişim kurabildiğinden emin olmanız gerekir. Daha fazla bilgi için bkz. [Azure HDInsight Için ağ güvenlik grubu (NSG) hizmet etiketleri](./hdinsight-service-tags.md) |
| Küme adının yeniden kullanılması | Daha önce kullandığınız bir küme adı kullandığınızda, kümeyi yeniden oluşturmadan önce X dakika beklemeniz gerekir. Aksi takdirde, kaynağın zaten var olduğunu belirten bir ileti görürsünüz. |

## <a name="how-do-i-manage-running-hdinsight-clusters"></a>Nasıl yaparım? HDInsight kümelerini yönetmek mı istiyorsunuz?

| Seçenek | Belgeler |
|---|---|
| Otomatik Ölçeklendirme | [Azure HDInsight kümelerini otomatik olarak ölçeklendirme](./hdinsight-autoscale-clusters.md) |
| El ile ölçeklendirme | [Azure HDInsight kümelerini ölçeklendirme](./hdinsight-scaling-best-practices.md) |
| Ambarı ile izleme| [Azure HDInsight 'ta küme performansını izleme](./hdinsight-key-scenarios-to-monitor.md) |
| Azure Izleyici günlükleri ile izleme | [Azure İzleyici günlüklerini kullanarak HDInsight kümelerini izleme](./hdinsight-hadoop-oms-log-analytics-tutorial.md) |

## <a name="how-do-i-check-on-deleted-hdinsight-clusters"></a>Nasıl yaparım? silinen HDInsight kümelerinde denetim yapılsın mı?

### <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

Silinen kümeleri izlemek için aşağıdaki sorguyu Azure Izleyici günlükleri ile birlikte kullanabilirsiniz.

```loganalytics
AzureActivity
| where ResourceProvider == "Microsoft.HDInsight" and (OperationName == "Create or Update Cluster" or OperationName == "Delete Cluster") and ActivityStatus == "Succeeded"
```

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight kümeleri için kapasite planlaması](./hdinsight-capacity-planning.md)
* [Azure HDInsight için varsayılan ve önerilen düğüm yapılandırması nelerdir?](./hdinsight-supported-node-configuration.md)