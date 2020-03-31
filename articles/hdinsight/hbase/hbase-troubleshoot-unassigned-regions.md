---
title: Azure HDInsight'ta bölge sunucularıyla ilgili sorunlar
description: Azure HDInsight'ta bölge sunucularıyla ilgili sorunlar
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/16/2019
ms.openlocfilehash: 85aeafb2c4461b50d399e40d9abff2ac04b677c0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79272766"
---
# <a name="issues-with-region-servers-in-azure-hdinsight"></a>Azure HDInsight'ta bölge sunucularıyla ilgili sorunlar

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="scenario-unassigned-regions"></a>Senaryo: Atanmamış bölgeler

### <a name="issue"></a>Sorun

Komutu `hbase hbck` çalıştırırken, şuna benzer bir hata iletisi görürsünüz:

```
multiple regions being unassigned or holes in the chain of regions
```

Apache HBase Master UI'den, tüm bölge sunucuları arasında dengesiz bölge sayısını görebilirsiniz. Daha sonra, `hbase hbck` bölge zincirindeki delikleri görmek için komutu çalıştırabilirsiniz.

### <a name="cause"></a>Nedeni

Delikler çevrimdışı bölgelerin sonucu olabilir.

### <a name="resolution"></a>Çözüm

Atamaları düzeltin. Atanamayan bölgeleri normal duruma getirmek için aşağıdaki adımları izleyin:

1. SSH kullanarak HDInsight HBase kümesinde oturum açın.

1. ZooKeeper kabuğuna bağlanmak için komutu çalıştırın. `hbase zkcli`

1. Çalıştır `rmr /hbase/regions-in-transition` `rmr /hbase-unsecure/regions-in-transition` ın veya komutedin.

1. Komutu kullanarak `exit` zookeeper kabuğundan çıkın.

1. Apache Ambari UI'yi açın ve ardından Active HBase Master hizmetini yeniden başlatın.

1. Komutu yeniden çalıştırın `hbase hbck` (başka seçenek olmadan). Çıktıyı denetleyin ve tüm bölgelerin atandığından emin olun.

---

## <a name="scenario-dead-region-servers"></a>Senaryo: Ölü bölge sunucuları

### <a name="issue"></a>Sorun

Bölge sunucuları başlatılmaz.

### <a name="cause"></a>Nedeni

Birden fazla bölme WAL dizinleri.

1. Geçerli WAL'lerin listesini `hadoop fs -ls -R /hbase/WALs/ > /tmp/wals.out`alın: .

1. Dosyayı `wals.out` inceleyin. Çok fazla bölme dizini varsa (*-bölmeyle başlayarak), bölge sunucusu büyük olasılıkla bu dizinler nedeniyle başarısız olur.

### <a name="resolution"></a>Çözüm

1. Ambari portalından HBase'i durdurun.

1. WALs taze listesini almak için çalıştırın. `hadoop fs -ls -R /hbase/WALs/ > /tmp/wals.out`

1. *-bölme dizinlerini geçici bir `splitWAL`klasöre taşıyın ve *-splitting dizinlerini silin.

1. Zookeeper kabuk ile bağlanmak için komutu çalıştırın. `hbase zkcli`

1. Çalıştırın. `rmr /hbase-unsecure/splitWAL`

1. HBase hizmetini yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
