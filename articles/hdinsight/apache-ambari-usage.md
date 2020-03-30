---
title: Azure HDInsight'ta Apache Ambari kullanımı
description: Azure HDInsight'ta Apache Ambari'nin nasıl kullanıldığını niçin tartıştığım.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/05/2020
ms.openlocfilehash: 466c170985715be52a90d579c19ca23aefefe2e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77067402"
---
# <a name="apache-ambari-usage-in-azure-hdinsight"></a>Azure HDInsight'ta Apache Ambari kullanımı

HDInsight küme dağıtımı ve yönetimi için Apache Ambari kullanır. Ambari ajanları her düğümde çalışır (baş düğüm, işçi düğümü, zookeeper ve varsa kenar düğümü). Ambari sunucusu yalnızca kafa düğümünde çalışır (hn0 veya hn1). Ambari sunucusunun yalnızca bir örneği aynı anda çalışır. Bu HDInsight failover denetleyicisi tarafından kontrol edilir. Headnodes biri yeniden başlatma veya bakım için aşağı olduğunda, diğer headnode aktif olacak ve ikinci headnode Ambari sunucu başlatılacaktır.

Tüm küme yapılandırması [Ambari UI](./hdinsight-hadoop-manage-ambari.md)üzerinden yapılmalıdır, düğüm yeniden başlatıldığında herhangi bir yerel değişiklik üzerine yazılır.

## <a name="failover-controller-services"></a>Failover denetleyici hizmetleri

HDInsight arıza denetleyicisi, mevcut etkin kafa düğümüne işaret eden headnode ana bilgisayarının IP adresini güncellemekten de sorumludur. Tüm Ambari ajanları, durumunu ve kalp atışını headnode host'a bildirecek şekilde yapılandırıldı. Failover denetleyicisi kümedeki her düğümde çalışan bir hizmet kümesidir, eğer çalışmıyorlarsa, headnode failover doğru çalışmayabilir ve Ambari sunucusuna erişmeye çalışırken HTTP 502 ile sonuçlanır.

Hangi başlık etkin olup olmadığını kontrol etmek için, bir yol kümedeki düğümlerden birine `ping headnodehost` shsh, sonra çalıştırın ve iki headnodes ile IP karşılaştırın.

Başarısız denetleyici hizmetleri çalışmıyorsa, headnode failover doğru olmayabilir ve bu da Ambari sunucusunun çalıştırılamamasına neden olabilir. Başarısız denetleyici hizmetlerinin çalışır olup olmadığını kontrol etmek için aşağıdakileri uygulayın:

```bash
ps -ef | grep failover
```

## <a name="logs"></a>Günlükler

Etkin başlıkta Ambari sunucu günlüklerini aşağıdaki konularda kontrol edebilirsiniz:

```
/var/log/ambari-server/ambari-server.log
/var/log/ambari-server/ambari-server-check-database.log
```

Kümedeki herhangi bir düğümde Ambari aracı günlüklerini şu anda kontrol edebilirsiniz:

```bash
/var/log/ambari-agent/ambari-agent.log
```

## <a name="service-start-sequences"></a>Hizmet başlangıç dizileri

Bu, önyükleme zamanında hizmet başlangıç sırasıdır:

1. Hdinsight-agent denetleyici hizmetleri üzerinde başarısız başlar.
1. Failover denetleyici hizmetleri ambari aracıyı etkin başlıktaki her düğümde ve Ambari sunucusunda başlatır.

## <a name="ambari-database"></a>Ambari Veritabanı

HDInsight, Ambari sunucusunun veritabanı olarak hizmet vermek üzere başlık altında SQL Azure Veritabanı oluşturur. Varsayılan [hizmet katmanı S0'dır.](../sql-database/sql-database-elastic-pool-scale.md)

Küme oluşturulurken alt düğüm sayısı 16'dan büyük olan herhangi bir küme için S2 veritabanı hizmet katmanıdır.

## <a name="takeaway-points"></a>Paket puanlar

Bir sorunu çözmek için hizmeti yeniden başlatmaya çalışmıyorsanız ambari-server veya ambari-agent hizmetlerini asla el ile başlatma/durdurma. Bir başarısızı zorlamak için, etkin başlık düğümlerini yeniden başlatabilirsiniz.

Hiçbir küme düğümündeki yapılandırma dosyalarını el ile değiştirmeyin, ambari ui'nin sizin için işini yapmasına izin verin.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight kümelerini Apache Ambari Web arabiriminden yönetme](hdinsight-hadoop-manage-ambari.md)
* [Apache Ambari REST API'yi kullanarak HDInsight kümelerini yönetme](hdinsight-hadoop-manage-ambari-rest-api.md)

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
