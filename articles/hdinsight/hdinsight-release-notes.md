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
ms.openlocfilehash: 0895e84363d40bdbf30408f2b2a0d95f951eb303
ms.sourcegitcommit: 3ea45bbda81be0a869274353e7f6a99e4b83afe2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97032567"
---
# <a name="azure-hdinsight-release-notes"></a>Azure HDInsight sürüm notları

Bu makalede **en son** Azure HDInsight sürüm güncelleştirmeleri hakkında bilgi sağlanır. Önceki sürümler hakkında daha fazla bilgi için bkz. [HDInsight sürüm notları Arşivi](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Özet

Azure HDInsight, Azure üzerinde açık kaynaklı analizler için kurumsal müşteriler arasındaki en popüler hizmetlerden biridir.

Sürüm notlarında abone olmak istiyorsanız, [Bu GitHub deposundaki](https://github.com/hdinsight/release-notes/releases)yayınları izleyin.

## <a name="release-date-11182020"></a>Yayın tarihi: 11/18/2020

Bu sürüm hem HDInsight 3,6 hem de HDInsight 4,0 için geçerlidir. HDInsight yayını, birkaç gün boyunca tüm bölgeler için kullanılabilir hale getirilir. Burada Yayımlanma tarihi, ilk bölgenin yayın tarihini gösterir. Değişiklikleri aşağıda görmüyorsanız, bölgenin bölgeniz için birkaç gün içinde canlı olmasını bekleyin.

## <a name="new-features"></a>Yeni özellikler
### <a name="auto-key-rotation-for-customer-managed-key-encryption-at-rest"></a>Müşteri tarafından yönetilen anahtar şifrelemesi için bekleyen otomatik anahtar döndürme
Müşteriler, bu sürümden itibaren, bekleyen müşteri tarafından yönetilen anahtar şifrelemesi için Azure KeyValut sürümü-Less şifreleme anahtarı URL 'Lerini kullanabilir. HDInsight, süreleri sona erdiğinde veya yeni sürümlerle değiştirildikleri zaman otomatik olarak bu anahtarları döndürür. Ayrıntılar hakkında daha [fazla bilgi edinin](./disk-encryption.md).

### <a name="ability-to-select-different-zookeeper-virtual-machine-sizes-for-spark-hadoop-and-ml-services"></a>Spark, Hadoop ve ML Hizmetleri için farklı Zookeeper sanal makine boyutları seçme olanağı
HDInsight daha önce Spark, Hadoop ve ML Hizmetleri küme türleri için Zookeeper düğüm boyutunu özelleştirmeyi desteklememektedir. Bu, ücretsiz olarak sunulan A2_v2/a2 sanal makine boyutlarını varsayılan olarak alır. Bu sürümden, senaryonuza en uygun Zookeeper sanal makine boyutunu seçebilirsiniz. Sanal makine boyutu A2_v2/a2 dışındaki Zookeeper düğümleri ücretlendirilecektir. A2_v2 ve a2 sanal makineler ücretsiz olarak sunulur.

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümelerine geçme
HDInsight artık kümeyi sağlamak için Azure sanal makinelerini kullanır. Bu sürümden itibaren, hizmet giderek [Azure sanal makine ölçek kümelerine](../virtual-machine-scale-sets/overview.md)geçiş yapar. İşlemin tamamı ayda sürebilir. Bölgelerinizden ve abonelikleriniz geçirildikten sonra, yeni oluşturulan HDInsight kümeleri, müşteri eylemleri olmadan sanal makine ölçek kümelerinde çalışır. Hiçbir bölme değişikliği beklenmez.

## <a name="deprecation"></a>Kullanımdan kaldırma
### <a name="deprecation-of-hdinsight-36-ml-services-cluster"></a>HDInsight 3,6 ML Hizmetleri kümesinin kullanımdan kaldırılması
HDInsight 3,6 ML Hizmetleri küme türü 31 2020 Aralık 'Ta destek bitişi olacak. Müşteriler, 31 2020 Aralık 'tan sonra yeni 3,6 ML Hizmetleri kümeleri oluşturmaz. Mevcut kümeler, Microsoft desteği olmadan olduğu gibi çalışır. HDInsight sürümleri için destek süre sonunu ve [küme türlerini kontrol](./hdinsight-component-versioning.md#available-versions)edin.

### <a name="disabled-vm-sizes"></a>Devre dışı VM boyutları
HDInsight, 16 2020 Kasım 'dan başlayarak standand_A8, standand_A9, standand_A10 ve standand_A11 VM boyutlarını kullanarak kümeler oluşturan yeni müşterileri engelleyecek. Son üç ay içinde bu VM boyutlarını kullanmış olan mevcut müşteriler etkilenmez. HDInsight, 9 2021 Ocak 'Tan başlayarak standand_A8, standand_A9, standand_A10 ve standand_A11 VM boyutlarını kullanarak küme oluşturan tüm müşterileri engelleyecek. Mevcut kümeler olduğu gibi çalışır. Olası sistem/destek kesintilerini önlemek için HDInsight 4,0 ' ye geçmeyi düşünün.

## <a name="behavior-changes"></a>Davranış değişiklikleri
### <a name="add-nsg-rule-checking-before-scaling-operation"></a>Ölçeklendirmeden önce NSG kuralı denetimi ekleme işlemi
HDInsight, ölçeklendirme işlemiyle ağ güvenlik grupları (NSG 'ler) ve Kullanıcı tanımlı yollar (UDRs) denetimi ekledi. Küme oluşturmanın yanı sıra küme ölçekleme için de aynı doğrulama yapılır. Bu doğrulama öngörülemeyen hataları önlemeye yardımcı olur. Doğrulama geçmezse ölçeklendirme başarısız olur. NSG 'ler ve UDRs 'nin doğru şekilde nasıl yapılandırılacağı hakkında daha fazla bilgi edinin. [HDInsight YÖNETIM IP adresleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-management-ip-addresses)bölümüne bakın.

## <a name="upcoming-changes"></a>Yaklaşan değişiklikler
Gelecek sürümlerde aşağıdaki değişiklikler olur.

### <a name="default-cluster-version-will-be-changed-to-40"></a>Varsayılan küme sürümü 4,0 olarak değiştirilecek
2021 Şubat tarihinden itibaren, HDInsight kümesinin varsayılan sürümü 3,6 ' den 4,0 ' ye değiştirilecek. Kullanılabilir sürümler hakkında daha fazla bilgi için bkz. [kullanılabilir sürümler](./hdinsight-component-versioning.md#available-versions). [Hdınsight 4,0](./hdinsight-version-release.md) ' deki yenilikler hakkında daha fazla bilgi edinin

### <a name="hdinsight-36-end-of-support-on-june-30-2021"></a>HDInsight 3,6 30 2021 Haziran 'da destek bitişi
HDInsight 3,6, destek sonu olacaktır. Form Haziran 30 2021 ' den itibaren, müşteriler yeni HDInsight 3,6 kümeleri oluşturamaz. Mevcut kümeler, Microsoft desteği olmadan olduğu gibi çalışır. Olası sistem/destek kesintilerini önlemek için HDInsight 4,0 ' ye geçmeyi düşünün.

## <a name="bug-fixes"></a>Hata düzeltmeleri
HDInsight, küme güvenilirliği ve performans iyileştirmeleri yapmaya devam eder. 

## <a name="component-version-change"></a>Bileşen sürümü değişikliği
Bu yayın için bileşen sürümü değişikliği yok. HDInsight 4,0 ve HDInsight 3,6 için geçerli bileşen sürümlerini [Bu belgede](./hdinsight-component-versioning.md)bulabilirsiniz.

## <a name="known-issues"></a>Bilinen sorunlar
### <a name="prevent-hdinsight-cluster-vms-from-rebooting-periodically"></a>HDInsight küme VM 'lerinin düzenli aralıklarla yeniden başlatılmasını engelle

2020 Kasım 'dan başlayarak, HDInsight kümesi VM 'lerinin düzenli olarak yeniden başlatılmasını fark etmiş olabilirsiniz. Bunun nedeni şunlar olabilir:

1.  CTIO v, kümenizde etkin. Yeni azsec-cBir v paketi, düğüm yeniden başlatılmasını tetikleyen büyük miktarda bellek tüketir. 
2.  Azure hizmetleri tarafından kullanılan sertifika yetkilileri (CA) listesinde yapılan değişiklikleri izleyen bir CRON işi günlük olarak zamanlanır. Yeni bir CA sertifikası kullanılabilir olduğunda, betik sertifikayı JDK güven deposuna ekler ve yeniden başlatma zamanlar.

HDInsight, düzeltmeleri dağıtmakta ve her iki sorun için çalışan tüm kümeler için düzeltme eki uyguluyor. Bu işlemi hemen uygulamak ve beklenmeyen VM 'Lerin yeniden başlatılmasını önlemek için, aşağıdaki betik eylemlerini tüm küme düğümlerinde kalıcı betik eylemi olarak çalıştırabilirsiniz. HDInsight düzeltme ve düzeltme eki tamamlandıktan sonra başka bir bildirim gönderecek.
```
https://hdiconfigactions.blob.core.windows.net/linuxospatchingrebootconfigv02/replace_cacert_script.sh
https://healingscriptssa.blob.core.windows.net/healingscripts/ChangeOOMPolicyAndApplyLatestConfigForClamav.sh
```

