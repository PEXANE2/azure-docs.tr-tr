---
title: Azure HDInsight 'ta Apache ambarı kullanımı
description: Apache ambarı 'nın Azure HDInsight 'ta nasıl kullanıldığına ilişkin tartışma.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/05/2020
ms.openlocfilehash: 466c170985715be52a90d579c19ca23aefefe2e5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77067402"
---
# <a name="apache-ambari-usage-in-azure-hdinsight"></a>Azure HDInsight 'ta Apache ambarı kullanımı

HDInsight, Küme dağıtımı ve yönetimi için Apache ambarı 'nı kullanır. Ambarı aracıları her düğüm üzerinde çalışır (headnode, çalışan düğümü, Zookeeper ve edgenode varsa). Ambarı sunucusu yalnızca baş düğümüne üzerinde çalışır (hn0 veya hn1). Tek seferde yalnızca bir adet bir örnek ambarı sunucusu çalışır. Bu, HDInsight yük devretme denetleyicisi tarafından denetlenir. Yayın düğümlerinden biri yeniden başlatma veya bakım için kapalıysa, diğer bir baş düğüm etkin hale gelir ve ikinci baş düğümüne üzerinde ambarı sunucusu başlatılır.

Tüm küme yapılandırması, [ambarı kullanıcı arabiriminden](./hdinsight-hadoop-manage-ambari.md)yapılmalıdır, düğüm yeniden başlatıldığında tüm yerel değişikliklerin üzerine yazılır.

## <a name="failover-controller-services"></a>Yük devretme denetleyicisi Hizmetleri

HDInsight yük devretme denetleyicisi, geçerli etkin baş düğümüne işaret eden, baş düğümüne konağının IP adresini güncelleştirmeden de sorumludur. Tüm ambarı aracıları durum ve sinyali, baş düğümüne konağına bildirmek üzere yapılandırılır. Yük devretme denetleyicisi kümedeki her düğüm üzerinde çalışan bir hizmetler kümesidir; bunlar çalışmıyorsa, yayın düğümü yük devretmesi doğru çalışmayabilir ve Ambarı sunucusuna erişmeye çalışırken HTTP 502 ile karşılaşırsınız.

Hangi baş düğümüne 'un etkin olduğunu denetlemek için, bir yol kümedeki düğümlerden birine SSH olarak çalışır `ping headnodehost` ve IP 'yi iki baş düğümüne ile aynı şekilde karşılaştırın.

Yük devretme denetleyicisi Hizmetleri çalışmıyorsa, yayın düğümü yük devretmesi doğru şekilde gerçekleşmeyebilir ve bu, ambarı sunucusu çalıştırmayan anlamına gelebilir. Yük devretme denetleyicisi hizmetlerinin çalışıp çalışmadığını denetlemek için şunu yürütün:

```bash
ps -ef | grep failover
```

## <a name="logs"></a>Günlükler

Etkin headnode 'da, şu konumda yer alan ambarı sunucusu günlüklerini kontrol edebilirsiniz:

```
/var/log/ambari-server/ambari-server.log
/var/log/ambari-server/ambari-server-check-database.log
```

Kümedeki herhangi bir düğümde, şu adreste bulunan ambarı Aracısı günlüklerini denetleyebilirsiniz:

```bash
/var/log/ambari-agent/ambari-agent.log
```

## <a name="service-start-sequences"></a>Hizmet başlatma dizileri

Bu, önyükleme zamanında hizmetin başlangıç dizisidir:

1. HDInsight-Agent yük devretme denetleyicisi hizmetlerini başlatır.
1. Yük devretme denetleyicisi Hizmetleri, etkin yayın düğümündeki her düğümde ve ambarı sunucusunda bulunan ambarı Aracısı 'nı başlatır.

## <a name="ambari-database"></a>Ambarı veritabanı

HDInsight, ambarı sunucusu için veritabanı olarak kullanılacak şekilde SQL Azure veritabanı oluşturur. Varsayılan [hizmet katmanı S0 ' dir](../sql-database/sql-database-elastic-pool-scale.md).

Küme oluştururken, çalışan düğüm sayısı 16 ' dan büyük herhangi bir küme için, S2 veritabanı hizmet katmandır.

## <a name="takeaway-points"></a>Kalkış noktaları

Bir sorunu geçici olarak çözmek için hizmeti yeniden başlatmaya çalışmadığınız müddetçe, ambarı-sunucu veya ambarı-Aracısı hizmetlerini hiçbir şekilde el ile başlatmayın/durdurmayın. Yük devretmeyi zorlamak için etkin yayın düğümünü yeniden başlatabilirsiniz.

Herhangi bir küme düğümünde herhangi bir yapılandırma dosyasını hiçbir şekilde el ile değiştirmeyin, bu iş için ambarı Kullanıcı arabirimine izin verin.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight kümelerini Apache Ambari Web arabiriminden yönetme](hdinsight-hadoop-manage-ambari.md)
* [Apache ambarı 'nı kullanarak HDInsight kümelerini yönetme REST API](hdinsight-hadoop-manage-ambari-rest-api.md)

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabına bağlanın. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
