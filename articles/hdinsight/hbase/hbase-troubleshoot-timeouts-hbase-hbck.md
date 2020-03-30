---
title: Azure HDInsight'ta 'hbase hbck' komutuyla zaman aşımları
description: Bölge atamaları düzeltirken 'hbase hbck' komutu ile zaman çıkış sorunu
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/16/2019
ms.openlocfilehash: 5604b42e1611830f3aaea9ae180cdb8142ab0942
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75887198"
---
# <a name="scenario-timeouts-with-hbase-hbck-command-in-azure-hdinsight"></a>Senaryo: Azure HDInsight'ta 'hbase hbck' komutu ile zaman ekmeleri

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Bölge atamalarını `hbase hbck` sabitlerken komutla zaman aralarıyla karşılaşın.

## <a name="cause"></a>Nedeni

`hbck` Komutu kullandığınızda zaman alabilen sorunlar için olası bir neden, birkaç bölgenin uzun süre "geçiş" durumunda olması olabilir. Bu bölgeleri HBase Master UI'da çevrimdışı olarak görebilirsiniz. Çok sayıda bölge geçiş yapmaya çalıştığı için, HBase Master zaman kaybedebilir ve bu bölgeleri tekrar çevrimiçi duruma getiremeyebilir.

## <a name="resolution"></a>Çözüm

1. SSH kullanarak HDInsight HBase kümesinde oturum açın.

1. Apache ZooKeeper kabuğuile bağlanmak için komutu çalıştırın. `hbase zkcli`

1. Çalıştır `rmr /hbase/regions-in-transition` `rmr /hbase-unsecure/regions-in-transition` ın veya komutedin.

1. Komutu `hbase zkcli` kullanarak `exit` kabuktan çıkın.

1. Apache Ambari UI'den Active HBase Master hizmetini yeniden başlatın.

1. `hbase hbck -fixAssignments` komutunu çalıştırın.

1. Hiçbir bölgenin takılıp kalmadığından emin olmak için bu bölümü HBase Master UI "geçiş bölgesi" izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

- [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

- [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

- Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
