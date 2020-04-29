---
title: Azure geçişi sunucu değerlendirmesi ile değerlendirmeler
description: Azure geçişi sunucu değerlendirmesinde değerlendirmeler hakkında bilgi edinin
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 2f76ea5f195be2914cdcdb4de9e93af38504d66e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81769933"
---
# <a name="assessments-in-azure-migrate-server-assessment"></a>Azure geçişi 'nde değerlendirmeler: Sunucu değerlendirmesi

Bu makalede, [Azure geçişi: Sunucu değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracında değerlendirmelere genel bakış sunulmaktadır. Araç, Azure 'a geçiş için şirket içi VMware sanal makinelerini, Hyper-V VM 'lerini ve fiziksel sunucuları değerlendirebilirler.

## <a name="whats-an-assessment"></a>Değerlendirme nedir?

Sunucu değerlendirmesi aracı ile bir değerlendirme, hazırlığı ölçer ve şirket içi sunucuları Azure 'a geçirme etkisini tahmin eder.

> [!NOTE]
> Azure Kamu 'da [desteklenen hedef](migrate-support-matrix.md#supported-geographies-azure-government) değerlendirme konumlarını gözden geçirin. Değerlendirmelere ait VM boyut önerilerinin, özellikle kamu bulut bölgeleri için VM serisini kullanabileceğini unutmayın. VM türleri hakkında [daha fazla bilgi edinin](https://azure.microsoft.com/global-infrastructure/services/?regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia&products=virtual-machines) .

## <a name="types-of-assessments"></a>Değerlendirme türleri

Sunucu değerlendirmesi ile oluşturduğunuz değerlendirmeler, verilerin bir zaman noktası anlık görüntüsüdür. Sunucu değerlendirmesi iki tür değerlendirme sağlar.

**Değerlendirme türü** | **Bilgileri** | **Veri**
--- | --- | ---
**Performans tabanlı** | Toplanan performans verilerine dayalı öneriler oluşturan değerlendirmeler | VM boyutu önerisi, CPU ve RAM kullanımı verilerine göre belirlenir.<br/><br/> Disk türü önerisi, saniye başına giriş/çıkış işlemi (ıOPS) ve şirket içi disklerin aktarım hızını temel alır. Disk türleri Azure Standart HDD, Azure Standart SSD ve Azure Premium disklerdir.
**Şirket içi olarak** | Öneriler oluşturmak için performans verilerini kullanmayan değerlendirmeler | VM boyutu önerisi, şirket içi VM boyutunu temel alır.<br/><br> Önerilen disk türü, değerlendirme için seçilen depolama türünü temel alır.

## <a name="how-do-i-run-an-assessment"></a>Nasıl yaparım? bir değerlendirme çalıştırmak istiyor musunuz?

Bir değerlendirme çalıştırmak için birkaç yol vardır.

- Hafif bir Azure geçişi gereci tarafından toplanan sunucu meta verilerini kullanarak makineleri değerlendirin. Gereç, şirket içi makineleri bulur. Daha sonra, Azure geçişi 'ne makine meta verilerini ve performans verilerini gönderir.
- Bir virgülle ayrılmış değerler (CSV) biçiminde içeri aktarılan sunucu meta verilerini kullanarak makineleri değerlendirin.

## <a name="how-do-i-assess-with-the-appliance"></a>Gereç ile Nasıl yaparım? değerlendirin?

Şirket içi sunucuları bulmaya yönelik bir Azure geçiş gereci dağıtıyorsanız, aşağıdaki adımları uygulayın:

1. Azure 'u ve şirket içi ortamınızı sunucu değerlendirmesi ile çalışacak şekilde ayarlayın.
1. İlk değerlendirmenizi için bir Azure projesi oluşturun ve sunucu değerlendirmesi aracını buna ekleyin.
1. Hafif bir Azure geçişi gereci dağıtın. Gereç, şirket içi makineleri sürekli bulur ve Azure geçişi 'ne makine meta verilerini ve performans verilerini gönderir. Gereci bir VM veya fiziksel makine olarak dağıtın. Değerlendirmek istediğiniz makinelere herhangi bir şey yüklemeniz gerekmez.

Gereç makine bulmayı başlattıktan sonra bir gruba değerlendirmek ve grup için bir değerlendirme çalıştırmak istediğiniz makineleri toplayabilirsiniz.

Bu adımları denemek için [VMware](tutorial-prepare-vmware.md), [Hyper-V](tutorial-prepare-hyper-v.md)veya [fiziksel sunucular](tutorial-prepare-physical.md) için öğreticilerimizi izleyin.

## <a name="how-do-i-assess-with-imported-data"></a>İçeri aktarılan verilerle Nasıl yaparım? değerlendirin?

Sunucuları bir CSV dosyası kullanarak değerlendiriyorsanız, bir gereç olması gerekmez. Bunun yerine, aşağıdaki adımları uygulayın:

1. Azure 'ı sunucu değerlendirmesi ile çalışacak şekilde ayarlayın.
1. İlk değerlendirmenizi için bir Azure projesi oluşturun ve sunucu değerlendirmesi aracını buna ekleyin.
1. Bir CSV şablonu indirin ve buna sunucu verileri ekleyin.
1. Şablonu sunucu değerlendirmesi içine aktarın.
1. İçeri aktarma ile eklenen sunucuları bulun, bunları bir gruba toplayın ve grup için bir değerlendirme çalıştırın.

## <a name="what-data-does-the-appliance-collect"></a>Gereç hangi verileri toplar?

Değerlendirme için Azure geçişi gereci kullanıyorsanız, [VMware](migrate-appliance.md#collected-data---vmware) ve [Hyper-V](migrate-appliance.md#collected-data---hyper-v)için toplanan meta veriler ve performans verileri hakkında bilgi edinin.

## <a name="how-does-the-appliance-calculate-performance-data"></a>Gereç performans verilerini nasıl hesaplar?

Bu gereci bulma işlemi için kullanıyorsanız, aşağıdaki adımlarla işlem ayarları için performans verilerini toplar:

1. Gereç gerçek zamanlı bir örnek noktası toplar.

    - **VMware VM 'leri**: her 20 saniyede bir örnek noktası toplanır.
    - **Hyper-V VM 'leri**: bir örnek nokta, her 30 saniyede bir toplanır.
    - **Fiziksel sunucular**: beş dakikada bir örnek nokta toplanır.

1. Gereç, her 10 dakikada bir tek bir veri noktası oluşturmak için örnek noktaları birleştirir. Veri noktasını oluşturmak için gereç tüm örneklerden tepe değerlerini seçer. Daha sonra veri noktasını Azure 'a gönderir.
1. Sunucu değerlendirmesi, geçen ay için 10 dakikalık tüm veri noktalarını depolar.
1. Bir değerlendirme oluşturduğunuzda sunucu değerlendirmesi, doğru bir şekilde kullanmak için kullanılacak uygun veri noktasını tanımlar. Kimlik, *performans geçmişi* ve *yüzdebirlik kullanımı*için yüzdebirlik değerlerini temel alır.

    - Örneğin, performans geçmişi bir hafta ise ve yüzdebirlik kullanımı 95. yüzdebirlik ise, sunucu değerlendirmesi geçen hafta için 10 dakikalık örnek noktaları sıralar. Bunları artan düzende sıralar ve doğru bir şekilde yapmak için 95. yüzdebirlik değerini seçer.
    - 95. yüzdebirlik değeri, 99. yüzdebirlik değerini seçtiğiniz takdirde dahil olabilen tüm aykırı değerleri yok saydığınızdan emin olmanızı sağlar.
    - Dönemin en yoğun kullanımını seçmek ve tüm aykırı değerleri kaçırmak istemiyorsanız, yüzdebirlik kullanımı için 99. yüzdebirlik ' ü seçin.

1. Bu değer, gerecin topladığı bu ölçümler için etkili performans kullanım verilerini elde etmek için, rahatlık faktörüyle çarpılır:

    - CPU kullanımı
    - RAM kullanımı
    - Disk ıOPS (okuma ve yazma)
    - Disk aktarım hızı (okuma ve yazma)
    - Ağ aktarım hızı (ın ve out)

## <a name="how-are-assessments-calculated"></a>Değerlendirmeler nasıl hesaplanır?

Sunucu değerlendirmesi, değerlendirmeleri hesaplamak için şirket içi makinelerin meta verilerini ve performans verilerini kullanır. Azure geçişi gerecini dağıtırsanız, değerlendirme gereç tarafından toplanan verileri kullanır. Ancak bir CSV dosyası kullanarak içeri aktarılan bir değerlendirmeyi çalıştırırsanız, hesaplama için meta verileri sağlarsınız.

Hesaplamalar şu üç aşamada gerçekleşir:

1. **Azure hazırlığını hesapla**: makinelerin Azure 'a geçiş için uygun olup olmadığını değerlendirin.
1. **Boyutlandırma önerilerini hesapla**: işlem, depolama ve ağ boyutlandırmayı tahmin etme.
1. **Aylık maliyetleri hesapla**: geçiş sonrasında Azure 'da makineleri çalıştırmaya yönelik tahmini aylık işlem ve depolama maliyetlerini hesaplayın.

Hesaplamalar önceki sıradadır. Bir makine sunucusu, yalnızca öncekini geçerse daha sonraki bir aşamaya geçer. Örneğin, bir sunucu Azure hazırlık aşamasına geçemezse Azure için uygun değil olarak işaretlenir. Bu sunucu için boyutlandırma ve maliyet hesaplamaları yapılmaz.

## <a name="whats-in-an-assessment"></a>Bir değerlendirme neleri içerir?

Sunucu değerlendirmesinde bir değerlendirmeye dahil edilmiştir:

Özellik | Ayrıntılar
--- | ---
**Hedef konum** | Geçirmek istediğiniz konum. Sunucu değerlendirmesi Şu anda bu hedef Azure bölgelerini destekliyor:<br/><br/> Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Kanada Orta, Kanada Doğu, Orta Hindistan, Orta ABD, Çin Doğu, Çin Kuzey, Doğu Asya, Doğu ABD, Doğu ABD 2, Almanya Orta, Almanya Kuzeydoğu, Japonya Doğu, Japonya Batı, Kore Orta, Kore Güney, Orta Kuzey ABD, Kuzey Avrupa, Orta Güney ABD, Güneydoğu Asya, Güney Hindistan, UK Güney, UK Batı, US gov Arizona, US Gov Teksas, US Gov Virginia , Orta Batı ABD, Batı Avrupa, Batı Hindistan, Batı ABD ve Batı ABD 2.
**Hedef depolama diski (örneğin, boyutlandırma)** | Azure 'da depolama için kullanılacak disk türü. <br/><br/> Hedef depolama diskini Premium tarafından yönetilen, Standart SSD yönetilen veya Standart HDD yönetilen olarak belirtin.
**Hedef depolama diski (performans tabanlı boyutlandırma)** | Hedef depolama diskinin türünü otomatik, Premium tarafından yönetilen, Standart HDD yönetilen veya Standart SSD yönetilen olarak belirtir.<br/><br/> **Otomatik**: disk önerisi, disklerin performans verilerine göre BELIRLENIR, IOPS ve aktarım hızı anlamına gelir.<br/><br/>**Premium veya standart**: değerlendirme, seçilen depolama türü içinde bir disk SKU 'su önerir.<br/><br/> % 99,9 için tek örnekli bir VM hizmet düzeyi sözleşmesi (SLA) istiyorsanız Premium ile yönetilen diskler kullanmayı düşünün. Bu kullanım, değerlendirmede tüm disklerin Premium yönetilen diskler olarak önerilmesini sağlar.<br/><br/> Azure geçişi, geçiş değerlendirmesi için yalnızca yönetilen diskleri destekler.
**Azure ayrılmış sanal makine örnekleri** | Değerlendirmede maliyet tahminleri bunları hesaba alacak şekilde [ayrılmış örnekleri](https://azure.microsoft.com/pricing/reserved-vm-instances/) belirtir.<br/><br/> Azure geçişi Şu anda Azure ayrılmış VM örneklerini yalnızca Kullandıkça Öde teklifleri için desteklemektedir.
**Boyutlandırma ölçütü** | Azure VM 'yi farenin altına almak için kullanılır.<br/><br/> As, boyutlandırma veya performans tabanlı boyutlandırma kullanın.
**Performans geçmişi** | Performans tabanlı boyutlandırma ile kullanılır. Performans geçmişi performans verileri değerlendirilirken kullanılan süreyi belirtir.
**Yüzdebirlik kullanımı** | Performans tabanlı boyutlandırma ile kullanılır. Yüzdebirlik kullanımı, doğru hale getirmek için kullanılan performans örneğinin yüzdebirlik değerini belirtir.
**VM serisi** | Doğru hale getirmek için göz önünde bulundurulması istediğiniz Azure VM Serisi. Örneğin, Azure 'da bir serisi VM gerektiren bir üretim ortamınız yoksa, seri listesinden bir serisi dışarıda bırakabilirsiniz.
**Konfor katsayısı** | Değerlendirme sırasında kullanılan arabellek. VM 'Ler için CPU, RAM, disk ve ağ kullanım verilerine uygulanır. Dönemsel kullanım, kısa performans geçmişi ve gelecekteki kullanımlarda olası artışlar gibi sorunlar için BT hesapları.<br/><br/> Örneğin, %20 kullanımındaki bir 10 çekirdekli VM normalde iki çekirdekli bir VM ile sonuçlanır. 2,0, bir BT faktörü ile bunun yerine dört çekirdekli bir VM olur.
**Sunduğu** | Kayıtlı olduğunuz [Azure teklifi](https://azure.microsoft.com/support/legal/offer-details/) . Sunucu değerlendirmesi, bu teklifin maliyetini tahmin eder.
**Birimindeki** | Hesabınız için faturalandırma para birimi.
**İndirim (%)** | Azure teklifinin üzerine aldığınız, aboneliğe özgü tüm indirimler. Varsayılan ayar, %0’dır.
**VM çalışma süresi** | Sürekli olarak çalışmayan Azure VM 'Leri için her ay gün ve saat başına gün cinsinden süre. Maliyet tahminleri bu süreye göre hesaplanır.<br/><br/> Varsayılan değerler ayda 31 gün ve günde 24 saat değerlerdir.
**Azure Hibrit Avantajı** | Yazılım Güvencesi olup olmadığını ve [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-use-benefit/)uygun olduğunu belirtir. Ayarın varsayılan değeri "Evet" ise, Windows dışındaki işletim sistemleri için Azure fiyatları Windows VM 'Leri olarak kabul edilir.

Sunucu değerlendirmesi ile bir değerlendirme oluşturmak için [en iyi uygulamaları gözden geçirin](best-practices-assessment.md) .

## <a name="calculate-readiness"></a>Hazırlığı hesapla

Tüm makineler Azure 'da çalışmak üzere uygun değildir. Sunucu değerlendirmesi tüm şirket içi makineleri değerlendirir ve onlara bir hazır olma kategorisi atar.

- **Azure Için hazırlanma**: makine, hiçbir değişiklik yapılmadan Azure 'a olarak geçirilebilir. Azure 'da tam Azure desteğiyle başlatılır.
- **Azure Için koşullu olarak hazırlanıyor**: makine Azure 'da başlayabilir, ancak tam Azure desteği olmayabilir. Örneğin, Azure, Windows Server 'ın eski bir sürümünü çalıştıran bir makineyi desteklemez. Bu makineleri Azure 'a geçirmeden önce dikkatli olmanız gerekir. Tüm hazırlık sorunlarını onarmak için değerlendirmenin önerdiği düzeltme kılavuzunu izleyin.
- **Azure için hazırlanma**: makine Azure 'da başlamıyor. Örneğin, bir şirket içi makinenin diski 64 TB 'tan fazla mağaza içeriyorsa, Azure makineyi barındırmıyor. Geçişten önce sorunu gidermek için düzeltme kılavuzunu izleyin.
- **Hazır olma bilinmiyor**: Azure geçişi, yetersiz meta veri nedeniyle makinenin hazır olup olmadığını belirleyemiyor.

Sunucu değerlendirmesi, hazırlığı hesaplamak için aşağıdaki tablolarda özetlenen makine özelliklerini ve işletim sistemi ayarlarını gözden geçirir.

### <a name="machine-properties"></a>Makine özellikleri

Sunucu değerlendirmesi, Azure üzerinde çalışıp çalışmadığını anlamak için bir şirket içi VM 'nin aşağıdaki özelliklerini inceler.

Özellik | Ayrıntılar | Azure hazırlık durumu
--- | --- | ---
**Önyükleme türü** | Azure, UEFı değil, BIOS 'un önyükleme türü olan VM 'Leri destekler. | Önyükleme türü UEFı ise koşullu olarak hazırlanıyor
**Çekirdekler** | Her makinede en fazla 128 çekirdek olmalıdır; bu, bir Azure VM 'nin desteklediği maksimum sayıdır.<br/><br/> Performans geçmişi varsa, Azure geçişi karşılaştırma için kullanılan çekirdekleri dikkate alır. Değerlendirme ayarları bir rahatetken belirtirseniz, kullanılan çekirdek sayısı, rahatlık faktörüyle çarpılarak çarpılır.<br/><br/> Hiçbir performans geçmişi yoksa, Azure geçişi, rakip etmenini uygulamadan ayrılmış çekirdekleri kullanır. | Çekirdek sayısı sınırın içindeyse, hazırlanıyor
**KOÇ** | Her makinede 3.892 GB 'den fazla RAM olması gerekir. bu boyut, en büyük boyut olan bir Azure ı serisi Standard_M128m&nbsp;<sup>2</sup> VM destekler. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).<br/><br/> Performans geçmişi varsa, Azure geçişi karşılaştırma için kullanılan RAM 'i dikkate alır. Bir rakip faktörü belirtilmişse, kullanılan RAM, rahatlık faktörüyle çarpılarak çarpılır.<br/><br/> Geçmiş yoksa, ayrılan RAM bir rahatörün uygulaması olmadan kullanılır.<br/><br/> | RAM miktarı sınırın içindeyse hazırlanıyor
**Depolama diski** | Bir diskin ayrılan boyutu 32 TB 'tan fazla olmamalıdır. Azure, Azure Ultra SSD diskleriyle 64 TB diskleri destekler, ancak Azure geçişi: Sunucu değerlendirmesi Şu anda Ultra SSD desteklemediği için, disk boyutu sınırı olarak 32 TB 'yi kontrol eder. <br/><br/> İşletim sistemi diski de dahil olmak üzere makineye bağlı disk sayısı 65 veya daha az olmalıdır. | Disk boyutu ve sayı limitlerin içindeyse,
**Ağ** | Bir makinenin kendisine bağlı 32 ' den fazla ağ arabirimi (NIC) olmaması gerekir. | NIC sayısı sınırın içindeyse, hazırlanıyor

### <a name="guest-operating-system"></a>Konuk işletim sistemi

Sunucu değerlendirmesi, sanal makine özelliklerini gözden geçirmede bir makinenin Konuk işletim sistemine bakar ve Azure üzerinde çalışıp çalışmadığını tespit edebilir.

> [!NOTE]
> VMware VM 'lerinin Konuk analizini işlemek için sunucu değerlendirmesi, vCenter Server içinde VM için belirtilen işletim sistemini kullanır. VMware üzerinde çalışan Linux VM 'Leri için sunucu değerlendirmesi Şu anda Konuk işletim sisteminin çekirdek sürümünü tanımlamıyor.

Sunucu değerlendirmesi, işletim sistemine bağlı olarak Azure hazırlığını belirlemek için aşağıdaki mantığı kullanır:

**İşletim sistemi** | **Bilgileri** | **Azure hazırlık durumu**
--- | --- | ---
Windows Server 2016 ve tüm SPs 'ler | Azure tam destek sağlar. | Azure için hazırlayın.
Windows Server 2012 R2 ve tüm SPs 'ler | Azure tam destek sağlar. | Azure için hazırlayın.
Windows Server 2012 ve tüm SPs 'ler | Azure tam destek sağlar. | Azure için hazırlayın.
Tüm SPs 'ler ile Windows Server 2008 R2 | Azure tam destek sağlar.| Azure için hazırlayın.
Windows Server 2008 (32-bit ve 64-bit) | Azure tam destek sağlar. | Azure için hazırlayın.
Windows Server 2003 ve Windows Server 2003 R2 | Bu işletim sistemleri destek son tarihlerini geçti ve Azure 'da destek için [özel bir destek sözleşmesine (CSA)](https://aka.ms/WSosstatement) ihtiyaç duyuyor. | Azure için koşullu olarak hazırlanın. Azure 'a geçiş yapmadan önce işletim sistemini yükseltmeniz göz önünde bulundurun.
Windows 2000, Windows 98, Windows 95, Windows NT, Windows 3,1 ve MS-DOS | Bu işletim sistemleri destek son tarihlerini geçti. Makine Azure 'da başlayabilir, ancak Azure işletim sistemi desteği sağlamaz. | Azure için koşullu olarak hazırlanın. Azure 'a geçiş yapmadan önce işletim sistemini yükseltmenizi öneririz.
Windows 7, Windows 8 ve Windows 10 | Azure [yalnızca bir Visual Studio aboneliği](https://docs.microsoft.com/azure/virtual-machines/windows/client-images) için destek sağlar. | Azure için koşullu olarak hazırlanın.
Windows 10 Pro | Azure, [çok kiracılı barındırma haklarıyla](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment) destek sağlar. | Azure için koşullu olarak hazırlanın.
Windows Vista ve Windows XP Professional | Bu işletim sistemleri destek son tarihlerini geçti. Makine Azure 'da başlayabilir, ancak Azure işletim sistemi desteği sağlamaz. | Azure için koşullu olarak hazırlanın. Azure 'a geçiş yapmadan önce işletim sistemini yükseltmenizi öneririz.
Linux | Azure 'un onaylayabileceği [Linux işletim sistemlerine](../virtual-machines/linux/endorsed-distros.md) bakın. Diğer Linux işletim sistemleri Azure 'da başlayabilir. Ancak, Azure 'a geçiş yapmadan önce işletim sistemini onaylı bir sürüme yükseltmenizi öneririz. | Sürüm onaylanmış ise Azure için hazırlanın.<br/><br/>Sürüm onaymadıysa koşullu olarak kabul edilebilir.
Oracle Solaris, Apple macOS ve FreeBSD gibi diğer işletim sistemleri | Azure, bu işletim sistemlerini onaylamaz. Makine Azure 'da başlayabilir, ancak Azure işletim sistemi desteği sağlamaz. | Azure için koşullu olarak hazırlanın. Azure 'a geçiş yapmadan önce desteklenen bir işletim sistemi yüklemenizi öneririz.  
VCenter Server içinde **diğeri** olarak belirtilen işletim sistemi | Azure geçişi bu durumda işletim sistemini tanımlayamıyor. | Bilinmeyen hazırlık. Azure 'un VM içinde çalışan işletim sistemini desteklediğinden emin olun.
32 bit işletim sistemleri | Makine Azure 'da başlayabilir, ancak Azure tam destek sunmayabilir. | Azure için koşullu olarak hazırlanın. Azure 'a geçiş yapmadan önce 64 bitlik bir IŞLETIM sistemine yükseltmeyi göz önünde bulundurun.

## <a name="calculating-sizing"></a>Boyutlandırma hesaplanıyor

Makine Azure için hazırlanıyor olarak işaretlendikten sonra, sunucu değerlendirmesi boyutlandırma önerilerini sağlar. Bu öneriler, Azure VM ve disk SKU 'sunu belirler. Boyutlandırma hesaplamaları, şirket içi boyutlandırma veya performans tabanlı boyutlandırmanın kullanılmasına bağlı olarak değişir.

### <a name="calculate-sizing-as-is-on-premises"></a>Boyutlandırmayı hesapla (Şirket içi olarak)

 Şirket içi olarak boyutlandırma kullanıyorsanız, sunucu değerlendirmesi VM 'Lerin ve disklerin performans geçmişini dikkate almaz.

- **İşlem boyutlandırma**: Sunucu değerlendirmesi, şirket içinde ayrılan boyuta göre BIR Azure VM SKU 'su ayırır.
- **Depolama ve disk boyutlandırma**: Sunucu değerlendirmesi, değerlendirme özelliklerinde belirtilen depolama türüne bakar ve uygun disk türünü önerir. Olası depolama türleri Standart HDD, Standart SSD ve Premium ' dur. Varsayılan depolama türü Premium ' dur.
- **Ağ boyutlandırma**: Sunucu değerlendirmesi, ağ bağdaştırıcısını şirket içi makinede değerlendirir.

### <a name="calculate-sizing-performance-based"></a>Boyutlandırmayı hesapla (performans tabanlı)

Performans tabanlı boyutlandırma kullanırsanız, sunucu değerlendirmesi, boyutlandırma önerilerini aşağıdaki gibi yapar:

- Sunucu değerlendirmesi, Azure 'da VM boyutunu ve disk türünü tanımlamak için makinenin performans geçmişini dikkate alır.
- Sunucuları bir CSV dosyası kullanarak içeri aktarırsanız, belirttiğiniz değerler kullanılır. Bu yöntem özellikle şirket içi makinenin fazla yüklenmiş olması, kullanımın düşük olması ve Azure VM 'nin maliyetleri kazanmak için sağ boyutunu kullanmak istediğinizde yararlıdır.
- Performans verilerini kullanmak istemiyorsanız, önceki bölümde açıklandığı gibi boyutlandırma ölçütünü şirket içinde olduğu gibi olarak sıfırlayın.

#### <a name="calculate-storage-sizing"></a>Depolama boyutunu hesapla

Azure geçişi, depolama boyutu için makineye bağlı her diski bir Azure diskine eşlemeye çalışır. Boyutlandırma aşağıdaki gibi çalışmaktadır:

1. Sunucu değerlendirmesi, gereken toplam ıOPS 'yi almak için bir diskin okuma ve yazma ıOPS 'sini ekler. Benzer şekilde, her diskin toplam aktarım hızını almak için okuma ve yazma aktarım hızı değerlerini ekler.
1. Depolama türünü otomatik olarak belirttiyseniz, seçilen tür, etkin ıOPS ve aktarım hızı değerlerini temel alır. Sunucu değerlendirmesi, diskin Azure 'da bir Standart HDD, Standart SSD veya Premium diskle eşlenip eşlenmeyeceğini belirler. Depolama türü bu disk türlerinden birine ayarlanırsa, sunucu değerlendirmesi seçilen depolama türü içinde bir disk SKU 'SU bulmaya çalışır.
1. Diskler şu şekilde seçilir:
    - Sunucu değerlendirmesi gereken ıOPS ve aktarım hızına sahip bir disk bulamazsa, makineyi Azure için uygun değil olarak işaretler.
    - Sunucu değerlendirmesi bir dizi uygun disk bulursa, değerlendirme ayarlarında belirtilen konumu destekleyen diskleri seçer.
    - Birden fazla uygun disk varsa, sunucu değerlendirmesi en düşük maliyetli diski seçer.
    - Herhangi bir disk için performans verileri kullanılamıyorsa, Azure 'da Standart SSD disk bulmak için yapılandırma disk boyutu kullanılır.

#### <a name="calculate-network-sizing"></a>Ağ boyutlandırmayı hesapla

Sunucu değerlendirmesi, şirket içi makineye bağlı ağ bağdaştırıcılarının sayısını ve gereken performansını destekleyen bir Azure VM bulmaya çalışır.

- Şirket içi VM 'nin etkili ağ performansını almak için, sunucu değerlendirmesi tüm ağ bağdaştırıcılarında makinenin (ağ üzerinden) veri iletimi hızını toplar. Daha sonra, rahatlık faktörünü uygular. Bu, gereken ağ performansını destekleyebilen bir Azure VM bulmak için elde edilen değeri kullanır.
- Sunucu değerlendirmesi, ağ performansının yanı sıra Azure VM 'nin gerekli sayıda ağ bağdaştırıcısını destekleyip desteklemediğini de dikkate alır.
- Ağ performansı verileri kullanılamazsa, sunucu değerlendirmesi yalnızca VM boyutlandırma için ağ bağdaştırıcısı sayısını dikkate alır.

#### <a name="calculate-compute-sizing"></a>İşlem boyutunu hesapla

Depolama ve ağ gereksinimlerini hesapladıktan sonra sunucu değerlendirmesi, Azure 'da uygun bir VM boyutu bulmak için CPU ve RAM gereksinimlerini dikkate alır.

- Azure geçişi, uygun bir Azure VM boyutu bulmak için etkin kullanılan çekirdekleri ve RAM 'i arar.
- Uygun boyut bulunamazsa, makine Azure için uygun olmayan olarak işaretlenir.
- Uygun bir boyut bulunursa Azure geçişi depolama ve ağ hesaplamalarını uygular. Ardından, son VM boyutu önerisi için konum ve fiyatlandırma katmanı ayarlarını uygular.
- Birden fazla uygun Azure sanal makinesi boyutu varsa, en düşük maliyetli olan önerilir.

## <a name="confidence-ratings-performance-based"></a>Güven derecelendirmeleri (performans tabanlı)

Azure geçişi 'ndeki her performans tabanlı değerlendirme, güvenirlik derecelendirmesi ile ilişkilendirilir. Derecelendirme, bir (en düşük) ile beş (en yüksek) yıldıza göre değişir. Güvenilirlik derecelendirmesi, Azure geçişi 'nin sağladığı önerilerin boyutunu tahmin etmenize yardımcı olur.

- Güvenirlik derecelendirmesi bir değerlendirmeye atanır. Derecelendirme, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliğine bağlıdır.
- Performans tabanlı boyutlandırma için sunucu değerlendirmesi şunları gerektirir:
    - CPU ve VM RAM için kullanım verileri.
    - SANAL makineye bağlı her diske yönelik disk ıOPS ve aktarım hızı verileri.
    - Bir VM 'ye bağlı her bir ağ bağdaştırıcısı için performans tabanlı boyutlandırmayı işlemek üzere ağ g/ç.

Bu kullanım numaralarının herhangi biri kullanılabilir değilse, boyut önerileri güvenilir olmayabilir.

> [!NOTE]
> Güven derecelendirmeleri, içeri aktarılan bir CSV dosyası kullanılarak değerlendirilen sunucular için atanmaz. Derecelendirmeler Ayrıca, şirket içi değerlendirme için de geçerli değildir.

### <a name="ratings"></a>Derecelendirmeler

Bu tabloda, kullanılabilir veri noktalarının yüzdesine bağlı olan değerlendirme güvenilirliği derecelendirmeleri gösterilmektedir:

   **Veri noktalarının kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
   --- | ---
   % 0-20 | 1 yıldız
   % 21-40 | 2 yıldız
   % 41-60 | 3 yıldız
   % 61-80 | 4 yıldız
   % 81-100 | 5 yıldız

### <a name="low-confidence-ratings"></a>Düşük güvenilirlikli derecelendirmeler

Bir değerlendirmenin en düşük güvenilirlik derecelendirmesinin neden olmasının birkaç nedeni aşağıda verilmiştir:

- Değerlendirmeyi oluşturmakta olduğunuz süre için ortamınızı profildiniz. Örneğin, performans süresi ile bir gün ayarlanmış bir değerlendirme oluşturursanız, toplanan tüm veri noktaları için bulmayı başlattıktan sonra en az bir gün beklemeniz gerekir.
- Bazı sanal makineler, değerlendirmenin hesaplanmakta olduğu süre boyunca kapatıldı. Bir süre için herhangi bir VM kapatılmışsa, sunucu değerlendirmesi söz konusu döneme ait performans verilerini toplayamazlar.
- Değerlendirmenin hesaplanışında bazı sanal makineler oluşturulmuştur. Örneğin, geçen ayın performans geçmişi için bir değerlendirme oluşturdunuz, ancak bazı VM 'Ler yalnızca bir hafta önce oluşturulmuştur. Yeni VM 'lerin performans geçmişi, tamamlanma süresi boyunca mevcut olmayacaktır.

> [!NOTE]
> Herhangi bir değerlendirmenin güvenilirlik derecelendirmesi beş yıldızlı günden azsa, gerecin ortamın profilini oluşturup daha sonra değerlendirmeyi yeniden hesaplaması için en az bir gün beklemeniz önerilir. Aksi takdirde, performans tabanlı boyutlandırma güvenilir olmayabilir. Bu durumda, değerlendirmeyi şirket içi boyutlandırılmasına geçmeniz önerilir.

## <a name="calculate-monthly-costs"></a>Aylık maliyetleri hesapla

Boyutlandırma önerileri tamamlandıktan sonra, Azure geçişi geçişten sonra işlem ve depolama maliyetlerini hesaplar.

- **İşlem maliyeti**: Azure geçişi, sanal makinenin aylık maliyetini hesaplamak Için ÖNERILEN Azure VM boyutunu ve Azure Faturalandırma API 'sini kullanır.

    Hesaplama şu şekilde hesaba girer:
    - İşletim sistemi
    - Yazılım Güvencesi
    - Ayrılmış örnekler
    - VM çalışma süresi
    - Konum
    - Para birimi ayarları

    Sunucu değerlendirmesi, toplam aylık işlem maliyetini hesaplamak için tüm makinelerdeki maliyeti toplar.

- **Depolama maliyeti**: bir makine için aylık depolama maliyeti, makineye bağlı tüm disklerin aylık maliyeti toplayarak hesaplanır.

    Sunucu değerlendirmesi, tüm makinelerin depolama maliyetlerini toplayarak aylık toplam depolama maliyetlerini hesaplar. Şu anda hesaplama, değerlendirme ayarlarında belirtilen teklifleri göz önünde bulundurmaz.

Ücretler, değerlendirme ayarlarında belirtilen para biriminde görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

Değerlendirme oluşturmak için en iyi uygulamaları [gözden geçirin](best-practices-assessment.md) . 

- [VMware VM 'leri](tutorial-prepare-vmware.md), [Hyper-V VM 'leri](tutorial-prepare-hyper-v.md)ve [fiziksel sunucular](tutorial-prepare-physical.md)için değerlendirme çalıştırma hakkında bilgi edinin.
- [BIR CSV dosyası ile içeri aktarılan](tutorial-assess-import.md)sunucuları değerlendirme hakkında bilgi edinin.
- [Bağımlılık görselleştirmesini](concepts-dependency-visualization.md)ayarlama hakkında bilgi edinin.
