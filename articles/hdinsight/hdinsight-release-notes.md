---
title: Azure HDInsight sürüm notları
description: Azure HDInsight için en son sürüm notları. Hadoop, Spark, R Server, Hive ve daha fazlası için geliştirme ipuçları ve ayrıntıları alın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/12/2020
ms.openlocfilehash: d0f12274f3cbc0cd2726097e0507079cb858d59f
ms.sourcegitcommit: 6a9f01bbef4b442d474747773b2ae6ce7c428c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84116567"
---
# <a name="release-notes"></a>Sürüm notları

Bu makalede **en son** Azure HDInsight sürüm güncelleştirmeleri hakkında bilgi sağlanır. Önceki sürümler hakkında daha fazla bilgi için bkz. [HDInsight sürüm notları Arşivi](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Özet

Azure HDInsight, Azure üzerinde açık kaynaklı analizler için kurumsal müşteriler arasındaki en popüler hizmetlerden biridir.

## <a name="release-date-01092020"></a>Yayın tarihi: 01/09/2020

Bu sürüm yalnızca HDInsight 3,6 ve 4,0 için geçerlidir. HDInsight yayını, birkaç gün boyunca tüm bölgeler için kullanılabilir hale getirilir. Burada Yayımlanma tarihi, ilk bölgenin yayın tarihini gösterir. Değişiklikleri aşağıda görmüyorsanız, lütfen bölgenizde bir kaç gün içinde etkin olmasını bekleyin.

> [!IMPORTANT]  
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [HDInsight sürüm oluşturma makalesi](hdinsight-component-versioning.md).

## <a name="new-features"></a>Yeni özellikler
### <a name="tls-12-enforcement"></a>TLS 1.2 zorlaması
Aktarım Katmanı Güvenliği (TLS) ve Güvenli Yuva Katmanı (SSL), bir bilgisayar ağı üzerinden iletişim güvenliği sağlayan şifreleme protokolleridir. [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0)hakkında daha fazla bilgi edinin. HDInsight, genel HTTPs uç noktalarında TLS 1,2 kullanır, ancak geriye dönük uyumluluk için TLS 1,1 hala desteklenmektedir. 

Bu sürümde, müşteriler yalnızca genel küme uç noktası üzerinden tüm bağlantılar için TLS 1,2 ' i kabul edebilir. Bunu desteklemek için, **Minsupportedtlsversion** yeni özelliği tanıtılmıştır ve küme oluşturma sırasında belirlenebilir. Özellik ayarlanmamışsa, küme yine de bugün davranışıyla aynı olan TLS 1,0, 1,1 ve 1,2 ' i destekler. Müşteriler bu özelliğin değerini "1,2" olarak ayarlayabilir, bu da kümenin yalnızca TLS 1,2 ve üstünü desteklediği anlamına gelir. Daha fazla bilgi için bkz. [Aktarım Katmanı Güvenliği](./transport-layer-security.md).

### <a name="bring-your-own-key-for-disk-encryption"></a>Disk şifrelemesi için kendi anahtarınızı getirin
HDInsight 'taki tüm yönetilen diskler Azure Depolama Hizmeti Şifrelemesi (SSE) ile korunmaktadır. Bu disklerdeki veriler, varsayılan olarak Microsoft tarafından yönetilen anahtarlarla şifrelenir. Bu sürümden itibaren, disk şifrelemesi için Kendi Anahtarını Getir (BYOK) ve Azure Key Vault kullanarak yönetebilirsiniz. BYOK şifrelemesi, küme oluşturma sırasında ek bir ücret olmadan tek adımlı bir yapılandırmadır. HDInsight 'ı Azure Key Vault ile yönetilen kimlik olarak kaydedin ve kümenizi oluştururken şifreleme anahtarını eklemeniz yeterlidir. Daha fazla bilgi için bkz. [müşteri tarafından yönetilen anahtar disk şifrelemesi](https://docs.microsoft.com/azure/hdinsight/disk-encryption).

## <a name="deprecation"></a>Kullanımdan kaldırma
Bu yayının kullanım dışı bırakılmasıyla ilgili değildir. Yakında kullanım dışı bırakılmaya hazırlanmak için, [yaklaşan değişikliklere](#upcoming-changes)bakın.

## <a name="behavior-changes"></a>Davranış değişiklikleri
Bu sürümde hiçbir davranış değişikliği yok. Yaklaşan değişikliklere hazırlanmak için, [yaklaşan değişikliklere](#upcoming-changes)bakın.

## <a name="upcoming-changes"></a>Yaklaşan değişiklikler
Gelecek sürümlerde aşağıdaki değişiklikler olur. 

### <a name="deprecation-of-spark-21-and-22-in-hdinsight-36-spark-cluster"></a>HDInsight 3,6 Spark kümesinde Spark 2,1 ve 2,2 ' nin kullanımdan kaldırılması
1 Temmuz 2020 ' den itibaren, müşteriler HDInsight 3,6 ' te Spark 2,1 ve 2,2 ile yeni Spark kümeleri oluşturamayacak. Mevcut kümeler, Microsoft desteği olmadan olduğu gibi çalışır. Olası sistem/destek kesintilerini önlemek için HDInsight 3,6 ' ye kadar Spark 2,3 ' ye geçerek 30 Haziran 2020 ' ye geçmeyi düşünün. Daha fazla bilgi için bkz. [Apache Spark 2,1 ve 2,2 iş yüklerini 2,3 ve 2,4 Için geçirme](./spark/migrate-versions.md).

### <a name="deprecation-of-spark-23-in-hdinsight-40-spark-cluster"></a>HDInsight 4,0 Spark kümesinde Spark 2,3 kullanımdan kaldırılması
1 Temmuz 2020 ' den itibaren, müşteriler HDInsight 4,0 üzerinde Spark 2,3 ile yeni Spark kümeleri oluşturamayacak. Mevcut kümeler, Microsoft desteği olmadan olduğu gibi çalışır. Olası sistem/destek kesintilerini önlemek için HDInsight 4,0 ' ye kadar Spark 2,4 ' ye geçerek 30 Haziran 2020 ' ye geçmeyi düşünün. Daha fazla bilgi için bkz. [Apache Spark 2,1 ve 2,2 iş yüklerini 2,3 ve 2,4 Için geçirme](./spark/migrate-versions.md).

### <a name="deprecation-of-kafka-11-in-hdinsight-40-kafka-cluster"></a>HDInsight 4,0 Kafka kümesi 'nde Kafka 1,1 kullanımdan kaldırılması
1 2020 Temmuz 'da, müşteriler HDInsight 4,0 üzerinde Kafka 1,1 ile yeni Kafka kümeleri oluşturamayacak. Mevcut kümeler, Microsoft desteği olmadan olduğu gibi çalışır. Olası sistem/destek kesintilerini önlemek için HDInsight 30 2020 4,0 ' de Kafka 2,1 ' ye geçmeyi göz önünde bulundurun. Daha fazla bilgi için bkz. [Apache Kafka iş yüklerini Azure HDInsight 'A geçirme 4,0](./kafka/migrate-versions.md).

### <a name="hbase-20-to-216"></a>HBase 2,0 2.1.6
Yaklaşan HDInsight 4,0 sürümünde, HBase sürümü 2,0 sürümünden 2.1.6 sürümüne yükseltilir.

### <a name="spark-240-to-244"></a>Spark 2.4.0 to 2.4.4
Yakında düzenlenecek HDInsight 4,0 sürümünde Spark sürümü 2.4.0 sürümünden 2.4.4 sürümüne yükseltilir.

### <a name="kafka-210-to-211"></a>Kafka 2.1.0 to 2.1.1
Yakında düzenlenecek HDInsight 4,0 sürümünde Kafka sürümü 2.1.0 sürümünden 2.1.1 sürümüne Yükseltilecek

### <a name="a-minimum-4-core-vm-is-required-for-head-node"></a>Baş düğüm için en az 4 çekirdekli bir VM gereklidir 
Baş düğüm için en az 4 çekirdekli bir VM, HDInsight kümelerinin yüksek kullanılabilirliğini ve güvenilirliğini güvence altına almak için gereklidir. Müşteriler, 6 2020 Nisan 'dan başlayarak yeni HDInsight kümeleri için yalnızca 4 çekirdekli veya VM 'nin üzerinde baş düğüm olarak seçim yapabilir. Mevcut kümeler beklenen şekilde çalışmaya devam edecektir. 

### <a name="esp-spark-cluster-node-size-change"></a>ESP Spark kümesi düğüm boyutu değişikliği 
Yaklaşan sürümde, ESP Spark kümesi için izin verilen en düşük düğüm boyutu Standard_D13_V2 olarak değiştirilecektir. A serisi VM 'Ler görece düşük CPU ve bellek kapasitesi nedeniyle ESP kümesi sorunlarına neden olabilir. Yeni ESP kümeleri oluşturmak için A serisi VM 'Ler kullanım dışı olacaktır.

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümelerine geçme
HDInsight artık kümeyi sağlamak için Azure sanal makinelerini kullanır. Yaklaşan sürümde, HDInsight bunun yerine Azure sanal makine ölçek kümelerini kullanacaktır. Azure sanal makine ölçek kümeleri hakkında daha fazla bilgi için bkz..

## <a name="bug-fixes"></a>Hata düzeltmeleri
HDInsight, küme güvenilirliği ve performans iyileştirmeleri yapmaya devam eder. 

## <a name="component-version-change"></a>Bileşen sürümü değişikliği
Bu yayın için bileşen sürümü değişikliği yok. HDInsight 4,0 ad HDInsight 3,6 için geçerli bileşen sürümlerini burada bulabilirsiniz.

