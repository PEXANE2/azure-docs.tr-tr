---
title: Azure HDInsight'ta WASB dosya işlemlerini hata ayıklama
description: Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımlarını ve sorunlariçin olası çözümleri açıklar.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 02/18/2020
ms.openlocfilehash: f1707c7f8d6324678c8bf5a470bbded1e58c719e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77470726"
---
# <a name="debug-wasb-file-operations-in-azure-hdinsight"></a>Azure HDInsight'ta WASB dosya işlemlerini hata ayıklama

WASB sürücüsünün Azure Depolama ile hangi işlemleri başlattığını anlamak isteyebileceğin zamanlar vardır. İstemci tarafı için WASB **sürücüsü, hata ayıklama** düzeyinde her dosya sistemi işlemi için günlükleri üretir. WASB sürücüsü günlük düzeyini kontrol etmek için log4j kullanır ve varsayılan **BILGI** düzeyidir. Azure Depolama sunucu tarafındaki analitik günlükleri için [Azure Depolama analitiği günlüğe bakın.](../../storage/common/storage-analytics-logging.md)

Üretilen bir günlük benzer görünecektir:

```log
18/05/13 04:15:55 DEBUG NativeAzureFileSystem: Moving wasb://xxx@yyy.blob.core.windows.net/user/livy/ulysses.txt/_temporary/0/_temporary/attempt_20180513041552_0000_m_000000_0/part-00000 to wasb://xxx@yyy.blob.core.windows.net/user/livy/ulysses.txt/part-00000
```

## <a name="turn-on-wasb-debug-log-for-file-operations"></a>Dosya işlemleri için WASB hata ayıklama günlüğünü açma

1. Bir web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net/#/main/services/SPARK2/configs`Kıvılcım `CLUSTERNAME` kümenizin adı olan yere gidin.

1. Gelişmiş **spark2-log4j-özellikleri**gidin.

    1. 'ye `log4j.appender.console.Threshold=DEBUG`değiştirin. `log4j.appender.console.Threshold=INFO`

    1. Ekle `log4j.logger.org.apache.hadoop.fs.azure.NativeAzureFileSystem=DEBUG`.

1. Gelişmiş **livy2-log4j-özellikleri**gidin.

    Ekle `log4j.logger.org.apache.hadoop.fs.azure.NativeAzureFileSystem=DEBUG`.

1. Değişiklikleri kaydedin.

## <a name="additional-logging"></a>Ek günlükler

Yukarıdaki günlükler, dosya sistemi işlemlerinin üst düzey anlaşılmasını sağlamalıdır. Yukarıdaki günlükler hala yararlı bilgiler vermiyorsa veya blob depolama api çağrılarını `fs.azure.storage.client.logging=true` araştırmak `core-site`istiyorsanız, . Bu ayar, wasb depolama sürücüsü için java sdk günlükleri etkinleştirecek ve blob depolama sunucusuna her arama yazdırır. Diski hızlı bir şekilde doldurabileceği nden ve işlemi yavaşlatabileceğinden, incelemelerden sonra ayarı kaldırın.

Arka uç Azure Veri Gölü tabanlıysa, bileşen için aşağıdaki log4j ayarını kullanın (örneğin, kıvılcım/tez/hdfs):

```
log4j.logger.com.microsoft.azure.datalake.store=ALL,adlsFile
log4j.additivity.com.microsoft.azure.datalake.store=true
log4j.appender.adlsFile=org.apache.log4j.FileAppender
log4j.appender.adlsFile.File=/var/log/adl/adl.log
log4j.appender.adlsFile.layout=org.apache.log4j.PatternLayout
log4j.appender.adlsFile.layout.ConversionPattern=%p\t%d{ISO8601}\t%r\t%c\t[%t]\t%m%n
```

Günlüklerin girişlerini `/var/log/adl/adl.log` arayın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
