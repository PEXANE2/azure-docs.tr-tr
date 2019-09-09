---
title: Azure HDInsight kümesine düğüm eklenemiyor
description: Azure HDInsight 'ta Apache Hadoop kümesine neden düğüm ekleyemedik sorunlarını giderme
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 07/31/2019
ms.openlocfilehash: 0c8fdf11f435cc459dfb8475f8983b29dfbc5d9f
ms.sourcegitcommit: fa4852cca8644b14ce935674861363613cf4bfdf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2019
ms.locfileid: "70810560"
---
# <a name="scenario-unable-to-add-nodes-to-azure-hdinsight-cluster"></a>Senaryo: Azure HDInsight kümesine düğüm eklenemiyor

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Azure HDInsight kümesine düğüm eklenemiyor.

## <a name="cause"></a>Nedeni

Nedenler farklılık gösterebilir.

## <a name="resolution"></a>Çözüm

[Küme boyutu](../hdinsight-scaling-best-practices.md) özelliğini kullanarak, küme için gereken ek çekirdek sayısını hesaplayın. Bu, yeni çalışan düğümlerindeki toplam çekirdek sayısına bağlıdır. Ardından aşağıdaki adımlardan birini veya daha fazlasını deneyin:

* Kümenin konumunda kullanılabilir çekirdekler olup olmadığını denetleyin.

* Diğer konumlardaki kullanılabilir çekirdek sayısına göz atın. Kümenizi yeterli sayıda kullanılabilir çekirdek içeren farklı bir konumda yeniden oluşturmayı deneyin.

* Belirli bir konumun çekirdek kotasını artırmak istiyorsanız lütfen, HDInsight çekirdek kotasını artırma isteği için bir destek bileti oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* Azure Community [@AzureSupport](https://twitter.com/azuresupport) 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
