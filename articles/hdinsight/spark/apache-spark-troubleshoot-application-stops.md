---
title: Azure HDInsight 'ta 24 gün sonra akış uygulaması Apache Spark durduruluyor
description: Bir Apache Spark akış uygulaması 24 gün boyunca yürütüldükten sonra duraklar ve günlük dosyalarında hata yoktur.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: 83bdb4a9913ae817204fb37320f5bdb8174d5baf
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93288033"
---
# <a name="scenario-apache-spark-streaming-application-stops-after-executing-for-24-days-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta 24 gün boyunca yürütmeden sonra akış uygulaması duraklarının Apache Spark

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorunlar için sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Bir Apache Spark akış uygulaması 24 gün boyunca yürütüldükten sonra duraklar ve günlük dosyalarında hata yoktur.

## <a name="cause"></a>Nedeni

`livy.server.session.timeout`Değer, Apache Livy 'ın bir oturumun tamamlanmasını ne kadar bekleyeceğini denetler. Oturumun uzunluğu `session.timeout` değere ulaştığında, Livy oturumu ve uygulama otomatik olarak sonlandırıldı.

## <a name="resolution"></a>Çözüm

Uzun süre çalışan işler için, `livy.server.session.timeout` ambarı Kullanıcı arabirimini kullanma değerini artırın. URL 'YI kullanarak, ambarı kullanıcı arabiriminden Livy yapılandırmasına erişebilirsiniz `https://<yourclustername>.azurehdinsight.net/#/main/services/LIVY/configs` .

`<yourclustername>`, Portalda gösterilen HDInsight kümenizin adıyla değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]