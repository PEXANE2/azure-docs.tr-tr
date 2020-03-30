---
title: Azure HDInsight'ta hizmet başlatırken bağlantı noktası çakışması
description: Azure HDInsight kümeleriyle etkileşimde olurken sorunlar için sorun giderme adımları ve olası çözümler.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 01/23/2020
ms.openlocfilehash: 4cb0d464a82d8da0a09f5391eb1d06dfacd84290
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76776228"
---
# <a name="scenario-port-conflict-when-starting-services-in-azure-hdinsight"></a>Senaryo: Azure HDInsight'ta hizmet başlatırken bağlantı noktası çakışması

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Bir hizmet başlatılmaz.

## <a name="cause"></a>Nedeni

Bir bağlantı noktası çakışması var.

## <a name="resolution"></a>Çözüm

### <a name="method-1"></a>Yöntem 1

Bağlantı noktası sorunundan etkilenen tüm çalışan işlemleri almak/öldürmek için aşağıdaki komutları kullanın.

```bash
netstat -lntp | grep <port>
ps -ef | grep <service>
kill -9 <service>
```

O zaman hizmete başla.

### <a name="method-2"></a>2. Yöntem

Düğümü yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
