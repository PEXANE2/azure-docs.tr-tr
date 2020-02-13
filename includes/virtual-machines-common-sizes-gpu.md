---
title: include dosyası
description: include dosyası
services: virtual-machines-windows, virtual-machines-linux
author: cynthn
ms.service: multiple
ms.topic: include
ms.date: 06/11/2019
ms.author: cynthn;azcspmt;jonbeck
ms.custom: include file
ms.openlocfilehash: 8f256749c363e2900fe62bda557f7cb4caa72e3e
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77179022"
---
GPU iyileştirilmiş VM boyutları, tek veya birden çok NVıDıA GPU ile kullanılabilen özelleştirilmiş sanal makinelerdir. Bu boyutlar, işlem yoğunluğu, grafik yoğun ve görselleştirme iş yükleri için tasarlanmıştır. Bu makalede GPU 'Lar, vCPU 'Lar, veri diskleri ve NIC 'lerin sayısı ve türleri hakkında bilgi sağlanır. Bu gruplandırmadaki her boyut için depolama verimlilik ve ağ bant genişliği de mevcuttur.

* **NC, NCv2, NCv3** boyutları yoğun işlem yoğunluğu ve ağ kullanımı yoğun uygulamalar ve algoritmalar için iyileştirilmiştir. Bazı örnekler CUDA ve OpenCL tabanlı uygulamalar ve benzetimler, AI ve derin öğrenime sahiptir. NCv3-Series, NVıDıA 'nin Tesla V100 GPU 'SU olan yüksek performanslı bilgi işlem iş yükleri üzerine odaklanır. NC serisi, Intel Xeon E5-2690 v3 2.60 GHz v3 (Haswell) işlemcisini kullanır ve NCv2-serisi ve NCv3 serisi VM 'Ler Intel Xeon E5-2690 v4 (geniş Iyi) işlemcisini kullanır.

* **ND ve NDv2** ND serisi, derinlemesine öğrenime yönelik eğitim ve çıkarım senaryolarına odaklanılmıştır. NVıDıA Tesla P40 GPU ve Intel Xeon E5-2690 v4 (geniş Iyi) işlemcisini kullanır. NDv2-Series, Intel Xeon Platinum 8168 (ufuk Gölü) işlemcisini kullanır.

* **NV ve NVv3** boyutları, OpenGL ve DirectX gibi çerçeveleri kullanarak uzaktan görselleştirme, akış, oyun, kodlama ve VDI senaryoları için iyileştirilmiştir ve tasarlanmıştır.  Bu VM 'Ler NVıDıA Tesla M60 GPU tarafından desteklenir.

* **NVv4** boyutları en iyi duruma GETIRILIR ve VDI ve uzaktan görselleştirme için tasarlanmıştır. Parulu GPU 'Lar ile NVv4, daha küçük GPU kaynakları gerektiren iş yükleri için doğru boyut sunar.  Bu VM 'Ler AMD Radeon Instinct MI25 GPU tarafından desteklenir.


## <a name="nc-series"></a>NC serisi

Premium Depolama: desteklenmiyor

Premium depolama önbelleği: desteklenmiyor

NC serisi VM 'Ler [NVIDIA Tesla K80](https://www.nvidia.com/content/dam/en-zz/Solutions/Data-Center/tesla-product-literature/Tesla-K80-BoardSpec-07317-001-v05.pdf) Card ve Intel Xeon E5-2690 v3 (Haswell) işlemcisi tarafından desteklenmektedir. Kullanıcılar, enerji araştırması uygulamaları, kilitlenme benzetimleri, Ray tarafından izlenen işleme, derin öğrenme ve daha fazlası için CUDA 'yı kullanarak verileri daha hızlı bir şekilde kullanabilir. NC24r yapılandırması, sıkı şekilde bağlanmış paralel bilgi işlem iş yükleri için iyileştirilmiş, düşük gecikme süreli ve yüksek performanslı bir ağ arabirimi sağlar.

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU belleği: GiB | Maksimum veri diskleri | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- | ---- |
| Standard_NC6 |6 |56 | 340 | 1 | 12 | 24 | 1 |
| Standard_NC12 |12 |112 | 680 | 2 | 24 | 48 | 2 |
| Standard_NC24 |24 |224 | 1440 | 4 | 48 | 64 | 4 |
| Standard_NC24r* |24 |224 | 1440 | 4 | 48 | 64 | 4 |

1 GPU = K80 kartın yarısı.

*RDMA özellikli

## <a name="ncv2-series"></a>NCv2 serisi

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

