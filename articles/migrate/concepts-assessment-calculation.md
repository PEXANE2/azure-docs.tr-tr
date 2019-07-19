---
title: Azure geçişi 'nde değerlendirme hesaplamaları | Microsoft Docs
description: Azure geçişi hizmetindeki değerlendirme hesaplamalarına genel bir bakış sağlar.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/15/2019
ms.author: raynew
ms.openlocfilehash: 87aa48c992b7887ce86c43cac29910dcc57b3e91
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234285"
---
# <a name="assessment-calculations"></a>Değerlendirme hesaplamaları

[Azure geçişi](migrate-services-overview.md) , şirket içi uygulamalarınızı, iş yüklerinizi ve özel/genel bulut örneklerini Azure 'a bulmayı, değerlendirmeyi ve geçirmeyi izlemek için bir merkezi Merkez sağlar. Hub, değerlendirme ve geçiş için Azure geçiş araçları ve ayrıca üçüncü taraf bağımsız yazılım satıcısı (ISV) teklifleri sağlar.

Sunucu değerlendirmesi, Azure 'da değerlendirir şirket içi sunucuları Azure 'a geçiş için geçirme konusunda bir araçtır. Bu makalede, değerlendirmelerin nasıl hesaplandığı hakkında bilgi sağlanır.

## <a name="whats-in-an-assessment"></a>Bir değerlendirme neleri içerir?

