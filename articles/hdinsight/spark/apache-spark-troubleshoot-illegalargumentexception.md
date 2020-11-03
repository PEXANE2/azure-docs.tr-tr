---
title: Apache Spark için ınmıgalargumentexception hatası-Azure HDInsight
description: Azure Data Factory için Azure HDInsight 'ta Apache Spark etkinliği için ınmıgalargumentexception
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: 61190a932389b429040d6b643db2dd2732b2c41d
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93287908"
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

Bu hata, Spark açık kaynak çerçevesi ile ilgili bilinen bir sorundur: [FS. defaultFS ve Application jar farklı URL Ise Spark işi başarısız olur](https://issues.apache.org/jira/browse/SPARK-22587).

Bu sorun Spark 2.3.0 'te çözüldü.

## <a name="resolution"></a>Çözüm

Application jar 'in HDInsight kümesi için varsayılan/birincil depolama alanı üzerinde depolandığından emin olun. Azure Data Factory durumda, ADF bağlantılı hizmetin ikincil bir kapsayıcı yerine HDInsight varsayılan kapsayıcısına işaret ettiği emin olun.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]