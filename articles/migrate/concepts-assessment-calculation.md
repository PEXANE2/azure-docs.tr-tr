---
title: Azure Geçir Sunucu Değerlendirmesinde Değerlendirmeler
description: Azure Geçir Sunucu Değerlendirmesi'ndeki değerlendirmeler hakkında bilgi edinin
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: d1f32eea0ec6a8a4877fd1dc134344cfe68dcaba
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81537772"
---
# <a name="assessments-in-azure-migrateserver-assessment"></a>Azure Geçişinde Değerlendirmeler:Sunucu Değerlendirmesi

Bu makalede, [Azure Geçiş:Sunucu Değerlendirmesi](migrate-services-overview.md#azure-migrate-server-assessment-tool) aracındaki değerlendirmelere genel bir bakış sağlanmaktadır. Sunucu Değerlendirmesi aracı, Azure'a geçiş için şirket içi VMware VM'leri, Hyper-V V'leri ve fiziksel sunucuları değerlendirebilir.

## <a name="whats-an-assessment"></a>Değerlendirme nedir?

Sunucu Değerlendirmesi aracıyla yapılan bir değerlendirme, şirket içi sunucuları Azure'a geçirmenin hazır olup luğunu ölçer ve etkisini tahmin eder.

> [!NOTE]
> Azure Kamu'da desteklenen hedef değerlendirme konumlarını gözden [geçirin.](migrate-support-matrix.md#supported-geographies-azure-government) Değerlendirmelerdeki VM boyutu önerilerinin, Özellikle Devlet Bulutu bölgeleri için VM serisini kullanacağını unutmayın. VM türleri hakkında [daha fazla bilgi edinin.](https://azure.microsoft.com/global-infrastructure/services/?regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia&products=virtual-machines)

## <a name="types-of-assessments"></a>Değerlendirme türleri

Sunucu Değerlendirmesi ile oluşturduğunuz değerlendirmeler, verilerin anlık görüntüsütür. Sunucu Değerlendirmesi iki tür değerlendirme sağlar.

**Değerlendirme türü** | **Şey** | **Veri**
--- | --- | ---
**Performansa dayalı** | Toplanan performans verilerine dayalı önerilerde bulunan değerlendirmeler | VM boyutu önerisi CPU ve bellek kullanım verilerini temel alır.<br/><br/> Disk türü önerisi (standart HDD/SSD veya premium yönetilen diskler) IOPS'ye ve şirket içi disklerin iş başına dayalıdır.
**Şirket içi olduğu gibi** | Önerilerde bulunmak için performans verilerini kullanmayan değerlendirmeler. | VM boyutu önerisi şirket içi VM boyutuna dayanır<br/><br> Önerilen disk türü, değerlendirme için seçili depolama türünü temel alıntır.

## <a name="how-do-i-run-an-assessment"></a>Değerlendirmeyi nasıl yürütebilirim?

Bir değerlendirmeyi çalıştırmanın birkaç yolu vardır:

- Hafif bir Azure Geçiş cihazı tarafından toplanan sunucu meta verilerini kullanarak makineleri değerlendirin. Cihaz şirket içi makineleri keşfeder ve makine meta verilerini/performans verilerini Azure Geçiş'e gönderir.
- Virgülle ayrılmış değerler (CSV) biçiminde içe aktarılan sunucu meta verilerini kullanarak makineleri değerlendirin.

## <a name="how-do-i-assess-with-the-appliance"></a>Cihazla nasıl değerlendirebilirim?

Şirket içi sunucuları keşfetmek için bir Azure Geçir cihazı dağıtıyorsanız, aşağıdakileri yaparsınız:

1. Azure'u ve şirket içi ortamınızı Sunucu Değerlendirmesi ile çalışacak şekilde ayarlarsınız.
2. İlk değerlendirmeniz için bir Azure projesi oluşturur ve sunucu değerlendirme aracını ekleyin.
3. Hafif bir Azure Geçiş cihazı dağıtın. Cihaz sürekli olarak şirket içi makineleri keşfeder ve makine meta verilerini ve performans verilerini Azure Geçiş'e gönderir. Cihaz VM veya fiziksel bir makine olarak dağıtılır. Değerlendirmek istediğiniz makinelere bir şey yüklemenize gerek yoktur.
4. Cihaz makine keşfine başladıktan sonra, değerlendirmek istediğiniz makineleri bir gruba toplayabilir ve grup için bir değerlendirme çalıştırabilirsiniz.

Bu adımları denemek için [VMware,](tutorial-prepare-vmware.md) [Hyper-V](tutorial-prepare-hyper-v.md)veya [fiziksel sunucular](tutorial-prepare-physical.md) için eğitimlerimizi takip edebilirsiniz.

## <a name="how-do-i-assess-with-imported-data"></a>İçe aktarılan verilerle nasıl değerlendirebilirim?

Sunucuları bir CSV dosyası kullanarak değerlendiriyorsanız, bir cihaza ihtiyacınız yoktur. Bunun yerine, aşağıdakileri yaparsınız:

1. Azure'u Sunucu Değerlendirmesi ile çalışacak şekilde ayarlarsınız.
2. İlk değerlendirmeniz için bir Azure projesi oluşturur ve sunucu değerlendirme aracını ekleyin.
3. Bir CSV şablonu indirir ve sunucu verilerini ona eklersiniz.
4. Şablonu Sunucu Değerlendirmesi'ne aktarın.
5. Alma yla eklenen sunucuları keşfederseniz, sonra bir gruba toplanır ve grup için bir değerlendirme çalıştırırsınız.

## <a name="what-data-does-the-appliance-collect"></a>Cihaz hangi verileri topluyor?

Değerlendirme için Azure Geçir cihazını kullanıyorsanız, [VMware](migrate-appliance.md#collected-data---vmware) ve [Hyper-V](migrate-appliance.md#collected-data---hyper-v)için toplanan meta veriler ve performans verileri hakkında bilgi edinin.

## <a name="how-does-the-appliance-calculate-performance-data"></a>Cihaz performans verilerini nasıl hesaplar?

Cihazı keşif için kullanırsanız, bilgi işlem ayarlarıiçin performans verileri aşağıdaki gibi toplanır:

1. Cihaz gerçek zamanlı bir örnek noktası toplar:

    - **VMware VM'ler**: Cihaz her 20 saniyelik aralıkta gerçek zamanlı bir numune noktası toplar.
    - **Hyper-V VM'ler**: Gerçek zamanlı numune noktası her 30 saniyelik aralıkta toplanır.
    - **Fiziksel sunucular**: Gerçek zamanlı örnek nokta her beş dakikalık aralıklarla toplanır. 
    
2. Cihaz, her 10 dakikada bir tek bir veri noktası oluşturmak için örnek noktaları (20 saniye, 30 saniye, beş dakika) yuvarlar. Tek bir noktayı oluşturmak için, cihaz tüm örneklerdeki en yüksek değeri seçer ve ardından Azure'a gönderir.
3. Sunucu Değerlendirmesi, son bir ayın tüm 10 dakikalık örnek noktalarını depolar.
4. Bir değerlendirme oluşturduğunuzda, Sunucu *Değerlendirmesi, Performans geçmişi* ve *Yüzdele kullanımı*için yüzdelik değerleri temel alınarak doğru boyutlandırma için kullanılacak uygun veri noktasını tanımlar.

    - Örneğin, performans geçmişi bir haftaolarak ayarlanmışsa ve yüzdelik kullanım yüzde95'lik yüzdelik değerlerse, Sunucu Değerlendirmesi artan sırada geçen haftanın 10 dakikalık örnek noktalarını sıralar ve doğru boyutlandırma için 95 yüzdelik değeri seçer. 
    - 95. yüzdelik değer, herhangi bir aykırılığı göz ardı etmenizi sağlar, bu da yüzde 99'u seçerseniz dahil edilebilir.
    - Bu dönem için en yüksek kullanımı seçmek istiyorsanız ve herhangi bir aykırılık kaçırmak istemiyorsanız, yüzdelik kullanım için yüzde 99'unu seçmelisiniz.

5. Bu değer, cihazın topladığı her metrik (CPU kullanımı, bellek kullanımı, disk IOPS (okuma ve yazma), disk çıktısı (okuma ve yazma) ve ağ işlemi (giriş ve çıkış) için etkili performans kullanım verilerini elde etmek için konfor faktörü ile çarpılır.



## <a name="how-are-assessments-calculated"></a>Değerlendirmeler nasıl hesaplanır? 

Sunucu Değerlendirmesi'ndeki değerlendirmeler, şirket içi makineler için meta veri/performans verileri kullanılarak hesaplanır. Azure Geçir cihazını dağıtıysanız, cihaz tarafından toplanan verileri kullanarak değerlendirme yaparsınız. Bir . CSV dosyası, hesaplama için meta veri sağlar. Hesaplamalar üç aşamada gerçekleşir:

1. **Azure'a hazır olma durumunu hesaplayın**: Makinelerin Azure'a geçiş için uygun olup olmadığını değerlendirin.
2. **Boyutlandırma önerilerini hesaplama**: Hesaplamayı, depolamayı ve ağ boyutlandırmayı tahmin edin. 
2. **Aylık maliyetleri hesaplama**: Geçişten sonra Makineleri Azure'da çalıştırmak için tahmini aylık işlem ve depolama maliyetlerini hesaplayın.

Hesaplamalar sırayla dır ve bir makine sunucusu yalnızca öncekinden geçerse daha sonraki bir aşamaya geçer. Örneğin, bir sunucu Azure hazırlığını başarısızlığa uğratıyorsa, Azure için uygun olmayan olarak işaretlenir ve bu sunucu için boyutlandırma ve maliyetlendirme yapılmaz.


## <a name="whats-in-an-assessment"></a>Bir değerlendirme neleri içerir?

Sunucu Değerlendirmesi'ndeki bir değerlendirmede yer alan budur.

**Özellik** | **Şey**
--- | ---
**Hedef konum** | Geçirmek istediğiniz konum. Sunucu Değerlendirmesi şu anda şu hedef Azure bölgelerini destekler:<br/><br/> Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Kanada Orta, Kanada Doğu, Orta Hindistan, Orta ABD, Çin Doğu, Çin Kuzey, Doğu Asya, Doğu ABD, Doğu US2, Almanya Orta, Almanya Kuzeydoğu, Japonya Doğu, Japonya Batı, Kore Orta, Kore Güney, Kuzey Orta ABD, Kuzey Avrupa, Güney Orta ABD, Güneydoğu Asya, Güney Hindistan, İngiltere Güney, İngiltere Batı, ABD Gov Arizona, ABD Gov Texas, ABD Gov Virginia , Batı Orta ABD, Batı Avrupa, Batı Hindistan, Batı ABD ve Batı US2.
*Hedef depolama diski (boyutlandırma olarak)** | Azure'da depolama için kullanılacak disk türü. <br/><br/> Hedef depolama diskini premium yönetilen, standart SSD yönetilen veya standart HDD yönetilen olarak belirtin.
**Hedef depolama diski (performans tabanlı boyutlandırma)** | Hedef depolama diskinin türünü otomatik, premium yönetilen, standart HDD yönetilen veya standart SSD yönetilen olarak belirtin.<br/><br/> **Otomatik**: Disk önerisi, disklerin performans verilerine (saniyede giriş/çıkış işlemleri (IOPS) ve üretim verilerine dayanır.<br/><br/>**Premium/standart**: Değerlendirme, seçilen depolama türü içinde bir disk SKU önerir.<br/><br/> Eğer prim yönetilen diskler kullanarak göz önüne alındığında,% 99,9 tek bir örnek VM SLA elde etmek istiyorsanız. Bu, değerlendirmedeki tüm disklerin premium yönetilen diskler olarak tavsiye edilmesini sağlar.<br/><br/> Azure Geçişi yalnızca yönetilen disklerin geçiş değerlendirmesini destekler.
**Ayrılmış Örnekler (RIs)** | Değerlendirmedeki maliyet tahminlerinin RI indirimlerini hesaba katabilmesi için Azure'da [Ayrılmış Örnekleri](https://azure.microsoft.com/pricing/reserved-vm-instances/) belirtin.<br/><br/> RIs şu anda yalnızca Azure Geçiş'inde Yalnızca Sizden Önce Öde teklifleri için desteklenir.
**Boyutlandırma kriterleri** | Azure'da VM'yi sağ boyutlandırmak için kullanılır.<br/><br/> Olarak boyutlandırma veya performansa dayalı boyutlandırma kullanın.
**Performans geçmişi** | Performans tabanlı boyutlandırma ile kullanılır. Performans verilerini değerlendirirken kullanılan süreyi belirtin.
**Yüzdebirlik kullanımı** | Performans tabanlı boyutlandırma ile kullanılır. Doğru boyutlandırma için kullanılacak performans örneğinin yüzdelik değerini belirtir. 
**VM serisi** | Doğru boyutlandırma için göz önünde bulundurmak istediğiniz Azure VM serisini belirtin. Örneğin, Azure'da A serisi VM'lere ihtiyaç olan bir üretim ortamınız yoksa, A serisini listeden veya seriden hariç tutabilirsiniz.
**Konfor katsayısı** | Değerlendirme sırasında kullanılan arabellek. VM'ler (CPU, bellek, disk ve ağ) için makine kullanım verilerinin üstüne uygulanır. Mevsimsel kullanım, kısa performans geçmişi ve gelecekteki kullanımdaki olası artışlar gibi sorunları hesaba katmaktadır.<br/><br/> Örneğin, %20 kullanımlı 10 çekirdekli bir VM normalde iki çekirdekli vm ile sonuçlanır. 2.0x konfor faktörü ile, sonuç yerine dört çekirdekli VM olduğunu.
**Sunduğu** | Kaydolduğunuz [Azure teklifini](https://azure.microsoft.com/support/legal/offer-details/) görüntüler. Sunucu Değerlendirmesi maliyeti buna göre tahmin ediyor.
**Para birimi** | Hesabınız için faturalandırma para birimi.
**İndirim (%)** | Azure teklifinin üstünde aldığınız abonelme özel indirimleri listeler. Varsayılan ayar, %0’dır.
**VM çalışma süresi** | Azure VM'ler haftanın 7 günü 24 saat çalışmazlarsa, çalışacakları süreyi (ay daki gün ve günün saatleri) belirtebilirsiniz. Maliyet tahminleri buna göre işlenir.<br/><br/> Varsayılan değer ayda 31 gün ve günde 24 saattir.
**Azure Hibrit Avantajı** | Yazılım güvenceniz olup olmadığını ve [Azure Karma Avantajı](https://azure.microsoft.com/pricing/hybrid-use-benefit/)için uygun olup olmadığını belirtir. Evet olarak ayarlanmışsa (varsayılan ayar), Windows VM'ler için Windows Azure olmayan fiyatlar olarak kabul edilir.

Sunucu Değerlendirmesi ile değerlendirme oluşturmak için [en iyi uygulamaları gözden geçirin.](best-practices-assessment.md)


## <a name="calculate-readiness"></a>Hazır olma durumunu hesaplama

Tüm makineler Azure'da çalıştırılamayan makinelere uygun değildir. Sunucu Değerlendirmesi her şirket içi makineyi değerlendirir ve ona bir hazırlık kategorisi atar. 
- **Azure'a Hazır**: Makine herhangi bir değişiklik yapmadan Azure'a olduğu gibi geçirilebilir. Azure'da tam Azure desteği yle başlar.
- **Azure için koşullu olarak hazır**: Makine Azure'da başlayabilir, ancak tam Azure desteğine sahip olmayabilir. Örneğin, Windows Server'ın eski bir sürümünü çalıştıran bir makine Azure'da desteklenmez. Bu makineleri Azure'a geçirmeden önce dikkatli olmalısınız. Hazırlık sorunlarını gidermek için değerlendirmede önerilen düzeltme kılavuzunu izleyin.
- **Azure için hazır değil**: Makine Azure'da başlatılamayacak. Örneğin, şirket içi bir makine diski 64'ten fazla tonlaysa, Azure'da barındırılamaz. Geçişten önce sorunu gidermek için düzeltme kılavuzunu izleyin. 
- **Hazırlık :** Azure Geçir, yetersiz meta veri nedeniyle bir makinenin hazır olup olmadığını belirleyemedi.

Hazır olma durumunu hesaplamak için, Sunucu Değerlendirmesi aşağıdaki tablolarda özetlenen makine özelliklerini ve işletim sistemi ayarlarını gözden geçirir. 

### <a name="machine-properties"></a>Makine özellikleri

Sunucu Değerlendirmesi, Azure'da çalışıp çalışmayacağını belirlemek için şirket içi VM'nin aşağıdaki özelliklerini gözden geçirir.

**Özellik** | **Şey** | **Azure hazırlık durumu**
--- | --- | ---
**Önyükleme türü** | Azure, VM'leri UEFI ile değil, önyükleme türü BIOS ile destekler. | Önyükleme türü UEFI ise koşullu olarak hazır.
**Çekirdekler** | Makinelerdeki çekirdek sayısı, Azure VM için desteklenen maksimum çekirdek sayısına (128) eşit veya daha az olmalıdır.<br/><br/> Performans geçmişi varsa, Azure Geçir karşılaştırma için kullanılan çekirdekleri dikkate alır. Değerlendirme ayarlarında bir konfor faktörü belirtilirse, kullanılan çekirdek sayısı konfor faktörü ile çarpılır.<br/><br/> Performans geçmişi yoksa, Azure Geçir, konfor faktörlerini uygulamadan ayrılan çekirdekleri kullanır. | Limitlerden daha az veya eşitse hazır olun.
**Bellek** | Makine bellek boyutu, Azure M serisindeki maksimum belleğe (Azure M serisi Standard_M128m&nbsp;<sup>2)</sup>Azure VM için izin verilen maksimum belleğe (3892 gigabayt [GB] eşit veya daha az olmalıdır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).<br/><br/> Performans geçmişi varsa, Azure Geçir karşılaştırma için kullanılan belleği dikkate alır. Bir konfor faktörü belirtilirse, kullanılan bellek konfor faktörü ile çarpılır.<br/><br/> Geçmiş yoksa, ayrılan bellek konfor faktörü uygulanmadan kullanılır.<br/><br/> | Sınırlar içinde hazır.
**Depolama diski** | Bir diskin ayrılan boyutu 32 TB veya daha az olmalıdır. Azure, Ultra SSD disklere sahip 64 TB diskleri desteklese de, Azure Geçiş: Sunucu Değerlendirmesi, henüz Ultra SSD'yi desteklemediği için disk boyutu sınırlanınşu anda 32 TB'yi denetler. <br/><br/> Makineye iliştirilen disk sayısı işletim sistemi de dahil olmak üzere 65 veya daha az olmalıdır. | Sınırlar içinde hazır.
**Ağ** | Bir makinenin 32 veya daha az ağ arabirimi (NIC) eklenmiş olması gerekir. | Sınırlar içinde hazır.

### <a name="guest-operating-system"></a>Konuk işletim sistemi
Sunucu Değerlendirmesi, VM özellikleriyle birlikte, Azure'da çalışıp çalışmayacağını belirlemek için makinelerin konuk işletim sistemine bakar.

> [!NOTE]
> VMware VM'ler için Sunucu Değerlendirmesi, konuk işletim sistemi çözümlemesi için vCenter Server'daki VM için belirtilen işletim sistemini kullanır. VMware üzerinde çalışan Linux VM'ler için, şu anda konuk işletim sistemi tam çekirdek sürümünü tanımlamaz.

Aşağıdaki mantık, Sunucu Değerlendirmesi tarafından azure işletim sistemine dayalı hazır olup olmadığını tanımlamak için kullanılır.

**İşletim Sistemi** | **Şey** | **Azure hazırlık durumu**
--- | --- | ---
Windows Server 2016 tüm SP'leri & | Azure tam destek sağlar. | Azure için hazır
Windows Server 2012 R2 tüm SP'leri & | Azure tam destek sağlar. | Azure için hazır
Windows Server 2012 tüm SP'leri & | Azure tam destek sağlar. | Azure için hazır
Tüm SP'lerle Windows Server 2008 R2 | Azure tam destek sağlar.| Azure için hazır
Windows Server 2008 (32 bit ve 64 bit) | Azure tam destek sağlar. | Azure için hazır
Windows Server 2003, 2003 R2 | Bu işletim sistemleri destek bitiş tarihlerini geçti ve Azure'da destek için özel [destek sözleşmesine (CSA)](https://aka.ms/WSosstatement) ihtiyaç duyuyor. | Azure için koşullu olarak hazır. Azure'a geçmeden önce işletim sistemi yükseltmeyi düşünün.
Windows 2000, 98, 95, NT, 3.1, MS-DOS | Bu işletim sistemleri destek bitiş tarihini geçti. Makine Azure'da başlayabilir, ancak Azure işletim sistemi desteği sağlamaz. | Azure için koşullu olarak hazır. Azure'a geçmeden önce işletim sistemi yükseltmenizi öneririz.
Windows İstemci 7, 8 ve 10 | Azure yalnızca [Visual Studio aboneliği](https://docs.microsoft.com/azure/virtual-machines/windows/client-images) yle destek sağlar. | Azure için koşullu olarak hazır
Windows 10 Pro Masaüstü | Azure, [Multitenant Barındırma Hakları](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment) ile destek sağlar. | Azure için koşullu olarak hazır
Windows Vista, XP Professional | Bu işletim sistemleri destek bitiş tarihini geçti. Makine Azure'da başlayabilir, ancak Azure işletim sistemi desteği sağlamaz. | Azure için koşullu olarak hazır. Azure'a geçmeden önce işletim sistemi yükseltmenizi öneririz.
Linux | Azure bu [Linux işletim sistemlerini](../virtual-machines/linux/endorsed-distros.md)onaylar. Diğer Linux işletim sistemleri Azure'da başlayabilir, ancak Azure'a geçmeden önce işletim sistemi onaylı bir sürüme yükseltmenizi öneririz. | Sürüm onaylanırsa Azure için hazır olun.<br/><br/>Sürüm onaylanmazsa koşullu olarak hazır.
Diğer işletim sistemleri<br/><br/> Örneğin, Oracle Solaris, Apple macOS vb, FreeBSD, vb. | Azure bu işletim sistemlerini onaylamaz. Makine Azure'da başlayabilir, ancak Azure işletim sistemi desteği sağlamaz. | Azure için koşullu olarak hazır. Azure'a geçmeden önce desteklenen bir işletim sistemi yüklemenizi öneririz.  
vCenter Server'da **Diğer** olarak belirtilen işletim sistemi | Azure Geçir bu durumda işletim sistemi tanımlayamaz. | Bilinmeyen hazırlık. VM içinde çalışan işletim sistemi Azure'da desteklenir emin olun.
32 bit işletim sistemleri | Makine Azure'da başlayabilir, ancak Azure tam destek sağlamayabilir. | Azure için koşullu olarak hazır. Azure'a geçmeden önce makinenin işletim sistemi'ni 32 bit işletim sistemi'nden 64 bit işletim sistemi'ne yükseltmeyi düşünün.

## <a name="calculating-sizing"></a>Boyutlandırma nın hesaplanması


Makine Azure için hazır olarak işaretlendikten sonra, Sunucu Değerlendirmesi Azure VM ve disk SKU'yu tanımlamak için boyutlandırma önerileri yapar. Boyutlandırma hesaplamaları, şirket içi boyutlandırma mı yoksa performansa dayalı boyutlandırma mı kullandığınıza bağlıdır.

### <a name="calculate-sizing-as-is-on-premises"></a>Boyutlandırmayı hesaplama (şirket içinde olduğu gibi)

 Şirket içi boyutlandırma kullanıyorsanız, Sunucu Değerlendirmesi VM'lerin ve disklerin performans geçmişini dikkate almaz.

- **İşlem boyutlandırma**: Şirket içinde ayrılan boyuta göre bir Azure VM SKU ayırır.
- **Depolama/disk boyutlandırma**: Sunucu Değerlendirmesi, değerlendirme özelliklerinde (standart HDD/SSD/premium) belirtilen depolama türüne bakar ve disk türünü buna göre önerir. Varsayılan depolama türü premium disklerdir.
- **Ağ boyutlandırma**: Sunucu Değerlendirmesi, şirket içi makinedeki ağ bağdaştırıcısını dikkate alır.


### <a name="calculate-sizing-performance-based"></a>Boyutlandırmayı hesaplama (performans tabanlı)

Performans ayarı boyutlandırma kullanıyorsanız, Sunucu Değerlendirmesi boyutlandırma önerileri nde aşağıdaki gibidir:

- Sunucu Değerlendirmesi, Azure'daki VM boyutunu ve disk türünü tanımlamak için makinenin performans geçmişini dikkate alır.
- Sunucular bir CSV dosyası kullanılarak içe aktarıldıysa, belirttiğiniz değerler kullanılır. Bu yöntem, şirket içi makineyi aşırı olarak tahsis ettiyseniz, kullanım düşükse ve maliyetlerden tasarruf etmek için Azure'da VM'yi doğru boyutlandırmak istiyorsanız özellikle yararlıdır. 
- Performans verilerini kullanmak istemiyorsanız, boyutlandırma ölçütlerini önceki bölümde açıklandığı gibi şirket içinde olduğu gibi sıfırlayın.

#### <a name="calculate-storage-sizing"></a>Depolama boyutlandırmayı hesaplama

Azure Geçir, depolama alanı boyutlandırması için makineye bağlı her diski Azure'daki bir diskle eşlemeye çalışır ve aşağıdaki gibi çalışır:

1. Sunucu Değerlendirmesi, gerekli toplam IOPS'yi almak için bir diskin IOPS okuma ve yazma sını ekler. Benzer şekilde, her diskin toplam iş elde etmek için okuma ve yazma iş bölümü değerlerini ekler.
2. Etkili IOPS ve iş gücü değerlerini temel alan depolama türünü Otomatik olarak belirttiyseniz, Sunucu Değerlendirmesi diskin standart bir HDD'ye mi, standart SSD'ye mi yoksa Azure'da bir premium diske mi eşleneceğini belirler. Depolama türü Standart HDD/SSD/Premium olarak ayarlanmışsa, Sunucu Değerlendirmesi seçilen depolama türü içinde bir disk SKU bulmaya çalışır (Standart HDD/SSD/Premium diskler).
3. Diskler aşağıdaki gibi seçilir:
    - Sunucu Değerlendirmesi gerekli IOPS ve iş verime sahip bir disk bulamazsa, makineyi Azure için uygun olmayan olarak işaretler.
    - Sunucu Değerlendirmesi uygun diskler kümesi bulursa, değerlendirme ayarlarında belirtilen konumu destekleyen diskleri seçer.
    - Birden çok uygun disk varsa, Sunucu Değerlendirmesi en düşük maliyetle diski seçer.
    - Herhangi bir diskin performans verileri kullanılamıyorsa, Azure'da standart bir SSD diskbulmak için diskin yapılandırma verileri (disk boyutu) kullanılır.

#### <a name="calculate-network-sizing"></a>Ağ boyutlandırmayı hesaplama

Sunucu Değerlendirmesi, şirket içi makineye bağlı ağ bağdaştırıcılarının sayısını ve bu ağ bağdaştırıcılarının gerektirdiği performansı desteklenebilen bir Azure VM bulmaya çalışır.
- Şirket içi VM'nin etkili ağ performansını elde etmek için Sunucu Değerlendirmesi, saniyede iletilen verileri (MBps) makineden (ağ dışarı) tüm ağ bağdaştırıcılarından toplar ve konfor faktörünün geçerlidir. Bu numarayı, gerekli ağ performansını desteklenebilen bir Azure VM bulmak için kullanır.
- Sunucu Değerlendirmesi, ağ performansının yanı sıra Azure VM'nin gerekli ağ bağdaştırıcısayısını destekleyip desteklemeyeceğini de dikkate alır.
- Ağ performansı verisi yoksa, Sunucu Değerlendirmesi vm boyutlandırmaiçin yalnızca ağ bağdaştırıcısı sayısını dikkate alır.


#### <a name="calculate-compute-sizing"></a>İşlem boyutlandırmayı hesaplama

Depolama ve ağ gereksinimlerini hesapladıktan sonra, Sunucu Değerlendirmesi Azure'da uygun bir VM boyutu bulmak için CPU ve bellek gereksinimlerini dikkate alır.
- Azure Geçir, Azure'da uygun bir VM boyutu bulmak için etkili olarak kullanılan çekirdeklere ve belleğe bakar.
- Uygun boyut bulunmazsa, makine Azure için uygun olmadığı olarak işaretlenir.
- Uygun bir boyut bulunursa, Azure Geçiş depolama ve ağ hesaplamalarını uygular. Daha sonra son VM boyutu önerisi için konum ve fiyatlandırma katmanı ayarlarını uygular.
- Birden fazla uygun Azure sanal makinesi boyutu varsa, en düşük maliyetli olan önerilir.


## <a name="confidence-ratings-performance-based"></a>Güven derecelendirmeleri (performansa dayalı)

Azure Geçiş'teki her performans tabanlı değerlendirme, bir (en düşük) ile beş yıldız (en yüksek) arasında değişen bir güven derecelendirmesiyle ilişkilidir. Güven derecelendirmesi, Azure Geçiş tarafından sağlanan boyut önerilerinin güvenilirliğini tahmin etamamanıza yardımcı olur.

- Güvenilirlik derecelendirmesi, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliği temelinde bir değerlendirmeye atanır.
- Performansa dayalı boyutlandırma için Sunucu Değerlendirmesi gereksinimleri:
    - CPU ve VM bellek için kullanım verileri.
    - VM'ye iliştirilen her disk için disk IOPS ve iş verisi.
    - VM'ye bağlı her ağ bağdaştırıcısı için performans tabanlı boyutlandırmayı işlemek için ağ G/Ç.
    - Bu kullanım numaralarından herhangi biri kullanılamıyorsa, boyut önerileri güvenilir olmayabilir.

> [!NOTE]
> Alınan bir 'kullanılarak değerlendirilen sunucular için güven derecelendirmeleri atanmıyor. CSV dosyası. Derecelendirmeler, şirket içi değerlendirme için de geçerli değildir.
   
### <a name="ratings"></a>Derecelendirmeler

Kullanılabilir veri noktalarının yüzdeye bağlı olarak, değerlendirme için güven derecelendirmesi aşağıdaki gibi gider.

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
- Bazı VM'ler değerlendirmenin hesaplandığı dönemde kapatıldı. Herhangi bir VM belirli bir süre için kapatılırsa, Sunucu Değerlendirmesi o dönem için performans verilerini toplayamaz.
- Değerlendirmenin hesaplandığı dönemde bazı VM'ler oluşturuldu. Örneğin, geçen ayın performans geçmişi için bir değerlendirme oluşturduysanız, ancak bazı VM'ler yalnızca bir hafta önce ortamda oluşturulduysa, yeni VM'lerin performans geçmişi tam süre boyunca var olmaz.

> [!NOTE]
> Herhangi bir değerlendirmenin güven derecelendirmesi beş yıldızdan azsa, cihazın çevreprofilini niçin en az bir gün beklemenizi ve değerlendirmeyi yeniden hesaplamanızı öneririz. Bunu yapmazsanız, performans tabanlı boyutlandırma güvenilir olmayabilir. Bu durumda, değerlendirmeyi şirket içi boyutlandırmaya değiştirmenizi öneririz.

## <a name="calculate-monthly-costs"></a>Aylık maliyetleri hesaplama

Boyutlandırma önerileri tamamlandıktan sonra, Azure Geçirmi geçiş sonrası için işlem ve depolama maliyetlerini hesaplar.

- **İşlem maliyeti**: Önerilen Azure VM boyutunu kullanarak, Azure Geçir VM'nin aylık maliyetini hesaplamak için Faturalandırma API'sini kullanır.
    - Hesaplama, işletim sistemi, yazılım güvencesi, ayrılmış örnekler, VM çalışma süresi, konum ve para birimi ayarlarını dikkate alır.
    - Toplam aylık işlem maliyetini hesaplamak için tüm makinelerde maliyeti toplar.
- **Depolama maliyeti**: Bir makinenin aylık depolama maliyeti, makineye bağlı tüm disklerin aylık maliyeti aşağıdaki gibi toplayarak hesaplanır:
    - Sunucu Değerlendirmesi, tüm makinelerin depolama maliyetlerini toplayarak aylık depolama maliyetlerini hesaplar.
    - Şu anda, hesaplama değerlendirme ayarlarında belirtilen teklifleri dikkate almaz.

Maliyetler değerlendirme ayarlarında belirtilen para biriminde görüntülenir.


## <a name="next-steps"></a>Sonraki adımlar

Değerlendirme oluşturmak için en iyi uygulamaları [gözden geçirin.](best-practices-assessment.md) 


- [VMware VM'ler,](tutorial-prepare-vmware.md) [Hyper-V VM'ler](tutorial-prepare-hyper-v.md)ve [fiziksel sunucular](tutorial-prepare-physical.md)için değerlendirme çalıştırma hakkında bilgi edinin.
- [CSV dosyasıyla alınan](tutorial-assess-import.md)sunucuları değerlendirme hakkında bilgi edinin.
- [Bağımlılık görselleştirmeyi](concepts-dependency-visualization.md)ayarlama hakkında bilgi edinin.
