---
title: Kopyalama etkinliği performans ve ölçeklenebilirlik Kılavuzu
description: Kopyalama etkinliğini kullandığınızda Azure Data Factory veri hareketinin performansını etkileyen anahtar faktörleri hakkında bilgi edinin.
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
ms.openlocfilehash: 231b0d77dc441e70dc0ec8de313291bb6b4f9292
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79261404"
---
# <a name="copy-activity-performance-and-scalability-guide"></a>Kopyalama etkinliği performans ve ölçeklenebilirlik Kılavuzu

> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Azure Data Factory sürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-copy-activity-performance.md)
> * [Geçerli sürüm](copy-activity-performance.md)

Data Lake veya kurumsal veri ambarından (EDW) Azure 'a büyük ölçekli veri geçişi gerçekleştirmek isteyip istemediğiniz ya da büyük veri analizi için farklı kaynaklardaki verileri Azure 'a dönüştürmek istiyorsanız, en iyi performansı elde etmek ve sorun.  Azure Data Factory, verileri ölçeklendirerek yüksek performanslı ve ölçeklenebilir veri alma işlem hatları oluşturmaya yönelik veri mühendislerine harika bir uyum sağlayan performans, esnek ve ekonomik bir mekanizma sağlar.

Bu makaleyi okuduktan sonra aşağıdaki soruları yanıtlamak mümkün olacaktır:

- Veri taşıma ve veri alma senaryolarında ADF kopyalama etkinliğini kullanarak ne düzeyde performans ve ölçeklenebilirlik elde edebilirim?

- ADF kopyalama etkinliğinin performansını ayarlamak için hangi adımları gerçekleştirmeniz gerekir?
- Tek bir kopyalama etkinliğinin çalışması için performansı iyileştirmek üzere ADF performans iyileştirmesi her ne kadar kullanabilir?
- Kopyalama performansını en iyi duruma getirirken ADF dışında hangi diğer faktörler göz önüne

> [!NOTE]
> Kopyalama etkinliğini genel olarak bilmiyorsanız, bu makaleyi kullanmadan önce [kopyalama etkinliğine genel bakış](copy-activity-overview.md) bölümüne bakın.

## <a name="copy-performance-and-scalability-achievable-using-adf"></a>ADF kullanarak performans ve ölçeklenebilirlik ulaşılabilir kopyalama

ADF, farklı düzeylerde paralellik sağlayan sunucusuz bir mimari sunar ve bu sayede, ortamınız için veri taşıma aktarım hızını en üst düzeye çıkarmak amacıyla, geliştiricilerin ağ bant genişliğinizi ve depolama ıOPS ve bant genişliğini tamamen kullanmak üzere işlem hatları oluşturmalarına olanak tanır.  Bu, elde ettiğiniz aktarım hızı, kaynak veri deposu, hedef veri deposu ve kaynak ile hedef arasındaki ağ bant genişliği tarafından sunulan minimum aktarım hızını ölçerek tahmin edilebilecek bir anlamına gelir.  Aşağıdaki tabloda, ortamınız için veri boyutuna ve bant genişliği sınırına göre kopyalama süresi hesaplanır. 

| Veri boyutu/ <br/> bant genişliği | 50 Mbps    | 100 Mbps  | 500 Mbps  | 1 Gbps   | 5 Gbps   | 10 Gbps  | 50 Gbps   |
| --------------------------- | ---------- | --------- | --------- | -------- | -------- | -------- | --------- |
| **1 GB**                    | 2,7 dk    | 1,4 dk   | 0,3 dk   | 0,1 dk  | 0,03 dk | 0,01 dk | 0,0 dk   |
| **10 GB**                   | 27,3 dk   | 13,7 dk  | 2,7 dk   | 1,3 dk  | 0,3 dk  | 0,1 dk  | 0,03 dk  |
| **100 GB**                  | 4,6 saat    | 2,3 saat   | 0,5 saat   | 0,2 saat  | 0,05 Saat | 0,02 Saat | 0,0 saat   |
| **1 TB**                    | 46,6 saat   | 23,3 saat  | 4,7 saat   | 2,3 saat  | 0,5 saat  | 0,2 saat  | 0,05 Saat  |
| **10 TB**                   | 19,4 gün  | 9,7 gün  | 1,9 gün  | 0,9 gün | 0,2 gün | 0,1 gün | 0,02 gün |
| **100 TB**                  | 194,2 gün | 97,1 gün | 19,4 gün | 9,7 gün | 1,9 gün | 1 gün    | 0,2 gün  |
| **1 PB**                    | 64,7 Mo    | 32,4 Mo   | 6,5 Mo    | 3,2 Mo   | 0,6 Mo   | 0,3 Mo   | 0,06 Mo   |
| **10 PB**                   | 647,3 Mo   | 323,6 Mo  | 64,7 Mo   | 31,6 Mo  | 6,5 Mo   | 3,2 Mo   | 0,6 Mo    |

