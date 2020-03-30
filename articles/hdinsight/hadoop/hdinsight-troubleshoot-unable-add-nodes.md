---
title: Azure HDInsight kümesine düğüm eklenemiyor
description: Azure HDInsight'ta Apache Hadoop kümesine düğüm eklenememesinin sorun giderme sorunu
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.openlocfilehash: 97d7f34fff324a9959292460e534c15110c3e532
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75894092"
---
# <a name="scenario-unable-to-add-nodes-to-azure-hdinsight-cluster"></a>Senaryo: Azure HDInsight kümesine düğüm eklenemiyor

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Azure HDInsight kümesine düğüm eklenemiyor.

## <a name="cause"></a>Nedeni

Nedenler değişebilir.

## <a name="resolution"></a>Çözüm

Küme [boyutu](../hdinsight-scaling-best-practices.md) özelliğini kullanarak, küme için gereken ek çekirdek sayısını hesaplayın. Bu, yeni çalışan düğümlerindeki toplam çekirdek sayısına bağlıdır. Ardından aşağıdaki adımlardan birini veya birkaçını deneyin:

* Kümenin bulunduğu konumda kullanılabilir çekirdek olup olmadığını denetleyin.

* Diğer konumlardaki kullanılabilir çekirdek sayısına göz atın. Kümenizi yeterli sayıda kullanılabilir çekirdek içeren farklı bir konumda yeniden oluşturmayı deneyin.

* Belirli bir konumun çekirdek kotasını artırmak istiyorsanız lütfen, HDInsight çekirdek kotasını artırma isteği için bir destek bileti oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
