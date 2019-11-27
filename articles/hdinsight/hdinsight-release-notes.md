---
title: Azure HDInsight için sürüm notları
description: Azure HDInsight için en son sürüm notları. Hadoop, Spark, R Server, Hive ve daha fazlası için geliştirme ipuçları ve ayrıntıları alın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 08/08/2019
ms.openlocfilehash: 54e28a9d434500915aa8cc8e07ade3592e5aa96c
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74185612"
---
# <a name="release-notes"></a>Sürüm notları

Bu makalede **en son** Azure HDInsight sürüm güncelleştirmeleri hakkında bilgi sağlanır. Önceki sürümler hakkında daha fazla bilgi için bkz. [HDInsight sürüm notları Arşivi](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Özet

Azure HDInsight, Azure üzerinde açık kaynaklı Apache Hadoop ve Apache Spark analizlere yönelik kurumsal müşteriler arasındaki en popüler hizmetlerden biridir.

## <a name="release-date-11072019"></a>Yayın tarihi: 11/07/2019

Bu sürüm yalnızca HDInsight 3,6 ve 4,0 için geçerlidir.

> [!IMPORTANT]  
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [HDInsight sürüm oluşturma makalesi](hdinsight-component-versioning.md).


## <a name="new-features"></a>Yeni özellikler

### <a name="hdinsight-identity-broker-hib-preview"></a>HDInsight Identity broker (HIB) (Önizleme)

HDInsight Identity broker (HIB), kullanıcıların Multi-Factor Authentication (MFA) kullanarak Apache ambarı 'nda oturum açmasını ve Azure Active Directory Domain Services (AAD-DS) içinde parola karmaları gerekmeden gerekli Kerberos biletlerini almasını sağlar. Şu anda HIB yalnızca ARM şablonu aracılığıyla dağıtılan kümeler için kullanılabilir.

### <a name="kafka-rest-api-proxy-preview"></a>Kafka REST API proxy (Önizleme)

Kafka REST API proxy, güvenli AAD yetkilendirmesi ve OAuth protokolü aracılığıyla Kafka kümesi ile yüksek oranda kullanılabilir REST proxy 'nin tek tıklamayla dağıtımını sağlar. 

### <a name="auto-scale"></a>Otomatik ölçeklendirme

Azure HDInsight için otomatik ölçeklendirme artık Apache Spark ve Hadoop kümesi türleri için tüm bölgelerde genel kullanıma sunulmuştur. Bu özellik, büyük veri analizi iş yüklerinin daha uygun maliyetli ve üretken bir şekilde yönetilmesini sağlar. Artık HDInsight kümelerinizin kullanımını iyileştirebilmeniz ve yalnızca ihtiyacınız olan şeyler için ödeme yapabilirsiniz.

Gereksinimlerinize bağlı olarak, yük tabanlı ve zamanlama tabanlı otomatik ölçeklendirme arasından seçim yapabilirsiniz. Zamanlama tabanlı otomatik ölçeklendirme, küme boyutunu önceden tanımlanmış bir zamanlamaya göre değiştirebilmesini sağlarken, yük tabanlı otomatik ölçeklendirme, geçerli kaynak gereksinimlerine bağlı olarak küme boyutunu yukarı ve aşağı ölçeklendirebilir. 

HBase ve LLAP iş yükünün otomatik ölçeklendirme desteği de genel önizlemeye sunuldu. Daha fazla bilgi için bkz. [Azure HDInsight kümelerini otomatik olarak ölçeklendirme](https://docs.microsoft.com/azure/hdinsight/hdinsight-autoscale-clusters).

### <a name="hdinsight-accelerated-writes-for-apache-hbase"></a>Apache HBase için HDInsight hızlandırılmış yazma Işlemleri 

Hızlandırılmış Yazmalar özelliği Apache HBase Önceden Yazılan Günlük (WAL) performansını geliştirmek için Azure premium SSD yönetilen disklerini kullanır. Daha fazla bilgi için bkz. [Apache HBase Için Azure HDInsight hızlandırılmış yazma](https://docs.microsoft.com/azure/hdinsight/hbase/apache-hbase-accelerated-writes).

### <a name="custom-ambari-db"></a>Özel Ambari DB

HDInsight artık müşterilerin ambarı için kendi SQL DB 'yi kullanmasını sağlamak için yeni bir kapasite sunmaktadır. Artık müşteriler, ambarı için doğru SQL DB 'yi seçip kendi iş büyüme gereksinimlerine göre kolayca yükseltebilir. Dağıtım bir Azure Resource Manager şablonuyla yapılır. Daha fazla bilgi için bkz. [HDInsight kümelerini özel bir AMBARı DB Ile ayarlama](https://docs.microsoft.com/azure/hdinsight/hdinsight-custom-ambari-db).

### <a name="f-series-virtual-machines-are-now-available-with-hdinsight"></a>F Serisi sanal makineler artık HDInsight ile kullanılabilir

F Serisi sanal makineler (VM 'Ler), hafif işlem gereksinimleriyle HDInsight kullanmaya başlamak için iyi bir seçimdir. Daha düşük bir saatlik liste fiyatına sahip olan F Serisi, vCPU başına Azure İşlem Birimi (ACU) açısından fiyat-performans alanında Azure portföyündeki en iyi seçenektir. Daha fazla bilgi için bkz. [Azure HDInsight kümeniz için doğru VM boyutunu seçme](https://docs.microsoft.com/azure/hdinsight/hdinsight-selecting-vm-size).

## <a name="deprecation"></a>Kullanımdan kaldırma

### <a name="g-series-virtual-machine-deprecation"></a>G Serisi sanal makinenin kullanımdan kaldırılması
Bu sürümden, G serisi VM 'Ler artık HDInsight 'ta sunulmamaktadır.

### <a name="dv1-virtual-machine-deprecation"></a>Dv1 sanal makinesini kullanımdan kaldırma
Bu sürümden, HDInsight ile Dv1 VM 'lerinin kullanımı kullanım dışıdır. Dv1 için herhangi bir müşteri isteği, Dv2 ile otomatik olarak sunulur. Dv1 ve dv2 VM 'Ler arasında bir fiyat farkı yoktur.

## <a name="behavior-changes"></a>Davranış değişiklikleri

### <a name="cluster-managed-disk-size-change"></a>Küme tarafından yönetilen disk boyutu değişikliği
HDInsight, kümeyle yönetilen disk alanı sağlar. Bu sürümden, yeni oluşturulan kümedeki her bir düğümün yönetilen disk boyutu 128 GB olarak değiştirilir.

## <a name="upcoming-changes"></a>Yaklaşan değişiklikler
Yaklaşan sürümlerde aşağıdaki değişiklikler olur. 

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümelerine geçme
HDInsight artık kümeyi sağlamak için Azure sanal makinelerini kullanır. HDInsight 'tan itibaren, HDInsight bunun yerine Azure sanal makine ölçek kümelerini kullanacaktır. [Azure sanal makine ölçek kümeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview)hakkında daha fazla bilgi için bkz.

### <a name="hbase-20-to-21"></a>HBase 2,0-2,1
Yakında düzenlenecek HDInsight 4,0 sürümünde HBase sürümü 2,0 sürümünden 2,1 sürümüne yükseltilir.

### <a name="a-series-virtual-machine-deprecation-for-esp-cluster"></a>A serisi sanal makine, ESP kümesi için kullanımdan kaldırıldı
A serisi VM 'Ler görece düşük CPU ve bellek kapasitesi nedeniyle ESP küme sorunlarına neden olabilir. Gelecek sürümde, yeni ESP kümeleri oluşturmak için A serisi VM 'Ler kullanım dışı bırakılır.

## <a name="bug-fixes"></a>Hata düzeltmeleri
HDInsight, küme güvenilirliği ve performans iyileştirmeleri yapmaya devam eder. 

## <a name="component-version-change"></a>Bileşen sürümü değişikliği
Bu yayın için bileşen sürümü değişikliği yok. HDInsight 4,0 ad HDInsight 3,6 için geçerli bileşen sürümlerini [burada](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning)bulabilirsiniz.
