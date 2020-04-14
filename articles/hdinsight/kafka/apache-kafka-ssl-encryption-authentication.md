---
title: Apache Kafka TLS şifreleme & kimlik doğrulama - Azure HDInsight
description: Kafka müşterileri ile Kafka brokerları arasında ve Kafka brokerları arasındaki iletişim için TLS şifrelemesi ayarlayın. İstemcilerin SSL kimlik doğrulamasını ayarlayın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 05/01/2019
ms.openlocfilehash: 02b64d77a4fb1af25e1022de3ac8e4775f916d9e
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81261780"
---
# <a name="set-up-tls-encryption-and-authentication-for-apache-kafka-in-azure-hdinsight"></a>Azure HDInsight'ta Apache Kafka için TLS şifreleme ve kimlik doğrulaması ayarlama

Bu makalede, Apache Kafka istemcileri ve Apache Kafka brokerları arasında daha önce Güvenli Soketkatmanı (SSL) şifrelemesi olarak bilinen Aktarım Katmanı Güvenliği (TLS) şifrelemesi nasıl ayarlayabileceğinizgösterilmektedir. Ayrıca, istemcilerin kimlik doğrulamasını nasıl ayarlayabileceğinizi de gösterir (bazen iki yönlü TLS olarak da adlandırılır).

> [!Important]
> Kafka uygulamaları için kullanabileceğiniz iki istemci vardır: Bir Java istemcisi ve bir konsol istemcisi. TLS'yi `ProducerConsumer.java` hem üretmek hem de tüketmek için yalnızca Java istemcisi kullanabilir. Konsol üreticisi `console-producer.sh` istemci TLS ile çalışmıyor.

> [!Note]
> SÜRÜM 1.1 ile HDInsight Kafka konsol üreticisi SSL desteklemiyor.

## <a name="apache-kafka-broker-setup"></a>Apache Kafka broker kurulumu

Kafka TLS broker kurulumu dört HDInsight kümesi VM'yi aşağıdaki şekilde kullanır:

* headnode 0 - Sertifika Yetkilisi (CA)
* işçi düğümü 0, 1 ve 2 - broker

> [!Note] 
> Bu kılavuz kendi imzalı sertifikaları kullanır, ancak en güvenli çözüm güvenilir CA'lar tarafından verilen sertifikaları kullanmaktır.

Broker kurulum sürecinin özeti aşağıdaki gibidir:

1. Üç alt düğümün her birinde aşağıdaki adımlar yinelenir:

    1. Bir sertifika oluşturun.
    1. Sertifika imzalama isteği oluşturun.
    1. Sertifika imzalama isteğini Sertifika Yetkilisi'ne (CA) gönderin.
    1. CA'da oturum açın ve isteği imzalayın.
    1. SCP imzalı sertifikayı işçi düğümüne geri döndürür.
    1. SCP ca kamu sertifikası işçi düğümü için.

1. Tüm sertifikaları aldıktan sonra, sertifikaları sertifika deposuna koyun.
1. Ambari'ye gidin ve yapılandırmaları değiştirin.

Broker kurulumuna son vermek için aşağıdaki ayrıntılı yönergeleri kullanın:

> [!Important]
> Aşağıdaki kod parçacıklarıwnX üç alt düğümlerinden biri için bir kısaltma ve değiştirilmelidir `wn0`, `wn1` ya `wn2` da uygun. `WorkerNode0_Name`ve `HeadNode0_Name` ilgili makinelerin isimleri ile değiştirilmelidir.

1. HDInsight için Sertifika Yetkilisi (CA) rolünü dolduracak kafa düğümü 0 üzerinde ilk kurulumu gerçekleştirin.

    ```bash
    # Create a new directory 'ssl' and change into it
    mkdir ssl
    cd ssl
    ```

1. Brokerlerin her birinde aynı ilk kurulumu gerçekleştirin (işçi düğümleri 0, 1 ve 2).

    ```bash
    # Create a new directory 'ssl' and change into it
    mkdir ssl
    cd ssl
    ```

