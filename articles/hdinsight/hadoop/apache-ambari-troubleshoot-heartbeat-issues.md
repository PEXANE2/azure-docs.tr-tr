---
title: Azure HDInsight'ta Apache Ambari sinyal sorunları
description: Azure HDInsight 'ta Apache ambarı sinyal sorunları için çeşitli nedenleri gözden geçirme
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 02/06/2020
ms.openlocfilehash: 8d23b89ab155c47e09f82d22c065db47ab9ac73d
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92540797"
---
# <a name="apache-ambari-heartbeat-issues-in-azure-hdinsight"></a>Azure HDInsight'ta Apache Ambari sinyal sorunları

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="scenario-high-cpu-utilization"></a>Senaryo: yüksek CPU kullanımı

### <a name="issue"></a>Sorun

Ambarı Aracısı yüksek CPU kullanımına sahiptir ve bu, bazı düğümlerde ambarı aracı sinyalinin kaybolduğu bir şekilde, ambarı kullanıcı arabiriminden gelen uyarılarla sonuçlanır. Sinyal kayıp uyarısı genellikle geçicidir.

### <a name="cause"></a>Nedeni

Çeşitli ambarı Aracısı hataları nedeniyle, ender durumlarda, ambarı aracılarınız yüksek (100 ' e kadar) CPU kullanımına sahip olabilir.

### <a name="resolution"></a>Çözüm

1. Ambarı aracısının işlem KIMLIĞINI (PID) tanımla:

    ```bash
    ps -ef | grep ambari_agent
    ```

1. Ardından CPU kullanımını göstermek için şu komutu çalıştırın:

    ```bash
    top -p <ambari-agent-pid>
    ```

1. Sorunu azaltmak için ambarı-aracıyı yeniden başlatın:

    ```bash
    service ambari-agent restart
    ```

1. Yeniden başlatma çalışmazsa, ambarı Aracısı işlemini sonlandırın ve sonra başlatın:

    ```bash
    kill -9 <ambari-agent-pid>
    service ambari-agent start
    ```

---

## <a name="scenario-ambari-agent-not-started"></a>Senaryo: ambarı aracısı başlatılmadı

### <a name="issue"></a>Sorun

Ambarı Aracısı, bazı düğümlerde, ambarı Aracısı sinyalinin kaybolduğu bazı düğümlere yönelik olarak, hangi düğümlerin uyarı ile sonuçlandığına neden olur.

### <a name="cause"></a>Nedeni

Uyarılar, ambarı aracısının çalışmadığı kaynaklardır.

### <a name="resolution"></a>Çözüm

1. Ambarı-aracısının durumunu onaylayın:

    ```bash
    service ambari-agent status
    ```

1. Yük devretme denetleyicisi hizmetlerinin çalışıp çalışmadığını onaylayın:

    ```bash
    ps -ef | grep failover
    ```

    Yük devretme denetleyicisi Hizmetleri çalışmıyorsa, muhtemelen bir sorun nedeniyle HDInsight-Agent ' ın yük devretme denetleyicisi 'ni başlatmasını engelliyor olabilir. HDInsight-Agent günlük dosyasını denetleyin `/var/log/hdinsight-agent/hdinsight-agent.out` .

## <a name="scenario-heartbeat-lost-for-ambari"></a>Senaryo: ambarı için Sinyal kayboldu

### <a name="issue"></a>Sorun

Ambarı sinyal Aracısı kayboldu.

### <a name="cause"></a>Nedeni

OMS günlükleri yüksek CPU kullanımına neden oluyor.

### <a name="resolution"></a>Çözüm

* [Disable-AzHDInsightMonitoring](/powershell/module/az.hdinsight/disable-azhdinsightmonitoring) PowerShell cmdlet 'Ini kullanarak Azure izleyici günlüğünü devre dışı bırakın.
* `mdsd.warn`Günlük dosyasını Sil

---

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport)Azure Community 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](../../azure-portal/supportability/how-to-create-azure-support-request.md)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.