NCv2 serisi VM 'Ler [NVIDIA Tesla P100](https://www.nvidia.com/en-us/data-center/tesla-p100/) GPU 'ları tarafından desteklenir. Bu GPU 'Lar, NC serisi için 2x miktardan fazla işlem performansı sağlayabilir. Müşteriler rezervoır modelleme, DNA sıralaması, protestolu analiz, Monte Carlo simülasyonları ve diğerleri gibi geleneksel HPC iş yükleri için bu güncelleştirilmiş GPU 'ların avantajlarından yararlanabilir. GPU 'Ların yanı sıra, NCv2 serisi VM 'Ler Intel Xeon E5-2690 v4 (geniş) CPU 'Lar tarafından da desteklenir.

NC24rs v2 yapılandırması, sıkı şekilde bağlanmış paralel bilgi işlem iş yükleri için iyileştirilmiş, düşük gecikme süreli ve yüksek performanslı bir ağ arabirimi sağlar.

> [!IMPORTANT]
> Bu boyut ailesi için, aboneliğinizdeki vCPU (çekirdek) kotası başlangıçta her bölgede 0 olarak ayarlanır. Bu aile için [kullanılabilir bir bölgede](https://azure.microsoft.com/regions/services/) [vCPU kotası artışı isteyin](../articles/azure-portal/supportability/resource-manager-core-quotas-request.md) .
>

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD): GiB | GPU | GPU belleği: GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | En fazla NIC |
| --- | --- | --- | --- | --- | --- | ---  | ---| --- |
| Standard_NC6s_v2 | 6 |112 | 736 | 1 | 16 | 12 | 20000/ 200 | 4 |
| Standard_NC12s_v2 | 12 |224 | 1474 | 2 | 32 | 24 | 40000/400 | 8 |
| Standard_NC24s_v2 | 24 |448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 |
| Standard_NC24rs_v2* | 24 |448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 |

1 GPU = bir P100 kart.

*RDMA özellikli

## <a name="ncv3-series"></a>NCv3 serisi

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

NCv3 serisi VM 'Ler [NVIDIA Tesla V100](https://www.nvidia.com/en-us/data-center/tesla-v100/) GPU 'ları tarafından desteklenir. Bu GPU 'Lar 1.5 x 'in NCv2 serisi hesaplama performansını sağlayabilir. Müşteriler rezervoır modelleme, DNA sıralaması, protestolu analiz, Monte Carlo simülasyonları ve diğerleri gibi geleneksel HPC iş yükleri için bu güncelleştirilmiş GPU 'ların avantajlarından yararlanabilir. NC24rs v3 yapılandırması, sıkı şekilde bağlanmış paralel bilgi işlem iş yükleri için iyileştirilmiş, düşük gecikme süreli ve yüksek performanslı bir ağ arabirimi sağlar. GPU 'Ların yanı sıra, NCv3 serisi VM 'Ler Intel Xeon E5-2690 v4 (geniş) CPU 'Lar tarafından da desteklenir.

> [!IMPORTANT]
> Bu boyut ailesi için, aboneliğinizdeki vCPU (çekirdek) kotası başlangıçta her bölgede 0 olarak ayarlanır. Bu aile için [kullanılabilir bir bölgede](https://azure.microsoft.com/regions/services/) [vCPU kotası artışı isteyin](../articles/azure-portal/supportability/resource-manager-core-quotas-request.md) .
>

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD): GiB | GPU | GPU belleği: GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NC6s_v3 | 6 |112 | 736 | 1 | 16 | 12 | 20000/200 | 4 |
| Standard_NC12s_v3 | 12 |224 | 1474 | 2 | 32 | 24 | 40000/400 | 8 |
| Standard_NC24s_v3 | 24 |448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 | 
| Standard_NC24rs_v3* |24 |448 | 2948 | 4 | 64 | 32 | 80000/800 | 8 |

1 GPU = bir V100 kart.

*RDMA özellikli

## <a name="updated-ndv2-series-preview"></a>NDv2-Series güncelleştirildi (Önizleme)

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

InfiniBand: destekleniyor

NDv2 serisi sanal makine, en zorlu GPU hızlandırmalı AI, makine öğrenimi, simülasyon ve HPC iş yüklerinin ihtiyaçları için tasarlanan GPU ailesine yeni bir ektir. 

NDv2, her biri 32 GB GPU belleği bulunan 8 NVıDıA Tesla V100 NVLINK bağlantılı GPU 'Lar tarafından desteklenir. Her NDv2 VM 'nin ayrıca 40 hiper olmayan Intel Xeon Platinum 8168 (ufuk Gölü) çekirdeği ve 672 GiB sistem belleği vardır. 

NDv2 örnekleri, CUDA GPU ile iyileştirilmiş hesaplama kernels ' lerini kullanan HPC ve AI, ML ve analiz araçlarının yanı sıra TensorFlow, Pytorch, Caffe, RAPıDS ve diğer gibi GPU hızlandırmasını destekleyen birçok AI, ML ve analiz aracını sağlar çerçeveleri. 