1. Alt düğümlerin her birinde, aşağıdaki kod parçacıklarını kullanarak aşağıdaki adımları uygulayın.
    1. Bir anahtar mağazası oluşturun ve yeni bir özel sertifikayla doldurun.
    1. Sertifika imzalama isteği oluşturun.
    1. SCP CA (headnode0) için sertifika imzalama isteği

    ```bash
    keytool -genkey -keystore kafka.server.keystore.jks -validity 365 -storepass "MyServerPassword123" -keypass "MyServerPassword123" -dname "CN=FQDN_WORKER_NODE" -storetype pkcs12
    keytool -keystore kafka.server.keystore.jks -certreq -file cert-file -storepass "MyServerPassword123" -keypass "MyServerPassword123"
    scp cert-file sshuser@HeadNode0_Name:~/ssl/wnX-cert-sign-request
    ```

1. CA makinesinde ca-cert ve ca-key dosyaları oluşturmak için aşağıdaki komutu çalıştırın:

    ```bash
    openssl req -new -newkey rsa:4096 -days 365 -x509 -subj "/CN=Kafka-Security-CA" -keyout ca-key -out ca-cert -nodes
    ```

1. CA makinesini değiştirin ve alınan tüm sertifika imzalama isteklerini imzalayın:

    ```bash
    openssl x509 -req -CA ca-cert -CAkey ca-key -in wn0-cert-sign-request -out wn0-cert-signed -days 365 -CAcreateserial -passin pass:"MyServerPassword123"
    openssl x509 -req -CA ca-cert -CAkey ca-key -in wn1-cert-sign-request -out wn1-cert-signed -days 365 -CAcreateserial -passin pass:"MyServerPassword123"
    openssl x509 -req -CA ca-cert -CAkey ca-key -in wn2-cert-sign-request -out wn2-cert-signed -days 365 -CAcreateserial -passin pass:"MyServerPassword123"
    ```

1. İmzalı sertifikaları CA'dan (headnode0) çalışan düğümlere geri gönderin.

    ```bash
    scp wn0-cert-signed sshuser@WorkerNode0_Name:~/ssl/cert-signed
    scp wn1-cert-signed sshuser@WorkerNode1_Name:~/ssl/cert-signed
    scp wn2-cert-signed sshuser@WorkerNode2_Name:~/ssl/cert-signed
    ```

1. CA'nın ortak sertifikasını her işçi düğümüne gönderin.

    ```bash
    scp ca-cert sshuser@WorkerNode0_Name:~/ssl/ca-cert
    scp ca-cert sshuser@WorkerNode1_Name:~/ssl/ca-cert
    scp ca-cert sshuser@WorkerNode2_Name:~/ssl/ca-cert
    ```

1. Her işçi düğümünde, kasa ve keystore'a CAs genel sertifikasını ekleyin. Ardından, işçi düğümünün kendi imzalı sertifikasını anahtar mağazasına ekleyin

    ```bash
    keytool -keystore kafka.server.truststore.jks -alias CARoot -import -file ca-cert -storepass "MyServerPassword123" -keypass "MyServerPassword123" -noprompt
    keytool -keystore kafka.server.keystore.jks -alias CARoot -import -file ca-cert -storepass "MyServerPassword123" -keypass "MyServerPassword123" -noprompt
    keytool -keystore kafka.server.keystore.jks -import -file cert-signed -storepass "MyServerPassword123" -keypass "MyServerPassword123" -noprompt

    ```

## <a name="update-kafka-configuration-to-use-tls-and-restart-brokers"></a>TLS'yi kullanmak ve brokerları yeniden başlatmak için Kafka yapılandırmasını güncelleştirin

Şimdi bir keystore ve truststore ile her Kafka broker kurduk ve doğru sertifikaları ithal. Ardından, Ambari'yi kullanarak ilgili Kafka yapılandırma özelliklerini değiştirin ve Kafka brokerlarını yeniden başlatın.

