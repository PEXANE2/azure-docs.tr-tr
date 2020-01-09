---
title: Azure HDInsight için sürüm notları
description: Azure HDInsight için en son sürüm notları. Hadoop, Spark, R Server, Hive ve daha fazlası için geliştirme ipuçları ve ayrıntıları alın.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.service: hdinsight
ms.topic: conceptual
ms.date: 12/17/2019
ms.openlocfilehash: 2abdae95e14ecc9dab673216a2c6aef652915988
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75435461"
---
# <a name="release-notes"></a>Sürüm notları

Bu makalede **en son** Azure HDInsight sürüm güncelleştirmeleri hakkında bilgi sağlanır. Önceki sürümler hakkında daha fazla bilgi için bkz. [HDInsight sürüm notları Arşivi](hdinsight-release-notes-archive.md).

## <a name="summary"></a>Özet

Azure HDInsight, Azure üzerinde açık kaynaklı Apache Hadoop ve Apache Spark analizlere yönelik kurumsal müşteriler arasındaki en popüler hizmetlerden biridir.

## <a name="release-date-12172019"></a>Yayın tarihi: 12/17/2019

Bu sürüm yalnızca HDInsight 3,6 ve 4,0 için geçerlidir.

> [!IMPORTANT]  
> Linux, HDInsight sürüm 3.4 ve üzerinde kullanılan tek işletim sistemidir. Daha fazla bilgi için bkz. [HDInsight sürüm oluşturma makalesi](hdinsight-component-versioning.md).


## <a name="new-features"></a>Yeni özellikler

