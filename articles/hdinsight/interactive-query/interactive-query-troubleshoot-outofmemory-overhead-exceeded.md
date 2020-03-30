---
title: Apache Hive'a katılır siler Bellek dışı hataya yol açar - Azure HDInsight
description: OutOfMemory hataları ile başa çıkmak "GC genel para sınırı hata aştı"
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: ab334dfb15044fd0734a107c12003ca2c1f86906
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75895182"
---
# <a name="scenario-joins-in-apache-hive-leads-to-an-outofmemory-error-in-azure-hdinsight"></a>Senaryo: Apache Hive'a katılmak Azure HDInsight'ta OutOfMemory hatasına yol açar

Bu makalede, Azure HDInsight kümelerinde Etkileşimli Sorgu bileşenleri kullanılırken sorun giderme adımları ve sorunların olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Apache Hive joins için varsayılan davranış, bir birleştirme bir Harita/ Azaltma adımı gerçekleştirmek zorunda kalmadan gerçekleştirilebilecek şekilde bir tablonun tüm içeriğini belleğe yüklemektir. Hive tablosu belleğe sığmayacak kadar büyükse, sorgu başarısız olabilir.

## <a name="cause"></a>Nedeni

Yeterli boyutta kovan birleştirmeleri çalıştırırken, aşağıdaki hata yla karşılaşılır:

```
Caused by: java.lang.OutOfMemoryError: GC overhead limit exceeded error.
```

## <a name="resolution"></a>Çözüm

Aşağıdaki Hive yapılandırma değerini ayarlayarak Hive'ın birleştirmelerde tabloları belleğe yüklemesini (bunun yerine Harita/Küçültme adımı gerçekleştirmeyi) engelleyin:

```
hive.auto.convert.join=false
```

## <a name="next-steps"></a>Sonraki adımlar

Bu değeri ayarlamak sorununuzu çözmediyse, aşağıdakilerden birini ziyaret edin...

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
