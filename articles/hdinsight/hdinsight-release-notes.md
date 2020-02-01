---
title: Azure HDInsight için sürüm notları
description: Azure HDInsight için en son sürüm notları. Hadoop, Spark, R Server, Hive ve daha fazlası için geliştirme ipuçları ve ayrıntıları alın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/29/2020
ms.openlocfilehash: 091ca4d632d89405d85c66e264aff9867979fcd4
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76905239"
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

Bu sürümde, müşteriler yalnızca genel küme uç noktası üzerinden tüm bağlantılar için TLS 1,2 ' i kabul edebilir. Bunu desteklemek için, **Minsupportedtlsversion** yeni özelliği tanıtılmıştır ve küme oluşturma sırasında belirlenebilir. Özellik ayarlanmamışsa, küme yine de bugün davranışıyla aynı olan TLS 1,0, 1,1 ve 1,2 ' i destekler. Müşteriler bu özelliğin değerini "1,2" olarak ayarlayabilir, bu da kümenin yalnızca TLS 1,2 ve üstünü desteklediği anlamına gelir. Daha fazla bilgi için bkz. [sanal ağ taşıma katmanı güvenliği planlaması](https://docs.microsoft.com/azure/hdinsight/hdinsight-plan-virtual-network-deployment#transport-layer-security).

### <a name="bring-your-own-key-for-disk-encryption"></a>Disk şifrelemesi için kendi anahtarınızı getirin
HDInsight 'taki tüm yönetilen diskler Azure Depolama Hizmeti Şifrelemesi (SSE) ile korunmaktadır. Bu disklerdeki veriler, varsayılan olarak Microsoft tarafından yönetilen anahtarlarla şifrelenir. Bu sürümden itibaren, disk şifrelemesi için Kendi Anahtarını Getir (BYOK) ve Azure Key Vault kullanarak yönetebilirsiniz. BYOK şifrelemesi, küme oluşturma sırasında ek bir ücret olmadan tek adımlı bir yapılandırmadır. HDInsight 'ı Azure Key Vault ile yönetilen kimlik olarak kaydedin ve kümenizi oluştururken şifreleme anahtarını eklemeniz yeterlidir. Daha fazla bilgi için bkz. [müşteri tarafından yönetilen anahtar disk şifrelemesi](https://docs.microsoft.com/azure/hdinsight/disk-encryption).

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

## <a name="known-issues"></a>Bilinen sorunlar

29 Ocak 2020 itibariyle, bir Jupyter Not defteri kullanmaya çalışırken bir hata alabileceğiniz etkin bir sorun vardır. Sorunu onarmak için aşağıdaki adımları kullanın. Ayrıca, bu [MSDN gönderisini](https://social.msdn.microsoft.com/Forums/en-us/8c763fb4-79a9-496f-a75c-44a125e934ac/hdinshight-create-not-create-jupyter-notebook?forum=hdinsight) veya güncel bilgiler Için bu [StackOverflow gönderisini](https://stackoverflow.com/questions/59687614/azure-hdinsight-jupyter-notebook-not-working/59831103) veya ek soru sorabilirsiniz. Bu sayfa, sorun düzeltildiğinde güncelleştirilecektir.

**Hatası**

* ValueError: Bu sürüm mevcut olmadığından Not defteri v5 'e dönüştürülemiyor
* Not defteri yüklenirken hata oluştu bu not defteri yüklenirken bilinmeyen bir hata oluştu. Bu sürüm, v4 veya daha önceki bir not defteri biçimlerini yükleyebilir

**Neden** 

Kümedeki _version. Kopyala dosyası, 4.4. x. # # yerine 5. x. x olarak güncelleştirildi veya ambarı yeniden başlatılmalıdır.

**Çözüm**

Yeni bir Jupyter Not defteri oluşturup yukarıda listelenen hatalardan birini alırsanız, sorunu onarmak için aşağıdaki adımları gerçekleştirin.

1. `https://CLUSTERNAME.azurehdinsight.net`' a giderek bir Web tarayıcısında ambarı açın, burada CLUSTERNAME kümenizin adıdır.
1. Ambarı ' nda, sol taraftaki menüden **Jupo**' a ve ardından **hizmet eylemleri**' ne tıklayın, **Durdur**' a tıklayın.
1. Jupyıter hizmetinin çalıştığı küme yayın düğümüne SSH.
1. Sudo modunda, aşağıdaki/usr/bin/Anaconda/lib/python2.7/site-packages/nbformat/_version. Kopyala dosyasını açın.
1. Version_info değerini denetleyin.
1. Version_info değeri şu şekilde ayarlanmışsa: 

    version_info = (5, 0, 3)

    Ardından girişi şu şekilde değiştirin: 
    
    version_info = (4, 4, 0)

    Ve dosyayı kaydedin. 

    Version_info zaten (4, 4, 0) olarak ayarlandıysa, daha sonra yalnızca ambarı yeniden başlatılması gerektiğinden sonraki adımla devam edin, ek değişiklik yapmanız gerekmez.
1. Ambarı 'na geri dönün ve **hizmet eylemlerinde** **Tümünü Yeniden Başlat**' a tıklayın.
