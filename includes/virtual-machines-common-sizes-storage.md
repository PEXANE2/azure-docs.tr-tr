---
title: include dosyası
description: include dosyası
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 04/17/2019
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 765ee3c737adbe1da89b9e908d0e22e44d0f29ba
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76748905"
---
Depolama için iyileştirilmiş VM boyutları, yüksek disk verimlilik ve GÇ sağlar ve büyük veri, SQL, NoSQL veritabanları, veri depolama ve büyük işlem veritabanları için idealdir.  Cassandra, MongoDB, Cloudera ve Redo örnekleri sayılabilir. Bu makalede, her iyileştirilmiş boyut için sanal CPU 'lar, veri diskleri ve NIC 'lerin yanı sıra yerel depolama alanı işleme ve ağ bant genişliği hakkında bilgi sağlanır.

Lsv2-Series, yüksek performans, düşük gecikme süresi ve [AMD EPIC &trade; 7551 işlemcisi](https://www.amd.com/en/products/epyc-7000-series) üzerinde çalışan, tüm çekirdek artışı 2.55 GHz ve en fazla 3,0 GHz olan yerel NVMe depolama özelliklerine sahiptir. Lsv2 serisi VM 'Ler, eşzamanlı bir çoklu iş parçacığı yapılandırmasında 8 ile 80 vCPU boyutunda olacak şekilde gelir.  Her vCPU için 8 GiB bellek ve 8 vCPU başına, L80s v2 'de bulunan 19.2 TB 'a kadar (10 x 1.92 TB) bir 1.92 TB NVMe SSD M. 2 cihaz vardır.

> [!NOTE]
> Lsv2 serisi VM 'Ler, dayanıklı veri disklerini kullanmak yerine doğrudan VM 'ye eklenen düğümdeki yerel diski kullanacak şekilde iyileştirilmiştir. Bu, iş yükleriniz için daha fazla IOPS/aktarım hızı sağlar. Lsv2 ve LS Serisi, dayanıklı veri disklerine yönelik IOPS ulaşılabilir arttırmak için yerel bir önbellek oluşturulmasını desteklemez.
>
> Yerel diskin yüksek aktarım hızı ve IOPS, Lsv2 ve LS Serisi VM 'Leri Apache Cassandra ve MongoDB gibi NoSQL depolarında ideal hale getirir ve tek bir VM 'nin arızası durumunda Kalıcılık elde etmek için verileri birden çok VM arasında çoğaltır.
>
> Daha fazla bilgi için bkz. [Lsv2 serisi sanal makinelerde performansı iyileştirme](../articles/virtual-machines/linux/storage-performance.md).  


## <a name="lsv2-series"></a>Lsv2 serisi

ACU: 150-175

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenmiyor

| Boyut          | Sanal işlemci | Bellek (GiB) | Geçici disk<sup>1</sup> (gib) | NVMe diskleri<sup>2</sup> | NVMe disk işleme<sup>3</sup> (Okuma IOPS/Mbps) | Önbelleğe alınmamış maksimum veri diski verimlilik (IOPS/MBps)<sup>4</sup> | En fazla veri diski | En fazla NIC/beklenen ağ bant genişliği (Mbps) |
|---------------|-----------|-------------|--------------------------|----------------|---------------------------------------------------|-------------------------------------------|------------------------------|------------------------------| 
| Standard_L8s_v2   |  8 |  64 |  80 |  1x 1.92 TB  | 400000/2000  | 8000/160   | 16 | 2 / 3200  |
| Standard_L16s_v2  | 16 | 128 | 160 |  2x 1.92 TB  | 800000/4000  | 16000/320  | 32 | 4 / 6400  |
| Standard_L32s_v2  | 32 | 256 | 320 |  4X 1.92 TB  | 1,5 milyon/8000    | 32000/640  | 32 | 8 / 12800 |
| Standard_L48s_v2  | 48 | 384 | 480 |  6x 1.92 TB  | 2.2 a/14000   | 48000/960  | 32 | 8/16000 + |
| Standard_L64s_v2  | 64 | 512 | 640 |  8X 1.92 TB  | 2.9 d/16000   | 64000/1280 | 32 | 8/16000 + |
| Standard_L80s_v2<sup>5</sup> | 80 | 640 | 800 | 10 x 1.92 TB   | 3.8 d/20000   | 80000/1400 | 32 | 8/16000 + |

<sup>1</sup> Lsv2 serisi VM 'ler, işletim sistemi disk belleği/takas dosyası kullanımı için standart bir SCSI tabanlı geçici kaynak diskine sahiptir (D: Windows 'Ta, Linux üzerinde/dev/sdb). Bu disk, her 8 vCPU için 80 GiB depolama, 4.000 ıOPS ve 80 MBps aktarım hızı sağlar (örneğin, Standard_L80s_v2 40.000 ıOPS ve 800 MBPS ' te 800 GiB sağlar). Bu, NVMe sürücülerinin uygulama kullanımına tamamen ayrılmasını sağlar. Bu disk geçici bir işlemdir ve tüm veriler durdurma/serbest bırakma durumunda kaybedilir.

<sup>2</sup> yerel NVMe diski kısa ömürlü, VM 'nizi durdurur/serbest bırakırsanız bu disklerde veri kaybolacaktır.

<sup>3</sup> Hyper-V NVMe Direct teknolojisi, Konuk VM alanı ile güvenli bir şekilde eşlenmiş yerel NVMe sürücülerine kısıtlı erişim sağlar.  En yüksek performansı elde etmek için Azure Marketi 'nden en son WS2019 Build veya Ubuntu 18,04 ya da 16,04 kullanılması gerekir.  Yazma performansı, GÇ boyutu, sürücü yükü ve kapasite kullanımına göre farklılık gösterir.

<sup>4</sup> Lsv2 serisi VM 'Ler, Lsv2 iş yüklerini avantajına sahip olmadığından veri diski için konak önbelleği sunmaz.  Ancak, Lsv2 VM 'Leri Azure 'un kısa ömürlü VM OS disk seçeneğine (en fazla 30 GiB) uyum sağlayabilir.

64 ' den fazla vCPU içeren <sup>5</sup> VM, desteklenen bu konuk işletim sistemlerinden birini gerektirir:
- Windows Server 2016 veya üzeri
- Ubuntu 16,04 LTS veya üzeri, Azure ayarlanmış çekirdekle (4,15 çekirdek veya üzeri)
- SLES 12 SP2 veya üzeri
- RHEL veya CentOS Sürüm 6,7 ile 6,10, Microsoft tarafından sunulan LIS paketi 4.3.1 (veya üzeri) yüklü
- RHEL veya CentOS Sürüm 7,3, Microsoft tarafından sunulan LIS paketi 4.2.1 (veya üzeri) yüklü
- RHEL veya CentOS Sürüm 7,6 veya üzeri
- UEK4 veya üzeri ile Oracle Linux
- Arka bağlantı noktaları çekirdeği, 10. ve üzeri ile 9.
- 4,14 çekirdekine sahip CoreOS veya üzeri


## <a name="size-table-definitions"></a>Boyut tablosu tanımları

- Depolama kapasitesi GiB veya 1024^3 bayt cinsinden gösterilmiştir. GB (1000^3 bayt) ile ölçülen diskleri GiB (1024^3 bayt) ile ölçülen disklerle karşılaştırırken GiB cinsinden verilen kapasite rakamlarının daha küçük görünebileceğini unutmayın. Örneğin: 1023 GiB = 1098,4 GB
- Disk aktarım hızı, saniye başına giriş/çıkış işlemi sayısı (IOPS) ve MB/sn (MB/sn = 10^6 bayt/sn) üzerinden ölçülür.
- Sanal makinelerinize en iyi performansı elde etmek istiyorsanız, vCPU başına veri disklerinin sayısını 2 disk ile sınırlamanız gerekir.
- **Beklenen ağ bant** genişliği tüm ormanlarda tüm NIC 'lerde [sanal makine türü başına ayrılan en yüksek bant genişliğine sahip](../articles/virtual-network/virtual-machine-network-throughput.md) olur. Üst sınırlar garanti edilmez, ancak hedeflenen uygulama için doğru VM türünün seçilmesine ilişkin kılavuzluk sağlamak için tasarlanmıştır. Gerçek ağ performansı, ağ tıkanıklığı, uygulama yükleri ve ağ ayarları gibi çeşitli faktörlere bağlıdır. Ağ aktarım hızını iyileştirme hakkında bilgi için bkz. [Windows ve Linux için ağ aktarım hızını iyileştirme](../articles/virtual-network/virtual-network-optimize-network-bandwidth.md). Linux veya Windows üzerinde beklenen ağ performansını gerçekleştirmek için belirli bir sürümü seçmeniz veya VM’nizi en iyi duruma getirmeniz gerekebilir. Daha fazla bilgi için bkz. [Sanal makine aktarım hızını güvenilir bir şekilde test etme](../articles/virtual-network/virtual-network-bandwidth-testing.md).
