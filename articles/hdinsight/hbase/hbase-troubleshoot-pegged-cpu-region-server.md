---
title: Azure HDInsight 'ta Apache HBase kümesindeki bölge sunucusunda gereğinden CPU
description: Azure HDInsight 'ta Apache HBase kümesindeki bölge sunucusunda gereğinden CPU
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/01/2019
ms.openlocfilehash: 3c48671595b0d7d7013519f59db5204272bf8b27
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68781380"
---
# <a name="scenario-pegged-cpu-on-region-server-in-apache-hbase-cluster-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta Apache HBase kümesindeki bölge sunucusunda gereğinden CPU

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache HBase bölge sunucusu işlemi,% 200 CPU kadar ücretsiz olarak başlıyor, bu da uyarıların HBase Master işlem ve küme üzerinde tetiklenmesine neden olur.

## <a name="cause"></a>Nedeni

HBase kümesi v 3.4 çalıştırıyorsanız, JDK 'nin 1.7.0 _151 sürümüne yükseltilmesiyle kaynaklanan olası bir hata nedeniyle bu hatayla karşılaşabilirsiniz. Belirlediğimiz belirti, bölge sunucusu işlemi% 200 CPU 'ya yakın bir şekilde başlıyor (Bunu doğrulamak için, bu `top` komutu çalıştırın.% 200 ' e yakın bir işlem varsa, PID 'sini alın ve çalıştırılarak `ps -aux | grep` bölge sunucusu işlemi olduğunu onaylayın).

## <a name="resolution"></a>Çözüm

1. Aşağıdaki gibi, tüm küme düğümlerine JDK 1,8 ' ü yükler:

    * Betik eylemini `https://raw.githubusercontent.com/Azure/hbase-utils/master/scripts/upgradetojdk18allnodes.sh`çalıştırın. Tüm düğümlerde çalıştırma seçeneğini seçtiğinizden emin olun.

    * Alternatif olarak, her bir düğümde oturum açabilir ve komutunu `sudo add-apt-repository ppa:openjdk-r/ppa -y && sudo apt-get -y update && sudo apt-get install -y openjdk-8-jdk`çalıştırabilirsiniz.

1. Ambarı Kullanıcı arabirimine `https://<clusterdnsname>.azurehdinsight.net`gidin.

1. **HBase-> configs > Gelişmiş > Gelişmiş** `hbase-env configs` ' e gidin ve değişkeni `JAVA_HOME` olarak `export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64`değiştirin. Yapılandırma değişikliğini kaydedin.

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

* Azure Community [@AzureSupport](https://twitter.com/azuresupport) 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
