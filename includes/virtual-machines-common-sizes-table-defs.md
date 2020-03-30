---
title: include dosyası
description: include dosyası
services: virtual-machines
author: jonbeck7
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: azcspmt;jonbeck;cynthn
ms.custom: include file
ms.openlocfilehash: 63c53a9b95e27486d7d6944c28f8fb085b1bc6ca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74279190"
---
<!-- Not used for Ls-series -->

## <a name="size-table-definitions"></a>Boyut tablosu tanımları

- Depolama kapasitesi GiB veya 1024^3 bayt cinsinden gösterilmiştir. GB cinsinden ölçülen diskleri (1000^3 bayt) GiB cinsinden ölçülen disklerle karşılaştırdığınızda (1024^3) GiB'de verilen kapasite numaralarının daha küçük görünebileceğini unutmayın. Örneğin, 1023 GiB = 1098,4 GB.
- Disk aktarım hızı, saniye başına giriş/çıkış işlemi sayısı (IOPS) ve MB/sn (MB/sn = 10^6 bayt/sn) üzerinden ölçülür.
- Veri diskleri önbelleğe alınmış veya alınmamış modlarda çalışabilir. Önbelleğe alınmış veri diski işlemi için ana bilgisayar önbelleği **SaltOkunur** veya **OkuYaz** moduna ayarlanır.  Önbelleğe alınmamış veri diski işlemi için ana bilgisayar önbelleği **Yok** moduna ayarlanır.
- VM'leriniz için en iyi performansı elde etmek istiyorsanız, veri diski sayısını vCPU başına iki diskle sınırlamanız gerekir.
- **Beklenen ağ bant genişliği,** tüm HEDEFLER için tüm NIC'ler arasında VM türüne göre ayrılan maksimum toplanan bant genişliğidir. Daha fazla bilgi için [Virtual machine network bant genişliğine](../articles/virtual-network/virtual-machine-network-throughput.md)bakın.

  Üst limitler garanti değildir. Sınırlar, amaçlanan uygulama için doğru VM türünü seçmek için kılavuz sunar. Gerçek ağ performansı, ağ tıkanıklığı, uygulama yükleri ve ağ ayarları gibi çeşitli etkenlere bağlıdır. Ağ iş akışını optimize etme hakkında bilgi için Azure [sanal makineleri için ağ bilginliğini optimize etme bilgisine](../articles/virtual-network/virtual-network-optimize-network-bandwidth.md)bakın. Linux veya Windows'da beklenen ağ performansını elde etmek için belirli bir sürümü seçmeniz veya VM'nizi optimize etmeniz gerekebilir. Daha fazla bilgi için [bkz.](../articles/virtual-network/virtual-network-bandwidth-testing.md)



