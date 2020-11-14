---
title: Azure HDInsight sürüm notları
description: Azure HDInsight için en son sürüm notları. Hadoop, Spark, R Server, Hive ve daha fazlası için geliştirme ipuçları ve ayrıntıları alın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 11/12/2020
ms.openlocfilehash: 1ae4e2a1e0d67a0a09c19b517245ffc6d92d17df
ms.sourcegitcommit: 9826fb9575dcc1d49f16dd8c7794c7b471bd3109
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2020
ms.locfileid: "94629929"
---
# <a name="azure-hdinsight-release-notes"></a>Azure HDInsight sürüm notları

Bu makalede **en son** Azure HDInsight sürüm güncelleştirmeleri hakkında bilgi sağlanır. Önceki sürümler hakkında daha fazla bilgi için bkz. [HDInsight sürüm notları Arşivi](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Özet

Azure HDInsight, Azure üzerinde açık kaynaklı analizler için kurumsal müşteriler arasındaki en popüler hizmetlerden biridir.

Sürüm notlarında abone olmak istiyorsanız, [Bu GitHub deposundaki](https://github.com/hdinsight/release-notes/releases)yayınları izleyin.

## <a name="release-date-11092020"></a>Yayın tarihi: 11/09/2020

Bu sürüm hem HDInsight 3,6 hem de HDInsight 4,0 için geçerlidir. HDInsight yayını, birkaç gün boyunca tüm bölgeler için kullanılabilir hale getirilir. Burada Yayımlanma tarihi, ilk bölgenin yayın tarihini gösterir. Değişiklikleri aşağıda görmüyorsanız, bölgenin bölgeniz için birkaç gün içinde canlı olmasını bekleyin.

## <a name="new-features"></a>Yeni özellikler
### <a name="hdinsight-identity-broker-hib-is-now-ga"></a>HDInsight Identity broker (HIB) artık GA 'dir
ESP kümelerinde OAuth kimlik doğrulamasını sağlayan HDInsight Identity broker (HIB), bu yayında genel kullanıma sunulmuştur. Bu sürümden sonra oluşturulan HIB kümeleri en son HIB özelliklerine sahip olur:
- Yüksek kullanılabilirlik (HA)
- Multi-Factor Authentication için destek (MFA)
- Federasyon kullanıcıları AAD-DS için Parola karması eşitlemesi olmadan oturum açıp daha fazla bilgi Için bkz. [Hib belgeleri](https://docs.microsoft.com/azure/hdinsight/domain-joined/identity-broker).

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümelerine geçme
HDInsight artık kümeyi sağlamak için Azure sanal makinelerini kullanır. Bu sürümden itibaren, hizmet giderek [Azure sanal makine ölçek kümelerine](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview)geçiş yapar. İşlemin tamamı ayda sürebilir. Bölgelerinizden ve abonelikleriniz geçirildikten sonra, yeni oluşturulan HDInsight kümeleri, müşteri eylemleri olmadan sanal makine ölçek kümelerinde çalışır. Hiçbir bölme değişikliği beklenmez.

## <a name="deprecation"></a>Kullanımdan kaldırma
### <a name="deprecation-of-hdinsight-36-ml-services-cluster"></a>HDInsight 3,6 ML Hizmetleri kümesinin kullanımdan kaldırılması
HDInsight 3,6 ML Hizmetleri küme türü 31 2020 Aralık 'Ta destek bitişi olacak. Müşteriler, 31 2020 Aralık 'tan sonra yeni 3,6 ML Hizmetleri kümeleri oluşturmaz. Mevcut kümeler, Microsoft desteği olmadan olduğu gibi çalışır. HDInsight sürümleri için destek süre sonunu ve [küme türlerini kontrol](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#available-versions)edin.

### <a name="disabled-vm-sizes"></a>Devre dışı VM boyutları
HDInsight, 16 2020 Kasım 'dan başlayarak standand_A8, standand_A9, standand_A10 ve standand_A11 VM boyutlarını kullanarak kümeler oluşturan yeni müşterileri engelleyecek. Son üç ay içinde bu VM boyutlarını kullanmış olan mevcut müşteriler etkilenmez. HDInsight, 9 2021 Ocak 'Tan başlayarak standand_A8, standand_A9, standand_A10 ve standand_A11 VM boyutlarını kullanarak küme oluşturan tüm müşterileri engelleyecek. Mevcut kümeler olduğu gibi çalışır. Olası sistem/destek kesintilerini önlemek için HDInsight 4,0 ' ye geçmeyi düşünün.

## <a name="behavior-changes"></a>Davranış değişiklikleri
Bu yayın için davranış değişikliği yok.

## <a name="upcoming-changes"></a>Yaklaşan değişiklikler
Gelecek sürümlerde aşağıdaki değişiklikler olur.

### <a name="ability-to-select-different-zookeeper-virtual-machine-sizes-for-spark-hadoop-and-ml-services"></a>Spark, Hadoop ve ML Hizmetleri için farklı Zookeeper sanal makine boyutları seçme olanağı
HDInsight, Spark, Hadoop ve ML Hizmetleri küme türleri için Zookeeper düğüm boyutunu özelleştirmeyi desteklemez. Bu, ücretsiz olarak sunulan A2_v2/a2 sanal makine boyutlarını varsayılan olarak alır. Yaklaşan sürümde, senaryonuza en uygun Zookeeper sanal makine boyutunu seçebilirsiniz. Sanal makine boyutu A2_v2/a2 dışındaki Zookeeper düğümleri ücretlendirilecektir. A2_v2 ve a2 sanal makineler ücretsiz olarak sunulur.

### <a name="default-cluster-version-will-be-changed-to-40"></a>Varsayılan küme sürümü 4,0 olarak değiştirilecek
2021 Şubat tarihinden itibaren, HDInsight kümesinin varsayılan sürümü 3,6 ' den 4,0 ' ye değiştirilecek. Kullanılabilir sürümler hakkında daha fazla bilgi için bkz. [kullanılabilir sürümler](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#available-versions). [Hdınsight 4,0](https://docs.microsoft.com/azure/hdinsight/hdinsight-version-release) ' deki yenilikler hakkında daha fazla bilgi edinin

### <a name="hdinsight-36-end-of-support-on-june-30-2021"></a>HDInsight 3,6 30 2021 Haziran 'da destek bitişi
HDInsight 3,6, destek sonu olacaktır. Form Haziran 30 2021 ' den itibaren, müşteriler yeni HDInsight 3,6 kümeleri oluşturamaz. Mevcut kümeler, Microsoft desteği olmadan olduğu gibi çalışır. Olası sistem/destek kesintilerini önlemek için HDInsight 4,0 ' ye geçmeyi düşünün.

## <a name="bug-fixes"></a>Hata düzeltmeleri
HDInsight, küme güvenilirliği ve performans iyileştirmeleri yapmaya devam eder. 
### <a name="fix-issue-for-restarting-vms-in-cluster"></a>Kümedeki VM 'Leri yeniden başlatma sorununu giderme
Kümedeki VM 'Leri yeniden başlatma sorunu düzeltildi, [kümedeki düğümleri yeniden başlatmak Için PowerShell veya REST API](https://docs.microsoft.com/azure/hdinsight/cluster-reboot-vm) kullanabilirsiniz.

## <a name="component-version-change"></a>Bileşen sürümü değişikliği
Bu yayın için bileşen sürümü değişikliği yok. HDInsight 4,0 ve HDInsight 3,6 için geçerli bileşen sürümlerini [Bu belgede](./hdinsight-component-versioning.md)bulabilirsiniz.
