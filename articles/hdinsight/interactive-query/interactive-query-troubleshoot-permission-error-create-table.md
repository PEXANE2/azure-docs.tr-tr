---
title: Azure HDInsight 'ta bir Apache Hive tablosu oluşturulmaya çalışılırken izin reddedildi hatası
description: Azure HDInsight 'ta bir Apache Hive tablosu oluşturulmaya çalışılırken izin reddedildi hatası
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/09/2019
ms.openlocfilehash: 139c9a8c89b18588ee6e640feb105f06ff94ae5f
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2019
ms.locfileid: "68947740"
---
# <a name="scenario-permission-denied-error-when-trying-to-create-an-apache-hive-table-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta bir Apache Hive tablosu oluşturulmaya çalışılırken izin reddedildi hatası

Bu makalede, Azure HDInsight kümelerinde etkileşimli sorgu bileşenlerini kullanırken sorunlar için sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Tablo oluşturmaya çalışırken şu hatayı görürsünüz:

```
java.sql.SQLException: Error while compiling statement: FAILED: HiveAccessControlException Permission denied: user [hdiuser] does not have [ALL] privilege on [wasbs://data@xxxxx.blob.core.windows.net/path/table]
```

Aşağıdaki, şu işlem depolama komutunu çalıştırırsanız benzer bir hata iletisi görürsünüz:

```
hdfs dfs -mkdir wasbs://data@xxxxx.blob.core.windows.net/path/table
```

## <a name="cause"></a>Nedeni

Apache Hive bir tablo oluşturma özelliği, kümenin depolama hesabına uygulanan izinler tarafından belirlenir. Küme depolama hesabı izinleri yanlış ise, tablo oluşturabileceksiniz. Bu, tablo oluşturma için doğru Ranger ilkelerine sahip olabileceği ve yine de "Izin reddedildi" hataları göremeyeceğiniz anlamına gelir.

## <a name="resolution"></a>Çözüm

Bu, kullanılan depolama kapsayıcısı üzerinde yeterli izinlerin bulunmaması nedeniyle oluşur. Hive tablosunu oluşturan kullanıcının kapsayıcıya karşı okuma, yazma ve yürütme izinleri olması gerekir. Daha fazla bilgi için bkz. [HDP 2,2 ' de Apache Ranger kullanarak Hive yetkilendirmesi Için En Iyi uygulamalar](https://hortonworks.com/blog/best-practices-for-hive-authorization-using-apache-ranger-in-hdp-2-2/).

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* Azure Community [@AzureSupport](https://twitter.com/azuresupport) 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
