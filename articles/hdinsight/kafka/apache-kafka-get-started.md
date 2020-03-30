---
title: "Quickstart: Azure portalını kullanarak HDInsight'ta Apache Kafka'yı ayarlama"
description: Bu hızlı başlangıçta, Azure portalını kullanarak Azure HDInsight’ta bir Apache Kafka kümesi oluşturmayı öğrenirsiniz. Kafka konuları, aboneleri ve tüketicileri hakkında da bilgi edinirsiniz.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.custom: mvc
ms.date: 02/24/2020
ms.openlocfilehash: 90f7010970f70379c8adecc4214c44d896a1beaf
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80130238"
---
# <a name="quickstart-create-apache-kafka-cluster-in-azure-hdinsight-using-azure-portal"></a>Hızlı başlangıç: Azure portalını kullanarak Azure HDInsight'ta Apache Kafka kümesi oluşturma

[Apache Kafka](./apache-kafka-introduction.md) açık kaynak kodlu, dağıtılmış bir akış platformudur. Yayımla-abone ol ileti kuyruğuna benzer işlevler sağladığı için genellikle ileti aracısı olarak kullanılır.

Bu hızlı başlangıçta, Azure portalını kullanarak Apache Kafka kümesi oluşturmayı öğrenirsiniz. Ayrıca Apache Kafka kullanarak ileti göndermek ve almak için verilen yardımcı programları kullanmayı da öğrenirsiniz. Kullanılabilir yapılandırmaların ayrıntılı açıklamaları için [bkz.](../hdinsight-hadoop-provision-linux-clusters.md) Kümeoluşturmak için portalın kullanımıyla ilgili ek bilgi için [bkz.](../hdinsight-hadoop-create-linux-clusters-portal.md)

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

