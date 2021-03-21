---
title: Azure HDInsight 'ta Apache Tez uygulama askıda kalıyor
description: Azure HDInsight 'ta Apache Tez uygulama askıda kalıyor
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 08/09/2019
ms.openlocfilehash: 56c68c26ae953034283031e2427b7a4afadee94e
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98935944"
---
# <a name="scenario-apache-tez-application-hangs-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta uygulama askıda Apache Tez

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache Hive işi gönderdikten sonra, tez görünümünden iş durumu "çalışıyor" dır, ancak herhangi bir ilerleme yapmadan görünmez

## <a name="cause"></a>Nedeni

Çok fazla iş gönderildi; uzun Yarn kuyruğu.

## <a name="resolution"></a>Çözüm

Kümeyi ölçeklendirin veya Yarn sırasının boşaltılana kadar beklemeniz yeterlidir.

Varsayılan olarak `yarn.scheduler.capacity.maximum-applications` , çalıştıran veya bekleyen en fazla uygulama sayısını denetler ve varsayılan olarak öğesine ayarlanır `10000` .

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport)Müşteri deneyimini iyileştirmek için resmi Microsoft Azure hesabına bağlanın. Azure Community 'yi doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](../../azure-portal/supportability/how-to-create-azure-support-request.md)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.