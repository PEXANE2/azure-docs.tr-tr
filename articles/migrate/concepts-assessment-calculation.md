---
title: Azure Geçir Sunucu Değerlendirmesinde Değerlendirmeler
description: Azure Geçir Sunucu Değerlendirmesi'ndeki değerlendirmeler hakkında bilgi edinin
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 2f76ea5f195be2914cdcdb4de9e93af38504d66e
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81769933"
---
# <a name="assessments-in-azure-migrate-server-assessment"></a>Azure Geçişinde Değerlendirmeler: Sunucu Değerlendirmesi

Bu makalede, [Azure Geçiş: Sunucu Değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracındaki değerlendirmelere genel bir bakış sağlanmaktadır. Araç, şirket içi VMware sanal makinelerini, Hyper-V VM'leri ve Azure'a geçiş için fiziksel sunucuları değerlendirebilir.

## <a name="whats-an-assessment"></a>Değerlendirme nedir?

Sunucu Değerlendirmesi aracıyla yapılan bir değerlendirme, hazır olup olma durumunu ölçer ve şirket içi sunucuları Azure'a geçirmenin etkisini tahmin eder.

> [!NOTE]
> Azure Kamu'da desteklenen hedef değerlendirme konumlarını gözden [geçirin.](migrate-support-matrix.md#supported-geographies-azure-government) Değerlendirmelerdeki VM boyutu önerilerinin, Özellikle Devlet Bulutu bölgeleri için VM serisini kullanacağını unutmayın. VM türleri hakkında [daha fazla bilgi edinin.](https://azure.microsoft.com/global-infrastructure/services/?regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia&products=virtual-machines)

## <a name="types-of-assessments"></a>Değerlendirme türleri

Sunucu Değerlendirmesi ile oluşturduğunuz değerlendirmeler, verilerin anlık görüntüsütür. Sunucu Değerlendirmesi iki tür değerlendirme sağlar.

**Değerlendirme türü** | **Şey** | **Veri**
--- | --- | ---
**Performansa dayalı** | Toplanan performans verilerine dayalı önerilerde bulunan değerlendirmeler | VM boyutu önerisi CPU ve RAM kullanım verilerini temel alır.<br/><br/> Disk türü önerisi, saniyede giriş/çıkış işlemlerine (IOPS) ve şirket içi disklerin çıktısını temel alır. Disk türleri Azure Standart HDD, Azure Standart SSD ve Azure Premium disklerdir.
**Şirket içi olduğu gibi** | Önerilerde bulunmak için performans verilerini kullanmayan değerlendirmeler | VM boyutu önerisi şirket içi VM boyutuna dayanır.<br/><br> Önerilen disk türü, değerlendirme için seçili depolama türünü temel alıntır.

## <a name="how-do-i-run-an-assessment"></a>Değerlendirmeyi nasıl yürütebilirim?

Bir değerlendirmeyi yürütmenin birkaç yolu vardır.

- Hafif bir Azure Geçiş cihazı tarafından toplanan sunucu meta verilerini kullanarak makineleri değerlendirin. Cihaz, şirket içi makineleri keşfeder. Daha sonra makine meta verilerini ve performans verilerini Azure Geçiş'e gönderir.
- Virgülle ayrılmış değerler (CSV) biçiminde içe aktarılan sunucu meta verilerini kullanarak makineleri değerlendirin.

## <a name="how-do-i-assess-with-the-appliance"></a>Cihazla nasıl değerlendirebilirim?

Şirket içi sunucuları keşfetmek için bir Azure Geçir cihazı dağıtıyorsanız, aşağıdaki adımları yapın:

1. Azure'u ve şirket içi ortamınızı Sunucu Değerlendirmesi ile çalışacak şekilde ayarlayın.
1. İlk değerlendirmeniz için bir Azure projesi oluşturun ve sunucu değerlendirme aracını ekleyin.
1. Hafif bir Azure Geçiş cihazı dağıtın. Cihaz sürekli olarak şirket içi makineleri keşfeder ve Azure Geçiş'e makine meta verileri ve performans verileri gönderir. Cihazı VM veya fiziksel bir makine olarak dağıtın. Değerlendirmek istediğiniz makinelere bir şey yüklemeniz gerekmez.

Cihaz makine keşfine başladıktan sonra, değerlendirmek istediğiniz makineleri bir gruba toplayabilir ve grup için bir değerlendirme çalıştırabilirsiniz.

Bu adımları denemek için [VMware,](tutorial-prepare-vmware.md) [Hyper-V](tutorial-prepare-hyper-v.md)veya [fiziksel sunucular](tutorial-prepare-physical.md) için eğitimlerimizi izleyin.

## <a name="how-do-i-assess-with-imported-data"></a>İçe aktarılan verilerle nasıl değerlendirebilirim?

Sunucuları bir CSV dosyası kullanarak değerlendiriyorsanız, bir cihaza ihtiyacınız yoktur. Bunun yerine, aşağıdaki adımları yapın:

1. Azure'u Sunucu Değerlendirmesi ile çalışacak şekilde ayarlayın.
1. İlk değerlendirmeniz için bir Azure projesi oluşturun ve sunucu değerlendirme aracını ekleyin.
1. Bir CSV şablonu indirin ve sunucu verilerini ekleyin.
1. Şablonu Sunucu Değerlendirmesi'ne aktarın.
1. Alma yla eklenen sunucuları keşfedin, bir gruba toplayın ve grup için bir değerlendirme çalıştırın.

## <a name="what-data-does-the-appliance-collect"></a>Cihaz hangi verileri topluyor?

Değerlendirme için Azure Geçir cihazını kullanıyorsanız, [VMware](migrate-appliance.md#collected-data---vmware) ve [Hyper-V](migrate-appliance.md#collected-data---hyper-v)için toplanan meta veriler ve performans verileri hakkında bilgi edinin.

## <a name="how-does-the-appliance-calculate-performance-data"></a>Cihaz performans verilerini nasıl hesaplar?

Cihazı keşif için kullanırsanız, aşağıdaki adımlarla bilgi işlem ayarları için performans verileri toplar:

1. Cihaz gerçek zamanlı bir numune noktası toplar.

    - **VMware VM'ler**: Her 20 saniyede bir örnek noktası toplanır.
    - **Hyper-V VM'ler**: Her 30 saniyede bir numune noktası toplanır.
    - **Fiziksel sunucular**: Her beş dakikada bir örnek noktası toplanır.

1. Cihaz, her 10 dakikada bir tek bir veri noktası oluşturmak için örnek noktaları birleştirir. Veri noktasını oluşturmak için cihaz tüm örneklerdeki en yüksek değerleri seçer. Daha sonra veri noktasını Azure'a gönderir.
1. Sunucu Değerlendirmesi, son bir ayın tüm 10 dakikalık veri noktalarını depolar.
1. Bir değerlendirme oluşturduğunuzda, Sunucu Değerlendirmesi hak elde etmek için kullanılacak uygun veri noktasını tanımlar. Tanımlama, *performans geçmişi* ve *yüzdelik kullanım*için yüzdelik değerleri ne kadar temel ehemdir.

    - Örneğin, performans geçmişi bir haftaysa ve yüzdelik kullanım yüzdelik 95 yüzdelik ise, Sunucu Değerlendirmesi geçen haftanın 10 dakikalık örnek puanlarını sıralar. Onları artan sırayla sıralar ve hak sahiplerinin yüzde 95'ini seçer.
    - 95. yüzdelik değer, herhangi bir aykırılığı göz ardı etmenizi sağlar, bu da yüzde 99'u seçtiyseniz dahil edilebilir.
    - Dönem için en yüksek kullanımı seçmek istiyorsanız ve herhangi bir aykırılık kaçırmak istemiyorsanız, yüzdelik kullanım için yüzde 99'unu seçin.

1. Bu değer, cihazın topladığı bu ölçümler için etkili performans kullanım verilerini elde etmek için konfor faktörü ile çarpılır:

    - CPU kullanımı
    - RAM kullanımı
    - Disk IOPS (okuma ve yazma)
    - Disk girişi (okuma ve yazma)
    - Ağ çıktısı (giriş ve çıkış)

## <a name="how-are-assessments-calculated"></a>Değerlendirmeler nasıl hesaplanır?

Sunucu Değerlendirmesi, değerlendirmeleri hesaplamak için şirket içi makinelerin meta verilerini ve performans verilerini kullanır. Azure Geçir cihazını dağıtirseniz, değerlendirme cihazın topladığı verileri kullanır. Ancak, bir CSV dosyası kullanarak içe aktarılan bir değerlendirme çalıştırıyorsanız, hesaplama için meta verileri sağlarsınız.

Hesaplamalar bu üç aşamada oluşur:

1. **Azure'a hazır olma durumunu hesaplayın**: Makinelerin Azure'a geçiş için uygun olup olmadığını değerlendirin.
1. **Boyutlandırma önerilerini hesaplama**: Hesaplamayı, depolamayı ve ağ boyutlandırmayı tahmin edin.
1. **Aylık maliyetleri hesaplama**: Geçişten sonra Makineleri Azure'da çalıştırmak için tahmini aylık işlem ve depolama maliyetlerini hesaplayın.

Hesaplamalar önceki sıradadır. Bir makine sunucusu yalnızca öncekinden geçerse daha sonraki bir aşamaya geçer. Örneğin, bir sunucu Azure hazır olma aşamasında başarısız olursa, Azure için uygun olmadığı işaretlenir. Boyutlandırma ve maliyet hesaplamaları bu sunucu için yapılmaz.

## <a name="whats-in-an-assessment"></a>Bir değerlendirme neleri içerir?

Sunucu Değerlendirmesi'ndeki bir değerlendirmede yer alan lar şunlardır:

Özellik | Ayrıntılar
--- | ---
**Hedef konum** | Geçirmek istediğiniz konum. Sunucu Değerlendirmesi şu anda şu hedef Azure bölgelerini destekler:<br/><br/> Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Kanada Orta, Kanada Doğu, Orta Hindistan, Orta ABD, Çin Doğu, Çin Kuzey, Doğu Asya, Doğu ABD, Doğu ABD 2, Almanya Orta, Almanya Kuzeydoğu, Japonya Doğu, Japonya Batı, Kore Orta, Kore Güney, Kuzey Orta ABD, Kuzey Avrupa, Güney Orta ABD, Güneydoğu Asya, Güney Hindistan, İngiltere Güney, İngiltere Batı, ABD Gov Arizona, ABD Gov Texas, ABD Gov Virginia , Batı Orta ABD, Batı Avrupa, Batı Hindistan, Batı ABD ve Batı ABD 2.
**Hedef depolama diski (boyutlandırma olarak)** | Azure'da depolama için kullanılacak disk türü. <br/><br/> Hedef depolama diskini Premium yönetilen, Standart SSD tarafından yönetilen veya Standart HDD tarafından yönetilen olarak belirtin.
**Hedef depolama diski (performans tabanlı boyutlandırma)** | Hedef depolama diskinin türünü otomatik, Premium yönetilen, Standart HDD tarafından yönetilen veya Standart SSD tarafından yönetilen olarak belirtir.<br/><br/> **Otomatik**: Disk önerisi, disklerin performans verilerine dayanır, yani IOPS ve iş kaynağı.<br/><br/>**Premium veya Standart**: Değerlendirme, seçilen depolama türü içinde bir disk SKU önerir.<br/><br/> %99,9'luk tek örnekli VM hizmet düzeyi sözleşmesi (SLA) istiyorsanız, Premium yönetilen diskler kullanmayı düşünün. Bu kullanım, değerlendirmedeki tüm disklerin Premium yönetilen diskler olarak tavsiye edilmesini sağlar.<br/><br/> Azure Geçiş yalnızca geçiş değerlendirmesi için yönetilen diskleri destekler.
**Azure Ayrılmış Sanal Makine Örnekleri** | Değerlendirmedeki maliyet tahminlerinin bunları dikkate alması için [ayrılmış örnekleri](https://azure.microsoft.com/pricing/reserved-vm-instances/) belirtir.<br/><br/> Azure Geçir şu anda Azure Rezerve Edilmiş VM Örneklerini yalnızca istediğiniz kadar öde teklifleri için destekler.
**Boyutlandırma kriterleri** | Azure VM'yi doğru boyutlandırmak için kullanılır.<br/><br/> Boyutlandırma veya performansa dayalı boyutlandırma olarak kullanın.
**Performans geçmişi** | Performans tabanlı boyutlandırma ile kullanılır. Performans geçmişi, performans verileri değerlendirildiğinde kullanılan süreyi belirtir.
**Yüzdebirlik kullanımı** | Performans tabanlı boyutlandırma ile kullanılır. Yüzdelik kullanım, hak kazandırma için kullanılan performans örneğinin yüzdelik değerini belirtir.
**VM serisi** | Hak kazanmak için göz önünde bulundurmak istediğiniz Azure VM serisi. Örneğin, Azure'da A serisi VM'lere ihtiyaç olan bir üretim ortamınız yoksa, A serisini dizi listesinden çıkarabilirsiniz.
**Konfor katsayısı** | Değerlendirme sırasında kullanılan arabellek. VM'ler için CPU, RAM, disk ve ağ kullanım verilerine uygulanır. Mevsimsel kullanım, kısa performans geçmişi ve gelecekteki kullanımdaki olası artışlar gibi sorunları hesaplar.<br/><br/> Örneğin, %20 kullanımlı 10 çekirdekli bir VM normalde iki çekirdekli vm ile sonuçlanır. 2.0 konfor faktörü ile, sonuç yerine dört çekirdekli VM olduğunu.
**Sunduğu** | Kaydolduğunuz [Azure teklifi.](https://azure.microsoft.com/support/legal/offer-details/) Sunucu Değerlendirmesi, bu teklifin maliyetini tahmin ediyor.
**Para birimi** | Hesabınızın faturalandırma birimi.
**İndirim (%)** | Azure teklifinin üzerine aboneolunan tüm indirimler. Varsayılan ayar, %0’dır.
**VM çalışma süresi** | Sürekli çalışmayacak olan Azure VM'leri için ay daki gün ve saat sayısı. Maliyet tahminleri bu süreyi temel alar.<br/><br/> Varsayılan değerler ayda 31 gün ve günde 24 saattir.
**Azure Hibrit Avantajı** | Yazılım güvenceniz olup olmadığını ve [Azure Karma Avantajı](https://azure.microsoft.com/pricing/hybrid-use-benefit/)için uygun olup olmadığını belirtir. Ayarın varsayılan değeri "Evet" varsa, Windows VM'ler için Windows dışındaki işletim sistemleri için Azure fiyatları dikkate alınır.

Sunucu Değerlendirmesi ile bir değerlendirme oluşturmak için [en iyi uygulamaları gözden geçirin.](best-practices-assessment.md)

## <a name="calculate-readiness"></a>Hazır olma durumunu hesaplama

Tüm makineler Azure'da çalıştırılamayan makinelere uygun değildir. Sunucu Değerlendirmesi tüm şirket içi makineleri değerlendirir ve onlara bir hazırlık kategorisi atar.

- **Azure'a Hazır**: Makine herhangi bir değişiklik yapmadan Azure'a olduğu gibi geçirilebilir. Azure'da tam Azure desteği yle başlar.
- **Azure için koşullu olarak hazır**: Makine Azure'da başlayabilir, ancak tam Azure desteğine sahip olmayabilir. Örneğin, Azure, Windows Server'ın eski bir sürümünü çalıştıran bir makineyi desteklemez. Bu makineleri Azure'a geçirmeden önce dikkatli olmalısınız. Herhangi bir hazırlık sorunlarını gidermek için, değerlendirme öneriyor düzeltme kılavuzu izleyin.
- **Azure için hazır değil**: Makine Azure'da başlatılamayacak. Örneğin, bir şirket içi makinenin diski 64 TB'den fazla depolarsa, Azure makineyi barındıramaz. Geçişten önce sorunu gidermek için düzeltme kılavuzunu izleyin.
- **Hazırlık bilinmiyor**: Azure Geçir, yetersiz meta veri nedeniyle makinenin hazır olup olmadığını belirleyemez.

Hazır olma durumunu hesaplamak için, Sunucu Değerlendirmesi aşağıdaki tablolarda özetlenen makine özelliklerini ve işletim sistemi ayarlarını gözden geçirir.

### <a name="machine-properties"></a>Makine özellikleri

Sunucu Değerlendirmesi, Azure'da çalışıp çalışmayacağını belirlemek için şirket içi bir VM'nin aşağıdaki özelliklerini gözden geçirir.

Özellik | Ayrıntılar | Azure hazırlık durumu
--- | --- | ---
**Önyükleme türü** | Azure, VM'leri UEFI ile değil, önyükleme türü BIOS ile destekler. | Önyükleme türü UEFI ise koşullu olarak hazır
**Çekirdekler** | Her makinenin en fazla 128 çekirdeği olmalıdır ve bu da Azure VM'nin desteklediği maksimum sayıdır.<br/><br/> Performans geçmişi varsa, Azure Geçir karşılaştırma için kullanılan çekirdekleri dikkate alır. Değerlendirme ayarları nda bir konfor faktörü belirtilmişse, kullanılan çekirdek sayısı konfor faktörü ile çarpılır.<br/><br/> Performans geçmişi yoksa, Azure Geçir, konfor faktörlerini uygulamadan ayrılan çekirdekleri kullanır. | Çekirdek sayısı limit dahilindeyse hazır
**Ram** | Her makinenin en fazla 3.892 GB RAM'i olmalıdır ve bu&nbsp;da 2 VM'Standard_M128m bir Azure M serisinin desteklediği maksimum boyuttur.<sup>2</sup> [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).<br/><br/> Performans geçmişi varsa, Azure Geçir karşılaştırma için kullanılan RAM'i dikkate alır. Bir konfor faktörü belirtilirse, kullanılan RAM konfor faktörü ile çarpılır.<br/><br/> Geçmiş yoksa, ayrılan RAM bir konfor faktörü uygulamadan kullanılır.<br/><br/> | RAM miktarı limit dahilindeyse hazır
**Depolama diski** | Bir diskin ayrılan boyutu en fazla 32 TB olmalıdır. Azure, Azure Ultra SSD diskleriyle 64 TB diskleri desteklese de, Azure Geçiş: Sunucu Değerlendirmesi şu anda 32 TB'yi disk boyutu sınırı olarak denetler, çünkü henüz Ultra SSD'yi desteklemez. <br/><br/> İşletim sistemi de dahil olmak üzere makineye iliştirilen disk sayısı 65 veya daha az olmalıdır. | Disk boyutu ve numarası sınırlar içindeyse hazır
**Ağ** | Bir makinenin en fazla 32 ağ arabirimi (NIC) bağlı olmalıdır. | NIC sayısı limit dahilindeyse hazır

### <a name="guest-operating-system"></a>Konuk işletim sistemi

Sunucu Değerlendirmesi, VM özelliklerini gözden geçirmenin yanı sıra, Azure'da çalışıp çalışmayacağını belirlemek için bir makinenin konuk işletim sistemini de inceler.

> [!NOTE]
> VMware VM'ler için konuk çözümlemesi işlemek için Sunucu Değerlendirmesi, vCenter Server'daki VM için belirtilen işletim sistemini kullanır. VMware üzerinde çalışan Linux VM'ler için, Sunucu Değerlendirmesi şu anda konuk işletim sistemi çekirdek sürümünü tanımlamaz.

Sunucu Değerlendirmesi, işletim sistemine dayalı Olarak Azure hazırlığını tanımlamak için aşağıdaki mantığı kullanır:

**İşletim sistemi** | **Şey** | **Azure hazırlık durumu**
--- | --- | ---
Windows Server 2016 ve tüm SP'ler | Azure tam destek sağlar. | Azure için hazır.
Windows Server 2012 R2 ve tüm SP'ler | Azure tam destek sağlar. | Azure için hazır.
Windows Server 2012 ve tüm SPs | Azure tam destek sağlar. | Azure için hazır.
Tüm SP'lerle Windows Server 2008 R2 | Azure tam destek sağlar.| Azure için hazır.
Windows Server 2008 (32 bit ve 64 bit) | Azure tam destek sağlar. | Azure için hazır.
Windows Server 2003 ve Windows Server 2003 R2 | Bu işletim sistemleri destek bitiş tarihlerini geçti ve Azure'da destek için özel [destek sözleşmesine (CSA)](https://aka.ms/WSosstatement) ihtiyaç duyuyor. | Azure için koşullu olarak hazır. Azure'a geçmeden önce işletim sistemi yükseltmeyi düşünün.
Windows 2000, Windows 98, Windows 95, Windows NT, Windows 3.1 ve MS-DOS | Bu işletim sistemleri destek bitiş tarihlerini geçti. Makine Azure'da başlayabilir, ancak Azure işletim sistemi desteği sağlamaz. | Azure için koşullu olarak hazır. Azure'a geçmeden önce işletim sistemi yükseltmenizi öneririz.
Windows 7, Windows 8 ve Windows 10 | Azure yalnızca Visual Studio aboneliği yle destek [sağlar.](https://docs.microsoft.com/azure/virtual-machines/windows/client-images) | Azure için koşullu olarak hazır.
Windows 10 Pro | Azure, [Multitenant Barındırma Hakları](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment) ile destek sağlar. | Azure için koşullu olarak hazır.
Windows Vista ve Windows XP Professional | Bu işletim sistemleri destek bitiş tarihlerini geçti. Makine Azure'da başlayabilir, ancak Azure işletim sistemi desteği sağlamaz. | Azure için koşullu olarak hazır. Azure'a geçmeden önce işletim sistemi yükseltmenizi öneririz.
Linux | Azure'un onayladığı [Linux işletim sistemlerine](../virtual-machines/linux/endorsed-distros.md) bakın. Diğer Linux işletim sistemleri Azure'da başlayabilir. Ancak Azure'a geçiş yapmadan önce işletim sistemi onaylı bir sürüme yükseltmenizi öneririz. | Sürüm onaylanırsa Azure için hazır olun.<br/><br/>Sürüm onaylanmazsa şartlı olarak hazır.
Oracle Solaris, Apple macOS ve FreeBSD gibi diğer işletim sistemleri | Azure bu işletim sistemlerini onaylamaz. Makine Azure'da başlayabilir, ancak Azure işletim sistemi desteği sağlamaz. | Azure için koşullu olarak hazır. Azure'a geçmeden önce desteklenen bir işletim sistemi yüklemenizi öneririz.  
vCenter Server'da **Diğer** olarak belirtilen işletim sistemi | Azure Geçir bu durumda işletim sistemi tanımlayamaz. | Bilinmeyen hazırlık. Azure'un VM içinde çalışan işletim sistemi'ni desteklediğinden emin olun.
32 bit işletim sistemleri | Makine Azure'da başlayabilir, ancak Azure tam destek sağlamayabilir. | Azure için koşullu olarak hazır. Azure'a geçmeden önce 64 bit işletim sistemi yükseltmeyi düşünün.

## <a name="calculating-sizing"></a>Boyutlandırma nın hesaplanması

Makine Azure için hazır olarak işaretlendikten sonra, Sunucu Değerlendirmesi boyutlandırma önerileri yapar. Bu öneriler Azure VM ve disk SKU'yu tanımlar. Boyutlandırma hesaplamaları, şirket içi boyutlandırma mı yoksa performansa dayalı boyutlandırma mı kullandığınıza bağlıdır.

### <a name="calculate-sizing-as-is-on-premises"></a>Boyutlandırmayı hesaplama (şirket içinde olduğu gibi)

 Şirket içi boyutlandırma kullanıyorsanız, Sunucu Değerlendirmesi VM'lerin ve disklerin performans geçmişini dikkate almaz.

- **İşlem boyutlandırma**: Sunucu Değerlendirmesi, şirket içinde ayrılan boyuta bağlı olarak bir Azure VM SKU tahsis eder.
- **Depolama ve disk boyutlandırma**: Sunucu Değerlendirmesi, değerlendirme özelliklerinde belirtilen depolama türüne bakar ve uygun disk türünü önerir. Olası depolama türleri Standart HDD, Standart SSD ve Premium'dir. Varsayılan depolama türü Premium'dur.
- **Ağ boyutlandırma**: Sunucu Değerlendirmesi, şirket içi makinedeki ağ bağdaştırıcısını dikkate alır.

### <a name="calculate-sizing-performance-based"></a>Boyutlandırmayı hesaplama (performans tabanlı)

Performans tabanlı boyutlandırma kullanıyorsanız, Sunucu Değerlendirmesi boyutlandırma önerilerini aşağıdaki gibi yapar:

- Sunucu Değerlendirmesi, Azure'daki VM boyutunu ve disk türünü tanımlamak için makinenin performans geçmişini dikkate alır.
- Bir CSV dosyası kullanarak sunucuları içe aktarıyorsanız, belirttiğiniz değerler kullanılır. Bu yöntem, şirket içi makineyi fazla tahsis ettiyseniz, kullanım düşükse ve maliyetlerden tasarruf etmek için Azure VM'yi doğru boyutlandırmak istiyorsanız özellikle yararlıdır.
- Performans verilerini kullanmak istemiyorsanız, boyutlandırma ölçütlerini önceki bölümde açıklandığı gibi şirket içinde olduğu gibi sıfırlayın.

#### <a name="calculate-storage-sizing"></a>Depolama boyutlandırmayı hesaplama

Depolama boyutlandırması için Azure Geçir, makineye bağlı her diski bir Azure diski ile eşlemeye çalışır. Boyutlandırma aşağıdaki gibi çalışır:

1. Sunucu Değerlendirmesi, gerekli toplam IOPS'yi almak için bir diskin IOPS okuma ve yazma sını ekler. Benzer şekilde, her diskin toplam iş elde etmek için okuma ve yazma iş bölümü değerlerini ekler.
1. Depolama türünü otomatik olarak belirttiyseniz, seçili tür etkili IOPS ve iş elde değerlerine dayanır. Sunucu Değerlendirmesi, diskin Azure'daki Standart HDD, Standart SSD veya Premium diskle eşlenip eşlenmeyeceğini belirler. Depolama türü bu disk türlerinden birine ayarlanmışsa, Sunucu Değerlendirmesi seçilen depolama türü içinde bir disk SKU bulmaya çalışır.
1. Diskler aşağıdaki gibi seçilir:
    - Sunucu Değerlendirmesi gerekli IOPS ve iş verime sahip bir disk bulamazsa, makineyi Azure için uygun olmayan olarak işaretler.
    - Sunucu Değerlendirmesi uygun diskler kümesi bulursa, değerlendirme ayarlarında belirtilen konumu destekleyen diskleri seçer.
    - Birden çok uygun disk varsa, Sunucu Değerlendirmesi en düşük maliyetle diski seçer.
    - Herhangi bir diskin performans verileri kullanılamıyorsa, yapılandırma diskboyutu Azure'da bir Standart SSD diski bulmak için kullanılır.

#### <a name="calculate-network-sizing"></a>Ağ boyutlandırmayı hesaplama

Sunucu Değerlendirmesi, şirket içi makineye bağlı ağ bağdaştırıcılarının sayısını ve gerekli performansını destekleyen bir Azure VM bulmaya çalışır.

- Şirket içi VM'nin etkili ağ performansını elde etmek için Sunucu Değerlendirmesi, tüm ağ bağdaştırıcılarında veri aktarım hızını makineden (ağ dışına) toplar. Daha sonra konfor faktörü uygular. Elde edilen değeri, gerekli ağ performansını desteklenebilen bir Azure VM bulmak için kullanır.
- Sunucu Değerlendirmesi, ağ performansının yanı sıra Azure VM'nin gerekli sayıda ağ bağdaştırıcısını destekleyip desteklemeyeceğini de dikkate alır.
- Ağ performansı verileri kullanılamıyorsa, Sunucu Değerlendirmesi yalnızca VM boyutlandırması için ağ bağdaştırıcısını dikkate alır.

#### <a name="calculate-compute-sizing"></a>İşlem boyutlandırmayı hesaplama

Depolama ve ağ gereksinimlerini hesapladıktan sonra, Sunucu Değerlendirmesi Azure'da uygun bir VM boyutu bulmak için CPU ve RAM gereksinimlerini dikkate alır.

- Azure Geçir, uygun bir Azure VM boyutu bulmak için etkili kullanılan çekirdeklere ve RAM'e bakar.
- Uygun boyut bulunmazsa, makine Azure için uygun olmadığı olarak işaretlenir.
- Uygun bir boyut bulunursa, Azure Geçiş depolama ve ağ hesaplamalarını uygular. Daha sonra son VM boyutu önerisi için konum ve fiyatlandırma katmanı ayarlarını uygular.
- Birden fazla uygun Azure sanal makinesi boyutu varsa, en düşük maliyetli olan önerilir.

## <a name="confidence-ratings-performance-based"></a>Güven derecelendirmeleri (performansa dayalı)

Azure Geçiş'teki her performans tabanlı değerlendirme, güven derecelendirmesiyle ilişkilidir. Derecelendirme bir (en düşük) ile beş (en yüksek) yıldız arasında değişir. Güven derecelendirmesi, Azure Geçiş'in sağladığı boyut önerilerinin güvenilirliğini tahmin edebilirsiniz.

- Güven derecelendirmesi bir değerlendirmeye atanır. Derecelendirme, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliğine dayanır.
- Performansa dayalı boyutlandırma için Sunucu Değerlendirmesi gereksinimleri:
    - CPU ve VM RAM için kullanım verileri.
    - VM'ye iliştirilen her disk için disk IOPS ve iş verisi.
    - VM'ye bağlı her ağ bağdaştırıcısı için performans tabanlı boyutlandırmayı işlemek için ağ G/Ç.

Bu kullanım numaralarından herhangi biri kullanılamıyorsa, boyut önerileri güvenilir olmayabilir.

> [!NOTE]
> Alınan bir CSV dosyası kullanılarak değerlendirilen sunucular için güven derecelendirmeleri atanmadı. Derecelendirmeler, şirket içi değerlendirme için de geçerli değildir.

### <a name="ratings"></a>Derecelendirmeler

Bu tablo, kullanılabilir veri noktalarının yüzdeye bağlı olarak değerlendirme güven derecelendirmelerini gösterir:

   **Veri noktalarının kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
   --- | ---
   0-20% | 1 yıldız
   21-40% | 2 yıldız
   41-60% | 3 yıldız
   61-80% | 4 yıldız
   81-100% | 5 yıldız

### <a name="low-confidence-ratings"></a>Düşük güven derecelendirmeleri

Bir değerlendirmenin düşük güven notu alabilecek birkaç nedeni şunlardır:

- Değerlendirmeyi oluşturduğunuz süre boyunca ortamınızın profilini oluşturmadınız. Örneğin, performans süresi bir güne ayarlanmış bir değerlendirme oluşturursanız, toplanan tüm veri noktaları için keşfetmeye başladıktan sonra en az bir gün beklemeniz gerekir.
- Bazı VM'ler, değerlendirmenin hesaplandığı süre içinde kapatıldı. Herhangi bir VM belirli bir süre için kapatılırsa, Sunucu Değerlendirmesi o dönem için performans verilerini toplayamaz.
- Değerlendirmenin hesaplandığı süre içinde bazı VM'ler oluşturuldu. Örneğin, geçen ayın performans geçmişi için bir değerlendirme oluşturduğunuzu, ancak bazı VM'lerin yalnızca bir hafta önce oluşturulduğunu varsayalım. Yeni VM'lerin performans geçmişi tam süre boyunca var olmaz.

> [!NOTE]
> Herhangi bir değerlendirmenin güven derecelendirmesi beş yıldızdan azsa, cihazın çevreprofilini belirlemesi için en az bir gün beklemenizi ve değerlendirmeyi yeniden hesaplamanızı öneririz. Aksi takdirde, performans tabanlı boyutlandırma güvenilmez olabilir. Bu durumda, değerlendirmeyi şirket içi boyutlandırmaya değiştirmenizi öneririz.

## <a name="calculate-monthly-costs"></a>Aylık maliyetleri hesaplama

Boyutlandırma önerileri tamamlandıktan sonra, Azure Geçirmi geçiş sonrası için işlem ve depolama maliyetlerini hesaplar.

- **İşlem maliyeti**: Azure Geçir, VM'nin aylık maliyetini hesaplamak için önerilen Azure VM boyutunu ve Azure Faturalandırma API'sini kullanır.

    Hesaplama dikkate alır:
    - İşletim sistemi
    - Yazılım güvencesi
    - Ayrılmış örnekler
    - VM çalışma süresi
    - Konum
    - Para birimi ayarları

    Sunucu Değerlendirmesi, toplam aylık işlem maliyetini hesaplamak için tüm makinelerde maliyeti toplar.

- **Depolama maliyeti**: Bir makinenin aylık depolama maliyeti, makineye bağlı tüm disklerin aylık maliyetinin toplanmasıyla hesaplanır.

    Sunucu Değerlendirmesi, tüm makinelerin depolama maliyetlerini toplayarak aylık depolama maliyetlerini hesaplar. Şu anda, hesaplama değerlendirme ayarlarında belirtilen teklifleri dikkate almaz.

Maliyetler değerlendirme ayarlarında belirtilen para biriminde görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

Değerlendirme oluşturmak için en iyi uygulamaları [gözden geçirin.](best-practices-assessment.md) 

- [VMware VM'ler,](tutorial-prepare-vmware.md) [Hyper-V VM'ler](tutorial-prepare-hyper-v.md)ve [fiziksel sunucular](tutorial-prepare-physical.md)için değerlendirme çalıştırma hakkında bilgi edinin.
- [CSV dosyasıyla alınan](tutorial-assess-import.md)sunucuları değerlendirme hakkında bilgi edinin.
- [Bağımlılık görselleştirmeyi](concepts-dependency-visualization.md)ayarlama hakkında bilgi edinin.
