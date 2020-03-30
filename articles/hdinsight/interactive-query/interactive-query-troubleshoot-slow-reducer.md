---
title: Azure HDInsight'ta azaltıcı yavaş
description: Azure HDInsight'ta olası veri eğrilirinden dolayı azaltıcı yavaş
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/30/2019
ms.openlocfilehash: 8a9c7ed9f6b5b8ec89bfca6dd59034b11f05f9a3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75895157"
---
# <a name="scenario-reducer-is-slow-in-azure-hdinsight"></a>Senaryo: Azure HDInsight'ta azaltıcı yavaş

Bu makalede, Azure HDInsight kümelerinde Etkileşimli Sorgu bileşenleri kullanılırken sorun giderme adımları ve sorunların olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Sorgu planı gibi `insert into table1 partition(a,b) select a,b,c from table2` bir sorgu çalıştırırken bir sürü indirgeyici başlatır, ancak her bölümden gelen veriler tek bir redüktöre gider. Bu, sorgunun en büyük bölümün indirgeyicisi tarafından alınan süre kadar yavaş olmasını sağlar.

## <a name="cause"></a>Nedeni

[Beeline'i](../hadoop/apache-hadoop-use-hive-beeline.md) açın ve `hive.optimize.sort.dynamic.partition`kümenin değerini doğrulayın.

Bu değişkenin değeri, verilerin yapısına göre doğru/yanlış olarak ayarlanmalıdır.

Giriş tablosundaki bölümler daha azsa (örneğin 10'dan az) ve çıktı bölümlerinin sayısı da `true`öyleyse ve değişken ayarlanmışsa, bu verilerin bölüm başına tek bir azaltıcı kullanılarak genel olarak sıralanıp yazılmasına neden olur. Kullanılabilir redüktör sayısı daha büyük olsa bile, veri çarpıklığı nedeniyle birkaç redüktör geride kalabilir ve maksimum paralelliğe ulaşılamaz. `false`Değiştirildiğinde, birden fazla redüktör tek bir bölümü işleyebilir ve birden çok küçük dosya yazılacak ve bu da daha hızlı ekleme yle sonuçlanır. Bu, daha küçük dosyaların varlığı nedeniyle daha fazla sorgu etkileyebilir.

Bölüm sayısı `true` daha büyük olduğunda ve veri eğriltmediğinde anlamlı bir değer. Bu gibi durumlarda, harita aşamasının sonucu, her bölümün tek bir indirici tarafından işleneceğini ve böylece daha iyi sonraki sorgu performansı elde edilecektir.

## <a name="resolution"></a>Çözüm

1. Verileri normalleştirmek için birden çok bölüme bölmeye çalışın.

1. #1 mümkün değilse, config'in değerini beeline oturumunda false olarak ayarlayın ve sorguyu yeniden deneyin. `set hive.optimize.sort.dynamic.partition=false`. Değeri küme düzeyinde false olarak ayarlamak önerilmez. Değeri `true` en uygun ve veri ve sorgu doğasına göre gerekli parametre ayarlayın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
