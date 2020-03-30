---
title: Azure HDInsight'ta Apache İplik günlüğü okunamıyor
description: Azure HDInsight kümeleriyle etkileşimde olurken sorunlar için sorun giderme adımları ve olası çözümler.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/23/2020
ms.openlocfilehash: f7861ae4980f13fbd87780ab2d26fa3376f33532
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76776202"
---
# <a name="scenario-unable-to-read-apache-yarn-log-in-azure-hdinsight"></a>Senaryo: Azure HDInsight'ta Apache İplik günlüğü okunamıyor

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Depolama hesabındabulunan Apache İplik günlükleri insan tarafından okunamıyor. Dosya ayrıştırıcısı çalışmıyor ve aşağıdaki hata iletisini üretir:

```
java.io.IOException: Not a valid BCFile.
```

## <a name="cause"></a>Nedeni

Apache İplik `IndexFile` günlüğü, dosya ayırıcıtarafından desteklenmeyen biçime toplanır.

## <a name="resolution"></a>Çözüm

1. Bir web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net`kümenizin adı nerede' `CLUSTERNAME` ye gidin.

1. Ambari UI itibaren, **İPN** > **Configs** > **Gelişmiş** > **gelişmiş iplik-site**gidin.

1. WASB depolama için: Varsayılan `yarn.log-aggregation.file-formats` `IndexedFormat,TFile`değer. Değeri `TFile`' ye değdirme

1. ADLS depolama için: Varsayılan `yarn.nodemanager.log-aggregation.compression-type` `gz`değer. Değeri `none`' ye değdirme

1. Değişikliği kaydedin ve etkilenen tüm hizmetleri yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
