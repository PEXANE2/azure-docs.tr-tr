---
title: Apache Spark için IllegalArgumentExceptionException hatası - Azure HDInsight
description: Azure Veri Fabrikası için Azure HDInsight'ta Apache Spark etkinliği için IllegalArgumentException
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: df62dbd8db7d41eb11207c7741aed76cec0ac7a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75894375"
---
# <a name="scenario-illegalargumentexception-for-apache-spark-activity-in-azure-hdinsight"></a>Senaryo: Azure HDInsight'ta Apache Spark etkinliği için IllegalArgumentExceptionException

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorun giderme adımları ve sorunların olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Bir Azure Veri Fabrikası ardışık hattında bir Spark etkinliğini yürütmeye çalışırken aşağıdaki özel durum alırsınız:

```error
Exception in thread "main" java.lang.IllegalArgumentException:
Wrong FS: wasbs://additional@xxx.blob.core.windows.net/spark-examples_2.11-2.1.0.jar, expected: wasbs://wasbsrepro-2017-11-07t00-59-42-722z@xxx.blob.core.windows.net
```

## <a name="cause"></a>Nedeni

Uygulama jar dosyası Spark kümesinin varsayılan/birincil depolama alanında bulunmazsa, Bir Kıvılcım işi başarısız olur.

Bu hata da izlenen Spark açık kaynak çerçevesi ile bilinen bir sorundur: [Fs.defaultFS ve uygulama kavanozu farklı url ise Kıvılcım iş başarısız olur.](https://issues.apache.org/jira/browse/SPARK-22587)

Bu sorun Spark 2.3.0'da çözüldü.

## <a name="resolution"></a>Çözüm

Uygulama kavanozunun HDInsight kümesi için varsayılan/birincil depolama da depolandıkdığından emin olun. Azure Veri Fabrikası durumunda, ADF bağlantılı hizmetin ikincil bir kapsayıcı yerine HDInsight varsayılan kapsayıcısına işaret aldığından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
