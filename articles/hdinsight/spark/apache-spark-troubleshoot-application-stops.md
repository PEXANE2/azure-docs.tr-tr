---
title: Azure HDInsight'ta 24 gün sonra Apache Spark Streaming uygulaması sona eriyor
description: Apache Spark Streaming uygulaması 24 gün boyunca çalıştırdıktan sonra durur ve günlük dosyalarında hata yoktur.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: ff410ea1b6c54d2f58babeb20c68fe95033e9728
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75894429"
---
# <a name="scenario-apache-spark-streaming-application-stops-after-executing-for-24-days-in-azure-hdinsight"></a>Senaryo: Azure HDInsight'ta 24 gün boyunca çalıştırdıktan sonra Apache Spark Streaming uygulaması durur

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorun giderme adımları ve sorunların olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache Spark Streaming uygulaması 24 gün boyunca çalıştırdıktan sonra durur ve günlük dosyalarında hata yoktur.

## <a name="cause"></a>Nedeni

Değer, `livy.server.session.timeout` Apache Livy'nin oturumun tamamlanmasını ne kadar beklemesi gerektiğini denetler. Oturum uzunluğu değere `session.timeout` ulaştığında, Livy oturumu ve uygulama otomatik olarak öldürülür.

## <a name="resolution"></a>Çözüm

Uzun süren işler için Ambari UI'yi kullanmanın `livy.server.session.timeout` değerini artırın. Livy yapılandırmasından Ambari UI'den URL'yi `https://<yourclustername>.azurehdinsight.net/#/main/services/LIVY/configs`kullanarak erişebilirsiniz.

Portalda gösterildiği gibi HDInsight kümenizin adıyla değiştirin. `<yourclustername>`

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
