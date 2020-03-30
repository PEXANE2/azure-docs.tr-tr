---
title: Azure HDInsight depolama da birçok dosya olduğunda Apache Spark yavaş
description: Azure depolama kapsayıcısı Azure HDInsight'ta birçok dosya içerdiğinde Apache Spark işi yavaş çalışır
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/21/2019
ms.openlocfilehash: e389c05a6de85287bc86eff510e137f470837e56
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75894337"
---
# <a name="apache-spark-job-run-slowly-when-the-azure-storage-container-contains-many-files-in-azure-hdinsight"></a>Azure HDInsight’ta Azure depolama kapsayıcısı birçok dosya içerdiğinde Apache Spark işleri yavaş çalışıyor

Bu makalede, Azure HDInsight kümelerinde Apache Spark bileşenleri kullanılırken sorun giderme adımları ve sorunların olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Bir HDInsight kümesini çalıştırırken, Azure depolama kapsayıcısına yazan Apache Spark işi, çok sayıda dosya/alt klasör olduğunda yavaşlar. Örneğin, yeni bir kapsayıcıya yazarken 20 saniye sürer, ancak 200k dosyaları olan bir kapsayıcıya yazarken yaklaşık 2 dakika.

## <a name="cause"></a>Nedeni

Bu bilinen bir Kıvılcım sorunudur. Yavaşlık ve Spark `GetBlobProperties` iş yürütme sırasında operasyonlar geliyor. `ListBlob`

Bölümleri izlemek için, Spark dizin yapısı hakkında bilgi içeren bir `FileStatusCache` korumak zorundadır. Bu önbelleği kullanarak, Spark yolları ayrışdırabilir ve kullanılabilir bölümlerden haberdar olabilir. Bölümleri izlemenin yararı, Spark'ın yalnızca verileri okuduğunuz da gerekli dosyalara dokunmasıdır. Bu bilgileri güncel tutmak için, yeni veriler yazdığınızda, Spark'ın dizinin altındaki tüm dosyaları listeleması ve bu önbelleği güncelleştirmesi vardır.

Spark 2.1'de, her yazıdan sonra önbelleği güncelleştirmemiz gerekmese de, Spark varolan bir bölüm sütununun geçerli yazma isteğinde önerilen sütunla eşleşip eşleşmediğini denetler, bu nedenle her yazmanın başındaki listeleme işlemlerine de yol açar.

Spark 2.2'de, ek moduyla veri yazarken, bu performans sorunu giderilmelidir.

## <a name="resolution"></a>Çözüm

Bölümlenmiş bir veri kümesi oluşturduğunuzda, Spark'ın `FileStatusCache`''yi güncelleştirmek için listelediği dosya sayısını sınırlayan bir bölümleme düzeni kullanmak önemlidir.

N % 100 == 0 (100 yalnızca bir örnektir) olan her Nth mikro toplu iş için, varolan verileri Spark tarafından yüklenebilen başka bir dizine taşıyın.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
