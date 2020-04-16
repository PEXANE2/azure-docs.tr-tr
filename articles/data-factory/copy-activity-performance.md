---
title: Etkinlik performansı ve ölçeklenebilirlik kılavuzunu kopyalama
description: Kopyalama etkinliğini kullandığınızda Azure Veri Fabrikası'ndaki veri hareketinin performansını etkileyen önemli etkenler hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/11/2020
ms.openlocfilehash: aedb3df69821d1436b03b2eb1f12873b624d426e
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414167"
---
# <a name="copy-activity-performance-and-scalability-guide"></a>Etkinlik performansı ve ölçeklenebilirlik kılavuzunu kopyalama

> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Azure Veri Fabrikası sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-copy-activity-performance.md)
> * [Geçerli sürüm](copy-activity-performance.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

İster veri gölünden ister kurumsal veri ambarından (EDW) Azure'a büyük ölçekli bir veri geçişi gerçekleştirmek, ister büyük veri analitiği için farklı kaynaklardan Azure'a ölçekte veri almak istiyorsanız, en iyi performans ve ölçeklenebilirlik elde etmek çok önemlidir.  Azure Veri Fabrikası, verileri ölçekte almak için performanslı, esnek ve uygun maliyetli bir mekanizma sağlayarak, yüksek performanslı ve ölçeklenebilir veri alma boru hatları oluşturmak isteyen veri mühendisleri için mükemmel bir uyum sağlar.

Bu makaleyi okuduktan sonra, aşağıdaki soruları cevaplamak mümkün olacak:

- Veri geçişi ve veri alma senaryoları için ADF kopyalama etkinliğini kullanarak ne düzeyde performans ve ölçeklenebilirlik elde edebilirim?

- ADF kopyalama etkinliğinin performansını ayarlamak için hangi adımları atmalıyım?
- Tek bir kopyalama etkinliği çalışması için performansı optimize etmek için hangi ADF perf optimizasyon uynikleme tonlarını kullanabilirim?
- Kopya performansını optimize ederken ADF dışındaki diğer faktörler göz önünde bulundurulması gereken ler nelerdir?

> [!NOTE]
> Genel olarak kopyalama etkinliğini bilmiyorsanız, bu makaleyi okumadan önce [kopyalama etkinliğine genel bakışa](copy-activity-overview.md) bakın.

## <a name="copy-performance-and-scalability-achievable-using-adf"></a>ADF kullanarak performansı ve ölçeklenebilirliği kopyala

ADF, geliştiricilerin ortamınız için veri hareketi veri veri miniliğini en üst düzeye çıkarmak için ağ bant genişliğinizi, depolama IOPS'nizi ve bant genişliğinizi tam olarak kullanmalarını sağlayan farklı düzeylerde paralellik sağlayan sunucusuz bir mimari sunar.  Bu, kaynak veri deposu, hedef veri deposu ve kaynak ve hedef arasındaki ağ bant genişliği tarafından sunulan minimum iş verisini ölçerek elde edebileceğiniz iş kaynağının tahmin edilebildiği anlamına gelir.  Aşağıdaki tablo, veri boyutunu ve ortamınız için bant genişliği sınırını temel alan kopyalama süresini hesaplar. 

| Veri boyutu / <br/> Bant genişliği | 50 Mbps    | 100 Mbps  | 500 Mbps  | 1 Gbps   | 5 Gbps   | 10 Gbps  | 50 Gbps   |
| --------------------------- | ---------- | --------- | --------- | -------- | -------- | -------- | --------- |
| **1 GB**                    | 2,7 dk    | 1,4 dk   | 0,3 dk   | 0,1 dk  | 0,03 dk | 0,01 dk | 0,0 dk   |
| **10 GB**                   | 27,3 dk   | 13,7 dk  | 2,7 dk   | 1,3 dk  | 0,3 dk  | 0,1 dk  | 0,03 dk  |
| **100 GB**                  | 4.6 saat    | 2.3 saat   | 0,5 saat   | 0,2 saat  | 0.05 saat | 0.02 saat | 0.0 saat   |
| **1 TB**                    | 46.6 saat   | 23.3 saat  | 4.7 saat   | 2.3 saat  | 0,5 saat  | 0,2 saat  | 0.05 saat  |
| **10 TB**                   | 19.4 gün  | 9.7 gün  | 1.9 gün  | 0.9 gün | 0,2 gün | 0.1 gün | 0.02 gün |
| **100 TB**                  | 194.2 gün | 97.1 gün | 19.4 gün | 9.7 gün | 1.9 gün | 1 gün    | 0,2 gün  |
| **1 PB**                    | 64.7 mo    | 32.4 mo   | 6,5 mo    | 3.2 mo   | 0,6 mo   | 0,3 mo   | 0.06 mo   |
| **10 PB**                   | 647.3 mo   | 323,6 mo  | 64.7 mo   | 31.6 mo  | 6,5 mo   | 3.2 mo   | 0,6 mo    |

ADF kopyası farklı düzeylerde ölçeklenebilir:

![ADF kopyalama ölçeklendirme](media/copy-activity-performance/adf-copy-scalability.png)

- ADF denetim akışı, örneğin [Her döngü için](control-flow-for-each-activity.md)kullanarak, paralel olarak birden çok kopyalama etkinlikleri başlatabilirsiniz.
- Tek bir kopyalama etkinliği ölçeklenebilir bilgi işlem kaynaklarından yararlanabilir: Azure Tümleştirme Çalışma Zamanı'nı kullanırken, her kopyalama etkinliği için sunucusuz bir şekilde [256'ya kadar DIUs](#data-integration-units) belirtebilirsiniz; kendi kendine barındırılan Tümleştirme Runtime kullanırken, makineyi el ile ölçeklendirebilir veya birden çok makineye[(en fazla 4 düğüm)](create-self-hosted-integration-runtime.md#high-availability-and-scalability)ölçeklendirebilirsiniz ve tek bir kopyalama etkinliği dosya kümesini tüm düğümler arasında bölümlere ayırır.
- Tek bir kopyalama etkinliği, [paralel olarak](#parallel-copy)birden çok iş parçacığı kullanarak veri deposundan okur ve yazar.

## <a name="performance-tuning-steps"></a>Performans alamı adımları

Azure Veri Fabrikası hizmetinizin performansını kopyalama etkinliğiyle ayarlamak için aşağıdaki adımları izleyin.

1. **Bir test veri kümesi seçin ve bir taban çizgisi kurun.** Geliştirme aşamasında, kopyalama etkinliğini temsili bir veri örneğine karşı kullanarak ardışık hattınızı test edin. Seçtiğiniz veri kümesi, tipik veri desenlerinizi (klasör yapısı, dosya deseni, veri şeması vb.) temsil etmeli ve kopyalama performansını değerlendirecek kadar büyükolmalıdır, örneğin kopyalama etkinliğinin tamamlanması 10 dakika veya ötesini alır. [Kopyalama etkinliği izlemesonrasında](copy-activity-monitoring.md)yürütme ayrıntılarını ve performans özelliklerini toplayın.

2. **Tek bir kopyalama etkinliğinin performansını en üst düzeye çıkarma:**

   Başlangıç olarak, önce tek bir kopyalama etkinliğini kullanarak performansı en üst düzeye çıkarmanızı öneririz.

   - **Kopyalama etkinliği Azure Tümleştirme Çalışma Zamanında yürütülüyorsa:** [Veri Tümleştirme Birimleri (DIU)](#data-integration-units) ve [paralel kopyalama](#parallel-copy) ayarları için varsayılan değerlerle başlayın. 

   - **Kopyalama etkinliği kendi barındırılan Tümleştirme Çalışma Zamanında yürütülüyorsa:** tümleştirme çalışma süresini barındırmak için veri deposunu barındıran sunucudan ayrı özel bir makine kullanmanızı öneririz. [Paralel kopya](#parallel-copy) ayarı için varsayılan değerlerle başlayın ve kendi barındırılan IR için tek bir düğüm kullanarak.  

   Bir performans testi çalışması gerçekleştirin ve elde edilen performansın yanı sıra DIUs ve paralel kopyalar gibi kullanılan gerçek değerleri de not alın. Kullanılan çalıştıran sonuçların ve performans ayarlarının nasıl toplandığına ilişkin [kopyalama etkinliği izleme](copy-activity-monitoring.md) sine bakın ve darboğazdan birini belirlemek ve çözmek için kopya etkinliği performansını nasıl [çözeceğinizi](copy-activity-performance-troubleshooting.md) öğrenin. 

   Sorun giderme ve aetme kılavuzunu takiben ek performans testi çalıştırmaları gerçekleştirmek için yineleyin. Tek kopya etkinliği çalıştırılamıyorsa, 3 adıma atıfta bulunarak aynı anda birden çok kopya çalıştırarak toplam iş elde etmeyi düşünün.


3. **Aynı anda birden çok kopya çalıştırarak toplam iş bünyesini en üst düzeye çıkarma:**

   Artık tek bir kopyalama etkinliğinin performansını en üst düzeye çıkardığınıza göre, henüz ortamınızın üretim üst limitlerini (ağ, kaynak veri deposu ve hedef veri deposu) elde etmediyseniz, Her döngü [için](control-flow-for-each-activity.md)adf denetim akışı yapılarını kullanarak paralel olarak birden çok kopyalama etkinliği çalıştırabilirsiniz. Birden [çok kapsayıcıdan dosyaları kopyalamaya](solution-template-copy-files-multiple-containers.md)bakın, [verileri Amazon S3'ten ADLS Gen2'ye geçirin](solution-template-migration-s3-azure.md)veya genel örnek olarak bir denetim tablosu çözümü şablonları [içeren toplu kopya.](solution-template-bulk-copy-with-control-table.md)

5. **Yapılandırmayı tüm veri kümenize genişletin.** Yürütme sonuçlarından ve performansından memnun olduğunuzda, tüm veri kümenizi kapsayacak şekilde tanımı ve ardışık alanı genişletebilirsiniz.

## <a name="troubleshoot-copy-activity-performance"></a>Sorun giderme kopyalama etkinliği performansı

Senaryonuz için performans testini planlamak ve gerçekleştirmek için [Performans tuning adımlarını](#performance-tuning-steps) izleyin. Ayrıca, Azure Veri Fabrikası'nda çalışan her bir kopyalama etkinliğinin performans sorununu [sorun giderme sorununu, Sorun Giderme etkinliği performansından](copy-activity-performance-troubleshooting.md)öğrenin.

## <a name="copy-performance-optimization-features"></a>Performans optimizasyonu özelliklerini kopyalama

Azure Veri Fabrikası aşağıdaki performans optimizasyonu özelliklerini sağlar:

- [Veri Tümleştirme Birimleri](#data-integration-units)
- [Kendi kendine barındırılan tümleştirme çalışma zamanı ölçeklenebilirliği](#self-hosted-integration-runtime-scalability)
- [Paralel kopya](#parallel-copy)
- [Aşamalı kopya](#staged-copy)

### <a name="data-integration-units"></a>Veri Tümleştirme Birimleri

Veri Tümleştirme Birimi, Azure Veri Fabrikası'ndaki tek bir birimin gücünü (CPU, bellek ve ağ kaynağı ayırmanın birleşimi) temsil eden bir ölçüdür. Veri Tümleştirme Birimi yalnızca [Azure tümleştirme çalışma zamanı](concepts-integration-runtime.md#azure-integration-runtime)için geçerlidir, ancak kendi kendine [barındırılan tümleştirme çalışma zamanı](concepts-integration-runtime.md#self-hosted-integration-runtime)için geçerli değildir. [Daha fazla bilgi edinin](copy-activity-performance-features.md#data-integration-units).

### <a name="self-hosted-integration-runtime-scalability"></a>Kendi kendine barındırılan tümleştirme çalışma zamanı ölçeklenebilirliği

Artan eşzamanlı iş yükünü barındırmak veya daha yüksek performans elde etmek için, Kendi kendine barındırılan Tümleştirme Çalışma Süresini büyütebilir veya ölçeklendirebilirsiniz. [Daha fazla bilgi edinin](copy-activity-performance-features.md#self-hosted-integration-runtime-scalability).

### <a name="parallel-copy"></a>Paralel kopya

Kopyalama etkinliğinin kullanmasını istediğiniz paralelliği belirtmek için paralel kopya ayarlayabilirsiniz. Bu özelliği, kaynağınızdan okunan veya paralel olarak lavabo veri depolarınıza yazan kopyalama etkinliği içindeki maksimum iş parçacığı sayısı olarak düşünebilirsiniz. [Daha fazla bilgi edinin](copy-activity-performance-features.md#parallel-copy).

### <a name="staged-copy"></a>Aşamalı kopya

Verileri bir kaynak veri deposundan lavabo veri deposuna kopyaladiğinizde, Blob depolama alanını geçici bir hazırlama deposu olarak kullanmayı seçebilirsiniz. [Daha fazla bilgi edinin](copy-activity-performance-features.md#staged-copy).

## <a name="next-steps"></a>Sonraki adımlar
Diğer kopyalama etkinliği makalelerini görün:

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Sorun giderme kopyalama etkinliği performansı](copy-activity-performance-troubleshooting.md)
- [Etkinlik performans optimizasyonu özelliklerini kopyalama](copy-activity-performance-features.md)
- [Verilerinizi veri gölünüzden veya veri ambarınızdan Azure'a geçirmek için Azure Veri Fabrikası'nı kullanın](data-migration-guidance-overview.md)
- [Verileri Amazon S3'ten Azure Depolama'ya geçirme](data-migration-guidance-s3-azure-storage.md)
