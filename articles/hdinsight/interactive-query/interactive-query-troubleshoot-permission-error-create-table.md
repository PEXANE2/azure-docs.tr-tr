---
title: Azure HDInsight 'ta Apache Hive tablo ile izin reddedildi hatası
description: Azure HDInsight 'ta bir Apache Hive tablosu oluşturulmaya çalışılırken izin reddedildi hatası
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/09/2019
ms.openlocfilehash: b6e71f5f2c389926a2e6267fadeedebdce1c51d9
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93288890"
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

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]