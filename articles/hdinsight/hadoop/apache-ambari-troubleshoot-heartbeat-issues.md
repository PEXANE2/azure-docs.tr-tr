---
title: Azure HDInsight'ta Apache Ambari sinyal sorunları
description: Azure HDInsight'ta Apache Ambari kalp atışı sorunlarının çeşitli nedenlerini gözden geçirme
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 02/06/2020
ms.openlocfilehash: ab88f65d535be2aef5f0b26fa1171c03276466e8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77057082"
---
# <a name="apache-ambari-heartbeat-issues-in-azure-hdinsight"></a>Azure HDInsight'ta Apache Ambari sinyal sorunları

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="scenario-high-cpu-utilization"></a>Senaryo: Yüksek CPU kullanımı

### <a name="issue"></a>Sorun

Ambari ajan bazı düğümler için Ambari ajan kalp atışı kaybolur Ambari UI uyarıları sonuçları yüksek CPU kullanımı vardır. Kalp atışı kaybı uyarısı genellikle geçicidir.

### <a name="cause"></a>Nedeni

Çeşitli ambari-agent hataları nedeniyle, nadir durumlarda, ambari-ajan yüksek (100'e yakın) yüzde CPU kullanımı olabilir.

### <a name="resolution"></a>Çözüm

1. Ambari-agent'ın proses kimliğini (pid) tanımlayın:

    ```bash
    ps -ef | grep ambari_agent
    ```

1. Ardından CPU kullanımını göstermek için şu komutu çalıştırın:

    ```bash
    top -p <ambari-agent-pid>
    ```

1. Sorunu azaltmak için ambari-agent'ı yeniden başlatın:

    ```bash
    service ambari-agent restart
    ```

1. Yeniden başlatma işe yaramazsa, ambari-agent işlemini öldürün ve sonra başlatın:

    ```bash
    kill -9 <ambari-agent-pid>
    service ambari-agent start
    ```

---

## <a name="scenario-ambari-agent-not-started"></a>Senaryo: Ambari ajan başlamadı

### <a name="issue"></a>Sorun

Ambari ajan bazı düğümler için Ambari ajan kalp atışı kaybolur Ambari UI uyarıları sonuçları başlamadı.

### <a name="cause"></a>Nedeni

Uyarılar, Ambari ajanının çalışmaması nedeniyle kaynaklanıyor.

### <a name="resolution"></a>Çözüm

1. Ambari-ajan Durumunu onaylayın:

    ```bash
    service ambari-agent status
    ```

1. Başarısız denetleyici hizmetlerinin çalışıyorsa onayla:

    ```bash
    ps -ef | grep failover
    ```

    Başarısız denetleyici hizmetleri çalışmıyorsa, büyük olasılıkla hdinsight aracısı'nın başarısız denetleyiciyi başlatmasını engelleyen bir sorundan kaynaklanmaktadır. Dosyadan `/var/log/hdinsight-agent/hdinsight-agent.out` hdinsight-agent günlük kontrol edin.

## <a name="scenario-heartbeat-lost-for-ambari"></a>Senaryo: Kalp Atışı Ambari için kayıp

### <a name="issue"></a>Sorun

Ambari kalp atışı ajanı kayboldu.

### <a name="cause"></a>Nedeni

OMS günlükleri yüksek CPU kullanımına neden oluyor.

### <a name="resolution"></a>Çözüm

* [Devre Dışı-AzHDInsightMonitoring](https://docs.microsoft.com/powershell/module/az.hdinsight/disable-azhdinsightmonitoring) PowerShell cmdlet'i kullanarak Azure Monitor günlüğe kaydetmeyi devre dışı bırakın.
* Günlük `mdsd.warn` dosyasını silme

---

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
