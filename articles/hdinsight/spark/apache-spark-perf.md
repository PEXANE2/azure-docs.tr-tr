---
title: Spark işlerini performans için iyileştirin-Azure HDInsight
description: Azure HDInsight 'ta Apache Spark kümelerinin en iyi performansı için ortak stratejileri görüntüleyin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/21/2020
ms.openlocfilehash: f92a351087670ce0b37921a496eabfa883a3b1fc
ms.sourcegitcommit: a9784a3fd208f19c8814fe22da9e70fcf1da9c93
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83780103"
---
# <a name="optimize-apache-spark-jobs-in-hdinsight"></a>HDInsight 'ta Apache Spark işlerini iyileştirme

Bu makalede, Azure HDInsight 'ta Apache Spark işlerini iyileştirmek için stratejiler hakkında bir genel bakış sunulmaktadır.

## <a name="overview"></a>Genel Bakış

Apache Spark işlerinizin performansı birden çok etkene bağlıdır. Bu performans faktörleri şunlardır: verilerinizin nasıl depolandığını, kümenin nasıl yapılandırıldığını ve verileri işlerken kullanılan işlemleri içerir.

Hatalı boyutlardaki yürüticilere, uzun süreli işlemlere ve Kartezyen işlemlere neden olan görevlere yönelik olarak bellek kısıtlamalarını içeren sık karşılaşılan güçlükler.

Ayrıca, önbelleğe alma gibi bu sorunları aşmanıza yardımcı olabilecek çeşitli stratejiler vardır ve veri eğmaya izin verebilirsiniz.

Aşağıdaki makalelerin her birinde, Spark iyileştirmesinin farklı bir yönü için sık karşılaşılan güçlükleri ve çözümleri bulabilirsiniz.

* [Veri depolamayı iyileştirme](optimize-data-storage.md)
* [Veri işlemeyi iyileştirme](optimize-data-processing.md)
* [Bellek kullanımını iyileştirme](optimize-memory-usage.md)
* [Küme yapılandırmasını iyileştirme](optimize-cluster-configuration.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight üzerinde çalışan Apache Spark işlerinin hatasını ayıklama](apache-spark-job-debugging.md)
* [HDInsight üzerinde Apache Spark kümesi için kaynakları yönetme](apache-spark-resource-manager.md)
* [Apache Spark ayarlarını yapılandırma](apache-spark-settings.md)
