---
title: Azure HDInsight'ta küme düğümünde disk alanı tükeniyor
description: Azure HDInsight'ta Sorun Giderme Apache Hadoop küme düğümü disk alanı sorunları.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/05/2019
ms.openlocfilehash: fbfd82473b68f5032d19834ac809191d498a5a67
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75894123"
---
# <a name="scenario-cluster-node-runs-out-of-disk-space-in-azure-hdinsight"></a>Senaryo: Azure HDInsight'ta küme düğümü disk alanı tükeniyor

Bu makalede, Azure HDInsight kümeleriyle etkileşimde olurken sorun giderme adımları ve sorunlarla ilgili olası çözümler açıklanmaktadır.

## <a name="issue"></a>Sorun

Bir iş, şuna benzer hata iletisiyle başarısız olabilir:`/usr/hdp/2.6.3.2-14/hadoop/libexec/hadoop-config.sh: fork: No space left on device.`

Ya da benzer Apache Ambari `local-dirs usable space is below configured utilization percentage`uyarı alabilirsiniz: .

## <a name="cause"></a>Nedeni

Apache İplik uygulama önbelleği kullanılabilir tüm disk alanını tüketmiş olabilir. Kıvılcım uygulamanız büyük olasılıkla verimsiz çalışıyor.

## <a name="resolution"></a>Çözüm

1. Hangi düğümün disk alanının tükendiğini belirlemek için Ambari UI'yi kullanın.

1. Rahatsız düğümdeki klasörün disk alanının çoğuna katkıda bulunabilen klasörü belirleyin. Önce düğüme SSH, sonra `df` tüm bağlar için disk kullanımını listelemek için çalıştırın. Genellikle OSS tarafından kullanılan geçici bir disktir. `/mnt` Bir klasöre girebilir, ardından `sudo du -hs` bir klasörün altında özetlenmiş dosya boyutlarını göstermek için yazabilirsiniz. Benzer bir klasör `/mnt/resource/hadoop/yarn/local/usercache/livy/appcache/application_1537280705629_0007`görürseniz, bu uygulamanın hala çalışıyor olduğu anlamına gelir. Bunun nedeni RDD kalıcılığı veya ara karıştırma dosyaları olabilir.

1. Sorunu azaltmak için, bu uygulama tarafından kullanılan disk alanı serbest bırakacaktır uygulama öldürmek.

1. Sonuçta sorunu gidermek için, uygulamanızı optimize edin.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediyseniz veya sorununuzu çözemiyorsanız, daha fazla destek için aşağıdaki kanallardan birini ziyaret edin:

* [Azure Topluluk Desteği](https://azure.microsoft.com/support/community/)aracılığıyla Azure uzmanlarından yanıtlar alın.

* [@AzureSupport](https://twitter.com/azuresupport) Azure topluluğunu doğru kaynaklara bağlayarak müşteri deneyimini geliştirmek için resmi Microsoft Azure hesabına bağlanın: yanıtlar, destek ve uzmanlar.

* Daha fazla yardıma ihtiyacınız varsa, [Azure portalından](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)bir destek isteği gönderebilirsiniz. Menü çubuğundan **Destek'i** seçin veya **Yardım + destek** merkezini açın. Daha ayrıntılı bilgi için lütfen [Azure destek isteği nin nasıl oluşturulabildiğini](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request)gözden geçirin. Abonelik Yönetimi'ne erişim ve faturalandırma desteği Microsoft Azure aboneliğinize dahildir ve Teknik Destek Azure [Destek Planlarından](https://azure.microsoft.com/support/plans/)biri aracılığıyla sağlanır.
