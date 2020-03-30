---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 01/22/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 2303d36e93cecfca03894a8b0e55458c03b13d78
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73413019"
---
**Giden veri aktarımları**: [Giden veri aktarımları](https://azure.microsoft.com/pricing/details/bandwidth/) (Azure veri merkezlerinden çıkan veriler) bant genişliği kullanımı için faturalandırmaya tabidir.

**Hareketler**: Standart yönetilen bir diskte gerçekleştirdiğiniz hareket sayısı için faturalandırılırsınız. Standart SSD'ler için, 256 KiB'den daha az veya 256 KiB'den daha az veya eşit olan her G/Ç işlemi tek bir G/Ç işlemi olarak kabul edilir. 256 KiB'den büyük i/ç işlemleri, 256 KiB boyutunda birden fazla G/Ç olarak kabul edilir. Standart HDD'ler için, Her IO işlemi, G/Ç boyutundan bağımsız olarak tek bir işlem olarak kabul edilir.

Yönetilen Diskler için işlem maliyetleri de dahil olmak üzere fiyatlandırma hakkında ayrıntılı bilgi [için](https://azure.microsoft.com/pricing/details/managed-disks)bkz.

### <a name="ultra-disk-vm-reservation-fee"></a>Ultra disk VM rezervasyon ücreti

Azure VM'ler, ultra disklerle uyumlu olup olmadıklarını belirtme özelliğine sahiptir. Ultra disk uyumlu VM, performansı optimize etmek ve gecikme süresini azaltmak için işlem VM örneği ile blok depolama ölçeği birimi arasında özel bant genişliği kapasitesi ayırır. VM'de bu özelliğin eklenmesi, yalnızca VM'de ultra disk özelliğini ultra disk eklemeden etkinleştirdiyseniz uygulanan bir rezervasyon ücretiyle sonuçlanır. Ultra disk uyumlu VM'ye bir ultra disk takıldığında, bu ücret uygulanmaz. Bu ücret VM'de sağlanan vCPU başına dır. 

> [!Note]
> [Kısıtlı çekirdek VM boyutları](../articles/virtual-machines/linux/constrained-vcpu.md)için rezervasyon ücreti, kısıtlı çekirdeklere değil, gerçek vCPUs sayısına bağlıdır. Standard_E32-8s_v3 için rezervasyon ücreti 32 çekirdek esas alınacaktır. 

Ultra disk fiyatlandırma ayrıntıları için [Azure Diskleri fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/managed-disks/) bakın.

### <a name="azure-disk-reservation"></a>Azure disk rezervasyonu

Disk rezervasyonu, bir yıllık disk depolama işlemini indirimli olarak peşin satın alma seçeneğidir ve toplam maliyetinizi düşürür. Bir disk rezervasyonu satın alırken, bir yıl için Doğu ABD 2 bölgesinde 10 P30 (1TiB) premium SSD gibi hedef bölgede belirli bir Disk SKU'su seçersiniz. Rezervasyon deneyimi, ayrılmış sanal makine (VM) örneklerine benzer. Tasarruflarınızı en üst düzeye çıkarmak için VM ve Disk rezervasyonlarını paketleyebilirsiniz. Azure Diskleri Rezervasyon, şimdilik tüm üretim bölgelerinde P30 (1TiB) ile P80 (32 TiB) arasında premium SSD SDO'lar için bir yıllık taahhüt planı sunmaktadır. Ayrılmış Diskler fiyatlandırması hakkında daha fazla bilgi için [Azure Diskleri fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/managed-disks/)bakın.