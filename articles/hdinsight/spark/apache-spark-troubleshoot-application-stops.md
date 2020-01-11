---
title: Azure HDInsight 'ta 24 gün sonra akış uygulaması Apache Spark durduruluyor
description: Bir Apache Spark akış uygulaması 24 gün boyunca yürütüldükten sonra duraklar ve günlük dosyalarında hata yoktur.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: ff410ea1b6c54d2f58babeb20c68fe95033e9728
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75894429"
---
# <a name="scenario-apache-spark-streaming-application-stops-after-executing-for-24-days-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta 24 gün boyunca yürütmeden sonra akış uygulaması duraklarının Apache Spark

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorunlar için sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Bir Apache Spark akış uygulaması 24 gün boyunca yürütüldükten sonra duraklar ve günlük dosyalarında hata yoktur.

## <a name="cause"></a>Nedeni

`livy.server.session.timeout` değeri, Apache Livy 'ın bir oturumun tamamlanmasını ne kadar bekleyeceğini denetler. Oturum uzunluğu `session.timeout` değerine ulaştığında, Livy oturumu ve uygulama otomatik olarak sonlandırıldı.

## <a name="resolution"></a>Çözünürlük

Uzun süre çalışan işler için, ambarı Kullanıcı arabirimini kullanarak `livy.server.session.timeout` değerini artırın. Uygun URL 'YI kullanarak, ambarı kullanıcı arabiriminden Livy yapılandırmasına erişebilirsiniz `https://<yourclustername>.azurehdinsight.net/#/main/services/LIVY/configs`.

`<yourclustername>`, portalda gösterilen HDInsight kümenizin adıyla değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport) ile bağlanma-Azure Community 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
