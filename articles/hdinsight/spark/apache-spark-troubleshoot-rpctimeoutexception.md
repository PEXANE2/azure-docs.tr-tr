---
title: Apache Spark Thrift-Azure HDInsight için RpcTimeoutException
description: Apache Spark Thrift Server kullanarak büyük veri kümelerini işlerken 502 hata görürsünüz
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: a29d36c5ba6fdd51de27afa3ab4dfe1258332200
ms.sourcegitcommit: 6fd28c1e5cf6872fb28691c7dd307a5e4bc71228
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85208425"
---
# <a name="scenario-rpctimeoutexception-for-apache-spark-thrift-server-in-azure-hdinsight"></a>Senaryo: Azure HDInsight 'ta Apache Spark Thrift sunucusu için RpcTimeoutException

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorunlar için sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Spark uygulaması `org.apache.spark.rpc.RpcTimeoutException` `Futures timed out` Aşağıdaki örnekte olduğu gibi bir özel durumla ve iletiyle başarısız olur:

```
org.apache.spark.rpc.RpcTimeoutException: Futures timed out after [120 seconds]. This timeout is controlled by spark.rpc.askTimeout
 at org.apache.spark.rpc.RpcTimeout.org$apache$spark$rpc$RpcTimeout$$createRpcTimeoutException(RpcTimeout.scala:48)
```

`OutOfMemoryError``overhead limit exceeded`Ayrıca, `sparkthriftdriver.log` Aşağıdaki örnekte olduğu gibi hatalar da görüntülenebilir:

```
WARN  [rpc-server-3-4] server.TransportChannelHandler: Exception in connection from /10.0.0.17:53218
java.lang.OutOfMemoryError: GC overhead limit exceeded
```

## <a name="cause"></a>Nedeni

Bu hatalar, veri işleme sırasında bellek kaynaklarının bulunmaması nedeniyle oluşur. Java atık toplama işlemi başlatılırsa, Spark uygulamasının yanıt vermeyi durdurmasına neden olabilir. Sorgular zaman aşımına uğrar ve işlemeyi durduracaktır. `Futures timed out`Hata, ciddi bir stres altında bir kümeyi gösterir.

## <a name="resolution"></a>Çözüm

Daha fazla çalışan düğümü ekleyerek veya mevcut küme düğümlerinin bellek kapasitesini artırarak küme boyutunu artırın. Aynı zamanda işlenen veri miktarını azaltmak için veri ardışık düzenini de ayarlayabilirsiniz.

`spark.network.timeout`Tüm ağ bağlantıları için zaman aşımını denetler. Ağ zaman aşımını artırmak bazı kritik işlemlerin tamamlanabilmesi için daha fazla zaman alabilir, ancak bu durum sorunu tamamen çözmeyecektir.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport)Azure Community 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
