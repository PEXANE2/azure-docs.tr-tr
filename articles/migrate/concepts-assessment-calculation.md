---
title: Azure geçişi 'nde değerlendirmeler
description: Azure geçişi 'nde değerlendirmeler hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: 0cf933dd1c8c61edfcea20ea954c5813f3848b28
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79269698"
---
# <a name="about-assessments-in-azure-migrate"></a>Azure geçişi 'nde değerlendirmeler hakkında

Bu makalede, [Azure geçişi: Sunucu değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool)' nde değerlendirmelerin nasıl hesaplandığı açıklanır. Azure geçişine geçiş için hazır olup olmadığını anlamak üzere, şirket içi makinelerin grupları üzerinde değerlendirmeler çalıştırırsınız.

## <a name="how-do-i-run-an-assessment"></a>Nasıl yaparım? bir değerlendirme çalıştırmak istiyor musunuz?
Azure geçişi: Sunucu değerlendirmesi veya başka bir Azure ya da üçüncü taraf aracı kullanarak bir değerlendirme gerçekleştirebilirsiniz. Bir Azure geçişi projesi oluşturduktan sonra, ihtiyacınız olan aracı eklersiniz. [Daha fazla bilgi](how-to-add-tool-first-time.md)

### <a name="collect-compute-data"></a>İşlem verilerini topla

İşlem ayarları için performans verileri aşağıdaki şekilde toplanır:

1. [Azure geçişi](migrate-appliance.md) gereci gerçek zamanlı bir örnek noktası toplar:

    - **VMware VM 'leri**: VMware VM 'Leri Için Azure geçişi gereci her 20 saniyelik aralığa göre gerçek zamanlı bir örnek noktası toplar.
    - **Hyper-v VM 'leri**: Hyper-v VM 'leri için, gerçek zamanlı örnek noktası her 30 saniyelik aralığa göre toplanır.
    - **Fiziksel sunucular**: fiziksel sunucular için gerçek zamanlı örnek noktası her beş dakikalık aralıkta toplanır. 
    
2. Gereç, her 10 dakikada bir tek bir veri noktası oluşturmak için örnek noktalara (20 saniye, 30 saniye, beş dakika) kaydolur. Tek bir veri noktası oluşturmak için, Gereç tüm örneklerden tepe değeri seçer ve ardından bunu Azure 'a gönderir.
3. Sunucu değerlendirmesi, geçen ay için 10 dakikalık tüm örnek noktalarını depolar.
4. Bir değerlendirme oluşturduğunuzda, sunucu değerlendirmesi, *performans geçmişi* ve *yüzdebirlik kullanımı*için yüzdelik değerler temelinde, doğru boyutlandırma için kullanılacak uygun veri noktasını belirler.

    - Örneğin, performans geçmişi bir hafta olarak ayarlandıysa ve yüzdebirlik kullanımı 95. yüzdebirlik ise, sunucu değerlendirmesi son hafta için 10 dakikalık örnek noktalarını artan düzende sıralar ve sağ boyutlandırma için 95. yüzdebirlik değerini seçer. 
    - 95. yüzdebirlik değeri, 99. yüzdebirlik değerini seçerseniz dahil edilen tüm aykırı değerleri yok saydığınızdan emin olmanızı sağlar.
    - Dönemin en yoğun kullanımını seçmek ve tüm aykırı değerleri kaçırmak istemiyorsanız, yüzdebirlik kullanımı için 99. yüzdebirlik ' ü seçmeniz gerekir.

