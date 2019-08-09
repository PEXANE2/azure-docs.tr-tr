---
title: Azure HDInsight 'ta bağlantı sorunlarını Apache Phoenix
description: Azure HDInsight 'ta bağlantı sorunlarını Apache Phoenix
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/07/2019
ms.openlocfilehash: 641d622377bad7a1239efd526b93c6f0f0c08d4a
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68887046"
---
# <a name="scenario-apache-phoenix-connectivity-issues-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta bağlantı sorunlarını Apache Phoenix

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache Phoenix ile Apache HBase bağlantısı kurulamıyor. Nedenler farklılık gösterebilir.

## <a name="cause-incorrect-ip"></a>Neden: Yanlış IP

Etkin Zookeeper düğümünün yanlış IP 'si.

### <a name="resolution"></a>Çözüm

Active Zookeeper düğümünün IP 'si, **HBase-> hızlı bağlantıları-> ZK***  **(etkin)-> Zookeeper Info**bağlantılarını izleyerek, ambarı kullanıcı arabiriminden belirlenebilir. Gerektiğinde düzeltin.

---

## <a name="cause-systemcatalog-table-offline"></a>Neden: Sistemin. Katalog tablosu çevrimdışı

Gibi komutları `!tables`çalıştırırken şuna benzer bir hata iletisi alırsınız:

```
Error while connecting to sqlline.py (Hbase - phoenix) Setting property: [isolation, TRANSACTION_READ_COMMITTED] issuing: !connect jdbc:phoenix:10.2.0.7 none none org.apache.phoenix.jdbc.PhoenixDriver Connecting to jdbc:phoenix:10.2.0.7 SLF4J: Class path contains multiple SLF4J bindings.
```

Gibi komutları `count 'SYSTEM.CATALOG'`çalıştırırken şuna benzer bir hata iletisi alırsınız:

```
ERROR: org.apache.hadoop.hbase.NotServingRegionException: Region SYSTEM.CATALOG,,1485464083256.c0568c94033870c517ed36c45da98129. is not online on 10.2.0.5,16020,1489466172189)
```

### <a name="resolution"></a>Çözüm

Tüm Zookeeper düğümlerinde HMaster hizmetini, ambarı kullanıcı arabiriminden yeniden başlatın.

1. HBase 'in Özet bölümünde **HBase-> Active HBase Master** bağlantısına gidin.

1. **Bileşenler** bölümünde HBase Master hizmetini yeniden başlatın.

1. Kalan **bekleme HBase Master** Hizmetleri için yukarıdaki adımları tekrarlayın.

HBase Master hizmetin kurtarma işleminin kararlı hale gelmesi ve tamamlanması 5 dakika kadar sürebilir. `SYSTEM.CATALOG` Tablo normal 'e geri yüklendikten sonra Apache Phoenix bağlantı sorunu otomatik olarak çözümlenmelidir.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabına bağlanın. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