Kritik olarak, NDv2 her ikisi de hesaplama için (VM başına 8 GPU) ve genişleme (birden çok VM birlikte çalışan birden fazla sanal makine) iş yükleri için oluşturulmuştur. NDv2 serisi artık 100 Gigabit InfiniBand EDR arka uç ağını destekleyerek, AI ve ML için dağıtılmış eğitim dahil paralel senaryolar için yüksek performanslı Kümelemeye olanak tanır. Bu arka uç ağı, NVıDıA 'nin NCCL2 kitaplıkları tarafından çalıştırılan ve GPU 'Ların sorunsuz kümelemesine izin veren tüm önemli InfiniBand protokollerini destekler.

> ND40rs_v2 VM 'de [InfiniBand etkinleştirilirken](https://docs.microsoft.com/azure/virtual-machines/workloads/hpc/enable-infiniband) lütfen 4.7-1.0.0.1 Mellanox ofed sürücüsünü kullanın.

> Daha fazla GPU belleği nedeniyle, yeni ND40rs_v2 VM [2. nesil sanal makinelerin](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2) ve Market görüntülerinin kullanılmasını gerektirir. 

> [NDv2 sanal makine önizlemesine erken erişim istemek için kaydolun.](https://aka.ms/AzureNDrv2Preview)

> Lütfen unutmayın: GPU başına 16 GB bellek içeren ND40s_v2 artık önizleme için kullanılamaz ve güncelleştirilmiş ND40rs_v2 yerini almıştır.
<br>

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD): GiB | GPU | GPU belleği: GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | En yüksek ağ bant genişliği | En fazla NIC |
|---|---|---|---|---|---|---|---|---|---|
| Standard_ND40rs_v2 | 40 | 672 | 2948 | 8 V100 32 GB (NVLink) | 16 | 32 | 80000/800 | 24000 Mbps | 8 |

## <a name="nd-series"></a>ND serisi

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

