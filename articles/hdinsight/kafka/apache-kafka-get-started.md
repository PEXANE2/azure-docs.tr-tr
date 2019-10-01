---
title: Azure portal kullanarak HDInsight 'ta Apache Kafka ayarlama-hızlı başlangıç
description: Bu hızlı başlangıçta, Azure portal kullanarak Azure HDInsight 'ta Apache Kafka kümesi oluşturmayı öğreneceksiniz. Ayrıca, Kafka konular, aboneler ve tüketiciler hakkında bilgi edinebilirsiniz.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: mvc
ms.topic: quickstart
ms.date: 06/12/2019
ms.openlocfilehash: f11cbdab59548906f751116a2ca7b9c545b25d91
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71677878"
---
# <a name="quickstart-create-apache-kafka-cluster-in-azure-hdinsight-using-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak Azure HDInsight 'ta Apache Kafka kümesi oluşturma

Apache Kafka, açık kaynaklı, dağıtılmış bir akış platformudur. Bir yayımla-abone ol ileti kuyruğuna benzer işlevsellik sağladığından genellikle ileti Aracısı olarak kullanılır.

Bu hızlı başlangıçta, Azure portal kullanarak [Apache Kafka](https://kafka.apache.org) kümesi oluşturmayı öğreneceksiniz. Ayrıca, Apache Kafka kullanarak ileti göndermek ve almak için dahil edilen yardımcı programları kullanmayı öğreneceksiniz.

[!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]

Apache Kafka API 'sine yalnızca aynı sanal ağ içindeki kaynaklar erişebilir. Bu hızlı başlangıçta, SSH kullanarak kümeye doğrudan erişirsiniz. Diğer hizmetlere, ağlara veya sanal makinelere Apache Kafka bağlamak için, önce bir sanal ağ oluşturmanız ve ardından ağ içinde kaynakları oluşturmanız gerekir. Daha fazla bilgi için bkz. [sanal ağ belgesi kullanarak Apache Kafka bağlanma](apache-kafka-connect-vpn-gateway.md) .

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Prerequisites

Bir SSH istemcisi. Daha fazla bilgi için bkz. [SSH kullanarak HDInsight 'A bağlanma (Apache Hadoop)](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-an-apache-kafka-cluster"></a>Apache Kafka kümesi oluşturma

HDInsight kümesinde bir Apache Kafka oluşturmak için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com)oturum açın.

1. Sol menüden **+ kaynak oluştur** > **analiz** > **HDInsight**' a gidin.

    ![Azure portal kaynak HDInsight oluşturma](./media/apache-kafka-get-started/create-hdinsight-cluster.png)

1. **Temel bilgiler**altında, aşağıdaki değerleri girin veya seçin:

    |Özellik  |Açıklama  |
    |---------|---------|
    |Aboneliğiniz    |  Azure aboneliğinizi seçin. |
    |Kaynak grubu     | Bir kaynak grubu oluşturun veya var olan bir kaynak grubunu seçin.  Kaynak grubu, Azure bileşenlerinin bir kapsayıcısıdır.  Bu durumda, kaynak grubu HDInsight kümesini ve bağımlı Azure Depolama hesabını içerir. |
    |Küme adı   | Hadoop kümesi için bir ad girin. HDInsight 'taki tüm kümeler aynı DNS ad alanını paylaştığından, bu ad bu adın benzersiz olması gerekir. Ad, harf, sayı ve kısa çizgi gibi en fazla 59 karakter içerebilir. Adın ilk ve son karakterleri kısa çizgi olamaz. |
    |Konum    | Kümenizi oluşturmak istediğiniz Azure konumunu seçin.  Daha iyi performans için size daha yakın bir konum seçin. |
    |Küme türü| **Küme türünü seç**' i seçin. Küme türü olarak **Kafka** öğesini seçin.|
    |Version|Küme türü için varsayılan sürüm belirtilecektir. Farklı bir sürüm belirtmek istiyorsanız, açılan listeden seçim yapın.|
    |Küme oturum açma Kullanıcı adı ve parolası    | Varsayılan oturum açma adı **admin**' dir. Parola en az 10 karakter uzunluğunda olmalıdır ve en az bir rakam, bir büyük harf ve bir küçük harf, bir alfasayısal olmayan karakter (' "' \) karakterleri dışında) içermelidir. "@No__t-1" gibi ortak parolalar **sağlamadığınızdan** emin olun.|
    |Secure Shell (SSH) Kullanıcı adı | Varsayılan Kullanıcı adı **sshuser**' dır.  SSH Kullanıcı adı için başka bir ad sağlayabilirsiniz. |
    |SSH için küme oturum açma parolasını kullanma| SSH kullanıcısı için, küme oturum açma kullanıcısı için sağladınız aynı parolayı kullanmak üzere bu onay kutusunu işaretleyin.|

   ![Azure portal küme temelleri oluşturma](./media/apache-kafka-get-started/azure-portal-cluster-basics-blank.png)

    Her Azure bölgesi (konum) _hata etki alanları_sağlar. Bir hata etki alanı, bir Azure veri merkezinde temel alınan donanımın mantıksal gruplandırmasıdır. Her hata etki alanı, ortak bir güç kaynağı ve ağ anahtarını paylaşır. Bir HDInsight kümesi içindeki düğümleri uygulayan sanal makineler ve yönetilen diskler, bu hata etki alanlarına dağıtılır. Bu mimari, fiziksel donanım hatalarının olası etkisini sınırlar.

    Verilerin yüksek kullanılabilirliği için, __üç hata etki alanı__içeren bir bölge (konum) seçin. Bir bölgedeki hata etki alanlarının sayısı hakkında daha fazla bilgi için bkz. [Linux sanal makinelerinin kullanılabilirliği](../../virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) belgesi.

    Depolama ayarlarına ilerlemek için **Sonraki: depolama > >** sekmesini seçin.

1. **Depolama** sekmesinden aşağıdaki değerleri sağlayın:

    |Özellik  |Açıklama  |
    |---------|---------|
    |Birincil depolama türü|Varsayılan **Azure Storage**değerini kullanın.|
    |Seçim yöntemi|Varsayılan değer **listesinden Seç ' i**kullanın.|
    |Birincil depolama hesabı|Açılan listeyi kullanarak mevcut bir depolama hesabını seçin veya **Yeni oluştur**' u seçin. Yeni bir hesap oluşturursanız, ad 3 ila 24 karakter uzunluğunda olmalı ve yalnızca rakamlar ve küçük harfler içerebilir|
    |Kapsayıcı|Oto doldurulmuş değeri kullanın.|

    ![HDInsight Linux kullanmaya başlama küme depolama değerleri],(./media/apache-kafka-get-started/azure-portal-cluster-storage-blank.png "HDInsight kümesi oluşturmak için depolama değerleri") sağlar

    **Güvenlik + ağ** sekmesini seçin.

1. Bu hızlı başlangıç için varsayılan güvenlik ayarlarını bırakın. Enterprise Security Package hakkında daha fazla bilgi edinmek için [Azure Active Directory Domain Services kullanarak kurumsal güvenlik paketi HDInsight kümesi yapılandırma](../domain-joined/apache-domain-joined-configure-using-azure-adds.md)makalesini ziyaret edin. Apache Kafka disk şifrelemesi için kendi anahtarınızı nasıl kullanacağınızı öğrenmek için bkz. [Azure HDInsight 'ta Apache Kafka için kendi anahtarınızı getir](apache-kafka-byok.md)

   Kümenizi bir sanal ağa bağlamak isterseniz **sanal** ağ açılan menüsünden bir sanal ağ seçin.

   ![Kümeyi bir sanal ağa ekleyin](./media/apache-kafka-get-started/azure-portal-cluster-security-networking-kafka-vn.png)

    **Yapılandırma + fiyatlandırma** sekmesini seçin.

1. HDInsight üzerinde Apache Kafka kullanılabilirliğini güvence altına almak için, **çalışan düğümü** için __düğüm sayısı__ girişinin 3 veya daha büyük olarak ayarlanması gerekir. Varsayılan değer 4 ' dir.

    **Çalışan düğümü başına standart disk** girdisi, hdınsight üzerinde Apache Kafka ölçeklenebilirliğini yapılandırır. HDInsight üzerinde Apache Kafka, verileri depolamak için kümedeki sanal makinelerin yerel diskini kullanır. Apache Kafka g/ç ağır olduğundan, düğüm başına yüksek aktarım hızı ve daha fazla depolama sağlamak için [Azure yönetilen diskler](../../virtual-machines/windows/managed-disks-overview.md) kullanılır. Yönetilen disk türü __Standart__ (HDD) veya __Premium__ (SSD) olabilir. Disk türü, çalışan düğümleri tarafından kullanılan VM boyutuna bağlıdır (Apache Kafka aracıları). Premium diskler, DS ve GS serisi VM 'Leri ile otomatik olarak kullanılır. Diğer tüm VM türleri standart kullanır.

   ![Apache Kafka küme boyutunu ayarlama](./media/apache-kafka-get-started/azure-portal-cluster-configuration-pricing-kafka.png)

    **Gözden geçir + oluştur** sekmesini seçin.

1. Kümenin yapılandırmasını gözden geçirin. Yanlış olan ayarları değiştirin. Son olarak, kümeyi oluşturmak için **Oluştur** ' u seçin.

    ![Kafka kümesi yapılandırma özeti](./media/apache-kafka-get-started/azure-portal-cluster-review-create-kafka.png)

    Kümenin oluşturulması 20 dakikaya kadar sürebilir.

## <a name="connect-to-the-cluster"></a>Kümeye Bağlan

1. Apache Kafka kümesinin birincil baş düğümüne bağlanmak için aşağıdaki komutu kullanın. @No__t-0 ' yı SSH kullanıcı adıyla değiştirin. @No__t-0 ' yı Apache Kafkacluster adıyla değiştirin.

    ```bash
    ssh sshuser@mykafka-ssh.azurehdinsight.net
    ```

2. Kümeye ilk kez bağlandığınızda, SSH istemciniz konağın orijinalliğini kuramadığına ilişkin bir uyarı gösterebilir. İstendiğinde, __Evet__yazın ve ardından __ENTER__ tuşuna basarak Konağı SSH istemcinizin güvenilen sunucu listesine ekleyin.

3. İstendiğinde, SSH kullanıcısının parolasını girin.

    Bağlandıktan sonra aşağıdaki metne benzer bilgiler görürsünüz:
    
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

## <a id="getkafkainfo"></a>Apache Zookeeper ve aracı ana bilgisayar bilgilerini al

Kafka ile çalışırken *Apache Zookeeper* ve *Aracı* konaklarınızı bilmeniz gerekir. Bu Konaklar Apache Kafka API 'SI ve Kafka ile birlikte gelen birçok yardımcı program ile kullanılır.

Bu bölümde, küme üzerindeki Apache ambarı REST API ana bilgisayar bilgilerini alırsınız.

1. Komut satırı JSON işlemcisi olan [JQ](https://stedolan.github.io/jq/)'yi yükler. Bu yardımcı program JSON belgelerini ayrıştırmak için kullanılır ve ana bilgisayar bilgilerini Ayrıştırmada faydalıdır. Açık SSH bağlantısından, yüklemek için aşağıdaki komutu girin `jq`:

    ```bash
    sudo apt -y install jq
    ```

2. Ortam değişkenlerini ayarlayın. @No__t-0 ve `CLUSTERNAME` ' i sırasıyla küme oturum açma parolasıyla ve küme adıyla değiştirin, ardından şu komutu girin:

    ```bash
    export password='PASSWORD'
    export clusterNameA='CLUSTERNAME'
    ```

3. Doğru şekilde oluşturulmuş küme adını ayıklayın. Kümenin nasıl oluşturulduğuna bağlı olarak, küme adının gerçek büyük küçük harfleri beklediğinizden farklı olabilir. Bu komut, gerçek büyük/küçük harf elde eder, bir değişkende depolar ve ardından doğru şekilde, daha önce verdiğiniz adı görüntüler. Aşağıdaki komutu girin:

    ```bash
    export clusterName=$(curl -u admin:$password -sS -G "https://$clusterNameA.azurehdinsight.net/api/v1/clusters" | jq -r '.items[].Clusters.cluster_name')
    echo $clusterName, $clusterNameA
    ```

4. Bir ortam değişkenini Zookeeper ana bilgisayar bilgileriyle ayarlamak için aşağıdaki komutu kullanın. Komut tüm Zookeeper Konakları alır, ardından yalnızca ilk iki girişi döndürür. Bunun nedeni, tek bir konağın ulaşılamaz olması durumunda bazı yedekliliğe sahip olmanız istedir.

    ```bash
    export KAFKAZKHOSTS=`curl -sS -u admin:$password -G http://headnodehost:8080/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    ```

    Bu komut, küme baş düğümündeki ambarı hizmetini doğrudan sorgular. Ayrıca, `https://$CLUSTERNAME.azurehdinsight.net:80/` ' ın genel adresini kullanarak, ambarı da erişebilirsiniz. Bazı ağ yapılandırmalarının genel adrese erişimi engelleyebilir. Örneğin, bir sanal ağdaki HDInsight 'a erişimi kısıtlamak için ağ güvenlik grupları (NSG) kullanma.

5. Ortam değişkeninin doğru şekilde ayarlandığını doğrulamak için şu komutu kullanın:

    ```bash
    echo $KAFKAZKHOSTS
    ```

    Bu komut aşağıdaki metne benzer bilgiler döndürür:

    `zk0-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk2-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181`

6. Bir ortam değişkenini Apache Kafka Broker ana bilgisayar bilgileriyle ayarlamak için aşağıdaki komutu kullanın:

    ```bash
    export KAFKABROKERS=`curl -sS -u admin:$password -G http://headnodehost:8080/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    ```

7. Ortam değişkeninin doğru şekilde ayarlandığını doğrulamak için şu komutu kullanın:

    ```bash   
    echo $KAFKABROKERS
    ```

    Bu komut aşağıdaki metne benzer bilgiler döndürür:

    `wn1-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092`

## <a name="manage-apache-kafka-topics"></a>Apache Kafka yönetme konuları

Kafka, veri akışlarını *konular*halinde depolar. @No__t-0 yardımcı programını kullanarak konuları yönetebilirsiniz.

* **Bir konu oluşturmak IÇIN**SSH bağlantısında aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
    ```

    Bu komut, `$KAFKAZKHOSTS` ' da depolanan konak bilgilerini kullanarak Zookeeper 'e bağlanır. Sonra **Test**adlı bir Apache Kafka konusu oluşturur.

    * Bu konu başlığında depolanan veriler sekiz bölüme göre bölümlenmiştir.

    * Her bölüm kümedeki üç çalışan düğümü arasında çoğaltılır.

        Kümeyi, üç hata etki alanı sağlayan bir Azure bölgesinde oluşturduysanız, yineleme faktörü 3 ' ü kullanın. Aksi takdirde, 4 çoğaltma faktörünü kullanın.
        
        Üç hata etki alanı olan bölgelerde, yineleme faktörü 3 ' ün hata etki alanlarına yayılmasını sağlar. İki hata etki alanına sahip bölgelerde, çoğaltmaları etki alanları genelinde eşit olarak yayın.
        
        Bir bölgedeki hata etki alanlarının sayısı hakkında daha fazla bilgi için bkz. [Linux sanal makinelerinin kullanılabilirliği](../../virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) belgesi.

        Apache Kafka, Azure hata etki alanlarının farkında değildir. Konular için bölüm çoğaltmaları oluştururken, çoğaltmaları yüksek kullanılabilirlik için düzgün bir şekilde dağıtmayabilir.

        Yüksek kullanılabilirlik sağlamak için [Apache Kafka bölüm yeniden dengeleme aracını](https://github.com/hdinsight/hdinsight-kafka-tools)kullanın. Bu aracın, Apache Kafka kümenizin baş düğümüne yönelik bir SSH bağlantısından çalıştırılmalıdır.

        Apache Kafka verilerinizin en yüksek kullanılabilirliği için, şu durumlarda konu başlığı için bölüm çoğaltmalarını yeniden dengelemeniz gerekir:

        * Yeni bir konu veya bölüm oluşturursunuz

        * Bir kümenin ölçeğini ölçeklendirir

* **Konuları listelemek için**aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
    ```

    Bu komut Apache Kafka kümesinde bulunan konuları listeler.

* **Bir konuyu silmek için**aşağıdaki komutu kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --delete --topic topicname --zookeeper $KAFKAZKHOSTS
    ```

    Bu komut `topicname` adlı konuyu siler.

    > [!WARNING]  
    > Daha önce oluşturulan `test` konusunu silerseniz, yeniden oluşturmanız gerekir. Bu belgenin ilerleyen bölümlerinde adımlar tarafından kullanılır.

@No__t-0 yardımcı programıyla kullanılabilen komutlar hakkında daha fazla bilgi için aşağıdaki komutu kullanın:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh
```

## <a name="produce-and-consume-records"></a>Kayıtları üretme ve kullanma

Kafka *kayıtları kayıt* halinde depolar. Kayıtlar, *üreticileri*tarafından üretilir ve *tüketiciler*tarafından kullanılır. Üreticileri ve tüketiciler *Kafka Broker* hizmeti ile iletişim kurar. HDInsight kümenizdeki her çalışan düğümü bir Apache Kafka Broker ana bilgisayarı.

Daha önce oluşturduğunuz test konusunun kayıtlarını depolamak ve ardından bir tüketici kullanarak bunları okumak için aşağıdaki adımları kullanın:

1. Konuya kayıt yazmak için SSH bağlantısından `kafka-console-producer.sh` yardımcı programını kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test
    ```

    Bu komuttan sonra boş bir satıra ulaşrsınız.

2. Boş satıra bir kısa mesaj yazın ve ENTER tuşuna basın. Bu şekilde birkaç ileti girin ve sonra normal istemine dönmek için **CTRL + C** tuşlarını kullanın. Her satır, Apache Kafka konusuna ayrı bir kayıt olarak gönderilir.

3. Konudan kayıtları okumak için SSH bağlantısından `kafka-console-consumer.sh` yardımcı programını kullanın:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic test --from-beginning
    ```

    Bu komut, konudan kayıtları alır ve görüntüler. @No__t-0 kullanmak tüketiciye akışın başından başlamasını söyler, bu nedenle tüm kayıtlar alınır.

    Eski bir Kafka sürümü kullanıyorsanız, `--bootstrap-server $KAFKABROKERS` ' ı `--zookeeper $KAFKAZKHOSTS` ile değiştirin.

4. Tüketiciyi durdurmak için __CTRL + C__ tuşlarını kullanın.

Ayrıca program aracılığıyla üreticileri ve tüketicileri de oluşturabilirsiniz. Bu API 'nin kullanımıyla ilgili bir örnek için bkz. [HDInsight Ile tüketici API 'si Apache Kafka üreticisi](apache-kafka-producer-consumer-api.md) belgesi.

## <a name="clean-up-resources"></a>Kaynakları Temizleme

Bu hızlı başlangıç tarafından oluşturulan kaynakları temizlemek için kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, ilişkili HDInsight kümesini ve kaynak grubuyla ilişkili diğer tüm kaynakları da siler.

Azure portal kullanarak kaynak grubunu kaldırmak için:

1. Azure portal, sol taraftaki menüyü genişleterek hizmet menüsünü açın ve kaynak __grupları__ ' nı seçerek kaynak gruplarınızın listesini görüntüleyin.
2. Silinecek kaynak grubunu bulun ve ardından listenin sağ tarafındaki __daha fazla__ düğmesine (...) sağ tıklayın.
3. __Kaynak grubunu sil__' i seçip onaylayın.

> [!WARNING]  
> HDInsight kümesi faturalandırma, küme oluşturulduğunda başlar ve küme silindiğinde duraklar. Faturalandırma, dakika başına dağıtılır, bu nedenle artık kullanımda olmadığında kümenizi silmeniz gerekir.
>
> HDInsight kümesinde Apache Kafka silindiğinde, Kafka içinde depolanan tüm veriler silinir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Apache Kafka ile Apache Spark kullanma](../hdinsight-apache-kafka-spark-structured-streaming.md)
