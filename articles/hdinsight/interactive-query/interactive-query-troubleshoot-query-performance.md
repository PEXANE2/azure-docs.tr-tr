---
title: Azure HDInsight 'ta Apache Hive LLAP sorgularında kötü performans
description: Apache Hive LLAP içindeki sorgular, Azure HDInsight 'ta beklenenden daha yavaş yürütülüyor.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: 8bd20849b15f8c8d5a14653f702f78c6404d82e5
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75895133"
---
# <a name="scenario-poor-performance-in-apache-hive-llap-queries-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta Apache Hive LLAP sorgularında kötü performans

Bu makalede, Azure HDInsight kümelerinde etkileşimli sorgu bileşenlerini kullanırken sorunlar için sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Varsayılan küme yapılandırması iş yükünüz için yeterince ayarlı değil. Hive LLAP içindeki sorgular beklenenden yavaş yürütülüyor.

## <a name="cause"></a>Nedeni

Bu durum çeşitli nedenlerden kaynaklanabilir.

## <a name="resolution"></a>Çözünürlük

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

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport) ile bağlanma-Azure Community 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
