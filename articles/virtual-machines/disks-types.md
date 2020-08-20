---
title: Azure IaaS VM 'Leri tarafından yönetilen diskler için bir disk türü seçin
description: Ultra diskler, Premium SSD 'Ler, standart SSD 'ler ve standart HDD 'Ler dahil olmak üzere sanal makineler için kullanılabilir Azure disk türleri hakkında bilgi edinin.
author: roygara
ms.author: rogarana
ms.date: 06/03/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 2b8a5cf1bd3df1405f148c5fe84701c04a4d0c0a
ms.sourcegitcommit: 271601d3eeeb9422e36353d32d57bd6e331f4d7b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88658237"
---
# <a name="what-disk-types-are-available-in-azure"></a>Azure'da hangi disk türleri kullanılabilir?

Azure yönetilen diskler Şu anda dört disk türü sunmaktadır, her tür belirli müşteri senaryolarına yönelik olarak tasarlanmıştır.

## <a name="disk-comparison"></a>Disk karşılaştırması

Aşağıdaki tabloda, ne kullanacağınıza karar vermenize yardımcı olmak üzere, Ultra diskler, Premium katı hal sürücüleri (SSD), standart SSD ve yönetilen diskler için standart sabit disk sürücüleri (HDD) karşılaştırması sunulmaktadır.

| Ayrıntı | Ultra disk | Premium SSD | Standart SSD | Standart HDD |
| ------ | ---------- | ----------- | ------------ | ------------ |
|Disk türü   |SSD   |SSD   |SSD   |HDD   |
|Senaryo   |[SAP HANA](~/articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md), üst katman veritabanları (ÖRNEĞIN, SQL, Oracle) ve diğer işlem açısından ağır iş YÜKLERI gibi GÇ yoğunluklu iş yükleri.   |Üretim ve performansa duyarlı iş yükleri   |Web sunucuları, az kullanılan kurumsal uygulamalar ve geliştirme/test   |Yedekleme, kritik olmayan, seyrek erişim   |
|En büyük disk boyutu   |65.536 gibibayt (GiB)    |32.767 GiB    |32.767 GiB   |32.767 GiB   |
|En yüksek işleme hızı   |2.000 MB/s    |900 MB/s   |750 MB/s   |500 MB/s   |
|Maks. IOPS   |160.000    |20.000   |6.000   |2.000   |

## <a name="ultra-disk"></a>Ultra disk

Azure ultra diskler Azure IaaS VM'leri içi yüksek işleme hızı, yüksek IOPS ve tutarlı olarak düşük gecikme süresi getirir. Ultra disklerin bazı ek avantajları, sanal makinelerinizi (VM) yeniden başlatmanıza gerek kalmadan, iş yükleriniz ile birlikte, diskin performansını dinamik olarak değiştirme imkanını içerir. Ultra diskler SAP HANA, en üst katman veritabanları ve ağır işlem içeren iş yükleri gibi yoğun veri kullanan iş yüklerine uygundur. Ultra diskler yalnızca veri diski olarak kullanılabilir. İşletim sistemi diskleri olarak Premium SSD'leri kullanmanızı öneririz.

### <a name="performance"></a>Performans

Bir ultra disk sağladığınızda, diskin kapasitesini ve performansını bağımsız olarak yapılandırabilirsiniz. Ultra diskler, 4 GiB 'den 64 TiB 'ye kadar olan çok sayıda sabit boyutta gelir ve ıOPS ve aktarım hızını bağımsız olarak yapılandırmanızı sağlayan esnek bir performans yapılandırma modeli özelliğidir.

Ultra disklerin bazı önemli özellikleri şunlardır:

