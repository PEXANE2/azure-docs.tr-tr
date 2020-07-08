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
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
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

Apache Yarn günlüğü, `IndexFile` dosya ayrıştırıcısı tarafından desteklenmeyen biçimde toplanmış.

## <a name="resolution"></a>Çözüm

1. Bir Web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net` , `CLUSTERNAME` Kümenizin adı olan ' a gidin.

1. Ambarı kullanıcı arabiriminden, **Yarn**  >  **configs**  >  **Gelişmiş**  >  **Gelişmiş Yarn-site**' ye gidin.

1. ILB depolaması için: için varsayılan değer `yarn.log-aggregation.file-formats` `IndexedFormat,TFile` . Değerini olarak değiştirin `TFile` .

1. ADLS depolaması için: varsayılan değeri `yarn.nodemanager.log-aggregation.compression-type` şeklindedir `gz` . Değerini olarak değiştirin `none` .

1. Değişikliği kaydedin ve etkilenen tüm hizmetleri yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport)Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabına bağlanın. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