ND serisi sanal makineler, AI için tasarlanan GPU ailesine ve derin öğrenme iş yüklerine yeni bir ektir. Eğitim ve çıkarım için mükemmel performans sunar. ND örnekleri [NVIDIA Tesla P40](https://images.nvidia.com/content/pdf/tesla/184427-Tesla-P40-Datasheet-NV-Final-Letter-Web.pdf) GPU 'Lar ve Intel Xeon E5-2690 v4 (çok Iyi) CPU 'lar tarafından desteklenir. Bu örnekler Microsoft Cognitive Toolkit, TensorFlow, Caffe ve diğer çerçeveleri kullanan AI iş yükleri için tek duyarlıklı kayan nokta işlemleri için mükemmel bir performans sağlar. ND serisi aynı zamanda çok daha büyük bir GPU bellek boyutu (24 GB) sunarak çok daha büyük sinir net modellerini mümkün hale getirecek şekilde etkinleştirir. NC serisi gibi, ND serisi, RDMA aracılığıyla ikincil düşük gecikmeli, yüksek performanslı bir ağ ve InfiniBand bağlantısı sunan bir yapılandırma sunarak çok sayıda GPU 'yu kapsayan büyük ölçekli eğitim işleri çalıştırabilirsiniz.

> [!IMPORTANT]
> Bu boyut ailesinde, aboneliğinizdeki bölge başına vCPU (çekirdek) kotası başlangıçta 0 olarak ayarlanır. Bu aile için [kullanılabilir bir bölgede](https://azure.microsoft.com/regions/services/) [vCPU kotası artışı isteyin](../articles/azure-portal/supportability/resource-manager-core-quotas-request.md) .
>

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU belleği: GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_ND6s | 6 |112 | 736 | 1 | 24 | 12 | 20000/200 | 4 |
| Standard_ND12s | 12 |224 | 1474 | 2 | 48 | 24 | 40000/400 | 8 | 
| Standard_ND24s | 24 |448 | 2948 | 4 | 96 | 32 | 80000/800 | 8 |
| Standard_ND24rs * | 24 |448 | 2948 | 4 | 96 | 32 | 80000/800 | 8 |

1 GPU = bir P40 kart.

*RDMA özellikli

## <a name="nv-series"></a>NV serisi

Premium Depolama: desteklenmiyor

Premium depolama önbelleği: desteklenmiyor

NV serisi sanal makineler, müşterilerin verilerini veya simülasyonlarını görselleştirerek, masaüstü hızlandırılmış uygulamalar ve sanal masaüstleri için [NVIDIA Tesla M60](https://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU 'LARı ve NVIDIA GRID Teknolojisi tarafından desteklenir. Kullanıcılar, üstün grafik yeteneği elde etmek ve kodlama ve işleme gibi tek duyarlıklı iş yüklerini çalıştırmak için, NV örneklerinde grafik yoğun iş akışlarını görselleştiriyor. NV serisi VM 'Ler, Intel Xeon E5-2690 v3 (Haswell) CPU 'Ları tarafından da desteklenir.

NV örneklerindeki her GPU bir KıLAVUZ lisansıyla birlikte gelir. Bu lisans, bir NV örneğini tek bir kullanıcı için sanal iş istasyonu olarak kullanma esnekliği sağlar veya 25 eşzamanlı kullanıcı sanal uygulama senaryosu için VM 'ye bağlanabilir.

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU belleği: GiB | Maksimum veri diskleri | En fazla NIC | Sanal Iş Istasyonları | Sanal uygulamalar |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV6 |6 |56 |380 | 1 | 8 | 24 | 1 | 1 | 25 |
| Standard_NV12 |12 |112 |680 | 2 | 16 | 48 | 2 | 2 | 50 |
| Standard_NV24 |24 |224 |1440 | 4 | 32 | 64 | 4 | 4 | 100 |

1 GPU = M60 kartın yarısı.

## <a name="nvv3-series--sup1sup"></a>NVv3-serisi <sup>1</sup>

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

NVv3 serisi sanal makineler, [NVIDIA Tesla M60](https://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU 'LARı ve NVIDIA GRID Teknolojisi Ile Intel E5-2690 v4 (çok Iyi) CPU 'lar tarafından desteklenir. Bu sanal makineler, müşterilerin verilerini görselleştirmek, görüntülemek için sonuçların benzetimini yapmak, CAD üzerinde çalışmak veya içerik oluşturmak ve akışa almak istedikleri GPU hızlandırmalı grafik uygulamalarına ve sanal masaüstlerine yöneliktir. Ayrıca, bu sanal makineler kodlama ve işleme gibi tek duyarlıklı iş yüklerini çalıştırabilir. NVv3 sanal makineleri, Premium depolamayı destekler ve öncül NV serisi ile karşılaştırıldığında, sistem belleği (RAM) ile birlikte gelir.  

NVv3 örneklerinde her GPU bir KıLAVUZ lisansıyla gelir. Bu lisans, bir NV örneğini tek bir kullanıcı için sanal iş istasyonu olarak kullanma esnekliği sağlar veya 25 eşzamanlı kullanıcı sanal uygulama senaryosu için VM 'ye bağlanabilir.

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU belleği: GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | En fazla NIC | Sanal Iş Istasyonları | Sanal uygulamalar | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_NV12s_v3 |12 |112 |736 | 1 | 8 | 12 | 20000/200 | 4 | 1 | 25 |
| Standard_NV24s_v3 |24 |224 |1474 | 2 | 16 | 24 | 40000/400 | 8 | 2 | 50 |
| Standard_NV48s_v3 |48 |448 |2948 | 4 | 32 | 32 | 80000/800 | 8 | 4 | 100 |

1 GPU = M60 kartın yarısı.

<sup>1</sup> NVv3 serisi VM 'Ler özelliği Intel Hiper Iş parçacığı teknolojisi

## <a name="nvv4-series-preview--sup1sup"></a>NVv4-serisi (Önizleme) <sup>1</sup>

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

NVv4 serisi sanal makineler, [AMD Radeon INSTINCT MI25](https://www.amd.com/en/products/professional-graphics/instinct-mi25) GPU 'LARı ve AMD epi dili 7V12 (Roma) CPU 'ları tarafından desteklenir. NVv4-Series ile Azure, kısmi GPU 'lara sahip sanal makineler ile tanışın. 16 GiB çerçeve arabelleğiyle tam bir GPU 'ya 2 GiB çerçeve arabelleğinden 1 GB 'den başlayan GPU hızlandırılmış grafik uygulamaları ve sanal masaüstleri için doğru boyutlu sanal makineyi seçin. NVv4 sanal makineler Şu anda yalnızca Windows Konuk işletim sistemini destekliyor.

[Önizleme sırasında bu makinelere kaydolun ve erişim kazanın](https://aka.ms/nvv4signup).
<br>

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU belleği: GiB | Maksimum veri diskleri | En fazla NIC |
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Standard_NV4as_v4 |4 |14 |88 | 1/8 | 2 | 4 | 2 |
| Standard_NV8as_v4 |8 |28 |176 | 1/4 | 4 | 8 | 4 |
| Standard_NV16as_v4 |16 |56 |352 | 1/2 | 8 | 16 | 8 | 
| Standard_NV32as_v4 |32 |112 |704 | 1 | 16 | 32 | 8 | 



<sup>1</sup> NVv4 serisi VM 'ler özelliği AMD eşzamanlı çoklu Iş parçacığı teknolojisi

