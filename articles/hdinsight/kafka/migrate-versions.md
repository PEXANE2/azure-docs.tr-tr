---
title: Apache Kafka iş yüklerini Azure HDInsight 'a geçirme 4,0
description: HDInsight 3,6 ' de Apache Kafka iş yüklerini HDInsight 4,0 ' ye geçirmeyi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 12/18/2019
ms.openlocfilehash: d9ad5da27b4b5f8e4e447036c46613bad0f1f5c7
ms.sourcegitcommit: 5925df3bcc362c8463b76af3f57c254148ac63e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75563794"
---
# <a name="migrate-apache-kafka-workloads-to-azure-hdinsight-40"></a>Apache Kafka iş yüklerini Azure HDInsight 'a geçirme 4,0

Azure HDInsight 4,0, en son açık kaynaklı bileşenleri performans, bağlantı ve güvenlik açısından önemli geliştirmelerle sunmaktadır. Bu belgede HDInsight 3,6 ' de HDInsight 4,0 ' de Apache Kafka iş yüklerinin nasıl geçirileceği açıklanmaktadır. İş yüklerinizi HDInsight 4,0 ' e geçirdikten sonra, HDInsight 3,6 ' de kullanılamayan yeni özelliklerden birçoğunu kullanabilirsiniz.

## <a name="hdinsight-36-kafka-migration-paths"></a>HDInsight 3,6 Kafka geçiş yolları

HDInsight 3,6 iki Kafka sürümü destekler: 1.0.0 ve 1.1.0. HDInsight 4,0, 1.1.0 ve 2.1.0 sürümlerini destekler. Hangi Kafka sürümüne ve hangi HDInsight sürümünü çalıştırmak istediğinize bağlı olarak, birden çok desteklenen geçiş yolu vardır. Bu yollar aşağıda açıklanmıştır ve aşağıdaki diyagramda gösterilmiştir.

* **Kafka ve HDInsight 'ı en son sürümlerde Çalıştır (önerilir)** : HDInsight 3,6 ve Kafka 1.0.0 ya da 1.1.0 uygulamasını HDInsight 4,0 ile Kafka 2.1.0 (yollar D ve aşağıdaki E) ile geçirin.
* **HDInsight 'ı en son sürümde çalıştırın, ancak yalnızca daha yeni bir sürümde Kafka**: bir HDInsight 3,6 ve Kafka 1.0.0 uygulamasını Kafka 1.1.0 ile HDInsight 4,0 'e geçirin (aşağıdaki yol B).
* **HDInsight 'ı en son sürümde çalıştırın, Kafka sürümünü koruyun**: HDInsight 3,6 ve Kafka 1.1.0 uygulamasını Kafka 1.1.0 ile HDInsight 4,0 'e geçirin (aşağıdaki yol C).
* **Daha yeni bir sürümde Kafka çalıştırın, HDInsight sürümünü koruyun**: bir Kafka 1.0.0 uygulamasını 1.1.0 'e geçirin ve HDInsight 3,6 (yol aşağıda) kalın. Bu seçeneğin hala yeni bir küme dağıtımına gerek olacağını unutmayın. Kafka sürümünün mevcut bir kümede yükseltilmesi desteklenmez. İstediğiniz sürümle bir küme oluşturduktan sonra, Kafka istemcilerinizi yeni kümeyi kullanacak şekilde geçirin.

![3,6 üzerindeki Apache Kafka için yükseltme yolları](./media/upgrade-threesix-to-four/apache-kafka-upgrade-path.png)

## <a name="apache-kafka-versions"></a>Apache Kafka sürümleri

### <a name="kafka-110"></a>Kafka 1.1.0
  
Kafka 1.0.0 'den 1.1.0 'e geçiş yaparsanız aşağıdaki yeni özelliklerden yararlanabilirsiniz:

