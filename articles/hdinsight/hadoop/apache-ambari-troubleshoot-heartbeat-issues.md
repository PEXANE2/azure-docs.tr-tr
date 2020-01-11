---
title: Azure HDInsight 'ta Apache ambarı sinyal sorunları
description: Azure HDInsight 'ta Apache ambarı sinyal sorunları için çeşitli nedenleri gözden geçirme
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 09/11/2019
ms.openlocfilehash: ae5cfcfcd394aab644b35ac66aafa213dc49dd42
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75895378"
---
# <a name="apache-ambari-heartbeat-issues-in-azure-hdinsight"></a>Azure HDInsight 'ta Apache ambarı sinyal sorunları

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="scenario-high-cpu-utilization"></a>Senaryo: yüksek CPU kullanımı

### <a name="issue"></a>Sorun

Ambarı Aracısı yüksek CPU kullanımına sahiptir ve bu, bazı düğümlerde ambarı aracı sinyalinin kaybolduğu bir şekilde, ambarı kullanıcı arabiriminden gelen uyarılarla sonuçlanır. Sinyal kayıp uyarısı genellikle geçicidir. 

### <a name="cause"></a>Nedeni

Çeşitli ambarı Aracısı hataları nedeniyle, ender durumlarda, ambarı aracılarınız yüksek (100 ' e kadar) CPU kullanımına sahip olabilir.

### <a name="resolution"></a>Çözünürlük

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

Ambarı Aracısı, bazı düğümlerde, ambarı Aracısı sinyalinin kaybolduğu bazı düğümlere yönelik olarak, belirli bir düğümde bulunan uyarılarla sonuçlanır.

### <a name="cause"></a>Nedeni

Uyarılar, ambarı aracısının çalışmadığı kaynaklardır.

### <a name="resolution"></a>Çözünürlük

1. Ambarı-aracısının durumunu onaylayın:

    ```bash
    service ambari-agent status
    ```

1. Yük devretme denetleyicisi hizmetlerinin çalışıp çalışmadığını onaylayın:

    ```bash
    ps -ef | grep failover
    ```

    Yük devretme denetleyicisi Hizmetleri çalışmıyorsa, büyük olasılıkla bir sorundan dolayı HDInsight-Agent ' ın yük devretme denetleyicisi 'ni başlatmasını engelliyor olabilir. `/var/log/hdinsight-agent/hdinsight-agent.out` dosyasından HDInsight-Agent günlüğünü denetleyin.

---

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport) ile bağlanma-Azure Community 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
