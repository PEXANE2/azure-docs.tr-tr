---
title: Apache Kafka artış ölçeği - Azure HDInsight
description: Ölçeklenebilirliği artırmak için Azure HDInsight üzerinde Apache Kafka kümesi için yönetilen diskleri yapılandırmayı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/09/2019
ms.openlocfilehash: 56c25b7c77809a5cb7f4e539cff8e1815cd9976f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77031723"
---
# <a name="configure-storage-and-scalability-for-apache-kafka-on-hdinsight"></a>HDInsight üzerinde Apache Kafka için depolamayı ve ölçeklenebilirliği yapılandırma

HDInsight'ta [Apache Kafka](https://kafka.apache.org/) tarafından kullanılan yönetilen disk sayısını nasıl yapılandıracağınızı öğrenin.

HDInsight üzerinde Kafka, HDInsight kümesindeki sanal makinelerin yerel diskini kullanır. Kafka G/Ç açısından oldukça yoğun olduğundan yüksek aktarım hızı ve düğüm başına daha fazla depolama alanı sağlamak için [Azure Yönetilen Diskler](../../virtual-machines/windows/managed-disks-overview.md) kullanılır. Kafka için geleneksel sanal sabit diskler (VHD) kullanıldıysa her düğüm 1 TB ile sınırlıdır. Yönetilen disklerle kümedeki her düğüm için 16 TB elde etmek üzere birden çok disk kullanabilirsiniz.

Aşağıdaki diyagramda, yönetilen diskli HDInsight üzerinde Kafka ile yönetilen disksiz HDInsight üzerinde Kafka karşılaştırılmaktadır:

![yönetilen diskler mimarisi ile kafka](./media/apache-kafka-scalability/kafka-with-managed-disks-architecture.png)

## <a name="configure-managed-disks-azure-portal"></a>Yönetilen diskleri yapılandırma: Azure portalı

1. Portalı kullanarak küme oluşturmaya yönelik genel adımları öğrenmek için [HDInsight kümesi oluşturma](../hdinsight-hadoop-create-linux-clusters-portal.md) bölümündeki adımları uygulayın. Portal oluşturma işlemini tamamlamayın.

2. Yapılandırma **& Fiyatlandırması** bölümünden, disk sayısını yapılandırmak için __Düğüm Sayısı__ alanını kullanın.

    > [!NOTE]  
    > Yönetilen diskin türü __Standart__ (HDD) veya __Premium__ (SSD) olabilir. Premium diskler, DS ve GS serisi VM'lerle kullanılır. Diğer tüm VM türleri standart disk kullanır.

    ![alt düğüm başına diskler ile küme boyutu bölümü vurgulanır](./media/apache-kafka-scalability/azure-portal-cluster-configuration-pricing-kafka-disks.png)

## <a name="configure-managed-disks-resource-manager-template"></a>Yönetilen diskleri yapılandırma: Resource Manager şablonu

Kafka kümesindeki çalışan düğümleri tarafından kullanılan disk sayısını denetlemek için şablonun şu bölümünü kullanın:

```json
"dataDisksGroups": [
    {
        "disksPerNode": "[variables('disksPerWorkerNode')]"
    }
    ],
```

Yönetilen disklerin [https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json](https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json)nasıl yapılandırılacağınızı gösteren eksiksiz bir şablon bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

HDInsight'ta Apache Kafka ile çalışma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

* [HDInsight'ta Apache Kafka'nın bir kopyasını oluşturmak için MirrorMaker'ı kullanın](apache-kafka-mirroring.md)
* [Apache Storm'u HdInsight'ta Apache Kafka ile kullanın](../hdinsight-apache-storm-with-kafka.md)
* [Apache Spark'ı Apache Kafka ile HDInsight'ta kullanın](../hdinsight-apache-spark-with-kafka.md)
* [Azure Sanal Ağı üzerinden Apache Kafka'ya bağlanın](apache-kafka-connect-vpn-gateway.md)

* [Apache Kafka ile yönetilen diskler HDInsight günlüğü](https://azure.microsoft.com/blog/announcing-public-preview-of-apache-kafka-on-hdinsight-with-azure-managed-disks/)
