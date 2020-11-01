---
title: 'Sorun giderme: Apache Hive Günlükler disk alanını doldurabilir-Azure HDInsight'
description: Bu makalede, Apache Hive Günlükler Azure HDInsight 'taki baş düğümlerdeki disk alanını doldururken izlenecek sorun giderme adımları sağlanmaktadır.
ms.service: hdinsight
ms.topic: troubleshooting
author: nisgoel
ms.author: nisgoel
ms.reviewer: jasonh
ms.date: 10/05/2020
ms.openlocfilehash: 64bf5714f5eb99df9929a47fef414a827ec680af
ms.sourcegitcommit: 4b76c284eb3d2b81b103430371a10abb912a83f4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2020
ms.locfileid: "93145642"
---
# <a name="scenario-apache-hive-logs-are-filling-up-the-disk-space-on-the-head-nodes-in-azure-hdinsight"></a>Senaryo: Apache Hive Günlükler Azure HDInsight 'taki baş düğümlerdeki disk alanını dolduriyor

Bu makalede, Azure HDInsight kümelerinde baş düğümlerdeki yetersiz disk alanı ile ilgili sorunlar için sorun giderme adımları ve olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache Hive/LLAP kümesinde, istenmeyen Günlükler baş düğümlerdeki tüm disk alanını kaplar. Bu durum aşağıdaki sorunlara neden olabilir:

- Baş düğümde hiçbir boşluk ayrılmadığından SSH erişimi başarısız olur.
- Ambarı oluşturan *http hatası: 503 hizmeti kullanılamıyor* .
- HiveServer2 etkileşimli yeniden başlatılamadı.

`ambari-agent`Bu, sorun oluştuğunda günlükler aşağıdaki girişleri içerir:
```
ambari_agent - Controller.py - [54697] - Controller - ERROR - Error:[Errno 28] No space left on device
```
```
ambari_agent - HostCheckReportFileHandler.py - [54697] - ambari_agent.HostCheckReportFileHandler - ERROR - Can't write host check file at /var/lib/ambari-agent/data/hostcheck.result
```

## <a name="cause"></a>Nedeni

Gelişmiş Hive Log4J yapılandırmalarında, geçerli varsayılan silme zamanlaması, son değiştirilme tarihine bağlı olarak 30 günden eski olan dosyaları silmektir.

## <a name="resolution"></a>Çözüm

1. Ambarı portalındaki Hive bileşen Özeti ' ne gidin ve **configs** sekmesini seçin.

2. `Advanced hive-log4j` **Gelişmiş ayarlar** ' da bölümüne gidin.

3. `appender.RFA.strategy.action.condition.age`Parametresini tercih ettiğiniz bir yaş olarak ayarlayın. Bu örnek, yaşı 14 gün olarak ayarlar: `appender.RFA.strategy.action.condition.age = 14D`

4. İlgili ayarları görmüyorsanız, şu ayarları ekleyin:
    ```
    # automatically delete hive log
    appender.RFA.strategy.action.type = Delete
    appender.RFA.strategy.action.basePath = ${sys:hive.log.dir}
    appender.RFA.strategy.action.condition.type = IfLastModified
    appender.RFA.strategy.action.condition.age = 30D
    appender.RFA.strategy.action.PathConditions.type = IfFileName
    appender.RFA.strategy.action.PathConditions.regex = hive*.*log.*
    ```

5. `hive.root.logger` `INFO,RFA` Aşağıdaki örnekte gösterildiği gibi olarak ayarlanır. Varsayılan ayar, `DEBUG` günlükleri büyük yapar.

    ```
    # Define some default values that can be overridden by system properties
    hive.log.threshold=ALL
    hive.root.logger=INFO,RFA
    hive.log.dir=${java.io.tmpdir}/${user.name}
    hive.log.file=hive.log
    ```

6. Konfigürasyonları kaydedin ve gerekli bileşenleri yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport)Azure Community 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](../../azure-portal/supportability/how-to-create-azure-support-request.md)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
