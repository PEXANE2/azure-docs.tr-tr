---
title: Azure HDInsight kullanarak HBase sorunlarını giderme
description: HBase ve Azure HDInsight ile çalışma hakkında sık sorulan soruların yanıtlarını alın.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.custom: hdinsightactive, seodec18
ms.topic: troubleshooting
ms.date: 08/14/2019
ms.openlocfilehash: 595acdc46aca76a86038acebdb9a7581c51e3688
ms.sourcegitcommit: 5ded08785546f4a687c2f76b2b871bbe802e7dae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69573949"
---
# <a name="troubleshoot-apache-hbase-by-using-azure-hdinsight"></a>Azure HDInsight kullanarak Apache HBase sorunlarını giderme

Apache ambarı 'nda Apache HBase yükleri ile çalışırken en popüler sorunlar ve çözümleri hakkında bilgi edinin.

## <a name="how-do-i-run-hbck-command-reports-with-multiple-unassigned-regions"></a>Nasıl yaparım? birden çok atanmamış bölgeyle hbck komut raporları çalıştırmak istiyor musunuz?

`hbase hbck` Komutu çalıştırdığınızda görebileceğiniz yaygın bir hata iletisi, "birden fazla bölge atanmakta veya bölge zincirindeki delikler." şeklindedir.

HBase Master Kullanıcı arabiriminde, tüm bölge sunucularında dengesiz olan bölge sayısını görebilirsiniz. Ardından, bölge zincirindeki delikleri `hbase hbck` görmek için komutunu çalıştırabilirsiniz.

Delik, çevrimdışı bölgelerden kaynaklanıyor olabilir, bu nedenle öncelikle atamaları düzeltir. 

Atanmamış bölgeleri normal duruma getirmek için aşağıdaki adımları izleyin:

1. SSH kullanarak HDInsight HBase kümesinde oturum açın.
2. Apache ZooKeeper Shell ile bağlanmak için `hbase zkcli` komutunu çalıştırın.
3. `rmr /hbase/regions-in-transition` Komutunu`rmr /hbase-unsecure/regions-in-transition` veya komutunu çalıştırın.
4. `hbase zkcli` Kabuktan çıkmak için `exit` komutunu kullanın.
5. Apache ambarı Kullanıcı arabirimini açın ve ardından etkin HBase Master hizmetini yeniden başlatın.
6. `hbase hbck` Komutu yeniden çalıştırın (herhangi bir seçenek olmadan). Tüm bölgelerin atandığından emin olmak için bu komutun çıkışını kontrol edin.


## <a name="how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments"></a>Bölge atamaları için hbck komutları kullanılırken zaman aşımı sorunlarını gidermek Nasıl yaparım? mı?

### <a name="issue"></a>Sorun

`hbck` Komutu kullandığınızda zaman aşımı sorunları için olası bir neden, "geçiş sürüyor" durumunda birkaç bölgenin uzun bir süre olması olabilir. Bu bölgeleri HBase Master Kullanıcı arabiriminde çevrimdışı olarak görebilirsiniz. Çok sayıda bölge geçişe çalıştığından HBase Master zaman aşımına uğrayabilir ve bu bölgeleri yeniden çevrimiçi hale getiremeyebilir.

### <a name="resolution-steps"></a>Çözüm adımları

1. SSH kullanarak HDInsight HBase kümesinde oturum açın.
2. Apache ZooKeeper Shell ile bağlanmak için `hbase zkcli` komutunu çalıştırın.
3. `rmr /hbase/regions-in-transition` Veya`rmr /hbase-unsecure/regions-in-transition` komutunu çalıştırın.
4. `hbase zkcli` Kabuktan çıkmak için `exit` komutunu kullanın.
5. Ambarı Kullanıcı arabiriminde, etkin HBase Master hizmetini yeniden başlatın.
6. `hbase hbck -fixAssignments` Komutu yeniden çalıştırın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabına bağlanın. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
