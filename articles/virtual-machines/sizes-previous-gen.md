---
title: Azure VM boyutları-önceki nesiller | Microsoft Docs
description: Azure 'daki sanal makineler için kullanılabilir olan önceki boyut türlerini listeler. Bu serideki boyutlarda vCPU sayısı, veri diskleri ve NIC 'lerin yanı sıra depolama aktarım hızı ve ağ bant genişliği hakkındaki bilgileri listeler.
services: virtual-machines
ms.subservice: sizes
author: mimckitt
ms.topic: article
ms.workload: infrastructure-services
ms.date: 02/20/2020
ms.author: jushiman
ms.openlocfilehash: 6ebd13317940d131facfb36987fb37358260ec05
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85119231"
---
# <a name="previous-generations-of-virtual-machine-sizes"></a>Önceki nesil sanal makine boyutları

Bu bölümde, önceki nesil sanal makine boyutları hakkında bilgi sağlanır. Bu boyutlar hala kullanılabilir, ancak yeni nesiller mevcuttur.

## <a name="f-series"></a>F Serisi

F Serisi, Intel Turbo Boost Technology 2.0 ile 3,1 GHz'e varan saat hızlarına ulaşabilen 2,4 GHz Intel Xeon® E5-2673 v3 (Haswell) işlemciyi temel alır. Bu değer Dv2 Serisi VM'lerdeki CPU performansıyla aynıdır.  

F Serisi VM'ler, daha hızlı CPU'ya ihtiyaç duyan ancak çok fazla belleğe veya vCPU başına geçici depolamaya ihtiyaç duymayan iş yükleri için harika bir seçenektir.  Analitikler, oyun sunucuları, web sunucuları ve toplu işleme gibi iş yükleri, F Serisinin özelliklerinden en fazla yarar sağlayacak iş yükleridir.

ACU: 210 - 250

Premium Depolama: desteklenmiyor

Premium depolama önbelleği: desteklenmiyor

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | En fazla geçici depolama aktarım hızı: ıOPS/okuma MBps/yazma MBps | Maksimum veri diski/aktarım hızı: ıOPS | En fazla NIC/beklenen ağ bant genişliği (Mbps) |
|---|---|---|---|---|---|---|
| Standard_F1  | 1  | 2  | 16  | 3000/46/23    | 4/4x500   | 2/750   |
| Standard_F2  | 2  | 4  | 32  | 6000/93/46    | 8/8x500   | 2/1500  |
| Standard_F4  | 4  | 8  | 64  | 12000/187/93  | 16/16x500 | 4/3000  |
| Standard_F8  | 8  | 16 | 128 | 24000/375/187 | 32/32x500 | 8/6000  |
| Standard_F16 | 16 | 32 | 256 | 48000/750/375 | 64/64x500 | 8/12000 |

## <a name="fs-series-sup1sup"></a>FS serisi <sup>1</sup>

Fs serisi, Premium depolamaya ek olarak F serisinin tüm avantajlarını sağlar.

