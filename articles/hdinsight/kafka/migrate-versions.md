---
title: Apache Kafka iş yüklerini Azure HDInsight 4.0'a geçirin
description: HDInsight 3.6'daki Apache Kafka iş yüklerini HDInsight 4.0'a nasıl geçireceğinizi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 12/18/2019
ms.openlocfilehash: 66ea8a93723c117a364a188ec05b050e1ef60564
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76548093"
---
# <a name="migrate-apache-kafka-workloads-to-azure-hdinsight-40"></a>Apache Kafka iş yüklerini Azure HDInsight 4.0'a geçirin

Azure HDInsight 4.0, performans, bağlantı ve güvenlik açısından önemli geliştirmelerle en son açık kaynak bileşenlerini sunar. Bu belge, HdInsight 3.6'daki Apache Kafka iş yüklerinin NASıL HDInsight 4.0'a geçirilen yollarla açıklanmaktadır. İş yüklerinizi HDInsight 4.0'a geçtikten sonra, HDInsight 3.6'da bulunmayan birçok yeni özelliği kullanabilirsiniz.

## <a name="hdinsight-36-kafka-migration-paths"></a>HDInsight 3.6 Kafka göç yolları

HDInsight 3.6 Kafka'nın iki versiyonunu destekler: 1.0.0 ve 1.1.0. HDInsight 4.0 1.1.0 ve 2.1.0 sürümlerini destekler. Kafka'nın hangi sürümünü ve HDInsight'ın hangi sürümünü çalıştırmak istediğinize bağlı olarak, birden çok desteklenen geçiş yolu vardır. Bu yollar aşağıda açıklanmıştır ve aşağıdaki diyagramda gösterilmiştir.

* **En son sürümler de Kafka ve HDInsight çalıştırın (önerilen)**: Kafka 2.1.0 (aşağıdaki yollar D ve E) ile HDInsight 4.0'a HDInsight 3.6 ve Kafka 1.0.0 veya 1.1.0 uygulamasını geçirin.
* **HDInsight'ı en son sürümde çalıştırın, ancak Kafka'yı yalnızca daha yeni bir sürümde**çalıştırın : HDInsight 3.6 ve Kafka 1.0.0 uygulamasını Kafka 1.1.0 (aşağıdaki B yolu) ile HDInsight 4.0'a geçirin.
* **HDInsight'ı en son sürümde çalıştırın, Kafka sürümünü koruyun**: HDInsight 3.6 ve Kafka 1.1.0 uygulamasını Kafka 1.1.0 (aşağıdaki C yolu) ile HDInsight 4.0'a geçirin.
* **Kafka'yı daha yeni bir sürümde çalıştırın, HDInsight sürümünü koruyun:** Kafka 1.0.0 uygulamasını 1.1.0'a geçirin ve HDInsight 3.6'da kalın (aşağıdaki A yolu). Bu seçeneğin yine de yeni bir küme dağıtmayı gerektireceğini unutmayın. Varolan bir kümedeki Kafka sürümünün yükseltilmesi desteklenmez. İstediğiniz sürümü içeren bir küme oluşturduktan sonra, kafka istemcilerinizi yeni kümeyi kullanmak üzere geçirin.

