---
title: izleme hatası bir Azure HDInsight kümesinden yazılım yavaş kilit hatası
description: İzleme hatası Soft kilitleniyor CPU, Azure HDInsight kümesi 'ndeki çekirdek Syslog 'lar 'da görünür
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.date: 08/05/2019
ms.openlocfilehash: 618a871bee762900d4c99700cde42d759506b4b9
ms.sourcegitcommit: fa4852cca8644b14ce935674861363613cf4bfdf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2019
ms.locfileid: "70810590"
---
# <a name="scenario-watchdog-bug-soft-lockup---cpu-error-from-an-azure-hdinsight-cluster"></a>Senaryo: "izleme: Hata: bir Azure HDInsight kümesinden yazılım kilitleme-CPU "hatası

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Çekirdek Syslog 'lar hata iletisini içerir: `watchdog: BUG: soft lockup - CPU`.

## <a name="cause"></a>Nedeni

Linux çekirdeğindeki bir [hata](https://bugzilla.kernel.org/show_bug.cgi?id=199437) , CPU yumuşak lockups sorunlarına neden oluyor.

## <a name="resolution"></a>Çözüm

Çekirdek Düzeltme Eki uygulayın. Aşağıdaki betik, Linux çekirdeğini yükseltir ve makineleri 24 saat üzerinden farklı zamanlarda yeniden başlatır. Betik eylemini iki toplu işlem halinde yürütün. İlk Batch, baş düğüm hariç tüm düğümlerde bulunur. İkinci toplu işlem baş düğümüdür. Baş düğüm ve diğer düğümleri aynı anda çalıştırmayın.

1. Azure portal HDInsight kümenize gidin.

1. Betik eylemlerine gidin.

1. **Yeni Gönder** ' i seçin ve girişi aşağıdaki gibi girin

    | Özellik | Value |
    | --- | --- |
    | Betik türü | -Özel |
    | Name |Çekirdek geçici kilit sorunu için çözüm |
    | Bash betiği URI'si |`https://raw.githubusercontent.com/hdinsight/hdinsight.github.io/master/ClusterCRUD/KernelSoftLockFix/scripts/KernelSoftLockIssue_FixAndReboot.sh` |
    | Düğüm türleri |Çalışan, Zookeeper |
    | Parametreler |Yok |

    Yeni düğümler eklendiğinde betiği yürütmek istiyorsanız **bu betiği kalıcı yap...** öğesini seçin.

1. **Oluştur**’u seçin.

1. Yürütmenin başarılı olmasını bekleyin.

1. Adım 3 ile aynı adımları izleyerek ve bu kez düğüm türleri ile birlikte betik eylemini baş düğümde yürütün: Başlı.

1. Yürütmenin başarılı olmasını bekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* Azure Community [@AzureSupport](https://twitter.com/azuresupport) 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı ile bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