ACU: 210 - 250

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: ıOPS/MBps (GiB 'de önbellek boyutu) | Önbelleğe alınmamış maksimum disk aktarım hızı: ıOPS/MBps | En fazla NIC/beklenen ağ bant genişliği (Mbps) |
|---|---|---|---|---|---|---|---|
| Standard_F1s  | 1  | 2  | 4  | 4  | 4000/32 (12)    | 3200/48   | 2/750   |
| Standard_F2s  | 2  | 4  | 8  | 8  | 8000/64 (24)    | 6400/96   | 2/1500  |
| Standard_F4s  | 4  | 8  | 16 | 16 | 16000/128 (48)  | 12800/192 | 4/3000  |
| Standard_F8s  | 8  | 16 | 32 | 32 | 32000/256 (96)  | 25600/384 | 8/6000  |
| Standard_F16s | 16 | 32 | 64 | 64 | 64000/512 (192) | 51200/768 | 8/12000 |

MB/sn = 10^6 bayt/saniye ve GiB = 1024^3 bayt.

<sup>1</sup> bir FS Serisi VM ile olası maksimum disk aktarım hızı (IOPS veya MB/sn), eklenen disklerin sayısı, boyutu ve şeritle sınırlı olabilir.  Ayrıntılar için bkz. [Windows](windows/premium-storage-performance.md) veya [Linux](linux/premium-storage-performance.md)için yüksek performans için tasarlama.  


## <a name="nvv2-series"></a>NVv2 serisi

**Daha yeni boyut önerisi**: [NVv3-Series](nvv3-series.md)

NVv2 serisi sanal makineler, [NVIDIA Tesla M60](https://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU 'LARı ve NVIDIA GRID Teknolojisi Ile Intel geniş CPU ile desteklenir. Bu sanal makineler, müşterilerin verilerini görselleştirmek, görüntülemek için sonuçların benzetimini yapmak, CAD üzerinde çalışmak veya içerik oluşturmak ve akışa almak istedikleri GPU hızlandırmalı grafik uygulamalarına ve sanal masaüstlerine yöneliktir. Ayrıca, bu sanal makineler kodlama ve işleme gibi tek duyarlıklı iş yüklerini çalıştırabilir. NVv2 sanal makineleri, Premium depolamayı destekler ve öncül NV serisi ile karşılaştırıldığında, sistem belleği (RAM) ile birlikte gelir.  

NVv2 örneklerinde her GPU bir KıLAVUZ lisansıyla gelir. Bu lisans, bir NV örneğini tek bir kullanıcı için sanal iş istasyonu olarak kullanma esnekliği sağlar veya 25 eşzamanlı kullanıcı sanal uygulama senaryosu için VM 'ye bağlanabilir.

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU belleği: GiB | Maksimum veri diskleri | En fazla NIC | Sanal Iş Istasyonları | Sanal Uygulamalar |
|---|---|---|---|---|---|---|---|---|---|
| Standard_NV6s_v2  | 6  | 112 | 320  | 1 | 8  | 12 | 4 | 1 | 25  |
| Standard_NV12s_v2 | 12 | 224 | 640  | 2 | 16 | 24 | 8 | 2 | 50  |
| Standard_NV24s_v2 | 24 | 448 | 1280 | 4 | 32 | 32 | 8 | 4 | 100 |

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="older-generations-of-virtual-machine-sizes"></a>Daha eski nesil sanal makine boyutları

Bu bölüm, daha eski nesil sanal makine boyutları hakkında bilgi sağlar. Bu boyutlar hala desteklenir, ancak ek kapasite almaz. Genel olarak kullanılabilen daha yeni veya alternatif boyutlar vardır. Gereksinimlerinize en uygun VM boyutlarını seçmek için lütfen [Azure 'Da Linux sanal makineleri Için boyutlara](linux/sizes.md) bakın.  

Linux VM 'yi yeniden boyutlandırma hakkında daha fazla bilgi için bkz. [Linux VM 'Yi yeniden boyutlandırma](linux/change-vm-size.md).  

<br>

### <a name="basic-a"></a>Temel A  

**Daha yeni boyut önerisi**: [AV2-Series](av2-series.md)

Premium Depolama: desteklenmiyor

Premium depolama önbelleği: desteklenmiyor

Temel katman boyutları genelde geliştirme iş yükleri ve yük dengeleme, otomatik ölçeklendirme veya bellek kullanımı yoğun sanal makineler gerektirmeyen diğer uygulamalar içindir.

| Boyut – Boyut\Ad | Sanal işlemci | Bellek|NIC'ler (Maks.)| En fazla geçici disk boyutu | En çok, veri diskleri (1023 GB)| En çok, IOPS (disk başına 300) |
|---|---|---|---|---|---|---|
| A0\Basic_A0 | 1 | 768 MB  | 2 | 20 GB  | 1  | 1x300  |
| A1\Basic_A1 | 1 | 1,75 GB | 2 | 40 GB  | 2  | 2x300  |
| A2\Basic_A2 | 2 | 3,5 GB  | 2 | 60 GB  | 4  | 4x300  |
| A3\Basic_A3 | 4 | 7 GB    | 2 | 120 GB | 8  | 8x300  |
| A4\Basic_A4 | 8 | 14 GB   | 2 | 240 GB | 16 | 16x300 |

<br>

### <a name="standard-a0---a4-using-cli-and-powershell"></a>Standard A0 - A4, CLI ve PowerShell kullanarak

Klasik dağıtım modelinde bazı VM boyutu adları CLI ve PowerShell'dekilerden biraz farklıdır:

* Standard_A0 = Çok Küçük
* Standard_A1 = Küçük
* Standard_A2 = Orta
* Standard_A3 = Büyük
* Standard_A4 = Çok Büyük

### <a name="a-series"></a>A Serisi  

**Daha yeni boyut önerisi**: [AV2-Series](av2-series.md)

ACU: 50-100

Premium Depolama: desteklenmiyor

Premium depolama önbelleği: desteklenmiyor

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (HDD): GiB | Maksimum veri diskleri | Maksimum veri diski aktarım hızı: IOPS | En fazla NIC/beklenen ağ bant genişliği (Mbps) |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A0 &nbsp; <sup>1</sup> | 1 | 0,768 | 20 | 1 | 1x500 | 2/100 |
| Standard_A1 | 1 | 1,75 | 70  | 2  | 2x500  | 2/500  |
| Standard_A2 | 2 | 3,5  | 135 | 4  | 4x500  | 2/500  |
| Standard_A3 | 4 | 7    | 285 | 8  | 8x500  | 2/1000 |
| Standard_A4 | 8 | 14   | 605 | 16 | 16x500 | 4/2000 |
| Standard_A5 | 2 | 14   | 135 | 4  | 4x500  | 2/500  |
| Standard_A6 | 4 | 28   | 285 | 8  | 8x500  | 2/1000 |
| Standard_A7 | 8 | 56   | 605 | 16 | 16x500 | 4/2000 |

<sup>1</sup> a0 boyutu, fiziksel donanımda abone olunmuş. Yalnızca bu boyutta diğer müşteri dağıtımları, çalışan iş yükünüzün performansını etkileyebilir. Göreli performans aşağıda beklenen temel düzey olarak belirtilmiştir ve %15 oranında değişiklik gösterebilir.

<br>

### <a name="a-series---compute-intensive-instances"></a>A Serisi - Yoğun işlem gücü kullanımlı örnekler  

**Daha yeni boyut önerisi**: [AV2-Series](av2-series.md)

ACU: 225

Premium Depolama: desteklenmiyor

Premium depolama önbelleği: desteklenmiyor

A8-A11 ve H Serisi boyutlar *yoğun işlem gücü kullanımlı örnekler* olarak da bilinir. Bu boyutları çalıştıran donanım; yüksek performanslı bilgi işlem (HPC) kümesi uygulamaları, modellemeler ve simülasyonlar gibi yoğun işlem ve ağ kullanımlı uygulamalar için tasarlanmış ve iyileştirilmiştir. A8-A11 Serisinde, Intel Xeon E5-2670 @ 2,6 GHZ, H Serisinde ise Intel Xeon E5-2667 v3 @ 3,2 GHz işlemciler kullanılmaktadır.  

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (HDD): GiB | Maksimum veri diskleri | Maksimum veri diski aktarım hızı: IOPS | En fazla NIC|
|---|---|---|---|---|---|---|
| Standard_A8 &nbsp; <sup>1</sup> | 8  | 56  | 382 | 32 | 32x500 | 2 |
| Standard_A9 &nbsp; <sup>1</sup> | 16 | 112 | 382 | 64 | 64x500 | 4 |
| Standard_A10 | 8  | 56  | 382 | 32 | 32x500 | 2 |
| Standard_A11 | 16 | 112 | 382 | 64 | 64x500 | 4 |

<sup>1</sup> MPı uygulamaları için, adanmış RDMA arka uç ağı, son derece düşük gecikme süresi ve yüksek bant genişliği sunan FDR InfiniBand ağı tarafından etkinleştirilir.  

> [!NOTE]
> A8 – A11 VM 'Leri 3/2021 tarihinde kullanımdan kaldırma için planlanmaktadır. Daha fazla bilgi için bkz. [HPC geçiş kılavuzu](https://azure.microsoft.com/resources/hpc-migration-guide/).

<br>

### <a name="d-series"></a>D Serisi  

**Daha yeni boyut önerisi**: [Dav4-Series](dav4-dasv4-series.md), [dv4-Series](dv4-dsv4-series.md) ve [Ddv4-Series](ddv4-ddsv4-series.md)

ACU: 160-250 <sup>1</sup>

Premium Depolama: desteklenmiyor

Premium depolama önbelleği: desteklenmiyor

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | En fazla geçici depolama aktarım hızı: ıOPS/okuma MBps/yazma MBps | Maksimum veri diski/aktarım hızı: ıOPS | En fazla NIC/beklenen ağ bant genişliği (Mbps) |
|---|---|---|---|---|---|---|
| Standard_D1  | 1 | 3,5 | 50  | 3000/46/23    | 4/4x500   | 2/500  |
| Standard_D2  | 2 | 7   | 100 | 6000/93/46    | 8/8x500   | 2/1000 |
| Standard_D3  | 4 | 14  | 200 | 12000/187/93  | 16/16x500 | 4/2000 |
| Standard_D4  | 8 | 28  | 400 | 24000/375/187 | 32/32x500 | 8/4000 |

<sup>1</sup> VM AILESI şu CPU 'dan birinde çalışabilir: 2,2 GHz ıntel Xeon® E5-2660 v2, 2,4 GHz ıntel Xeon® E5-2673 v3 (Haswell) veya 2,3 GHz ıntel Xeon® E5-2673 v4 (çok iyi)  

<br>

### <a name="d-series---memory-optimized"></a>D serisi-bellek için iyileştirilmiş  

**Daha yeni boyut önerisi**: [Dav4-Series](dav4-dasv4-series.md), [dv4-Series](dv4-dsv4-series.md) ve [Ddv4-Series](ddv4-ddsv4-series.md)

ACU: 160-250 <sup>1</sup>

Premium Depolama: desteklenmiyor

Premium depolama önbelleği: desteklenmiyor

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | En fazla geçici depolama aktarım hızı: ıOPS/okuma MBps/yazma MBps | Maksimum veri diski/aktarım hızı: ıOPS | En fazla NIC/beklenen ağ bant genişliği (Mbps) |
|---|---|---|---|---|---|---|
| Standard_D11 | 2  | 14  | 100 | 6000/93/46    | 8/8x500   | 2/1000 |
| Standard_D12 | 4  | 28  | 200 | 12000/187/93  | 16/16x500 | 4/2000 |
| Standard_D13 | 8  | 56  | 400 | 24000/375/187 | 32/32x500 | 8/4000 |
| Standard_D14 | 16 | 112 | 800 | 48000/750/375 | 64/64x500 | 8/8000 |

<sup>1</sup> VM AILESI şu CPU 'dan birinde çalışabilir: 2,2 GHz ıntel Xeon® E5-2660 v2, 2,4 GHz ıntel Xeon® E5-2673 v3 (Haswell) veya 2,3 GHz ıntel Xeon® E5-2673 v4 (çok iyi)  

<br>

## <a name="preview-dc-series"></a>Önizleme: DC Serisi

**Daha yeni boyut önerisi**: [DCsv2-Series](dcv2-series.md)

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

DC Serisi, en son nesil 3.7 GHz Intel XEON E-2176G Işlemcisini SGX teknolojisini kullanır ve Intel Turbo Boost teknolojisi, 4.7 GHz 'ye kadar olabilir. 

| Boyut          | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | En fazla NIC/beklenen ağ bant genişliği (Mbps) |
|---------------|------|-------------|------------------------|----------------|-------------------------------------------------------------------------|-------------------------------------------|----------------------------------------------|
| Standard_DC2s | 2    | 8           | 100                    | 2              | 4000/32 (43)                                                          | 3200/48                                  | 2 / 1500                                     |
| Standard_DC4s | 4    | 16          | 200                    | 4              | 8000/64 (86)                                                          | 6400/96                                  | 2 / 3000                                     |

> [!IMPORTANT]
>
> DC Serisi VM 'Ler [2. nesil sanal makineler](./linux/generation-2.md#creating-a-generation-2-vm) ve yalnızca `Gen2` görüntüleri destekler.


### <a name="ds-series"></a>DS serisi  

**Daha yeni boyut önerisi**: [Dasv4-Series](dav4-dasv4-series.md), [Dsv4-Series](dv4-dsv4-series.md) ve [Ddsv4-Series](ddv4-ddsv4-series.md)

ACU: 160-250 <sup>1</sup>

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: ıOPS/MBps (GiB 'de önbellek boyutu) | Önbelleğe alınmamış maksimum disk aktarım hızı: ıOPS/MBps | En fazla NIC/beklenen ağ bant genişliği (Mbps) |
|---|---|---|---|---|---|---|---|
| Standard_DS1 | 1 | 3,5 | 7  | 4  | 4000/32 (43)    | 3200/32   | 2/500  |
| Standard_DS2 | 2 | 7   | 14 | 8  | 8000/64 (86)    | 6400/64   | 2/1000 |
| Standard_DS3 | 4 | 14  | 28 | 16 | 16000/128 (172) | 12800/128 | 4/2000 |
| Standard_DS4 | 8 | 28  | 56 | 32 | 32000/256 (344) | 25600/256 | 8/4000 |

<sup>1</sup> VM AILESI şu CPU 'dan birinde çalışabilir: 2,2 GHz ıntel Xeon® E5-2660 v2, 2,4 GHz ıntel Xeon® E5-2673 v3 (Haswell) veya 2,3 GHz ıntel Xeon® E5-2673 v4 (çok iyi)  

<br>

### <a name="ds-series---memory-optimized"></a>DS serisi bellek için iyileştirilmiş  

**Daha yeni boyut önerisi**: [Dasv4-Series](dav4-dasv4-series.md), [Dsv4-Series](dv4-dsv4-series.md) ve [Ddsv4-Series](ddv4-ddsv4-series.md)

ACU: 160-250 <sup>1, 2</sup>

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: ıOPS/MBps (GiB 'de önbellek boyutu) | Önbelleğe alınmamış maksimum disk aktarım hızı: ıOPS/MBps | En fazla NIC/beklenen ağ bant genişliği (Mbps) |
|---|---|---|---|---|---|---|---|
| Standard_DS11 | 2  | 14  | 28  | 8  | 8000/64 (72)    | 6400/64   | 2/1000 |
| Standard_DS12 | 4  | 28  | 56  | 16 | 16000/128 (144) | 12800/128 | 4/2000 |
| Standard_DS13 | 8  | 56  | 112 | 32 | 32000/256 (288) | 25600/256 | 8/4000 |
| Standard_DS14 | 16 | 112 | 224 | 64 | 64000/512 (576) | 51200/512 | 8/8000 |

<sup>1</sup> DS serisi VM ile olası maksimum disk aktarım hızı (IOPS veya MB/sn), eklenen disklerin sayısı, boyutu ve şeritle sınırlı olabilir.  Ayrıntılar için bkz. [Windows](windows/premium-storage-performance.md) veya [Linux](linux/premium-storage-performance.md)için yüksek performans için tasarlama.
<sup>2</sup> VM AILESI şu CPU 'dan birinde çalışabilir: 2,2 GHz ıntel Xeon® E5-2660 v2, 2,4 GHz ıntel Xeon® E5-2673 v3 (Haswell) veya 2,3 GHz ıntel Xeon® E5-2673 v4 (geniş bir)  

<br>

### <a name="ls-series"></a>Ls serisi

**Daha yeni boyut önerisi**: [Lsv2-Series](lsv2-series.md)

Ls serisi, [Intel® Xeon İşlemci E5 v3 ailesi](https://www.intel.com/content/www/us/en/processors/xeon/xeon-e5-solutions.html) ile 32’ye kadar vCPU kullanım olanağı sunar. Ls serisi, G/GS serisi ile aynı CPU performansı sunar ve her vCPU başına 8 GiB bellek içerir.

Ls serisi, dayanıklı veri diskleri tarafından ulaşılabilir ıOPS 'yi artırmak için yerel bir önbellek oluşturulmasını desteklemez. Yerel diskin yüksek işleme ve ıOPS değeri, tek bir VM 'nin arızası durumunda Kalıcılık sağlamak için verileri birden çok VM arasında çoğaltan Apache Cassandra ve MongoDB gibi NoSQL depoları için ideal olan LS Serisi VM 'Leri sağlar.

ACU: 180-240

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenmiyor

| Boyut | Sanal işlemci | Bellek (GiB) | Geçici depolama (GiB) | Maksimum veri diskleri | En fazla geçici depolama aktarım hızı (ıOPS/MBps) | Önbelleğe alınmamış maksimum disk aktarım hızı (ıOPS/MBps) | En fazla NIC/beklenen ağ bant genişliği (Mbps) |
|---|---|---|---|---|---|---|---|
| Standart_L4s   | 4  | 32  | 678  | 16 | 20000/200 | 5000/125  | 2/4000  |
| Standart_L8s   | 8  | 64  | 1388 | 32 | 40000/400 | 10000/250 | 4/8000  |
| Standart_L16s  | 16 | 128 | 2807 | 64 | 80000/800 | 20000/500 | 8/16000 |
| Standard_L32s &nbsp; <sup>1</sup> | 32 | 256 | 5630 | 64 | 160000/1600 | 40000/1000 | 8/20000 |

LS Serisi VM 'lerle mümkün olan en fazla disk aktarım hızı, ekli disklerin sayısı, boyutu ve şeritleriyle sınırlı olabilir. Ayrıntılar için bkz. [Windows](windows/premium-storage-performance.md) veya [Linux](linux/premium-storage-performance.md)için yüksek performans için tasarlama.

<sup>1</sup> örnek, tek bir müşteriye adanmış donanımlar için yalıtılmıştır.

### <a name="gs-series"></a>GS serisi

**Daha yeni boyut önerisi**: [Easv4-Series](eav4-easv4-series.md), [Esv4-serisi](ev4-esv4-series.md), [Edsv4-serisi](edv4-edsv4-series.md) ve [d serisi](m-series.md)

ACU: 180-240 <sup>1</sup>

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Önbelleğe alınmamış maksimum disk aktarım hızı: ıOPS/MBps | En fazla NIC/beklenen ağ bant genişliği (Mbps) |
|---|---|---|---|---|---|---|---|
| Standard_GS1 | 2 | 28  | 56  | 8  | 10000/100 (264)  | 5000/ 125  | 2/2000 |
| Standard_GS2 | 4 | 56  | 112 | 16 | 20000/200 (528)  | 10000/ 250 | 2/4000 |
| Standard_GS3 | 8 | 112 | 224 | 32 | 40000/400 (1056) | 20000/ 500 | 4/8000 |
| Standard_GS4 &nbsp; <sup>3</sup> | 16 | 224 | 448 | 64 | 80000/800 (2112) | 40000/1000 | 8/16000 |
| Standard_GS5 &nbsp; <sup>2, &nbsp; 3</sup> | 32 | 448 |896 | 64 |160000/1600 (4224) | 80000/2000 | 8/20000 |

<sup>1</sup> GS serisi VM ile mümkün olan maksimum disk aktarım hızı (IOPS veya MB/sn), eklenen disklerin sayısı, boyutu ve şeritle sınırlı olabilir. Ayrıntılar için bkz. [Windows](windows/premium-storage-performance.md) veya [Linux](linux/premium-storage-performance.md)için yüksek performans için tasarlama.

<sup>2</sup> örnek, tek bir müşteriye adanmış donanımlar için yalıtılmıştır.

<sup>3</sup> kısıtlı çekirdek boyutu var.

<br>

### <a name="g-series"></a>G Serisi

**Daha yeni boyut önerisi**: [Eav4-Series](eav4-easv4-series.md), [Ev4-serisi](ev4-esv4-series.md) ve [Edv4-](edv4-edsv4-series.md) serisi ve [d serisi](m-series.md)

ACU: 180 - 240

Premium Depolama: desteklenmiyor

Premium depolama önbelleği: desteklenmiyor

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | En fazla geçici depolama aktarım hızı: ıOPS/okuma MBps/yazma MBps | Maksimum veri diski/aktarım hızı: ıOPS | En fazla NIC/beklenen ağ bant genişliği (Mbps) |
|---|---|---|---|---|---|---|
| Standard_G1  | 2  | 28  | 384  | 6000/93/46    | 8/8x500   | 2/2000  |
| Standard_G2  | 4  | 56  | 768  | 12000/187/93  | 16/16x500 | 2/4000  |
| Standard_G3  | 8  | 112 | 1536 | 24000/375/187 | 32/32x500 | 4/8000  |
| Standard_G4  | 16 | 224 | 3072 | 48000/750/375 | 64/64x500 | 8/16000 |
| Standard_G5 &nbsp; <sup>1</sup> | 32 | 448 | 6144 | 96000/1500/750| 64/64x500 | 8/20000 |

<sup>1</sup> örnek, tek bir müşteriye adanmış donanımlar için yalıtılmıştır.
<br>

## <a name="nv-series"></a>NV serisi
**Daha yeni boyut önerisi**: [NVv3-Series](nvv3-series.md) ve [NVv4-Series](nvv4-series.md)

NV serisi sanal makineler, müşterilerin verilerini veya simülasyonlarını görselleştirerek, masaüstü hızlandırılmış uygulamalar ve sanal masaüstleri için [NVIDIA Tesla M60](https://images.nvidia.com/content/tesla/pdf/188417-Tesla-M60-DS-A4-fnl-Web.pdf) GPU 'LARı ve NVIDIA GRID Teknolojisi tarafından desteklenir. Kullanıcılar, üstün grafik yeteneği elde etmek ve kodlama ve işleme gibi tek duyarlıklı iş yüklerini çalıştırmak için, NV örneklerinde grafik yoğun iş akışlarını görselleştiriyor. NV serisi VM 'Ler, Intel Xeon E5-2690 v3 (Haswell) CPU 'Ları tarafından da desteklenir.

NV örneklerindeki her GPU bir KıLAVUZ lisansıyla birlikte gelir. Bu lisans, bir NV örneğini tek bir kullanıcı için sanal iş istasyonu olarak kullanma esnekliği sağlar veya 25 eşzamanlı kullanıcı sanal uygulama senaryosu için VM 'ye bağlanabilir.

Premium Depolama: desteklenmiyor

Premium depolama önbelleği: desteklenmiyor

Dinamik Geçiş: desteklenmiyor

Güncelleştirmeleri koruyan bellek: desteklenmiyor

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | GPU | GPU belleği: GiB | Maksimum veri diskleri | En fazla NIC | Sanal Iş Istasyonları | Sanal Uygulamalar |
|---|---|---|---|---|---|---|---|---|---|
| Standard_NV6  | 6  | 56  | 340  | 1 | 8  | 24 | 1 | 1 | 25  |
| Standard_NV12 | 12 | 112 | 680  | 2 | 16 | 48 | 2 | 2 | 50  |
| Standard_NV24 | 24 | 224 | 1440 | 4 | 32 | 64 | 4 | 4 | 100 |

1 GPU = M60 kartın yarısı.
<br>

## <a name="other-sizes"></a>Diğer boyutlar

* [Genel amaçlı](sizes-general.md)
* [İşlem için iyileştirilmiş](sizes-compute.md)
* [Bellek için iyileştirilmiş](sizes-memory.md)
* [Depolama için iyileştirilmiş](sizes-storage.md)
* [GPU](sizes-gpu.md)
* [Yüksek performanslı işlem](sizes-hpc.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.
