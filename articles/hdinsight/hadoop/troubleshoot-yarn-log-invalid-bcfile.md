---
title: Azure HDInsight 'ta Apache Yarn günlüğü okunamıyor
description: Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/23/2020
ms.openlocfilehash: f7861ae4980f13fbd87780ab2d26fa3376f33532
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76776202"
---
# <a name="scenario-unable-to-read-apache-yarn-log-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta Apache Yarn günlüğü okunamıyor

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Depolama hesabından bulunan Apache Yarn günlükleri insan tarafından okunabilir değil. Dosya ayrıştırıcısı çalışmıyor ve aşağıdaki hata iletisini veriyor:

```
java.io.IOException: Not a valid BCFile.
```

## <a name="cause"></a>Nedeni

Apache Yarn günlüğü, dosya ayrıştırıcısı tarafından desteklenmeyen `IndexFile` biçiminde toplanır.

## <a name="resolution"></a>Çözünürlük

1. Bir Web tarayıcısından `https://CLUSTERNAME.azurehdinsight.net`' a gidin; burada `CLUSTERNAME` kümenizin adıdır.

1. Ambarı kullanıcı arabiriminden, **yarn** > **configs** ** > gelişmiş** > **Gelişmiş Yarn-sitesi**' ne gidin.

1. Bellek depolama için: `yarn.log-aggregation.file-formats` için varsayılan değer `IndexedFormat,TFile`. Değeri `TFile`olarak değiştirin.

1. ADLS depolaması için: `yarn.nodemanager.log-aggregation.compression-type` için varsayılan değer `gz`. Değeri `none`olarak değiştirin.

1. Değişikliği kaydedin ve etkilenen tüm hizmetleri yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport) ile bağlanma-müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabı. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