**Özellik** | **Ayrıntılar**
--- | ---
**Hedef konum** | Geçişi yapmak istediğiniz Azure konumu.<br/> Sunucu değerlendirmesi Şu anda bu hedef bölgeleri destekliyor: Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Kanada Orta, Kanada Doğu, Orta Hindistan, Orta ABD, Çin Doğu, Çin Kuzey, Doğu Asya, Doğu ABD, Doğu ABD2, Almanya Orta, Almanya Kuzeydoğu, Japonya Doğu, Japonya Batı, Kore Orta, Kore Güney, Kuzey Orta ABD, Kuzey Avrupa, Orta Güney ABD, Güneydoğu Asya, Güney Hindistan, UK Güney, UK Batı, US Gov Arizona, US Gov Teksas, US Gov Virginia, Orta Batı ABD, Batı Avrupa, Batı Hindistan, Batı ABD ve Batı ABD2.
**Depolama türü** | Azure 'da, ' ye taşımak istediğiniz disk türünü belirtmek için bu özelliği kullanabilirsiniz. <br/><br/> Şirket içi olarak boyutlandırma için, hedef depolama türünü Premium yönetilen diskler, Standart SSD yönetilen diskler veya Standart HDD tarafından yönetilen diskler olarak belirtebilirsiniz. Performans tabanlı boyutlandırma için, hedef disk türünü otomatik, Premium yönetilen diskler, Standart HDD yönetilen diskler veya Standart SSD tarafından yönetilen diskler olarak belirtebilirsiniz.<br/><br/> Depolama türünü otomatik olarak belirttiğinizde, disk önerisi, disklerin performans verilerine (ıOPS ve aktarım hızı) göre yapılır. Depolama türünü Premium/standart olarak belirtirseniz, değerlendirme, seçilen depolama türü içinde bir disk SKU 'SU önerir. % 99,9 tek örnekli bir VM SLA 'Sı elde etmek istiyorsanız, depolama türünü Premium ile yönetilen diskler olarak belirtmek isteyebilirsiniz. Bu, değerlendirmede tüm disklerin Premium tarafından yönetilen diskler olarak önerilmesini sağlar. Azure Geçişi’nin yönetilen diskleri yalnızca geçiş değerlendirmesi için desteklediğini unutmayın.
**Ayrılmış örnekler (RI)** | Bu özellik, Azure 'da [ayrılmış örneklere](https://azure.microsoft.com/pricing/reserved-vm-instances/) sahip olup olmadığını belirtmenize yardımcı olur, değerlendirmede maliyet tahminleri daha sonra RI indirimlerine göre yapılır. Ayrılmış örnekler şu anda yalnızca Azure geçişi 'nde Kullandıkça Öde teklifi için desteklenmektedir.
**Boyutlandırma ölçütü** | Azure için VM 'Leri doğru şekilde boyutlandırma için kullanılacak ölçüt. Performans geçmişini dikkate almadan, *performans tabanlı* boyutlandırma yapabilir veya VM 'leri *Şirket içinde olacak şekilde boyut olarak*ayarlayabilirsiniz.
**Performans geçmişi** | Makinelerin performans verilerini değerlendirmek için göz önünde bulundurulması gereken süre. Bu özellik yalnızca boyutlandırma ölçütü *performans tabanlı*olduğunda geçerlidir.
**Yüzdebirlik kullanımı** | Doğru boyutlandırma için dikkate alınacak performans örnek kümesinin yüzdebirlik değeri. Bu özellik yalnızca boyutlandırma *performans tabanlı*olduğunda geçerlidir.
**VM serisi** |     Uygun boyutlandırma için değerlendirmek istediğiniz bir VM serisini belirtebilirsiniz. Örneğin, Azure 'da bir serisi VM 'lere geçirmeyi planlamadığınız bir üretim ortamınız varsa, bir serisi listeden veya seriden hariç tutabilir ve sağ boyutlandırma yalnızca seçili seriler içinde yapılır.
**Konfor katsayısı** | Azure geçişi sunucu değerlendirmesi, değerlendirme sırasında bir arabellek (rahatetken) kabul eder. Bu tampon, VM’lerin makine kullanım verilerinin (CPU, bellek, disk ve ağ) üzerine uygulanır. Konfor katsayısı; sezona özgü kullanım, kısa performans geçmişi ve gelecek kullanımlarda oluşabilecek artışlar gibi konuları hesaba katar.<br/><br/> Örneğin, %20 kullanıma sahip 10 çekirdekli bir VM normalde 2 çekirdekli VM ile sonuçlanır. Ancak, 2.0x konfor katsayısı ile sonuç 4 çekirdekli VM olur.
**Teklif** | Kaydolduğunuz [Azure teklifi](https://azure.microsoft.com/support/legal/offer-details/). Azure Geçişi, buna göre bir maliyet tahmini oluşturur.
**Para Birimi** | Fatura para birimi.
**İndirim (%)** | Azure teklifinin yanı sıra aldığınız, aboneliğe özgü indirim.<br/> Varsayılan ayar, %0’dır.
**VM çalışma süresi** | VM 'leriniz Azure 'da 7/24 çalışmaya devam etmeyeceklerse, çalıştırılacağı süreyi (aylık gün sayısı ve gün başına saat sayısı), maliyet tahminleri de uygun şekilde yapılacak şekilde belirtebilirsiniz.<br/> Varsayılan değer ayda 31 gün ve günde 24 saat olur.
**Azure Hibrit Avantajı** | Yazılım Güvencesi olup olmadığını ve [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-use-benefit/)için uygun olup olmadığını belirtin. Evet olarak ayarlanırsa, Windows sanal makineleri için Windows olmayan Azure fiyatları dikkate alınır. Varsayılan değer Evet’tir.

## <a name="how-are-assessments-calculated"></a>Değerlendirmeler nasıl hesaplanır?

Azure geçişi sunucu değerlendirmesi ' nde bir değerlendirme, şirket içi sunucular hakkında toplanan meta veriler kullanılarak hesaplanır. Değerlendirme hesaplaması üç aşamada yapılır; değerlendirme hesaplaması, her bir sunucu için Azure uygunluk analizi, ardından boyutlandırma ve son olarak aylık maliyet tahmini ile başlar. Bir sunucu, bir öncekini geçerse yalnızca sonraki bir aşamaya geçer. Örneğin, bir sunucu, Azure uygunluk denetiminde başarısız olursa, Azure için uygun değil olarak işaretlenir ve boyutlandırma ve maliyetlendirme aynı şekilde yapılmaz.

## <a name="azure-suitability-analysis"></a>Azure uygunluk Analizi

Tüm makineler Azure 'da çalışmaya uygun değildir. Azure geçişi sunucu değerlendirmesi, her şirket içi makineyi Azure 'a uygunluk için değerlendirir ve değerlendirilen makineleri aşağıdaki uygunluk kategorilerinden birine kategorilere ayırır:
- **Azure Için hazırlanıyor** -makine herhangi bir değişiklik yapılmadan Azure 'a olarak geçirilebilir. Azure 'da tam Azure desteğiyle önyükleme yapılır.
- **Azure Için koşullu** olarak kullanılabilir-makine Azure 'da başlatılabilir, ancak tam Azure desteği olmayabilir. Örneğin, eski bir Windows Server işletim sistemi sürümüne sahip bir makine Azure 'da desteklenmez. Bu makineleri Azure 'a geçirmeden önce dikkatli olmanız ve geçirmeden önce hazırlık sorunlarını gidermek için değerlendirmede önerilen düzeltme kılavuzunu izlemeniz gerekir.
- **Azure için hazırlanma** -makine Azure 'da önyükleme olmayacak. Örneğin, şirket içi bir makinede 64 TB 'den daha fazla disk varsa, Azure üzerinde barındırılamaz. Azure 'a geçiş yapmadan önce hazırlık sorununu onarmak için değerlendirmede önerilen düzeltme kılavuzunu izlemeniz gerekir. Doğru boyut ve maliyet tahmini, Azure için uygun değil olarak işaretlenen makineler için yapılmaz.
- **Hazır olma durumu bilinmiyor** -şirket içi ortamdan toplanan meta veriler nedeniyle Azure geçişi makinenin hazır olup olmadığını bulamadı.

Azure geçişi sunucu değerlendirmesi, şirket içi makinenin Azure 'un hazır olduğunu belirlemek için makine özelliklerini ve konuk işletim sistemini gözden geçirir.

### <a name="machine-properties"></a>Makine özellikleri

Sunucu değerlendirmesi, Azure üzerinde çalışıp çalışmadığını belirlemek için şirket içi VM 'nin aşağıdaki özelliklerini gözden geçirir.

**Özellik** | **Ayrıntılar** | **Azure hazırlık durumu**
--- | --- | ---
**Önyükleme türü** | Azure, önyükleme türü olan VM 'Leri, UEFı değil, BIOS olarak destekler. | Önyükleme türü UEFı ise koşullu olarak hazırlanın.
**Sayısı** | Makinelerdeki çekirdek sayısı, bir Azure VM için desteklenen en yüksek çekirdek sayısına (128 çekirdek) eşit veya ondan daha az olmalıdır.<br/><br/> Performans geçmişi varsa, Azure geçişi karşılaştırma için kullanılan çekirdekleri dikkate alır. Değerlendirme ayarlarında bir rahatlık faktörü belirtilmişse, kullanılan çekirdek sayısı, rahatlık faktörüyle çarpılarak çarpılır.<br/><br/> Hiçbir performans geçmişi yoksa, Azure geçişi,, rahatlık faktörünü uygulamadan ayrılmış çekirdekleri kullanır. | Sınırlara eşit veya daha küçükse hazırlanın.
**Bellek** | Makinenin bellek boyutu, bir Azure sanal makinesi için izin verilen en büyük bellekten (Azure 14serisi Standard_M128m&nbsp;<sup>2</sup>' de 3892 GB) eşit veya ondan küçük olmalıdır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).<br/><br/> Performans geçmişi varsa, Azure geçişi karşılaştırma için kullanılan belleği kabul eder. Bir rakip faktörü belirtilmişse, kullanılan bellek, rahatlık faktörüyle çarpılarak çarpılır.<br/><br/> Hiçbir geçmiş yoksa, rahatlık faktörünü uygulamadan ayrılan bellek kullanılır.<br/><br/> | Sınırlar içindeyse hazırlanın.
**Depolama diski** | Bir diskin ayrılan boyutu 32TB veya daha az olmalıdır.<br/><br/> Makineye bağlı disk sayısı, işletim sistemi diski dahil 65 veya daha az olmalıdır. | Sınırlar içindeyse hazırlanın.
**Ağ** | Bir makinenin kendisine bağlı 32 veya daha az NIC 'e sahip olması gerekir. | Sınırlar içindeyse hazırlanın.

### <a name="guest-operating-system"></a>Konuk işletim sistemi
VM özellikleriyle birlikte Azure geçişi sunucu değerlendirmesi, Azure üzerinde çalışıp çalışmadığını belirlemek için makinelerin Konuk işletim sistemine bakar.

> [!NOTE]
> VMware VM 'Leri için Azure geçişi sunucu değerlendirmesi, Konuk IŞLETIM sistemi analizini yapmak üzere vCenter Server ' de VM için belirtilen işletim sistemini kullanır. VMware üzerinde çalışan Linux VM 'Leri için şu anda Konuk işletim sisteminin tam çekirdek sürümünü tanımlamaz.

Aşağıdaki mantık, Azure geçişi sunucu değerlendirmesi tarafından, işletim sistemine göre Azure hazırlığını belirlemek için kullanılır.

**İşletim Sistemi** | **Ayrıntılar** | **Azure hazırlık durumu**
--- | --- | ---
Tüm SPs & Windows Server 2016 | Azure tam destek sağlar. | Azure için hazır
Tüm SPs & Windows Server 2012 R2 | Azure tam destek sağlar. | Azure için hazır
Tüm SPs & Windows Server 2012 | Azure tam destek sağlar. | Azure için hazır
Tüm SPs 'ler ile Windows Server 2008 R2 | Azure tam destek sağlar.| Azure için hazır
Windows Server 2008 (32-bit ve 64-bit) | Azure tam destek sağlar. | Azure için hazır
Windows Server 2003, 2003 R2 | Bu işletim sistemleri destek tarihlerinden geçen ve Azure 'da destek için [özel bir destek sözleşmesine (CSA)](https://aka.ms/WSosstatement) ihtiyaç duyuyor. | Azure için koşullu olarak, Azure 'a geçiş yapmadan önce işletim sistemini yükseltmeyi düşünün.
Windows 2000, 98, 95, NT, 3,1, MS-DOS | Bu işletim sistemleri, destek tarihi sonunu geçti, makine Azure 'da başlatılabilir, ancak hiçbir işletim sistemi desteği Azure tarafından sağlanmaz. | Azure için koşullu olarak, Azure 'a geçiş yapmadan önce işletim sistemini yükseltmeniz önerilir.
Windows Istemcisi 7, 8 ve 10 | Azure [yalnızca Visual Studio aboneliği](https://docs.microsoft.com/azure/virtual-machines/windows/client-images) için destek sağlar. | Azure için koşullu olarak hazır
Windows 10 Pro masaüstü | Azure, [çok kiracılı barındırma haklarıyla](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment) destek sağlar. | Azure için koşullu olarak hazır
Windows Vista, XP Professional | Bu işletim sistemleri, destek tarihi sonunu geçti, makine Azure 'da başlatılabilir, ancak hiçbir işletim sistemi desteği Azure tarafından sağlanmaz. | Azure için koşullu olarak, Azure 'a geçiş yapmadan önce işletim sistemini yükseltmeniz önerilir.
Linux | Azure bu [Linux işletim sistemlerini](../virtual-machines/linux/endorsed-distros.md)doğrubir şekilde algılar. Diğer Linux işletim sistemleri Azure 'da önbaşlatılabilir, ancak Azure 'a geçiş yapmadan önce işletim sistemini onaylı bir sürüme yükseltmeniz önerilir. | Sürüm onaylanmış ise Azure için hazırlanın.<br/><br/>Sürüm henüz hazırlanmadığından koşullu olarak kabul edilebilir.
Diğer işletim sistemleri<br/><br/> Örneğin, Oracle Solaris, Apple Mac OS vb., FreeBSD vb. | Azure, bu işletim sistemlerini desteklemez. Makine Azure 'da başlatılabilir, ancak Azure tarafından bir işletim sistemi desteği sağlanmaz. | Azure için koşullu olarak, Azure 'a geçiş yapmadan önce desteklenen bir işletim sistemi yüklenmesi önerilir.  
VCenter Server içinde **diğeri** olarak belirtilen işletim sistemi | Azure geçişi bu durumda işletim sistemini tanımlayamıyor. | Bilinmeyen hazırlık. VM içinde çalışan işletim sisteminin Azure 'da desteklendiğinden emin olun.
32 bit işletim sistemleri | Makine Azure 'da önbaşlatılabilir, ancak Azure tam destek sunmayabilir. | Azure için koşullu olarak, Azure 'a geçiş yapmadan önce makinenin işletim sistemini 32 bitlik IŞLETIM sisteminden 64 bit işletim sistemine yükseltmeyi düşünün.

## <a name="sizing"></a>Boyutlandırma

Bir makine Azure için hazırlanın olarak işaretlendikten sonra sunucu değerlendirmesi, şirket içi VM için uygun Azure VM ve disk SKU 'sunu tanımlamayı içeren boyutlandırma işlemini gerçekleştirir. Boyut önerileri, belirtilen değerlendirme özelliklerine göre farklılık gösterir.

- Değerlendirme *performans tabanlı boyutlandırma*kullanıyorsa, Azure geçişi, Azure 'da VM boyutunu ve disk türünü belirlemek için makinenin performans geçmişini dikkate alır. Bu yöntem, şirket içi VM 'yi aşırı ayırdıysanız, ancak kullanım düşükse ve Azure 'da VM 'yi doğru boyuta kaydederek maliyetleri kazanmak için yararlıdır. Bu yöntem, geçiş sırasında boyutları iyileştirmenize yardımcı olur.
- VM boyutlandırma için performans verilerini göz önünde bulundurmayın ve şirket içi makineleri Azure 'da olduğu gibi almak istiyorsanız, boyutlandırma ölçütünü *Şirket* içi olarak belirtebilir ve sunucu değerlendirmesi, VM 'leri şirket içine göre boyutlandıracaktır kullanım verilerini dikkate almadan yapılandırma. Bu durumda disk boyutlandırma, değerlendirme özelliklerinde (Standart HDD diskler, Standart SSD diskler veya Premium diskler) belirttiğiniz depolama türüne göre yapılır.

### <a name="performance-based-sizing"></a>Performans tabanlı boyutlandırma

Performans tabanlı boyutlandırma için, Azure geçişi sunucu değerlendirmesi, VM 'ye bağlı olan disklerle başlar, ardından ağ bağdaştırıcıları gelir ve ardından Şirket içi VM 'nin işlem gereksinimlerine bağlı olarak bir Azure VM SKU 'sunu eşler. Azure geçişi gereci, CPU, bellek, disk ve ağ bağdaştırıcısı için performans verilerini toplamak üzere şirket içi ortamı profiller.

**Performans verileri toplama**

- VMware VM 'Leri için Azure geçişi gereci her 20 saniyelik aralıkta gerçek zamanlı bir örnek noktası toplayıp, Hyper-V VM 'lerinde gerçek zamanlı örnek noktanın her 30 saniyelik aralığa göre toplanması gerekir.
- Böylece gereç, her 10 dakikada toplanan örnek noktaların dökümünü yapar ve son 10 dakikalık en büyük değeri Azure geçişi sunucu değerlendirmesi 'ne gönderir.
- Azure geçişi sunucu değerlendirmesi, son bir ay için 10 dakikalık tüm örnek noktaları depolar ve *performans geçmişi* ve *yüzdebirlik kullanımı*için belirtilen değerlendirme özelliklerine bağlı olarak, uygun veri noktasını tanımlar Bu, doğru boyutlandırma için kullanılmalıdır. Örneğin, performans geçmişi bir güne ayarlanmışsa ve yüzdebirlik kullanımı 95. yüzdebirlik ise, son bir gün için 10 dakikalık örnek noktalarını kullanır, bunları artan düzende sıralar ve sağ boyutlandırma için 95. yüzdebirlik değerini seçer.
- Daha sonra yukarıdaki değer, her ölçüm için etkili performans kullanım verilerini (CPU kullanımı, bellek kullanımı, disk ıOPS (okuma ve yazma), disk aktarım hızı (okuma ve yazma), ağ üretimi (gelen ve giden)) almak için rahatlık faktörüyle çarpılır. Gereç toplar.
- Etkin kullanım değeri tanımlandıktan sonra, işlem, depolama ve ağ boyutlandırma aşağıdaki gibi yapılır:

**Depolama boyutu**: Azure geçişi, makineye bağlı her diski Azure 'daki bir diske eşlemeye çalışır.

> [!NOTE]
> Azure Geçişi: Sunucu değerlendirmesi yalnızca değerlendirme için yönetilen diskleri destekler.

  - Diskin okuma ve yazma ıOPS 'si, her diskin toplam aktarım hızını almak için gereken toplam ıOPS 'yi, benzer okuma ve yazma işleme değerlerini almak üzere eklenmiştir
  - Depolama türünü otomatik olarak belirttiyseniz, etkin ıOPS ve aktarım hızı değerlerine göre Azure geçişi sunucu değerlendirmesi, diskin bir standart HDD, standart SSD veya Azure 'da bir Premium diskle eşlenmesi gerekip gerekmediğini tanımlar. Depolama türü Standart HDD/SSD/Premium olarak ayarlandıysa, seçilen depolama türü (Standart HDD/SSD/Premium diskler) içinde bir disk SKU 'SU bulmaya çalışır.
  - Sunucu değerlendirmesi gereken ıOPS & işleme sahip bir disk bulamazsa, makineyi Azure için uygun değil olarak işaretler.
  - Uygun bir disk kümesi bulunursa, değerlendirme ayarlarında belirtilen konumu destekleyen olanları seçer.
  - Birden fazla uygun disk varsa, en düşük maliyetli olanı seçer.
  - Kullanılamayan bir disk için performans verileri, Azure 'da standart bir SSD disk bulmak için diskin yapılandırma verileri (disk boyutu) kullanılır.

**Ağ boyutlandırma**: Azure geçişi sunucu değerlendirmesi, şirket içi makineye bağlı ağ bağdaştırıcılarının sayısını ve bu ağ bağdaştırıcılarının gerektirdiği performansı destekleyebilen bir Azure VM bulmaya çalışır.
    - Şirket içi VM 'nin etkili ağ performansını almak için sunucu değerlendirmesi, saniyede aktarılan verileri (MB/sn), tüm ağ bağdaştırıcılarında (ağ üzerinden) toplar ve rahatlık faktörünü uygular. Bu sayı, gerekli ağ performansını destekleyebilen bir Azure VM bulmak için kullanılır.
    - Ayrıca, ağ performansının yanı sıra Azure VM 'nin gerekli ağ bağdaştırıcısı sayısını destekleyebilirler.
    - Kullanılabilir ağ performans verisi yoksa, VM boyutlandırma için yalnızca ağ bağdaştırıcılarının sayısı kabul edilir.

**İşlem boyutlandırma**: Depolama ve ağ gereksinimleri hesaplandıktan sonra, Azure geçişi sunucu değerlendirmesi, Azure 'da uygun bir VM boyutu bulmak için CPU ve bellek gereksinimlerini dikkate alır.
    - Azure geçişi, Azure 'da uygun bir VM boyutu bulmak için kullanılan etkin çekirdekler ve belleğe bakar.
    - Uygun boyut bulunamazsa, makine Azure için uygun olmayan olarak işaretlenir.
    - Uygun bir boyut bulunursa Azure geçişi depolama ve ağ hesaplamalarını uygular. Ardından, son VM boyutu önerisi için konum ve fiyatlandırma katmanı ayarlarını uygular.
    - Birden fazla uygun Azure sanal makinesi boyutu varsa, en düşük maliyetli olan önerilir.

### <a name="as-on-premises-sizing"></a>Şirket içi boyutlandırma olarak

*Şirket içi boyutlandırma*olarak kullanıyorsanız, sunucu değerlendirmesi VM 'lerin ve disklerin performans geçmişini dikkate almaz ve şirket içinde ayrılan boyuta göre Azure 'DA BIR VM SKU 'su ayırır. Benzer şekilde disk boyutlandırması için, değerlendirme özelliklerinde belirtilen depolama türüne bakar (Standart HDD/SSD/Premium) ve disk türünü uygun şekilde önerir. Varsayılan depolama türü Premium disklerdir.

## <a name="confidence-ratings"></a>Güven derecelendirmeleri
Azure geçişi 'ndeki her performans tabanlı değerlendirme, bir (en düşük) ila beş ' a (en yüksek) kadar uzanan bir güvenirlik derecelendirmesi ile ilişkilendirilir.
- Güvenilirlik derecelendirmesi, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliği temelinde bir değerlendirmeye atanır.
- Bir değerlendirmenin güvenilirlik derecesi, Azure Geçişi tarafından sağlanan boyut önerilerinin güvenilirliğini tahmin etmenize yardımcı olur.
- Güvenilirlik derecelendirmesi, şirket içi değerlendirmelere uygun değildir.
- Performans tabanlı boyutlandırma için Azure geçişi sunucu değerlendirmesi şunları gerektirir:
    - CPU ve VM belleği için kullanım verileri.
    - Bunlara ek olarak, VM'ye eklenen her disk için disk IOPS ve aktarım hızı değerleri de gerekir.
    - Aynı şekilde, bir VM 'ye bağlı her ağ bağdaştırıcısı için, güven derecelendirmesi ağ g/ç 'ye performans tabanlı boyutlandırma yapmak için ihtiyaç duyuyor.
    - Yukarıdaki kullanım numaralarının herhangi biri vCenter Server yoksa, boyut önerisi güvenilir olmayabilir.

Kullanılabilir veri noktalarının yüzdesine bağlı olarak değerlendirme için aşağıdaki gibi güvenilirlik derecelendirmesi sağlanır:

   **Veri noktalarının kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
   --- | ---
   %0-%20 | 1 Yıldız
   %21-%40 | 2 Yıldız
   %41-%60 | 3 Yıldız
   %61-%80 | 4 Yıldız
   %81-%100 | 5 Yıldız

### <a name="low-confidence-ratings"></a>Düşük güvenilirlikli derecelendirmeler

Bir değerlendirmenin en düşük güvenilirlik derecelendirmesine neden olabilecek nedenlerden bazıları:

- Değerlendirmeyi oluşturduğunuz süre için ortamınızı profildiniz. Örneğin, performans süresi ile bir gün ayarlanmış bir değerlendirme oluşturursanız, toplanan tüm veri noktaları için bulmayı başlattıktan sonra en az bir gün beklemeniz gerekir.
- Bazı sanal makineler, değerlendirmenin hesaplanmakta olduğu süre boyunca kapatıldı. Herhangi bir sanal makine bir süre için kapatılmışsa, Azure geçişi sunucu değerlendirmesi bu döneme ait performans verilerini toplayamıyor.
- Bazı VM 'Ler, değerlendirmenin hesaplandığı dönem arasında oluşturulmuştur. Örneğin, son bir ayın performans geçmişi için bir değerlendirme oluşturursanız, ancak bazı sanal makineler ortamda yalnızca bir hafta önce oluşturulduysa, yeni VM 'lerin performans geçmişi sürenin tamamına göre olmaz.

> [!NOTE]
> Herhangi bir değerlendirmenin güvenilirlik derecelendirmesi beş yıldızın altındaysa, gerecin ortamın profilini oluşturup daha sonra değerlendirmeyi yeniden hesaplayabilmeniz için en az bir gün beklemeniz önerilir. Aksi takdirde, performans tabanlı boyutlandırma güvenilir olmayabilir ve değerlendirmesi şirket içi boyutlandırma olarak kullanmak üzere geçmeniz önerilir.

## <a name="monthly-cost-estimation"></a>Aylık maliyet tahmini

Boyutlandırma önerileri tamamlandıktan sonra, Azure geçişi geçişten sonra işlem ve depolama maliyetlerini hesaplar.

- **İşlem maliyeti**: Azure geçişi, önerilen Azure VM boyutunu kullanarak VM 'nin aylık maliyetini hesaplamak için faturalandırma API 'sini kullanır.
    - Hesaplama, işletim sistemi, yazılım güvencesi, ayrılmış örnekler, VM çalışma süresi, konum ve para birimi ayarlarını hesaba göre alır.
    - Toplam aylık işlem maliyetini hesaplamak için tüm makinelerdeki maliyeti toplar.
- **Depolama maliyeti**: Bir makineye yönelik aylık depolama maliyeti, makineye bağlı tüm disklerin aylık maliyeti toplayarak hesaplanır
    - Azure geçişi sunucu değerlendirmesi, tüm makinelerin depolama maliyetlerini toplayarak aylık toplam depolama maliyetlerini hesaplar.
    - Şu anda hesaplama, değerlendirme ayarlarında belirtilen teklifleri hesaba götüremez.

Ücretler, değerlendirme ayarlarında belirtilen para biriminde görüntülenir.


## <a name="next-steps"></a>Sonraki adımlar

[VMware VM](tutorial-assess-vmware.md) 'Leri veya [Hyper-V VM 'leri](tutorial-assess-hyper-v.md)için bir değerlendirme oluşturun.
