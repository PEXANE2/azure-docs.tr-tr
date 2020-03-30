---
title: JDBC/ODBC & Apache Thrift çerçevesi ile ilgili sorunlar - Azure HDInsight
description: Azure HDInsight'ta JDBC/ODBC ve Apache Thrift yazılım çerçevesini kullanarak büyük veri kümelerini indiremiyor
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: 23693dcae2f361b88440ec88ca39fd8ed229d85a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75894270"
---
# <a name="unable-to-download-large-data-sets-using-jdbcodbc-and-apache-thrift-software-framework-in-hdinsight"></a>HDInsight'ta JDBC/ODBC ve Apache Thrift yazılım çerçevesini kullanarak büyük veri kümelerini indiremiyor

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorun giderme adımları ve sorunların olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Azure HDInsight'ta JDBC/ODBC ve Apache Thrift yazılım çerçevesini kullanarak büyük veri kümelerini indirmeye çalışırken aşağıdaki gibi bir hata iletisi alırsınız:

```
org.apache.spark.SparkException: Kryo serialization failed:
Buffer overflow. Available: 0, required: 36518. To avoid this, increase spark.kryoserializer.buffer.max value.
```

## <a name="cause"></a>Nedeni

Bu özel durum, izin verilenden daha fazla arabellek alanı kullanmaya çalışan serileştirme işleminden kaynaklanır. Spark 2.0.0'da, `org.apache.spark.serializer.KryoSerializer` verilere Apache Thrift yazılım çerçevesi üzerinden erişildiğinde sınıf nesneleri serihale getirmek için kullanılır. Ağ üzerinden gönderilecek veya seri biçiminde önbelleğe alınacak veriler için farklı bir sınıf kullanılır.

## <a name="resolution"></a>Çözüm

Arabellek `Kryoserializer` değerini artırın. Adlı `spark.kryoserializer.buffer.max` bir anahtar ekleyin `2048` ve altında `Custom spark2-thrift-sparkconf`spark2 config ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
