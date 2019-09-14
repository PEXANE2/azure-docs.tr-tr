---
title: Apache Kafka Azure HDInsight 'ta otomatik konu oluşturmayı etkinleştirme
description: HDInsight üzerinde Apache Kafka otomatik olarak konu başlıkları oluşturacak şekilde yapılandırma hakkında bilgi edinin. Auto. Create. konularını ayarlayarak Kafka 'yi yapılandırabilirsiniz. ambarı aracılığıyla veya PowerShell veya Kaynak Yöneticisi şablonları aracılığıyla küme oluşturma sırasında doğru olarak etkinleştirin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/18/2018
ms.openlocfilehash: 5d990a1c39495090fed1c78f1ddf5e879490e6c4
ms.sourcegitcommit: dd69b3cda2d722b7aecce5b9bd3eb9b7fbf9dc0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70960647"
---
# <a name="how-to-configure-apache-kafka-on-hdinsight-to-automatically-create-topics"></a>HDInsight üzerinde Apache Kafka otomatik olarak konu başlıkları oluşturacak şekilde yapılandırma

Varsayılan olarak, HDInsight üzerinde [Apache Kafka](https://kafka.apache.org/) otomatik konu oluşturmayı etkinleştirmez. [Apache ambarı](https://ambari.apache.org/)'nı kullanarak mevcut kümeler için otomatik konu oluşturmayı etkinleştirebilirsiniz. Ayrıca, bir Azure Resource Manager şablonu kullanarak yeni bir Kafka kümesi oluştururken otomatik konu oluşturmayı etkinleştirebilirsiniz.

## <a name="apache-ambari-web-ui"></a>Apache ambarı Web Kullanıcı arabirimi

Ambarı Web Kullanıcı arabirimi aracılığıyla mevcut bir kümede otomatik konu oluşturmayı etkinleştirmek için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com), Kafka kümesini seçin.

2. __Kümeye Genel Bakış__' da __küme panosu__' nu seçin. 

    ![Küme panosu seçiliyken portalın görüntüsü](./media/apache-kafka-auto-create-topics/kafka-cluster-overview.png)

3. Ardından __HDInsight kümesi panosu__' nu seçin. İstendiğinde, küme için oturum açma (yönetici) kimlik bilgilerini kullanarak kimlik doğrulaması yapın.

    ![HDInsight kümesi Pano girişinin görüntüsü](./media/apache-kafka-auto-create-topics/hdinsight-cluster-dashboard.png)

3. Sayfanın solundaki listeden Kafka hizmetini seçin.

    ![Hizmet listesi](./media/apache-kafka-auto-create-topics/hdinsight-service-list.png)

4. Sayfanın ortasında configs ' ı seçin.

    ![Hizmet yapılandırma sekmesi](./media/apache-kafka-auto-create-topics/hdinsight-service-config.png)

5. Filtre alanına bir değeri `auto.create`girin. 

    ![Filtre alanının görüntüsü](./media/apache-kafka-auto-create-topics/hdinsight-filter-field.png)

    Bu, Özellik listesini filtreler ve `auto.create.topics.enable` ayarı görüntüler.

6. Değerini `auto.create.topics.enable` olarak`true`değiştirin ve ardından Kaydet ' i seçin. Bir Note ekleyin ve sonra yeniden Kaydet ' i seçin.

    ![Auto. Create. konular. Enable girişinin görüntüsü](./media/apache-kafka-auto-create-topics/auto-create-topics-enable.png)

7. Kafka hizmetini seçin, __Yeniden Başlat__' ı seçin ve ardından __etkilenen tümünü yeniden Başlat__' ı seçin. İstendiğinde, __Tümünü Yeniden Başlat__' ı seçin.

    ![Yeniden başlatma seçiminin görüntüsü](./media/apache-kafka-auto-create-topics/restart-all-affected.png)

> [!NOTE]  
> Ayrıca, ambarı değerlerini ambarı REST API aracılığıyla da ayarlayabilirsiniz. Bu, geçerli yapılandırmayı almak için birden çok REST çağrısı yapmak, değiştirmek, vb. için genellikle daha zordur. Daha fazla bilgi için [Apache ambarı REST API belgelerini kullanarak HDInsight kümelerini yönetme](../hdinsight-hadoop-manage-ambari-rest-api.md) bölümüne bakın.

## <a name="resource-manager-templates"></a>Resource Manager şablonları

Bir Azure Resource Manager şablonu kullanarak bir Kafka kümesi oluştururken, doğrudan öğesine ekleyerek `auto.create.topics.enable` `kafka-broker`ayarlayabilirsiniz. Aşağıdaki JSON kod parçacığında bu değerin `true`nasıl ayarlanacağı gösterilmektedir:

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

* [Apache Kafka günlüklerini çözümle](apache-kafka-log-analytics-operations-management.md)
* [Apache Kafka kümeleri arasında veri çoğaltma](apache-kafka-mirroring.md)
