---
title: Azure HDInsight sürüm notları
description: Azure HDInsight için en son sürüm notları. Hadoop, Spark, R Server, Hive ve daha fazlası için geliştirme ipuçları ve ayrıntılar alın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/20/2020
ms.openlocfilehash: cdb31f1a46d7f46b69e9e0ad47a77ba6b32a50a0
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81770826"
---
# <a name="release-notes"></a>Sürüm notları

Bu makalede, **en son** Azure HDInsight sürüm güncelleştirmeleri hakkında bilgi verilmektedir. Önceki sürümler hakkında daha fazla bilgi için [HDInsight Sürüm Notları Arşivi'ne](hdinsight-release-notes-archive.md)bakın.

## <a name="summary"></a>Özet

Azure HDInsight, Azure'da açık kaynak analizi için kurumsal müşteriler arasında en popüler hizmetlerden biridir.

## <a name="release-date-01092020"></a>Yayın tarihi: 01.09.2020

Bu sürüm hem HDInsight 3.6 hem de 4.0 için geçerlidir. HDInsight sürümü birkaç gün içinde tüm bölgeleriçin kullanılabilir hale getirilir. Buradaki çıkış tarihi ilk bölge çıkış tarihini gösterir. Aşağıdaki değişiklikleri göremiyorsanız, lütfen sürümün bölgenizde birkaç gün içinde yayınlanmasını bekleyin.

> [!IMPORTANT]  
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için [HDInsight sürüm makalesine](hdinsight-component-versioning.md)bakın.

## <a name="new-features"></a>Yeni özellikler
### <a name="tls-12-enforcement"></a>TLS 1.2 zorlaması
Aktarım Katmanı Güvenliği (TLS) ve Güvenli Soket katmanı (SSL), bir bilgisayar ağı üzerinden iletişim güvenliği sağlayan şifreleme protokolleridir. [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0)hakkında daha fazla bilgi edinin. HDInsight, ortak HTTS uç noktalarında TLS 1.2 kullanır, ancak TLS 1.1 geriye dönük uyumluluk için hala desteklenir. 

Bu sürümle, müşteriler TLS 1.2'yi yalnızca ortak küme bitiş noktası üzerinden tüm bağlantılar için tercih edebilirler. Bunu desteklemek için, yeni özellik **minSupportedTlsVersion** tanıtıldı ve küme oluşturma sırasında belirtilebilir. Özellik ayarlanmazsa, küme hala TLS 1.0, 1.1 ve 1.2'yi destekler, bu da bugünkü davranışla aynıdır. Müşteriler bu özelliğin değerini "1,2" olarak ayarlayabilir, bu da kümenin yalnızca TLS 1,2 ve üzeri destekler anlamına gelir. Daha fazla bilgi için [Taşıma Katmanı Güvenliği'ne](./transport-layer-security.md)bakın.

### <a name="bring-your-own-key-for-disk-encryption"></a>Disk şifreleme için kendi anahtarınızı getirin
HDInsight'taki tüm yönetilen diskler Azure Depolama Hizmeti Şifrelemesi (SSE) ile korunur. Bu disklerde ki veriler varsayılan olarak Microsoft tarafından yönetilen anahtarlarla şifrelenir. Bu sürümden başlayarak, disk şifreleme için Kendi Anahtarınızı Getirebilirsiniz (BYOK) ve Azure Key Vault'u kullanarak yönetebilirsiniz. BYOK şifreleme, ek bir ücret ödemeden küme oluşturma sırasında tek adımlı bir yapılandırmadır. HDInsight'ı Azure Key Vault ile yönetilen bir kimlik olarak kaydedin ve kümenizi oluştururken şifreleme anahtarını ekleyin. Daha fazla bilgi için [Müşteri tarafından yönetilen anahtar disk şifrelemesi'ne](https://docs.microsoft.com/azure/hdinsight/disk-encryption)bakın.

## <a name="deprecation"></a>Kullanımdan kaldırma
Bu sürüm için amortisman yok. Yaklaşan amortismanlara hazırlanmak [için, yaklaşan değişikliklere](#upcoming-changes)bakın.

## <a name="behavior-changes"></a>Davranış değişiklikleri
Bu sürüm için davranış değişikliği yok. Yaklaşan değişikliklere hazırlanmak [için, yaklaşan değişikliklere](#upcoming-changes)bakın.

## <a name="upcoming-changes"></a>Yaklaşan değişiklikler
Aşağıdaki değişiklikler gelecek sürümlerde gerçekleşecektir. 

### <a name="a-minimum-4-core-vm-is-required-for-head-node"></a>Kafa Düğümü için en az 4 çekirdekli VM gereklidir 
HDInsight kümelerinin yüksek kullanılabilirliğini ve güvenilirliğini sağlamak için Head Node için en az 4 çekirdekli VM gereklidir. 6 Nisan 2020'den itibaren müşteriler yeni HDInsight kümeleri için Kafa Düğümü olarak yalnızca 4 çekirdekli veya vm'nin üzerinde seçebilecekler. Varolan kümeler beklendiği gibi çalışmaya devam eder. 

### <a name="esp-spark-cluster-node-size-change"></a>ESP Kıvılcım küme düğümü boyutu değişikliği 
Gelecek sürümde, ESP Spark kümesi için izin verilen minimum düğüm boyutu Standard_D13_V2 olarak değiştirilir. A serisi VM'ler, nispeten düşük CPU ve bellek kapasitesi nedeniyle ESP küme sorunlarına neden olabilir. A serisi VM'ler yeni ESP kümeleri oluşturmak için amortismana hazırlanır.

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümelerine geçme
HDInsight artık kümeyi sağlamak için Azure sanal makinelerini kullanıyor. Yaklaşan sürümde, HDInsight bunun yerine Azure sanal makine ölçek kümelerini kullanacaktır. Azure sanal makine ölçek kümeleri hakkında daha fazla bilgi edinin.

### <a name="hbase-20-to-21"></a>HBase 2.0 ile 2.1 arasında
Yaklaşan HDInsight 4.0 sürümünde, HBase sürümü sürüm 2.0'dan 2.1'e yükseltilecektir.

## <a name="bug-fixes"></a>Hata düzeltmeleri
HDInsight küme güvenilirliği ve performans iyileştirmeleri yapmaya devam ediyor. 

## <a name="component-version-change"></a>Bileşen sürüm değişikliği
Bu sürüm için bileşen sürümü değişikliği yok. HDInsight 4.0 reklam HDInsight 3.6 için geçerli bileşen sürümlerini burada bulabilirsiniz.

## <a name="known-issues"></a>Bilinen sorunlar

18 Mart 2020 itibarıyla Batı Avrupa veya Kuzey Avrupa'daki bazı Azure HDInsight müşterileri, bu bölgelerde HDInsight kümeleri oluştururken veya ölçeklenirken hata bildirimleri aldı. Bu sorunla ilgili hatalar şunlardır:

- İstek işlenirken dahili sunucu hatası oluştu. Lütfen isteği yeniden deneyin veya desteğe başvurun.
- En az bir kaynak dağıtım işlemi başarısız oldu. Ayrıntılar için lütfen dağıtım işlemlerini listeleyin. Kullanım https://aka.ms/DeployOperations detayları için lütfen bkz.
- Kullanıcı SubscriptionId\<'\>Abonelik Kimliği ' kaynak oluşturmak\<için çekirdek leri kalmadı ' küme adı>'. Gerekli: \<\>X , Kullanılabilir: 0.

Mühendisler bu sorunun farkındadır ve aktif olarak araştırmaktadır.

Ek yardım için bir [destek isteği](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)oluşturun.
