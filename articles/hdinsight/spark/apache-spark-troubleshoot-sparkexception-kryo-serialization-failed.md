---
title: JDBC/ODBC ve Apache Thrift çerçevesini kullanarak sorunları indirin-Azure HDInsight
description: Azure HDInsight 'ta JDBC/ODBC ve Apache Thrift yazılım çerçevesi kullanılarak büyük veri kümeleri indirilemedi
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/29/2019
ms.openlocfilehash: 97aaec9d42baa3e94be72a748e82345d159f4583
ms.sourcegitcommit: 97605f3e7ff9b6f74e81f327edd19aefe79135d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70736236"
---
# <a name="scenario-unable-to-download-large-data-sets-using-jdbcodbc-and-apache-thrift-software-framework-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta JDBC/ODBC ve Apache Thrift yazılım çerçevesi kullanılarak büyük veri kümeleri indirilemedi

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorunlar için sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Azure HDInsight 'ta JDBC/ODBC ve Apache Thrift yazılım çerçevesini kullanarak büyük veri kümelerini indirmeye çalışırken, aşağıdakine benzer bir hata iletisi alırsınız:

```
org.apache.spark.SparkException: Kryo serialization failed:
Buffer overflow. Available: 0, required: 36518. To avoid this, increase spark.kryoserializer.buffer.max value.
```

## <a name="cause"></a>Nedeni

Bu özel durum, serileştirme işleminin izin verilenden daha fazla arabellek alanı kullanmaya çalışılması nedeniyle oluşur. Spark 2.0.0 içinde, veri Apache `org.apache.spark.serializer.KryoSerializer` Thrift yazılım çerçevesi aracılığıyla erişildiğinde nesneleri serileştirmek için sınıfı kullanılır. Ağ üzerinden gönderilecek veya seri hale getirilmiş biçimde önbelleğe alınacak veriler için farklı bir sınıf kullanılır.

## <a name="resolution"></a>Çözüm

`Kryoserializer` Arabellek değerini artırın. Adlı `spark.kryoserializer.buffer.max` bir anahtar ekleyin ve altında `Custom spark2-thrift-sparkconf`spark2 config `2048` içine ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* Azure Community [@AzureSupport](https://twitter.com/azuresupport) 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
