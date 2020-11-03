---
title: Azure HDInsight 'ta Apache ambarı Dizin uyarıları
description: HDInsight 'ta Apache ambarı Dizin uyarıları için olası nedenleri ve çözümleri tartışma ve çözümleme.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/22/2020
ms.openlocfilehash: d8b1f705eb08d1c58b32e0cbd7c57722a6de93cc
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93285536"
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

## <a name="resolution"></a>Çözüm

Etkilenen çalışan düğümleri üzerinde eksik dizinleri el ile oluşturun.

1. İlgili çalışan düğümüne SSH.

1. Kök kullanıcıyı al: `sudo su` .

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

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]