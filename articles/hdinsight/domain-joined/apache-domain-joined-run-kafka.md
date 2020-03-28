---
title: Öğretici - Apache Kafka & Kurumsal Güvenlik - Azure HDInsight
description: Öğretici - Kurumsal Güvenlik Paketi ile Azure HDInsight'ta Kafka için Apache Ranger politikalarını nasıl yapılandıracağınızı öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: tutorial
ms.date: 09/04/2019
ms.openlocfilehash: 58c5b3bdd6d50f2e512cccffe78bd4e70805d729
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78204744"
---
# <a name="tutorial-configure-apache-kafka-policies-in-hdinsight-with-enterprise-security-package-preview"></a>Öğretici: Kurumsal Güvenlik Paketi ile HDInsight Apache Kafka politikaları yapılandırma (Önizleme)

Kurumsal Güvenlik Paketi (ESP) Apache Kafka kümeleri için Apache Ranger politikalarını nasıl yapılandıracağınızı öğrenin. ESP kümeleri bir etki alanına bağlıdır ve kullanıcıların etki alanı kimlik bilgileriyle kimlik doğrulaması yapmasına olanak sağlar. Bu öğreticide, `sales` ve `marketingspend` konularına erişimi kısıtlamak için iki Ranger ilkesi oluşturacaksınız.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Etki alanı kullanıcılarını oluşturma
> * Ranger ilkelerini oluşturma
> * Kafka kümesinde konu oluşturma
> * Ranger ilkelerini test etme

## <a name="prerequisite"></a>Önkoşul

[Kurumsal Güvenlik Paketi ile hdinsight Kafka küme](./apache-domain-joined-configure-using-azure-adds.md).

## <a name="connect-to-apache-ranger-admin-ui"></a>Apache Ranger Yönetici Arabirimine bağlanma

1. Bir tarayıcıdan, `https://ClusterName.azurehdinsight.net/Ranger/` URL’sini kullanarak Ranger Yönetici kullanıcı arabirimine bağlanın. `ClusterName` öğesini Kafka kümenizin adıyla değiştirmeyi unutmayın. Ranger kimlik bilgileri Hadoop kümesi kimlik bilgileriyle aynı değildir. Tarayıcıların ön belleğe alınmış Hadoop kimlik bilgilerini kullanmasını önlemek için Ranger Yönetici Arabirimine yeni bir InPrivate tarayıcı penceresinden bağlanın.

2. Azure Active Directory (AD) yönetici kimlik bilgilerinizi kullanarak oturum açın. Azure AD yönetici kimlik bilgileri HDInsight küme kimlik bilgileri veya Linux HDInsight düğümü SSH kimlik bilgileriyle aynı değildir.

   ![HDInsight Apache Ranger Yönetici UI](./media/apache-domain-joined-run-kafka/apache-ranger-admin-login.png)

## <a name="create-domain-users"></a>Etki alanı kullanıcılarını oluşturma

**sales_user** ve **marketing_user** etki alanı kullanıcılarını nasıl oluşturacağınızı öğrenmek için [Kurumsal Güvenlik Paketi ile HDInsight kümesi oluşturma](./apache-domain-joined-configure-using-azure-adds.md) bölümünü ziyaret edin. Bir üretim senaryosunda, etki alanı kullanıcıları Active Directory kiracınızdan gelir.

## <a name="create-ranger-policy"></a>Ranger ilkesini oluşturma

**sales_user** ve **marketing_user** için Ranger ilkesi oluşturun.

1. **Ranger Yönetici Arabirimini** açın.

2. **Kafka**altında ** \<ClusterName>_kafka'ı** seçin. Bir önceden yapılandırılmış ilke listelenebilir.

