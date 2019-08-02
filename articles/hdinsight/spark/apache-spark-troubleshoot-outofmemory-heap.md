---
title: Azure HDInsight 'ta Apache Spark için OutOfMemoryError özel durumu
description: Azure HDInsight 'ta Apache Spark için OutOfMemoryError özel durumu
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/26/2019
ms.openlocfilehash: ac360312fdb3c4a238e6280872bc8c8b548e8544
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68620993"
---
# <a name="scenario-outofmemoryerror-exception-for-apache-spark-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta Apache Spark için OutOfMemoryError özel durumu

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorunlar için sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache Spark uygulamanız OutOfMemoryError işlenmemiş özel durumuyla başarısız oldu. Şuna benzer bir hata iletisi alabilirsiniz:

```error
ERROR Executor: Exception in task 7.0 in stage 6.0 (TID 439)

java.lang.OutOfMemoryError
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source)
    at java.io.ByteArrayOutputStream.grow(Unknown Source)
    at java.io.ByteArrayOutputStream.ensureCapacity(Unknown Source)
    at java.io.ByteArrayOutputStream.write(Unknown Source)
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(Unknown Source)
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(Unknown Source)
    at java.io.ObjectOutputStream.writeObject0(Unknown Source)
    at java.io.ObjectOutputStream.writeObject(Unknown Source)
    at org.apache.spark.serializer.JavaSerializationStream.writeObject(JavaSerializer.scala:44)
    at org.apache.spark.serializer.JavaSerializerInstance.serialize(JavaSerializer.scala:101)
    at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:239)
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source)
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source)
    at java.lang.Thread.run(Unknown Source)
```

```error
ERROR SparkUncaughtExceptionHandler: Uncaught exception in thread Thread[Executor task launch worker-0,5,main]

java.lang.OutOfMemoryError
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source)
    ...
```

## <a name="cause"></a>Nedeni

Bu özel durumun en olası nedeni, yeterli yığın belleği değildir. Spark uygulamanız, yürüticileri veya sürücüleri çalıştırırken yeterli Java sanal makinesi (JVM) yığın belleği gerektirir.

## <a name="resolution"></a>Çözüm

1. Spark uygulamasının işleyeceği verilerin boyut üst sınırını belirleyin. Giriş verilerini dönüştürerek oluşturulan ara veriler ve ara verileri daha fazla dönüştürme ile üretilen çıkış verileri temelinde, en fazla giriş verisi boyutuna göre boyut tahmini yapın. Başlangıçtaki tahmin yeterli değilse, boyutu biraz artırabilir ve bellek hataları alt tarafına kadar yineleme yapın.

1. Kullanılacak HDInsight kümesinin, Spark uygulamasını barındırmak için yeterli kaynağa (bellek ve ayrıca çekirdek olarak) sahip olduğundan emin olun. Bu, kümenin YARN kullanıcı arabirimindeki Cluster Metrics bölümünde şu değerlerin karşılaştırmasına bakarak saptanabilir: Memory Used, Memory Total ve VCores Used, VCores Total

    ![Yarn çekirdek bellek görünümü](./media/apache-spark-ts-outofmemory/yarn-core-memory-view.png)

1. Aşağıdaki Spark yapılandırmasını uygun değerlere ayarlayın. Uygulama gereksinimlerini kümedeki kullanılabilir kaynaklarla dengeleyin. Bu değerler, YARN tarafından görüntülenen kullanılabilir bellek ve çekirdekler için% 90 ' ı aşmamalıdır ve Spark uygulamasının en düşük bellek gereksinimini de karşılamalıdır:

    ```
    spark.executor.instances (Example: 8 for 8 executor count)
    spark.executor.memory (Example: 4g for 4 GB)
    spark.yarn.executor.memoryOverhead (Example: 384m for 384 MB)
    spark.executor.cores (Example: 2 for 2 cores per executor)
    spark.driver.memory (Example: 8g for 8GB)
    spark.driver.cores (Example: 4 for 4 cores)
    spark.yarn.driver.memoryOverhead (Example: 384m for 384MB)
    ```

    Tüm yürüticileri tarafından kullanılan toplam bellek =

    ```
    spark.executor.instances * (spark.executor.memory + spark.yarn.executor.memoryOverhead) 
    ```

    Sürücü tarafından kullanılan toplam bellek =

    ```
    spark.driver.memory + spark.yarn.driver.memoryOverhead
    ```

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Spark bellek yönetimine genel bakış](https://spark.apache.org/docs/latest/tuning.html#memory-management-overview).

* [HDInsight kümelerinde Spark uygulamasında hata ayıklama](https://blogs.msdn.microsoft.com/azuredatalake/2016/12/19/spark-debugging-101/).

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* Azure Community [@AzureSupport](https://twitter.com/azuresupport) 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
