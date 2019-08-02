---
title: Apache Spark iş, Azure HDInsight 'ta ınvalidclassexception, sınıf sürümü uyuşmazlığı ile başarısız oluyor
description: Apache Spark iş, Azure HDInsight 'ta ınvalidclassexception, sınıf sürümü uyuşmazlığı ile başarısız oluyor
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/29/2019
ms.openlocfilehash: 6f81ed121c98c8428e710082aae7ab75b672335e
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707506"
---
# <a name="scenario-apache-spark-job-fails-with-invalidclassexception-class-version-mismatch-in-azure-hdinsight"></a>Senaryo: Apache Spark iş, Azure HDInsight 'ta ınvalidclassexception, sınıf sürümü uyuşmazlığı ile başarısız oluyor

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorunlar için sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Spark 2. x kümesinde bir Apache Spark işi oluşturmaya çalışırsınız. Aşağıdakine benzer bir hata ile başarısız olur:

```
18/09/18 09:32:26 WARN TaskSetManager: Lost task 0.0 in stage 1.0 (TID 1, wn7-dev-co.2zyfbddadfih0xdq0cdja4g.ax.internal.cloudapp.net, executor 4): java.io.InvalidClassException:
org.apache.commons.lang3.time.FastDateFormat; local class incompatible: stream classdesc serialVersionUID = 2, local class serialVersionUID = 1
        at java.io.ObjectStreamClass.initNonProxy(ObjectStreamClass.java:699)
        at java.io.ObjectInputStream.readNonProxyDesc(ObjectInputStream.java:1885)
        at java.io.ObjectInputStream.readClassDesc(ObjectInputStream.java:1751)
        at java.io.ObjectInputStream.readOrdinaryObject(ObjectInputStream.java:2042)
        at java.io.ObjectInputStream.readObject0(ObjectInputStream.java:1573)
```

## <a name="cause"></a>Nedeni

Bu hata, farklı bir `spark.yarn.jars` `commons-lang3` paket sürümü içeren ve bir sınıf uyumsuzluğu sunan "gölgeli" jar olan yapılandırmaya ek bir jar eklenerek oluşabilir. Varsayılan olarak, Spark 2.1/2/3 sürüm 3,5 `commons-lang3`' ü kullanır.

## <a name="resolution"></a>Çözüm

Jar 'yi kaldırın veya özelleştirilmiş jar 'yi (AzureLogAppender) yeniden derleyin ve sınıfları yeniden konumlandırmak için [Maven-gölge-Plugin](https://maven.apache.org/plugins/maven-shade-plugin/examples/class-relocation.html) kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* Azure Community [@AzureSupport](https://twitter.com/azuresupport) 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