3. **Yeni İlke Ekle'yi** seçin ve aşağıdaki değerleri girin:

   |Ayar  |Önerilen değer  |
   |---------|---------|
   |İlke Adı  |  hdi satış* ilkesi   |
   |Konu başlığı   |  satış* |
   |Kullanıcı Seçin  |  sales_user1 |
   |İzinler  | yayımlama, kullanma, oluşturma |

   Konu adında şu joker karakterler bulunabilir:

   * ’*’ karakterlerin sıfır veya daha fazla kez geçtiğini gösterir.
   * ’?‘ tek karakteri gösterir.

   ![Apache Ranger Yönetici UI Create Policy1](./media/apache-domain-joined-run-kafka/apache-ranger-admin-create-policy.png)

   **Select User** için bir etki alanı kullanıcısı otomatik olarak doldurulmazsa, Ranger’ın Azure AD ile eşitlenmesi için birkaç dakika bekleyin.

4. İlkeyi kaydetmek için **Ekle'yi** seçin.

5. **Yeni İlke Ekle'yi** seçin ve ardından aşağıdaki değerleri girin:

   |Ayar  |Önerilen değer  |
   |---------|---------|
   |İlke Adı  |  hdi pazarlama ilkesi   |
   |Konu başlığı   |  marketingspend |
   |Kullanıcı Seçin  |  marketing_user1 |
   |İzinler  | yayımlama, kullanma, oluşturma |

   ![Apache Ranger Yönetici UI Create Policy2](./media/apache-domain-joined-run-kafka/apache-ranger-admin-create-policy-2.png)  

6. İlkeyi kaydetmek için **Ekle'yi** seçin.

## <a name="create-topics-in-a-kafka-cluster-with-esp"></a>ESP ile Kafka kümesinde konu oluşturma

İki konu oluşturmak `salesevents` `marketingspend`ve:

