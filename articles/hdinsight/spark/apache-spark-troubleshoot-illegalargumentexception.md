---
title: Azure HDInsight 'ta Apache Spark etkinliği için ınmıgalargumentexception
description: Azure Data Factory için Azure HDInsight 'ta Apache Spark etkinliği için ınmıgalargumentexception
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: 57ba285b7de34dd548128b1f58644a32e153d056
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71087154"
---
# <a name="scenario-illegalargumentexception-for-apache-spark-activity-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta Apache Spark etkinliği için ınmıgalargumentexception

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorunlar için sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Bir Azure Data Factory ardışık düzeninde Spark etkinliği yürütmeye çalışırken aşağıdaki özel durumu alırsınız:

```error
Exception in thread "main" java.lang.IllegalArgumentException:
Wrong FS: wasbs://additional@xxx.blob.core.windows.net/spark-examples_2.11-2.1.0.jar, expected: wasbs://wasbsrepro-2017-11-07t00-59-42-722z@xxx.blob.core.windows.net
```

## <a name="cause"></a>Nedeni

Uygulama jar dosyası Spark kümesinin varsayılan/birincil depolama alanında bulunmuyorsa bir Spark işi başarısız olur.

Bu, Spark açık kaynaklı çerçevesinin Bu hatada izlenen bilinen bir sorundur: [FS. defaultFS ve Application jar farklı URL Ise Spark işi başarısız olur](https://issues.apache.org/jira/browse/SPARK-22587).

Bu sorun Spark 2.3.0 'te çözüldü.

## <a name="resolution"></a>Çözüm

Application jar 'in HDInsight kümesi için varsayılan/birincil depolama alanı üzerinde depolandığından emin olun. Azure Data Factory durumda, ADF bağlantılı hizmetin ikincil bir kapsayıcı yerine HDInsight varsayılan kapsayıcısına işaret ettiği emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* Azure Community [@AzureSupport](https://twitter.com/azuresupport) 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