- Disk kapasitesi: Ultra diskler kapasitesi 4 GiB 'den 64 TiB 'ye kadar değişir.
- Disk ıOPS: Ultra diskler, disk başına en fazla 160 K ıOPS 'ye kadar 300 ıOPS/GiB 'nin ıOPS sınırlarını destekler. Sağladığınız ıOPS 'yi başarmak için, seçilen disk ıOPS 'nin VM ıOPS sınırından küçük olduğundan emin olun. Disk başına en az garanti edilen minimum ıOPS, en az 100 ıOPS olan 2 ıOPS/GiB 'dir. Örneğin, 4 GiB Ultra diskiniz varsa sekiz ıOPS yerine en az 100 ıOPS olur.
- Disk işleme: Ultra disklerle, tek bir diskin aktarım hızı sınırı her sağlanan ıOPS için 256 KiB/sn, disk başına en fazla 2000 MB/sn (MB/sn = 10 ^ 6 bayt/saniye). Her bir sağlanan ıOPS için en düşük değer 1 MB/sn olan her bir disk için en az garanti edilen aktarım hızı en az 1 MBps 'dir.
- Ultra diskler, diski sanal makineden kullanımdan çıkarmadan, çalışma zamanında disk performansı özniteliklerinin (ıOPS ve aktarım hızı) ayarlanmasını destekler. Disk performansı yeniden boyutlandırma işlemi bir diskte verildikten sonra, değişikliğin gerçekten etkili olması bir saate kadar sürebilir. 24 saatlik bir pencere sırasında dört performans yeniden boyutlandırma işlemi sınırı vardır. Performans yeniden boyutlandırma işleminin performans bant genişliği kapasitesinin olmaması nedeniyle başarısız olması mümkündür.

### <a name="disk-size"></a>Disk boyutu

|Disk boyutu (GiB)  |IOPS Cap  |Verimlilik sınırı (MBps)  |
|---------|---------|---------|
|4     |1.200         |300         |
|8     |2.400         |600         |
|16     |4.800         |1.200         |
|32     |9.600         |2.000         |
|64     |19.200         |2.000         |
|128     |38.400         |2.000         |
|256     |76.800         |2.000         |
|512     |80,000         |2.000         |
|1024-65536 (Bu aralıktaki Boyutlar 1 TiB artışlarına göre artar)     |160.000         |2.000         |

### <a name="ga-scope-and-limitations"></a>GA kapsamı ve sınırlamaları

[!INCLUDE [managed-disks-ultra-disks-GA-scope-and-limitations](~/includes/managed-disks-ultra-disks-GA-scope-and-limitations.md)]


Ultra diskler kullanmaya başlamak istiyorsanız, konudaki makalemize bakın: [Azure Ultra diskleri kullanma](disks-enable-ultra-ssd.md).

## <a name="premium-ssd"></a>Premium SSD

Azure Premium SSD 'ler, giriş/çıkış (GÇ) yoğun iş yükleri ile sanal makineler (VM) için yüksek performanslı ve düşük gecikmeli disk desteği sunar. Premium Depolama disklerinin hız ve performansından yararlanmak için, mevcut VM disklerini Premium SSD 'Ler 'e geçirebilirsiniz. Premium SSD 'Ler, görev açısından kritik üretim uygulamaları için uygundur. Premium SSD 'Ler, yalnızca Premium Depolama ile uyumlu olan VM serileri ile kullanılabilir.

Windows için Azure 'daki tek tek VM türleri ve boyutları hakkında daha fazla bilgi edinmek için, Premium Depolama ile uyumlu olan boyutlar dahil, bkz. [WINDOWS VM boyutları](~/articles/virtual-machines/windows/sizes.md). Linux için Azure 'daki tek tek VM türleri ve boyutları hakkında daha fazla bilgi edinmek için, bkz. [LINUX VM boyutları](~/articles/virtual-machines/linux/sizes.md). Bu makalelerden herhangi birinden, Premium Depolama ile uyumlu olup olmadığını öğrenmek için her bir VM boyutu makalesini denetlemeniz gerekir.

### <a name="disk-size"></a>Disk boyutu
[!INCLUDE [disk-storage-premium-ssd-sizes](~/includes/disk-storage-premium-ssd-sizes.md)]