1. Kümeye SSH bağlantısı açmak için aşağıdaki komutu kullanın:

   ```cmd
   ssh DOMAINADMIN@CLUSTERNAME-ssh.azurehdinsight.net
   ```

   Küme `DOMAINADMIN` [oluşturma](./apache-domain-joined-configure-using-azure-adds.md#create-an-hdinsight-cluster-with-esp)sırasında yapılandırılan kümeniz için yönetici `CLUSTERNAME` kullanıcıyla değiştirin ve kümenizin adıyla değiştirin. İstenirse, yönetici kullanıcı hesabının parolasını girin. HDInsight ile `SSH` kullanma hakkında daha fazla bilgi için bkz. [HDInsight ile SSH kullanma](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

2. Küme adını bir değişkene kaydedip JSON ayrıştırma yardımcı programını (`jq`) yüklemek için aşağıdaki komutları kullanın. İstendiğinde, Kafka kümesi adını girin.

   ```bash
   sudo apt -y install jq
   read -p 'Enter your Kafka cluster name:' CLUSTERNAME
   ```

3. Kafka broker ev sahiplerini almak için aşağıdaki komutları kullanın. İstendiğinde, küme yöneticisi hesabı için parolayı girin.

   ```bash
   export KAFKABROKERS=`curl -sS -u admin -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`; \
   ```

   Devam etmeden önce, geliştirme ortamınızı ayarlamanız gerekebilir. Java JDK, Apache Maven ve scp içeren bir SSH istemcisi gibi bileşenlere ihtiyacınız olacaktır. Daha fazla bilgi için [kurulum yönergelerine](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/tree/master/DomainJoined-Producer-Consumer)bakın.

1. [Apache Kafka etki alanına katılmış üretici tüketici örneklerini](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started/tree/master/DomainJoined-Producer-Consumer) indirin.

1. [Öğretici: Apache Kafka Üretici ve Tüketici API’lerini kullanma](../kafka/apache-kafka-producer-consumer-api.md#build-and-deploy-the-example) sayfasının **Örneği derleme ve dağıtma** bölümündeki 2 ve 3 numaralı adımları izleyin.

1. Aşağıdaki komutları çalıştırın:

   ```bash
   java -jar -Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf kafka-producer-consumer.jar create salesevents $KAFKABROKERS
   java -jar -Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf kafka-producer-consumer.jar create marketingspend $KAFKABROKERS
   ```

## <a name="test-the-ranger-policies"></a>Ranger ilkelerini test etme

Yapılandırılan Ranger politikalarına göre, **sales_user** konu `salesevents` üretebilir/tüketebilir, ancak konu `marketingspend`oluşturamaz. Tersine, **marketing_user** konu `marketingspend` üretebilir/tüketebilir, `salesevents`ancak konuyu tüketemez.

1. Kümeye yeni bir SSH bağlantısı açın. **sales_user1** olarak oturum açmak için aşağıdaki komutu kullanın:

   ```bash
   ssh sales_user1@CLUSTERNAME-ssh.azurehdinsight.net
   ```

2. Aşağıdaki komutu yürütün:

   ```bash
   export KAFKA_OPTS="-Djava.security.auth.login.config=/usr/hdp/current/kafka-broker/config/kafka_client_jaas.conf"
   ```

3. Aşağıdaki ortam değişkenini ayarlamak için önceki bölümdeki broker adlarını kullanın:

   ```bash
   export KAFKABROKERS=<brokerlist>:9092
   ```

   Örnek: `export KAFKABROKERS=wn0-khdicl.contoso.com:9092,wn1-khdicl.contoso.com:9092`

4. Yapı altında Adım 3'i izleyin ve Öğretici'deki **örneği dağıtın:** [Apache Kafka Üretici ve Tüketici API'lerini kullanarak](../kafka/apache-kafka-producer-consumer-api.md#build-and-deploy-the-example) `kafka-producer-consumer.jar` **sales_user'nın**da kullanılabilir olduğundan emin olun.

**Not: Bu öğretici için lütfen "DomainJoined-Producer-Consumer" projesi altında kafka-producer-consumer.jar kullanın (alan adı olmayan birleştirilmiş senaryolar için olan Üretici-Tüketici projesi kapsamındaki projeyi değil).**

5. Aşağıdaki **sales_user1** komutu çalıştırarak `salesevents` sales_user1 konuya üretebileceğini doğrulayın:

   ```bash
   java -jar kafka-producer-consumer.jar producer salesevents $KAFKABROKERS
   ```

6. Konu `salesevents`dan tüketmek için aşağıdaki komutu çalıştırın:

   ```bash
   java -jar kafka-producer-consumer.jar consumer salesevents $KAFKABROKERS
   ```

   İletileri okuyabildiğinizi doğrulayın.

7. **Sales_user1'nin** aynı ssh penceresinde `marketingspend` aşağıdakileri çalıştırarak konuya üretilemediğini doğrulayın:

   ```bash
   java -jar kafka-producer-consumer.jar producer marketingspend $KAFKABROKERS
   ```

   Bir yetkilendirme hatası oluşur ve bu yok sayılabilir.

8. **marketing_user1'ın** bu konudan `salesevents`tüketilememesine dikkat edin.

   Yukarıdaki adımları 1-4 tekrarlayın, ama **marketing_user1**olarak bu kez .

   Konu `salesevents`dan tüketmek için aşağıdaki komutu çalıştırın:

   ```bash
   java -jar kafka-producer-consumer.jar consumer salesevents $KAFKABROKERS
   ```

   Önceki iletiler görülemez.

9. Ranger kullanıcı arabiriminden denetim erişimi olaylarını görüntüleyin.

   ![Ranger UI ilke denetimi erişim olayları ](./media/apache-domain-joined-run-kafka/apache-ranger-admin-audit.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz, oluşturduğunuz Kafka kümesini aşağıdaki adımlarla silin:

1. [Azure portalında](https://portal.azure.com/)oturum açın.
1. Üstteki **Arama** kutusuna **HDInsight**yazın.
1. **Hizmetler**altında **HDInsight kümelerini** seçin.
1. Görünen HDInsight kümeleri listesinde, bu öğretici için oluşturduğunuz kümenin yanındaki **...** 'yı tıklatın. 
1. **Sil'i**tıklatın. **Evet'i**tıklatın.

## <a name="troubleshooting"></a>Sorun giderme
Kafka-producer-consumer.jar bir etki alanında çalışmıyorsa, lütfen "DomainJoined-Producer-Consumer" projesi kapsamında kafka-producer-consumer.jar'ı kullandığınızdan emin olun (Alan adı olmayan Üretici-Tüketici projesi kapsamındaki proje birleştirilmiş senaryolar).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Müşteri tarafından yönetilen anahtar disk şifrelemesi](../disk-encryption.md)
