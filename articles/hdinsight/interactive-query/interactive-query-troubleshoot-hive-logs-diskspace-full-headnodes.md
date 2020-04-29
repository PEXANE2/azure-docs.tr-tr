---
title: Apache Hive günlük doldurma disk alanı-Azure HDInsight
description: Apache Hive Günlükler, Azure HDInsight 'taki baş düğümlerdeki disk alanını doldurana.
ms.service: hdinsight
ms.topic: troubleshooting
author: nisgoel
ms.author: nisgoel
ms.reviewer: jasonh
ms.date: 03/05/2020
ms.openlocfilehash: d843b942702d335065a5f3798572e34c71b4cd0e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78943963"
---
# <a name="scenario-apache-hive-logs-are-filling-up-the-disk-space-on-the-head-nodes-in-azure-hdinsight"></a>Senaryo: Apache Hive Günlükler Azure HDInsight 'taki baş düğümlerdeki disk alanını dolduriyor

Bu makalede, Azure HDInsight kümelerinde baş düğümlerde yeterli disk alanı olmadığından ilgili sorunlar için sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache Hive/LLAP kümesinde, istenmeyen Günlükler baş düğümlerdeki tüm disk alanını kaplar. Bu nedenle, aşağıdaki sorunlar görülebilir.

1. Baş düğümde hiç boşluk kalmadı, SSH erişimi başarısız olur.
2. Ambarı *http hatası veriyor: 503 hizmeti kullanılamıyor*.

Sorun `ambari-agent` oluştuğunda günlükler aşağıdaki gibi görünür.
```
ambari_agent - Controller.py - [54697] - Controller - ERROR - Error:[Errno 28] No space left on device
```
```
ambari_agent - HostCheckReportFileHandler.py - [54697] - ambari_agent.HostCheckReportFileHandler - ERROR - Can't write host check file at /var/lib/ambari-agent/data/hostcheck.result
```

## <a name="cause"></a>Nedeni

Gelişmiş Hive-Log4J yapılandırmalarında, *Log4J. appender. RFA. Maxbackupındex* parametresi atlanır. Günlük dosyalarının sonsuz şekilde oluşturulmasına neden olur.

## <a name="resolution"></a>Çözüm

1. Ambarı portalındaki Hive bileşen Özeti ' ne gidin ve `Configs` sekme ' ye tıklayın.

2. Gelişmiş ayarlar içindeki `Advanced hive-log4j` bölüme gidin.

3. Parametreyi `log4j.appender.RFA` Rollingfileappınfıas olarak ayarlayın. 

4. Ve `log4j.appender.RFA.MaxFileSize` `log4j.appender.RFA.MaxBackupIndex` aşağıdaki gibi ayarlayın.

```
log4jhive.log.maxfilesize=1024MB
log4jhive.log.maxbackupindex=10

log4j.appender.RFA=org.apache.log4j.RollingFileAppender
log4j.appender.RFA.File=${hive.log.dir}/${hive.log.file}
log4j.appender.RFA.MaxFileSize=${log4jhive.log.maxfilesize}
log4j.appender.RFA.MaxBackupIndex=${log4jhive.log.maxbackupindex}
log4j.appender.RFA.layout=org.apache.log4j.PatternLayout
log4j.appender.RFA.layout.ConversionPattern=%d{ISO8601} %-5p [%t] %c{2}: %m%n
```
5. Aşağıdaki `hive.root.logger` `INFO,RFA` şekilde ayarlayın. Varsayılan ayar, günlükleri çok büyük hale getiren hata AYıKLA.

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

* Azure Community [@AzureSupport](https://twitter.com/azuresupport) 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
