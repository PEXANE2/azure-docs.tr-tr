---
title: Apache Spark uygulamasından BodyTooLarge hatası İsteği - Azure HDInsight
description: NativeAzureFileSystem ... RequestBodyTooLarge, Azure HDInsight'ta Apache Spark akış uygulaması için günlükte görünür
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: 777d06670238a7625d190c92f78a55cd4794d226
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75894392"
---
# <a name="nativeazurefilesystemrequestbodytoolarge-appear-in-apache-spark-streaming-app-log-in-hdinsight"></a>"NativeAzureFileSystem... RequestBodyTooLarge" HDInsight'ta Apache Spark akış uygulaması günlüğünde görünür

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorun giderme adımları ve sorunların olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Hata: `NativeAzureFileSystem ... RequestBodyTooLarge` Bir Apache Spark akış uygulamasının sürücü günlüğünde görünür.

## <a name="cause"></a>Nedeni

Kıvılcım olay günlüğü dosyanız büyük olasılıkla WASB için dosya uzunluk sınırına isabet ediyor.

Spark 2.3'te, her Spark uygulaması bir Spark olay günlüğü dosyası oluşturur. Bir Spark akış uygulamasının Spark olay günlüğü dosyası, uygulama çalışırken büyümeye devam eder. Bugün WASB'deki bir dosya50000 blok sınırına sahiptir ve varsayılan blok boyutu 4 MB'dır. Yani varsayılan yapılandırmada maksimum dosya boyutu 195 GB'dır. Ancak Azure Depolama, maksimum blok boyutunu 100 MB'a çıkararak tek dosya sınırını etkin bir şekilde 4,75 TB'ye çıkardı. Daha fazla bilgi için [Blob depolama için Ölçeklenebilirlik ve performans hedeflerine](../../storage/blobs/scalability-targets.md)bakın.

## <a name="resolution"></a>Çözüm

Bu hata için üç çözüm vardır:

* Blok boyutunu 100 MB'a kadar artırın. Ambari UI'de, HDFS `fs.azure.write.request.size` yapılandırma özelliğini `Custom core-site` değiştirin (veya bölümde oluşturun). Özelliği daha büyük bir değere ayarlayın, örneğin: 33554432. Güncelleştirilmiş yapılandırmayı kaydedin ve etkilenen bileşenleri yeniden başlatın.

* Düzenli olarak durdurun ve kıvılcım akışı işini yeniden gönderin.

* Spark olay günlüklerini depolamak için HDFS'yi kullanın. Depolama için HDFS'nin kullanılması, küme ölçekleme veya Azure yükseltmeleri sırasında Kıvılcım olay verilerinin kaybolmasına neden olabilir.

    1. Ambari `spark.eventlog.dir` UI ile ve `spark.history.fs.logDirectory` bu değişiklikler yapın:

        ```
        spark.eventlog.dir = hdfs://mycluster/hdp/spark2-events
        spark.history.fs.logDirectory = "hdfs://mycluster/hdp/spark2-events"
        ```

    1. HDFS'de dizin oluşturma:

        ```
        hadoop fs -mkdir -p hdfs://mycluster/hdp/spark2-events
        hadoop fs -chown -R spark:hadoop hdfs://mycluster/hdp
        hadoop fs -chmod -R 777 hdfs://mycluster/hdp/spark2-events
        hadoop fs -chmod -R o+t hdfs://mycluster/hdp/spark2-events
        ```

    1. Ambari UI aracılığıyla etkilenen tüm hizmetleri yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
