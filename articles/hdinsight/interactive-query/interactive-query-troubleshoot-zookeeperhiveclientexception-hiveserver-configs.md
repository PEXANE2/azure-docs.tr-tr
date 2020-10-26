---
title: Apache Hive Zeppelin yorumlayıcı hatası-Azure HDInsight
description: Apache Zeppelin Hive JDBC yorumlayıcı, Azure HDInsight 'ta yanlış URL 'yi işaret ediyor
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: a5dd7d9f292036af00140d89347678e5e548b8d9
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92534643"
---
# <a name="scenario-apache-hive-zeppelin-interpreter-gives-a-zookeeper-error-in-azure-hdinsight"></a>Senaryo: Apache Hive Zeppelin yorumlayıcı, Azure HDInsight 'ta bir Zookeeper hatası veriyor

Bu makalede, Azure HDInsight kümelerinde etkileşimli sorgu bileşenlerini kullanırken sorunlar için sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache Hive LLAP kümesinde, Zeppelin yorumlayıcı bir sorguyu yürütmeye çalışırken aşağıdaki hata iletisini verir:

```
java.sql.SQLException: org.apache.hive.jdbc.ZooKeeperHiveClientException: Unable to read HiveServer2 configs from ZooKeeper
```

## <a name="cause"></a>Nedeni

Zeppelin Hive JDBC yorumlayıcı yanlış URL 'yi işaret ediyor.

## <a name="resolution"></a>Çözüm

1. Hive bileşen Özeti ' ne gidin ve "Hive JDBC URL" öğesini panoya kopyalayın.

1. `https://clustername.azurehdinsight.net/zeppelin/#/interpreter` sayfasına gidin

1. JDBC ayarlarını düzenleyin: 1. adımda kopyalanmış olan Hive JDBC URL 'si için Hive. URL değerini güncelleştirin

1. Sorguyu kaydedin ve sonra yeniden deneyin

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport)Azure Community 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](../../azure-portal/supportability/how-to-create-azure-support-request.md)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.