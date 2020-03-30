---
title: Azure HDInsight kümeniz için doğru VM boyutunu seçme
description: HDInsight kümeniz için doğru VM boyutunu nasıl seçeceğimize öğrenin.
keywords: vm boyutları, küme boyutları, küme yapılandırması
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/09/2019
ms.openlocfilehash: a21e8d6c76c93b3084619c09f6a7664a25c1929c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73682199"
---
# <a name="selecting-the-right-vm-size-for-your-azure-hdinsight-cluster"></a>Azure HDInsight kümeniz için doğru VM boyutunu seçme

Bu makalede, HDInsight kümenizdeki çeşitli düğümler için doğru VM boyutunun nasıl seçilen anlatılmaktadır. 

CPU işleme, RAM boyutu ve ağ gecikmesi gibi sanal bir makinenin özelliklerinin iş yüklerinizin işlenmesini nasıl etkileyeceğini anlayarak başlayın. Ardından, uygulamanızı ve farklı VM ailelerinin hangi leri için optimize edilmiş olduğuyla nasıl eşleştiğini düşünün. Kullanmak istediğiniz VM ailesinin dağıtmayı planladığınız küme türüyle uyumlu olduğundan emin olun. Her küme türü için desteklenen ve önerilen tüm VM boyutlarının listesi için [Azure HDInsight desteklenen düğüm yapılandırmalarına](hdinsight-supported-node-configuration.md)bakın. Son olarak, bazı örnek iş yüklerini test etmek ve bu aile içinde hangi SKU'nun sizin için uygun olduğunu kontrol etmek için bir kıyaslama işlemi kullanabilirsiniz.

Depolama türü veya küme boyutu seçme gibi kümenizin diğer yönlerini planlama hakkında daha fazla bilgi [için HDInsight kümeleri için Kapasite planlamasına](hdinsight-capacity-planning.md)bakın.

## <a name="vm-properties-and-big-data-workloads"></a>VM özellikleri ve büyük veri iş yükleri

VM boyutu ve türü CPU işleme gücü, RAM boyutu ve ağ gecikmesi ile belirlenir:

- CPU: VM boyutu çekirdek sayısını belirler. Daha fazla çekirdek, her düğüm elde edebilirsiniz paralel hesaplama derecesi daha büyüktür. Ayrıca, bazı VM türleri daha hızlı çekirdek var.

- RAM: VM boyutu da VM kullanılabilir RAM miktarını dikte. Diskten okuma yerine verileri işlemek için bellekte depolayan iş yükleri için, çalışan düğümlerinizin verilere uyacak kadar belleğe sahip olmasını sağlayın.

- Ağ: Çoğu küme türünde küme tarafından işlenen veriler yerel diskte değil, Veri Gölü Depolama veya Azure Depolama gibi harici bir depolama hizmetindedir. VM düğümü ile depolama hizmeti arasındaki ağ bant genişliğini ve iş buzunu göz önünde bulundurun. VM'nin kullanılabilen ağ bant genişliği genellikle daha büyük boyutlarda artar. Ayrıntılar için [VM boyutlarına genel bakış](https://docs.microsoft.com/azure/virtual-machines/linux/sizes)alabilirsiniz.

## <a name="understanding-vm-optimization"></a>VM optimizasyonu anlama

Azure'daki sanal makine aileleri, farklı kullanım durumlarına uyacak şekilde optimize edilebiyi kullanır. Aşağıdaki tabloda, en popüler kullanım servis lerinden bazılarını ve bunlara uyan VM ailelerini bulabilirsiniz.

| Tür                     | Boyutlar           |    Açıklama       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Giriş seviyesi](../virtual-machines/linux/sizes-general.md)          | A, Av2  | Cpu performansı ve bellek yapılandırmaları en iyi geliştirme ve test gibi giriş düzeyi iş yükleri için uygundur. Ekonomiktirler ve Azure'a başlamak için düşük maliyetli bir seçenek sunarlar. |
| [Genel amaç](../virtual-machines/linux/sizes-general.md)          | D, DSv2, Dv2  | Dengeli CPU-bellek oranı. Test ve geliştirme, küçük-orta büyüklükteki veritabanları ve küçük-orta büyüklükte trafik hacmine sahip web sunucuları için idealdir. |
| [İşlem için iyileştirilmiş](../virtual-machines/linux/sizes-compute.md)        | F           | Yüksek CPU-bellek oranı. Orta trafik web sunucuları, ağ cihazları, toplu işlem ve uygulama sunucuları için idealdir.        |
| [Bellek için iyileştirilmiş](../virtual-machines/linux/sizes-memory.md)         | Esv3, Ev3  | Yüksek bellek-CPU oranı. İlişkisel veritabanı sunucuları, orta veya büyük boyutlu önbellekler ve bellek içi analiz için idealdir.                 |

- HDInsight destekli bölgelerde mevcut VM örneklerinin fiyatlandırması hakkında daha fazla bilgi için [HDInsight Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/hdinsight/)bakın.

## <a name="cost-saving-vm-types-for-light-workloads"></a>Hafif iş yükleri için maliyet tasarrufu Sağlayan VM türleri

Hafif işleme gereksinimleriniz varsa, [F serisi](https://azure.microsoft.com/blog/f-series-vm-size/) HDInsight ile başlamak için iyi bir seçim olabilir. Daha düşük bir saatlik liste fiyatına sahip olan F Serisi, vCPU başına Azure İşlem Birimi (ACU) açısından fiyat-performans alanında Azure portföyündeki en iyi seçenektir.

Aşağıdaki tabloda, Fsv2 serisi VM'lerle oluşturulabilen küme türleri ve düğüm türleri açıklanmaktadır.

| Küme Türü | Sürüm | İşçi Düğümü | Kafa Düğümü | Zookeeper Düğümü |
|---|---|---|---|---|
| Spark | Tümü | F4 ve üzeri | hayır | hayır |
| Hadoop | Tümü | F4 ve üzeri | hayır | hayır |
| Kafka | Tümü | F4 ve üzeri | hayır | hayır |
| HBase | Tümü | F4 ve üzeri | hayır | hayır |
| LLAP | devre dışı | hayır | hayır | hayır |
| Storm | devre dışı | hayır | hayır | hayır |
| ML Servisi | SADECE HDI 3.6 | F4 ve üzeri | hayır | hayır |

Her F serisi SKU'nun teknik özelliklerini görmek için [F serisi VM boyutlarına](https://azure.microsoft.com/blog/f-series-vm-size/)bakın.

## <a name="benchmarking"></a>Kıyaslama

Kıyaslama, üretim iş yükleri için ne kadar iyi performans göstereceğini ölçmek için farklı VM'lerde simüle edilmiş iş yüklerini çalıştırma işlemidir. 

VM SK'ler ve küme boyutları için kıyaslama hakkında daha fazla bilgi için Azure [HDInsight'ta Küme kapasite planlamasına ](hdinsight-capacity-planning.md#choose-the-vm-size-and-type)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure HDInsight desteklenen düğüm yapılandırmaları](hdinsight-supported-node-configuration.md)
- [Azure'daki Linux sanal makinesi boyutları](../virtual-machines/linux/sizes.md)
