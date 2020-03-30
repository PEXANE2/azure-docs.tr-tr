---
title: Azure HDInsight'ta disk alanını yönetme
description: Azure HDInsight kümeleriyle etkileşimde olurken sorunlar için sorun giderme adımları ve olası çözümler.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 02/17/2020
ms.openlocfilehash: 577bed7ce342be14a50077a3ffd841cd901b5b31
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77473018"
---
# <a name="manage-disk-space-in-azure-hdinsight"></a>Azure HDInsight'ta disk alanını yönetme

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="hive-log-configurations"></a>Kovan günlüğü yapılandırmaları

1. Bir web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net`kümenizin adı nerede' `CLUSTERNAME` ye gidin.

1. **Hive** > **Configs** > **Gelişmiş** > Gelişmiş**kovan-log4j**gidin. Aşağıdaki ayarları gözden geçirin:

    * `hive.root.logger=DEBUG,RFA`. Bu varsayılan değerdir, [günlük](https://logging.apache.org/log4j/2.x/log4j-api/apidocs/org/apache/logging/log4j/Level.html) düzeyini `INFO` daha az günlük girişlerini yazdırmak için değiştirin.

    * `log4jhive.log.maxfilesize=1024MB`. Bu varsayılan değer, istediğiniz gibi değiştirin.

    * `log4jhive.log.maxbackupindex=10`. Bu varsayılan değer, istediğiniz gibi değiştirin. Parametre atlanmışsa, oluşturulan günlük dosyaları sonsuz olacaktır.

## <a name="yarn-log-configurations"></a>İplik günlüğü yapılandırmaları

Aşağıdaki yapılandırmaları gözden geçirin:

* Apaçi Ambari

    1. Bir web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net`kümenizin adı nerede' `CLUSTERNAME` ye gidin.

    1. **Hive** > **Configs** > **Gelişmiş** > Kaynak**Yöneticisi**gidin. **Günlük Toplamayı Etkinleştir'in** denetlediğinden emin olun. Devre dışı bırakılırsa, ad düğümleri günlükleri yerel olarak tutar ve uygulama tamamlama veya sonlandırma üzerine uzak depoda toplamaz.

* Küme boyutunun iş yükü için uygun olduğundan emin olun. İş yükü son zamanlarda değişmiş veya küme yeniden boyutlandırılmış olabilir. Kümeyi daha yüksek bir iş yüküyle eşleşecek şekilde [ölçeklendirin.](../hdinsight-scaling-best-practices.md)

* `/mnt/resource`yetim dosyalarla doldurulabilir (kaynak yöneticisinin yeniden başlatılması durumunda olduğu gibi). Gerekirse, el ile `/mnt/resource/hadoop/yarn/log` `/mnt/resource/hadoop/yarn/local`temiz ve .

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın. Azure topluluğunu doğru kaynaklara bağlama: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)yı gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
