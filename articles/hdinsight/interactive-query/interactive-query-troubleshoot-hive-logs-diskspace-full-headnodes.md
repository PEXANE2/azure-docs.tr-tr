---
title: Apache Hive günlük doldurma disk alanı-Azure HDInsight
description: Apache Hive Günlükler, Azure HDInsight 'taki baş düğümlerdeki disk alanını doldurana.
ms.service: hdinsight
ms.topic: troubleshooting
author: nisgoel
ms.author: nisgoel
ms.reviewer: jasonh
ms.date: 10/05/2020
ms.openlocfilehash: a102c9f375b37579cf6f92b08d67f762d3dfd26a
ms.sourcegitcommit: 8d8deb9a406165de5050522681b782fb2917762d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92220899"
---
# <a name="scenario-apache-hive-logs-are-filling-up-the-disk-space-on-the-head-nodes-in-azure-hdinsight"></a>Senaryo: Apache Hive Günlükler Azure HDInsight 'taki baş düğümlerdeki disk alanını dolduriyor

Bu makalede, Azure HDInsight kümelerinde baş düğümlerde yeterli disk alanı olmadığından ilgili sorunlar için sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache Hive/LLAP kümesinde, istenmeyen Günlükler baş düğümlerdeki tüm disk alanını kaplar. Bu nedenle, aşağıdaki sorunlar görülebilir.

1. Baş düğümde hiç boşluk kalmadı, SSH erişimi başarısız olur.
2. Ambarı *http hatası veriyor: 503 hizmeti kullanılamıyor*.
3. HiveServer2 etkileşimli yeniden başlatılamadı.

`ambari-agent`Sorun oluştuğunda günlükler aşağıdaki gibi görünür.
```
ambari_agent - Controller.py - [54697] - Controller - ERROR - Error:[Errno 28] No space left on device
```
```
ambari_agent - HostCheckReportFileHandler.py - [54697] - ambari_agent.HostCheckReportFileHandler - ERROR - Can't write host check file at /var/lib/ambari-agent/data/hostcheck.result
```

## <a name="cause"></a>Nedeni

Gelişmiş Hive-Log4J yapılandırmalarında, geçerli varsayılan silme zamanlaması, son değiştirilme tarihine bağlı olarak 30 günden eski dosyalar için ayarlanır.

## <a name="resolution"></a>Çözüm

1. Ambarı portalındaki Hive bileşen Özeti ' ne gidin ve sekme ' ye tıklayın `Configs` .

2. `Advanced hive-log4j`Gelişmiş ayarlar içindeki bölüme gidin.

3. `appender.RFA.strategy.action.condition.age`Parametreyi tercih ettiğiniz bir yaşa ayarlayın. 14 gün için örnek: `appender.RFA.strategy.action.condition.age = 14D`

4. İlgili ayarları görmüyorsanız, lütfen aşağıdaki ayarları ekleyin.
    ```
    # automatically delete hive log
    appender.RFA.strategy.action.type = Delete
    appender.RFA.strategy.action.basePath = ${sys:hive.log.dir}
    appender.RFA.strategy.action.condition.type = IfLastModified
    appender.RFA.strategy.action.condition.age = 30D
    appender.RFA.strategy.action.PathConditions.type = IfFileName
    appender.RFA.strategy.action.PathConditions.regex = hive*.*log.*
    ```

5. `hive.root.logger` `INFO,RFA` Aşağıdaki şekilde ayarlayın. Varsayılan ayar, günlükleri çok büyük hale getiren hata AYıKLA.

    ```
    # Define some default values that can be overridden by system properties
    hive.log.threshold=ALL
    hive.root.logger=INFO,RFA
    hive.log.dir=${java.io.tmpdir}/${user.name}
    hive.log.file=hive.log
    ```

6. Yapılandırmaları kaydedin ve gerekli bileşenleri yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport)Azure Community 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
