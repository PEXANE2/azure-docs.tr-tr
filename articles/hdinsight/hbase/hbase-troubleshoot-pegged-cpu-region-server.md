---
title: Apache HBase kümesinde gereğinden CPU-Azure HDInsight
description: Azure HDInsight 'ta Apache HBase kümesindeki bölge sunucusunda gereğinden CPU sorunlarını giderme
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/01/2019
ms.openlocfilehash: 16c994029e91d743f1c2a7e2eab51eb86fc378e8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75887317"
---
# <a name="scenario-pegged-cpu-on-region-server-in-apache-hbase-cluster-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta Apache HBase kümesindeki bölge sunucusunda gereğinden CPU

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache HBase bölge sunucusu işlemi, %200 CPU kadar ücretsiz olarak başlıyor, bu da uyarıların HBase Master işlem ve küme üzerinde tetiklenmesine neden olur.

## <a name="cause"></a>Nedeni

HBase kümesi v 3.4 çalıştırıyorsanız, JDK 'nin 1.7.0 _151 sürümüne yükseltilmesiyle kaynaklanan olası bir hata nedeniyle bu hatayla karşılaşabilirsiniz. Belirlediğimiz belirti, bölge sunucusu işlemi %200 CPU 'ya yakın bir şekilde başlıyor (Bunu doğrulamak için, bu `top` komutu çalıştırın. %200 ' e yakın bir işlem varsa, PID 'sini alın ve çalıştırılarak bölge sunucusu işlemi olduğunu onaylayın `ps -aux | grep` ).

## <a name="resolution"></a>Çözüm

1. Aşağıdaki gibi, tüm küme düğümlerine JDK 1,8 ' ü yükler:

    * Betik eylemini çalıştırın `https://raw.githubusercontent.com/Azure/hbase-utils/master/scripts/upgradetojdk18allnodes.sh` . Tüm düğümlerde çalıştırma seçeneğini seçtiğinizden emin olun.

    * Alternatif olarak, her bir düğümde oturum açabilir ve komutunu çalıştırabilirsiniz `sudo add-apt-repository ppa:openjdk-r/ppa -y && sudo apt-get -y update && sudo apt-get install -y openjdk-8-jdk` .

1. Ambarı Kullanıcı arabirimine gidin `https://<clusterdnsname>.azurehdinsight.net` .

1. **HBase->configs >gelişmiş >gelişmiş** `hbase-env configs` ' e gidin ve değişkeni `JAVA_HOME` olarak değiştirin `export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64` . Yapılandırma değişikliğini kaydedin.

1. [İsteğe bağlı ancak önerilir] [Kümedeki tüm tabloları temizler](https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/).

1. Ambarı kullanıcı arabiriminden yeniden başlatmak için gereken tüm HBase hizmetlerini yeniden başlatın.

1. Kümedeki verilere bağlı olarak, kümenin kararlı duruma ulaşması birkaç dakika sürebilir. Kümenin kararlı duruma ulaşmasını sağlamanın yolu, HMaster Kullanıcı arabirimini (tüm bölge sunucularının etkin olması), ambarı (yenileme) veya yayın düğümünden HBase kabuğu 'nu çalıştırıp durum komutunu çalıştırır.

Yükseltmenin başarılı olduğunu doğrulamak için ilgili HBase işlemlerinin, bölge sunucusu için uygun Java sürümü kullanılarak başlatıldığını kontrol edin:

```
ps -aux | grep regionserver, and verify the version like '''/usr/lib/jvm/java-8-openjdk-amd64/bin/java
```

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport)Azure Community 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
