---
title: Apache Kafka Azure HDInsight 'ta otomatik konu oluşturmayı etkinleştirme
description: HDInsight üzerinde Apache Kafka otomatik olarak konu başlıkları oluşturacak şekilde yapılandırma hakkında bilgi edinin. Kafka `auto.create.topics.enable` 'i, ambarı aracılığıyla true olarak ayarlayarak yapılandırabilirsiniz. Ya da PowerShell veya Kaynak Yöneticisi şablonları aracılığıyla küme oluşturma sırasında.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive,seoapr2020
ms.date: 04/28/2020
ms.openlocfilehash: 829f91452725615af4d444426e25ffad62d6ab6d
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86087528"
---
# <a name="how-to-configure-apache-kafka-on-hdinsight-to-automatically-create-topics"></a>HDInsight üzerinde Apache Kafka otomatik olarak konu başlıkları oluşturacak şekilde yapılandırma

Varsayılan olarak, HDInsight üzerinde Apache Kafka otomatik konu oluşturmayı etkinleştirmez. Apache ambarı 'nı kullanarak mevcut kümeler için otomatik konu oluşturmayı etkinleştirebilirsiniz. Ayrıca, bir Azure Resource Manager şablonu kullanarak yeni bir Kafka kümesi oluştururken otomatik konu oluşturmayı etkinleştirebilirsiniz.

## <a name="apache-ambari-web-ui"></a>Apache ambarı Web Kullanıcı arabirimi

Ambarı Web Kullanıcı arabirimi aracılığıyla mevcut bir kümede otomatik konu oluşturmayı etkinleştirmek için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com), Kafka kümenizi seçin.

1. **Küme panolarında**, **ambarı giriş**' i seçin.

    ![Küme panosu seçiliyken portalın görüntüsü](./media/apache-kafka-auto-create-topics/azure-portal-cluster-dashboard-ambari.png)

    İstendiğinde, küme için oturum açma (yönetici) kimlik bilgilerini kullanarak kimlik doğrulaması yapın. Bunun yerine, `https://CLUSTERNAME.azurehdinsight.net/` Kafka Kümenizin adı Iken Amabrı öğesine doğrudan bağlanabilirsiniz `CLUSTERNAME` .

1. Sayfanın solundaki listeden Kafka hizmetini seçin.

    ![Apache ambarı hizmet listesi sekmesi](./media/apache-kafka-auto-create-topics/hdinsight-service-list.png)

1. Sayfanın ortasında configs ' ı seçin.

    ![Apache ambarı hizmet yapılandırması yapılandırma](./media/apache-kafka-auto-create-topics/hdinsight-service-config.png)

1. Filtre alanına bir değeri girin `auto.create` .

    ![Apache ambarı arama filtresi alanı](./media/apache-kafka-auto-create-topics/hdinsight-filter-field.png)

    Bu ayar, Özellik listesini filtreler ve `auto.create.topics.enable` ayarı görüntüler.

1. Değerini `auto.create.topics.enable` olarak değiştirin `true` ve ardından **Kaydet**' i seçin. Bir Note ekleyin ve sonra yeniden **Kaydet** ' i seçin.

    ![Auto. Create. konular. Enable girişinin görüntüsü](./media/apache-kafka-auto-create-topics/auto-create-topics-enable.png)

1. Kafka hizmetini seçin, __Yeniden Başlat__' ı seçin ve ardından __etkilenen tümünü yeniden Başlat__' ı seçin. İstendiğinde, __Tümünü Yeniden Başlat__' ı seçin.

    ![' Apache ambarı tüm etkilenmeden yeniden Başlat '](./media/apache-kafka-auto-create-topics/restart-all-affected.png)

> [!NOTE]  
> Ayrıca, ambarı değerlerini ambarı REST API aracılığıyla da ayarlayabilirsiniz. Bu, geçerli yapılandırmayı almak için birden çok REST çağrısı yapmak, değiştirmek, vb. için genellikle daha zordur. Daha fazla bilgi için [Apache ambarı REST API belgelerini kullanarak HDInsight kümelerini yönetme](../hdinsight-hadoop-manage-ambari-rest-api.md) bölümüne bakın.

## <a name="resource-manager-templates"></a>Resource Manager şablonları

Bir Azure Resource Manager şablonu kullanarak bir Kafka kümesi oluştururken, doğrudan öğesine `auto.create.topics.enable` ekleyerek ayarlayabilirsiniz `kafka-broker` . Aşağıdaki JSON kod parçacığında bu değerin nasıl ayarlanacağı gösterilmektedir `true` :

```json
"clusterDefinition": {
    "kind": "kafka",
    "configurations": {
    "gateway": {
        "restAuthCredential.isEnabled": true,
        "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
        "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
    },
    "kafka-broker": {
        "auto.create.topics.enable": "true"
    }
}
```

## <a name="next-steps"></a>Sonraki Adımlar

Bu belgede, HDInsight üzerinde Apache Kafka için otomatik konu oluşturmayı nasıl etkinleştireceğinizi öğrendiniz. Kafka ile çalışma hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın:

* [Apache Kafka günlüklerini çözümleme](apache-kafka-log-analytics-operations-management.md)
* [Apache Kafka kümeleri arasında veri çoğaltma](apache-kafka-mirroring.md)
