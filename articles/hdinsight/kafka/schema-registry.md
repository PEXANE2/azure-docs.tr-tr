---
title: Azure HDInsight 'ta confluent şema kayıt defteriyle Apache Kafka
description: Bir sanal ağ içinde bir Edge düğümüyle bir HDInsight Managed Kafka kümesi dağıtın ve ardından Gelişmiş şema kayıt defteri 'ni kenar düğümüne yükler.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/18/2020
ms.openlocfilehash: 7e17cdca508db81551d988c795bd1235fa729e82
ms.sourcegitcommit: 295db318df10f20ae4aa71b5b03f7fb6cba15fc3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2020
ms.locfileid: "94636869"
---
# <a name="apache-kafka-with-confluent-schema-registry-in-azure-hdinsight"></a>Azure HDInsight 'ta confluent şema kayıt defteriyle Apache Kafka

Kafka şeması kayıt defteri, ileti şeması depolamayı işleyen Apache Kafka istemcileri ve avro biçiminde gönderilen Kafka iletileri için alımı sağlayan serileştiriciler sağlar. Bu, confby ile bir OSS projesi olmak için kullanılır, ancak şimdi de, bu [topluluk lisansı](https://www.confluent.io/blog/license-changes-confluent-platform/)altında. Şema kayıt defteri Ayrıca aşağıdaki amaçlara hizmet eder:

* Üreticileri ve tüketiciler için şemaları depolayın ve alın.
* Konular üzerinde geri/ileri/Full uyumluluğunu zorunlu tutun.
* Kafka adresine gönderilen yükün boyutunu küçültün.

HDInsight Managed Kafka kümesinde, şema kayıt defteri tipik olarak bir kenar düğümüne dağıtılır ve bu da baş düğümlerden işlem ayrımı sağlar.

Aşağıda, şema kayıt defterinin bir HDInsight kümesine nasıl dağıtıldığına ilişkin temsili bir mimari verilmiştir. Şema kayıt defteri, üzerinde işlemler için bir REST API yerel olarak kullanıma sunar.  Üreticileri ve tüketiciler, sanal ağ içinden veya [Kafka Rest proxy](rest-proxy.md)'Sini kullanarak şema kayıt defteriyle etkileşim kurabilir.

![HDInsight Kafka şema kayıt defteri](./media/schema-registry/pic1.png)

## <a name="deploy-an-hdinsight-managed-kafka-with-confluent-schema-registry"></a>Yapılandırmalı şema kayıt defteri ile HDInsight Managed Kafka dağıtma

Bu bölümde, bir sanal ağ içinde bir Edge düğümü olan bir HDInsight Yönetilen Kafka kümesi dağıyoruz ve ardından Gelişmiş şema kayıt defteri 'ni kenar düğümüne yüklersiniz.  

1. Azure 'da oturum açmak ve Kaynak Yöneticisi şablonunu açmak için aşağıdaki **Azure 'A dağıt** düğmesini seçin.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Farnabganguly%2FKafkaschemaregistry%2Fmaster%2Fazuredeploy.json" target="_blank">:::image type="icon" source="media/schema-registry/hdi-deploy-to-azure1.png":::</a>

1. Özel dağıtım şablonunda, alanları aşağıda açıklandığı gibi doldurun:

    |Özellik |Açıklama |
    |---|---|
    |Abonelik|Aşağı açılan listeden, küme için kullanılan Azure aboneliğini seçin.|
    |Kaynak grubu|Aşağı açılan listeden, mevcut kaynak grubunuzu seçin veya **Yeni oluştur** ' u seçin.|
    |Region|Aşağı açılan listeden, kümenin oluşturulduğu bir bölge seçin.|
    |Küme Adı|Genel olarak benzersiz bir ad girin. Ya da varsayılan adı kullanmak için olduğu gibi bırakın.|
    |Küme Oturum Açma Kullanıcı Adı|Kullanıcı adını belirtin, varsayılan olarak **admin** ' dir.|
    |Küme Oturum Açma Parolası|Parolayı girin.|
    |SSH Kullanıcı adı|Kullanıcı adını belirtin, varsayılan olarak **sshuser**.|
    |SSH parolası|Parolayı girin.|

    Diğer alanları olduğu gibi bırakın. Devam etmek için **gözden geçir + oluştur** ' u seçin.

1. Dağıtım ayrıntılarını gözden geçirin ve ardından dağıtımı başlatmak için **Oluştur** ' u seçin. Dağıtımın tamamlanması 45 dakika sürebilir.

## <a name="configure-the-confluent-schema-registry"></a>Yapılandırma şeması kayıt defterini yapılandırma

Bu bölümde, kenar düğümüne yüklediğimiz confluent Kafka şema kayıt defterini yapılandıracağız. Uygun şema kayıt defteri konumunda bulunur  `/etc/schema-registry/schema-registry.properties` ve hizmet yürütülebilirleri başlatma ve durdurma mekanizmaları  `/usr/bin/` klasöründe bulunur.

Şema yazmacın, Zookeeper hizmetinin HDInsight Kafka kümesiyle etkileşime girebileceklerini bilmeleri gerekir. Zookeeper çekirdeğinin ayrıntılarını almak için aşağıdaki adımları izleyin.

1. Küme kenar düğümüne bağlanmak için [SSH komutunu](../hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME öğesini kümenizin adıyla değiştirerek aşağıdaki komutu düzenleyin ve ardından şu komutu girin:

    ```cmd
    ssh sshuser@schema-registry.CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. Parola değişkenini ayarlayın. PAROLAYı küme oturum açma parolasıyla değiştirin, ardından şu komutu girin:

    ```bash
    export password='PASSWORD'
    ```

1. Doğru şekilde oluşturulmuş küme adını ayıklayın. Aşağıdaki komutu yürütün:

    ```bash
    export clusterName=$(curl -u admin:$password -sS -G "http://headnodehost:8080/api/v1/clusters" | jq -r '.items[].Clusters.cluster_name')
    ```

1. Kafka Zookeeper Konakları ayıklayın. Aşağıdaki komutu yürütün:

    ```bash
    export KAFKAZKHOSTS=$(curl -sS -u admin:$password -G https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2);

    echo $KAFKAZKHOSTS
    ```

    Daha sonra kullanılacak şekilde bu değeri bir yere unutmayın.

1. Kafka Broker Konakları ayıklayın. Aşağıdaki komutu yürütün:

    ```bash
    export KAFKABROKERS=$(curl -sS -u admin:$password -G https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2);

    echo $KAFKABROKERS
    ```

1. Şema kayıt defteri özellikleri dosyalarını düzenleme modunda açın. Aşağıdaki komutu yürütün:

    ```bash
    sudo nano /etc/schema-registry/schema-registry.properties
    ```

    1. Değerini `kafkastore.connection.url` daha önce tanımlanan Zookeeper dizesiyle güncelleştirin.
    1. Değerini `debug` olarak güncelleştirin `true` .

    Artık Özellikler dosyası şuna benzer:  

    ```
    listeners=http://0.0.0.0:8081
    kafkastore.connection.url=zk1-ag4kaf.q2hwzr1xkxjuvobkaagmjjkhta.gx.internal.cloudapp.net:2181,zk2-ag4kaf.q2hwzr1xkxjuvobkaagmjjkhta.gx.internal.cloudapp.net:2181
    kafkastore.topic=_schemas
    debug=true
    ```

1. Dosyayı kaydetmek için **CTRL + X** , **Y** kullanın ve ardından **girin**.

1. Şema kayıt defteri ' ni başlatın ve güncelleştirilmiş şema kayıt defteri özellikleri dosyasını kullanmak için işaretleyin. Aşağıdaki komutları yürütün:

    ```bash
    cd /bin
    sudo schema-registry-start /etc/schema-registry/schema-registry.properties
    ```

1. Tek bir SSH oturumunda çalıştırılan şema kayıt defteri ile başka bir SSH penceresi başlatın.  "Kafka-Key" konusunun altına bir şemanın yeni bir sürümünü kaydedin ve çıktıyı aklınızda yapın:

    ```cmd
    curl -X POST -i -H "Content-Type: application/vnd.schemaregistry.v1+json" \
        --data '{"schema": "{\"type\": \"string\"}"}'
    ```

    ```output
       HTTP/1.1 200 OK
    Date: Sun, 22 Mar 2020 16:33:04 GMT
    Content-Type: application/vnd.schemaregistry.v1+json
    Content-Length: 9
    Server: Jetty(9.2.24.v20180105)
    ```

1. "Kafka-value" konusunun altına bir şemanın yeni bir sürümünü kaydedin ve çıktıyı aklınızda yapın:

    ```cmd
    curl -X POST -i -H "Content-Type: application/vnd.schemaregistry.v1+json" \
        --data '{"schema": "{\"type\": \"string\"}"}' \
    ```

    ```output
    HTTP/1.1 200 OK
    Date: Sun, 22 Mar 2020 16:34:18 GMT
    Content-Type: application/vnd.schemaregistry.v1+json
    Content-Length: 9
    Server: Jetty(9.2.24.v20180105)
    ```

1. Tüm konuları listeleyin ve çıktıyı denetleyin:

    ```cmd
    curl -X GET -i -H "Content-Type: application/vnd.schemaregistry.v1+json" \
        http://localhost:8081/subjects
    ```

    ```output
    HTTP/1.1 200 OK
    Date: Sun, 22 Mar 2020 16:34:39 GMT
    Content-Type: application/vnd.schemaregistry.v1+json
    Content-Length: 27
    Server: Jetty(9.2.24.v20180105)

    ["Kafka-value","Kafka-key"]
    ```

[Burada listelenen başka bazı gelişmiş komutları](https://docs.confluent.io/1.0/schema-registry/docs/intro.html#quickstart)denemek isteyebilirsiniz.

## <a name="send-and-consume-avro-data-from-kafka-using-schema-registry"></a>Şema kayıt defteri kullanarak Kafka 'den avro verilerini gönderin ve kullanın

Bu bölümde, standart girişten gelen verileri okuyacağız ve bir biçimde Kafka konusuna yazacak. Daha sonra bu verileri okunabilir biçime dönüştürmek için bir avro biçimlendiricisine sahip bir tüketici kullanarak Kafka konularındaki verileri okuyacağız.

1. Kafka konu başlığı oluşturun `agkafkaschemareg` .

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic agkafkaschemareg --zookeeper $KAFKAZKHOSTS
    ```

1. Şema oluşturmak, konuya şemayı atamak ve avro biçimindeki konuya veri göndermeye başlamak için **Kafka avro konsol üreticisi** ' ni kullanın. Önceki adımdaki Kafka konusunun başarıyla oluşturulduğundan ve **$KAFKABROKERS** içinde bir değer bulunduğundan emin olun.

    Gönderdiğimiz şema bir anahtar: değer çifti

    ```
    Value
    {
      "type": "record",
      "name": "example_schema",
      "namespace": "com.example",
      "fields": [
        {
          "name": "cust_id",
          "type": "int",
          "doc": "Id of the customer account"
        },
        {
          "name": "year",
          "type": "int",
          "doc": "year of expense"
        },
        {
          "name": "expenses",
          "type": {
            "type": "array",
            "items": "float"
          },
          "doc": "Expenses for the year"
        }
      ],
      "doc:": "A basic schema for storing messages"
    }
    ```

    **Kafka avro konsol üreticisi** 'ni başlatmak için aşağıdaki komutu kullanın:

    ```bash
    /usr/bin/kafka-avro-console-producer     --broker-list $KAFKABROKERS     --topic agkafkaschemareg     --property parse.key=true --property key.schema='{"type" : "int", "name" : "id"}'     --property value.schema='{ "type" : "record", "name" : "example_schema", "namespace" : "com.example", "fields" : [ { "name" : "cust_id", "type" : "int", "doc" : "Id of the customer account" }, { "name" : "year", "type" : "int", "doc" : "year of expense" }, { "name" : "expenses", "type" : {"type": "array", "items": "float"}, "doc" : "Expenses for the year" } ], "doc:" : "A basic schema for storing messages" }'
    ```

1. Üretici iletileri kabul etmeye hazırsa, iletileri önceden tanımlanmış avro şeması biçiminde göndermeye başlayın. Anahtar ve değer arasında boşluk oluşturmak için Tab tuşunu kullanın.

    ```
    1 TAB {"cust_id":1313131, "year":2012, "expenses":[1313.13, 2424.24]}
    2 TAB {"cust_id":3535353, "year":2011, "expenses":[761.35, 92.18, 14.41]}
    3 TAB {"cust_id":7979797, "year":2011, "expenses":[4489.00]}
    ```

1. Üreticinin artık önceden tanımlanmış avro şemasıyla uyumlu olmayan verilere nasıl izin vermediğini görmek için, konsol üreticisi 'ne rastgele şema olmayan veriler girmeyi deneyin.

    ```
    1       {"cust_id":1313131, "year":2012, "expenses":[1313.13, 2424.24]}
    2       {"cust_id":1313131,"cust_age":34 "year":2012, "expenses":[1313.13, 2424.24,34.212]}
    org.apache.kafka.common.errors.SerializationException: Error deserializing json {"cust_id":1313131,"cust_age":34 "year":2012, "expenses":[1313.13, 2424.24,34.212]} to Avro of schema {"type":"record","name":"example_schema","namespace":"com.example","fields":[{"name":"cust_id","type":"int","doc":"Id of the customer account"},{"name":"year","type":"int","doc":"year of expense"},{"name":"expenses","type":{"type":"array","items":"float"},"doc":"Expenses for the year"}],"doc:":"A basic schema for storing messages"}
    Caused by: org.codehaus.jackson.JsonParseException: Unexpected character ('"' (code 34)): was expecting comma to separate OBJECT entries
     at [Source: java.io.StringReader@3561c410; line: 1, column: 35]
            at org.codehaus.jackson.JsonParser._constructError(JsonParser.java:1433)
            at org.codehaus.jackson.impl.JsonParserMinimalBase._reportError(JsonParserMinimalBase.java:521)
            at org.codehaus.jackson.impl.JsonParserMinimalBase._reportUnexpectedChar(JsonParserMinimalBase.java:442)
            at org.codehaus.jackson.impl.ReaderBasedParser.nextToken(ReaderBasedParser.java:406)
            at org.apache.avro.io.JsonDecoder.getVaueAsTree(JsonDecoder.java:549)
            at org.apache.avro.io.JsonDecoder.doAction(JsonDecoder.java:474)
            at org.apache.avro.io.parsing.Parser.advance(Parser.java:88)
            at org.apache.avro.io.JsonDecoder.advance(JsonDecoder.java:139)
            at org.apache.avro.io.JsonDecoder.readInt(JsonDecoder.java:166)
            at org.apache.avro.io.ValidatingDecoder.readInt(ValidatingDecoder.java:83)
            at org.apache.avro.generic.GenericDatumReader.readInt(GenericDatumReader.java:511)
            at org.apache.avro.generic.GenericDatumReader.readWithoutConversion(GenericDatumReader.java:182)
            at org.apache.avro.generic.GenericDatumReader.read(GenericDatumReader.java:152)
            at org.apache.avro.generic.GenericDatumReader.readField(GenericDatumReader.java:240)
            at org.apache.avro.generic.GenericDatumReader.readRecord(GenericDatumReader.java:230)
            at org.apache.avro.generic.GenericDatumReader.readWithoutConversion(GenericDatumReader.java:174)
            at org.apache.avro.generic.GenericDatumReader.read(GenericDatumReader.java:152)
            at org.apache.avro.generic.GenericDatumReader.read(GenericDatumReader.java:144)
            at io.confluent.kafka.formatter.AvroMessageReader.jsonToAvro(AvroMessageReader.java:213)
            at io.confluent.kafka.formatter.AvroMessageReader.readMessage(AvroMessageReader.java:200)
            at kafka.tools.ConsoleProducer$.main(ConsoleProducer.scala:59)
            at kafka.tools.ConsoleProducer.main(ConsoleProducer.scala)
    
    ```

1. Farklı bir ekranda, Kafka avro konsol tüketicisini başlatın

    ```bash
    sudo /usr/bin/kafka-avro-console-consumer --bootstrap-server $KAFKABROKERS --topic agkafkaschemareg --from-beginning
    ```

    Aşağıdaki çıktıyı görmeye başlamanız gerekir:

    ```output
    {"cust_id":1313131,"year":2012,"expenses":[1313.13,2424.24]}
    {"cust_id":7979797,"year":2011,"expenses":[4489.0]}
    {"cust_id":3535353,"year":2011,"expenses":[761.35,92.18,14.41]}
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Kafka Producer ve tüketici API 'Lerini kullanma](apache-kafka-producer-consumer-api.md)