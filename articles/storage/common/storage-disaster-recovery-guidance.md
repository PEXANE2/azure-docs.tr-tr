---
title: Olağanüstü durum kurtarma ve depolama hesabı başarısız (önizleme)
titleSuffix: Azure Storage
description: Azure Depolama, coğrafi yedekli depolama hesapları için hesap başarısızlığı (önizleme) destekler. Hesap başarısız olduğunda, birincil bitiş noktası kullanılamıyorsa depolama hesabınız için başarısızlık işlemini başlatabilirsiniz.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: f7a8f6d0d3ab3b456c41128da9b689f6b7eda0f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79365381"
---
# <a name="disaster-recovery-and-account-failover-preview"></a>Olağanüstü durum kurtarma ve hesap başarısız (önizleme)

Microsoft, Azure hizmetlerinin her zaman kullanılabilir olmasını sağlamaya çalışır. Ancak, planlanmamış hizmet kesintileri oluşabilir. Uygulamanız esneklik gerektiriyorsa, Microsoft verilerinizin ikinci bir bölgeye kopyalanması için coğrafi yedekli depolama alanı kullanılmasını önerir. Ayrıca, müşterilerin bölgesel bir hizmet kesintisini işlemek için bir olağanüstü durum kurtarma planına sahip olması gerekir. Olağanüstü durum kurtarma planının önemli bir bölümü, birincil bitiş noktasının kullanılamaması durumunda ikincil bitiş noktasına kadar başarısız olmaya hazırlanıyor.

Azure Depolama, coğrafi yedekli depolama hesapları için hesap başarısızlığı (önizleme) destekler. Hesap başarısız olduğunda, birincil bitiş noktası kullanılamıyorsa depolama hesabınız için başarısızlık işlemini başlatabilirsiniz. Failover, depolama hesabınız için birincil bitiş noktası olmak için ikincil bitiş noktasını güncelleştirir. Başarısız nokta tamamlandıktan sonra, istemciler yeni birincil bitiş noktasına yazmaya başlayabilir.

[!INCLUDE [updated-for-az](../../../includes/storage-data-lake-gen2-support.md)]

Bu makalede, bir hesap başarısızlığıyla ilgili kavramları ve süreci açıklar ve depolama hesabınızı en az müşteri etkisiyle kurtarma için nasıl hazırlayacağınızı tartışır. Azure portalında veya PowerShell'de bir hesap başarısızmasını nasıl başlatabileceğinizi öğrenmek için [bkz.](storage-initiate-account-failover.md)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="choose-the-right-redundancy-option"></a>Doğru artıklık seçeneğini seçin

Azure Depolama, dayanıklılık ve yüksek kullanılabilirlik sağlamak için depolama hesabınızın birden çok kopyasını saklar. Hesabınız için seçtiğiniz fazlalık seçeneği, ihtiyacınız olan esneklik derecesine bağlıdır. Bölgesel kesintilere karşı koruma için, ikincil bölgeden okuma erişimi seçeneği olan veya olmayan coğrafi yedekli depolamayı seçin:  

**Coğrafi yedekli depolama (GRS) veya coğrafi bölge yedekli depolama (GZRS) (önizleme)** verilerinizi en az yüzlerce mil arayla iki coğrafi bölgede eşzamanlı olarak kopyalar. Birincil bölge bir kesinti yaşıyorsa, ikincil bölge verileriniz için gereksiz bir kaynak olarak hizmet vermektedir. İkincil bitiş noktasını birincil bitiş noktasına dönüştürmek için bir başarısızlık başlatabilirsiniz.

**Okuma-erişim coğrafi yedekli depolama (RA-GRS) veya okuma-erişim coğrafi bölge yedekli depolama (RA-GZRS) (önizleme)** ikincil uç noktaya okuma erişimi ek yararı ile coğrafi yedekli depolama sağlar. Birincil bitiş noktasında bir kesinti oluşursa, RA-GRS için yapılandırılan ve yüksek kullanılabilirlik için tasarlanmış uygulamalar ikincil bitiş noktasından okumaya devam edebilir. Microsoft, uygulamalarınız için maksimum esneklik için RA-GRS önerir.

