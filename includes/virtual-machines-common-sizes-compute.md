---
title: include dosyası
description: include dosyası
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 11/04/2019
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 21b982389b186e949b21352f4b11bd6b4aa06dcb
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74279188"
---
<!-- F-series, Fs-series* -->

İşlem için iyileştirilmiş VM boyutları yüksek CPU-bellek oranına sahiptir. Bu boyutlar orta trafikli web sunucuları, ağ gereçleri, toplu süreçler ve uygulama sunucuları için uygundur. Bu makale, vCPU sayısı, veri diskleri ve NIC 'ler hakkında bilgi sağlar. Ayrıca, bu gruplandırmadaki her bir boyut için depolama alanı işleme ve ağ bant genişliği hakkında bilgiler içerir.

Fsv2 serisi, Intel® Xeon® Platinum 8168 işlemcisini temel alır. Sürekli olarak 3,4 GHz 'nin tüm Core Turbo saat hızına ve en yüksek çekirdekli Turbo 3,7 GHz sıklığına sahiptir. Intel® AVX-512 yönergeleri Intel Ölçeklenebilir Işlemcilerde yenidir. Bu yönergeler, tek ve çift duyarlıklı kayan nokta işlemlerinde vektör işleme iş yükleri için bir 2X performans artışı sağlar. Diğer bir deyişle, tüm hesaplama iş yükleri oldukça hızlıdır.

Daha düşük bir saatlik liste fiyatına göre, Fsv2-Series, vCPU başına Azure Işlem birimi (ACU) temelinde Azure portföyündeki fiyat performansı için en iyi değerdir.

## <a name="fsv2-series-sup1sup"></a>Fsv2-serisi <sup>1</sup>

ACU: 195-210

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

| Boyut             | vCPU | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | En fazla NIC/beklenen ağ bant genişliği (Mbps) |
|------------------|--------|-------------|----------------|----------------|--------------------------|--------------------------|-------------------------|
| Standard_F2s_v2  | 2      | 4           | 16             | 4              | 4000/31 (32)           | 3200/47                | 2 / 875                 |
| Standard_F4s_v2  | 4      | 8           | 32             | 8              | 8000/63 (64)           | 6400/95                | 2 / 1750               |
| Standard_F8s_v2  | 8      | 16          | 64             | 16             | 16000/127 (128)        | 12800/190              | 4 / 3500               |
| Standard_F16s_v2 | 16     | 32          | 128            | 32             | 32000/255 (256)        | 25600/380              | 4 / 7000               |
| Standard_F32s_v2 | 32     | 64          | 256            | 32             | 64000/512 (512)        | 51200/750              | 8 / 14000              |
| Standard_F48s_v2 | 48     | 6400/96          | 384            | 32             | 96000/768 (768)        | 76800/1100             | 8 / 21000              |
| Standard_F64s_v2 | 64     | 128         | 512            | 32             | 128000/1024 (1024)     | 80000/1100             | 8 / 28000              |
| Standard_F72s_v2<sup>2,&nbsp;3</sup> | 72 | 144 | 576         | 32             | 144000/1152 (1520)     | 80000/1100             | 8 / 30000              |

<sup>1</sup> Fsv2 serisi VM 'ler özelliği, hiper Iş parçacığı teknolojisinin Intel®.

<sup>2</sup> 64 'Den fazla vCPU kullanımı, desteklenen bu konuk işletim sistemlerinden birini gerektirir: Windows Server 2016, Ubuntu 16,04 LTS, SLES 12 SP2 ve Red Hat Enterprise Linux, centos 7,3 ya da LIS 4.2.1 ile Oracle Linux 7,3.

<sup>3</sup> örnek, tek bir müşteriye adanmış donanımlar için yalıtılmıştır.
