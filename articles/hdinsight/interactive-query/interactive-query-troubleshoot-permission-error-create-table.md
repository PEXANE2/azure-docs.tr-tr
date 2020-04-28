---
title: Azure HDInsight 'ta Apache Hive tablo ile izin reddedildi hatası
description: Azure HDInsight 'ta bir Apache Hive tablosu oluşturulmaya çalışılırken izin reddedildi hatası
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/09/2019
ms.openlocfilehash: 8ebad9300c126193ddb96d5f07057b9a825dbfcd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75895153"
---
# <a name="scenario-permission-denied-error-when-trying-to-create-an-apache-hive-table-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta bir Apache Hive tablosu oluşturulmaya çalışılırken Izin reddedildi hatası

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

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
