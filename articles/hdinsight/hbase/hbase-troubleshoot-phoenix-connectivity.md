---
title: Azure HDInsight'ta Apache Phoenix bağlantı sorunları
description: Azure HDInsight'ta Apache HBase ve Apache Phoenix arasındaki bağlantı sorunları
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/14/2019
ms.openlocfilehash: b886f51bcb2bb7308c49c76563dcb70148bbc583
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75887300"
---
# <a name="scenario-apache-phoenix-connectivity-issues-in-azure-hdinsight"></a>Senaryo: Azure HDInsight'ta Apache Phoenix bağlantı sorunları

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache Phoenix ile Apache HBase'e bağlanamıyor. Nedenler değişebilir.

## <a name="cause-incorrect-ip"></a>Neden: Yanlış IP

Etkin Zookeeper düğümünün yanlış IP'si.

### <a name="resolution"></a>Çözüm

Aktif Zookeeper düğümün **IP'si, HBase** > **Hızlı Bağlantılar** > **ZK (Aktif)** > **Zookeeper Info**bağlantılarını izleyerek Ambari UI'den tanımlanabilir. IP'yi gerektiği gibi düzeltin.

---

## <a name="cause-systemcatalog-table-offline"></a>Neden: SİsteM. CATALOG tablosu çevrimdışı

`!tables`Gibi komutları çalıştırırken, benzer bir hata iletisi alırsınız:

```output
Error while connecting to sqlline.py (Hbase - phoenix) Setting property: [isolation, TRANSACTION_READ_COMMITTED] issuing: !connect jdbc:phoenix:10.2.0.7 none none org.apache.phoenix.jdbc.PhoenixDriver Connecting to jdbc:phoenix:10.2.0.7 SLF4J: Class path contains multiple SLF4J bindings.
```

`count 'SYSTEM.CATALOG'`Gibi komutları çalıştırırken, benzer bir hata iletisi alırsınız:

```output
ERROR: org.apache.hadoop.hbase.NotServingRegionException: Region SYSTEM.CATALOG,,1485464083256.c0568c94033870c517ed36c45da98129. is not online on 10.2.0.5,16020,1489466172189)
```

### <a name="resolution"></a>Çözüm

Apache Ambari UI'den, tüm ZooKeeper düğümlerinde HMaster hizmetini yeniden başlatmak için aşağıdaki adımları tamamlayın:

1. **HBase'in Özet** bölümünden **HBase** > **Active HBase Master'a**gidin.

1. **Bileşenler** bölümünden HBase Master hizmetini yeniden başlatın.

1. Kalan tüm Bekleme **HBase Master** hizmetleri için bu adımları yineleyin.

HBase Master hizmetinin stabilize etmesi ve kurtarmayı tamamlaması beş dakika kadar sürebilir. `SYSTEM.CATALOG` Tablo normale döndükten sonra, Apache Phoenix'e bağlantı sorunu otomatik olarak çözülmelidir.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
