---
title: Mirror Apache Kafka konuları - Azure HDInsight
description: Konuları ikincil bir kümeye yansıtarak HDInsight kümesindeki Kafka'nın bir kopyasını korumak için Apache Kafka'nın yansıtma özelliğini nasıl kullanacağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/29/2019
ms.openlocfilehash: 45977f52226fac0a3e23455ce9457a721947a8cc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77425893"
---
# <a name="use-mirrormaker-to-replicate-apache-kafka-topics-with-kafka-on-hdinsight"></a>MirrorMaker kullanarak Apache Kafka konularını HDInsight üzerinde Kafka ile çoğaltma

Konuları ikincil bir kümeye çoğaltmak için Apache Kafka'nın yansıtma özelliğini nasıl kullanacağınızı öğrenin. Yansıtma sürekli bir işlem olarak çalıştırılabilir veya verileri bir kümeden diğerine geçirme yöntemi olarak aralıklı olarak kullanılabilir.

Bu örnekte, iki HDInsight kümesi arasındaki konuları çoğaltmak için yansıtma kullanılır. Her iki küme de farklı veri merkezlerinde farklı sanal ağlarda bulunmaktadır.

> [!WARNING]  
> Yansıtma, hata toleransı elde etmek için bir araç olarak düşünülmemelidir. Bir konu içindeki öğelere mahsup, birincil ve ikincil kümeler arasında farklıdır, bu nedenle istemciler ikisini birbirinin yerine kullanamaz.
>
> Hata toleransı konusunda endişeleriniz varsa, kümenizdeki konular için çoğaltma ayarlamanız gerekir. Daha fazla bilgi için [HDInsight'ta Apache Kafka ile başlayın.](apache-kafka-get-started.md)

## <a name="how-apache-kafka-mirroring-works"></a>Apaçi Kafka aynalama nasıl çalışır?

Yansıtma, birincil kümedeki konulardan kayıtları tüketmek ve ardından ikincil kümeüzerinde yerel bir kopya oluşturmak için [MirrorMaker](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) aracını (Apache Kafka'nın bir parçası) kullanarak çalışır. MirrorMaker, birincil kümeden okuyan bir (veya daha fazla) *tüketici* ve yerel (ikincil) kümeye yazan bir *üretici* kullanır.

Olağanüstü durum kurtarma için en kullanışlı yansıtma kurulumu, farklı Azure bölgelerindeki Kafka kümelerini kullanır. Bunu başarmak için, kümelerin bulunduğu sanal ağlar birlikte bakılır.

Aşağıdaki diyagram, yansıtma işlemini ve kümeler arasındaki iletişimin nasıl aktığını göstermektedir:

![Yansıtma işleminin diyagramı](./media/apache-kafka-mirroring/kafka-mirroring-vnets2.png)

Birincil ve ikincil kümeler düğüm ve bölüm sayısında farklı olabilir ve konular içindeki uzaklıklar da farklıdır. Yansıtma, bölümleme için kullanılan anahtar değerini korur, bu nedenle kayıt sırası her anahtar bazında korunur.

### <a name="mirroring-across-network-boundaries"></a>Ağ sınırları boyunca yansıtma

Kafka kümeleri arasında farklı ağlarda yansıtmanız gerekiyorsa, aşağıdaki ek hususlar vardır:

* **Ağ geçitleri**: Ağlar TCP/IP düzeyinde iletişim kurabilmelidir.

* **Sunucu adresi**: IP adreslerini veya tam nitelikli alan adlarını kullanarak küme düğümlerinizi ele almayı seçebilirsiniz.

    * **IP adresleri**: Kafka kümelerinizi IP adresi reklamcılığı kullanacak şekilde yapılandırırsanız, broker düğümlerinin ve zookeeper düğümlerinin IP adreslerini kullanarak yansıtma kurulumuna devam edebilirsiniz.
    
    * **Alan adları**: Kafka kümelerinizi IP adresi reklamı için yapılandırmazsanız, kümelerin Tam Nitelikli Alan Adları (FQDNs) kullanarak birbirlerine bağlanabilmeleri gerekir. Bu, istekleri diğer ağlara iletmek üzere yapılandırılan her ağdaki bir Etki Alanı Adı Sistemi (DNS) sunucusu gerektirir. Bir Azure Sanal Ağı oluştururken, ağla sağlanan otomatik DNS'yi kullanmak yerine, sunucuiçin özel bir DNS sunucusu ve IP adresi belirtmeniz gerekir. Sanal Ağ oluşturulduktan sonra, bu IP adresini kullanan bir Azure Sanal Makinesi oluşturmanız ve ardından üzerine DNS yazılımını yüklemeniz ve yapılandırmanız gerekir.

    > [!WARNING]  
    > HDInsight'ı Sanal Ağa yüklemeden önce özel DNS sunucusuoluşturun ve yapılandırın. HDInsight'ın Sanal Ağ için yapılandırılan DNS sunucusunu kullanması için ek bir yapılandırma gerekmez.

İki Azure Sanal Ağı'nı bağlama hakkında daha fazla bilgi için [vnet-vnet bağlantısını yapılandırma bilgisine](../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)bakın.

## <a name="mirroring-architecture"></a>Yansıtma mimarisi

Bu mimari, farklı kaynak gruplarında ve sanal ağlarda iki küme içerir: **birincil** ve **ikincil.**

### <a name="creation-steps"></a>Oluşturma adımları

1. İki yeni kaynak grubu oluşturun:

    |Kaynak Grubu | Konum |
    |---|---|
    | kafka-birincil-rg | Orta ABD |
    | kafka-ikincil-rg | Orta Kuzey ABD |

1. **Kafka-primary-rg'de**yeni bir sanal ağ **kafka-primary-vnet** oluşturun. Varsayılan ayarları bırakın.
1. **Kafka-secondary-rg'de**de varsayılan ayarlarla yeni bir sanal ağ **kafka-secondary-vnet** oluşturun.

1. İki yeni Kafka kümesi oluşturun:

    | Küme adı | Kaynak Grubu | Sanal Ağ | Depolama Hesabı |
    |---|---|---|---|
    | kafka-birincil-küme | kafka-birincil-rg | kafka-birincil-vnet | kafkaprimarystorage |
    | kafka-ikincil küme | kafka-ikincil-rg | kafka-ikincil-vnet | kafkasecondarystorage |

1. Sanal ağ eşlemeleri oluşturun. Bu adım iki eşleme yaratacaktır: biri **kafka-primary-vnet'ten** **kafka-secondary-vnet'e,** diğeri **kafka-secondary-vnet'ten** **kafka-primary-vnet'e**.
    1. **Kafka-primary-vnet** sanal ağını seçin.
    1. **Ayarlar**altında **Eşler'i** seçin.
    1. **Ekle'yi**seçin.
    1. **Eşlene ekle** ekranında, aşağıdaki ekran görüntüsünde gösterildiği gibi ayrıntıları girin.

        ![HDInsight Kafka vnet peering ekleyin](./media/apache-kafka-mirroring/hdi-add-vnet-peering.png)

### <a name="configure-ip-advertising"></a>IP reklamlarını yapılandırma

Bir istemcinin etki alanı adları yerine aracıIP adreslerini kullanarak bağlanmasını sağlamak için IP reklamlarını yapılandırın.

1. Birincil küme için Ambari panosuna `https://PRIMARYCLUSTERNAME.azurehdinsight.net`gidin: .
1. **Hizmetleri** > **Seçin Kafka**. **Configs** sekmesini CliSelectck.
1. Alt **kafka-env şablon** bölümüne aşağıdaki config satırları ekleyin. **Kaydet'i**seçin.

    ```
    # Configure Kafka to advertise IP addresses instead of FQDN
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

1. **Yapılandırmayı Kaydet** ekranına bir not girin ve **Kaydet'e**tıklayın.
1. Yapılandırma uyarısı istenirse, Yine de **Devam Et'i**tıklatın.
1. **Yapılandırma Yık'ı Kaydet'te** **Tamam'ı** seçin.
1. **Gerekli** Yeniden Başlat bildiriminde**Etkilenen TümÜnü Yeniden** **Başlat'ı** > seçin. **Tümünü Yeniden Başlat'ı**Onayla'yı seçin.

    ![Apache Ambari tüm etkilenen yeniden başlatın](./media/apache-kafka-mirroring/ambari-restart-notification.png)

### <a name="configure-kafka-to-listen-on-all-network-interfaces"></a>Kafka'yı tüm ağ arabirimlerini dinleyecek şekilde yapılandırın.
    
1.  >  **Hizmetler****Kafka**altında **Configs** sekmesinde kalın. Kafka **Broker** bölümünde **dinleyici** özelliği ayarlayın `PLAINTEXT://0.0.0.0:9092`.
1. **Kaydet'i**seçin.
1. **Yeniden Başlat'ı**seçin ve **Tümünü Yeniden Başlat'ı Onaylayın'** ı seçin.

### <a name="record-broker-ip-addresses-and-zookeeper-addresses-for-primary-cluster"></a>Birincil küme için Kayıt BrokerIP adresleri ve Zookeeper adresleri.

1. Ambari panosunda **Hosts'ı** seçin.
1. Brokerler ve Zookeepers için IP Adresleri bir not olun. Broker düğümleri ana adının ilk iki harfi olarak **wn** var ve zookeeper düğümleri ev sahibi adının ilk iki harfi olarak **zk** var.

    ![Apache Ambari görünüm düğümü ip adresleri](./media/apache-kafka-mirroring/view-node-ip-addresses2.png)

1. İkinci küme **kafka-ikincil küme**için önceki üç adımı tekrarlayın: IP reklamlarını yapılandırın, dinleyicileri ayarlayın ve Broker ve Zookeeper IP adreslerini not edin.

## <a name="create-topics"></a>Konu oluşturma

1. SSH kullanarak **birincil** kümeye bağlanın:

    ```bash
    ssh sshuser@PRIMARYCLUSTER-ssh.azurehdinsight.net
    ```

    Küme oluşturulurken kullanılan SSH kullanıcı adı ile **sshuser** değiştirin. **PRIMARYCLUSTER'ı** küme oluştururken kullanılan temel adla değiştirin.

    Bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

1. Birincil küme için Apache Zookeeper ana bilgisayarları ile bir değişken oluşturmak için aşağıdaki komutu kullanın. Gibi `ZOOKEEPER_IP_ADDRESS1` dizeleri gibi daha önce kaydedilen gerçek IP adresleri ile `10.23.0.11` `10.23.0.7`değiştirilmelidir, gibi ve . Özel bir DNS sunucusuyla FQDN çözünürlüğü kullanıyorsanız, broker ve zookeeper adlarını almak için [aşağıdaki adımları](apache-kafka-get-started.md#getkafkainfo) izleyin.:

    ```bash
    # get the zookeeper hosts for the primary cluster
    export PRIMARY_ZKHOSTS='ZOOKEEPER_IP_ADDRESS1:2181, ZOOKEEPER_IP_ADDRESS2:2181, ZOOKEEPER_IP_ADDRESS3:2181'
    ```

1. Adlı `testtopic`bir konu oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic testtopic --zookeeper $PRIMARY_ZKHOSTS
    ```

1. Konunun oluşturulduğunu doğrulamak için aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $PRIMARY_ZKHOSTS
    ```

    Yanıt içerir. `testtopic`

1. Bu **(birincil)** küme için Zookeeper ana bilgisayar bilgilerini görüntülemek için aşağıdakileri kullanın:

    ```bash
    echo $PRIMARY_ZKHOSTS
    ```

    Bu, aşağıdaki metne benzer bilgileri döndürür:

    `10.23.0.11:2181,10.23.0.7:2181,10.23.0.9:2181`

    Bu bilgileri kaydedin. Bir sonraki bölümde kullanılır.

## <a name="configure-mirroring"></a>Yansıtmayı yapılandırma

1. Farklı bir SSH oturumu kullanarak **ikincil** kümeye bağlanın:

    ```bash
    ssh sshuser@SECONDARYCLUSTER-ssh.azurehdinsight.net
    ```

    Küme oluşturulurken kullanılan SSH kullanıcı adı ile **sshuser** değiştirin. **İKINCIl KÜME'yi** küme oluştururken kullanılan adla değiştirin.

    Bilgi için bkz. [HDInsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).

1. Bir `consumer.properties` dosya **birincil** küme ile iletişimi yapılandırmak için kullanılır. Dosyayı oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    nano consumer.properties
    ```

    `consumer.properties` Aşağıdaki metni dosyanın içeriği olarak kullanın:

    ```yaml
    zookeeper.connect=PRIMARY_ZKHOSTS
    group.id=mirrorgroup
    ```

    birincil **kümedeki** Zookeeper IP Adresleri ile **PRIMARY_ZKHOSTS** değiştirin.

    Bu dosya, birincil Kafka kümesinden okurken kullanılacak tüketici bilgilerini açıklar. Daha fazla bilgi için tüketici yapılandırması için kafka.apache.org'daki [Consumer Configs'e](https://kafka.apache.org/documentation#consumerconfigs) bakın.

    Dosyayı kaydetmek için **Ctrl + X**, **Y**ve ardından **Enter**'u kullanın.

1. İkincil kümeyle iletişim kuran üreticiyi yapılandırmadan önce, **ikincil** kümenin broker IP adresleri için bir değişken ayarlayın. Bu değişkeni oluşturmak için aşağıdaki komutları kullanın:

    ```bash
    export SECONDARY_BROKERHOSTS='BROKER_IP_ADDRESS1:9092,BROKER_IP_ADDRESS2:9092,BROKER_IP_ADDRESS2:9092'
    ```

    Komut `echo $SECONDARY_BROKERHOSTS` aşağıdaki metne benzer bilgileri döndürmelidir:

    `10.23.0.14:9092,10.23.0.4:9092,10.23.0.12:9092`

1. Bir `producer.properties` dosya **ikincil** küme iletişim kurmak için kullanılır. Dosyayı oluşturmak için aşağıdaki komutu kullanın:

    ```bash
    nano producer.properties
    ```

    `producer.properties` Aşağıdaki metni dosyanın içeriği olarak kullanın:

    ```yaml
    bootstrap.servers=SECONDARY_BROKERHOSTS
    compression.type=none
    ```

    **SECONDARY_BROKERHOSTS** önceki adımda kullanılan broker IP adresleriyle değiştirin.

    Daha fazla bilgi için üretici yapılandırması için kafka.apache.org'daki [Producer Configs'e](https://kafka.apache.org/documentation#producerconfigs) bakın.

1. İkincil küme için Zookeeper ana bilgisayarlarının IP adresleriyle bir ortam değişkeni oluşturmak için aşağıdaki komutları kullanın:

    ```bash
    # get the zookeeper hosts for the secondary cluster
    export SECONDARY_ZKHOSTS='ZOOKEEPER_IP_ADDRESS1:2181,ZOOKEEPER_IP_ADDRESS2:2181,ZOOKEEPER_IP_ADDRESS3:2181'
    ```

1. HDInsight'ta Kafka için varsayılan yapılandırma, konuların otomatik olarak oluşturulmasına izin vermez. Yansıtma işlemini başlatmadan önce aşağıdaki seçeneklerden birini kullanmanız gerekir:

    * **İkincil kümedeki konuları oluşturma**: Bu seçenek, bölüm sayısını ve çoğaltma faktörlerini ayarlamanızı da sağlar.

        Aşağıdaki komutu kullanarak konuları önceden oluşturabilirsiniz:

        ```bash
        /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic testtopic --zookeeper $SECONDARY_ZKHOSTS
        ```

        Oluşturmak `testtopic` için konunun adı ile değiştirin.

    * **Otomatik konu oluşturma için kümeyi yapılandırın**: Bu seçenek MirrorMaker'ın otomatik olarak konuları oluşturmasına olanak sağlar, ancak bunları birincil başlıktan farklı sayıda bölüm veya çoğaltma faktörüyle oluşturabilir.

        İkincil kümeyi otomatik olarak konu oluşturacak şekilde yapılandırmak için aşağıdaki adımları gerçekleştirin:

        1. İkincil küme için Ambari panosuna gidin: `https://SECONDARYCLUSTERNAME.azurehdinsight.net`.
        1.  > Hizmetler **Services****Kafka'ya**tıklayın. **Configs** sekmesini tıklatın.
        1. __Filtre__ alanına bir değer `auto.create`girin. Bu özellik listesini filtreler ve `auto.create.topics.enable` ayarı görüntüler.
        1. Gerçeğe uygun `auto.create.topics.enable` değeri değiştirin ve sonra __Kaydet'i__seçin. Not ekleyin ve sonra yeniden __Kaydet'i__ seçin.
        1. __Kafka__ hizmetini seçin, __Yeniden Başlat'ı__seçin ve etkilenen __tümünü Yeniden Başlat'ı__seçin. İstendiğinde, __tümünü yeniden başlat'ı onayla'yı__seçin.

        ![kafka otomatik oluşturma konuları etkinleştirin](./media/apache-kafka-mirroring/kafka-enable-auto-create-topics.png)

## <a name="start-mirrormaker"></a>MirrorMaker başlat

1. SSH bağlantısından **ikincil** kümeye, MirrorMaker işlemini başlatmak için aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-run-class.sh kafka.tools.MirrorMaker --consumer.config consumer.properties --producer.config producer.properties --whitelist testtopic --num.streams 4
    ```

    Bu örnekte kullanılan parametreler şunlardır:

    |Parametre |Açıklama |
    |---|---|
    |--consumer.config|Tüketici özelliklerini içeren dosyayı belirtir. Bu *özellikler, birincil* Kafka kümesinden okuyan bir tüketici oluşturmak için kullanılır.|
    |--producer.config|Üretici özelliklerini içeren dosyayı belirtir. Bu özellikler *ikincil* Kafka kümesine yazan bir üretici oluşturmak için kullanılır.|
    |--beyaz liste|MirrorMaker'ın birincil kümeden ikincil kümeye kopyaettiği konuların listesi.|
    |--num.streams|Oluşturulacak tüketici iş parçacığı sayısı.|

    İkincil düğümdeki tüketici şimdi ileti leri almayı bekliyor.

2. SSH bağlantısından **birincil** kümeye, bir üretici başlatmak ve konuya ileti göndermek için aşağıdaki komutu kullanın:

    ```bash
    export PRIMARY_BROKERHOSTS=BROKER_IP_ADDRESS1:9092,BROKER_IP_ADDRESS2:9092,BROKER_IP_ADDRESS2:9092
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $SOURCE_BROKERHOSTS --topic testtopic
    ```

     İmleçli boş bir satıra vardığınızda, birkaç kısa mesaj yazın. İletiler **birincil** kümedeki konuya gönderilir. Bittiğinde, üretici işlemini sona erdirmek için **Ctrl + C** kullanın.

3. SSH bağlantısından **ikincil** kümeye, MirrorMaker işlemini sona erdirmek için **Ctrl + C'yi** kullanın. İşlemin sona ermesi birkaç saniye sürebilir. İletilerin ikincil olarak çoğaltıldığını doğrulamak için aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $SECONDARY_ZKHOSTS --topic testtopic --from-beginning
    ```

    MirrorMaster konuyu birincil `testtopic`kümeden ikincil kümeye yansıttığında oluşturulan konular şimdi içerir. Konudan alınan iletiler, birincil kümeye girdiğiniz iletilerle aynıdır.

## <a name="delete-the-cluster"></a>Küme silme

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

Bu belgedeki adımlar, farklı Azure kaynak gruplarında kümeler oluşturdu. Oluşturulan tüm kaynakları silmek için oluşturulan iki kaynak grubunu silebilirsiniz: **kafka-primary-rg** ve **kafka-secondary_rg.** Kaynak gruplarının silmesi, kümeler, sanal ağlar ve depolama hesapları da dahil olmak üzere bu belgeyi izleyerek oluşturulan tüm kaynakları kaldırır.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Bir [Apache Kafka](https://kafka.apache.org/) kümesinin bir kopyasını oluşturmak için [MirrorMaker'ı](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) kullanmayı öğrendiniz. Kafka ile çalışmanın diğer yollarını keşfetmek için aşağıdaki bağlantıları kullanın:

* [cwiki.apache.org'de Apache Kafka MirrorMaker belgeleri.](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330)
* [Kafka Ayna Yapıcı En İyi Uygulamalar](https://community.cloudera.com/t5/Community-Articles/Kafka-Mirror-Maker-Best-Practices/ta-p/249269)
* [HDInsight'ta Apache Kafka ile başlayın](apache-kafka-get-started.md)
* [Apache Spark'ı Apache Kafka ile HDInsight'ta kullanın](../hdinsight-apache-spark-with-kafka.md)
* [Azure Sanal Ağı üzerinden Apache Kafka'ya bağlanın](apache-kafka-connect-vpn-gateway.md)
