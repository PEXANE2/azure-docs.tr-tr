---
title: Apache Spark akış işleri, Azure HDInsight 'ta işleme alışından daha uzun sürer
description: Apache Spark akış işleri, Azure HDInsight 'ta işleme alışından daha uzun sürer
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/29/2019
ms.openlocfilehash: 7c7baaaa5fd08a102b3b55c38fb2c4bf892480c5
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68679438"
---
# <a name="scenario-apache-spark-streaming-jobs-take-longer-than-usual-to-process-in-azure-hdinsight"></a>Senaryo: Apache Spark akış işleri, Azure HDInsight 'ta işleme alışından daha uzun sürer

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorunlar için sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache Spark akış işlerinin bazılarının yavaş olduğunu veya normalden daha uzun sürdüğünü gözlemleyebilirsiniz. Spark akış uygulamaları için her bir ileti toplu işi Spark 'a gönderilen bir işe karşılık gelir. Bir işin işlemek için normalde X saniye sürüyorsa, bu durum bazen normalden 2-3 dakika sürebilir.

## <a name="cause"></a>Nedeni

Olası nedenlerden biri, Apache Kafka üreticisi Kafka kümesine yazma işleminin tamamlanmasının 2 dakikadan uzun sürmesine neden olur. Kafka sorununa daha fazla hata ayıklamak için, ileti göndermek için bir Kafka üreticisi kullanan koda bir günlüğe kaydetme ekleyebilirsiniz ve bunları Kafka kümesinden alınan Günlükler ile ilişkilendirin.

Diğer bir olası neden, sık kullanılan mikro işlerin sonraki mikro işlerin geçmesine neden olabilir. Yinelenenleri kaldırma `O(n!)` algoritmasından dolayı `Filesystem.listStatus` IDB 'nin uygulanması çok yavaştır. ' Den `BlobListItem` `FileMetadata` 'ekadarçok`FileStatus`fazla bellek kullanır. Örneğin, algoritma 700.000 dosyalarını listelemek için 30 dakika sürer. `ListBlobs` Bu nedenle, her mikro-toplu iş için bir ön uç tarafından kararlılığı çağrılırsa, daha sonraki mikro toplu işlerin arkasında gecikme, yüksek zamanlama gecikmeleriyle karşılaşacaktır. [Bu düzeltme eki](https://issues.apache.org/jira/browse/HADOOP-15547) sorunu düzeltir, ancak ortamınızda `ListBlobs` yoksa, yüksek gecikme süresine sahip olur. Ayrıca, dosyaları her saat silseniz bile arka uçtaki listenin tüm satırları (silindi dahil) üzerinde yinelemesi gerekir çünkü çöp toplama işlemi henüz tamamlanmadı. Düzeltme Eki sorunu çözebilirken, çöp toplama sorunu, toplu işlerin akış işlemesinde gecikmeye neden olabilir.

## <a name="resolution"></a>Çözüm

[HADOOP-15547](https://issues.apache.org/jira/browse/HADOOP-15547) düzeltmesini uygulayın. Bu mümkün değilse, denetim noktası konumu olarak, Şöyle `checkpointDirectory` bir değere ayarla: `hdfs://mycluster/checkpoint`.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* Azure Community [@AzureSupport](https://twitter.com/azuresupport) 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
