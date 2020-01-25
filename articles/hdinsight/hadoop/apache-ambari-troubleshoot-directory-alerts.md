---
title: Azure HDInsight 'ta Apache ambarı Dizin uyarıları
description: HDInsight 'ta Apache ambarı Dizin uyarıları için olası nedenleri ve çözümleri tartışma ve çözümleme.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/22/2020
ms.openlocfilehash: 7fd287377a82caeaecea264f0165d12ced57f5cd
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76722843"
---
# <a name="scenario-apache-ambari-directory-alerts-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta Apache ambarı Dizin uyarıları

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache ambarı 'ndan şuna benzer hatalar alıyorsunuz:

```
1/1 local-dirs have errors: [ /mnt/resource/hadoop/yarn/local : Cannot create directory: /mnt/resource/hadoop/yarn/local ]
1/1 log-dirs have errors: [ /mnt/resource/hadoop/yarn/log : Cannot create directory: /mnt/resource/hadoop/yarn/log ]
```

## <a name="cause"></a>Nedeni

Etkilenen çalışan düğümlerde belirtilen ambarı uyarısından belirtilen dizinler yok.

## <a name="resolution"></a>Çözünürlük

Etkilenen çalışan düğümleri üzerinde eksik dizinleri el ile oluşturun.

1. İlgili çalışan düğümüne SSH.

1. Kök kullanıcıyı al: `sudo su`.

1. Gereken dizinleri yinelemeli olarak oluşturun.

1. Bu dizinlerin sahibini ve grubunu değiştirin.

    ```bash
    chown -R yarn /mnt/resource/hadoop/yarn/local
    chgrp -R hadoop /mnt/resource/hadoop/yarn/local
    chown -R yarn /mnt/resource/hadoop/yarn/log
    chgrp -R hadoop /mnt/resource/hadoop/yarn/log
    ```

1. Apache ambarı kullanıcı arabiriminden, uyarıyı devre dışı bırakın ve sonra yeniden etkinleştirin.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport) ile bağlanma-müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabı. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
