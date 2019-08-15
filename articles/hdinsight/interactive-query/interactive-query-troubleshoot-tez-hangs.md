---
title: Azure HDInsight 'ta Apache Tez uygulama askıda kalıyor
description: Azure HDInsight 'ta Apache Tez uygulama askıda kalıyor
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/09/2019
ms.openlocfilehash: fa56667c039133700c100b3e01a56ad784d16a6c
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68977436"
---
# <a name="scenario-apache-tez-application-hangs-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta Apache Tez uygulama askıda kalıyor

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache Hive işi gönderdikten sonra, tez görünümünden iş durumu "çalışıyor" dır, ancak herhangi bir ilerleme yapmadan görünmez

## <a name="cause"></a>Nedeni

Çok fazla iş gönderildi; uzun Yarn kuyruğu.

## <a name="resolution"></a>Çözüm

Kümeyi ölçeklendirin veya Yarn sırasının boşaltılana kadar beklemeniz yeterlidir.

Varsayılan `yarn.scheduler.capacity.maximum-applications` olarak, çalıştıran veya bekleyen en fazla uygulama sayısını denetler ve varsayılan olarak öğesine `10000`ayarlanır.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabına bağlanın. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