Yapılandırma değişikliğini tamamlamak için aşağıdaki adımları yapın:

1. Azure portalında oturum açın ve Azure HDInsight Apache Kafka kümenizi seçin.
1. **Küme panoları**altında **Ambari ev** tıklayarak Ambari UI gidin.
1. **Kafka Broker** altında **dinleyici** özelliği ayarlamak`PLAINTEXT://localhost:9092,SSL://localhost:9093`
1. **Gelişmiş kafka-broker** altında **security.inter.broker.protocol** özelliği ayarlamak`SSL`

    ![Ambari'de Kafka ssl yapılandırma özelliklerinin düzenlenmesi](./media/apache-kafka-ssl-encryption-authentication/editing-configuration-ambari.png)

1. **Özel kafka-broker** altında **ssl.client.auth** `required`özelliği ni . Bu adım yalnızca kimlik doğrulaması ve şifreleme ayarlıyorsanız gereklidir.

    ![Ambari'de kafka ssl yapılandırma özelliklerinin düzenlenmesi](./media/apache-kafka-ssl-encryption-authentication/editing-configuration-ambari2.png)

1. HDI sürüm 3.6 için Ambari UI'ye gidin ve **Advanced kafka-env** ve **kafka-env şablon özelliği** altında aşağıdaki yapılandırmaları ekleyin.

    ```bash
    # Configure Kafka to advertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092,SSL://$IP_ADDRESS:9093" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.keystore.location=/home/sshuser/ssl/kafka.server.keystore.jks" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.keystore.password=MyServerPassword123" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.key.password=MyServerPassword123" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.truststore.location=/home/sshuser/ssl/kafka.server.truststore.jks" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.truststore.password=MyServerPassword123" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

1. Burada bu değişiklikler ile Ambari yapılandırma UI gösteren ekran görüntüsü.

    HDI sürüm 3.6 için:

    ![Ambari'de kafka-env şablon özelliğinin düzenlenmesi](./media/apache-kafka-ssl-encryption-authentication/editing-configuration-kafka-env.png)

    HDI sürüm 4.0 için:

     ![Ambari dört kafka-env şablon özelliği düzenleme](./media/apache-kafka-ssl-encryption-authentication/editing-configuration-kafka-env-four.png)

1. Tüm Kafka brokerlarını yeniden başlatın.

## <a name="client-setup-without-authentication"></a>İstemci kurulumu (kimlik doğrulaması olmadan)

Kimlik doğrulamanız gerekmiyorsa, yalnızca TLS şifrelemesini ayarlama adımlarının özeti şunlardır:

1. CA(etkin kafa düğümü) oturum açın.
1. CA sertifikasını CA makinesinden istemci makinesine kopyalayın (wn0).
1. İstemci makinesinde oturum açın (hn1) ve klasöre `~/ssl` gidin.
1. CA sertifikasını güven deposuna aktarın.
1. CA sertifikasını keystore'a aktarın.

Bu adımlar aşağıdaki kod parçacıklarında ayrıntılı olarak açıklanmaktadır.

1. CA düğümünde oturum açın.

    ```bash
    ssh sshuser@HeadNode0_Name
    cd ssl
    ```

1. Ca-cert'i istemci makinesine kopyalama

    ```bash
    scp ca-cert sshuser@HeadNode1_Name:~/ssl/ca-cert
    ```

1. İstemci makinesinde oturum açın (bekleme kafası düğümü).

    ```bash
    ssh sshuser@HeadNode1_Name
    cd ssl
    ```

1. CA sertifikasını güven deposuna aktarın.

    ```bash
    keytool -keystore kafka.client.truststore.jks -alias CARoot -import -file ca-cert -storepass "MyClientPassword123" -keypass "MyClientPassword123" -noprompt
    ```

1. CA sertifikasını keystore'a aktarın.
    
    ```bash
    keytool -keystore kafka.client.keystore.jks -alias CARoot -import -file ca-cert -storepass "MyClientPassword123" -keypass "MyClientPassword123" -noprompt
    ```

1. İstemci `client-ssl-auth.properties` makinesinde dosyayı oluşturun (hn1) . Aşağıdaki satırları olmalıdır:

    ```config
    security.protocol=SSL
    ssl.truststore.location=/home/sshuser/ssl/kafka.client.truststore.jks
    ssl.truststore.password=MyClientPassword123
    ```

1. Hem üreticilerin hem de tüketicilerin 9093 bağlantı noktası üzerinde çalıştığını doğrulamak için yönetici istemciyi üretici ve tüketici seçenekleriyle başlatın. Konsol üreticisi/tüketicisi kullanarak kurulumu doğrulamak için gereken adımlar için lütfen aşağıdaki [Doğrulama](apache-kafka-ssl-encryption-authentication.md#verification) bölümüne bakın.

## <a name="client-setup-with-authentication"></a>İstemci kurulumu (kimlik doğrulama ile)

> [!Note]
> Aşağıdaki adımlar yalnızca hem TLS **şifrelemehem** de kimlik doğrulaması ayarlıyorsanız gereklidir. Yalnızca şifreleme ayarlıyorsanız, [kimlik doğrulaması olmadan Istemci kurulumuna](apache-kafka-ssl-encryption-authentication.md#client-setup-without-authentication)bakın.

Aşağıdaki dört adım, istemci kurulumünü tamamlamak için gereken görevleri özetler:

1. İstemci makinesinde oturum açın (bekleme kafası düğümü).
1. Bir java keystore oluşturun ve broker için imzalı bir sertifika alın. Ardından sertifikayı CA'nın çalıştığı VM'ye kopyalayın.
1. İstemci sertifikasını imzalamak için CA makinesine (etkin kafa düğümü) geçin.
1. İstemci makinesine gidin (bekleme kafası düğümü) `~/ssl` ve klasöre gidin. İmzalı sertifikayı istemci makinesine kopyalayın.

Her adımın ayrıntıları aşağıda verilmiştir.

1. İstemci makinesinde oturum açın (bekleme kafası düğümü).

    ```bash
    ssh sshuser@HeadNode1_Name
    ```

1. Varolan ssl dizinini kaldırın.

    ```bash
    rm -R ~/ssl
    mkdir ssl
    cd ssl
    ```

1. Bir java anahtar mağazası oluşturun ve bir sertifika imzalama isteği oluşturun. 

    ```bash
    keytool -genkey -keystore kafka.client.keystore.jks -validity 365 -storepass "MyClientPassword123" -keypass "MyClientPassword123" -dname "CN=HEADNODE1_FQDN" -storetype pkcs12
    
    keytool -keystore kafka.client.keystore.jks -certreq -file client-cert-sign-request -storepass "MyClientPassword123" -keypass "MyClientPassword123"
    ```

1. Sertifika imzalama isteğini CA'ya kopyalama

    ```bash
    scp client-cert-sign-request sshuser@HeadNode0_Name:~/ssl/client-cert-sign-request
    ```

1. CA makinesine (etkin kafa düğümü) geçin ve istemci sertifikasını imzalayın.

    ```bash
    ssh sshuser@HeadNode0_Name
    cd ssl
    openssl x509 -req -CA ca-cert -CAkey ca-key -in ~/ssl/client-cert-sign-request -out ~/ssl/client-cert-signed -days 365 -CAcreateserial -passin pass:MyClientPassword123
    ```

1. İmzalı istemci sertifikasını CA'dan (etkin kafa düğümü) istemci makinesine kopyalayın.

    ```bash
    scp client-cert-signed sshuser@HeadNode1_Name:~/ssl/client-signed-cert
    ```

1. Ca-cert'i istemci makinesine kopyalama

    ```bash
    scp ca-cert sshuser@HeadNode1_Name:~/ssl/ca-cert
    ```

    1. İstemci makinesinde oturum açın (bekleme kafası düğümü) ve ssl dizinine gidin.

    ```bash
    ssh sshuser@HeadNode1_Name
    cd ssl
    ```

1. İmzalı sertifikaile istemci mağazası oluşturun ve istemci makinesindeki anahtar deposuna ve truststore'a ca sertifikasını aktarın (hn1):

    ```bash
    keytool -keystore kafka.client.truststore.jks -alias CARoot -import -file ca-cert -storepass "MyClientPassword123" -keypass "MyClientPassword123" -noprompt
    
    keytool -keystore kafka.client.keystore.jks -alias CARoot -import -file ca-cert -storepass "MyClientPassword123" -keypass "MyClientPassword123" -noprompt
    
    keytool -keystore kafka.client.keystore.jks -import -file client-cert-signed -storepass "MyClientPassword123" -keypass "MyClientPassword123" -noprompt
    ```

1. İstemci `client-ssl-auth.properties` makinesinde (hn1) bir dosya oluşturun. Aşağıdaki satırları olmalıdır:

    ```bash
    security.protocol=SSL
    ssl.truststore.location=/home/sshuser/ssl/kafka.client.truststore.jks
    ssl.truststore.password=MyClientPassword123
    ssl.keystore.location=/home/sshuser/ssl/kafka.client.keystore.jks
    ssl.keystore.password=MyClientPassword123
    ssl.key.password=MyClientPassword123
    ```

## <a name="verification"></a>Doğrulama

Bu adımları istemci makinesinde çalıştırın.

> [!Note]
> HDInsight 4.0 ve Kafka 2.1 yüklüyse, kurulumunuzu doğrulamak için konsol üreticisini/tüketicilerini kullanabilirsiniz. Değilse, 9092 portundaki Kafka üreticisini çalıştırın ve konuya mesaj gönderin ve TLS kullanan 9093 portundaki Kafka tüketicisini kullanın.

### <a name="kafka-21-or-above"></a>Kafka 2.1 ve üzeri

1. Zaten yoksa bir konu oluşturun.

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --zookeeper <ZOOKEEPER_NODE>:2181 --create --topic topic1 --partitions 2 --replication-factor 2
    ```

1. Konsol üreticisini başlatın `client-ssl-auth.properties` ve üretici için bir yapılandırma dosyası olarak yolu sağlayın.

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list <FQDN_WORKER_NODE>:9093 --topic topic1 --producer.config ~/ssl/client-ssl-auth.properties
    ```

1. İstemci makinesine başka bir ssh bağlantısı açın `client-ssl-auth.properties` ve konsol tüketicisini başlatın ve tüketici için bir yapılandırma dosyası olarak yolu sağlayın.

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server <FQDN_WORKER_NODE>:9093 --topic topic1 --consumer.config ~/ssl/client-ssl-auth.properties --from-beginning
    ```

### <a name="kafka-11"></a>Kafka 1.1

1. Zaten yoksa bir konu oluşturun.

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --zookeeper <ZOOKEEPER_NODE_0>:2181 --create --topic topic1 --partitions 2 --replication-factor 2
    ```

1. Konsol üreticisini başlatın ve istemci-ssl-auth.properties'e giden yolu üretici için bir yapılandırma dosyası olarak sağlayın.

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list <FQDN_WORKER_NODE>:9092 --topic topic1 
    ```

1. İstemci makinesine başka bir ssh bağlantısı açın `client-ssl-auth.properties` ve konsol tüketicisini başlatın ve tüketici için bir yapılandırma dosyası olarak yolu sağlayın.

    ```bash
    $ /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server <FQDN_WORKER_NODE>:9093 --topic topic1 --consumer.config ~/ssl/client-ssl-auth.properties --from-beginning
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight üzerinde Apache Kafka nedir?](apache-kafka-introduction.md)
