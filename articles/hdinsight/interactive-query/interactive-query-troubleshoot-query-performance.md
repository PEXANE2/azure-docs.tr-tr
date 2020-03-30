---
title: Azure HDInsight'ta Apache Hive LLAP sorgularında düşük performans
description: Apache Hive LLAP'deki sorgular Azure HDInsight'ta beklenenden daha yavaş yürütüliyor.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: 8bd20849b15f8c8d5a14653f702f78c6404d82e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75895133"
---
# <a name="scenario-poor-performance-in-apache-hive-llap-queries-in-azure-hdinsight"></a>Senaryo: Azure HDInsight'ta Apache Hive LLAP sorgularında düşük performans

Bu makalede, Azure HDInsight kümelerinde Etkileşimli Sorgu bileşenleri kullanılırken sorun giderme adımları ve sorunların olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Varsayılan küme yapılandırmaları iş yükünüz için yeterince ayarlanmamıştır. Hive LLAP'deki sorgular beklenenden daha yavaş yürütülme de dir.

## <a name="cause"></a>Nedeni

Bu çeşitli nedenlerle olabilir.

## <a name="resolution"></a>Çözüm

LLAP, birleştirmeler ve toplamlar içeren sorgular için optimize edilebiyi iyileştirir. Aşağıdaki gibi sorgular Etkileşimli Kovan kümesinde iyi performans göstermez:

```
select * from table where column = "columnvalue"
```

Hive LLAP'de nokta sorgusu performansını artırmak için aşağıdaki yapılandırmaları ayarlayın:

```
hive.llap.io.enabled=false; (disable LLAP IO)
hive.optimize.index.filter=false; (disable ORC row index)
hive.exec.orc.split.strategy=BI; (to avoid recombining splits)
```

Aşağıdaki yapılandırma değişikliğiyle performansı artırmak için LLAP önbelleğinin kullanımını da artırabilirsiniz:

```
hive.fetch.task.conversion=none
```

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
