---
title: Apache Spark'tan GeçersizClassException hatası - Azure HDInsight
description: Azure HDInsight'ta Apache Spark işi, sınıf sürümü uyuşmazlığı Olan Geçersiz Sınıf Exception ile başarısız oluyor
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: be50f8716835b0842f854842e5340b0bb8594136
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75894361"
---
# <a name="apache-spark-job-fails-with-invalidclassexception-class-version-mismatch-in-azure-hdinsight"></a>Azure HDInsight'ta Apache Spark işi, sınıf sürümü uyuşmazlığı Olan Geçersiz Sınıf Exception ile başarısız oluyor

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorun giderme adımları ve sorunların olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Bir Spark 2.x kümesinde bir Apache Spark işi oluşturmaya çalışırsınız. Aşağıdakilere benzer bir hata ile başarısız olur:

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

Bu hata config ek bir kavanoz, `spark.yarn.jars` `commons-lang3` özellikle paketin farklı bir sürümünü içeren ve bir sınıf uyuşmazlığı tanıttı gölgeli bir kavanoz ekleyerek neden olabilir. Varsayılan olarak, Spark 2.1/2/3 sürüm `commons-lang3`3.5 kullanır.

> [!TIP]
> Bir kütüphane gölge için kendi kavanoz içine içeriğini koymak için, paketini değiştirerek. Bu, yeniden paketlemeden kütüphaneyi kendi kavanozunuza koyan kitaplığı paketlemeden farklıdır.

## <a name="resolution"></a>Çözüm

Kavanozu çıkarın veya özelleştirilmiş kavanozu (AzureLogAppender) yeniden derleyip sınıfları taşımak için [maven-shade eklentisini](https://maven.apache.org/plugins/maven-shade-plugin/examples/class-relocation.html) kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
