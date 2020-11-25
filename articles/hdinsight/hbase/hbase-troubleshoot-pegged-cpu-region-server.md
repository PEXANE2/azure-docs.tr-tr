---
title: Apache HBase kümesinde gereğinden CPU-Azure HDInsight
description: Azure HDInsight 'ta Apache HBase kümesindeki bölge sunucusunda gereğinden CPU sorunlarını giderme
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/01/2019
ms.openlocfilehash: bed73c3ccc7f514ffc9ff8f97534ae4b249834ce
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96017024"
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

1. [İsteğe bağlı ancak önerilir] [Kümedeki tüm tabloları temizler](/archive/blogs/azuredatalake/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables).

1. Ambarı kullanıcı arabiriminden yeniden başlatmak için gereken tüm HBase hizmetlerini yeniden başlatın.

1. Kümedeki verilere bağlı olarak, kümenin kararlı duruma ulaşması birkaç dakika sürebilir. Kümenin kararlı duruma ulaşmasını sağlamanın yolu, HMaster Kullanıcı arabirimini (tüm bölge sunucularının etkin olması), ambarı (yenileme) veya yayın düğümünden HBase kabuğu 'nu çalıştırıp durum komutunu çalıştırır.

Yükseltmenin başarılı olduğunu doğrulamak için ilgili HBase işlemlerinin, bölge sunucusu için uygun Java sürümü kullanılarak başlatıldığını kontrol edin:

```
ps -aux | grep regionserver, and verify the version like '''/usr/lib/jvm/java-8-openjdk-amd64/bin/java
```

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]