ADF kopyası farklı düzeylerde ölçeklenebilir:

![ADF kopyalama nasıl ölçeklendirilir](media/copy-activity-performance/adf-copy-scalability.png)

- ADF denetim akışı, örneğin [her döngü için](control-flow-for-each-activity.md)kullanarak birden çok kopyalama etkinliğini paralel olarak başlatabilir.
- Tek bir kopyalama etkinliği ölçeklenebilir işlem kaynaklarından yararlanabilir: Azure Integration Runtime kullanırken, her bir kopyalama etkinliği için sunucusuz bir şekilde [en fazla 256 DIUs](#data-integration-units) belirtebilirsiniz; Şirket içinde barındırılan Integration Runtime kullanılırken, makineyi el ile ölçeklendirebilir veya birden fazla makineye ([4 düğüme kadar](create-self-hosted-integration-runtime.md#high-availability-and-scalability)) ölçeklendirebilirsiniz ve tek bir kopyalama etkinliği dosya kümesini tüm düğümlerde bölümleyebilir.
- Tek bir kopyalama etkinliği, [paralel olarak](#parallel-copy)birden çok iş parçacığı kullanarak veri deposundan okur ve yazar.

## <a name="performance-tuning-steps"></a>Performans ayarlama adımları

Kopyalama etkinliğiyle Azure Data Factory hizmetinizin performansını ayarlamak için bu adımları uygulayın.

1. **Bir test veri kümesi seçin ve bir taban çizgisi oluşturun.** Geliştirme aşamasında, bir temsili veri örneğine göre kopyalama etkinliğini kullanarak işlem hattınızı test edin. Seçtiğiniz veri kümesi, normal veri desenlerinizi (klasör yapısı, dosya deseni, veri şeması vb.) temsil etmelidir ve kopyalama performansını değerlendirmek için yeterince büyük olur, örneğin kopyalama etkinliğinin tamamlanabilmesi için 10 dakika veya daha fazla süre sürer. [Kopyalama etkinliği izlemeyi](copy-activity-monitoring.md)izleyen yürütme ayrıntılarını ve performans özelliklerini toplayın.

2. **Tek bir kopyalama etkinliğinin performansını en üst düzeye çıkarma**:

   İle başlamak için, ilk olarak tek bir kopyalama etkinliği kullanarak performansı en üst düzeye çıkarmanızı öneririz.

   - **Kopyalama etkinliği bir Azure Integration Runtime üzerinde yürütülürse:** [veri tümleştirme birimleri (Diu)](#data-integration-units) ve [paralel kopya](#parallel-copy) ayarları için varsayılan değerlerle başlayın. 

   - **Kopyalama etkinliği şirket içinde barındırılan bir Integration Runtime yürütülürse:** Integration Runtime 'ı barındırmak için veri deposunu barındıran sunucudan ayrı bir adanmış makine kullanmanızı öneririz. [Paralel kopyalama](#parallel-copy) ayarı için varsayılan değerlerle başlayın ve kendınden konak IR için tek bir düğüm kullanın.  

   Bir performans testi çalıştırması gerçekleştirin ve performans ve paralel kopyalar gibi kullanılan gerçek değerleri de göz önünde bulunun. Bkz. çalışma sonuçlarını ve performans ayarlarını toplama hakkında [Etkinlik izlemeyi kopyalama](copy-activity-monitoring.md) ve performans sorunlarını belirlemek ve çözmek için [etkinlik performansını kopyalama sorunlarını giderme](copy-activity-performance-troubleshooting.md) hakkında bilgi edinin. 

   Sorun giderme ve ayarlama kılavuzlarından sonra ek performans testi çalıştırmaları yürütmek için yineleme yapın. Tek bir kopyalama etkinliği çalıştırması daha iyi işleme sağlayamayabilir, aynı anda 3. adıma başvuran birden çok kopya çalıştırarak toplam aktarım hızını en üst düzeye çıkarmayı düşünün.


3. **Birden çok kopyayı eşzamanlı olarak çalıştırarak toplam aktarım hızını en iyi duruma getirme:**

   Tek bir kopyalama etkinliğinin performansını zaten kapladığınıza göre, ortamınız-ağ, kaynak veri deposu ve hedef veri deposu Için üretilen iş üst limitlerine henüz ulaşmadıysanız, [her döngü için](control-flow-for-each-activity.md)gibi ADF denetim akış yapılarını kullanarak birden çok kopyalama etkinliğini paralel olarak çalıştırabilirsiniz. Bkz. [birden çok kapsayıcıdan dosya kopyalama](solution-template-copy-files-multiple-containers.md), [Amazon S3 ' dan ADLS 2.](solution-template-migration-s3-azure.md)veya [toplu kopyalama ile genel örnek olarak bir denetim tablosu](solution-template-bulk-copy-with-control-table.md) çözüm şablonlarıyla veri geçirme.

5. **Yapılandırmayı tüm veri kümeniz için genişletin.** Yürütme sonuçları ve performansından memnun olduğunuzda, tüm veri kümenizi kapsayacak şekilde tanımı ve işlem hattını genişletebilirsiniz.

## <a name="troubleshoot-copy-activity-performance"></a>Kopyalama etkinliği performansını sorun giderme

Senaryolarınız için performans testi planlamak ve yürütmek için [performans ayarlama adımlarını](#performance-tuning-steps) izleyin. Ve [kopyalama etkinliği performansıyla Ilgili sorun giderme](copy-activity-performance-troubleshooting.md)Azure Data Factory içindeki her bir kopyalama etkinliği çalışmasının performans sorununu nasıl giderebileceğinizi öğrenin.

## <a name="copy-performance-optimization-features"></a>Performansı en iyi duruma getirme özelliklerini Kopyala

Azure Data Factory aşağıdaki performans iyileştirme özelliklerini sağlar:

- [Veri Tümleştirme Birimleri](#data-integration-units)
- [Şirket içinde barındırılan tümleştirme çalışma zamanı ölçeklenebilirliği](#self-hosted-integration-runtime-scalability)
- [Paralel kopya](#parallel-copy)
- [Hazırlanmış kopya](#staged-copy)

### <a name="data-integration-units"></a>Veri tümleştirme birimleri

Veri tümleştirme birimi, Azure Data Factory içinde tek bir birimin gücünü (CPU, bellek ve ağ kaynak ayırma birleşimi) temsil eden bir ölçüdür. Veri tümleştirme birimi yalnızca [Azure tümleştirme çalışma zamanı](concepts-integration-runtime.md#azure-integration-runtime)için geçerlidir, ancak [Şirket içinde barındırılan tümleştirme çalışma zamanı](concepts-integration-runtime.md#self-hosted-integration-runtime)için geçerli değildir. [Daha fazla bilgi edinin](copy-activity-performance-features.md#data-integration-units).

### <a name="self-hosted-integration-runtime-scalability"></a>Şirket içinde barındırılan tümleştirme çalışma zamanı ölçeklenebilirliği

Artan eşzamanlı iş yükünü barındırmak ya da daha yüksek performans elde etmek için, şirket içinde barındırılan Integration Runtime ölçeklendirebilir veya ölçeklendirebilirsiniz. [Daha fazla bilgi edinin](copy-activity-performance-features.md#self-hosted-integration-runtime-scalability).

### <a name="parallel-copy"></a>Paralel kopya

Kopyalama etkinliğinin kullanmasını istediğiniz paralellik belirtmek için paralel kopya ayarlayabilirsiniz. Bu özelliği, kopyalama etkinliğindeki en fazla iş parçacığı sayısı olarak, kaynağınızdan okunan veya kanal veri depolarınız paralel olarak yazabilirsiniz. [Daha fazla bilgi edinin](copy-activity-performance-features.md#parallel-copy).

### <a name="staged-copy"></a>Hazırlanmış kopya

Bir kaynak veri deposundan bir havuz veri deposuna veri kopyalama, geçici bir hazırlama deposu Blob depolamayı kullanmak seçebilirsiniz. [Daha fazla bilgi edinin](copy-activity-performance-features.md#staged-copy).

## <a name="next-steps"></a>Sonraki adımlar
Diğer kopyalama etkinliği makalelerine bakın:

- [Kopyalama etkinliğine genel bakış](copy-activity-overview.md)
- [Kopyalama etkinliği performansını sorun giderme](copy-activity-performance-troubleshooting.md)
- [Etkinlik performansını en iyi duruma getirme özelliklerini Kopyala](copy-activity-performance-features.md)
- [Data Lake veya veri ambarınızdan verileri Azure 'a geçirmek için Azure Data Factory kullanın](data-migration-guidance-overview.md)
- [Amazon S3 'ten Azure Storage 'a veri geçirme](data-migration-guidance-s3-azure-storage.md)
