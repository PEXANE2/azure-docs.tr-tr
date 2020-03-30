---
title: Lsv2 serisi - Azure Sanal Makineler
description: Lsv2 serisi VM'ler için teknik özellikler.
services: virtual-machines
author: sasha-melamed
ms.service: virtual-machines
ms.topic: article
ms.date: 02/03/2020
ms.author: lahugh
ms.openlocfilehash: 103e19d6e299956b5ee1ad45b577e25f9f2de1c4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78164041"
---
# <a name="lsv2-series"></a>Lsv2 serisi

Lsv2 serisi yüksek iş kolu, düşük gecikme, doğrudan 2.55GHz tüm çekirdek desteği ve 3.0GHz maksimum destek ile [AMD EPYC<sup>TM</sup> 7551 işlemci](https://www.amd.com/en/products/epyc-7000-series) üzerinde çalışan yerel NVMe depolama eşlenen özellikleri. Lsv2 serisi VM'ler aynı anda çoklu iş parçacığı yapılandırmasında 8 ile 80 vCPU arasında boyutlarda gelir.  VCPU başına 8 GiB bellek ve 8 vCPUs başına bir 1.92TB NVMe SSD M.2 cihazı vardır ve L80s v2'de 19.2 TB'ye (10x1.92TB) kadar bellek mevcuttur.

> [!NOTE]
> Lsv2 serisi VM'ler, dayanıklı veri diskleri kullanmak yerine doğrudan VM'ye bağlı düğümdeki yerel diski kullanmak için optimize edilmiştir. Bu, iş yükleri için daha fazla IOS / iş elde etmenizi sağlar. Lsv2 ve Ls serisi, dayanıklı veri diskleri tarafından elde edilebilen IOP'leri artırmak için yerel bir önbellek oluşturulmasını desteklemez.
>
> Yerel diskin yüksek iş verisi ve IOPs lsv2 serisi VM'ler, tek bir VM'nin başarısız olması durumunda kalıcılık elde etmek için birden çok VM'de verileri çoğaltan Apache Cassandra ve MongoDB gibi NoSQL depoları için idealdir.
>
> Daha fazla bilgi için Windows [veya](../virtual-machines/windows/storage-performance.md) [Linux](../virtual-machines/linux/storage-performance.md)için Lsv2 serisi sanal makinelerde performansı optimize etme 'ye bakın.  

ACU: 150-175

Premium Depolama: Desteklenen

Premium Depolama önbelleğe alma: Desteklenmiyor

Canlı Geçiş: Desteklenmiyor

Bellek Koruma Güncelleştirmeleri: Desteklenmiyor

| Boyut | Sanal işlemci | Bellek (GİB) | Geçici disk<sup>1</sup> (GiB) | NVMe Diskler<sup>2</sup> | NVMe Disk iş özeti<sup>3</sup> (IOPS/MBps oku) | Maksimum caçrlanmamış veri diski veri girişi (IOS/MBps)<sup>4</sup> | Maksimum Veri Diskleri | Max NIC ' ler / Beklenen ağ bant genişliği (Mbps) |
|---|---|---|---|---|---|---|---|---|
| Standard_L8s_v2   |  8 |  64 |  80 |  1x1.92 TB  | 400000/2000  | 8000/160   | 16 | 2 / 3200   |
| Standard_L16s_v2  | 16 | 128 | 160 |  2x1.92 TB  | 800000/4000  | 16000/320  | 32 | 4 / 6400   |
| Standard_L32s_v2  | 32 | 256 | 320 |  4x1.92 TB  | 1.5M/8000    | 32000/640  | 32 | 8 / 12800  |
| Standard_L48s_v2  | 48 | 384 | 480 |  6x1.92 TB  | 2.2M/14000   | 48000/960  | 32 | 8 / 16000+ |
| Standard_L64s_v2  | 64 | 512 | 640 |  8x1.92 TB  | 2.9M/16000   | 64000/1280 | 32 | 8 / 16000+ |
| Standard_L80s_v2<sup>5</sup> | 80 | 640 | 800 | 10x1.92TB | 3.8M/20000 | 80000/1400 | 32 | 8 / 16000+ |

<sup>1</sup> Lsv2 serisi VM'ler işletim sistemi sayfalama/takas dosya kullanımı için standart bir SCSI tabanlı geçici kaynak diskine sahiptir (D: Windows'da, /dev/sdb on Linux). Bu disk, her 8 vCPUs için 80 GiB depolama, 4.000 IOPS ve 80 MBps aktarım hızı sağlar (örneğin Standard_L80s_v2 40.000 IOPS ve 800 MBPS'de 800 GiB sağlar). Bu, NVMe sürücülerinin tamamen uygulama kullanımına adanmasını sağlar. Bu disk Geçicidir ve tüm veriler stop/deallocate'da kaybolur.

<sup>2</sup> Yerel NVMe Diskleri geçicidir, VM'nizi durdurur/kullanırsanız bu disklerde veriler kaybolur.

<sup>3</sup> Hyper-V NVMe Direct teknolojisi, konuk VM alanına güvenli bir şekilde eşlenen yerel NVMe sürücülere sorunsuz erişim sağlar.  Maksimum performans elde etmek için Azure Marketi'nden en son WS2019 veya Ubuntu 18.04 veya 16.04'ü kullanmak gerekir.  Yazma performansı IO boyutuna, sürücü yüküne ve kapasite kullanımına bağlı olarak değişir.

<sup>4</sup> Lsv2 serisi VM'ler, Lsv2 iş yüklerinden yararlanamadığı için veri diski için ana bilgisayar önbelleği sağlamaz.  Ancak Lsv2 VM'ler Azure'un Geçici VM OS disk seçeneğini (en fazla 30 GiB) barındırabilir.

64 vCP'den fazla vCP's's i olan <sup>5</sup> VM bu desteklenen konuk işletim sistemlerinden birini gerektirir:

- Windows Server 2016 veya sonrası
- Ubuntu 16.04 LTS veya daha sonraki, Azure ayarlı çekirdekli (4,15 çekirdek veya daha sonraki)
- SLES 12 SP2 veya sonrası
- Microsoft tarafından sağlanan LIS paketi 4.3.1 (veya daha sonra) yüklü olan RHEL veya CentOS sürüm 6.7 ile 6.10
- Microsoft tarafından sağlanan LIS paketi 4.2.1 (veya daha sonra) yüklü OLAN RHEL veya CentOS sürüm 7.3
- RHEL veya CentOS sürüm 7.6 veya sonrası
- UEK4 veya sonrası ile Oracle Linux
- Debian 9 backports çekirdek, Debian 10 veya daha sonra
- CoreOS 4.14 çekirdek veya daha sonra

## <a name="size-table-definitions"></a>Boyut tablosu tanımları

- Depolama kapasitesi GiB veya 1024^3 bayt cinsinden gösterilmiştir. GB (1000^3 bayt) ile ölçülen diskleri GiB (1024^3 bayt) ile ölçülen disklerle karşılaştırırken GiB cinsinden verilen kapasite rakamlarının daha küçük görünebileceğini unutmayın. Örneğin: 1023 GiB = 1098,4 GB
- Disk aktarım hızı, saniye başına giriş/çıkış işlemi sayısı (IOPS) ve MB/sn (MB/sn = 10^6 bayt/sn) üzerinden ölçülür.
- VM'leriniz için en iyi performansı elde etmek istiyorsanız, veri diski sayısını vCPU başına 2 diskle sınırlamanız gerekir.
- **Beklenen ağ bant genişliği,** tüm HEDEFLER için tüm NIC'ler arasında [VM türüne göre ayrılan](../virtual-network/virtual-machine-network-throughput.md) maksimum toplanan bant genişliğidir. Üst sınırlar garanti edilmez, ancak hedeflenen uygulama için doğru VM türünün seçilmesine ilişkin kılavuzluk sağlamak için tasarlanmıştır. Gerçek ağ performansı, ağ tıkanıklığı, uygulama yükleri ve ağ ayarları gibi çeşitli faktörlere bağlıdır. Ağ aktarım hızını iyileştirme hakkında bilgi için bkz. [Windows ve Linux için ağ aktarım hızını iyileştirme](../virtual-network/virtual-network-optimize-network-bandwidth.md). Linux veya Windows üzerinde beklenen ağ performansını gerçekleştirmek için belirli bir sürümü seçmeniz veya VM’nizi en iyi duruma getirmeniz gerekebilir. Daha fazla bilgi için bkz. [Sanal makine aktarım hızını güvenilir bir şekilde test etme](../virtual-network/virtual-network-bandwidth-testing.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure bilgi [işlem birimlerinin (ACU)](acu.md) Azure SK'leri genelinde bilgi işlem performansını karşılaştırmanıza nasıl yardımcı olabileceği hakkında daha fazla bilgi edinin.
