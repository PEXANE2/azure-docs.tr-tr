---
title: Spark işlerini performans için iyileştirin-Azure HDInsight
description: Azure HDInsight 'ta Apache Spark kümelerinin en iyi performansı için ortak stratejileri görüntüleyin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/21/2020
ms.openlocfilehash: 0b152f7882c7d7a3bab762253da0febc0257ceae
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84117956"
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
