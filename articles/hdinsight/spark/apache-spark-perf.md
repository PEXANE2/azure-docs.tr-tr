---
title: Spark işlerini performans için iyileştirin-Azure HDInsight
description: Azure HDInsight 'ta Apache Spark kümelerinin en iyi performansı için ortak stratejileri görüntüleyin.
ms.service: hdinsight
ms.topic: conceptual
ms.date: 08/21/2020
ms.custom: contperf-fy21q1
ms.openlocfilehash: 567fc2637538408d9727d07d3185a5b0e3cf20c5
ms.sourcegitcommit: 2f9f306fa5224595fa5f8ec6af498a0df4de08a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98929944"
---
# <a name="optimize-apache-spark-jobs-in-hdinsight"></a>HDInsight 'ta Apache Spark işlerini iyileştirme

Bu makalede, Azure HDInsight 'ta Apache Spark işlerini iyileştirmek için stratejiler hakkında bir genel bakış sunulmaktadır.

## <a name="overview"></a>Genel Bakış

Apache Spark işlerinizin performansı birden çok etkene bağlıdır. Bu performans faktörleri şunlardır: verilerinizin nasıl depolandığını, kümenin nasıl yapılandırıldığını ve verileri işlerken kullanılan işlemleri içerir.

Yüz yüze olabilecek yaygın sorunlar: Hatalı boyutlardaki yürüticileri, uzun süreli işlemler ve Kartezyen işlemlere neden olan görevler nedeniyle bellek kısıtlamaları.

Önbelleğe alma gibi bu sorunları aşmanıza yardımcı olabilecek birçok iyileştirme da vardır ve veri eğriliği için izin verir.

Aşağıdaki makalelerin her birinde, Spark iyileştirmesinin farklı yönlerini öğrenmek için bilgileri bulabilirsiniz.

* [Apache Spark için veri depolamayı iyileştirin](optimize-data-storage.md)
* [Apache Spark için veri işlemeyi iyileştirin](optimize-data-processing.md)
* [Apache Spark için bellek kullanımını iyileştirin](optimize-memory-usage.md)
* [Apache Spark için HDInsight küme yapılandırmasını iyileştirme](optimize-cluster-configuration.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight üzerinde çalışan Apache Spark işlerinin hatasını ayıklama](apache-spark-job-debugging.md)
* [HDInsight üzerinde Apache Spark kümesi için kaynakları yönetme](apache-spark-resource-manager.md)
* [Apache Spark ayarlarını yapılandırma](apache-spark-settings.md)
