---
title: Apache HBase kümesinde pegged CPU - Azure HDInsight
description: Azure HDInsight'ta Apache HBase kümesinde bölge sunucusunda saptanmış CPU'yu sorun giderme
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/01/2019
ms.openlocfilehash: 16c994029e91d743f1c2a7e2eab51eb86fc378e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75887317"
---
# <a name="scenario-pegged-cpu-on-region-server-in-apache-hbase-cluster-in-azure-hdinsight"></a>Senaryo: Azure HDInsight'ta Apache HBase kümesinde bölge sunucusunda pegged CPU

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache HBase bölge sunucu işlemi % 200'e yakın CPU'yu işgal etmeye başlar ve uyarıların HBase Master işleminde ve kümede tam kapasitede çalışmamasına neden olur.

## <a name="cause"></a>Nedeni

HBase kümesi v3.4 çalıştırıyorsanız, sürüm 1.7.0_151 jdk yükseltme nedeniyle olası bir hata tarafından vuruldu olabilir. Gördüğümüz belirti bölge sunucu işlemi % 200 CPU'ya yakın işgal `top` başlar (bu komutu çalıştırmak doğrulamak için; %200'e yakın CPU işgal `ps -aux | grep` eden bir işlem varsa pid almak ve çalıştırarak bölge sunucusu işlemi olduğunu onaylamak).

## <a name="resolution"></a>Çözüm

1. Aşağıdaki gibi kümenin TÜM düğümleri jdk 1.8 yükleyin:

    * Komut dosyası `https://raw.githubusercontent.com/Azure/hbase-utils/master/scripts/upgradetojdk18allnodes.sh`eylemini çalıştırın. Tüm düğümlerde çalıştırma seçeneğini seçtiğinizden emin olun.

    * Alternatif olarak, her bir düğüm oturum açabilir ve `sudo add-apt-repository ppa:openjdk-r/ppa -y && sudo apt-get -y update && sudo apt-get install -y openjdk-8-jdk`komutu çalıştırın.

1. Ambari UI gidin `https://<clusterdnsname>.azurehdinsight.net`- .

1. **HBase->Configs->Advanced->Advanced'e** `hbase-env configs` `JAVA_HOME` gidin `export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64`ve değişkeni '' ye değiştirin. Config değişikliğini kaydedin.

1. [İsteğe bağlı ama önerilen] [Kümedeki tüm tabloları temizle.](https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/)

1. Ambari UI'den yeniden başlatılması gereken tüm HBase hizmetlerini yeniden başlatın.

1. Kümedeki verilere bağlı olarak, kümenin kararlı duruma ulaşması birkaç dakika ile bir saat arasında sürebilir. Kümenin kararlı duruma ulaştığını onaylamanın yolu, Ambari'den (yenileme) HMaster Kullanıcı Arabirimi'ni (tüm bölge sunucuları etkin olmalıdır) veya kafa düğümü çalıştırHBase kabuğundan kontrol etmek ve ardından durum komutunu çalıştırmaktır.

Yükseltmenizin başarılı olduğunu doğrulamak için, ilgili HBase işlemlerinin uygun java sürümünü kullanmaya başlatıldığından emin olun - örneğin bölge sunucusu denetimi aşağıdaki gibi:

```
ps -aux | grep regionserver, and verify the version like '''/usr/lib/jvm/java-8-openjdk-amd64/bin/java
```

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