Standart depolamanın aksine bir Premium Depolama diski sağladığınızda, bu diskin kapasitesi, ıOPS ve aktarım hızı garanti edilir. Örneğin, bir P50 diski oluşturursanız, Azure bu disk için 4.095 GB depolama kapasitesi, 7.500 ıOPS ve 250 MB/sn aktarım hızı sağlar. Uygulamanız, kapasite ve performansın tümünü veya bir bölümünü kullanabilir. Premium SSD diskler, önceki tabloda% 99,9 ' de açıklanan düşük tek basamaklı milisaniyelik gecikme süreleri ve hedef ıOPS ve aktarım hızı sağlamak üzere tasarlanmıştır.

## <a name="bursting"></a>Patlaması

P30 ' den küçük Premium SSD boyutları artık disk patlaması sunar ve disk başına ıOPS 'yi 3.500 ve bant genişliğine kadar en fazla 170 Mbps olacak şekilde alabilir. Burdıya otomatik ve bir kredi sistemine göre çalışır. Krediler, disk trafiği sağlanan performans hedefinin altındaysa otomatik olarak bir patlama demetini içinde biriktirilir ve trafik hedefin ötesinde en fazla patlama sınırına kadar, krediler otomatik olarak tüketilir. En fazla patlama sınırı, ' den tüketmek üzere patlama kredileriniz olsa bile, disk ıOPS & bant genişliği üst sınırını tanımlar. Disk patlaması, GÇ desenlerinin öngörülemeyen değişikliklerinde daha iyi tolerans sağlar. Çıkmanız gerekirse trafiği olan işletim sistemi diski önyüklemesi ve uygulamaları için en iyi şekilde yararlanabilirsiniz.    

Bir kullanıcı eylemi gerekmeden, varsayılan olarak geçerli disk boyutlarının yeni dağıtımları üzerinde, disk patlaması desteği etkinleştirilecek. Geçerli boyutlardaki mevcut diskler için, iki seçenekten birini kullanarak gereksiz bir şekilde etkinleştirebilirsiniz: diski ayırma ve yeniden bağlama ya da bağlı VM 'yi durdurup yeniden başlatma. Disk, en yüksek veri bloğu sınırı olan 30 dakikalık bir sanal makineye eklendiğinde, tüm veri bloğu uygulanabilir disk boyutları tam bir patlama kredisi demeti ile başlar. Azure disklerinde ne kadar iş çalıştığı hakkında daha fazla bilgi edinmek için bkz. [Premium SSD burdımı](~/articles/virtual-machines/linux/disk-bursting.md). 

### <a name="transactions"></a>İşlemler

Premium SSD 'Ler için, 256 kıb aktarım hızına eşit veya daha küçük olan her g/ç işlemi tek bir g/ç işlemi olarak kabul edilir. 256 Kigb 'den büyük g/ç işlemleri, 256 KiB boyutundaki birden fazla g/ç işlemi olarak kabul edilir.

## <a name="standard-ssd"></a>Standart SSD

Azure Standart SSD 'ler, düşük ıOPS düzeylerinde tutarlı performans gerektiren iş yükleri için optimize edilmiş uygun maliyetli bir depolama seçeneğidir. Standart SSD, özellikle de şirket içinde HDD çözümlerinizde çalışan iş yüklerinin farkıyla ilgili sorunlarla karşılaşırsanız, buluta taşımak isteyen kişiler için iyi bir giriş düzeyi deneyimi sunmaktadır. Standart HDD 'ler ile karşılaştırıldığında, standart SSD 'Ler daha iyi kullanılabilirlik, tutarlılık, güvenilirlik ve gecikme süresi sağlar. Standart SSD 'Ler, Web sunucuları, düşük ıOPS uygulama sunucuları, hafif kullanılan kurumsal uygulamalar ve geliştirme/test iş yükleri için uygundur. Standart HDD 'ler gibi tüm Azure sanal makinelerinde standart SSD 'ler de mevcuttur.

