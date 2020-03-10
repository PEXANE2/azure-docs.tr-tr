---
title: Azure HDInsight 'ta bölge sunucularıyla ilgili sorunlar
description: Azure HDInsight 'ta bölge sunucularıyla ilgili sorunlar
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/16/2019
ms.openlocfilehash: 85aeafb2c4461b50d399e40d9abff2ac04b677c0
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78395124"
---
# <a name="issues-with-region-servers-in-azure-hdinsight"></a>Azure HDInsight 'ta bölge sunucularıyla ilgili sorunlar

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="scenario-unassigned-regions"></a>Senaryo: atanmamış bölgeler

### <a name="issue"></a>Sorun

`hbase hbck` komutu çalıştırılırken şuna benzer bir hata iletisi görürsünüz:

```
multiple regions being unassigned or holes in the chain of regions
```

Apache HBase Master kullanıcı arabiriminden, tüm bölge sunucularında dengesiz olan bölge sayısını görebilirsiniz. Ardından, bölge zincirindeki delikleri görmek için `hbase hbck` komutu çalıştırabilirsiniz.

### <a name="cause"></a>Nedeni

Boşluklar, çevrimdışı bölgelerin sonucu olabilir.

### <a name="resolution"></a>Çözüm

Atamaları düzeltir. Atanmamış bölgeleri normal duruma geri getirmek için aşağıdaki adımları izleyin:

1. SSH kullanarak HDInsight HBase kümesinde oturum açın.

1. ZooKeeper Shell ile bağlanmak için `hbase zkcli` komutunu çalıştırın.

1. `rmr /hbase/regions-in-transition` veya `rmr /hbase-unsecure/regions-in-transition` komutunu çalıştırın.

1. `exit` komutu kullanarak Zookeeper kabuğu 'ndan çıkın.

1. Apache ambarı Kullanıcı arabirimini açın ve ardından etkin HBase Master hizmetini yeniden başlatın.

1. `hbase hbck` komutunu yeniden çalıştırın (başka hiçbir seçenek olmadan). Çıktıyı denetleyin ve tüm bölgelerin atandığından emin olun.

---

## <a name="scenario-dead-region-servers"></a>Senaryo: ölü bölge sunucuları

### <a name="issue"></a>Sorun

Bölge sunucuları başlatılamadı.

### <a name="cause"></a>Nedeni

Birden çok bölme dizini.

1. Geçerli WALs listesini al: `hadoop fs -ls -R /hbase/WALs/ > /tmp/wals.out`.

1. `wals.out` dosyasını inceleyin. Çok fazla sayıda bölme dizini varsa (*-bölme ile başlayarak), bu dizinler nedeniyle bölge sunucusu muhtemelen başarısız olur.

### <a name="resolution"></a>Çözüm

1. Ambarı portalından HBase 'i durdurun.

1. WALs 'in yeni listesini almak için `hadoop fs -ls -R /hbase/WALs/ > /tmp/wals.out` yürütün.

1. *-Bölme dizinlerini geçici bir klasöre taşıyın, `splitWAL`ve *-bölme dizinlerini silin.

1. Zookeeper Shell ile bağlanmak için `hbase zkcli` komutunu yürütün.

1. `rmr /hbase-unsecure/splitWAL`yürütün.

1. HBase hizmetini yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport) ile bağlanma-müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabı. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
