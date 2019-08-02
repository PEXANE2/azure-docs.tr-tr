---
title: Apache Spark akış uygulaması, Azure HDInsight 'taki günlüklerde bilinen hatalar olmadan 24 gün boyunca yürüttükten sonra verileri işlemeyi durduruyor
description: Bir Apache Spark akış uygulaması 24 gün boyunca yürütüldükten sonra duraklar ve günlük dosyalarında hata yoktur.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/29/2019
ms.openlocfilehash: 002c45c514b5d8207a1aa70ec8e1c749677a239a
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68620889"
---
# <a name="scenario-apache-spark-streaming-application-stops-after-executing-for-24-days-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta 24 gün boyunca yürütmeden sonra akış uygulaması Apache Spark durduruluyor

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorunlar için sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Bir Apache Spark akış uygulaması 24 gün boyunca yürütüldükten sonra duraklar ve günlük dosyalarında hata yoktur.

## <a name="cause"></a>Nedeni

Değer `livy.server.session.timeout` , Apache Livy 'ın bir oturumun tamamlanmasını ne kadar bekleyeceğini denetler. Oturumun uzunluğu `session.timeout` değere ulaştığında, Livy oturumu ve uygulama otomatik olarak sonlandırıldı.

## <a name="resolution"></a>Çözüm

Uzun süre çalışan işler için, ambarı Kullanıcı arabirimini `livy.server.session.timeout` kullanma değerini artırın. URL 'YI `https://<yourclustername>.azurehdinsight.net/#/main/services/LIVY/configs`kullanarak, ambarı kullanıcı arabiriminden Livy yapılandırmasına erişebilirsiniz.

, `<yourclustername>` Portalda gösterilen HDInsight kümenizin adıyla değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* Azure Community [@AzureSupport](https://twitter.com/azuresupport) 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
