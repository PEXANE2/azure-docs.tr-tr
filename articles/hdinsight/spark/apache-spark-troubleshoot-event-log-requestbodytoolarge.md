---
title: Azure HDInsight 'ta Apache Spark akış uygulaması için günlük RequestBodyTooLarge
description: NativeAzureFileSystem ... Azure HDInsight 'ta Apache Spark akış uygulaması günlüğünde RequestBodyTooLarge görünüyor
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/29/2019
ms.openlocfilehash: 92e9e41d5829e10bc0130d209249301d78f1b5d3
ms.sourcegitcommit: 97605f3e7ff9b6f74e81f327edd19aefe79135d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70736323"
---
# <a name="scenario-nativeazurefilesystem--requestbodytoolarge-appears-in-log-for-apache-spark-streaming-app-in-azure-hdinsight"></a>Senaryo: "NativeAzureFileSystem ... Azure HDInsight 'ta Apache Spark akış uygulaması günlüğünde RequestBodyTooLarge "görüntülenir

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorunlar için sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Hata: `NativeAzureFileSystem ... RequestBodyTooLarge` Apache Spark bir akış uygulamasının sürücü günlüğünde görüntülenir.

## <a name="cause"></a>Nedeni

Spark olay günlüğü dosyanız muhtemelen LıBB 'nin dosya uzunluğu sınırına ulaşıyordur.

Spark 2,3 ' de, her Spark uygulaması bir Spark olay günlüğü dosyası oluşturur. Spark akış uygulamasının Spark olay günlüğü dosyası, uygulama çalışırken büyümeye devam eder. Günümüzde IDB 'deki bir dosya 50000 blok sınırına sahiptir ve varsayılan blok boyutu 4 MB 'tır. Bu nedenle, varsayılan yapılandırmada en büyük dosya boyutu 195 GB 'tır. Ancak, Azure Storage en büyük blok boyutunu 100 MB olarak artırmıştır ve bu, tek dosya sınırını 4,75 TB 'ye etkin bir şekilde getirdi. Daha fazla bilgi için bkz. [Azure Storage ölçeklenebilirlik ve performans hedefleri](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets).

## <a name="resolution"></a>Çözüm

Bu hata için üç çözüm bulunmaktadır:

* Blok boyutunu 100 MB 'a kadar artırın. Ambarı Kullanıcı arabiriminde, suconfiguration özelliğini `fs.azure.write.request.size` değiştirin (veya `Custom core-site` bölümünde oluşturun). Özelliği daha büyük bir değere ayarlayın, örneğin: 33554432. Güncelleştirilmiş yapılandırmayı kaydedin ve etkilenen bileşenleri yeniden başlatın.

* Spark akış işini düzenli aralıklarla durdurup yeniden gönderin.

* Spark olay günlüklerini depolamak için, "kullanın. Depolama için, bellek kullanımı, küme ölçekleme veya Azure yükseltmeleri sırasında Spark olay verilerinin kaybedilmesine neden olabilir.

    1. Ambarı Kullanıcı arabiriminde `spark.eventlog.dir` ve `spark.history.fs.logDirectory` aracılığıyla değişiklik yapın:

        ```
        spark.eventlog.dir = hdfs://mycluster/hdp/spark2-events
        spark.history.fs.logDirectory = "hdfs://mycluster/hdp/spark2-events"
        ```

    1. Şu sunucuda dizin oluşturma:

        ```
        hadoop fs -mkdir -p hdfs://mycluster/hdp/spark2-events
        hadoop fs -chown -R spark:hadoop hdfs://mycluster/hdp
        hadoop fs -chmod -R 777 hdfs://mycluster/hdp/spark2-events
        hadoop fs -chmod -R o+t hdfs://mycluster/hdp/spark2-events
        ```

    1. Tüm etkilenen Hizmetleri, ambarı Kullanıcı arabirimi aracılığıyla yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* Azure Community [@AzureSupport](https://twitter.com/azuresupport) 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
