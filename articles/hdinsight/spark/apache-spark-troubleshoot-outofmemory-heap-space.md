---
title: Azure HDInsight 'ta Apache Spark geçmişi sunucusunu açmaya çalışırken Java yığın alanı hatası
description: Apache Livy sunucusu, Azure HDInsight 'ta Java. lang. OutOfMemoryError ile başlayamaz
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/30/2019
ms.openlocfilehash: 5c2ae90bdca8512802c845a5ac58c3c4aeedd5b7
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68667798"
---
# <a name="scenario-java-heap-space-error-when-trying-to-open-apache-spark-history-server-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta Apache Spark geçmişi sunucusunu açmaya çalışırken Java yığın alanı hatası

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorunlar için sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Spark geçmiş sunucusunda olayları açarken aşağıdaki hatayı alıyorsunuz:

```
scala.MatchError: java.lang.OutOfMemoryError: Java heap space (of class java.lang.OutOfMemoryError)
```

## <a name="cause"></a>Nedeni

Bu sorun genellikle büyük Spark-Event dosyalarını açarken kaynak yetersizliği nedeniyle oluşur. Spark yığın boyutu varsayılan olarak 1 GB olarak ayarlanır, ancak büyük Spark olay dosyaları bundan daha fazlasını gerektirebilir.

Yüklemeye çalıştığınız dosyaların boyutunu doğrulamak isterseniz, aşağıdaki komutları gerçekleştirebilirsiniz:.

```bash
hadoop fs -du -s -h wasb:///hdp/spark2-events/application_1503957839788_0274_1/
**576.5 M**  wasb:///hdp/spark2-events/application_1503957839788_0274_1

hadoop fs -du -s -h wasb:///hdp/spark2-events/application_1503957839788_0264_1/
**2.1 G**  wasb:///hdp/spark2-events/application_1503957839788_0264_1
```

## <a name="resolution"></a>Çözüm

Spark yapılandırmasındaki `SPARK_DAEMON_MEMORY` özelliği düzenleyerek Spark geçmiş sunucu belleğini artırabilir ve tüm hizmetleri yeniden başlatabilirsiniz.

Bunu, Spark2/config/Advanced Spark2-env bölümünü seçerek, ambarı tarayıcısı kullanıcı arabiriminden yapabilirsiniz.

![Advanced spark2-env bölümü](./media/apache-spark-ts-outofmemory-heap-space/image01.png)

Spark geçmiş sunucusu belleğini 1G 'den 4g 'ye değiştirmek için aşağıdaki özelliği ekleyin: `SPARK_DAEMON_MEMORY=4g`.

![Spark özelliği](./media/apache-spark-ts-outofmemory-heap-space/image02.png)

Tüm etkilenen hizmetleri ambarı 'ndan yeniden başlattığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* Azure Community [@AzureSupport](https://twitter.com/azuresupport) 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