### <a name="disk-size"></a>Disk boyutu
[!INCLUDE [disk-storage-standard-ssd-sizes](~/includes/disk-storage-standard-ssd-sizes.md)]

Standart SSD 'ler, tek basamaklı milisaniyelik gecikme süreleri ve ıOPS ve aktarım hızı, önceki tabloda %99 ' de tanımlanan sınırlara kadar bir süre sağlamak üzere tasarlanmıştır. Gerçek ıOPS ve aktarım hızı bazen trafik desenlerine bağlı olarak farklılık gösterebilir. Standart SSD 'Ler, daha düşük gecikme süresine sahip HDD disklerinden daha tutarlı performans sağlayacaktır.

### <a name="transactions"></a>İşlemler

Standart SSD 'Ler için, 256 kıb aktarım hızına eşit veya daha küçük olan her g/ç işlemi tek bir g/ç işlemi olarak kabul edilir. 256 Kigb 'den büyük g/ç işlemleri, 256 KiB boyutundaki birden fazla g/ç işlemi olarak kabul edilir. Bu işlemlerin bir faturalandırma etkisi vardır.

## <a name="standard-hdd"></a>Standart HDD

Azure Standart HDD 'Ler, gecikme süresine duyarlı iş yükleri çalıştıran VM 'Ler için güvenilir, düşük maliyetli disk desteği sunar. Standart depolama ile, veriler sabit disk sürücülerinde (HDD 'Ler) depolanır. Standart HDD disklerin gecikmesi, ıOPS ve verimlilik, SSD tabanlı disklere kıyasla daha geniş farklılık gösterebilir. Standart HDD diskler, 10ms altında yazma gecikmeleri ve g/ç işlemleri için 20 MS 'nin altında okuma gecikmeleri sunacak şekilde tasarlanmıştır, ancak gerçek performans, GÇ boyutu ve iş yükü düzenine göre farklılık gösterebilir. VM 'lerle çalışırken geliştirme/test senaryoları ve daha az kritik iş yükleri için standart HDD disklerini kullanabilirsiniz. Standart HDD 'ler tüm Azure bölgelerinde kullanılabilir ve tüm Azure VM 'Leri ile kullanılabilir.

### <a name="disk-size"></a>Disk boyutu
[!INCLUDE [disk-storage-standard-hdd-sizes](~/includes/disk-storage-standard-hdd-sizes.md)]

### <a name="transactions"></a>İşlemler

Standart HDD 'Ler için, her GÇ işlemi, g/ç boyutundan bağımsız olarak tek bir işlem olarak değerlendirilir. Bu işlemlerin bir faturalandırma etkisi vardır.

## <a name="billing"></a>Faturalandırma

Yönetilen diskler kullanılırken, aşağıdaki faturalandırma konuları geçerlidir:

- Disk türü
- yönetilen disk boyutu
- Anlık Görüntüler
- Giden veri aktarımları
- İşlem sayısı

**Yönetilen disk boyutu**: yönetilen diskler sağlanan boyutta faturalandırılır. Azure sağlanan boyutu (yuvarlanır) en yakın sunulan disk boyutuna eşler. Sunulan disk boyutlarının ayrıntıları için bkz. önceki tablolar. Her disk desteklenen bir sağlanan disk boyutu teklifiyle eşlenir ve buna göre faturalandırılır. Örneğin, bir 200 GiB Standart SSD sağladıysanız, E15 (256 GiB) disk boyutu teklifiyle eşlenir. Sağlanan tüm diskler için faturalandırma, depolama teklifi için aylık fiyat kullanılarak günlere eşit olarak dağıtılır. Örneğin, bir E10 diski sağladıysanız ve 20 saat sonra sildiyseniz, E10 teklifi için 20 saate göre faturalandırılırsınız. Bu, diske yazılan gerçek veri miktarına bakılmaksızın olur.

**Anlık görüntüler**: anlık görüntüler kullanılan boyuta göre faturalandırılır. Örneğin, sağlanan 64 GiB kapasitesine sahip bir yönetilen diskin anlık görüntüsünü ve 10 GiB 'nin gerçek kullanılan veri boyutunu oluşturursanız, anlık görüntü yalnızca 10 GiB 'nin kullanılan veri boyutu için faturalandırılır.

Anlık görüntüler hakkında daha fazla bilgi için [yönetilen diske genel bakış](managed-disks-overview.md)' daki anlık görüntüler bölümüne bakın.

**Giden veri aktarımları**: [giden veri aktarımları](https://azure.microsoft.com/pricing/details/bandwidth/) (Azure veri merkezlerinden çıkan veriler) bant genişliği kullanımı için faturalandırılır.

**İşlemler**: Standart yönetilen bir diskte gerçekleştirdiğiniz işlem sayısı için faturalandırılırsınız. Standart SSD 'Ler için, 256 kıb aktarım hızına eşit veya daha küçük olan her g/ç işlemi tek bir g/ç işlemi olarak kabul edilir. 256 Kigb 'den büyük g/ç işlemleri, 256 KiB boyutundaki birden fazla g/ç işlemi olarak kabul edilir. Standart HDD 'Ler için, her GÇ işlemi, g/ç boyutundan bağımsız olarak tek bir işlem olarak değerlendirilir.

İşlem maliyetleri dahil olmak üzere yönetilen disklere yönelik fiyatlandırma hakkında ayrıntılı bilgi için bkz. [yönetilen diskler fiyatlandırması](https://azure.microsoft.com/pricing/details/managed-disks).

### <a name="ultra-disk-vm-reservation-fee"></a>Ultra disk VM ayırma ücreti

Azure VM 'Leri, Ultra disklerle uyumlu olup olmadığını gösterebilir. Bir ultra disk uyumlu VM, performansı iyileştirmek ve gecikme süresini azaltmak için işlem VM örneği ve blok depolama ölçek birimi arasında ayrılmış bant genişliği kapasitesini ayırır. Bu özelliği sanal makineye eklemek, yalnızca bir ultra disk iliştirmeden VM 'de Ultra disk özelliğini etkinleştirdiyseniz uygulanan bir rezervasyon ücretine neden olur. Ultra disk uyumlu VM 'ye bir ultra disk eklendiğinde, bu ücret uygulanmaz. Bu ücret, VM 'de sağlanan vCPU başına. 

> [!Note]
> [Kısıtlanmış çekırdek VM boyutları](~/articles/virtual-machines/linux/constrained-vcpu.md)için, rezervasyon ücreti, kısıtlı çekirdekler değil, gerçek vCPU sayısına dayanır. Standard_E32 8s_v3 için, ayırma ücreti 32 çekirdekleri temel alır. 

Ultra disk fiyatlandırma ayrıntıları için [Azure diskleri fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/managed-disks/) bakın.

### <a name="azure-disk-reservation"></a>Azure disk ayırma

Disk ayırma, indirimli bir yıllık disk depolama alanı satın alma seçeneğidir ve toplam maliyetinizi azaltır. Bir disk ayırması satın alırken, bir yıllık dönem için Doğu ABD 2 bölgede bulunan 10 P30 (1TiB) Premium SSDs gibi bir hedef bölgede belirli bir disk SKU 'SU seçersiniz. Ayırma deneyimi, ayrılmış sanal makine (VM) örneklerine benzerdir. Tasarruf etmenizi en üst düzeye çıkarmak için VM ve disk ayırmalarını paketleyebilirsiniz. Azure disk rezervasyonu şimdilik, P30 (1TiB) ile P80 (32 TiB) arasında Premium SSD SKU 'Ları için tüm üretim bölgelerinde bir yıl taahhüt planı sunar. Ayrılmış diskler fiyatlandırması hakkında daha fazla bilgi için bkz. [Azure diskleri fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/managed-disks/).

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için bkz. [yönetilen diskler fiyatlandırması](https://azure.microsoft.com/pricing/details/managed-disks/) .
