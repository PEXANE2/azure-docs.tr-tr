---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 08/15/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 72d87142f9b9c1f7bcb2b02281851bd1e29bc9c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78262488"
---
Azure yönetilen diskler şu anda dört disk türü sunar ve her tür belirli müşteri senaryolarına yöneliktir.

## <a name="disk-comparison"></a>Disk karşılaştırması

Aşağıdaki tablo, ne kullanacağınıza karar vermenize yardımcı olmak için yönetilen diskler için ultra diskler, birinci sınıf katı hal sürücüleri (SSD), standart SSD ve standart sabit disk sürücülerinin (HDD) karşılaştırmasını sağlar.

|   | Ultra disk   | Premium SSD   | Standart SSD   | Standart HDD   |
|---------|---------|---------|---------|---------|
|Disk türü   |SSD   |SSD   |SSD   |HDD   |
|Senaryo   |[SAP HANA,](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)üst düzey veritabanları (örneğin, SQL, Oracle) ve diğer işlem ağırlıklı iş yükleri gibi IO yoğun iş yükleri.   |Üretim ve performansa duyarlı iş yükleri   |Web sunucuları, az kullanılan kurumsal uygulamalar ve geliştirme/test   |Yedekleme, kritik olmayan, seyrek erişim   |
|En büyük disk boyutu   |65.536 gibibayt (GiB)    |32.767 GiB    |32.767 GiB   |32.767 GiB   |
|En yüksek işleme hızı   |2.000 MiB/sn    |900 MiB/sn   |750 MiB/sn   |500 MiB/sn   |
|Maks. IOPS   |160.000    |20.000   |6.000   |2.000   |

## <a name="ultra-disk"></a>Ultra disk

Azure ultra diskler Azure IaaS VM'leri içi yüksek işleme hızı, yüksek IOPS ve tutarlı olarak düşük gecikme süresi getirir. Ultra disklerin bazı ek avantajları arasında, sanal makinelerinizi (VM) yeniden başlatmaya gerek kalmadan diskin performansını nislinize göre iş yüklerinizle birlikte dinamik olarak değiştirme olanağı yer almaktadır. Ultra diskler SAP HANA, en üst katman veritabanları ve ağır işlem içeren iş yükleri gibi yoğun veri kullanan iş yüklerine uygundur. Ultra diskler yalnızca veri diski olarak kullanılabilir. İşletim sistemi diskleri olarak Premium SSD'leri kullanmanızı öneririz.

### <a name="performance"></a>Performans

Ultra bir disk sağlarken, diskin kapasitesini ve performansını bağımsız olarak yapılandırabilirsiniz. Ultra diskler, 4 GiB'den 64 TiB'ye kadar çeşitli sabit boyutlarda gelir ve IOPS ve iş buzunu bağımsız olarak yapılandırmanızı sağlayan esnek bir performans yapılandırma modeline sahiptir.

Ultra disklerin bazı önemli özellikleri şunlardır:

- Disk kapasitesi: Ultra disk kapasitesi 4 GiB ile 64 TiB arasında değişir.
- Disk IOPS: Ultra diskler, disk başına maksimum 160 K IOPS'ye kadar 300 IOPS/GiB IOPS sınırlarını destekler. Sağladığınız IOPS'yi elde etmek için, seçili Disk IOPS'nin VM IOPS sınırından daha az olduğundan emin olun. Disk başına minimum garantili IOPS 2 IOPS/GiB olup, genel taban çizgisi minimum u 100 IOPS'dir. Örneğin, 4 GiB ultra diskinvarsa, sekiz IOPS yerine en az 100 IOPS'ye sahip olursunuz.
- Disk verimi: Ultra disklerde, tek bir diskin verim sınırı, her bir Sağlanan IOPS için 256 KiB/s'dir ve disk başına en fazla 2000 MBps'dir (mbps = saniyede 10^6 Bayt). Disk başına minimum garantili iş başı, her bir IOPS için 4KiB/s'dir ve genel taban çizgisi minimumu 1 MBp'dir.
- Ultra diskler, diski sanal makineden ayırmadan çalışma zamanında disk performans özniteliklerini (IOPS ve iş bölümü) ayarlamayı destekler. Bir disküzerinde disk performansı yeniden boyutlandırma işlemi düzenlendikten sonra, değişikliğin gerçekten etkili olması bir saat kadar sürebilir. 24 saatlik bir süre içinde dört performans yeniden boyutlandırma işlemi sınırı vardır. Performans bant genişliği kapasitesi eksikliği nedeniyle performans yeniden boyutlandırma işleminin başarısız olması mümkündür.

### <a name="disk-size"></a>Disk boyutu

|Disk Boyutu (GiB)  |IOPS Kapağı  |Elde İbadet Kapağı (MBps)  |
|---------|---------|---------|
|4     |1.200         |300         |
|8     |2,400         |600         |
|16     |4.800         |1.200         |
|32     |9.600         |2.000         |
|64     |19,200         |2.000         |
|128     |38,400         |2.000         |
|256     |76.800         |2.000         |
|512     |80,000         |2.000         |
|1,024-65,536 (bu aralıktaki boyutlar 1 TiB'lik artışlarla artmaktadır)     |160.000         |2.000         |

### <a name="ga-scope-and-limitations"></a>GA kapsamı ve sınırlamaları

[!INCLUDE [managed-disks-ultra-disks-GA-scope-and-limitations](managed-disks-ultra-disks-GA-scope-and-limitations.md)]
