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
ms.openlocfilehash: 7385888c54d46e706621f781a64d12d3ae7aa5fb
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69512652"
---
# <a name="what-disk-types-are-available-in-azure"></a>Azure 'da hangi disk türleri kullanılabilir?

Azure yönetilen diskler Şu anda dört disk türü sunmaktadır, her tür belirli müşteri senaryolarına yönelik olarak tasarlanmıştır.

## <a name="disk-comparison"></a>Disk karşılaştırması

Aşağıdaki tabloda, ne kullanacağınıza karar vermenize yardımcı olmak üzere, Ultra diskler, Premium katı hal sürücüleri (SSD), standart SSD ve yönetilen diskler için standart sabit disk sürücüleri (HDD) karşılaştırması sunulmaktadır.

|   | Ultra disk   | Premium SSD   | Standart SSD   | Standart HDD   |
|---------|---------|---------|---------|---------|
|Disk türü   |SSD   |SSD   |SSD   |HDD   |
|Senaryo   |SAP HANA, üst katman veritabanları (örneğin, SQL, Oracle) ve diğer işlem açısından ağır iş yükleri gibi GÇ yoğunluklu iş yükleri.   |Üretim ve performansa duyarlı iş yükleri   |Web sunucuları, hafif kullanılan kurumsal uygulamalar ve geliştirme/test   |Yedekleme, kritik olmayan, seyrek erişimli erişim   |
|Disk boyutu   |65.536 gibibayt (gib)    |32.767 GiB    |32.767 GiB   |32.767 GiB   |
|En fazla aktarım hızı   |2\.000 MIB/sn    |900 MIB/sn   |750 MIB/sn   |500 MIB/sn   |
|Maks. IOPS   |160.000    |20.000   |6,000   |2,000   |

## <a name="ultra-disk"></a>Ultra disk

Azure Ultra diskler, Azure IaaS VM 'Leri için yüksek aktarım hızı, yüksek ıOPS ve tutarlı düşük gecikmeli disk depolama alanı sunar. Ultra disklerin bazı ek avantajları, sanal makinelerinizi yeniden başlatmanıza gerek kalmadan, iş yükleriniz ile birlikte, diskin performansını dinamik olarak değiştirme olanağını içerir. Ultra diskler SAP HANA, üst katman veritabanları ve işlem açısından ağır iş yükleri gibi veri kullanımı yoğun iş yükleri için uygundur. Ultra diskler yalnızca veri diski olarak kullanılabilir. Premium SSDs 'yi işletim sistemi diskleri olarak kullanmanızı öneririz.

### <a name="performance"></a>Performans

Bir ultra disk sağladığınızda, diskin kapasitesini ve performansını bağımsız olarak yapılandırabilirsiniz. Ultra diskler, 4 GiB 'den 64 TiB 'ye kadar olan çok sayıda sabit boyutta gelir ve ıOPS ve aktarım hızını bağımsız olarak yapılandırmanızı sağlayan esnek bir performans yapılandırma modeli özelliğidir.

Ultra disklerin bazı önemli özellikleri şunlardır:

- Disk kapasitesi: Ultra diskler kapasitesi, 4 GiB 'den 64 TiB 'ye kadar olan kapasite aralıklarıyla değişir.
- Disk ıOPS: Ultra diskler, disk başına en fazla 160 K ıOPS 'ye kadar 300 ıOPS/GiB 'nin ıOPS sınırlarını destekler. Sağladığınız ıOPS 'yi başarmak için, seçilen disk ıOPS 'nin VM ıOPS sınırından küçük olduğundan emin olun. Disk başına minimum ıOPS, en az 100 ıOPS olan 2 ıOPS/GiB 'dir. Örneğin, 4 GiB Ultra diskiniz varsa, en az 100 IOPS, 8 ıOPS yerine olacaktır.
- Disk işleme: Ultra disklerle, tek bir diskin aktarım hızı sınırı her sağlanan ıOPS için 256 KiB/sn, disk başına en fazla 2000 MB/sn (MB/sn = 10 ^ 6 bayt/saniye). Disk başına en düşük aktarım hızı, sağlanan her ıOPS için 4KiB/sn, genel taban çizgisi 1 MB/sn 'dir.
- Ultra diskler, diski sanal makineden kullanımdan çıkarmadan, çalışma zamanında disk performansı özniteliklerinin (ıOPS ve aktarım hızı) ayarlanmasını destekler. Disk performansı yeniden boyutlandırma işlemi bir diskte verildikten sonra, değişikliğin gerçekten etkili olması bir saate kadar sürebilir.

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

Şimdilik, Ultra diskler ek sınırlamalara sahiptir ve bunlar şu şekildedir:

- Her bölge için iki kullanılabilirlik bölgesinde Doğu ABD 2, Güneydoğu Asya ve Kuzey Avrupa desteklenir  
- Yalnızca kullanılabilirlik alanları ile kullanılabilir (bölge dışındaki kullanılabilirlik kümeleri ve tek VM dağıtımları, bir ultra disk iliştirebilme özelliğine sahip olmaz)
- Yalnızca ES/DS v3 sanal makinelerinde desteklenir
- Yalnızca veri diskleri olarak kullanılabilir ve yalnızca 4k fiziksel sektör boyutunu destekler  
- Yalnızca boş disk olarak oluşturulabilir  
- Disk anlık görüntülerini, VM görüntülerini, kullanılabilirlik kümelerini, sanal makine ölçek kümelerini ve Azure disk şifrelemesini henüz desteklemez
- Azure Backup veya Azure Site Recovery tümleştirmeyi henüz desteklemez