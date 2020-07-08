---
title: Azure HDInsight 'ta hizmetler başlatılırken bağlantı noktası çakışması
description: Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/23/2020
ms.openlocfilehash: 4cb0d464a82d8da0a09f5391eb1d06dfacd84290
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76776228"
---
# <a name="scenario-port-conflict-when-starting-services-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta hizmetler başlatılırken bağlantı noktası çakışması

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Bir hizmet başlatılamıyor.

## <a name="cause"></a>Nedeni

Bir bağlantı noktası çakışması var.

## <a name="resolution"></a>Çözüm

### <a name="method-1"></a>Yöntem 1

Bağlantı noktası sorunundan etkilenen tüm çalışan süreçler almak/sonlandırmak için aşağıdaki komutları kullanın.

```bash
netstat -lntp | grep <port>
ps -ef | grep <service>
kill -9 <service>
```

Ardından hizmeti başlatın.

### <a name="method-2"></a>2. Yöntem

Düğümü yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport)Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabına bağlanın. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
