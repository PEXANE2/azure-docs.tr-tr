---
title: Apache Kafka'da otomatik konu oluşturmayı etkinleştirin - Azure HDInsight
description: Apache Kafka'yı HDInsight'ta otomatik olarak konular oluşturmak için nasıl yapılandıracağınızı öğrenin. Kafka'yı ambari üzerinden veya PowerShell veya Resource Manager şablonları aracılığıyla küme oluşturma sırasında gerçeğe uygun olarak auto.create.topics ayarlayarak yapılandırabilirsiniz.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/25/2019
ms.openlocfilehash: 7ec7d15806927306b12624962facbafddf2ce08b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73242368"
---
# <a name="how-to-configure-apache-kafka-on-hdinsight-to-automatically-create-topics"></a>Apache Kafka HDInsight'ta otomatik olarak konu oluşturmak için nasıl yapılandırılabilen

Varsayılan olarak, HDInsight'taki [Apache Kafka](https://kafka.apache.org/) otomatik konu oluşturmayı etkinleştirmez. [Apache Ambari'yi](https://ambari.apache.org/)kullanarak varolan kümeler için otomatik konu oluşturmayı etkinleştirebilirsiniz. Azure Kaynak Yöneticisi şablonu kullanarak yeni bir Kafka kümesi oluştururken otomatik konu oluşturmayı da etkinleştirebilirsiniz.

## <a name="apache-ambari-web-ui"></a>Apache Ambari Web UI

Ambari Web Kullanıcı Arabirimi aracılığıyla varolan bir kümede otomatik konu oluşturmayı etkinleştirmek için aşağıdaki adımları kullanın:

1. Azure [portalından](https://portal.azure.com)Kafka kümenizi seçin.

1. **Küme panolarından** **Ambari home'u**seçin.

    ![Küme panosu seçili portalın görüntüsü](./media/apache-kafka-auto-create-topics/azure-portal-cluster-dashboard-ambari.png)

    İstendiğinde, küme için giriş (yönetici) kimlik bilgilerini kullanarak kimlik doğrulaması yapın. Alternatif olarak, Kafka kümenizin `https://CLUSTERNAME.azurehdinsight.net/` adının `CLUSTERNAME` bulunduğu yerden Amabri'ye doğrudan bağlanabilirsiniz.

1. Sayfanın solundaki listeden Kafka hizmetini seçin.

    ![Apache Ambari hizmet listesi sekmesi](./media/apache-kafka-auto-create-topics/hdinsight-service-list.png)

1. Sayfanın ortasında Configs'ı seçin.

    ![Apache Ambari hizmet configs sekmesi](./media/apache-kafka-auto-create-topics/hdinsight-service-config.png)

1. Filtre alanına bir değer `auto.create`girin.

    ![Apache Ambari arama filtresi alanı](./media/apache-kafka-auto-create-topics/hdinsight-filter-field.png)

    Bu özellik listesini filtreler ve `auto.create.topics.enable` ayarı görüntüler.

1. 'nin `auto.create.topics.enable` değerini değiştirin ve sonra **Kaydet'i seçin.** `true` Not ekleyin ve sonra yeniden **Kaydet'i** seçin.

    ![Image of the auto.create.topics.enable girişi](./media/apache-kafka-auto-create-topics/auto-create-topics-enable.png)

1. Kafka hizmetini seçin, __Yeniden Başlat'ı__seçin ve etkilenen __tümünü Yeniden Başlat'ı__seçin. İstendiğinde, __tümünü yeniden başlat'ı onayla'yı__seçin.

    ![Apache Ambari tüm etkilenen yeniden başlatın](./media/apache-kafka-auto-create-topics/restart-all-affected.png)

> [!NOTE]  
> Ambari REST API ile Ambari değerlerini de ayarlayabilirsiniz. Geçerli yapılandırmayı almak, değiştirmek, vb. için birden çok REST çağrısı yapmak zorunda olduğunuz için bu genellikle daha zordur. Daha fazla bilgi [için, Apache Ambari REST API belgesini kullanarak HDInsight kümelerini yönet'e](../hdinsight-hadoop-manage-ambari-rest-api.md) bakın.

## <a name="resource-manager-templates"></a>Resource Manager şablonları

Azure Kaynak Yöneticisi şablonu kullanarak Kafka kümesi oluştururken, `auto.create.topics.enable` doğrudan `kafka-broker`bir . Aşağıdaki JSON snippet bu değeri nasıl `true`ayarlayişgösterdiğini gösterir:

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

Bu belgede, HDInsight'ta Apache Kafka için otomatik konu oluşturmayı nasıl etkinleştireceğinizi öğrendiniz. Kafka ile çalışma hakkında daha fazla bilgi edinmek için aşağıdaki bağlantılara bakın:

* [Apache Kafka günlüklerini çözümleme](apache-kafka-log-analytics-operations-management.md)
* [Apache Kafka kümeleri arasındaki verileri çoğaltma](apache-kafka-mirroring.md)