* Kafka denetleyicisinde yapılan iyileştirmeler, denetimli kapatmadan sonra aracıları yeniden başlatabilir ve sorunlardan daha hızlı kurtulabilir. 
* [Fetchrequests mantığındaki](https://issues.apache.org/jira/browse/KAFKA-6254) geliştirmeler, kümede daha fazla bölüm (ve bu nedenle daha fazla konu) olmasını sağlar. 
* Kafka Connect, Konular için [kayıt üst bilgilerini](https://issues.apache.org/jira/browse/KAFKA-5142) ve [Normal ifadeleri](https://issues.apache.org/jira/browse/KAFKA-3073) destekler. 

Güncelleştirmelerin tüm listesi için, bkz. [Apache Kafka 1,1 sürüm notları](https://archive.apache.org/dist/kafka/1.1.0/RELEASE_NOTES.html).

### <a name="apache-kafka-210"></a>Apache Kafka 2.1.0

Kafka 2,1 ' ye geçiş yaparsanız, aşağıdaki özelliklerden faydalanabilirsiniz:

* İyileştirilmiş bir çoğaltma Protokolü nedeniyle daha iyi aracı dayanıklılığı.
* KafkaAdminClient API 'sindeki yeni işlevsellik.
* Yapılandırılabilir Kota yönetimi.
* Zstandart sıkıştırma desteği.

Güncelleştirmelerin tüm listesi için, bkz. [Apache Kafka 2,0 sürüm notları](https://archive.apache.org/dist/kafka/2.0.0/RELEASE_NOTES.html) ve [Apache Kafka 2,1 sürüm notları](https://archive.apache.org/dist/kafka/2.1.0/RELEASE_NOTES.html).

## <a name="kafka-client-compatibility"></a>Kafka istemci uyumluluğu

Yeni Kafka aracıları eski istemcileri destekler. [TLA-35-alma Protokolü sürümü](https://cwiki.apache.org/confluence/display/KAFKA/KIP-35+-+Retrieving+protocol+version) , bir Kafka aracısının işlevlerini dinamik olarak belirlemek için bir mekanizma sunmuştur ve [TLA-97: IYILEŞTIRILMIŞ Kafka Istemci RPC uyumluluk ilkesi](https://cwiki.apache.org/confluence/display/KAFKA/KIP-97%3A+Improved+Kafka+Client+RPC+Compatibility+Policy) , Java istemcisi için yeni bir uyumluluk ilkesi ve garantisi getirmiştir. Daha önce, bir Kafka istemcisinin aynı sürümün veya daha yeni bir sürümün aracısıyla etkileşim kurması gerekiyordu. Artık Java istemcilerinin ve `librdkafka` gibi TLA-35 ' i destekleyen diğer istemcilerin daha yeni sürümleri, daha eski istek türlerine geri dönebilir veya işlevler kullanılamıyorsa uygun hatalar oluşturabilir.

![Yükseltme Kafka istemci uyumluluğu](./media/upgrade-threesix-to-four/apache-kafka-client-compatibility.png)

İstemcinin eski aracıları desteklediğine yönelik olduğunu unutmayın.  Daha fazla bilgi için bkz. [uyumluluk matrisi](https://cwiki.apache.org/confluence/display/KAFKA/Compatibility+Matrix).

## <a name="general-migration-process"></a>Genel geçiş işlemi

Aşağıdaki geçiş kılavuzunda, tek bir sanal ağda HDInsight 3,6 ' de dağıtılan bir Apache Kafka 1.0.0 veya 1.1.0 kümesi varsayılmaktadır. Mevcut aracıda bazı konuları vardır ve üreticileri ve tüketiciler tarafından etkin bir şekilde kullanılır.

![Geçerli Kafka mış ortam](./media/upgrade-threesix-to-four/apache-kafka-presumed-environment.png)

Geçişi gerçekleştirmek için aşağıdaki adımları uygulayın:

1. **Test için yeni bir HDInsight 4,0 kümesi ve istemcileri dağıtın.** Yeni bir HDInsight 4,0 Kafka kümesi dağıtın. Birden çok Kafka küme sürümü seçilebiliyorsanız, en son sürümü seçmeniz önerilir. Dağıtımdan sonra bazı parametreleri gerektiği şekilde ayarlayın ve var olan ortamınız ile aynı şekilde bir konu oluşturun. Ayrıca, SSL 'yi ayarlayın ve gerektiğinde kendi anahtarını getir (BYOK) şifrelemesini yapın. Ardından, yeni kümeyle düzgün çalışıp çalışmadığını denetleyin.

    ![Yeni HDInsight 4,0 kümelerini dağıtma](./media/upgrade-threesix-to-four/deploy-new-hdinsight-clusters.png)

1. **Üretici uygulaması için kümeyi değiştirin ve tüm kuyruk verileri geçerli tüketicilerle tüketilene kadar bekleyin.** Yeni HDInsight 4,0 Kafka kümesi hazırsanız, mevcut üretici hedefini yeni kümeye geçirin. Mevcut tüketici uygulaması var olan kümeden tüm verileri tüketene kadar bunu olduğu gibi bırakın.

    ![Üretici uygulaması için küme değiştirme](./media/upgrade-threesix-to-four/switch-cluster-producer-app.png)

1. **Tüketici uygulamasındaki kümeyi değiştirin.** Mevcut tüketici uygulamasının var olan kümeden tüm verileri tükettiğini doğruladıktan sonra, bağlantıyı yeni kümeye geçirin.

    ![Tüketici uygulamasında kümeyi değiştirme](./media/upgrade-threesix-to-four/switch-cluster-consumer-app.png)

1. **Eski kümeyi ve test uygulamalarını gerektiği şekilde kaldırın.** Anahtar tamamlandıktan ve düzgün şekilde çalışmaya başladıktan sonra, sınamada kullanılan eski HDInsight 3,6 Kafka kümesini ve üreticileri ve tüketicilerini gerektiği şekilde kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Kafka HDInsight kümeleri için performans iyileştirme](apache-kafka-performance-tuning.md)
* [Hızlı başlangıç: Azure portal kullanarak Azure HDInsight 'ta Apache Kafka kümesi oluşturma](apache-kafka-get-started.md)