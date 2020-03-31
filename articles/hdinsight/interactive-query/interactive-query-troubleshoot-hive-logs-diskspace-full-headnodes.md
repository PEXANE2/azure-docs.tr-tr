---
title: Disk alanını dolduran Apache Hive Günlükleri - Azure HDInsight
description: Apache Hive günlükleri Azure HDInsight'ta kafa düğümleri üzerindeki disk alanını dolduruyor.
ms.service: hdinsight
ms.topic: troubleshooting
author: nisgoel
ms.author: nisgoel
ms.reviewer: jasonh
ms.date: 03/05/2020
ms.openlocfilehash: d843b942702d335065a5f3798572e34c71b4cd0e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78943963"
---
# <a name="scenario-apache-hive-logs-are-filling-up-the-disk-space-on-the-head-nodes-in-azure-hdinsight"></a>Senaryo: Apache Hive günlükleri Azure HDInsight'ta Kafa düğümleri üzerindeki disk alanını dolduruyor

Bu makalede, Azure HDInsight kümelerinde kafa düğümlerinde yeterli disk alanı olmamasıyla ilgili sorunlar için sorun giderme adımları ve olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Bir Apache Hive/LLAP kümesinde, istenmeyen günlükler kafa düğümleri üzerindeki tüm disk alanını kaplar. Bu nedenle, aşağıdaki konular görülebilir.

1. SSH erişimi, baş düğümünde boşluk kalmadığı için başarısız olur.
2. Ambari *HTTP HATA verir: 503 Hizmet Kullanılamaz*.

Günlükler, `ambari-agent` sorun olduğunda aşağıdakileri gösterir.
```
ambari_agent - Controller.py - [54697] - Controller - ERROR - Error:[Errno 28] No space left on device
```
```
ambari_agent - HostCheckReportFileHandler.py - [54697] - ambari_agent.HostCheckReportFileHandler - ERROR - Can't write host check file at /var/lib/ambari-agent/data/hostcheck.result
```

## <a name="cause"></a>Nedeni

Gelişmiş Hive-log4j yapılandırmalarında, parametre *log4j.appender.RFA.MaxBackupIndex* atlanır. Bu günlük dosyaları sonsuz nesil neden olur.

## <a name="resolution"></a>Çözüm

1. Ambari portalında Kovan bileşen özetine `Configs` gidin ve sekmeye tıklayın.

2. Gelişmiş ayarlar `Advanced hive-log4j` içindeki bölüme gidin.

3. Parametreyi RollingFileAppender olarak ayarlayın. `log4j.appender.RFA` 

4. Set `log4j.appender.RFA.MaxFileSize` `log4j.appender.RFA.MaxBackupIndex` ve aşağıdaki gibidir.

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
5. Aşağıdaki `hive.root.logger` `INFO,RFA` gibi ayarlayın. Varsayılan ayar, günlüklerin çok büyük olmasını sağlayan HATA Ayıklama'dır.

```
# Define some default values that can be overridden by system properties
hive.log.threshold=ALL
hive.root.logger=INFO,RFA
hive.log.dir=${java.io.tmpdir}/${user.name}
hive.log.file=hive.log
```

6. Configs kaydedin ve gerekli bileşenleri yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