Apache Kafka API'sine yalnızca aynı sanal ağ içindeki kaynaklar tarafından erişilebilir. Bu hızlı başlangıçta, doğrudan SSH kullanarak kümeye erişirsiniz. Diğer hizmetleri, ağları veya sanal makineleri Apache Kafka'ya bağlamak için önce bir sanal ağ oluşturmanız e sonra ağ içinde kaynakları oluşturmanız gerekir. Daha fazla bilgi için [Sanal ağ kullanarak Apache Kafka'ya bağlanma](apache-kafka-connect-vpn-gateway.md) belgesine bakın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar

Bir SSH istemcisi. Daha fazla bilgi için [SSH kullanarak HDInsight'a (Apache Hadoop) bağlan'a](../hdinsight-hadoop-linux-use-ssh-unix.md)bakın.

## <a name="create-an-apache-kafka-cluster"></a>Apache Kafka kümesi oluşturma

HDInsight'ta bir Apache Kafka kümesi oluşturmak için aşağıdaki adımları kullanın:

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Üst menüden **+ Kaynak oluştur'u**seçin.

    ![Azure portalı kaynak HDInsight oluşturmak](./media/apache-kafka-get-started/azure-portal-create-resource.png)

1. **HDInsight oluştur küme** sayfasına gitmek için **Analytics** > **Azure HDInsight'ı** seçin.

1. **Temel Bilgiler sekmesinden** aşağıdaki bilgileri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |Abonelik    |  Açılan listeden, küme için kullanılan Azure aboneliğini seçin. |
    |Kaynak grubu     | Bir kaynak grubu oluşturun veya mevcut bir kaynak grubunu seçin.  Kaynak grubu, Azure bileşenleri için bir kapsayıcıdır.  Bu durumda, kaynak grubu HDInsight kümesini ve bağımlı Azure Depolama hesabını içermektedir. |
    |Küme adı   | Genel olarak benzersiz bir ad girin. Ad, harfler, sayılar ve tireler de dahil olmak üzere en fazla 59 karakterden oluşabilir. Adın ilk ve son karakterleri, kısa çizgi olamaz. |
    |Bölge    | Açılan listeden, kümenin oluşturulduğu bir bölge seçin.  Daha iyi performans için size daha yakın bir bölge seçin. |
    |Küme türü| Liste açmak için **küme türünü seçin'i** seçin. Listeden küme türü olarak **Kafka'yı** seçin.|
    |Sürüm|Küme türü için varsayılan sürüm belirtilir. Farklı bir sürüm belirtmek istiyorsanız açılan listeden seçim yap.|
    |Küme oturum açma kullanıcı adı ve parolası    | Varsayılan giriş adı **yöneticidir.** Parola en az 10 karakter uzunluğunda olmalı ve en az bir basamak, bir büyük harf ve bir küçük harf, bir alfanümerik olmayan karakter içermelidir (karakterler hariç ' ' . \) "Pass@word1" gibi genel parolalar **sağlamadığınızdan** emin olun.|
    |Secure Shell (SSH) kullanıcı adı | Varsayılan kullanıcı adı **sshuser** şeklindedir.  SSH kullanıcı adı için başka bir ad sağlayabilirsiniz. |
    |SSH için küme giriş parolası kullanma| Küme giriş kullanıcısı için sağladığınız parolayla aynı Parolayı Kullanmak için bu onay kutusunu seçin.|

   ![Azure portalı küme temelleri oluşturma](./media/apache-kafka-get-started/azure-portal-cluster-basics.png)

    Her Azure bölgesi (konum) _hata etki alanları_ sağlar. Hata etki alanı, bir Azure veri merkezinde temel donanımlardan oluşan mantıksal bir gruplandırmadır. Her hata etki alanı ortak bir güç kaynağı ve ağ anahtarına sahiptir. Bir HDInsight kümesi içindeki düğümleri uygulayan sanal makineler ve yönetilen diskler, bu hata etki alanlarına dağıtılır. Bu mimari, fiziksel donanım hatalarının olası etkisini sınırlar.

    Verilerin yüksek kullanılabilirliği için, __üç hata etki alanı__ içeren bir bölge (konum) seçin. Bir bölgedeki hata etki alanlarının sayısı hakkında bilgi almak için [Linux sanal makinelerinin kullanılabilirliği](../../virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) belgesine bakın.

    **Sonraki: Depolama >>** sekmesini seçerek depolama ayarlarına ilerleyin.

1. **Depolama** sekmesinden aşağıdaki değerleri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |Birincil depolama türü|Varsayılan değeri kullanın **Azure Depolama**.|
    |Seçim yöntemi|Varsayılan değeri kullanın **Listeden seçin.**|
    |Birincil depolama hesabı|Varolan bir depolama hesabını seçmek için açılır listeyi kullanın veya **yeni hesap oluştur'u**seçin. Yeni bir hesap oluşturursanız, ad uzunluğu 3 ile 24 karakter arasında olmalıdır ve yalnızca sayılar ve küçük harfler içerebilir|
    |Kapsayıcı|Otomatik doldurulan değeri kullanın.|

    ![HDInsight Linux küme depolama değerleri sağlamak başlamak olsun](./media/apache-kafka-get-started/azure-portal-cluster-storage.png "HDInsight kümesi oluşturmak için depolama değerleri sağlama")

    Güvenlik **+ ağ sekmesini** seçin.

1. Bu hızlı başlangıçta varsayılan güvenlik ayarlarını değiştirmeden bırakın. Kurumsal Güvenlik paketi hakkında daha fazla bilgi edinmek için [Azure Active Directory Domain Services'i kullanarak bir HDInsight kümesi ile Kurumsal Güvenlik Paketi yapılandırma](../domain-joined/apache-domain-joined-configure-using-azure-adds.md) sayfasını ziyaret edin. Apache Kafka Disk Şifreleme için kendi anahtarınızı nasıl kullanacağınızı öğrenmek için [Müşteri tarafından yönetilen anahtar disk şifrelemeadresini](../disk-encryption.md) ziyaret edin

   Kümenizi bir sanal ağa bağlamak istiyorsanız, aşağı açılan **Sanal ağ** listesinden bir sanal ağ seçin.

   ![Sanal ağa küme ekleme](./media/apache-kafka-get-started/azure-portal-cluster-security-networking-kafka-vnet.png)

    Yapılandırma **+ fiyatlandırma** sekmesini seçin.

1. HdInsight'ta Apache Kafka'nın kullanılabilirliğini garanti etmek **için, İşçi düğümü** için düğüm giriş __sayısı__ 3 veya daha fazla olarak ayarlanmalıdır. Varsayılan değer 4'tür.

    Alt düğüm girişi **başına standart diskler** HDInsight'ta Apache Kafka'nın ölçeklenebilirliğini yapılandırır. HDInsight üzerinde Apache Kafka, veri depolamak için kümedeki sanal makinelerin yerel diskini kullanır. Apache Kafka yoğun G/Ç kullandığından yüksek aktarım hızı ve düğüm başına daha fazla depolama alanı sağlamak için [Azure Yönetilen Diskler](../../virtual-machines/windows/managed-disks-overview.md) kullanılır. Yönetilen diskin türü __Standart__ (HDD) veya __Premium__ (SSD) olabilir. Disk türü, çalışan düğümler (Apache Kafka aracıları) tarafından kullanılan sanal makine boyutuna bağlıdır. Premium diskler otomatik olarak DS ve GS serisi sanal makinelerle kullanılır. Diğer tüm VM türleri standart disk kullanır.

   ![Apache Kafka küme boyutunu ayarlama](./media/apache-kafka-get-started/azure-portal-cluster-configuration-pricing-kafka.png)

    Gözden **Geçir + oluştur** sekmesini seçin.

1. Küme nin yapılandırmasını gözden geçirin. Yanlış olan ayarları değiştirin. Son olarak, küme oluşturmak için **Oluştur'u** seçin.

    ![kafka küme yapılandırma özeti](./media/apache-kafka-get-started/azure-portal-cluster-review-create-kafka.png)

    Kümenin oluşturulması 20 dakika sürebilir.

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

1. Kümenize bağlanmak için [ssh komutunu](../hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME'yi kümenizin adıyla değiştirerek aşağıdaki komutu düzenleme ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. İstendiğinde, SSH kullanıcısının parolasını girin.

    Bağlandığında, aşağıdaki metne benzer bilgiler görürsünüz:

    ```output
    Authorized uses only. All activity may be monitored and reported.
    Welcome to Ubuntu 16.04.4 LTS (GNU/Linux 4.13.0-1011-azure x86_64)

     * Documentation:  https://help.ubuntu.com
     * Management:     https://landscape.canonical.com
     * Support:        https://ubuntu.com/advantage

      Get cloud support with Ubuntu Advantage Cloud Guest:
        https://www.ubuntu.com/business/services/cloud

    83 packages can be updated.
    37 updates are security updates.


    Welcome to Apache Kafka on HDInsight.

    Last login: Thu Mar 29 13:25:27 2018 from 108.252.109.241
    ```

## <a name="get-the-apache-zookeeper-and-broker-host-information"></a><a id="getkafkainfo"></a>Apache Zookeeper ve Broker ev sahibi bilgilerini alın

Kafka ile çalışırken, *Apache Zookeeper* ve *Broker* ev sahiplerini tanımalısınız. Bu konaklar Apache Kafka API'si ve Kafka ile gönderilen yardımcı programların birçoğu ile birlikte kullanılır.

Bu bölümde, kümedeki Apache Ambari REST API'sinden ana bilgisayar bilgilerini alırsınız.

1. Bir komut satırı JSON işlemci [jq](https://stedolan.github.io/jq/)yükleyin. Bu yardımcı program JSON belgelerini ayrıştırmak için kullanılır ve ana bilgisayar bilgilerini ayrıştırmada yararlıdır. Açık SSH bağlantısından, yüklemek `jq`için aşağıdaki komutu girin:

    ```bash
    sudo apt -y install jq
    ```

1. Parola değişkenini ayarlayın. Küme `PASSWORD` giriş parolası ile değiştirin ve ardından komutu girin:

    ```bash
    export password='PASSWORD'
    ```

1. Doğru kasalı küme adını ayıklayın. Küme adının gerçek gövdesi, kümenin nasıl oluşturulduğuna bağlı olarak beklediğiniz den farklı olabilir. Bu komut, gerçek kasayı alır ve sonra bir değişkende saklar. Aşağıdaki komutu girin:

    ```bash
    export clusterName=$(curl -u admin:$password -sS -G "http://headnodehost:8080/api/v1/clusters" | jq -r '.items[].Clusters.cluster_name')
    ```

    > [!Note]  
    > Bu işlemi kümenin dışından yapıyorsanız, küme adını depolamak için farklı bir yordam vardır. Azure portalından küçük harfli küme adını alın. Ardından, küme adını `<clustername>` aşağıdaki komutta değiştirin `export clusterName='<clustername>'`ve çalıştırın: .


1. Zookeeper ana bilgisayar bilgileriyle bir ortam değişkeni ayarlamak için aşağıdaki komutu kullanın. Komut tüm Zookeeper ana bilgisayarlarını alır ve yalnızca ilk iki girişi döndürür. Bunun nedeni, bir ana bilgisayarın ulaşılamaz olması durumunda yedeklilik istemenizdir.

    ```bash
    export KAFKAZKHOSTS=$(curl -sS -u admin:$password -G https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2);
    ```

    > [!Note]  
    > Bu komut Ambari erişimi gerektirir. Kümeniz bir NSG'nin arkasındaysa, bu komutu Ambari'ye erişebilen bir makineden çalıştırın. 

1. Ortam değişkeninin düzgün şekilde ayarlandığını doğrulamak için aşağıdaki komutu kullanın:

    ```bash
    echo $KAFKAZKHOSTS
    ```

    Bu komutun aşağıdaki metne benzer bilgiler döndürmesi gerekir:

    `zk0-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk2-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181`

1. Bir ortam değişkenini Apache Kafka aracı konak bilgileriyle ayarlamak için aşağıdaki komutu kullanın:

    ```bash
    export KAFKABROKERS=$(curl -sS -u admin:$password -G https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2);
    ```

    > [!Note]  
    > Bu komut Ambari erişimi gerektirir. Kümeniz bir NSG'nin arkasındaysa, bu komutu Ambari'ye erişebilen bir makineden çalıştırın. 

1. Ortam değişkeninin düzgün şekilde ayarlandığını doğrulamak için aşağıdaki komutu kullanın:

    ```bash
    echo $KAFKABROKERS
    ```

    Bu komutun aşağıdaki metne benzer bilgiler döndürmesi gerekir:

    `wn1-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092`

## <a name="manage-apache-kafka-topics"></a>Apache Kafka konularını yönetme

Kafka, veri akışlarını *konular* içinde depolar. Konuları yönetmek için `kafka-topics.sh` yardımcı programını kullanabilirsiniz.

* **Bir konu oluşturmak için**, SSH bağlantısında aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
    ```

    Bu komut, `$KAFKAZKHOSTS` içinde depolanan konak bilgilerini kullanarak Zookeeper’a bağlanır. Daha sonra **test** adlı bir Apache Kafka konusu oluşturur.

    * Bu konuda depolanan veriler, sekiz bölüm halinde bölümlenir.

    * Her bölüm, kümedeki üç çalışan düğümü arasında çoğaltılır.

        Üç hata etki alanı sağlayan bir Azure bölgesinde kümeyi oluşturduysanız, 3 çoğaltma katsayısını kullanın. Aksi takdirde 4 çoğaltma katsayısını kullanın.
        
        Üç hata etki alanı içeren bölgelerde 3 çoğaltma katsayısı, çoğaltmaların hata etki alanları arasında yayılmasına olanak sağlar. İki hata etki alanı içeren bölgelerde dört çoğaltma katsayısı, çoğaltmaların etki alanları arasında eşit şekilde yayılmasına olanak sağlar.
        
        Bir bölgedeki hata etki alanlarının sayısı hakkında bilgi almak için [Linux sanal makinelerinin kullanılabilirliği](../../virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) belgesine bakın.

        Apache Kafka, Azure hata etki alanları ile uyumlu değildir. Konular için bölüm çoğaltmaları oluşturulurken, çoğaltmalar yüksek kullanılabilirlik için düzgün şekilde dağıtılmayabilir.

        Yüksek kullanılabilirlik sağlamak [için, Apache Kafka bölüm yeniden dengeleme aracını](https://github.com/hdinsight/hdinsight-kafka-tools)kullanın. Bu araç, Apache Kafka kümenizin baş düğümüyle kurulan bir SSH bağlantısından çalıştırılmalıdır.

        Apache Kafka verilerinizin en yüksek kullanılabilirliğe sahip olması için aşağıdaki durumlarda konunuz için bölüm çoğaltmalarını yeniden dengelemeniz gerekir:

        * Yeni bir konu veya bölüm oluşturduğunuzda

        * Bir kümenin ölçeğini artırdığınızda

* **Konuları listelemek için** aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
    ```

    Bu komut, Apache Kafka kümesinde bulunan konuları listeler.

* **Bir konuyu silmek için** aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --delete --topic topicname --zookeeper $KAFKAZKHOSTS
    ```

    Bu komut, `topicname` adlı konuyu siler.

    > [!WARNING]  
    > Daha önce oluşturulan `test` konusunu silerseniz, yeniden oluşturmanız gerekir. Bu belgenin ilerleyen kısmındaki adımlarda kullanılacaktır.

`kafka-topics.sh` yardımcı programı ile kullanılabilen komutlar hakkında daha fazla bilgi için aşağıdaki komutu kullanın:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh
```

## <a name="produce-and-consume-records"></a>Kayıt oluşturma ve kullanma

Kafka, konu başlıklarında *records* (kayıtlar) depolar. Kayıtlar, *Üreticiler* tarafından oluşturulur ve *tüketiciler* tarafından kullanılır. Üreticiler ve tüketiciler, *Kafka aracısı* hizmetiyle iletişim kurar. HDInsight kümenizdeki her çalışan düğümü bir Apache Kafka aracı konağıdır.

Daha önce oluşturduğunuz test konu başlığında kayıtları depolamak ve ardından bir tüketici kullanarak bunları okumak için aşağıdaki adımları kullanın:

1. Konuya kayıtlar yazmak için SSH bağlantısından `kafka-console-producer.sh` yardımcı programını kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test
    ```

    Bu komuttan sonra boş bir satıra ulaşırsınız.

2. Boş satıra bir metin iletisi yazın ve Enter tuşuna basın. Bu şekilde birkaç ileti girin ve sonra normal isteme geri dönmek için **Ctrl + C** tuşlarını kullanın. Her satır, Apache Kafka konusuna ayrı bir kayıt olarak gönderilir.

3. Konudan kayıtları okumak için SSH bağlantısından `kafka-console-consumer.sh` yardımcı programını kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic test --from-beginning
    ```

    Bu komutla, kayıtlar konu başlığından alınır ve görüntülenir. `--from-beginning` kullanılması, tüketiciye akışın başından başlamasını söyler, böylece tüm kayıtlar alınır.

    Kafka’nın eski bir sürümünü kullanıyorsanız `--bootstrap-server $KAFKABROKERS` değerini `--zookeeper $KAFKAZKHOSTS` ile değiştirin.

4. Tüketiciyi durdurmak için __Ctrl + C__ tuşlarını kullanın.

Ayrıca programlı olarak üretici ve tüketici de oluşturabilirsiniz. Bu API'yi kullanma örneğine bakın, [HDInsight belgeli Apache Kafka Üretici ve Tüketici API'si.](apache-kafka-producer-consumer-api.md)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıç tarafından oluşturulan kaynakları temizlemek için kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, ilişkili HDInsight kümesini ve kaynak grubuyla ilişkili diğer tüm kaynakları da siler.

Azure portalını kullanarak kaynak grubunu kaldırmak için:

1. Azure portalında sol taraftaki menüyü genişleterek hizmet menüsünü açın ve sonra __Kaynak Grupları__'nı seçerek kaynak gruplarınızın listesini görüntüleyin.
2. Silinecek kaynak grubunu bulun ve sonra listenin sağ tarafındaki __Daha fazla__ düğmesine (...) sağ tıklayın.
3. __Kaynak grubunu sil__'i seçip onaylayın.

> [!WARNING]  
> HDInsight'ta bir Apache Kafka kümesini silmek Kafka'da depolanan tüm verileri siler.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Apache Kafka ile Apache Spark kullanın](../hdinsight-apache-kafka-spark-structured-streaming.md)
