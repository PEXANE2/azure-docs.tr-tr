---
title: Apache Spark-Azure HDInsight 'ta bellek kullanımını iyileştirme
description: Azure HDInsight üzerinde Apache Spark bellek kullanımının nasıl iyileştirileceği hakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/20/2020
ms.custom: contperf-fy21q1
ms.openlocfilehash: ed10648fb115ae0dc12300f3e48db165fcb4c215
ms.sourcegitcommit: 3ea45bbda81be0a869274353e7f6a99e4b83afe2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97028164"
---
# <a name="memory-usage-optimization-for-apache-spark"></a>Apache Spark için bellek kullanımı iyileştirmesi

Bu makalede, Azure HDInsight 'ta en iyi performansı elde etmek için Apache Spark kümenizin bellek yönetimini en iyi hale getirmeyi ele alınmaktadır.

## <a name="overview"></a>Genel Bakış

Spark, verileri belleğe yerleştirerek çalışır. Bu nedenle, bellek kaynaklarını yönetmek Spark işlerinin yürütülmesini iyileştirmeye yönelik önemli bir yönüdür.  Kümenizin belleğini verimli bir şekilde kullanmak için uygulayabileceğiniz çeşitli teknikler vardır.

* Bölümleme stratejinizde veri boyutu, türleri ve dağıtımı için daha küçük veri bölümleri ve hesabı tercih edin.
* Varsayılan Java serileştirmesi yerine daha yeni, daha verimli bir şekilde düşünün [`Kryo data serialization`](https://github.com/EsotericSoftware/kryo) .
* Batch 'e göre ayrılan YARN kullanmayı tercih edin `spark-submit` .
* Spark yapılandırma ayarlarını izleyin ve ayarlayın.

Başvurunuz için Spark bellek yapısı ve bazı önemli yürütücü belleği parametreleri sonraki görüntüde gösterilmiştir.

## <a name="spark-memory-considerations"></a>Spark bellek konuları

Apache Hadoop YARN kullanıyorsanız, YARN her Spark düğümündeki tüm kapsayıcılar tarafından kullanılan belleği denetler.  Aşağıdaki diyagramda, temel nesneler ve bunların ilişkileri gösterilmektedir.

![YARN Spark bellek yönetimi](./media/apache-spark-perf/apache-yarn-spark-memory.png)

' Bellek yetersiz ' iletilerini ele almak için şunu deneyin:

* DAG yönetim karışık Les 'yi gözden geçirin. Harita tarafı reducting, ön bölüm (veya bucketize) kaynaklı verileri azaltarak, tek bir karayı en üst düzeye çıkarın ve gönderilen veri miktarını azaltın.
* `ReduceByKey` `GroupByKey` Toplamaları, Pencereleme ve diğer işlevleri sağlayan ancak sınırsız bellek sınırına sahip olan sabit bellek sınırı ile tercih edilir.
* `TreeReduce`Yürüticilere veya bölümlerle üzerinde daha fazla çalışmayı, yani `Reduce` sürücü üzerinde çalışmayı tercih eder.
* Alt düzey RDD nesneleri yerine veri çerçevelerini kullanın.
* "Ilk N", çeşitli toplamalar veya Pencereleme işlemleri gibi eylemleri kapsülleyen Karmaşıktürler oluşturun.

Ek sorun giderme adımları için bkz. [Azure HDInsight 'ta Apache Spark Için OutOfMemoryError özel durumları](apache-spark-troubleshoot-outofmemory.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Spark için veri işlemeyi iyileştirin](optimize-cluster-configuration.md)
* [Apache Spark için veri depolamayı iyileştirin](optimize-data-storage.md)
* [Apache Spark için küme yapılandırmasını iyileştirme](optimize-cluster-configuration.md)
