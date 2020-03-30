---
title: Apache Hive Zeppelin Interpreter hatası - Azure HDInsight
description: Apache Zeppelin Hive JDBC Tercümanı Azure HDInsight'ta yanlış URL'yi işaret ediyor
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: 20309babb9ece0ae20e7442543b0d378f9a51060
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75895055"
---
# <a name="scenario-apache-hive-zeppelin-interpreter-gives-a-zookeeper-error-in-azure-hdinsight"></a>Senaryo: Apache Hive Zeppelin Interpreter Azure HDInsight bir Zookeeper hata verir

Bu makalede, Azure HDInsight kümelerinde Etkileşimli Sorgu bileşenleri kullanılırken sorun giderme adımları ve sorunların olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Bir Apache Hive LLAP kümesinde, Zeppelin yorumlayıcısı bir sorgu yürütmeye çalışırken aşağıdaki hata iletisini verir:

```
java.sql.SQLException: org.apache.hive.jdbc.ZooKeeperHiveClientException: Unable to read HiveServer2 configs from ZooKeeper
```

## <a name="cause"></a>Nedeni

Zeppelin Hive JDBC Tercüman yanlış URL işaret ediyor.

## <a name="resolution"></a>Çözüm

1. Hive bileşen özetine gidin ve "Hive JDBC Url"sini panoya kopyalayın.

1. Gezinme`https://clustername.azurehdinsight.net/zeppelin/#/interpreter`

1. JDBC ayarlarını değiştirin: Hive.url değerini adım 1'de kopyalanan Kovan JDBC URL'sine güncelleyin

1. Kaydet, ardından sorguyu yeniden deneyin

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
