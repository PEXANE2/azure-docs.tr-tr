---
title: Dv4 ve Dsv4 serisi-Azure sanal makineleri
description: Dv4 ve Dsv4 serisi VM 'Ler için Özellikler.
author: brbell
ms.author: brbell
ms.reviewer: cynthn
ms.custom: mimckitt
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 06/08/2020
ms.openlocfilehash: 68cd6673283362380fc5a1f4b780f0a22aa53402
ms.sourcegitcommit: 52d2f06ecec82977a1463d54a9000a68ff26b572
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84783592"
---
# <a name="dv4-and-dsv4-series"></a>Dv4 ve Dsv4 serisi

Dv4 ve Dsv4 serisi, &reg; &reg; bir hiper iş yükleri için daha iyi bir değer teklifi sağlayan, bir hiper iş parçacığı yapılandırmasındaki Intel Xeon Platinum 8272CL (Cascade Lake) işlemcilerde çalışır. Sürekli olarak 3,4 GHz 'nin tüm çekirdek Turbo saat hızına sahiptir. 

> [!NOTE]
> Sık sorulan sorular için, [Yerel geçici disk olmadan Azure VM boyutlarına](azure-vms-no-temp-disk.md)bakın.
## <a name="dv4-series"></a>Dv4 serisi

Dv4 serisi boyutlar Intel &reg; Xeon &reg; Platinum 8272CL (Cascade Lake) üzerinde çalışır. Dv4 serisi boyutları, üretim iş yüklerinin çoğu için vCPU, bellek ve uzak depolama seçeneklerinin bir birleşimini sunar. Dv4 serisi VM 'Ler özelliği [Intel &reg; hiper Iş parçacığı teknolojisi](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html).

Uzak veri diski depolaması, sanal makinelerden ayrı olarak faturalandırılır. Premium Depolama disklerini kullanmak için Dsv4 boyutlarını kullanın. Dsv4 boyutları için fiyatlandırma ve faturalandırma ölçümleri dv4 serisi ile aynıdır.


> [!IMPORTANT]
> Bu yeni boyutlar Şu anda yalnızca genel önizleme aşamasındadır. Bu dv4 ve Dsv4 serisi için [buradan](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR_Y3toRKxchLjARedqtguBRURE1ZSkdDUzg1VzJDN0cwWUlKTkcyUlo5Mi4u)kaydolma yapabilirsiniz. 


ACU: 195-210

Premium Depolama: desteklenmiyor

Premium depolama önbelleği: desteklenmiyor

Dinamik Geçiş: destekleniyor

Güncelleştirmeleri koruyan bellek: desteklenir

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | En fazla NIC/beklenen ağ bant genişliği (Mbps) |
|---|---|---|---|---|---|
| Standard_D2_v4 | 2 | 8 | Yalnızca uzak depolama | 4 | 2/1000 |
| Standard_D4_v4 | 4 | 16  | Yalnızca uzak depolama | 8 | 2/2000 |
| Standard_D8_v4 | 8 | 32 | Yalnızca uzak depolama | 16 | 4/4000 |
| Standard_D16_v4 | 16 | 64 | Yalnızca uzak depolama | 32 | 8/8000 |
| Standard_D32_v4 | 32 | 128 | Yalnızca uzak depolama | 32 | 8/16000 |
| Standard_D48_v4 | 48 | 192 | Yalnızca uzak depolama | 32 | 8/24000 |
| Standard_D64_v4 | 64 | 256 | Yalnızca uzak depolama | 32 | 8/30000 |

## <a name="dsv4-series"></a>Dsv4 serisi

Dsv4 serisi boyutlar Intel &reg; Xeon &reg; Platinum 8272CL (Cascade Lake) üzerinde çalışır. Dv4 serisi boyutları, üretim iş yüklerinin çoğu için vCPU, bellek ve uzak depolama seçeneklerinin bir birleşimini sunar. Dsv4 serisi VM 'Ler özelliği [Intel &reg; hiper Iş parçacığı teknolojisi](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html). Uzak veri diski depolaması, sanal makinelerden ayrı olarak faturalandırılır.

> [!IMPORTANT]
> Bu yeni boyutlar Şu anda yalnızca genel önizleme aşamasındadır. Bu dv4 ve Dsv4 serisi için [buradan](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR_Y3toRKxchLjARedqtguBRURE1ZSkdDUzg1VzJDN0cwWUlKTkcyUlo5Mi4u)kaydolma yapabilirsiniz. 

ACU: 195-210

Premium Depolama: desteklenir

Premium depolama önbelleği: desteklenir

Dinamik Geçiş: destekleniyor

Güncelleştirmeleri koruyan bellek: desteklenir

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Önbelleğe alınan en fazla aktarım hızı: ıOPS/MBps (GiB 'de önbellek boyutu) | Önbelleğe alınmamış maksimum disk aktarım hızı: ıOPS/MBps | En fazla NIC/beklenen ağ bant genişliği (Mbps) |
|---|---|---|---|---|---|---|---|
| Standard_D2s_v4 | 2 | 8  | Yalnızca uzak depolama | 4 | 19000/120 (50) | 3000/48 | 2/1000 |
| Standard_D4s_v4 | 4 | 16 | Yalnızca uzak depolama | 8 | 38500/242 (100) | 6400/96 | 2/2000 |
| Standard_D8s_v4 | 8 | 32 | Yalnızca uzak depolama | 16 | 77000/485 (200) | 12800/192 | 4/4000 |
| Standard_D16s_v4 | 16 | 64  | Yalnızca uzak depolama | 32 | 154000/968 (400) | 25600/384 | 8/8000 |
| Standard_D32s_v4 | 32 | 128 | Yalnızca uzak depolama | 32 | 308000/1936 (800) | 51200/768 | 8/16000 |
| Standard_D48s_v4 | 48 | 192 | Yalnızca uzak depolama | 32 | 462000/2904 (1200) | 76800/1152 | 8/24000 |
| Standard_D64s_v4 | 64 | 256 | Yalnızca uzak depolama | 32 | 615000/3872 (1600) | 80000/1200 | 8/30000 |
