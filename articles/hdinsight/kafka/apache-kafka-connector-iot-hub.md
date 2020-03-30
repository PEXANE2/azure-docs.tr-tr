---
title: Azure IoT Hub ile HDInsight'ta Apache Kafka'yı kullanma
description: Azure IoT Hub ile HDInsight'ta Apache Kafka'yı nasıl kullanacağınızı öğrenin. Kafka Connect Azure IoT Hub projesi Kafka için bir kaynak ve lavabo konektörü sağlar. Kaynak bağlayıcısı IoT Hub'dan gelen verileri okuyabilir ve lavabo konektörü IoT Hub'a yazar.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/26/2019
ms.openlocfilehash: 48a72b5ba3819712b9e1d2536ae2dd3a06eaf3f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80238812"
---
# <a name="use-apache-kafka-on-hdinsight-with-azure-iot-hub"></a>Azure IoT Hub ile HDInsight'ta Apache Kafka'yı kullanma

HDInsight'ta Apache Kafka ile Azure IoT Hub arasında veri taşımak için [Apache Kafka Connect Azure IoT Hub](https://github.com/Azure/toketi-kafka-connect-iothub) bağlayıcısını nasıl kullanacağınızı öğrenin. Bu belgede, IoT Hub bağlayıcısını kümedeki bir kenar düğümünden çalıştırmayı öğrenirsiniz.