5. Bu değer, her ölçüm için etkili performans kullanım verilerini (CPU kullanımı, bellek kullanımı, disk ıOPS (okuma ve yazma), disk aktarım hızı (okuma ve yazma) ve ağ aktarım hızını (ın ve out) almak için rahatlık faktörüyle çarpılır. Gereç toplar.

Sunucu değerlendirmesinde değerlendirmeleri çalıştırmak için, şirket içi ve Azure 'da değerlendirmeye hazırlandınız ve şirket içi makineleri sürekli olarak keşfetmeye yönelik Azure geçiş gereci ' nı ayarlayın. Makineler bulunduktan sonra bunları değerlendirmek için gruplar halinde toplamanız gerekir. Daha ayrıntılı ve yüksek güvenilirlikli değerlendirmeler için, bunları nasıl geçirebileceğinizi anlamak üzere makineler arasındaki bağımlılıkları görselleştirin ve eşleyebilirsiniz.

- [VMware VM 'leri](tutorial-prepare-vmware.md), [Hyper-V VM 'leri](tutorial-prepare-hyper-v.md)ve [fiziksel sunucular](tutorial-prepare-physical.md)için değerlendirme çalıştırma hakkında bilgi edinin.
- [BIR CSV dosyası ile içeri aktarılan](tutorial-assess-import.md)sunucuları değerlendirme hakkında bilgi edinin.
- [Bağımlılık görselleştirmesini](concepts-dependency-visualization.md)ayarlama hakkında bilgi edinin.

## <a name="assessments-in-server-assessment"></a>Sunucu değerlendirmesinde değerlendirmeler 

Azure geçişi sunucu değerlendirmesi ile oluşturduğunuz değerlendirmeler, verilerin bir zaman noktası anlık görüntüsüdür. Sunucu değerlendirmesi aracı iki tür değerlendirme sağlar.

**Değerlendirme türü** | **Ayrıntılar** | **Veriler**
--- | --- | ---
**Performans tabanlı** | Toplanan performans verilerine dayalı öneriler oluşturan değerlendirmeler | VM boyutu önerisi, CPU ve bellek kullanımı verilerine göre belirlenir.<br/><br/> Disk türü önerisi (Standart HDD/SSD veya Premium yönetilen diskler), şirket içi disklerin ıOPS ve aktarım hızını temel alır.
**Şirket içi olarak** | Öneriler oluşturmak için performans verilerini kullanmayan değerlendirmeler. | VM boyutu önerisi, şirket içi VM boyutunu temel alır<br/><br> Önerilen disk türü, değerlendirme için seçilen depolama türünü temel alır.

## <a name="collecting-performance-data"></a>Performans verileri toplanıyor

Performans verileri aşağıdaki gibi toplanır:

1. [Azure geçişi](migrate-appliance.md) gereci gerçek zamanlı bir örnek noktası toplar:

    - **VMware VM 'leri**: VMware VM 'Leri Için Azure geçişi gereci her 20 saniyelik aralığa göre gerçek zamanlı bir örnek noktası toplar.
    - **Hyper-v VM 'leri**: Hyper-v VM 'leri için, gerçek zamanlı örnek noktası her 30 saniyelik aralığa göre toplanır.
    - **Fiziksel sunucular**: fiziksel sunucular için gerçek zamanlı örnek noktası her beş dakikalık aralıkta toplanır. 
    
2. Gereç, her 10 dakikada bir tek bir veri noktası oluşturmak için örnek noktalara (20 saniye, 30 saniye, beş dakika) kaydolur. Tek bir veri noktası oluşturmak için, Gereç tüm örneklerden tepe değeri seçer ve ardından bunu Azure 'a gönderir.
3. Sunucu değerlendirmesi, geçen ay için 10 dakikalık tüm örnek noktalarını depolar.
4. Bir değerlendirme oluşturduğunuzda, sunucu değerlendirmesi, *performans geçmişi* ve *yüzdebirlik kullanımı*için yüzdelik değerler temelinde, doğru boyutlandırma için kullanılacak uygun veri noktasını belirler.

    - Örneğin, performans geçmişi bir hafta olarak ayarlandıysa ve yüzdebirlik kullanımı 95. yüzdebirlik ise, sunucu değerlendirmesi son hafta için 10 dakikalık örnek noktalarını artan düzende sıralar ve sağ boyutlandırma için 95. yüzdebirlik değerini seçer. 
    - 95. yüzdebirlik değeri, 99. yüzdebirlik değerini seçerseniz dahil edilen tüm aykırı değerleri yok saydığınızdan emin olmanızı sağlar.
    - Dönemin en yoğun kullanımını seçmek ve tüm aykırı değerleri kaçırmak istemiyorsanız, yüzdebirlik kullanımı için 99. yüzdebirlik ' ü seçmeniz gerekir.

5. Bu değer, her ölçüm için etkili performans kullanım verilerini (CPU kullanımı, bellek kullanımı, disk ıOPS (okuma ve yazma), disk aktarım hızı (okuma ve yazma) ve ağ aktarım hızını (ın ve out) almak için rahatlık faktörüyle çarpılır. Gereç toplar.
## <a name="whats-in-an-assessment"></a>Bir değerlendirme neleri içerir?

Azure geçişi: Sunucu değerlendirmesi ' nde bir değerlendirmeye dahil olanlar aşağıda verilmiştir.

**Özellik** | **Ayrıntılar**
--- | ---
**Hedef konum** | Geçirmek istediğiniz konum. Sunucu değerlendirmesi Şu anda bu hedef Azure bölgelerini destekliyor:<br/><br/> Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Kanada Orta, Kanada Doğu, Orta Hindistan, Orta ABD, Çin Doğu, Çin Kuzey, Doğu Asya, Doğu ABD, Doğu ABD2, Almanya Orta, Almanya Kuzeydoğu, Japonya Doğu, Japonya Batı, Kore Orta, Kore Güney, Kuzey Orta ABD, Kuzey Avrupa, Orta Güney ABD, Güneydoğu Asya, Güney Hindistan, UK Güney, UK Batı, US Gov Arizona, US Gov Teksas, US Gov Virginia, Orta Batı ABD, Batı Avrupa, Batı Hindistan, Batı ABD ve Batı ABD2.
*Hedef depolama diski (örneğin, boyutlandırma)* * | Azure 'da depolama için kullanılacak disk türü. <br/><br/> Hedef depolama diskini Premium yönetilen, standart SSD yönetimli veya standart HDD yönetimli olarak belirtin.
**Hedef depolama diski (performans tabanlı boyutlandırma)** | Hedef depolama diskinin türünü otomatik, Premium yönetilen, standart HDD yönetimli veya standart SSD yönetimli olarak belirtin.<br/><br/> **Otomatik**: disk önerisi, disklerin performans verilerine (saniye başına giriş/çıkış IŞLEMI (IOPS) ve aktarım hızı) göre belirlenir.<br/><br/>**Premium/standart**: değerlendirme, seçilen depolama türü içinde bir disk SKU 'su önerir.<br/><br/> % 99,9 ' un tek örnekli VM SLA 'Sı elde etmek istiyorsanız, Premium yönetilen diskler kullanmayı düşünün. Bu, değerlendirmede tüm disklerin Premium tarafından yönetilen diskler olarak önerilmesini sağlar.<br/><br/> Azure Geçişi yalnızca yönetilen disklerin geçiş değerlendirmesini destekler.
**Ayrılmış örnekler (RIS)** | Değerlendirmede uygulanan maliyet tahminleri hesap halinde RI indirimleri alacak şekilde Azure 'da [ayrılmış örnekler](https://azure.microsoft.com/pricing/reserved-vm-instances/) belirtin.<br/><br/> Şu anda yalnızca Azure geçişi 'ndeki Kullandıkça Öde teklifleri için yapılandırılmış olan RIS desteği sunulmaktadır.
**Boyutlandırma ölçütü** | Azure 'da VM 'yi sağ boyuta eklemek için kullanılır.<br/><br/> As, boyutlandırma veya performans tabanlı boyutlandırma kullanın.
**Performans geçmişi** | Performans tabanlı boyutlandırma ile kullanılır. Performans verilerini değerlendirirken kullanılan süreyi belirtin.
**Yüzdebirlik kullanımı** | Performans tabanlı boyutlandırma ile kullanılır. Sağ boyutlandırmanın kullanılacağı performans örneğinin yüzdebirlik değerini belirtir. 
**VM serisi** | Doğru boyutlandırma için göz önünde bulundurmanız istediğiniz Azure VM serisini belirtin. Örneğin, Azure 'da A serisi VM 'Ler gerektiren bir üretim ortamınız yoksa, bir serisi listeden veya seriden hariç bırakabilirsiniz.
**Konfor katsayısı** | Değerlendirme sırasında kullanılan arabellek. VM 'Ler için makine kullanım verilerinin üzerine uygulanır (CPU, bellek, disk ve ağ). Dönemsel kullanım, kısa performans geçmişi ve gelecekteki kullanımlarda olası artışlar gibi sorunlar için BT hesapları.<br/><br/> Örneğin, %20 kullanımındaki bir 10 çekirdekli VM normalde iki çekirdekli bir VM ile sonuçlanır. 2\.0 x ile ilgili bir etken olan sonuç, bunun yerine dört çekirdekli bir VM 'dir.
**Teklif** | Kayıtlı olduğunuz [Azure teklifini](https://azure.microsoft.com/support/legal/offer-details/) görüntüler. Sunucu değerlendirmesi maliyeti uygun şekilde tahmin eder.
**Para Birimi** | Hesabınız için faturalama para birimi.
**İndirim (%)** | Azure teklifinin üzerine aldığınız aboneliğe özgü indirimleri listeler. Varsayılan ayar, %0’dır.
**VM çalışma süresi** | Azure VM 'Leri günde 24 saat, haftanın 7 günü ise, çalıştırılacak süreyi (günde gün ve saat başına gün) belirtebilirsiniz. Maliyet tahminleri buna göre işlenir.<br/><br/> Varsayılan değer ayda 31 gün ve günde 24 saat olur.
**Azure Hibrit Avantajı** | Yazılım Güvencesi olup olmadığını ve [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-use-benefit/)uygun olduğunu belirtir. Evet (varsayılan ayar) olarak ayarlanırsa Windows VM 'Leri için Windows dışı Azure fiyatları kabul edilir.

Sunucu değerlendirmesi ile değerlendirme oluşturmak için [en iyi uygulamaları gözden geçirin](best-practices-assessment.md) .

## <a name="how-are-assessments-calculated"></a>Değerlendirmeler nasıl hesaplanır? 

Azure geçişi 'nde değerlendirmeler: Sunucu değerlendirmesi, şirket içi makineler hakkında toplanan meta veriler kullanılarak hesaplanır. Kullanılarak içeri aktarılan makinelerde bir değerlendirme çalıştırırsanız. CSV dosyası, hesaplama için meta verileri sağlarsınız. Hesaplamalar üç aşamada oluşur:

1. **Azure hazırlığını hesapla**: makinelerin Azure 'a geçiş için uygun olup olmadığını değerlendirin.
2. **Boyutlandırma önerilerini hesapla**: işlem, depolama ve ağ boyutlandırmayı tahmin etme. 
2. **Aylık maliyetleri hesapla**: geçiş sonrasında Azure 'da makineleri çalıştırmaya yönelik tahmini aylık işlem ve depolama maliyetlerini hesaplayın.

Hesaplamalar sırayla ve bir makine sunucusu, yalnızca öncekini geçerse daha sonraki bir aşamaya geçer. Örneğin, bir sunucu Azure hazırlığını başaramazsa, Azure için uygun değil olarak işaretlenir ve bu sunucu için boyutlandırma ve maliyetlendirme yapılmaz.



## <a name="calculate-readiness"></a>Hazırlığı hesapla

Tüm makineler Azure 'da çalışmak üzere uygun değildir. Sunucu değerlendirmesi her bir şirket içi makine değerlendirir ve bunu bir hazır olma kategorisi atar. 
- **Azure Için hazırlanma**: makine, hiçbir değişiklik yapılmadan Azure 'a olarak geçirilebilir. Azure 'da tam Azure desteğiyle başlatılır.
- **Azure Için koşullu olarak hazırlanıyor**: makine Azure 'da başlayabilir, ancak tam Azure desteği olmayabilir. Örneğin, eski bir Windows Server sürümünü çalıştıran bir makine Azure 'da desteklenmez. Bu makineleri Azure 'a geçirmeden önce dikkatli olmanız gerekir. Hazırlık sorunlarını gidermek için değerlendirmede önerilen düzeltme kılavuzunu izleyin.
- **Azure için hazırlanma**: makine Azure 'da başlamıyor. Örneğin, bir şirket içi makine diski 64-TBs ' den büyükse Azure 'da barındırılamaz. Geçişten önce sorunu gidermeye yönelik düzeltme kılavuzunu izleyin. 
- **Hazır olma bilinmiyor**: Azure geçişi, yetersiz meta veri nedeniyle makinenin hazır olduğunu saptayamadık.

Sunucu değerlendirmesi, hazırlığı hesaplamak için aşağıdaki tablolarda özetlenen makine özelliklerini ve işletim sistemi ayarlarını gözden geçirir. 

### <a name="machine-properties"></a>Makine özellikleri

Sunucu değerlendirmesi, Azure 'da çalışıp çalışmadığını öğrenmek için şirket içi VM 'nin aşağıdaki özelliklerini gözden geçirir.

**Özellik** | **Ayrıntılar** | **Azure hazırlık durumu**
--- | --- | ---
**Önyükleme türü** | Azure, UEFı değil, BIOS 'un önyükleme türü olan VM 'Leri destekler. | Önyükleme türü UEFı ise koşullu olarak hazırlanın.
**Sayısı** | Makinelerdeki çekirdek sayısı, bir Azure VM için desteklenen en yüksek çekirdek sayısına (128) eşit veya ondan daha az olmalıdır.<br/><br/> Performans geçmişi varsa, Azure geçişi karşılaştırma için kullanılan çekirdekleri dikkate alır. Değerlendirme ayarlarında bir rahatlık faktörü belirtilmişse, kullanılan çekirdek sayısı, rahatlık faktörüyle çarpılarak çarpılır.<br/><br/> Hiçbir performans geçmişi yoksa, Azure geçişi, rakip etmenini uygulamadan ayrılmış çekirdekleri kullanır. | Sınırlara eşit veya daha küçükse hazırlanın.
**Bellek** | Makinenin bellek boyutu, bir Azure sanal makinesi için izin verilen en yüksek belleğe (3892 gigabayt [GB] Standard_M128m&nbsp;<sup>2</sup>) kadar bir değere eşit veya ondan küçük olmalıdır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).<br/><br/> Performans geçmişi varsa, Azure geçişi karşılaştırma için kullanılan belleği kabul eder. Bir rakip faktörü belirtilmişse, kullanılan bellek, rahatlık faktörüyle çarpılarak çarpılır.<br/><br/> Geçmiş yoksa, ayrılan bellek, rahatlık faktörü uygulanmadan kullanılır.<br/><br/> | Sınırlar içindeyse hazırlanın.
**Depolama diski** | Bir diskin ayrılan boyutu 32 TB veya daha az olmalıdır. Azure, Ultra SSD diskleriyle 64 TB diskleri destekler, ancak Azure geçişi: Sunucu değerlendirmesi Şu anda Ultra SSD desteklemediği için disk boyutu sınırları olarak 32 TB 'yi kontrol eder. <br/><br/> Makineye bağlı disk sayısı, işletim sistemi diski dahil 65 veya daha az olmalıdır. | Sınırlar içindeyse hazırlanın.
**Ağ** | Bir makinenin kendisine bağlı 32 veya daha az ağ arabirimi (NIC) olmalıdır. | Sınırlar içindeyse hazırlanın.

### <a name="guest-operating-system"></a>Konuk işletim sistemi
Sunucu değerlendirmesi, VM özellikleriyle birlikte makinelerin Konuk işletim sistemine bakar ve bunların Azure üzerinde çalışıp çalışmadığını tespit edebilir.

> [!NOTE]
> VMware VM 'Leri için sunucu değerlendirmesi, Konuk IŞLETIM sistemi analizini işlemek üzere vCenter Server ' de VM için belirtilen işletim sistemini kullanır. VMware üzerinde çalışan Linux VM 'Leri için şu anda Konuk işletim sisteminin tam çekirdek sürümünü tanımlamaz.

Aşağıdaki mantık, sunucu değerlendirmesi tarafından, işletim sistemine bağlı olarak Azure hazırlığını belirlemek için kullanılır.

**İşletim Sistemi** | **Ayrıntılar** | **Azure hazırlık durumu**
--- | --- | ---
Tüm SPs & Windows Server 2016 | Azure tam destek sağlar. | Azure için hazır
Tüm SPs & Windows Server 2012 R2 | Azure tam destek sağlar. | Azure için hazır
Tüm SPs & Windows Server 2012 | Azure tam destek sağlar. | Azure için hazır
Tüm SPs 'ler ile Windows Server 2008 R2 | Azure tam destek sağlar.| Azure için hazır
Windows Server 2008 (32-bit ve 64-bit) | Azure tam destek sağlar. | Azure için hazır
Windows Server 2003, 2003 R2 | Bu işletim sistemleri destek son tarihlerini geçti ve Azure 'da destek için [özel bir destek sözleşmesine (CSA)](https://aka.ms/WSosstatement) ihtiyaç duyuyor. | Azure için koşullu olarak hazırlanın. Azure 'a geçiş yapmadan önce işletim sistemini yükseltmeniz göz önünde bulundurun.
Windows 2000, 98, 95, NT, 3,1, MS-DOS | Bu işletim sistemleri destek son tarihleri geçti. Makine Azure 'da başlayabilir, ancak Azure işletim sistemi desteği sağlamaz. | Azure için koşullu olarak hazırlanın. Azure 'a geçiş yapmadan önce işletim sistemini yükseltmenizi öneririz.
Windows Istemcisi 7, 8 ve 10 | Azure [yalnızca Visual Studio aboneliği](https://docs.microsoft.com/azure/virtual-machines/windows/client-images) için destek sağlar. | Azure için koşullu olarak hazır
Windows 10 Pro masaüstü | Azure, [çok kiracılı barındırma haklarıyla](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment) destek sağlar. | Azure için koşullu olarak hazır
Windows Vista, XP Professional | Bu işletim sistemleri destek son tarihleri geçti. Makine Azure 'da başlayabilir, ancak Azure işletim sistemi desteği sağlamaz. | Azure için koşullu olarak hazırlanın. Azure 'a geçiş yapmadan önce işletim sistemini yükseltmenizi öneririz.
Linux | Azure bu [Linux işletim sistemlerini](../virtual-machines/linux/endorsed-distros.md)doğrubir şekilde algılar. Diğer Linux işletim sistemleri Azure 'da başlayabilir, ancak Azure 'a geçiş yapmadan önce işletim sistemini onaylı bir sürüme yükseltmenizi öneririz. | Sürüm onaylanmış ise Azure için hazırlanın.<br/><br/>Sürüm henüz hazırlanmadığından koşullu olarak kabul edilebilir.
Diğer işletim sistemleri<br/><br/> Örneğin, Oracle Solaris, Apple macOS vb., FreeBSD vb. | Azure, bu işletim sistemlerini onaylamaz. Makine Azure 'da başlayabilir, ancak Azure işletim sistemi desteği sağlamaz. | Azure için koşullu olarak hazırlanın. Azure 'a geçiş yapmadan önce desteklenen bir işletim sistemi yüklemenizi öneririz.  
VCenter Server içinde **diğeri** olarak belirtilen işletim sistemi | Azure geçişi bu durumda işletim sistemini tanımlayamıyor. | Bilinmeyen hazırlık. VM içinde çalışan işletim sisteminin Azure 'da desteklendiğinden emin olun.
32 bit işletim sistemleri | Makine Azure 'da başlayabilir, ancak Azure tam destek sunmayabilir. | Azure için koşullu olarak hazırlanın. Azure 'a geçiş yapmadan önce makinenin işletim sistemini 32 bitlik IŞLETIM sisteminden 64 bit işletim sistemine yükseltmeniz göz önünde bulundurun.

## <a name="calculate-sizing-as-is-on-premises"></a>Boyutlandırmayı hesapla (Şirket içi olarak)

Makine Azure için hazırlanıyor olarak işaretlendikten sonra sunucu değerlendirmesi, Azure VM ve disk SKU 'sunu belirlemek için boyutlandırma önerileri sağlar. Şirket içi olarak boyutlandırma kullanıyorsanız, sunucu değerlendirmesi VM 'Lerin ve disklerin performans geçmişini dikkate almaz.

**İşlem boyutlandırma**: şirket içinde ayrılan boyuta göre BIR Azure VM SKU 'su ayırır.
**Depolama/disk boyutlandırma**: Sunucu değerlendirmesi, değerlendirme özelliklerinde belirtilen depolama türüne bakar (Standart HDD/SSD/Premium) ve disk türünü uygun şekilde önerir. Varsayılan depolama türü Premium disklerdir.
**Ağ boyutlandırma**: Sunucu değerlendirmesi, ağ bağdaştırıcısını şirket içi makinede değerlendirir.


## <a name="calculate-sizing-performance-based"></a>Boyutlandırmayı hesapla (performans tabanlı)

Bir makine Azure için hazırlanın olarak işaretlendikten sonra, performans tabanlı boyutlandırma kullanırsanız, boyutlandırma önerilerini aşağıdaki gibi kullanarak sunucu değerlendirmesi yapın:

- Sunucu değerlendirmesi, Azure 'da VM boyutunu ve disk türünü tanımlamak için makinenin performans geçmişini dikkate alır.
- Sunucular bir CSV dosyası kullanılarak içeri aktarılmışsa, belirttiğiniz değerler kullanılır. Bu yöntem özellikle şirket içi makineyi aşırı ayırdıysanız, kullanım gerçekten düşüktür ve maliyetleri kaydetmek için Azure 'da VM 'yi sağ boyuta eklemek istiyorsanız özellikle yararlıdır. 
- Performans verilerini kullanmak istemiyorsanız, önceki bölümde açıklandığı gibi boyutlandırma ölçütünü şirket içinde olduğu gibi olarak sıfırlayın.

### <a name="calculate-storage-sizing"></a>Depolama boyutunu hesapla

Azure geçişi, depolama boyutu için makineye bağlı her diski Azure 'daki bir diske eşlemeye çalışır ve aşağıdaki gibi çalışır:

1. Sunucu değerlendirmesi, gereken toplam ıOPS 'yi almak için bir diskin okuma ve yazma ıOPS 'sini ekler. Benzer şekilde, her diskin toplam aktarım hızını almak için okuma ve yazma aktarım hızı değerlerini ekler.
2. Depolama türünü otomatik olarak belirttiyseniz, etkin ıOPS ve aktarım hızı değerlerine göre, sunucu değerlendirmesi, diskin bir standart HDD, standart SSD veya Azure 'da bir Premium diskle eşlenmesi gerekip gerekmediğini belirler. Depolama türü Standart HDD/SSD/Premium olarak ayarlandıysa, sunucu değerlendirmesi seçilen depolama türü (Standart HDD/SSD/Premium diskler) içinde bir disk SKU 'SU bulmayı dener.
3. Diskler şu şekilde seçilir:
    - Sunucu değerlendirmesi gereken ıOPS ve aktarım hızına sahip bir disk bulamazsa, makineyi Azure için uygun değil olarak işaretler.
    - Sunucu değerlendirmesi bir dizi uygun disk bulursa, değerlendirme ayarlarında belirtilen konumu destekleyen diskleri seçer.
    - Birden fazla uygun disk varsa, sunucu değerlendirmesi en düşük maliyetli diski seçer.
    - Herhangi bir disk için performans verileri kullanılamaz durumdaysa, Azure 'da standart SSD disk bulmak için diskin yapılandırma verileri (disk boyutu) kullanılır.

### <a name="calculate-network-sizing"></a>Ağ boyutlandırmayı hesapla

Sunucu değerlendirmesi, şirket içi makineye bağlı ağ bağdaştırıcılarının sayısını ve bu ağ bağdaştırıcılarının gerektirdiği performansı destekleyebilen bir Azure VM bulmaya çalışır.
- Şirket içi VM 'nin etkili ağ performansını almak için sunucu değerlendirmesi, saniyede aktarılan verileri (MB/sn), tüm ağ bağdaştırıcılarında (ağ üzerinden) toplar ve rahatlık faktörünü uygular. Bu numarayı, gerekli ağ performansını destekleyebilen bir Azure VM bulmak için kullanır.
- Sunucu değerlendirmesi, ağ performansının yanı sıra Azure VM 'nin gerekli ağ bağdaştırıcısı sayısını destekleyip desteklemediğini de dikkate alır.
- Ağ performans verisi yoksa, sunucu değerlendirmesi yalnızca VM boyutlandırma için ağ bağdaştırıcısı sayısını dikkate alır.


### <a name="calculate-compute-sizing"></a>İşlem boyutunu hesapla

Depolama ve ağ gereksinimlerini hesapladıktan sonra sunucu değerlendirmesi, Azure 'da uygun bir VM boyutu bulmak için CPU ve bellek gereksinimlerini dikkate alır.
- Azure geçişi, Azure 'da uygun bir VM boyutu bulmak için kullanılan etkin çekirdekler ve belleğe bakar.
- Uygun boyut bulunamazsa, makine Azure için uygun olmayan olarak işaretlenir.
- Uygun bir boyut bulunursa Azure geçişi depolama ve ağ hesaplamalarını uygular. Ardından, son VM boyutu önerisi için konum ve fiyatlandırma katmanı ayarlarını uygular.
- Birden fazla uygun Azure sanal makinesi boyutu varsa, en düşük maliyetli olan önerilir.


### <a name="calculate-confidence-ratings"></a>Güven derecelendirmelerini hesapla

Azure geçişi 'ndeki her performans tabanlı değerlendirme, bir (en düşük) ile beş yıldızlı (en yüksek) arasında değişen bir güvenirlik derecelendirmesi ile ilişkilendirilir.
- Güvenilirlik derecelendirmesi, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliği temelinde bir değerlendirmeye atanır.
- Bir değerlendirmenin güvenilirlik derecesi, Azure Geçişi tarafından sağlanan boyut önerilerinin güvenilirliğini tahmin etmenize yardımcı olur.
- Güven derecelendirmeleri, *Şirket içi* değerlendirmelere uygun değildir.
- Performans tabanlı boyutlandırma için sunucu değerlendirmesi şunları gerektirir:
    - CPU ve VM belleği için kullanım verileri.
    - SANAL makineye bağlı her diske yönelik disk ıOPS ve aktarım hızı verileri.
    - Bir VM 'ye bağlı her bir ağ bağdaştırıcısı için performans tabanlı boyutlandırmayı işlemek üzere ağ g/ç.

   Bu kullanım numaralarının herhangi biri vCenter Server kullanılamazsa, boyut önerisi güvenilir olmayabilir.

Kullanılabilir veri noktalarının yüzdesine bağlı olarak, değerlendirmenin güvenilirlik derecelendirmesi aşağıdaki gibi gider.

   **Veri noktalarının kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
   --- | ---
   % 0-20 | 1 yıldız
   % 21-40 | 2 yıldız
   % 41-60 | 3 yıldız
   % 61-80 | 4 yıldız
   % 81-100 | 5 yıldız

> [!NOTE]
> Güven derecelendirmeleri, kullanılarak içeri aktarılan sunucuların değerlendirmelerine atanmaz. CSV dosyasını Azure geçişi 'ne ekleyin. 

#### <a name="low-confidence-ratings"></a>Düşük güvenilirlikli derecelendirmeler

Bir değerlendirmenin en düşük güvenilirlik derecelendirmesinin neden olmasının birkaç nedeni aşağıda verilmiştir:

- Değerlendirmeyi oluşturduğunuz süre için ortamınızı profildiniz. Örneğin, performans süresi ile bir gün ayarlanmış bir değerlendirme oluşturursanız, toplanan tüm veri noktaları için bulmayı başlattıktan sonra en az bir gün beklemeniz gerekir.
- Bazı sanal makineler, değerlendirmenin hesaplanmakta olduğu süre boyunca kapatıldı. Bir süre için herhangi bir VM kapatılmışsa, sunucu değerlendirmesi söz konusu döneme ait performans verilerini toplayamazlar.
- Değerlendirmenin hesaplandığı süre boyunca bazı sanal makineler oluşturulmuştur. Örneğin, geçen ayın performans geçmişi için bir değerlendirme oluşturduysanız ancak bazı sanal makineler ortamda yalnızca bir hafta önce oluşturulduysa, yeni VM 'lerin performans geçmişi, tamamlanma süresi boyunca mevcut olmayacaktır.

> [!NOTE]
> Herhangi bir değerlendirmenin güvenilirlik derecelendirmesi beş yıldızlı günden azsa, gerecin ortamın profilini oluşturup daha sonra değerlendirmeyi yeniden hesaplayabilmeniz için en az bir gün beklemeniz önerilir. Aksi takdirde, performans tabanlı boyutlandırma güvenilir olmayabilir. Bu durumda, değerlendirmeyi şirket içi boyutlandırılmasına geçmeniz önerilir.

## <a name="calculate-monthly-costs"></a>Aylık maliyetleri hesapla

Boyutlandırma önerileri tamamlandıktan sonra, Azure geçişi geçişten sonra işlem ve depolama maliyetlerini hesaplar.

- **İşlem maliyeti**: ÖNERILEN Azure VM boyutunu kullanarak Azure GEÇIŞI, VM 'nin aylık maliyetini hesaplamak IÇIN faturalandırma API 'sini kullanır.
    - Hesaplama, işletim sistemi, yazılım güvencesi, ayrılmış örnekler, VM çalışma süresi, konum ve para birimi ayarlarını hesaba göre alır.
    - Toplam aylık işlem maliyetini hesaplamak için tüm makinelerdeki maliyeti toplar.
- **Depolama maliyeti**: bir makine için aylık depolama maliyeti, makineye bağlı tüm disklerin aylık maliyeti aşağıda gösterildiği gibi hesaplanır:
    - Sunucu değerlendirmesi, tüm makinelerin depolama maliyetlerini toplayarak aylık toplam depolama maliyetlerini hesaplar.
    - Şu anda hesaplama, değerlendirme ayarlarında belirtilen teklifleri göz önünde bulundurmaz.

Ücretler, değerlendirme ayarlarında belirtilen para biriminde görüntülenir.


## <a name="next-steps"></a>Sonraki adımlar

Değerlendirme oluşturmak için en iyi uygulamaları [gözden geçirin](best-practices-assessment.md) . 
