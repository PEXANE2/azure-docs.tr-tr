---
title: Azure HDInsight sürüm notları
description: Azure HDInsight için en son sürüm notları. Hadoop, Spark, R Server, Hive ve daha fazlası için geliştirme ipuçları ve ayrıntıları alın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 06/11/2020
ms.openlocfilehash: a4faab9ac6d5e1c39c1120e09dae792b95892d60
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85564409"
---
# <a name="release-notes"></a>Sürüm notları

Bu makalede **en son** Azure HDInsight sürüm güncelleştirmeleri hakkında bilgi sağlanır. Önceki sürümler hakkında daha fazla bilgi için bkz. [HDInsight sürüm notları Arşivi](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Özet

Azure HDInsight, Azure üzerinde açık kaynaklı analizler için kurumsal müşteriler arasındaki en popüler hizmetlerden biridir.

## <a name="release-date-06112020"></a>Yayın tarihi: 06/11/2020

Bu sürüm yalnızca HDInsight 3,6 ve 4,0 için geçerlidir. HDInsight yayını, birkaç gün boyunca tüm bölgeler için kullanılabilir hale getirilir. Burada Yayımlanma tarihi, ilk bölgenin yayın tarihini gösterir. Değişiklikleri aşağıda görmüyorsanız, bölgenin bölgeniz için birkaç gün içinde canlı olmasını bekleyin.

## <a name="new-features"></a>Yeni özellikler
### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümelerine geçme
HDInsight, kümeyi şimdi sağlamak için Azure sanal makinelerini kullanır. Bu sürümden, yeni oluşturulan HDInsight kümeleri Azure sanal makine ölçek kümesini kullanmaya başlar. Değişiklik kademeli olarak kullanıma alınıyor. Hiç bir değişiklik olmaması beklenmelidir. [Azure sanal makine ölçek kümeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview)hakkında daha fazla bilgi için bkz..
 
### <a name="reboot-vms-in-hdinsight-cluster"></a>HDInsight kümesinde VM 'Leri yeniden başlatma
Bu sürümde, yanıt vermeyen düğümleri yeniden başlatmak için HDInsight kümesindeki sanal makinelerin yeniden başlatılmasını destekliyoruz. Şu anda yalnızca API aracılığıyla yapabilirsiniz, PowerShell ve CLı desteği bu şekilde yapılır. API hakkında daha fazla bilgi için [Bu belgeye](https://github.com/Azure/azure-rest-api-specs/codeowners/master/specification/hdinsight/resource-manager/Microsoft.HDInsight/stable/2018-06-01-preview/virtualMachines.json)bakın.
 
## <a name="deprecation"></a>Kullanımdan kaldırma
### <a name="deprecation-of-spark-21-and-22-in-hdinsight-36-spark-cluster"></a>HDInsight 3.6 Spark kümesinde Spark 2.1 ile 2.2’nin kullanımdan kaldırılması
1 2020 Temmuz 'dan başlayarak, müşteriler HDInsight 3,6 üzerinde Spark 2,1 ve 2,2 ile yeni Spark kümeleri oluşturamaz. Mevcut kümeler, Microsoft desteği olmadan olduğu gibi çalışır. Olası sistem/destek kesintilerini önlemek için HDInsight 3,6 ' ye 30 2020 göre Spark 2,3 ' ye geçmek için göz önünde bulundurun.
 
### <a name="deprecation-of-spark-23-in-hdinsight-40-spark-cluster"></a>HDInsight 4.0 Spark kümesinde Spark 2.3’ün kullanımdan kaldırılması
1 2020 Temmuz 'dan başlayarak, müşteriler HDInsight 4,0 üzerinde Spark 2,3 ile yeni Spark kümeleri oluşturamaz. Mevcut kümeler, Microsoft desteği olmadan olduğu gibi çalışır. Olası sistem/destek kesintilerini önlemek için 30 Haziran 2020’ye kadar HDInsight 4.0’da Spark 2.4’e geçmeyi göz önünde bulundurun.
 
### <a name="deprecation-of-kafka-11-in-hdinsight-40-kafka-cluster"></a>HDInsight 4.0 Kafka kümesinde Kafka 1.1’i kullanımdan kaldırma
1 2020 Temmuz 'dan başlayarak, müşteriler HDInsight 4,0 üzerinde Kafka 1,1 ile yeni Kafka kümeleri oluşturamayacak. Mevcut kümeler, Microsoft desteği olmadan olduğu gibi çalışır. Olası sistem/destek kesintilerini önlemek için 30 Haziran 2020’ye kadar HDInsight 4.0’da Kafka 2.1’e geçmeyi göz önünde bulundurun.
 
## <a name="behavior-changes"></a>Davranış değişiklikleri
### <a name="esp-spark-cluster-head-node-size-change"></a>ESP Spark kümesi baş düğüm boyutu değişikliği 
ESP Spark kümesi için izin verilen en düşük baş düğüm boyutu Standard_D13_V2 olarak değiştirilir. Düşük çekirdekler ve bellek ile baş düğümü olan VM 'Ler görece düşük CPU ve bellek kapasitesi nedeniyle ESP kümesi sorunlarına neden olabilir. Yayından başlayarak, Standard_D13_V2 ve ESP Spark kümeleri için baş düğüm olarak Standard_E16_V3 SKU 'Ları kullanın.
 
### <a name="a-minimum-4-core-vm-is-required-for-head-node"></a>Baş düğüm için en az 4 çekirdekli bir VM gereklidir 
Baş düğüm için en az 4 çekirdekli bir VM, HDInsight kümelerinin yüksek kullanılabilirliğini ve güvenilirliğini güvence altına almak için gereklidir. Müşteriler, 6 2020 Nisan 'dan başlayarak yeni HDInsight kümeleri için yalnızca 4 çekirdekli veya VM 'nin üzerinde baş düğüm olarak seçim yapabilir. Mevcut kümeler beklenen şekilde çalışmaya devam edecektir. 
 
### <a name="cluster-worker-node-provisioning-change"></a>Küme çalışan düğümü sağlama değişikliği
Çalışan düğümlerinin %80 ' u hazır olduğunda, küme **İşlemsel** aşamasına girer. Bu aşamada, müşteriler betikleri ve işleri çalıştırmak gibi tüm veri düzlemi işlemlerini gerçekleştirebilir. Ancak müşteriler ölçeği artırma/azaltma gibi denetim düzlemi işlemlerini yapamıyor. Yalnızca silme desteklenir.
 
**İşletimsel** aşamada, küme kalan %20 çalışan düğümü için başka bir 60 dakika bekler. Bu 60 dakikanın sonunda, tüm çalışan düğümleri hala kullanılabilir olmasa bile küme **çalışan** aşamaya geçer. Bir küme **çalışan** aşamasına girdiğinde, normal olarak kullanabilirsiniz. Her ikisi de ölçeği artırma/azaltma gibi denetim planı işlemleri ve komut dosyaları ve işler gibi veri planı işlemleri kabul edilir. İstenen çalışan düğümlerinden bazıları yoksa, küme kısmen başarılı olarak işaretlenir. Başarıyla dağıtılan düğümler için ücretlendirilirsiniz. 
 
### <a name="create-new-service-principal-through-hdinsight"></a>HDInsight ile yeni hizmet sorumlusu oluşturma
Daha önce, küme oluşturma ile müşteriler, Azure portal bağlı ADLS Gen 1 hesabına erişmek için yeni bir hizmet sorumlusu oluşturabilir. 15 2020 Haziran 'dan itibaren müşteriler HDInsight oluşturma iş akışında yeni hizmet sorumlusu oluşturamaz, yalnızca mevcut hizmet sorumlusu desteklenir. Bkz. [Azure Active Directory kullanarak hizmet sorumlusu ve sertifikaları oluşturma](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

### <a name="time-out-for-script-actions-with-cluster-creation"></a>Küme oluşturma ile betik eylemleri için zaman aşımı
HDInsight, küme oluşturma ile betik eylemlerinin çalıştırılmasını destekler. Bu sürümden, küme oluşturma ile birlikte tüm betik eylemlerinin **60 dakika**içinde tamamlaması veya zaman aşımına geçmesi gerekir. Çalışan kümelere gönderilen betik eylemleri etkilenmez. Ayrıntılar hakkında daha [fazla bilgi edinin](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#script-action-in-the-cluster-creation-process).
 
## <a name="upcoming-changes"></a>Yaklaşan değişiklikler
Dikkat etmeniz gereken yaklaşan Son değişiklik yok.
 
## <a name="bug-fixes"></a>Hata düzeltmeleri
HDInsight, küme güvenilirliği ve performans iyileştirmeleri yapmaya devam eder. 
 
## <a name="component-version-change"></a>Bileşen sürümü değişikliği
### <a name="hbase-20-to-216"></a>HBase 2,0 2.1.6
HBase sürümü 2,0 sürümünden 2.1.6 sürümüne yükseltilir.
 
### <a name="spark-240-to-244"></a>Spark 2.4.0 to 2.4.4
Spark sürümü, 2.4.0 sürümünden 2.4.4 sürümüne yükseltilir.
 
### <a name="kafka-210-to-211"></a>Kafka 2.1.0 to 2.1.1
Kafka sürümü 2.1.0 sürümünden 2.1.1 sürümüne yükseltilir.
 
HDInsight 4,0 ad HDInsight 3,6 için geçerli bileşen sürümlerini [Bu belgede](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#apache-hadoop-components-available-with-different-hdinsight-versions) bulabilirsiniz

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="hive-warehouse-connector-issue"></a>Hive ambarı bağlayıcı sorunu
Bu sürümde Hive ambar Bağlayıcısı için bir sorun var. Bu çözüm bir sonraki sürüme eklenecektir. Bu sürümden önce oluşturulan mevcut kümeler etkilenmiyor. Mümkün olduğunda, önlemeyi ve kümeyi yeniden oluşturmayı önleyin. Bu konuda daha fazla yardıma ihtiyacınız varsa lütfen destek bileti 'ni açın.
