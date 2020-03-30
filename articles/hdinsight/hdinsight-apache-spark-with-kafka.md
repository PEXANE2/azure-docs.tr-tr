---
title: Apache Kafka ile Apache Spark akışı - Azure HDInsight
description: DStreams kullanarak Apache Kafka'ya veri akışı sağlamak için Apache Spark'ı nasıl kullanacağınızı öğrenin. Bu örnekte, HDInsight üzerinde Spark’tan bir Jupyter not defterini kullanarak verilerinizi akışla aktaracaksınız.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/21/2019
ms.openlocfilehash: d868cdd346c79cf77d4f8c1ea6e4b20adcd99b6c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74327391"
---
# <a name="apache-spark-streaming-dstream-example-with-apache-kafka-on-hdinsight"></a>HDInsight'ta Apache Kafka ile Apache Spark akışı (DStream) örneği

[DStreams'i](https://spark.apache.org/docs/latest/api/java/org/apache/spark/streaming/dstream/DStream.html)kullanarak HDInsight'ta [Apache Kafka'ya](https://kafka.apache.org/) veri akışı sağlamak için [Apache Spark'ı](https://spark.apache.org/) nasıl kullanacağınızı öğrenin. Bu örnek, Kıvılcım kümesinde çalışan bir [Jupyter Notebook](https://jupyter.org/) kullanır.

> [!NOTE]  
> Bu belgede yer alan adımlar hem HDInsight üzerinde Spark hem de HDInsight kümesinde Kafka içeren bir Azure kaynak grubu oluşturur. Bu kümelerin her ikisi de Spark kümesinin Kafka kümesiyle doğrudan iletişim kurmasına olanak tanıyan bir Azure Sanal Ağı içinde bulunur.
>
> Bu belgedeki adımları tamamladığınızda, aşırı ücretlerden kaçınmak için kümeleri silmeyi unutmayın.

> [!IMPORTANT]  
> Bu örnek, eski bir Spark akış teknolojisi olan DStreams'i kullanır. Daha yeni Spark akış özellikleri kullanan bir örnek için, [Apache Kafka belgeli Kıvılcım Yapılandırılmış Akış'a](hdinsight-apache-kafka-spark-structured-streaming.md) bakın.

## <a name="create-the-clusters"></a>Kümeleri oluşturma

HDInsight'taki Apache Kafka, Kafka brokerlarına halka açık internet üzerinden erişim sağlamaz. Kafka ile konuşan her şey, Kafka kümesindeki düğümlerle aynı Azure sanal ağında olmalıdır. Bu örnekte, hem Kafka hem de Kıvılcım kümeleri bir Azure sanal ağında bulunur. Aşağıdaki diyagram, kümeler arasındaki iletişimin nasıl aktığını gösterir:

![Bir Azure sanal ağında Spark ve Kafka kümeleri diyagramı](./media/hdinsight-apache-spark-with-kafka/apache-spark-kafka-vnet.png)

> [!NOTE]  
> Kafka'nın kendisi sanal ağ içindeki iletişimle sınırlı olsa da, SSH ve Ambari gibi kümedeki diğer hizmetlere internet üzerinden erişilebiliyor. HDInsight üzerinde kullanılabilir olan genel bağlantı noktaları hakkında daha fazla bilgi için bkz. [HDInsight Tarafından Kullanılan Bağlantı Noktaları ve URI’ler](hdinsight-hadoop-port-settings-for-services.md).

Bir Azure sanal ağı, Kafka ve Spark kümelerini el ile oluşturabiliyorken, Azure Kaynak Yöneticisi şablonu kullanmak daha kolaydır. Azure sanal ağıKafka ve Spark kümelerini Azure aboneliğinize dağıtmak için aşağıdaki adımları kullanın.

1. Aşağıdaki düğmeyi kullanarak Azure'da oturum açın ve şablonu Azure portalında açın.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v4.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/hdi-deploy-to-azure1.png" alt="Deploy to Azure button for new cluster"></a>

    Azure Kaynak Yöneticisi şablonu **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**' nda yer alır.

    > [!WARNING]  
    > HDInsight üzerinde Kafka'yı kullanabilmeniz için kümenizin en az üç çalışan düğümü içermesi gerekir. Bu şablon, üç çalışan düğümü içeren bir Kafka kümesi oluşturur.

    Bu şablon, hem Kafka hem de Kıvılcım için bir HDInsight 3.6 kümesi oluşturur.

1. **Özel dağıtım** bölümündeki girişleri doldurmak için aşağıdaki bilgileri kullanın:

    |Özellik |Değer |
    |---|---|
    |Kaynak grubu|Bir grup oluşturun veya varolan bir grup seçin.|
    |Konum|Coğrafi olarak size yakın bir konum seçin.|
    |Taban Küme Adı|Bu değer, Kıvılcım ve Kafka kümeleri için temel ad olarak kullanılır. Örneğin, **hdistreaming** girdiğinizde __spark-hdistreaming__ adlı bir Kıvılcım kümesi ve **kafka-hdistreaming**adlı bir Kafka kümesi oluşturur.|
    |Küme Oturum Açma Kullanıcı Adı|Kıvılcım ve Kafka kümelerinin yönetici kullanıcı adı.|
    |Küme Oturum Açma Parolası|Kıvılcım ve Kafka kümeleri için yönetici kullanıcı parolası.|
    |SSH Kullanıcı Adı|SSH kullanıcı Kıvılcım ve Kafka kümeleri için oluşturmak için.|
    |SSH Parolası|Kıvılcım ve Kafka kümeleri için SSH kullanıcısının şifresi.|

    ![HDInsight özel dağıtım parametreleri](./media/hdinsight-apache-spark-with-kafka/hdinsight-parameters.png)

1. **Hüküm ve Koşullar**’ı okuyun ve ardından **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**’u seçin.

1. Son olarak, **Satın Al'ı**seçin. Kümeleri oluşturmak yaklaşık 20 dakika sürer.

Kaynaklar oluşturulduktan sonra bir özet sayfası görüntülenir.

![vnet ve kümeler için kaynak grubu özeti](./media/hdinsight-apache-spark-with-kafka/hdinsight-group-blade.png)

> [!IMPORTANT]  
> HDInsight kümelerinin adlarının, ŞABLONA sağladığınız adıN BASENAME olduğu **kıvılcım-BASENAME** ve **kafka-BASENAME**olduğuna dikkat edin. Kümelere bağlanırken bu adları sonraki adımlarda kullanırsınız.

## <a name="use-the-notebooks"></a>Not defterlerini kullanma

Bu belgede açıklanan örneğin kodu . [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka)

## <a name="delete-the-cluster"></a>Küme silme

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Bu belgedeki adımlar aynı Azure kaynak grubunda her iki küme oluşturduğundan, Azure portalındaki kaynak grubunu silebilirsiniz. Grubu silerken, bu belgeyi, Azure Sanal Ağı'nı ve kümeler tarafından kullanılan depolama hesabını izleyerek oluşturulan tüm kaynakları kaldırır.

## <a name="next-steps"></a>Sonraki adımlar

Bu örnekte, Kafka'ya okuma yazma kıvılcımı kullanmayı öğrendiniz. Kafka ile çalışmanın diğer yollarını keşfetmek için aşağıdaki bağlantıları kullanın:

* [HDInsight'ta Apache Kafka ile başlayın](kafka/apache-kafka-get-started.md)
* [HDInsight'ta Apache Kafka'nın bir kopyasını oluşturmak için MirrorMaker'ı kullanın](kafka/apache-kafka-mirroring.md)
* [Apache Storm'u HdInsight'ta Apache Kafka ile kullanın](hdinsight-apache-storm-with-kafka.md)