Azure Depolama'da artıklık hakkında daha fazla bilgi için [Azure Depolama artıklığı'na](storage-redundancy.md)bakın.

> [!WARNING]
> Coğrafi yedekli depolama veri kaybı riski taşır. Veriler eşzamanlı olarak ikincil bölgeye kopyalanır, yani birincil bölgeye yazılan verilerin ikincil bölgeye yazılması arasında bir gecikme olur. Bir kesinti durumunda, henüz ikincil bitiş noktasına kopyalanmamış birincil bitiş noktasına yazma işlemleri kaybolur.

## <a name="design-for-high-availability"></a>Yüksek kullanılabilirliğe yönelik tasarım

Uygulamanızı en başından itibaren yüksek kullanılabilirlik için tasarlamak önemlidir. Uygulamanızı tasarlamave olağanüstü durum kurtarma planlamasında rehberlik etmek için bu Azure kaynaklarına bakın:

- [Azure için esnek uygulamalar tasarlama](/azure/architecture/checklist/resiliency-per-service): Azure'da yüksek kullanılabilir uygulamaları tasarlamak için temel kavramlara genel bakış.
- [Kullanılabilirlik denetim listesi](/azure/architecture/checklist/resiliency-per-service): Uygulamanızın yüksek kullanılabilirlik için en iyi tasarım uygulamalarını uyguladığını doğrulamak için bir denetim listesidir.
- [RA-GRS kullanarak yüksek kullanılabilir uygulamaların tasarlanması : RA-GRS'den](storage-designing-ha-apps-with-ragrs.md)yararlanmak için bina uygulamaları için tasarım kılavuzu.
- [Öğretici: Blob depolama ile son derece kullanılabilir bir uygulama oluşturun](../blobs/storage-create-geo-redundant-storage.md): Hatalar ve kurtarmalar simüle edildikçe uç noktalar arasında otomatik olarak geçiş yapan yüksek kullanılabilir bir uygulamanın nasıl oluşturulabildiğini gösteren bir öğretici. 

Ayrıca, Azure Depolama verileriniz için yüksek kullanılabilirlik sağlamak için en iyi uygulamaları da aklınızda bulundurun:

- **Diskler:** Azure sanal makineleriniz tarafından kullanılan VM disklerini yedeklemek için [Azure Yedekleme'yi](https://azure.microsoft.com/services/backup/) kullanın. Ayrıca, bölgesel bir felaket durumunda VM'lerinizi korumak için [Azure Site Kurtarma'yı](https://azure.microsoft.com/services/site-recovery/) kullanmayı da düşünün.
- **Blok lekeleri:** [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md)veya [Azure Veri Hareketi kitaplığını](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/)kullanarak nesne düzeyinde ki silmelere ve üzerine yazmalara karşı korumak için [yumuşak silmeyi](../blobs/storage-blob-soft-delete.md) açın veya blok lekelerini farklı bir bölgedeki başka bir depolama hesabına kopyalayın.
- **Dosyalar:** Dosyalarınızı farklı bir bölgedeki başka bir depolama hesabına kopyalamak için [AzCopy](storage-use-azcopy.md) veya [Azure PowerShell'i](storage-powershell-guide-full.md) kullanın.
- **Tablolar:** Tablo verilerini farklı bir bölgedeki başka bir depolama hesabına aktarmak için [AzCopy'yi](storage-use-azcopy.md) kullanın.

## <a name="track-outages"></a>Kesintileri takip et

Müşteriler, Azure Depolama ve diğer Azure hizmetlerinin sistem durumunu ve durumunu izlemek için [Azure Hizmet Durumu Panosu'na](https://azure.microsoft.com/status/) abone olabilir.

Microsoft ayrıca, yazma hataları olasılığına hazırlanmak için uygulamanızı tasarlamanızı önerir. Uygulamanız, yazma hatalarını birincil bölgede bir kesinti olasılığına karşı sizi uyaracak şekilde ortaya çıkarmalıdır.

## <a name="understand-the-account-failover-process"></a>Hesap başarısız ı işlemini anlama

Müşteri tarafından yönetilen hesap başarısız olması (önizleme), birincil hesap herhangi bir nedenle kullanılamıyorsa, tüm depolama hesabınızı ikincil bölgeye geçmenizi sağlar. İkincil bölgeye bir başarısızlık zorladığınızda, istemciler hata tamamlandıktan sonra ikincil bitiş noktasına veri yazmaya başlayabilir. Başarısızlık genellikle yaklaşık bir saat sürer.

### <a name="how-an-account-failover-works"></a>Hesap yük devretmesi nasıl çalışır?

Normal koşullarda, bir istemci verileri birincil bölgedeki bir Azure Depolama hesabına yazar ve bu veriler ikincil bölgeye eşit olarak kopyalanır. Aşağıdaki resim, birincil bölge kullanılabilir olduğunda senaryoyu gösterir:

![İstemciler birincil bölgedeki depolama hesabına veri yazar](media/storage-disaster-recovery-guidance/primary-available.png)

Birincil bitiş noktası herhangi bir nedenle kullanılamaz hale gelirse, istemci artık depolama hesabına yazamaz. Aşağıdaki resim, birincil kullanılamaz hale geldi, ancak henüz hiçbir kurtarma gerçekleşmedi senaryogösterir:

![Birincil kullanılabilir, bu nedenle istemciler veri yazamaz](media/storage-disaster-recovery-guidance/primary-unavailable-before-failover.png)

Müşteri hesap hatasını ikincil bitiş noktasına doğru başlatır. Başarısız işlem, Azure Depolama tarafından sağlanan DNS girişini güncelleştirir, böylece ikincil bitiş noktası aşağıdaki resimde gösterildiği gibi depolama hesabınız için yeni birincil bitiş noktası olur:

![Müşteri ikincil bitiş noktasına hesap başarısızmı başlatır](media/storage-disaster-recovery-guidance/failover-to-secondary.png)

DNS girişi güncelleştirildikten ve istekler yeni birincil bitiş noktasına yönlendirildikten sonra GRS ve RA-GRS hesapları için yazma erişimi geri yüklenir. Lekeler, tablolar, kuyruklar ve dosyalar için varolan depolama hizmeti uç noktaları, hatadan sonra aynı kalır.

> [!IMPORTANT]
> Hata tamamlandıktan sonra, depolama hesabı yeni birincil bitiş noktasında yerel olarak yedeklenecek şekilde yapılandırılır. Yeni ikincil çoğaltma devam etmek için, yeniden coğrafi yedekli depolama kullanmak için hesap yapılandırır (RA-GRS veya GRS).
>
> Bir LRS hesabını RA-GRS veya GRS'ye dönüştürmenin bir maliyete yol açabilen bir maliyete yol açadığını unutmayın. Bu maliyet, yeni birincil bölgedeki depolama hesabının bir arızadan sonra RA-GRS veya GRS'yi kullanmak üzere güncelleştirilmesi için geçerlidir.  

### <a name="anticipate-data-loss"></a>Veri kaybını tahmin edin

> [!CAUTION]
> Bir hesap başarısız genellikle bazı veri kaybı içerir. Bir hesap başarısızlığı başlatmanın sonuçlarını anlamak önemlidir.  

Veriler birincil bölgeden ikincil bölgeye eş senkronize olarak yazıldığından, birincil bölgeye yazmanın ikincil bölgeye kopyalanmasını önce her zaman bir gecikme olur. Birincil bölge kullanılamıyorsa, en son yazmalar henüz ikincil bölgeye kopyalanmamış olabilir.

Bir başarısızlığı zorladığınızda, ikincil bölge yeni birincil bölge haline geldikçe birincil bölgedeki tüm veriler kaybolur ve depolama hesabı yerel olarak gereksiz olacak şekilde yapılandırılır. Zaten ikincil kopyalanan tüm veriler failover gerçekleştiğinde korunur. Ancak, birincil olarak yazılan ve ikincil kopyaya kopyalanmamış tüm veriler kalıcı olarak kaybolur.

**Son Eşitleme Zamanı** özelliği, birincil bölgeden gelen verilerin ikincil bölgeye yazıldığının garanti edildiği en son zamanı gösterir. Son eşitleme saatinden önce yazılan tüm veriler ikincil de kullanılabilirken, son eşitleme süresinden sonra yazılan veriler ikincil verilere yazılmamış olabilir ve kaybolabilir. Bir hesap başarısızlığı başlatarak ortaya çıkabilecek veri kaybı miktarını tahmin etmek için bir kesinti durumunda bu özelliği kullanın.

En iyi uygulama olarak, beklenen veri kaybını değerlendirmek için son eşitleme süresini kullanabilmeniz için uygulamanızı tasarlayın. Örneğin, tüm yazma işlemlerini günlüğe kaydediyorsanız, hangi yazmaların ikincil işlemle eşitlenmediğini belirlemek için son yazma işlemlerinizi son eşitleme süresiyle karşılaştırabilirsiniz.

### <a name="use-caution-when-failing-back-to-the-original-primary"></a>Orijinal birincil geri başarısız olduğunda dikkatli olun

Birincil bölgeden ikincil bölgeye başarısız olduktan sonra, depolama hesabınız yeni birincil bölgede yerel olarak yedeklenecek şekilde yapılandırılır. GS veya RA-GRS kullanmak üzere güncelayarlayarak hesabı coğrafi artıklık için yeniden yapılandırabilirsiniz. Hesap bir hatadan sonra yeniden coğrafi artıklık için yapılandırıldığında, yeni birincil bölge hemen verileri orijinal hatadan önce birincil olan yeni ikincil bölgeye kopyalamaya başlar. Ancak, birincil mevcut verilerin tamamen yeni ikincil kopyalanır önce biraz zaman alabilir.

Depolama hesabı coğrafi artıklık için yeniden yapılandırıldıktan sonra, yeni birincil den yeni ikincil geri başka bir hata başlatmak mümkündür. Bu durumda, başarısızolmadan önceki özgün birincil bölge yeniden birincil bölge olur ve yerel olarak gereksiz olacak şekilde yapılandırılır. Başarısız sonrası birincil bölgedeki tüm veriler (özgün ikincil) sonra kaybolur. Depolama hesabındaki verilerin çoğu geri başarısız olmadan önce yeni ikincil kopyalanmadıysa, büyük bir veri kaybına uğrayabilirsiniz.

Büyük bir veri kaybını önlemek için, geri başarısız olmadan önce **Son Eşitleme Zamanı** özelliğinin değerini denetleyin. Beklenen veri kaybını değerlendirmek için son eşitleme süresini yeni birincile veri yazıldığı son kez karşılaştırın. 

## <a name="initiate-an-account-failover"></a>Hesap yük devretmesi başlatma

Azure portalı, PowerShell, Azure CLI veya Azure Depolama kaynak sağlayıcısı API'den hesap kesintisi başlatabilirsiniz. Bir hatanın nasıl başlatılabildiğini hakkında daha fazla bilgi için [bkz.](storage-initiate-account-failover.md)

## <a name="about-the-preview"></a>Önizleme hakkında

Hesap başarısız mısrı, Azure Kaynak Yöneticisi dağıtımları ile GRS veya RA-GRS kullanan tüm müşteriler için önizlemede kullanılabilir. Genel amaçlı v1, Genel amaçlı v2 ve Blob depolama hesap türleri desteklenir. Hesap başarısız olması şu anda tüm ortak bölgelerde kullanılabilir. Hesap başarısız mı şu anda egemen/ulusal bulutlarda kullanılamıyor.

Önizleme yalnızca üretim dışı kullanım için tasarlanmıştır. Üretim hizmeti düzeyindeki anlaşmalar (SLA'lar) şu anda kullanılamıyor.

### <a name="additional-considerations"></a>Diğer konular

Önizleme süresi boyunca başarısız olmaya zorladığınızda uygulamalarınızın ve hizmetlerinizin nasıl etkilenebileceğini anlamak için bu bölümde açıklanan ek hususları gözden geçirin.

#### <a name="storage-account-containing-archived-blobs"></a>Arşivlenmiş lekeler içeren depolama hesabı

Arşivlenmiş blobs destek hesabı failover içeren depolama hesapları. Başarısız bir şekilde tamamlandığında, hesabı GRS veya RA-GRS'ye dönüştürmek için arşivlenmiş tüm lekelerin önce çevrimiçi bir katmana yeniden susuz kalması gerekir.

#### <a name="storage-resource-provider"></a>Depolama kaynak sağlayıcısı

Bir hata tamamlandıktan sonra, istemciler yeni birincil bölgede Azure Depolama verilerini yeniden okuyabilir ve yazabilir. Ancak, Azure Depolama kaynak sağlayıcısı başarısız olmadığı ndan kaynak yönetimi işlemlerinin yine de birincil bölgede gerçekleşmesi gerekir. Birincil bölge kullanılamıyorsa, depolama hesabında yönetim işlemleri gerçekleştiremeyeceksiniz.

Azure Depolama kaynak sağlayıcısı başarısız olmadığından, [Konum](/dotnet/api/microsoft.azure.management.storage.models.trackedresource.location) özelliği, hata tamamlandıktan sonra özgün birincil konumu döndürecektir.

#### <a name="azure-virtual-machines"></a>Azure sanal makineleri

Azure sanal makineleri (VM'ler) bir hesap başarısızlığı nedeniyle başarısız olmaz. Birincil bölge kullanılamaz hale gelirse ve ikincil bölgeye geçemezse, başarısız olduktan sonra herhangi bir VM'yi yeniden oluşturmanız gerekir. Ayrıca, hesap başarısız olmasıyla ilişkili olası bir veri kaybı vardır. Microsoft, Azure'daki sanal makinelere özel aşağıdaki [yüksek kullanılabilirlik](../../virtual-machines/windows/manage-availability.md) ve [olağanüstü durum kurtarma](../../virtual-machines/virtual-machines-disaster-recovery-guidance.md) kılavuzunu önerir.

#### <a name="azure-unmanaged-disks"></a>Azure yönetilmeyen diskler

En iyi yöntem olarak Microsoft, yönetilmeyen diskleri yönetilen disklere dönüştürmenizi önerir. Ancak, Azure VM'lerine bağlı yönetilmeyen diskler içeren bir hesap üzerinde başarısız olmanız gerekiyorsa, başarısız olmayı başlatmadan önce VM'yi kapatmanız gerekir.

Yönetilmeyen diskler Azure Depolama'da sayfa blob'ları olarak depolanır. Azure'da bir VM çalışırken, VM'ye bağlı yönetilmeyen diskler kiralanır. Bir hesap başarısızlığı, bir blob üzerinde bir kira olduğunda devam edemez. Başarısız olmayı gerçekleştirmek için aşağıdaki adımları izleyin:

1. Başlamadan önce, yönetilmeyen disklerin adlarını, mantıksal birim numaralarını (LUN) ve bağlı oldukları VM'yi not edin. Bunu yapmak, başarısız olduktan sonra disklerin yeniden dikilmesini kolaylaştırır.
2. VM'yi kapatın.
3. VM'yi silin, ancak yönetilmeyen diskler için VHD dosyalarını saklar. VM'yi sildiğiniz zamana dikkat edin.
4. **Son Eşitleme Süresi** güncellenene ve VM'yi sildiğiniz zamandan daha geç olana kadar bekleyin. Bu adım önemlidir, çünkü başarısızlık gerçekleştiğinde ikincil bitiş noktası VHD dosyalarıyla tam olarak güncelleştirilemediyse, VM yeni birincil bölgede düzgün çalışmayabilir.
5. Hesap başarısızını başlatın.
6. Hesap başarısız mıtamamla ve ikincil bölge yeni birincil bölge haline gelene kadar bekleyin.
7. Yeni birincil bölgede bir VM oluşturun ve VHD'leri yeniden takın.
8. Yeni VM'yi başlatın.

Geçici bir diskte depolanan verilerin VM kapatıldığında kaybolduğunu unutmayın.

### <a name="unsupported-features-and-services"></a>Desteklenmeyen özellikler ve hizmetler

Aşağıdaki özellikler ve hizmetler önizleme sürümü için hesap başarısızlığına karşı desteklenmez:

- Azure Dosya Eşitlemesi depolama hesabının başarısız olduğunu desteklemez. Azure Dosya Eşitleme'de bulut uç noktaları olarak kullanılan Azure dosya paylaşımlarının bulunduğu depolama hesapları yük devretmemelidir. Bunun yapılması eşitlemenin çalışmayı durdurmasına neden olur ve yeni katmanlanmış dosyalar söz konusu olduğunda beklenmedik veri kaybına da yol açabilir.
- ADLS Gen2 depolama hesapları (hiyerarşik ad alanı etkinleştirilmiş hesaplar) şu anda desteklenmez.
- Premium blok lekeleri içeren bir depolama hesabı üzerinde başarısız olamaz. Premium blok bloblarını destekleyen depolama hesapları şu anda coğrafi artıklığı desteklemez.
- [Worm immutability ilkesi](../blobs/storage-blob-immutable-storage.md) etkin kaplar içeren bir depolama hesabı üzerinde başarısız olamaz. Kilitsiz/kilitli zaman tabanlı saklama veya yasal tutma ilkeleri, uyumluluğu korumak için başarısız olmayı önler.
- Başarısız lık tamamlandıktan sonra, başlangıçta etkinleştirilmişse aşağıdaki özellikler çalışmayı durdurabilir: [Etkinlik abonelikleri,](../blobs/storage-blob-event-overview.md) [Özet Akışı Değiştir,](../blobs/storage-blob-change-feed.md) [Yaşam Döngüsü ilkeleri](../blobs/storage-lifecycle-management-concepts.md)ve Depolama [Analizi Günlüğü](storage-analytics-logging.md).

## <a name="copying-data-as-an-alternative-to-failover"></a>Başarısız olmaya alternatif olarak verileri kopyalama

Depolama hesabınız RA-GRS için yapılandırıldıysa, ikincil bitiş noktasını kullanarak verilerinize erişimi okumuşsunuzdur. Birincil bölgede bir kesinti durumunda başarısız olmayı tercih ederseniz, ikincil bölgedeki depolama hesabınızdaki depolama hesabınızdaki verileri etkilenmemiş bir bölgedeki başka bir depolama hesabına kopyalamak için [AzCopy](storage-use-azcopy.md), [Azure PowerShell](storage-powershell-guide-full.md)veya [Azure Veri Hareketi kitaplığı](https://azure.microsoft.com/blog/introducing-azure-storage-data-movement-library-preview-2/) gibi araçları kullanabilirsiniz. Daha sonra hem okuma hem de yazma kullanılabilirliği için uygulamalarınızı bu depolama hesabına yönlendirebilirsiniz.

> [!CAUTION]
> Bir hesap başarısızlığı, veri geçiş stratejinizin bir parçası olarak kullanılmamalıdır.


## <a name="microsoft-managed-failover"></a>Microsoft tarafından yönetilen failover

Önemli bir felaket nedeniyle bir bölgenin kaybolduğu ekstrem durumlarda, Microsoft bölgesel bir hata başlatabilir. Bu durumda, sizin tarafınızdan herhangi bir işlem yapılması gerekmez. Microsoft tarafından yönetilen başarısız olana kadar depolama hesabınıza yazma erişiminiz olmaz. Depolama hesabınız RA-GRS için yapılandırıldıysa, uygulamalarınız ikincil bölgeden okunabilir. 

## <a name="see-also"></a>Ayrıca bkz.

- [Hesap başarısızmı başlatma (önizleme)](storage-initiate-account-failover.md)
- [RA-GRS’yi kullanarak yüksek kullanılabilirliğe sahip uygulamalar tasarlama](storage-designing-ha-apps-with-ragrs.md)
- [Öğretici: Blob depolama ile son derece kullanılabilir bir uygulama oluşturun](../blobs/storage-create-geo-redundant-storage.md) 
