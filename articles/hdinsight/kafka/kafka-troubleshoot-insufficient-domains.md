---
title: Azure HDInsight'ta bölge hatasında yeterli hata etki alanı yok
description: Azure HDInsight'ta bölgede yeterli hata etki alanı olmadığı için küme oluşturma başarısız oldu
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/09/2019
ms.openlocfilehash: 8be7e05ac85ce0b1b10edf18d3885a07e016b9ae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75895024"
---
# <a name="scenario-cluster-creation-failed-due-to-not-sufficient-fault-domains-in-region-in-azure-hdinsight"></a>Senaryo: Azure `not sufficient fault domains in region` HDInsight'ta küme oluşturma nedeniyle başarısız oldu

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache Kafka `not sufficient fault domains in region` kümesi oluşturmaya çalışırken benzer hata iletisi alın.

## <a name="cause"></a>Nedeni

Hata etki alanı, bir Azure veri merkezinde temel donanımlardan oluşan mantıksal bir gruplandırmadır. Her hata etki alanı ortak bir güç kaynağı ve ağ anahtarına sahiptir. Bir HDInsight kümesi içindeki düğümleri uygulayan sanal makineler ve yönetilen diskler, bu hata etki alanlarına dağıtılır. Bu mimari, fiziksel donanım hatalarının olası etkisini sınırlar.

Her Azure bölgesinde belirli sayıda hata etki alanı bulunur. Etki alanlarının listesi ve içerdikleri hata etki alanlarının sayısı için [Kullanılabilirlik Kümeleri'ndeki](../../virtual-machines/windows/manage-availability.md)belgelere bakın.

HDInsight'ta Kafka kümelerinin en az üç Hata etki alanına sahip bir bölgede sağlanması gerekir.

## <a name="resolution"></a>Çözüm

Küme oluşturmak istediğiniz bölge yeterli hata etki alanına sahip değilse, üç hata etki alanı olmasa bile kümenin sağlanmasına izin vermek için ürün ekibine ulaşın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