### <a name="service-tags"></a>Hizmet etiketleri
Hizmet etiketleri, Azure hizmetlerine ağ erişimini kolayca kısıtlayabilmesini sağlayarak Azure sanal makineleri ve Azure sanal ağları için güvenliği basitleştirir. Ağ güvenlik grubu (NSG) kurallarınızın hizmet etiketlerini kullanarak belirli bir Azure hizmetine genel olarak veya Azure bölgesine giden trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Azure, her etiketi temel alan IP adreslerinin bakımını sağlar. Ağ güvenlik grupları (NSG 'ler) için HDInsight hizmet etiketleri, sistem durumu ve Yönetim Hizmetleri için IP adresi gruplarıdır. Bu gruplar, güvenlik kuralı oluşturma karmaşıklığına en aza indirmenize yardımcı olur. HDInsight müşterileri, Azure portalı, PowerShell ve REST API aracılığıyla hizmet etiketi etkinleştirebilir. Daha fazla bilgi için bkz. [Azure HDInsight Için ağ güvenlik grubu (NSG) hizmet etiketleri](https://docs.microsoft.com/azure/hdinsight/hdinsight-service-tags).

### <a name="custom-ambari-db"></a>Özel Ambari DB
HDInsight artık Apache ambarı için kendi SQL DB 'nizi kullanmanıza olanak tanır. Bu özel ambarı DB 'yi Azure portal veya Resource Manager şablonu aracılığıyla yapılandırabilirsiniz.  Bu özellik, işleme ve kapasite gereksinimleriniz için doğru SQL DB 'yi seçmenizi sağlar. Ayrıca, iş büyüme gereksinimlerini eşleştirmek için kolayca yükseltebilirsiniz. Daha fazla bilgi için bkz. [HDInsight kümelerini özel bir AMBARı DB Ile ayarlama](hdinsight-custom-ambari-db.md).

![Özel Ambari DB](./media/hdinsight-release-notes/custom-ambari-db.png)

## <a name="deprecation"></a>Kullanımdan kaldırma
Bu yayının kullanım dışı bırakılmasıyla ilgili değildir. Yakında kullanım dışı bırakılmaya hazırlanmak için, [yaklaşan değişikliklere](#upcoming-changes)bakın.

## <a name="behavior-changes"></a>Davranış değişiklikleri
Bu sürümde hiçbir davranış değişikliği yok. Yaklaşan davranış değişikliklerine hazırlanmak için, [yaklaşan değişikliklere](#upcoming-changes)bakın.

## <a name="upcoming-changes"></a>Yaklaşan değişiklikler
Gelecek sürümlerde aşağıdaki değişiklikler olur. 

### <a name="transport-layer-security-tls-12-enforcement"></a>Aktarım Katmanı Güvenliği (TLS) 1,2 zorlaması
Aktarım Katmanı Güvenliği (TLS) ve Güvenli Yuva Katmanı (SSL), bir bilgisayar ağı üzerinden iletişim güvenliği sağlayan şifreleme protokolleridir. Daha fazla bilgi için bkz. [Aktarım Katmanı Güvenliği](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0). Azure HDInsight kümeleri genel HTTPS uç noktalarında TLS 1,2 bağlantılarını kabul ederken, daha eski istemcilerle geriye dönük uyumluluk için TLS 1,1 hala desteklenmektedir.

Sonraki sürümden itibaren, yeni HDInsight kümelerinizi yalnızca TLS 1,2 bağlantılarını kabul edecek şekilde kabul edebilir ve yapılandırabilirsiniz. 

6/30/2020 tarihinden itibaren Azure HDInsight, yılda daha sonra tüm HTTPS bağlantıları için TLS 1,2 veya sonraki sürümleri uygular. Tüm istemcilerinizin TLS 1,2 veya sonraki sürümlerini işlemeye hazırlanmasını öneririz.

### <a name="moving-to-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümelerine geçme
HDInsight artık kümeyi sağlamak için Azure sanal makinelerini kullanır. Feburary 2020 'den başlayarak (tam tarihin daha sonra iletileceği), HDInsight bunun yerine Azure sanal makine ölçek kümelerini kullanacaktır. [Azure sanal makine ölçek kümeleri](https://docs.microsoft.com/azure/virtual-machine-scale-sets/overview)hakkında daha fazla bilgi için bkz.

### <a name="esp-spark-cluster-node-size-change"></a>ESP Spark kümesi düğüm boyutu değişikliği 
Yakında çıkacak sürümde:
- ESP Spark kümesi için izin verilen en düşük düğüm boyutu Standard_D13_V2 olarak değiştirilecek. 
- A serisi VM 'Ler, yeni ESP kümeleri oluşturmak için kullanım dışı olacaktır, çünkü bir serisi VM 'Ler görece düşük CPU ve bellek kapasitesi nedeniyle ESP kümesi sorunlarına neden olabilir.

### <a name="hbase-20-to-21"></a>HBase 2,0-2,1
Yakında düzenlenecek HDInsight 4,0 sürümünde HBase sürümü 2,0 sürümünden 2,1 sürümüne yükseltilir.

## <a name="bug-fixes"></a>Hata düzeltmeleri
HDInsight, küme güvenilirliği ve performans iyileştirmeleri yapmaya devam eder. 

## <a name="component-version-change"></a>Bileşen sürümü değişikliği
HDInsight 3,6 desteğini 31 Aralık 2020 ' ye genişlettik. [Desteklenen HDInsight sürümlerinde](hdinsight-component-versioning.md#supported-hdinsight-versions)daha fazla ayrıntı bulabilirsiniz.

HDInsight 4,0 için bileşen sürümü değişikliği yoktur.

HDInsight 'ta Apache Zeppelin 3,6:0.7.0--> 0.7.3. 

[Bu belge](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning#apache-hadoop-components-available-with-different-hdinsight-versions)içinden en güncel bileşen sürümlerini bulabilirsiniz.

## <a name="new-regions"></a>Yeni bölgeler

### <a name="uae-north"></a>BAE Kuzey
BAE Kuzey yönetim IP 'Leri şunlardır: `65.52.252.96` ve `65.52.252.97`.
