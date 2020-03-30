---
title: Azure HDInsight kümesinden Watchdog BUG yumuşak kilitleme CPU hatası
description: Watchdog BUG yumuşak kilitleme CPU Azure HDInsight kümesinden çekirdek syslogs görünür
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/05/2019
ms.openlocfilehash: 701e314ad2a3762b1e8ca022ce18d9435ce2db37
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75894116"
---
# <a name="scenario-watchdog-bug-soft-lockup---cpu-error-from-an-azure-hdinsight-cluster"></a>Senaryo: "watchdog: BUG: yumuşak kilitleme - CPU" hatası bir Azure HDInsight kümesinden

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Çekirdek syslogs hata iletisi `watchdog: BUG: soft lockup - CPU`içerir: .

## <a name="cause"></a>Nedeni

Linux Kernel bir [hata](https://bugzilla.kernel.org/show_bug.cgi?id=199437) CPU yumuşak kilitleme neden oluyor.

## <a name="resolution"></a>Çözüm

Çekirdek yama uygulayın. Aşağıdaki komut dosyası linux çekirdeğini yükseltir ve makineleri 24 saat içinde farklı zamanlarda yeniden başlatacaktır. Komut dosyası eylemini iki toplu olarak yürütün. İlk parti baş düğümü hariç tüm düğümler üzerindedir. İkinci parti baş düğümünde. Kafa düğümü ve diğer düğümleri aynı anda çalıştırmayın.

1. Azure portalından HDInsight kümenize gidin.

1. Komut dosyası eylemlerine gidin.

1. **Yeni Gönder'i** seçin ve girişi aşağıdaki gibi girin

    | Özellik | Değer |
    | --- | --- |
    | Komut dosyası türü | -Özel |
    | Adı |Çekirdek yumuşak kilit sorunu için düzeltme |
    | Bash script URI |`https://raw.githubusercontent.com/hdinsight/hdinsight.github.io/master/ClusterCRUD/KernelSoftLockFix/scripts/KernelSoftLockIssue_FixAndReboot.sh` |
    | Düğüm türü(ler) |İşçi, Hayvan bakıcısı |
    | Parametreler |Yok |

    **Bu komut dosyası eylemini devam la...** yeni düğümler eklendiğinde komut dosyasının yürütülmesini istiyorsanız devam edin'i seçin.

1. **Oluştur'u**seçin.

1. Yürütmenin başarılı olmasını bekleyin.

1. Baş düğüm üzerinde komut dosyası eylemini adım 3 ile aynı adımları izleyerek uygulayın, ancak bu kez Düğüm türleri: Head ile.

1. Yürütmenin başarılı olmasını bekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
