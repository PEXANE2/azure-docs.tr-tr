---
title: Apache ambarı tez görünümü, Azure HDInsight 'ta yavaş yükleniyor
description: Apache ambarı tez görünümü yavaş yükleme veya Azure HDInsight 'ta hiç yüklenmeyebilir
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: 0e294566da4c6f514704abc2ac014b8345020b5a
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93288843"
---
# <a name="scenario-apache-ambari-tez-view-loads-slowly-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta Apache ambarı tez görünümü yavaş yükleniyor

Bu makalede, Azure HDInsight kümelerinde etkileşimli sorgu bileşenlerini kullanırken sorunlar için sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache ambarı tez görünümü yavaş çalışabilir veya hiç yüklenmeyebilir. Ambarı tez görünümü yüklenirken, yük düğümlerinde işlemlerin yanıt vermemesine neden olabilir.

## <a name="cause"></a>Nedeni

Yarn ATS API 'Lerine erişmek, küme üzerinde çok sayıda Hive işi çalıştırılan durumlarda 2017 eki olmadan önce oluşturulan kümeler üzerinde düşük performansa sahip olabilir.

## <a name="resolution"></a>Çözüm

Bu, Ekim 2017 ' de düzeltilen bir sorundur. Kümenizi yeniden oluşturmak bu sorunu tekrar çalıştırmayacak.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]