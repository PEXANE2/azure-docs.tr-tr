---
title: Azure HDInsight'ta Apache Ambari Tez View yavaş yüklenir
description: Apache Ambari Tez View, Azure HDInsight'ta yavaş yüklenebilir veya hiç yüklenmeyebilir
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: f4b1ffbc1e5b8147279d1e0320bd5f55aec90ea2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75895103"
---
# <a name="scenario-apache-ambari-tez-view-loads-slowly-in-azure-hdinsight"></a>Senaryo: Azure HDInsight'ta Apache Ambari Tez View yükleri yavaş yavaş

Bu makalede, Azure HDInsight kümelerinde Etkileşimli Sorgu bileşenleri kullanılırken sorun giderme adımları ve sorunların olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache Ambari Tez View yavaş yüklenebilir veya hiç yüklemeyebilir. Ambari Tez Görünümü'ne yüklenirken, Kafa Düğümleri üzerindeki işlemlerin yanıt vermediğini görebilirsiniz.

## <a name="cause"></a>Nedeni

İplik ATS API'lerine erişmek, kümenin üzerinde çok sayıda Kovan işi olduğu Ekim 2017'den önce oluşturulan kümelerde bazen düşük performansa sahip olabilir.

## <a name="resolution"></a>Çözüm

Bu, Ekim 2017'de çözülen bir sorundur. Kümenizi yeniden oluşturmak, kümenin bu sorunla tekrar bir daha çalışmamalarını sağlayacaktır.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