![3.6'da Apache Kafka için yükseltme yolları](./media/upgrade-threesix-to-four/apache-kafka-upgrade-path.png)

## <a name="apache-kafka-versions"></a>Apache Kafka versiyonları

### <a name="kafka-110"></a>Kafka 1.1.0
  
Kafka 1.0.0'dan 1.1.0'a geçiş yapacaksanız aşağıdaki yeni özelliklerden yararlanabilirsiniz:

* Kafka denetleyicisindeki geliştirmeler kontrollü kapatmayı hızlandırabilir, böylece brokerları yeniden başlatabilir ve sorunlardan daha hızlı kurtulabilirsiniz. 
* Kümede daha fazla bölüm (ve dolayısıyla daha fazla konu) olmasını sağlayan [FetchRequests mantığındaki](https://issues.apache.org/jira/browse/KAFKA-6254) geliştirmeler. 
* Kafka Connect, [kayıt üstbilgileri](https://issues.apache.org/jira/browse/KAFKA-5142) ve konular için [düzenli ifadeleri](https://issues.apache.org/jira/browse/KAFKA-3073) destekler. 

Güncellemelerin tam listesi için [Apache Kafka 1.1 sürüm notlarına](https://archive.apache.org/dist/kafka/1.1.0/RELEASE_NOTES.html)bakın.

### <a name="apache-kafka-210"></a>Apaçi Kafka 2.1.0

Kafka 2.1'e göç ederseniz, aşağıdaki özelliklerden yararlanabilirsiniz:

* Geliştirilmiş çoğaltma protokolü sayesinde daha iyi broker esnekliği.
* KafkaAdminClient API'de yeni işlevsellik.
* Yapılandırılabilir kota yönetimi.
* Zstandard sıkıştırma desteği.

Güncellemelerin tam listesi için [Apache Kafka 2.0 sürüm notlarına](https://archive.apache.org/dist/kafka/2.0.0/RELEASE_NOTES.html) ve [Apache Kafka 2.1 sürüm notlarına](https://archive.apache.org/dist/kafka/2.1.0/RELEASE_NOTES.html)bakın.

## <a name="kafka-client-compatibility"></a>Kafka istemci uyumluluğu

Yeni Kafka brokerları eski müşterileri destekler. [KIP-35 - Alma protokolü sürümü,](https://cwiki.apache.org/confluence/display/KAFKA/KIP-35+-+Retrieving+protocol+version) Kafka broker ve [KIP-97'nin](https://cwiki.apache.org/confluence/display/KAFKA/KIP-97%3A+Improved+Kafka+Client+RPC+Compatibility+Policy) işlevselliğini dinamik olarak belirleyen bir mekanizma getirmiştir: Geliştirilmiş Kafka Client RPC Uyumluluk Politikası, Java istemcisi için yeni bir uyumluluk politikası ve garantiler getirmiştir. Daha önce, bir Kafka istemcisi aynı sürümü veya daha yeni bir sürümü bir komisyoncu ile etkileşim vardı. Şimdi, Java istemcileri ve KIP-35 gibi `librdkafka` destekleyen diğer istemcilerin yeni sürümleri eski istek türlerine geri düşebilir veya işlevsellik yoksa uygun hataları atabilir.

![Kafka istemci uyumluluğunu yükseltme](./media/upgrade-threesix-to-four/apache-kafka-client-compatibility.png)

Bu, istemcinin eski brokerları desteklediği anlamına gelmediğini unutmayın.  Daha fazla bilgi için Uyumluluk [Matrisi'ne](https://cwiki.apache.org/confluence/display/KAFKA/Compatibility+Matrix)bakın.

## <a name="general-migration-process"></a>Genel geçiş süreci

Aşağıdaki geçiş kılavuzu, HDInsight 3.6'da tek bir sanal ağda dağıtılan bir Apache Kafka 1.0.0 veya 1.1.0 kümesini varsayar. Mevcut broker bazı konular vardır ve aktif üreticiler ve tüketiciler tarafından kullanılmaktadır.

![Mevcut Kafka tahmin ortamı](./media/upgrade-threesix-to-four/apache-kafka-presumed-environment.png)

Geçişi tamamlamak için aşağıdaki adımları yapın:

1. **Test için yeni bir HDInsight 4.0 kümesi ve istemciler dağıtın.** Yeni bir HDInsight 4.0 Kafka kümesi dağıtın. Birden çok Kafka küme sürümü seçilebilirse, en son sürümü seçmeniz önerilir. Dağıtımdan sonra, gerektiğinde bazı parametrelerayarlayın ve varolan ortamınızla aynı ada sahip bir konu oluşturun. Ayrıca, gerektiğinde SSL'yi ve kendi anahtarını getir (BYOK) şifrelemeyi ayarlayın. Ardından, yeni kümeyle doğru çalışıp çalışmayolmadığını kontrol edin.

    ![Yeni HDInsight 4.0 kümelerini dağıtın](./media/upgrade-threesix-to-four/deploy-new-hdinsight-clusters.png)

1. **Üretici uygulaması için kümeyi değiştirin ve tüm sıra verilerinin geçerli tüketiciler tarafından tüketilmesini bekleyin.** Yeni HDInsight 4.0 Kafka kümesi hazır olduğunda, mevcut üretici hedefini yeni kümeye geçirin. Mevcut Tüketici uygulaması varolan kümedeki tüm verileri tüketene kadar olduğu gibi bırakın.

    ![Üretici uygulaması için anahtar kümesi](./media/upgrade-threesix-to-four/switch-cluster-producer-app.png)

1. **Tüketici uygulamasında ki kümeyi değiştirin.** Varolan tüketici uygulamasının varolan kümedeki tüm verileri tüketme yi bitirdiğini doğruladıktan sonra, bağlantıyı yeni kümeye geçin.

    ![Tüketici uygulamasında anahtar kümesi](./media/upgrade-threesix-to-four/switch-cluster-consumer-app.png)

1. **Eski kümeyi kaldırın ve gerektiğinde uygulamaları test edin.** Anahtar tamamlandıktan ve düzgün çalıştığında, eski HDInsight 3.6 Kafka kümesini ve testte kullanılan üretici ve tüketicileri gerektiği gibi kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Kafka HDInsight kümeleri için performans iyileştirme](apache-kafka-performance-tuning.md)
* [Hızlı başlangıç: Azure portalını kullanarak Azure HDInsight'ta Apache Kafka kümesi oluşturma](apache-kafka-get-started.md)
