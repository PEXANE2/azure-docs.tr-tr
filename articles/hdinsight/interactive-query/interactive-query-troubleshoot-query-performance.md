---
title: Azure HDInsight 'ta Apache Hive LLAP sorgularında kötü performans
description: Apache Hive LLAP içindeki sorgular, Azure HDInsight 'ta beklenenden daha yavaş yürütülüyor.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: ee7ce401f889dd9c06b14860f4fc9674c5350b52
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93288884"
---
# <a name="scenario-poor-performance-in-apache-hive-llap-queries-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta Apache Hive LLAP sorgularında kötü performans

Bu makalede, Azure HDInsight kümelerinde etkileşimli sorgu bileşenlerini kullanırken sorunlar için sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Varsayılan küme yapılandırması iş yükünüz için yeterince ayarlı değil. Hive LLAP içindeki sorgular beklenenden yavaş yürütülüyor.

## <a name="cause"></a>Nedeni

Bu durum çeşitli nedenlerden kaynaklanabilir.

## <a name="resolution"></a>Çözüm

LLAP, birleştirmeler ve toplamalar içeren sorgular için iyileştirilmiştir. Aşağıdakiler gibi sorgular etkileşimli bir Hive kümesinde iyi bir şekilde gerçekleştirmez:

```
select * from table where column = "columnvalue"
```

Hive LLAP içindeki nokta sorgu performansını artırmak için aşağıdaki konfigürasyonları ayarlayın:

```
hive.llap.io.enabled=false; (disable LLAP IO)
hive.optimize.index.filter=false; (disable ORC row index)
hive.exec.orc.split.strategy=BI; (to avoid recombining splits)
```

Ayrıca, aşağıdaki yapılandırma değişikliği ile performansı artırmak için LLAP önbelleğinin kullanımını artırabilirsiniz:

```
hive.fetch.task.conversion=none
```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]