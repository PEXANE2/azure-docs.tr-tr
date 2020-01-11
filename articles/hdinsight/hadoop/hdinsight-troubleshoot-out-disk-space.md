---
title: Azure HDInsight 'ta küme düğümünün disk alanı tükeniyor
description: Azure HDInsight 'ta küme düğümü disk alanı sorunlarını giderme Apache Hadoop.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/05/2019
ms.openlocfilehash: fbfd82473b68f5032d19834ac809191d498a5a67
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75894123"
---
# <a name="scenario-cluster-node-runs-out-of-disk-space-in-azure-hdinsight"></a>Senaryo: küme düğümünde Azure HDInsight 'ta disk alanı tükeniyor

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Bir iş şuna benzer bir hata iletisiyle başarısız olabilir: `/usr/hdp/2.6.3.2-14/hadoop/libexec/hadoop-config.sh: fork: No space left on device.`

Ya da şuna benzer Apache ambarı uyarısı alabilirsiniz: `local-dirs usable space is below configured utilization percentage`.

## <a name="cause"></a>Nedeni

Apache YARN uygulama önbelleği, kullanılabilir tüm disk alanını tüketilebilir. Spark uygulamanız muhtemelen yeterince çalışıyor.

## <a name="resolution"></a>Çözünürlük

1. Hangi düğümün disk alanının tükençalıştığını anlamak için, ambarı Kullanıcı arabirimini kullanın.

1. Araba düğümündeki hangi klasörün disk alanının çoğuna katkıda bulunduğunu belirleme. Önce bir düğüme SSH, sonra tüm takar disk kullanımını listelemek için `df` çalıştırın. Genellikle, OSS tarafından kullanılan bir geçici disk olan `/mnt`. Bir klasöre girip, bir klasör altında özetlenen dosya boyutlarını göstermek için `sudo du -hs` yazın. `/mnt/resource/hadoop/yarn/local/usercache/livy/appcache/application_1537280705629_0007`benzer bir klasör görürseniz, bu uygulamanın çalışmaya devam ettiği anlamına gelir. Bunun nedeni, RDD kalıcılığı veya ara karıştırma dosyalarından kaynaklanabilir.

1. Sorunu azaltmak için, uygulamayı sonlandırın, bu uygulama tarafından kullanılan disk alanını serbest bırakacaktır.

1. Sorunu en sonunda çözmek için uygulamanızı iyileştirin.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmüyorsanız veya sorununuzu çözemediyseniz, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* Azure [topluluk desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıt alın.

* [@AzureSupport](https://twitter.com/azuresupport) ile bağlanma-Azure Community 'yi doğru kaynaklara bağlayarak müşteri deneyimini iyileştirmeye yönelik resmi Microsoft Azure hesabı: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **destek** ' i seçin veya **Yardım + Destek** hub 'ını açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği oluşturma](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)konusunu inceleyin. Abonelik yönetimi ve faturalandırma desteği 'ne erişim Microsoft Azure aboneliğinize dahildir ve [Azure destek planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla teknik destek sağlanır.
