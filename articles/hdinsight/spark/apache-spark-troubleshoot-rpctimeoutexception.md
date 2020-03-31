---
title: Apache Spark tutumluluk için RpcTimeoutException - Azure HDInsight
description: Apache Spark tutumlu sunucu kullanarak büyük veri kümelerini işlerken 502 hata görürsünüz
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: b15ac80295a0113eb0c384e1cc3185f3304c39c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75894274"
---
# <a name="scenario-rpctimeoutexception-for-apache-spark-thrift-server-in-azure-hdinsight"></a>Senaryo: Azure HDInsight'ta Apache Spark tutumlu sunucusu için RpcTimeoutException

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorun giderme adımları ve sorunların olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Spark uygulaması bir `org.apache.spark.rpc.RpcTimeoutException` özel durum `Futures timed out`ve bir ileti ile başarısız olur: , aşağıdaki örnekte olduğu gibi:

```
org.apache.spark.rpc.RpcTimeoutException: Futures timed out after [120 seconds]. This timeout is controlled by spark.rpc.askTimeout
 at org.apache.spark.rpc.RpcTimeout.org$apache$spark$rpc$RpcTimeout$$createRpcTimeoutException(RpcTimeout.scala:48)
```

`OutOfMemoryError`ve `overhead limit exceeded` hatalar aşağıdaki örnekte olduğu `sparkthriftdriver.log` gibi de görünebilir:

```
WARN  [rpc-server-3-4] server.TransportChannelHandler: Exception in connection from /10.0.0.17:53218
java.lang.OutOfMemoryError: GC overhead limit exceeded
```

## <a name="cause"></a>Nedeni

Bu hatalar, veri işleme sırasında bellek kaynaklarının eksikliğinden kaynaklanır. Java çöp toplama işlemi başlarsa, Spark uygulamasıasılı yol açabilir. Sorgular zaman dışarı başlar ve işleme durdurmak. Hata, `Futures timed out` şiddetli stres altındaki bir kümeyi gösterir.

## <a name="resolution"></a>Çözüm

Daha fazla altdüğüm ekleyerek veya varolan küme düğümlerinin bellek kapasitesini artırarak küme boyutunu artırın. Ayrıca, aynı anda işlenen veri miktarını azaltmak için veri ardışık işlemini de ayarlayabilirsiniz.

Tüm `spark.network.timeout` ağ bağlantıları için zaman ağını denetler. Ağ zaman ağını artırmak bazı kritik işlemlerin tamamlanması için daha fazla zaman sağlayabilir, ancak bu sorunu tamamen çözmez.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
