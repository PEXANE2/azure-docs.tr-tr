---
title: Spark işlerini performans için iyileştirin-Azure HDInsight
description: Azure HDInsight 'ta Apache Spark kümelerinin en iyi performansı için ortak stratejileri görüntüleyin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 08/21/2020
ms.custom: contperf-fy21q1
ms.openlocfilehash: d808339dd842e88e74efce5d56d12bc5250eb238
ms.sourcegitcommit: 3ea45bbda81be0a869274353e7f6a99e4b83afe2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97029303"
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
