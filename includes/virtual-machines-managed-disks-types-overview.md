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
ms.openlocfilehash: 31a9da0678f602afcc117e5b2f7927af379da668
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75466583"
---
Azure yönetilen diskler Şu anda dört disk türü sunmaktadır, her tür belirli müşteri senaryolarına yönelik olarak tasarlanmıştır.

## <a name="disk-comparison"></a>Disk karşılaştırması

Aşağıdaki tabloda, ne kullanacağınıza karar vermenize yardımcı olmak üzere, Ultra diskler, Premium katı hal sürücüleri (SSD), standart SSD ve yönetilen diskler için standart sabit disk sürücüleri (HDD) karşılaştırması sunulmaktadır.

|   | Ultra disk   | Premium SSD   | Standart SSD   | Standart HDD   |
|---------|---------|---------|---------|---------|
|Disk türü   |SSD   |SSD   |SSD   |HDD   |
|Senaryo   |[SAP HANA](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md), üst katman veritabanları (ÖRNEĞIN, SQL, Oracle) ve diğer işlem açısından ağır iş YÜKLERI gibi GÇ yoğunluklu iş yükleri.   |Üretim ve performansa duyarlı iş yükleri   |Web sunucuları, az kullanılan kurumsal uygulamalar ve geliştirme/test   |Yedekleme, kritik olmayan, seyrek erişim   |
|En büyük disk boyutu   |65.536 gibibayt (GiB)    |32.767 GiB    |32.767 GiB   |32.767 GiB   |
|En yüksek işleme hızı   |2\.000 MiB/sn    |900 MiB/sn   |750 MiB/sn   |500 MiB/sn   |
|Maks. IOPS   |160.000    |20.000   |6,000   |2,000   |

## <a name="ultra-disk"></a>Ultra disk

Azure ultra diskler Azure IaaS VM'leri içi yüksek işleme hızı, yüksek IOPS ve tutarlı olarak düşük gecikme süresi getirir. Ultra disklerin bazı ek avantajları, sanal makinelerinizi (VM) yeniden başlatmanıza gerek kalmadan, iş yükleriniz ile birlikte, diskin performansını dinamik olarak değiştirme imkanını içerir. Ultra diskler SAP HANA, en üst katman veritabanları ve ağır işlem içeren iş yükleri gibi yoğun veri kullanan iş yüklerine uygundur. Ultra diskler yalnızca veri diski olarak kullanılabilir. İşletim sistemi diskleri olarak Premium SSD'leri kullanmanızı öneririz.

### <a name="performance"></a>Performans

Bir ultra disk sağladığınızda, diskin kapasitesini ve performansını bağımsız olarak yapılandırabilirsiniz. Ultra diskler, 4 GiB 'den 64 TiB 'ye kadar olan çok sayıda sabit boyutta gelir ve ıOPS ve aktarım hızını bağımsız olarak yapılandırmanızı sağlayan esnek bir performans yapılandırma modeli özelliğidir.

Ultra disklerin bazı önemli özellikleri şunlardır:

- Disk kapasitesi: Ultra diskler kapasitesi 4 GiB 'den 64 TiB 'ye kadar değişir.
- Disk ıOPS: Ultra diskler, disk başına en fazla 160 K ıOPS 'ye kadar 300 ıOPS/GiB 'nin ıOPS sınırlarını destekler. Sağladığınız ıOPS 'yi başarmak için, seçilen disk ıOPS 'nin VM ıOPS sınırından küçük olduğundan emin olun. Disk başına minimum ıOPS, en az 100 ıOPS olan 2 ıOPS/GiB 'dir. Örneğin, 4 GiB Ultra diskiniz varsa sekiz ıOPS yerine en az 100 ıOPS olur.
- Disk işleme: Ultra disklerle, tek bir diskin aktarım hızı sınırı her sağlanan ıOPS için 256 KiB/sn, disk başına en fazla 2000 MB/sn (MB/sn = 10 ^ 6 bayt/saniye). Disk başına en düşük aktarım hızı, sağlanan her ıOPS için 4KiB/sn, genel taban çizgisi 1 MB/sn 'dir.
- Ultra diskler, diski sanal makineden kullanımdan çıkarmadan, çalışma zamanında disk performansı özniteliklerinin (ıOPS ve aktarım hızı) ayarlanmasını destekler. Disk performansı yeniden boyutlandırma işlemi bir diskte verildikten sonra, değişikliğin gerçekten etkili olması bir saate kadar sürebilir. 24 saatlik bir pencere sırasında dört performans yeniden boyutlandırma işlemi sınırı vardır. Performans yeniden boyutlandırma işleminin performans bant genişliği kapasitesinin olmaması nedeniyle başarısız olması mümkündür.

### <a name="disk-size"></a>Disk boyutu

|Disk Boyutu (GiB)  |IOPS Cap  |Verimlilik sınırı (MBps)  |
|---------|---------|---------|
|4     |1,200         |300         |
|8     |2,400         |600         |
|16     |4,800         |1,200         |
|32     |9\.600         |2,000         |
|64     |19.200         |2,000         |
|128     |38.400         |2,000         |
|256     |76.800         |2,000         |
|512     |80,000         |2,000         |
|1024-65536 (Bu aralıktaki Boyutlar 1 TiB artışlarına göre artar)     |160.000         |2,000         |

### <a name="ga-scope-and-limitations"></a>GA kapsamı ve sınırlamaları

[!INCLUDE [managed-disks-ultra-disks-GA-scope-and-limitations](managed-disks-ultra-disks-GA-scope-and-limitations.md)]