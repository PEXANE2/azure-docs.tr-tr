---
title: Azure HDInsight için sürüm notları
description: Azure HDInsight için en son sürüm notları. Hadoop, Spark, R Server, Hive ve daha fazlası için geliştirme ipuçları ve ayrıntıları alın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/08/2019
ms.openlocfilehash: 37b49b3fbe91d199b13f548e8aaf72a6a2f0f848
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75895306"
---
# <a name="release-notes"></a>Sürüm notları

Bu makalede **en son** Azure HDInsight sürüm güncelleştirmeleri hakkında bilgi sağlanır. Önceki sürümler hakkında daha fazla bilgi için bkz. [HDInsight sürüm notları Arşivi](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Özet

Azure HDInsight, Azure üzerinde açık kaynaklı analizler için kurumsal müşteriler arasındaki en popüler hizmetlerden biridir.

## <a name="release-date-01092019"></a>Yayın tarihi: 01/09/2019

Bu sürüm yalnızca HDInsight 3,6 ve 4,0 için geçerlidir. HDInsight yayını, birkaç gün boyunca tüm bölgeler için kullanılabilir hale getirilir. Burada Yayımlanma tarihi, ilk bölgenin yayın tarihini gösterir. Değişiklikleri aşağıda görmüyorsanız, lütfen bölgenizde bir kaç gün içinde etkin olmasını bekleyin.

> [!IMPORTANT]  
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [HDInsight sürüm oluşturma makalesi](hdinsight-component-versioning.md).

## <a name="new-features"></a>Yeni özellikler
### <a name="tls-12-enforcement"></a>TLS 1.2 zorlaması
Aktarım Katmanı Güvenliği (TLS) ve Güvenli Yuva Katmanı (SSL), bir bilgisayar ağı üzerinden iletişim güvenliği sağlayan şifreleme protokolleridir. [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0)hakkında daha fazla bilgi edinin. HDInsight, genel HTTPs uç noktalarında TLS 1,2 kullanır, ancak geriye dönük uyumluluk için TLS 1,1 hala desteklenmektedir. 

Müşteriler bu sürümden, TLS 1,2 üzerinden tüm bağlantılar için TLS 1,2 enformence 'yi kabul edebilir. Yeni bir **Minsupportedtlsversion** özelliği, küme oluşturma için Azure Resource Manager şablonu aracılığıyla tanıtılmıştır. Özellik ayarlanmamışsa, küme yine de 1,0, 1,1 ve 1,2 ' i destekler, bu da bugünün davranışıyla aynıdır. Müşteriler bu özelliğin değerini "1,2" olarak ayarlayabilir, bu da kümenin yalnızca TLS 1,2 ve üstünü desteklediği anlamına gelir. 

### <a name="bring-your-own-key-for-disk-encryption"></a>Disk şifrelemesi için kendi anahtarınızı getirin
HDInsight 'taki tüm yönetilen diskler Azure Depolama Hizmeti Şifrelemesi (SSE) ile korunmaktadır. Bu disklerdeki veriler, varsayılan olarak Microsoft tarafından yönetilen anahtarlarla şifrelenir. Bu sürümden itibaren, disk şifrelemesi için Kendi Anahtarını Getir (BYOK) ve Azure Key Vault kullanarak yönetebilirsiniz. BYOK şifrelemesi, küme oluşturma sırasında ek bir ücret olmadan tek adımlı bir yapılandırmadır. HDInsight 'ı Azure Key Vault ile yönetilen kimlik olarak kaydedin ve kümenizi oluştururken şifreleme anahtarını eklemeniz yeterlidir. 

## <a name="deprecation"></a>Kullanımdan kaldırma
Bu yayının kullanım dışı bırakılmasıyla ilgili değildir. Yakında kullanım dışı bırakılmaya hazırlanmak için, [yaklaşan değişikliklere](#upcoming-changes)bakın.

## <a name="behavior-changes"></a>Davranış değişiklikleri
Bu sürümde hiçbir davranış değişikliği yok. Yaklaşan değişikliklere hazırlanmak için, [yaklaşan değişikliklere](#upcoming-changes)bakın.

## <a name="upcoming-changes"></a>Yaklaşan değişiklikler
Gelecek sürümlerde aşağıdaki değişiklikler olur. 

### <a name="a-minimum-4-core-vm-is-required-for-head-node"></a>Baş düğüm için en az 4 çekirdekli bir VM gereklidir 
Baş düğüm için en az 4 çekirdekli bir VM, HDInsight kümelerinin yüksek kullanılabilirliğini ve güvenilirliğini güvence altına almak için gereklidir. Müşteriler, 6 Nisan 2020 ' den başlayarak yeni HDInsight kümeleri için yalnızca 4 çekirdekli veya VM 'nin üzerinde baş düğüm olarak seçim yapabilir. Mevcut kümeler beklenen şekilde çalışmaya devam edecek. 

### <a name="esp-spark-cluster-node-size-change"></a>ESP Spark kümesi düğüm boyutu değişikliği 
Yaklaşan sürümde, ESP Spark kümesi için izin verilen en düşük düğüm boyutu Standard_D13_V2 olarak değiştirilecektir. A serisi VM 'Ler görece düşük CPU ve bellek kapasitesi nedeniyle ESP kümesi sorunlarına neden olabilir. Yeni ESP kümeleri oluşturmak için A serisi VM 'Ler kullanım dışı olacaktır.

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümelerine geçme
HDInsight artık kümeyi sağlamak için Azure sanal makinelerini kullanır. Yaklaşan sürümde, HDInsight bunun yerine Azure sanal makine ölçek kümelerini kullanacaktır. Azure sanal makine ölçek kümeleri hakkında daha fazla bilgi için bkz.

### <a name="hbase-20-to-21"></a>HBase 2,0-2,1
Yakında düzenlenecek HDInsight 4,0 sürümünde HBase sürümü 2,0 sürümünden 2,1 sürümüne yükseltilir.

## <a name="bug-fixes"></a>Hata düzeltmeleri
HDInsight, küme güvenilirliği ve performans iyileştirmeleri yapmaya devam eder. 

## <a name="component-version-change"></a>Bileşen sürümü değişikliği
Bu yayın için bileşen sürümü değişikliği yok. HDInsight 4,0 ad HDInsight 3,6 için geçerli bileşen sürümlerini burada bulabilirsiniz.
