---
title: Azure HDInsight 'ta küme düğümünün disk alanı tükeniyor
description: Azure HDInsight 'ta küme düğümü disk alanı sorunlarını giderme Apache Hadoop.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 04/30/2020
ms.openlocfilehash: f1a994ad07980c67e37d00bffb7e605ed610bb08
ms.sourcegitcommit: 7863fcea618b0342b7c91ae345aa099114205b03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2020
ms.locfileid: "93289076"
---
# <a name="scenario-cluster-node-runs-out-of-disk-space-in-azure-hdinsight"></a>Senaryo: küme düğümünde Azure HDInsight 'ta disk alanı tükeniyor

Bu makalede, Azure HDInsight kümeleriyle etkileşim kurarken sorun giderme adımları ve olası çözümleri açıklanmaktadır.

## <a name="issue"></a>Sorun

Bir iş şuna benzer bir hata iletisiyle başarısız olabilir: `/usr/hdp/2.6.3.2-14/hadoop/libexec/hadoop-config.sh: fork: No space left on device.`

Ya da şuna benzer Apache ambarı uyarısı alabilirsiniz: `local-dirs usable space is below configured utilization percentage` .

## <a name="cause"></a>Nedeni

Apache YARN uygulama önbelleği, kullanılabilir tüm disk alanını tüketilebilir. Spark uygulamanız muhtemelen yeterince çalışıyor.

## <a name="resolution"></a>Çözüm

1. Hangi düğümün disk alanının tükençalıştığını anlamak için, ambarı Kullanıcı arabirimini kullanın.

1. Araba düğümündeki hangi klasörün disk alanının çoğuna katkıda bulunduğunu belirleme. Önce bir düğüme SSH, sonra `df` Tüm takar disk kullanımını listelemek için çalıştırın. Genellikle, `/mnt` OSS tarafından kullanılan bir geçici disktir. Bir klasöre girip, ardından `sudo du -hs` bir klasör altında özetlenen dosya boyutlarını göstermek için yazabilirsiniz. Benzer bir klasör görürseniz `/mnt/resource/hadoop/yarn/local/usercache/livy/appcache/application_1537280705629_0007` , bu, uygulamanın hala çalıştığı anlamına gelir. Bunun nedeni, RDD kalıcılığı veya ara karıştırma dosyalarından kaynaklanabilir.

1. Sorunu azaltmak için, uygulamayı sonlandırın, bu uygulama tarafından kullanılan disk alanını serbest bırakacaktır.

1. Sorun genellikle çalışan düğümlerinde gerçekleşirse, kümedeki YARN yerel önbellek ayarlarını ayarlayabilirsiniz.

    Ambarı açma kullanıcı arabirimini açın YARN--> configs--> Gelişmiş ' e gidin.  
    Aşağıdaki 2 özelliklerini özel yarn-site.xml bölümüne ekleyin ve kaydedin:

    ```
    yarn.nodemanager.localizer.cache.target-size-mb=2048
    yarn.nodemanager.localizer.cache.cleanup.interval-ms=300000
    ```

1. Yukarıdaki sorun sorunu kalıcı olarak gidermezse, uygulamanızı iyileştirin.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [troubleshooting next steps](../../../includes/hdinsight-troubleshooting-next-steps.md)]