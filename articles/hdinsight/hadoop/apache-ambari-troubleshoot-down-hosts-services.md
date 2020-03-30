---
title: Apache Ambari UI, Azure HDInsight'ta ev sahiplerini ve hizmetleri gösterir
description: Azure HDInsight'ta ev sahiplerini ve hizmetleri gösterirken Apache Ambari UI sorununu sorun giderme
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/02/2019
ms.openlocfilehash: 5340b1c7a6510595376789bc5777e6fb6f07dd4a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75895647"
---
# <a name="scenario-apache-ambari-ui-shows-down-hosts-and-services-in-azure-hdinsight"></a>Senaryo: Apache Ambari UI, Azure HDInsight'ta ev sahiplerini ve hizmetleri gösterir

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache Ambari UI erişilebilir, ancak UI hemen hemen tüm hizmetleri aşağı, tüm hosts kalp atışı kayıp gösteren gösterir.

## <a name="cause"></a>Nedeni

Çoğu senaryoda, bu Ambari sunucusunun etkin başlıküzerinde çalışmamasıyla ilgili bir sorundur. Hangi başlık nodonun etkin başlık olduğunu kontrol edin ve ambari sunucunuzun doğru anda çalıştığından emin olun. Ambari-server'ı el ile başlatmayın, failover denetleyici hizmetinin sağ başlıkta ambari-server'ı başlatmasorumlusuna izin verin. Bir başarısızlık zorlamak için aktif headnode yeniden başlatın.

Ağ sorunları da bu soruna neden olabilir. Her küme düğümünden ping olup `headnodehost`olmadığını görmek. Hiçbir küme düğümüne `headnodehost`bağlanamayacağı nadir bir durum vardır:

```
$>telnet headnodehost 8440
... No route to host
```

## <a name="resolution"></a>Çözüm

Genellikle etkin headnode yeniden başlatılması bu sorunu azaltmak olacaktır. Değilse lütfen HDInsight destek ekibine başvurun.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