Kafka Connect API, Kafka'ya sürekli veri çeken veya Kafka'dan gelen verileri başka bir sisteme iten konektörler uygulamanızı sağlar. [Apache Kafka Connect Azure IoT Hub,](https://github.com/Azure/toketi-kafka-connect-iothub) Azure IoT Hub'ından Kafka'ya veri çeken bir bağlayıcıdır. Ayrıca Kafka'dan IoT Hub'ına veri itebilir.

IoT Hub'ından çekerken bir __kaynak__ bağlayıcısı kullanırsınız. IoT Hub'a iterken __lavabo__ konektörü kullanırsınız. IoT Hub konektörü hem kaynak hem de lavabo konektörlerini sağlar.

Aşağıdaki diyagram, bağlayıcıyı kullanırken HDInsight'ta Azure IoT Hub ile Kafka arasındaki veri akışını gösterir.

![Bağlayıcı aracılığıyla IoT Hub'dan Kafka'ya akan verileri gösteren görüntü](./media/apache-kafka-connector-iot-hub/iot-hub-kafka-connector-hdinsight.png)

Connect API hakkında daha fazla [https://kafka.apache.org/documentation/#connect](https://kafka.apache.org/documentation/#connect)bilgi için bkz.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight'ta bir Apache Kafka kümesi. Daha fazla bilgi için [HDInsight üzerinde Kafka hızlı başlangıcı](apache-kafka-get-started.md) belgesine bakın.

* Kafka kümesinde bir kenar düğümü. Daha fazla bilgi için [HDInsight belgesiyle Kullanım kenarı düğümlerine](../hdinsight-apps-use-edge-node.md) bakın.

* Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](../hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

* Azure IoT Hub'ı ve aygıtı. Bu makale için, [Azure IoT Hub'a Connect Raspberry Pi çevrimiçi simülatörünü](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-web-simulator-get-started)kullanmayı düşünün.

* [Scala inşa aracı](https://www.scala-sbt.org/).

## <a name="build-the-connector"></a>Konektörü oluşturun

1. Bağlayıcının [https://github.com/Azure/toketi-kafka-connect-iothub/](https://github.com/Azure/toketi-kafka-connect-iothub/) kaynağını yerel ortamınıza indirin.

2. Komut isteminden dizine `toketi-kafka-connect-iothub-master` gidin. Ardından projeyi oluşturmak ve paketlemek için aşağıdaki komutu kullanın:

    ```cmd
    sbt assembly
    ```

    Yapının tamamlanması birkaç dakika sürer. Komut, proje dizininde `kafka-connect-iothub-assembly_2.11-0.7.0.jar` `toketi-kafka-connect-iothub-master\target\scala-2.11` adı geçen bir dosya oluşturur.

## <a name="install-the-connector"></a>Konektörü yükleme

1. .jar dosyasını HDInsight kümesinde Kafka'nızın kenar düğümüne yükleyin. Kümenizin gerçek adını `CLUSTERNAME` değiştirerek aşağıdaki komutu düzenleme. SSH kullanıcı hesabı ve [kenar düğümü](../hdinsight-apps-use-edge-node.md#access-an-edge-node) adı için varsayılan değerler aşağıda kullanılır, gerektiği gibi değiştirin.

    ```cmd
    scp kafka-connect-iothub-assembly*.jar sshuser@new-edgenode.CLUSTERNAME-ssh.azurehdinsight.net:
    ```

1. Dosya kopyası tamamlandıktan sonra, SSH kullanarak kenar düğümüne bağlanın:

    ```bash
    ssh sshuser@new-edgenode.CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Konektörü Kafka `libs` dizinine yüklemek için aşağıdaki komutu kullanın:

    ```bash
    sudo mv kafka-connect-iothub-assembly*.jar /usr/hdp/current/kafka-broker/libs/
    ```

Kalan adımlar için SSH bağlantınızı etkin tutun.

## <a name="configure-apache-kafka"></a>Apache Kafka'yı yapılandır

SSH bağlantınızdan kenar düğümüne kadar, kafka'yı konektörü bağımsız modda çalıştırmak üzere yapılandırmak için aşağıdaki adımları kullanın:

1. Parola değişkenini ayarlayın. Parola'yı küme giriş parolasıyla değiştirin ve ardından komutu girin:

    ```bash
    export password='PASSWORD'
    ```

1. [jq](https://stedolan.github.io/jq/) yardımcı programını yükleyin. jq, Ambari sorgularından döndürülen JSON belgelerinin işlenmesini kolaylaştırır. Aşağıdaki komutu girin:

    ```bash
    sudo apt -y install jq
    ```

1. Kafka komisyoncularının adresini al. Kümenizde birçok aracı olabilir, ancak yalnızca bir veya iki başvuru yapmanız gerekir. İki broker ana bilgisayarının adresini almak için aşağıdaki komutu kullanın:

    ```bash
    export clusterName=$(curl -u admin:$password -sS -G "http://headnodehost:8080/api/v1/clusters" | jq -r '.items[].Clusters.cluster_name')

    export KAFKABROKERS=`curl -sS -u admin:$password -G http://headnodehost:8080/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    echo $KAFKABROKERS
    ```

    Daha sonra kullanmak için değerleri kopyalayın. Döndürülen değer aşağıdaki metne benzer:

    `wn0-kafka.w5ijyohcxt5uvdhhuaz5ra4u5f.ex.internal.cloudapp.net:9092,wn1-kafka.w5ijyohcxt5uvdhhuaz5ra4u5f.ex.internal.cloudapp.net:9092`

1. Apache Zookeeper düğümlerinin adresini alın. Kümede birkaç Zookeeper düğümü vardır, ancak yalnızca bir veya iki başvuru yapmanız gerekir. Değişkendeki `KAFKAZKHOSTS`adresleri depolamak için aşağıdaki komutu kullanın:

    ```bash
    export KAFKAZKHOSTS=`curl -sS -u admin:$password -G http://headnodehost:8080/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    ```

1. Konektörü bağımsız modda çalıştırırken, `/usr/hdp/current/kafka-broker/config/connect-standalone.properties` dosya Kafka brokerları ile iletişim kurmak için kullanılır. Dosyayı `connect-standalone.properties` yeniden kullanmak için aşağıdaki komutu kullanın:

    ```bash
    sudo nano /usr/hdp/current/kafka-broker/config/connect-standalone.properties
    ```

1. Aşağıdaki leri yapın:

    |Geçerli değer |Yeni değer | Açıklama |
    |---|---|---|
    |`bootstrap.servers=localhost:9092`|`localhost:9092` Değeri önceki adımdaki broker ana bilgisayarlarıyla değiştirme|Kafka brokerlarını bulmak için kenar düğümüiçin bağımsız yapılandırmayı yapılandırır.|
    |`key.converter=org.apache.kafka.connect.json.JsonConverter`|`key.converter=org.apache.kafka.connect.storage.StringConverter`|Bu değişiklik, Kafka ile birlikte verilen konsol üreticisini kullanarak test etmenizi sağlar. Diğer üreticiler ve tüketiciler için farklı dönüştürücüler gerekebilir. Diğer dönüştürücü değerlerini kullanma hakkında [https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md)bilgi için bkz.|
    |`value.converter=org.apache.kafka.connect.json.JsonConverter`|`value.converter=org.apache.kafka.connect.storage.StringConverter`|Yukarıdakiyle aynı.|
    |Yok|`consumer.max.poll.records=10`|Dosyanın sonuna ekleyin. Bu değişiklik, lavabo konektöründeki zaman aşamalarını bir seferde 10 kayıtla sınırlandırarak önlemek tir. Daha fazla bilgi [https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md)için bkz.|

1. Dosyayı kaydetmek için __Ctrl + X__, __Y__ve ardından __Enter__'u kullanın.

1. Bağlayıcı tarafından kullanılan konuları oluşturmak için aşağıdaki komutları kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic iotin --zookeeper $KAFKAZKHOSTS

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic iotout --zookeeper $KAFKAZKHOSTS
    ```

    Konuların `iotin` ve `iotout` konuların var olduğunu doğrulamak için aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
    ```

    Konu, `iotin` IoT Hub'dan ileti almak için kullanılır. Konu, IoT Hub'a `iotout` ileti göndermek için kullanılır.

## <a name="get-iot-hub-connection-information"></a>IoT Hub bağlantı bilgilerini alın

Bağlayıcı tarafından kullanılan IoT hub bilgilerini almak için aşağıdaki adımları kullanın:

1. IoT hub'ınız için Event Hub uyumlu uç nokta ve Event Hub uyumlu uç nokta adını alın. Bu bilgileri almak için aşağıdaki yöntemlerden birini kullanın:

   * __Azure [portalından](https://portal.azure.com/)__ aşağıdaki adımları kullanın:

     1. IoT Hub'ınıza gidin ve __Uç Noktaları__seçin.
     2. __Yerleşik uç noktalardan,__ __Etkinlikler'i__seçin.
     3. __Özellikler'den,__ aşağıdaki alanların değerini kopyalayın:

         * __Olay Hub uyumlu ad__
         * __Olay Hub uyumlu bitiş noktası__
         * __Bölüm__

        > [!IMPORTANT]  
        > Portaldan gelen uç nokta değeri, bu örnekte gerekli olmayan ek metin içerebilir. Bu desenle `sb://<randomnamespace>.servicebus.windows.net/`eşleşen metni ayıklayın.

   * __Azure [CLI'den](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)__ aşağıdaki komutu kullanın:

       ```azurecli
       az iot hub show --name myhubname --query "{EventHubCompatibleName:properties.eventHubEndpoints.events.path,EventHubCompatibleEndpoint:properties.eventHubEndpoints.events.endpoint,Partitions:properties.eventHubEndpoints.events.partitionCount}"
       ```

       IoT hub'ınızın adıyla değiştirin. `myhubname` Yanıt aşağıdaki metne benzer:

       ```json
       "EventHubCompatibleEndpoint": "sb://ihsuprodbnres006dednamespace.servicebus.windows.net/",
       "EventHubCompatibleName": "iothub-ehub-myhub08-207673-d44b2a856e",
       "Partitions": 2
       ```

2. Paylaşılan __erişim ilkesini__ ve __anahtarı__alın. Bu örnekte, __hizmet__ anahtarını kullanın. Bu bilgileri almak için aşağıdaki yöntemlerden birini kullanın:

    * __Azure [portalından](https://portal.azure.com/)__ aşağıdaki adımları kullanın:

        1. __Paylaşılan erişim ilkelerini__seçin ve ardından __hizmet__seçin.
        2. Birincil __anahtar__ değerini kopyalayın.
        3. Bağlantı __dizesini kopyalayın-- birincil anahtar__ değeri.

    * __Azure [CLI'den](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli)__ aşağıdaki komutu kullanın:

        1. Birincil anahtar değerini almak için aşağıdaki komutu kullanın:

            ```azurecli
            az iot hub policy show --hub-name myhubname --name service --query "primaryKey"
            ```

            IoT hub'ınızın adıyla değiştirin. `myhubname` Yanıt, bu hub için `service` ilke için birincil anahtardır.

        2. İlkenin bağlantı dizesini `service` almak için aşağıdaki komutu kullanın:

            ```azurecli
            az iot hub show-connection-string --name myhubname --policy-name service --query "connectionString"
            ```

            IoT hub'ınızın adıyla değiştirin. `myhubname` Yanıt, ilkenin bağlantı `service` dizesidir.

## <a name="configure-the-source-connection"></a>Kaynak bağlantısını yapılandırma

Kaynağı IoT Hub'ınızla çalışacak şekilde yapılandırmak için, ssh bağlantısından kenar düğümüne aşağıdaki eylemleri gerçekleştirin:

1. `/usr/hdp/current/kafka-broker/config/` Dizinde dosyanın `connect-iot-source.properties` bir kopyasını oluşturun. Dosyayı toketi-kafka-connect-iothub projesinden indirmek için aşağıdaki komutu kullanın:

    ```bash
    sudo wget -P /usr/hdp/current/kafka-broker/config/ https://raw.githubusercontent.com/Azure/toketi-kafka-connect-iothub/master/connect-iothub-source.properties
    ```

1. Dosyayı `connect-iot-source.properties` ve IoT hub bilgilerini eklemek için aşağıdaki komutu kullanın:

    ```bash
    sudo nano /usr/hdp/current/kafka-broker/config/connect-iothub-source.properties
    ```

1. Düzenleyicide, aşağıdaki girişleri bulun ve değiştirin:

    |Geçerli değer |Düzenle|
    |---|---|
    |`Kafka.Topic=PLACEHOLDER`|`PLACEHOLDER` yerine `iotin` yazın. IoT hub'ından alınan iletiler `iotin` konuya yerleştirilir.|
    |`IotHub.EventHubCompatibleName=PLACEHOLDER`|Olay `PLACEHOLDER` Hub uyumlu adla değiştirin.|
    |`IotHub.EventHubCompatibleEndpoint=PLACEHOLDER`|Olay `PLACEHOLDER` Hub uyumlu bitiş noktası ile değiştirin.|
    |`IotHub.AccessKeyName=PLACEHOLDER`|`PLACEHOLDER` yerine `service` yazın.|
    |`IotHub.AccessKeyValue=PLACEHOLDER`|İlkenin birincil anahtarıyla değiştirin. `PLACEHOLDER` `service`|
    |`IotHub.Partitions=PLACEHOLDER`|Önceki `PLACEHOLDER` adımlardaki bölüm sayısıyla değiştirin.|
    |`IotHub.StartTime=PLACEHOLDER`|UTC tarihiyle değiştirin. `PLACEHOLDER` Bu tarih, bağlayıcının iletileri denetlemeye başladığı tarihtir. Tarih `yyyy-mm-ddThh:mm:ssZ`biçimi.|
    |`BatchSize=100`|`100` yerine `5` yazın. Bu değişiklik, IoT hub'ında beş yeni ileti olduğunda bağlayıcının Kafka'ya ileti okumasına neden olur.|

    Örnek bir yapılandırma için Azure [IoT Hub için Kafka Connect Kaynak Bağlayıcı](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Source.md)Bağlayıcısı'na bakın.

1. Değişiklikleri kaydetmek için __Ctrl + X__, __Y__ve ardından __Enter__' ı kullanın.

Bağlayıcı kaynağını yapılandırma hakkında daha fazla [https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Source.md](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Source.md)bilgi için bkz.

## <a name="configure-the-sink-connection"></a>Lavabo bağlantısını yapılandırma

Lavabo bağlantısını IoT Hub'ınızla çalışacak şekilde yapılandırmak için, ssh bağlantısından kenar düğümüne aşağıdaki eylemleri gerçekleştirin:

1. `/usr/hdp/current/kafka-broker/config/` Dizinde dosyanın `connect-iothub-sink.properties` bir kopyasını oluşturun. Dosyayı toketi-kafka-connect-iothub projesinden indirmek için aşağıdaki komutu kullanın:

    ```bash
    sudo wget -P /usr/hdp/current/kafka-broker/config/ https://raw.githubusercontent.com/Azure/toketi-kafka-connect-iothub/master/connect-iothub-sink.properties
    ```

1. Dosyayı `connect-iothub-sink.properties` ve IoT hub bilgilerini eklemek için aşağıdaki komutu kullanın:

    ```bash
    sudo nano /usr/hdp/current/kafka-broker/config/connect-iothub-sink.properties
    ```

1. Düzenleyicide, aşağıdaki girişleri bulun ve değiştirin:

    |Geçerli değer |Düzenle|
    |---|---|
    |`topics=PLACEHOLDER`|`PLACEHOLDER` yerine `iotout` yazın. `iotout` Konuya yazılan iletiler IoT hub'ına iletilir.|
    |`IotHub.ConnectionString=PLACEHOLDER`|İlke için bağlantı `service` dizesiyle değiştirin. `PLACEHOLDER`|

    Örnek bir yapılandırma için Azure [IoT Hub için Kafka Connect Büzerek Bağlayıcısı'na](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md)bakın.

1. Değişiklikleri kaydetmek için __Ctrl + X__, __Y__ve ardından __Enter__' ı kullanın.

Konektör lavabosu yapılandırma hakkında daha [https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md)fazla bilgi için bkz.

## <a name="start-the-source-connector"></a>Kaynak bağlayıcısını başlatın

1. Kaynak bağlayıcıyı başlatmak için, ssh bağlantısından kenar düğümüne aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/connect-standalone.sh /usr/hdp/current/kafka-broker/config/connect-standalone.properties /usr/hdp/current/kafka-broker/config/connect-iothub-source.properties
    ```

    Konektör başladıktan sonra, cihazınızdan IoT hub'ına ileti gönderin. Bağlayıcı IoT hub'ından gelen iletileri okur ve Kafka konusuna saklarken, bilgileri konsola kaydeder:

    ```output
    [2017-08-29 20:15:46,112] INFO Polling for data - Obtained 5 SourceRecords from IotHub (com.microsoft.azure.iot.kafka.connect.IotHubSourceTask:39)
    [2017-08-29 20:15:54,106] INFO Finished WorkerSourceTask{id=AzureIotHubConnector-0} commitOffsets successfully in 4 ms (org.apache.kafka.connect.runtime.WorkerSourceTask:356)
    ```

    > [!NOTE]  
    > Bağlayıcı çalışırken çeşitli uyarılar görebilirsiniz. Bu uyarılar, IoT hub'ından ileti alma da sorunlara neden olmaz.

1. **Ctrl + C** kullanarak birkaç dakika sonra konektörü iki kez durdurun. Konektörün durması birkaç dakika sürer.

## <a name="start-the-sink-connector"></a>Lavabo konektörünü başlatın

SSH bağlantısından kenar düğümüne kadar, lavabo konektörünü bağımsız modda başlatmak için aşağıdaki komutu kullanın:

```bash
/usr/hdp/current/kafka-broker/bin/connect-standalone.sh /usr/hdp/current/kafka-broker/config/connect-standalone.properties /usr/hdp/current/kafka-broker/config/connect-iothub-sink.properties
```

Bağlayıcı çalışırken, aşağıdaki metne benzer bilgiler görüntülenir:

```output
[2017-08-30 17:49:16,150] INFO Started tasks to send 1 messages to devices. (com.microsoft.azure.iot.kafka.connect.sink.
IotHubSinkTask:47)
[2017-08-30 17:49:16,150] INFO WorkerSinkTask{id=AzureIotHubSinkConnector-0} Committing offsets (org.apache.kafka.connect.runtime.WorkerSinkTask:262)
```

> [!NOTE]  
> Konektör çalışırken çeşitli uyarılar fark edebilirsiniz. Bunları güvenle yok sayabilirsiniz.

## <a name="send-messages"></a>İleti gönderme

Bağlayıcı üzerinden ileti göndermek için aşağıdaki adımları kullanın:

1. Kafka kümesine *ikinci bir* SSH oturumu açın:

    ```bash
    ssh sshuser@new-edgenode.CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Yeni ssh oturumu için Kafka brokerlarının adresini alın. Parola'yı küme giriş parolasıyla değiştirin ve ardından komutu girin:

    ```bash
    export password='PASSWORD'

    export clusterName=$(curl -u admin:$password -sS -G "http://headnodehost:8080/api/v1/clusters" | jq -r '.items[].Clusters.cluster_name')

    export KAFKABROKERS=`curl -sS -u admin:$password -G http://headnodehost:8080/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    ```

1. `iotout` Konuya ileti göndermek için aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic iotout
    ```

    Bu komut sizi normal Bash istemine döndürmez. Bunun yerine, `iotout` konuya klavye girişi gönderir.

1. Cihazınıza ileti göndermek için, bir JSON belgesini SSH `kafka-console-producer`oturumuna yapıştırın.

    > [!IMPORTANT]  
    > Cihazınızın `"deviceId"` kimliğine giriş değerini ayarlamanız gerekir. Aşağıdaki örnekte, aygıt adlandırılmış: `myDeviceId`

    ```json
    {"messageId":"msg1","message":"Turn On","deviceId":"myDeviceId"}
    ```

    Bu JSON belge için şema daha ayrıntılı [https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md)olarak açıklanmıştır .

    Simüle edilmiş Raspberry Pi aygıtını kullanıyorsanız ve çalışıyorsa, aşağıdaki ileti aygıt tarafından günlüğe kaydedilir:

    ```output
    Receive message: Turn On
    ```

    JSON belgesini yeniden gönderin, ancak `"message"` girişin değerini değiştirin. Yeni değer aygıt tarafından günlüğe kaydedilir.

Lavabo konektörünü kullanma hakkında [https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md](https://github.com/Azure/toketi-kafka-connect-iothub/blob/master/README_Sink.md)daha fazla bilgi için bkz.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, HDInsight'ta IoT Kafka Konektörü'ni başlatmak için Apache Kafka Connect API'yi nasıl kullanacağınızı öğrendiniz. Kafka ile çalışmanın diğer yollarını keşfetmek için aşağıdaki bağlantıları kullanın:

* [Apache Spark'ı Apache Kafka ile HDInsight'ta kullanın](../hdinsight-apache-spark-with-kafka.md)
* [Apache Storm'u HdInsight'ta Apache Kafka ile kullanın](../hdinsight-apache-storm-with-kafka.md)
