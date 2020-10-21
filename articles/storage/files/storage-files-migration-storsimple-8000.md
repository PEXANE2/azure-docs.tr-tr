---
title: Azure Dosya Eşitleme 'e StorSimple 8000 serisi geçişi
description: StorSimple 8100 veya 8600 gerecini Azure Dosya Eşitleme 'e geçirmeyi öğrenin.
author: fauhse
ms.service: storage
ms.topic: how-to
ms.date: 10/16/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: dcb690829aaed1d5a256f53766f05c922f8b2a9a
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92331123"
---
# <a name="storsimple-8100-and-8600-migration-to-azure-file-sync"></a>StorSimple 8100 ve 8600 Azure Dosya Eşitleme 'e geçiş

StorSimple 8000 serisi, 8100 ya da 8600 fiziksel, şirket içi gereçlerden ve bulut hizmeti bileşenleriyle temsil edilir. Bu gereçlerden birindeki verileri bir Azure Dosya Eşitleme ortamına geçirmek mümkündür. Azure Dosya Eşitleme, StorSimple gereçlerinin geçirilebilmesi için varsayılan ve stratejik, uzun süreli Azure hizmetidir.

StorSimple 8000 serisi, adım 2022 ' de [yaşam sonuna](https://support.microsoft.com/en-us/lifecycle/search?alpha=StorSimple%208000%20Series) ulaşacaktır. Geçişinizi mümkün olan en kısa sürede planlamaya başlamak önemlidir. Bu makalede, Azure Dosya Eşitleme başarılı bir geçiş için gereken arka plan bilgisi ve geçiş adımları sağlanmaktadır. 

## <a name="phase-1-prepare-for-migration"></a>1. Aşama: geçişe hazırlanma

Bu bölüm, StorSimple birimlerinden Azure dosya paylaşımlarına geçişinizin başlangıcında gerçekleştirmeniz gereken adımları içerir.

### <a name="inventory"></a>Envanter
Geçişinizi planlamaya başladığınızda öncelikle geçirmeniz gereken tüm StorSimple gereçlerini ve birimlerini belirlemeniz gerekir. Bunu yaptıktan sonra, sizin için en iyi geçiş yoluna karar verebilirsiniz.

* StorSimple Fiziksel gereçlerine (8000 serisi) bu geçiş kılavuzunu kullanın. 
* Sanal gereçler, [StorSimple 1200 serisi, farklı bir geçiş kılavuzu kullanın](storage-files-migration-storsimple-1200.md).

### <a name="migration-cost-summary"></a>Geçiş maliyeti Özeti
StorSimple Veri Yöneticisi bir kaynaktaki veri dönüştürme işleri aracılığıyla StorSimple birimlerinden Azure dosya paylaşımlarına geçiş yapmak ücretsizdir. Ancak, bir geçiş sırasında ve sonrasında tahakkuk eden başka maliyetler de mevcuttur:

* **Ağ çıkış:** StorSimple dosyalarınız, belirli bir Azure bölgesindeki bir depolama hesabında etkin. İçinde geçiş yaptığınız Azure dosya paylaşımlarını aynı Azure bölgesinde bulunan bir depolama hesabında temin ediyorsanız, çıkış maliyeti gerçekleşmez. Bu geçişin bir parçası olarak, dosyalarınızı farklı bir bölgedeki depolama hesabına taşıyabilirsiniz. Bu durumda, çıkış maliyetleri sizin için geçerlidir.
* **Azure dosya paylaşma işlemleri:** Dosyalar bir Azure dosya paylaşımında kopyalandığında (geçişin bir parçası veya bir diğeri dışında), dosya ve meta veriler yazıldığı sürece işlem maliyetleri geçerlidir. En iyi uygulama olarak, geçiş sırasında işlem için iyileştirilmiş katmanda Azure dosya paylaşımınızı başlatın. Geçiş tamamlandıktan sonra istediğiniz katmana geçiş yapın. Aşağıdaki aşamalar bunu uygun noktada çağıracaktır.
* **Azure dosya paylaşma katmanını değiştirme:** Azure dosya paylaşma maliyeti işlemlerinin katmanını değiştirme. Çoğu durumda, önceki noktadan gelen önerileri izlemek daha uygun maliyetli olacaktır.
* **Depolama maliyeti:** Bu geçiş dosyaları bir Azure dosya paylaşımında kopyalamaya başladığında Azure dosya depolaması tüketilir ve faturalandırılır. 
* **StorSimple:** StorSimple cihazlarının ve depolama hesaplarının sağlanması ihtimaline kadar depolama, yedeklemeler ve Gereçler için StorSimple maliyeti gerçekleşmeye devam edecektir.

### <a name="direct-share-access-vs-azure-file-sync"></a>Doğrudan paylaşma erişimi vs. Azure Dosya Eşitleme
Azure dosya paylaşımları, dosya hizmetleri dağıtımınızı yapılandırmak için yepyeni bir fırsat dünyasını açar. Azure dosya paylaşımında yalnızca bulutta bulunan bir SMB paylaşımıdır. kullanıcılar yerel olarak çalışan tanıdık Kerberos kimlik doğrulaması ve var olan NTFS izinleri (dosya ve klasör ACL 'Leri) ile SMB protokolü üzerinde doğrudan erişim sahibi olacak şekilde ayarlayabiliyor. [Azure dosya paylaşımlarına kimlik tabanlı erişim hakkında daha fazla bilgi edinin](storage-files-active-directory-overview.md).

Doğrudan erişime bir alternatifi [Azure dosya eşitleme](https://aka.ms/AFS). Azure Dosya Eşitleme, StorSimple 'ın şirket içinde sık kullanılan dosyaları önbelleğe alma yeteneğine yönelik doğrudan bir örneksel işlemdir. 

Azure Dosya Eşitleme, iki ana bileşeni temel alan bir Microsoft bulut hizmetidir:

* Dosya eşitleme ve bulut katmanlama.
* Azure 'da, SMB ve dosya geri kalanı gibi birden çok protokol üzerinden erişilebilen yerel depolama olarak dosya paylaşımları. 

Azure dosya paylaşımları, öznitelikler, izinler ve zaman damgaları gibi depolanmış dosyalardaki önemli dosya uygunluk yönlerini korurlar. Azure dosya paylaşımları ile, artık bulutta depolanan dosya ve klasörleri yorumlamak için bir uygulama veya hizmet gereksinimi yoktur. Bunlara Windows Dosya Gezgini gibi tanıdık protokoller ve istemciler üzerinden yerel olarak erişebilirsiniz. Azure dosya paylaşımları, genel amaçlı dosya sunucusu verilerini ve uygulama verilerini bulutta depolamanıza olanak tanır. Azure dosya paylaşımının yedeklenmesi yerleşik bir işlevdir ve Azure Backup tarafından daha da geliştirilebilir.

Bu makalede geçiş adımları ele alınmaktadır. Azure Dosya Eşitleme hakkında daha fazla bilgi edinmek istiyorsanız, aşağıdaki makaleleri öneririz:

* [Azure Dosya Eşitleme-genel bakış](https://aka.ms/AFS "Genel Bakış")
* [Azure Dosya Eşitleme dağıtım kılavuzu](storage-sync-files-deployment-guide.md)

### <a name="storsimple-service-data-encryption-key"></a>StorSimple hizmeti veri şifreleme anahtarı
StorSimple gerecinizi ilk kez ayarladığınızda, bir '*hizmet veri şifreleme anahtarı*' oluşturdu ve anahtarı güvenli bir şekilde depolamanızı istedi.
Bu anahtar, StorSimple gerecinin dosyalarınızı depoladığı ilişkili Azure depolama hesabındaki tüm verileri şifrelemek için kullanılır.

Başarılı bir geçiş için ' hizmet veri şifreleme anahtarı ' gereklidir. Şimdi, stoğunuzdaki her bir gereçde bu anahtarı almak için uygun bir zamandır.

Kayıtlarınızda anahtar bulamazsanız, bu anahtarı gereç içinden alabilirsiniz. Her gereç benzersiz bir şifreleme anahtarına sahiptir.

* Azure portal üzerinden Microsoft Azure bir destek isteği dosyası. İsteğin içeriği StorSimple cihaz seri numaralarına ve "hizmet verileri şifreleme anahtarı" alma isteğine sahip olmalıdır. 
* StorSimple Destek Mühendisi bir ekran paylaşım toplantısı isteğiyle sizinle iletişim kuracaktır.
* Toplantıdan başlamadan önce, [bir seri konsol](../../storsimple/storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) veya [uzak bir PowerShell oturumu](../../storsimple/storsimple-8000-windows-powershell-administration.md#connect-remotely-to-storsimple-using-windows-powershell-for-storsimple)aracılığıyla StorSimple gerecinize bağlantığınızdan emin olun. 

> [!CAUTION]
> StorSimple gerecinize nasıl bağlanacağınıza karar verirken aşağıdakileri göz önünde bulundurun:
> * HTTPS oturumu üzerinden bağlanmak en güvenli ve önerilen seçenektir.
> * Doğrudan cihaz seri konsoluna bağlanmak güvenlidir, ancak ağ anahtarları üzerinden seri konsoluna bağlanmak değildir. 
> * HTTP oturum bağlantıları bir seçenektir, ancak **şifrelenmez** ve bu nedenle kapatılmış, güvenilen bir ağda kullanılmadığı takdirde önerilmez.

### <a name="storsimple-volume-backups"></a>StorSimple birim yedeklemeleri
StorSimple, birim düzeyinde değişiklik yedeklemeleri sunmaktadır. Azure dosya paylaşımları, paylaşım anlık görüntüleri olarak da adlandırılan bu becerisine sahiptir.

Geçişinizin bir parçası olarak ne olduğuna karar verin, her türlü yedeklemeyi taşıma yükümlülüğü de vardır.

> [!CAUTION]
> , StorSimple birimlerinden yedeklemeleri geçirmeniz gerekiyorsa, bunu durdurun.
>
> Şu anda yalnızca en son birim yedeklemenizi geçirebilirsiniz. Yedekleme geçişine yönelik destek, 2020 sonuna ulaştığında gönderilir.
> Şimdi başlatırsanız, yedeklemelerinizi daha sonra "başlatamazsınız".
> Gelecek sürümde, yedeklemelerin Azure dosya paylaşımı anlık görüntülerinin aralarında yer aldığı Azure dosya paylaşımlarına en eskiye "geri oynamaları" gerekir.

Yalnızca canlı verileri geçirmek istiyorsanız ve yedeklemeler için herhangi bir gereksinim yoksa, bu kılavuzu izleyerek devam edebilirsiniz.
Yalnızca çok kısa süreli yedek saklama gereksinimine sahipseniz, bir ay veya iki durumda, geçişinize hemen devam etmeye ve bu dönemden sonra StorSimple kaynaklarınızı sağlamayı seçebilirsiniz. Bu yaklaşım, Azure dosya paylaşımında ihtiyacınız olduğu kadar yedekleme geçmişi oluşturmanızı sağlar. Her iki sistemi de çalıştırdığınız zaman, ek maliyet uygulanır ve bu, çok sayıda kısa süreli yedekleme saklamasına ihtiyacınız olduğunda göz önünde bulundurmanız gereken bir yaklaşım değildir.

### <a name="map-your-existing-storsimple-volumes-to-azure-file-shares"></a>Mevcut StorSimple birimlerinizi Azure dosya paylaşımlarına eşleyin
[!INCLUDE [storage-files-migration-namespace-mapping](../../../includes/storage-files-migration-namespace-mapping.md)]

### <a name="number-of-storage-accounts"></a>Depolama hesabı sayısı
Geçişiniz büyük olasılıkla birden çok depolama hesabı dağıtımından faydalanır, bu da her biri daha az sayıda Azure dosya paylaşımı tutmaktadır.

Dosya paylaşımlarınız yüksek düzeyde etkin ise (çok sayıda kullanıcı ve/veya uygulama tarafından kullanılan), yalnızca iki Azure dosya paylaşımı depolama hesabınızın performans sınırına ulaşabilirler. Bu nedenle, en iyi uygulama, her biri kendi bireysel dosya paylaşımlarına ve genellikle depolama hesabı başına ikiden fazla ya da üç paylaşımdan daha fazla depolama hesabına geçmeyecektir.

En iyi yöntem, depolama hesaplarını her bir dosya paylaşımıyla dağıtmaktır. Birden çok Azure dosya paylaşımını aynı depolama hesabında, bunlar içinde arşiv paylaşımlarınız varsa havuza alabilirsiniz.

Bu konular, [buluta erişimi doğrudan](#direct-share-access-vs-azure-file-sync) (bır Azure VM veya hizmeti aracılığıyla) Azure dosya eşitleme kıyasla daha fazla geçerlidir. Yalnızca bu paylaşımlar üzerinde Azure Dosya Eşitleme kullanmayı planlıyorsanız, birkaçını tek bir Azure depolama hesabına göre gruplamak iyidir. Ayrıca, bir uygulamayı buluta taşımak ve daha sonra bir dosya paylaşımında doğrudan erişmek isteyebileceğiniz göz önünde bulundurun. Ya da Azure 'da, daha yüksek ıOPS ve üretilen iş numaralarına sahip olmanın avantajlarından faydalanabileceğiniz bir hizmeti kullanmaya başlayabilirsiniz. 

Paylaşımlarınızın bir listesini yaptıysanız, her bir paylaşımı bulunacağı depolama hesabıyla eşlemeniz gerekir.

> [!IMPORTANT]
> Azure bölgesine karar aldığınızdan emin olun ve ardından her depolama hesabı ve Azure Dosya Eşitleme kaynağı seçmiş olduğunuz bölgeyle eşleştirin.

### <a name="phase-1-summary"></a>Aşama 1 Özeti
1. aşama sonunda:
* StorSimple cihazlarınız ve birimleriniz için iyi bir genel bakış sunulmaktadır.
* Veri dönüştürme hizmeti, her StorSimple cihazı için hizmet veri şifreleme anahtarınızı aldığınız için buluttaki StorSimple birimlerine erişmeye hazır.
* Yalnızca hangi birimlerin geçirilmesi gerektiğini, ancak aynı zamanda birimlerinizi uygun sayıda Azure dosya paylaşımı ve depolama hesabı ile nasıl eşleneceğini gösteren bir planınız vardır.

> [!CAUTION]
> , StorSimple birimlerinden yedeklemeleri geçirmeniz gerekiyorsa, **burada durun**.
>
> Bu geçiş yaklaşımı, şu anda yedeklemeleri geçiremeyen yeni veri dönüştürme hizmeti özelliklerine bağımlıdır. Yedekleme geçişine yönelik destek, 2020 sonuna ulaştığında gönderilir.
> Şu anda yalnızca canlı verilerinizi geçirebilirsiniz. Şimdi başlatırsanız, yedeklemelerinizi daha sonra "başlatamazsınız".
> Yedeklemeler arasında Azure dosya paylaşımı anlık görüntüleri ile, Azure dosya paylaşımlarının en eskiden en yeniye ve canlı verilere "oynanabilir" olması gerekir.

Yalnızca canlı verileri geçirmek istiyorsanız ve yedeklemeler için herhangi bir gereksinim yoksa, bu kılavuzu izleyerek devam edebilirsiniz.

## <a name="phase-2-deploying-azure-storage-and-migration-resources"></a>2. Aşama: Azure depolama ve geçiş kaynaklarını dağıtma

Bu bölümde, Azure 'da gereken farklı kaynak türlerini dağıtmaya yönelik konular açıklanmaktadır. Bazıları, veri geçişinizi tutacaktır ve bazıları yalnızca geçiş için gereklidir. Dağıtım planınızı sonuçlandırana kadar kaynak dağıtmaya başlamaın. Dağıtıldıklarında Azure kaynaklarınızın belirli yönlerini değiştirmek zor, bazen imkansız olabilir.

### <a name="deploy-storage-accounts"></a>Depolama hesaplarını dağıtma
Muhtemelen birçok Azure depolama hesabı dağıtmanız gerekecektir. Her biri dağıtım planınıza göre daha az sayıda Azure dosya paylaşımını tutar ve bu makalenin önceki bölümünde tamamlanır. [Planlı depolama hesaplarınızı dağıtmak](../common/storage-account-create.md#create-a-storage-account)için Azure Portal alındı. Tüm yeni depolama hesapları için aşağıdaki temel ayarları göz önünde bulundurun:

##### <a name="subscription"></a>Abonelik
StorSimple dağıtımınız veya farklı bir dağıtım için kullandığınız aboneliği kullanabilirsiniz. Tek sınırlama, aboneliğinizin StorSimple aboneliğiyle aynı Azure AD kiracısında olması gerekir. Geçişe başlamadan önce StorSimple aboneliğini doğru kiracıya taşımayı düşünün. Yalnızca tüm aboneliği taşıyabilirsiniz, bireysel StorSimple kaynakları farklı bir kiracıya veya aboneliğe taşınamaz.

##### <a name="resource-group"></a>Kaynak grubu
Kaynak grupları, kaynakların ve yönetici yönetim izinlerinin organizasyonu ile ilgili yardım vardır. [Azure 'da kaynak grupları](../../azure-resource-manager/management/manage-resource-groups-portal.md#what-is-a-resource-group)hakkında daha fazla bilgi edinin.

##### <a name="storage-account-name"></a>Depolama hesabı adı
Depolama hesabınızın adı bir URL 'nin parçası olacak ve belirli karakter kısıtlamalarına sahip olacaktır. Adlandırma kuralızda, depolama hesabı adlarının dünya genelinde benzersiz olması, yalnızca küçük harf ve sayılara izin vermek, 3 ila 24 karakter arasında olmasını gerektirmek ve tireler veya alt çizgi gibi özel karakterlere izin vermeyi göz önünde bulundurun. Ayrıca bkz: [Azure depolama kaynak adlandırma kuralları](../../azure-resource-manager/management/resource-name-rules.md#microsoftstorage).

##### <a name="location"></a>Konum
Depolama hesabının "konum" veya Azure bölgesi çok önemlidir. Azure Dosya Eşitleme kullanıyorsanız, tüm depolama hesaplarınız depolama eşitleme hizmeti kaynağınız ile aynı bölgede olmalıdır. Seçtiğiniz Azure bölgesi yerel sunucularınız/kullanıcılarınıza yakın veya orta olmalıdır. Kaynağınız dağıtıldıktan sonra bölgesini değiştiremezsiniz.

StorSimple verilerinizin (depolama hesabı) Şu anda bulunduğu farklı bir bölge seçebilirsiniz.

> [!IMPORTANT]
> Geçerli, StorSimple depolama hesabı konumundan farklı bir bölge seçerseniz, geçiş sırasında [Çıkış ücretleri uygulanır](https://azure.microsoft.com/pricing/details/bandwidth) . Veri StorSimple bölgesinden bırakılır ve yeni depolama hesabı bölgenizi girer. Aynı Azure bölgesinin içinde kaldıysanız bant genişliği ücretleri uygulanmaz.

##### <a name="performance"></a>Performans
Azure dosya paylaşımları veya standart depolama için Premium Depolama (SSD) seçme seçeneğiniz vardır. Standart depolama, [bir dosya paylaşımının çeşitli katmanlarını](storage-how-to-create-file-share.md#changing-the-tier-of-an-azure-file-share)içerir. Standart depolama, çoğu müşterinin StorSimple 'dan geçiş yapma konusunda doğru seçenektir.

Hala emin değil misiniz?
* [Premium Azure dosya paylaşımının performansına](storage-files-planning.md#understanding-provisioning-for-premium-file-shares)ihtiyacınız varsa Premium Depolama ' yı seçin.
* Etkin veriler ve arşiv verileri dahil olmak üzere genel amaçlı dosya sunucusu iş yükleri için standart depolama ' yı seçin. Ayrıca, buluttaki paylaşımdaki tek iş yükünün Azure Dosya Eşitleme olması durumunda standart depolamayı da seçin.

##### <a name="account-kind"></a>Hesap türü
* Standart depolama için şunu seçin: *StorageV2 (genel amaçlı v2)*
* Premium dosya paylaşımları için şunu seçin: *FileStorage*

##### <a name="replication"></a>Çoğaltma
Kullanılabilir birkaç çoğaltma ayarı vardır. Farklı çoğaltma türleri hakkında daha fazla bilgi edinin.

Yalnızca aşağıdaki iki seçenekten birini seçin:
* *Yerel olarak yedekli depolama (LRS)*
* *Bölge yedekli depolama (ZRS)* -tüm Azure bölgelerinde kullanılamaz.

> [!NOTE]
> Yalnızca LRS ve ZRS artıklık türleri, büyük, 100TiB kapasiteli Azure dosya paylaşımlarıyla uyumludur.

Genel olarak yedekli depolama (tüm Çeşitlemeler) Şu anda desteklenmiyor. Yedeklilik türü ' ne daha sonra ve Azure 'a yönelik destek geldiğinde GRS 'ye geçiş yapabilirsiniz.

##### <a name="enable-100tib-capacity-file-shares"></a>100TiB-kapasite dosya paylaşımlarını etkinleştir

:::row:::
    :::column:::
        :::image type="content" source="media/storage-files-how-to-create-large-file-share/large-file-shares-advanced-enable.png" alt-text="Bir depolama hesabı oluşturmak için Azure portal Gelişmiş sekmesini gösteren bir görüntü.":::
    :::column-end:::
    :::column:::
        Azure portal yeni depolama hesabı sihirbazının *Gelişmiş* bölümü altında, bu depolama hesabında *büyük dosya paylaşımları* desteğini etkinleştirebilirsiniz. Bu seçenek sizin için kullanılabilir değilse, büyük olasılıkla yanlış artıklık türünü seçmiş olursunuz. Bu seçeneğin kullanılabilir olması için yalnızca LRS veya ZRS seçeneklerini seçtiğinizden emin olun.
    :::column-end:::
:::row-end:::

Büyük, 100Gb kapasiteli dosya paylaşımlarının çok sayıda avantajı vardır:
* Daha küçük, 5 TİB kapasiteli dosya paylaşımlarına kıyasla performansınız büyük ölçüde artar. (örneğin: ıOPS 'nin 10 katı)
* Geçişiniz önemli ölçüde daha hızlı tamamlanır.
* Bir dosya paylaşımının, kendisine geçirilecek tüm verileri tutmak için yeterli kapasiteye sahip olacağını doğrulayın.
* Gelecekteki büyüme ele alınmıştır.

### <a name="azure-file-shares"></a>Azure dosya paylaşımları
Depolama hesaplarınız oluşturulduktan sonra, depolama hesabının *"dosya paylaşımı"* bölümüne gidebilir ve Aşama 1 ' den Geçiş planınız uyarınca uygun sayıda Azure dosya paylaşımı dağıtabilirsiniz. Azure 'daki yeni dosya paylaşımlarınız için aşağıdaki temel ayarları göz önünde bulundurun:

:::row:::
    :::column:::
        :::image type="content" source="media/storage-files-migration-storsimple-8000/storage-files-migration-storsimple-8000-new-share.png" alt-text="Bir depolama hesabı oluşturmak için Azure portal Gelişmiş sekmesini gösteren bir görüntü.":::
    :::column-end:::
    :::column:::
        </br>**Ad**</br>Küçük harf, sayı ve kısa çizgi desteklenir.</br></br>**Kota**</br>Buradaki kota, bir Windows Server 'daki SMB sabit kotasına benzer. En iyi uygulama, geçiş işlemi sırasında bir kota ayarlanmamalıdır ve kotaya ulaşıldığında diğer hizmetler başarısız olur.</br></br>**Katman**</br>Yeni dosya paylaşımınız için *Iyileştirilmiş işlem* seçin. Geçiş sırasında birçok işlem gerçekleşir ve katmanınızı daha sonra iş yükünüze en uygun katmana değiştirmek daha verimli olacaktır.
    :::column-end:::
:::row-end:::

### <a name="storsimple-data-manager"></a>StorSimple Data Manager
Geçiş işlerinizi tutacak Azure kaynağına *"StorSimple veri Yöneticisi"* adı verilir. Yeni kaynak ' a tıklayın ve arama yapın, ardından *Oluştur*' a tıklayın.

Bu, düzenleme için kullanılan geçici bir kaynaktır, geçiş işlemi tamamlandıktan sonra onu hazırlayacaksınız. StorSimple depolama hesabınızla aynı abonelikte, kaynak grubunda ve bölgede dağıtılmalıdır.

### <a name="azure-file-sync"></a>Azure Dosya Eşitleme
Azure Dosya Eşitleme (AFS) en sık erişilen dosyaları şirket içi önbelleğe alma özelliği eklemenize olanak tanır. StorSimple 'ın önbelleğe alma yeteneklerine benzer şekilde, AFS bulut katmanlama özelliği, Windows Server ve çok siteli eşitleme için kullanılabilir önbellek kapasitesi üzerinde geliştirilmiş denetimle birlikte yerel erişim gecikmesi sağlar. Hedefiniz şirket içi bir önbellekleriniz varsa, yerel ağınızda, yeterli "doğrudan bağlı depolama (DAS)" kapasitesine sahip bir Windows Server VM 'si hazırlayın (fiziksel sunucular ve yük devretme kümeleri de desteklenir). 

> [!IMPORTANT]
> Henüz Azure Dosya Eşitleme ayarlama, paylaşımınızın geçişi tamamlandıktan sonra Azure Dosya Eşitleme ayarlanması en iyisidir. AFS dağıtımı, bir geçişin 4. aşamasından önce başlamamalıdır.

### <a name="phase-2-summary"></a>2. aşama Özeti
2. aşama sonunda, depolama hesaplarınız, tüm Azure dosya paylaşımlarının tamamında ve ayrıca bir StorSimple Veri Yöneticisi kaynağı olacak şekilde dağıtılır. Geçiş işlerinizi gerçekten yapılandırdığınızda ikinci aşama 3 ' ü kullanacaksınız.

## <a name="phase-3-create-and-run-a-migration-job"></a>3. Aşama: geçiş işi oluşturma ve çalıştırma
Bu bölümde, bir geçiş işinin nasıl ayarlanacağı ve seçtiğiniz hedef Azure dosya paylaşımında kopyalanması gereken bir StorSimple birimindeki dizinlerin dikkatle nasıl eşlenileceği açıklanmaktadır. Başlamak için StorSimple Veri Yöneticisi gidin, menüdeki **iş tanımlarını** bulun ve **+ iş tanımı**' nı seçin. Hedef depolama türü varsayılan: *Azure dosya paylaşımıdır*.

![StorSimple 8000 serisi geçiş işi türleri.](media/storage-files-migration-storsimple-8000/storage-files-migration-storsimple-8000-new-job-type.png "İş tanımlarının bir ekran görüntüsü, yeni Iş tanımı iletişim kutusu açık olan Azure portal iş türünü sorar. bir dosya paylaşımında veya bir blob kapsayıcısına kopyalama.")

> [!IMPORTANT]
> Herhangi bir geçiş işini çalıştırmadan önce, StorSimple birimlerinizin otomatik olarak zamanlanmış yedeklemelerini durdurun.

:::row:::
    :::column:::
        ![StorSimple 8000 serisi geçiş işi.](media/storage-files-migration-storsimple-8000/storage-files-migration-storsimple-8000-new-job.png "Veri dönüştürme Işi için yeni iş oluşturma formunun ekran görüntüsü.")
    :::column-end:::
    :::column:::
        **İş tanımı adı**</br>Bu ad, taşıdığınız dosya kümesinin göstergesi olmalıdır. Azure dosya paylaşımınıza benzer bir ad vermek iyi bir uygulamadır. </br></br>**İşin çalıştığı konum**</br>Bir bölge seçerken, StorSimple depolama hesabınızla aynı bölgeyi seçmeniz gerekir, aksi takdirde, bu durumda bir bölge de buraya yakın olur. </br></br><h3>Kaynak</h3>**Kaynak aboneliği**</br>StorSimple Aygıt Yöneticisi kaynağınızı depoladığınız aboneliği seçin. </br></br>**StorSimple kaynağı**</br>Gerecinizin ile kayıtlı olduğu StorSimple Aygıt Yöneticisi seçin. </br></br>**Hizmeti Verileri Şifreleme Anahtarı**</br>Kayıtlarınızda anahtarı bulamıyorsanız bu [makalenin önceki bölümüne](#storsimple-service-data-encryption-key)bakın. </br></br>**Cihaz**</br>Geçirmek istediğiniz birimi tutan StorSimple cihazınızı seçin. </br></br>**Birim**</br>Kaynak birimi seçin. Daha sonra, tüm birim veya alt dizinleri hedef Azure dosya paylaşımında geçirmek istediğinize karar verirsiniz. </br></br><h3>Hedef</h3>Bu geçiş işinin hedefi olarak abonelik, depolama hesabı ve Azure dosya paylaşımından birini seçin.
    :::column-end:::
:::row-end:::

> [!IMPORTANT]
> Geçişi gerçekleştirmek için en son birim yedeklemesi kullanılacaktır. En az bir birim yedeğinin bulunduğundan emin olun, aksi halde iş başarısız olur. Ayrıca, en son yedeklemenin son derece güncel olduğundan emin olun. Yeni oluşturduğunuz işi çalıştırmadan **önce** el ile başka bir birim yedeklemesi tetiklenmesi ve tamamlanması yararlı olabilir.

### <a name="directory-mapping"></a>Dizin eşleme
Bu, geçiş işiniz için isteğe bağlıdır. Boş bırakırsanız, StorSimple biriminiz kökündeki **Tüm** dosyalar ve klasörler hedef Azure dosya paylaşımınızın köküne taşınır. Çoğu durumda, bir birimin içeriğinin tamamını bir Azure dosya paylaşımında depolamak en iyi yaklaşım değildir. Bir birimin içeriğini Azure 'da birden çok dosya paylaşımı arasında bölmeniz genellikle daha iyidir. Henüz bir plan yapmadıysanız, önce bu bölüme göz atın: [StorSimple biriminizi Azure dosya paylaşımlarına eşleme](#map-your-existing-storsimple-volumes-to-azure-file-shares)

Geçiş planınızın bir parçası olarak, StorSimple birimindeki klasörlerin birden çok Azure dosya paylaşımına bölünmesi gerektiğini kararmış olabilirsiniz. Böyle bir durum söz konusu olduğunda, bu bölmeyi şu şekilde yapabilirsiniz:
1. Klasörlerin bir birimde geçirilmesi için birden çok iş tanımlama, her birinin aynı StorSimple birimi kaynağına, ancak hedef olarak farklı bir Azure dosya paylaşımıyla aynı olacaktır.
1. StorSimple biriminden hangi klasörlerin belirtilen dosya paylaşımında geçirilmesi gerektiğini tam olarak belirtme, iş oluşturma formunun *Dizin eşleme* bölümünü kullanarak ve belirli [eşleme semantiğini](#semantic-elements)takip etmeniz gerekir.

> [!IMPORTANT]
> Form gönderildiğinde, bu formdaki yollar ve eşleme ifadeleri doğrulanamaz. Eşlemeler yanlış belirtilirse, bir iş tamamen başarısız olabilir ya da istenmeyen bir sonuç üretebilir. Bu durumda, genellikle en iyisi Azure dosya paylaşımının silinmesi, yeniden oluşturulması ve ardından paylaşıma yönelik yeni bir geçiş işinde eşleme deyimlerini düzeltmesine yöneliktir. Sabit bir eşleme deyimleriyle yeni bir işi çalıştırmak, atlanan klasörleri çözebilir ve var olan paylaşıma getirebilir. Ancak, yalnızca yol yazım hataları nedeniyle atlanan klasörler bu şekilde çözülebilir.

#### <a name="semantic-elements"></a>Anlam öğeleri
Bir eşleme soldan sağa ifade edilir: [\Source Path] \> [\target Path].

|Anlam karakteri          | Anlamı  |
|:---------------------------|:---------|
| **\\**                     | kök düzeyi göstergesi        |
| **\>**                     | [kaynak] ve [hedef] eşleme işleci        |
|**\|** veya RETURN (yeni satır) | iki klasör eşleme yönergelerinin ayırıcısı. </br>Alternatif olarak, bir sonraki eşleme ifadesini kendi satırına almak için bu karakteri atlayabilir ve ENTER tuşuna basabilirsiniz.        |

### <a name="examples"></a>Örnekler
"Kullanıcı verileri" klasörünün içeriğini hedef dosya paylaşımının köküne taşımalıdır:
``` console
\User data > \\
```
Tüm birim içeriğini hedef dosya paylaşımında yeni bir yola taşımalıdır:
``` console
\ \> \Apps\HR tracker
```
Kaynak klasör içeriğini hedef dosya paylaşımında yeni bir yola taşımalıdır:
``` console
\HR resumes-Backup \> \Backups\HR\resumes
```
Birden çok kaynak konumu yeni bir dizin yapısına sıralar:
``` console
\HR\Candidate Tracker\v1.0 > \Apps\Candidate tracker
\HR\Candidates\Resumes > \HR\Candidates\New
\Archive\HR\Old Resumes > \HR\Candidates\Archived
```

### <a name="semantic-rules"></a>Anlam kuralları
* Her zaman kök düzeyine göre klasör yolları belirtin. 
* Her klasör yolunu bir kök düzeyi göstergesi ile başlatın " \" . 
* Sürücü harflerini eklemeyin. 
* Birden çok yol belirtirken, kaynak veya hedef yollar çakışamaz:</br>
   Geçersiz kaynak yolu örtüşme örneği:</br>
    *\\folder\1 > \\ klasörü*</br>
    *\\klasör \\ 1 \\ 2 > \\ klasör2*</br>
   Geçersiz hedef yolu örtüşme örneği:</br>
   *\\klasör > \\*</br>
   *\\klasör2 > \\*</br>
* Varolmayan kaynak klasörleri yok sayılır. 
* Hedefte mevcut olmayan klasör yapıları oluşturulacaktır. 
* Pencereler gibi: klasör adları büyük/küçük harfe duyarlıdır ancak durum korunur.

> [!NOTE]
> StorSimple biriminizdeki "*\System Volume Information*" klasörünün ve "*$Recycle. bin*" içeriği, dönüştürme işi tarafından kopyalanmayacak.

### <a name="phase-3-summary"></a>3. aşama Özeti
3. aşama sonunda, StorSimple birimlerinden Azure dosya paylaşımlarına veri dönüştürme işlerinizi çalıştırmış olursunuz. Artık, paylaşıma yönelik Azure Dosya Eşitleme (bir paylaşımın geçiş işleri tamamlandıktan sonra) veya bilgi çalışanlarınız ve uygulamalarınızın Azure dosya paylaşımında doğrudan paylaşma erişimini ayarlamaya odaklanırsınız.

## <a name="phase-4-accessing-your-azure-file-shares"></a>4. Aşama: Azure dosya paylaşımlarınızı erişme

Azure dosya paylaşımlarınızın erişimine yönelik iki ana strateji vardır:

* **Azure dosya eşitleme:** [Azure dosya eşitleme](#deploy-azure-file-sync) şirket içi Windows Server 'a dağıtma. AFS, tıpkı StorSimple gibi yerel bir önbelleğin tüm avantajlarını içerir.
* **Doğrudan paylaşma erişimi:** [doğrudan paylaşma-erişim dağıtımı](#deploy-direct-share-access). Belirli bir Azure dosya paylaşımının erişim senaryonuz yerel önbellekten yararlanmuyorsa veya artık şirket içi bir Windows Server 'ı barındırmanıza izin vermemesi durumunda bu stratejiyi kullanın. Burada, kullanıcılarınız ve uygulamalarınız SMB protokolü üzerinden SMB paylaşımlarına erişmeye devam edecektir, ancak bu paylaşımlar artık şirket içi bir sunucuda değil, ancak doğrudan bulutta yer alır.

Bu kılavuzun [1. aşamasında](#phase-1-prepare-for-migration) , sizin için en uygun seçeneği zaten kararmış olmanız gerekir.

Bu bölümün geri kalanı dağıtım yönergelerine odaklanır.

### <a name="deploy-azure-file-sync"></a>Azure Dosya Eşitleme’yi dağıtma

Azure Dosya Eşitleme bir parçasını dağıtmak zaman alabilir:
1. Azure Dosya Eşitleme bulut kaynağı oluşturun.
1. Azure Dosya Eşitleme aracısını şirket içi sunucunuza dağıtın.
1. Sunucuyu bulut kaynağına kaydetme

Henüz hiçbir eşitleme grubu oluşturmayın. Bir Azure dosya paylaşımıyla eşitlemenin kurulması, yalnızca bir Azure dosya paylaşımında geçiş işleriniz tamamlandıktan sonra gerçekleşmelidir. Geçişiniz tamamlanmadan önce Dosya Eşitleme kullanmaya başlayadıysanız geçişinizin, kesilen bir süre sonra kolayca söylenemez.

#### <a name="deploy-the-azure-file-sync-cloud-resource"></a>Azure Dosya Eşitleme bulut kaynağını dağıtma

[!INCLUDE [storage-files-migration-deploy-afs-sss](../../../includes/storage-files-migration-deploy-azure-file-sync-storage-sync-service.md)]

> [!TIP]
> Geçiş tamamlandığında verilerinizin bulunduğu Azure bölgesinde değişiklik yapmak istiyorsanız, bu geçiş için hedef depolama hesapları ile aynı bölgedeki depolama eşitleme hizmeti 'ni dağıtın.

#### <a name="deploy-an-on-premises-windows-server"></a>Şirket içi Windows Server dağıtma

* Bir sanal makine veya fiziksel sunucu olarak en az 2012R2 ' de bir Windows Server 2019 oluşturun. Windows Server yük devretme kümesi de desteklenir. StorSimple 8100 veya 8600 ' den muaf olan sunucuyu yeniden kullanmayın.
* Doğrudan bağlı depolama alanı sağlayın veya ekleyin (desteklenmeyen bir şekilde, NAS ile karşılaştırıldığında).

Yeni Windows Server 'a, StorSimple 8100 veya 8600 gerecinin önbelleğe alma için yerel olarak kullanılabilir olandan eşit veya daha büyük bir depolama miktarı vermesi en iyi uygulamadır. Windows Server 'ı StorSimple gerecini kullandığınız şekilde kullanırsınız. Bu, Gereç ile aynı miktarda depolama alanına sahipse, aynı değilse önbelleğe alma deneyiminin benzer olması gerekir.
Windows Server 'dan ' de depolama ekleyebilir veya depolama alanını kaldırabilirsiniz. Bu, yerel birim boyutunuzu ve önbelleğe alma için kullanılabilir yerel depolama miktarını ölçeklendirmenizi sağlar.

#### <a name="prepare-the-windows-server-for-file-sync"></a>Windows Server 'ı dosya eşitleme için hazırlama

[!INCLUDE [storage-files-migration-deploy-afs-agent](../../../includes/storage-files-migration-deploy-azure-file-sync-agent.md)]

#### <a name="configure-azure-file-sync-on-the-windows-server"></a>Windows Server 'da Azure Dosya Eşitleme yapılandırma

Kayıtlı şirket içi Windows Server, bu işlem için önceden ve internet 'e bağlı olmalıdır.

> [!IMPORTANT]
> Devam etmeden önce, dosya ve klasörlerin Azure dosya paylaşımında StorSimple geçişinin tamamlanmış olması gerekir. Dosya paylaşımında başka değişiklik yapıldığından emin olun.

[!INCLUDE [storage-files-migration-configure-sync](../../../includes/storage-files-migration-configure-sync.md)]

> [!IMPORTANT]
> Bulut katmanlaması 'nı etkinleştirdiğinizden emin olun! Bulut katmanlama, yerel sunucunun bulutta depolankıyasla daha az depolama kapasitesine sahip olmasına izin veren Azure Dosya Eşitleme özelliğidir, ancak tam ad alanı kullanılabilir. Yerel olarak ilginç veriler de hızlı, yerel erişim performansı için yerel olarak önbelleğe alınır. Bu adımda bulut katmanlamayı açmak için başka bir nedenden dolayı, bu aşamada dosya içeriğini eşitlemek istemiyorum, yalnızca ad alanı şu anda taşınıyor.

### <a name="deploy-direct-share-access"></a>Doğrudan paylaşma-erişim dağıtma

:::row:::
    :::column:::
        [![Azure dosya paylaşımlarını doğrudan bilgi çalışanlarına ve uygulamalarına güvenli bir şekilde kullanıma sunma hakkında adım adım kılavuz ve tanıtım-oynatmak için tıklayın!](./media/storage-files-migration-storsimple-8000/azure-files-direct-access-video-placeholder.png)](https://youtu.be/KG0OX0RgytI)
    :::column-end:::
    :::column:::
        Bu video, Azure dosya paylaşımlarını doğrudan beş, basit adımlarda bilgi çalışanları ve uygulamalarına güvenle kullanıma sunma kılavuzudur.</br>
        Video, bazı konular için adanmış belgelere başvurur:
* [Kimliğe genel bakış](storage-files-active-directory-overview.md)
* [Etki alanına bir depolama hesabına ekleme](storage-files-identity-auth-active-directory-enable.md)
* [Azure dosya paylaşımları için ağa genel bakış](storage-files-networking-overview.md)
* [Ortak ve özel uç noktaları yapılandırma](storage-files-networking-endpoints.md)
* [S2S VPN yapılandırma](storage-files-configure-s2s-vpn.md)
* [Windows P2S VPN 'yi yapılandırma](storage-files-configure-p2s-vpn-windows.md)
* [Linux P2S VPN yapılandırma](storage-files-configure-p2s-vpn-linux.md)
* [DNS iletmeyi yapılandırma](storage-files-networking-dns.md)
* [DFS 'yi yapılandırma-N](https://aka.ms/AzureFiles/Namespaces)
   :::column-end:::
:::row-end:::

### <a name="phase-4-summary"></a>4. aşama Özeti
Bu aşamada *StorSimple veri Yöneticisi*birden çok *veri dönüştürme hizmeti* (DTS) işi oluşturdunuz ve çalıştırdık. Bu işler, dosyalarınızı ve klasörlerinizi Azure dosya paylaşımlarına geçirdiniz. Ayrıca, Azure Dosya Eşitleme veya ağ ve depolama hesaplarınızı doğrudan Share-Access için hazırladınız.

## <a name="phase-5-user-cut-over"></a>5. Aşama: Kullanıcı tarafından kesilen
Bu aşama, geçişinizi sarmalandırmak için gereklidir:
* Kapalı kalma süresini planlayın.
* StorSimple tarafında üretilen kullanıcılarınızın ve uygulamalarınızın yaptığı değişikliklerle, 1. aşama 'da veri dönüştürme işleri çalışırken her türlü değişikliği yakalayın. 
* Doğrudan paylaşım erişimi aracılığıyla Azure Dosya Eşitleme veya Azure dosya paylaşımları ile kullanıcılarınızı yeni Windows Server 'a devretmek.

### <a name="plan-your-downtime"></a>Kapalı kalma süresini planlayın
Bu geçiş yaklaşımı, kullanıcılarınız ve uygulamalarınız için bazı kapalı kalma süresi gerektirir. Amaç, kapalı kalma süresini en düşük tutmaya ve aşağıdaki noktalara yardımcı olabilir:

* Veri dönüştürme işlerinizi çalıştırırken StorSimple birimlerinizi kullanılabilir tutun.
* Bir paylaşım için veri geçiş işinizi çalıştırmayı tamamladığınızda, StorSimple birimlerinden/paylaşımlarında Kullanıcı erişimini (en azından yazma erişimi) kaldırmanız zaman olur. Son bir RoboCopy Azure dosya paylaşımınızı yakalar ve ardından kullanıcılarınızı kesebilir. RoboCopy çalıştırdığınız yer, Azure Dosya Eşitleme veya doğrudan paylaşma erişimi seçtiğinizden bağımsız olarak değişir. RoboCopy üzerindeki yakında çıkacak bölüm, bunu ele almaktadır.
* RoboCopy yakala 'yı tamamladıktan sonra, yeni konumu kullanıcılarınıza sunmaya hazırsınızdır. Azure dosyası, AFS ile bir Windows Server üzerinde doğrudan veya bir SMB paylaşımıdır. Genellikle bir DFS-N dağıtımı, hızla ve verimli bir şekilde kesme gerçekleştirmeye yardımcı olur. Bu, mevcut paylaşımlarınızın adreslerini tutarlı ve yeni bir konuma, geçirilen dosya ve klasörlerinizi içerecek şekilde yeniden işaret edecek şekilde tutacaktır.

### <a name="determine-when-your-namespace-has-fully-synced-to-your-server"></a>Ad alanınız, sunucunuza tam olarak eşitlendiğinde belirlenir

Bir Azure dosya paylaşımında Azure Dosya Eşitleme kullandığınızda, herhangi bir yerel RoboCopy başlamadan önce tüm ad alanınızı sunucuya indirmeyi tamamladığından emin olmanız önemlidir. Ad alanınızı indirmek için gereken süre, Azure dosya paylaşımınızda bulunan öğelerin sayısına bağlıdır. Ad uzayının sunucuda tam olarak ulaşıp ulaşmadığını belirlemek için iki yöntem vardır:

#### <a name="azure-portal"></a>Azure portal
Ad alanınız ne zaman tam olarak geldiğini görmek için Azure portal kullanabilirsiniz.
* Azure portal oturum açın, eşitleme grubunuza gidin ve eşitleme grubunuzun ve sunucu uç noktanızın eşitleme durumunu kontrol edin. 
* İlginç bir yön indirilir: sunucu uç noktası yeni sağlandıysa, ad alanının hala kaldığını belirten **ilk eşitleme** görüntülenir.
Her şeye, ancak *ilk eşitlemeye*göre değişiklik yaptıktan sonra, ad alanınız sunucuda tamamen doldurulur ve yerel bir Robocopy ile devam etmeniz iyi olur.

#### <a name="windows-server-event-viewer"></a>Windows Server Olay Görüntüleyicisi
OU, ad alanı tam olarak ulaştığı zaman söylemek için Windows sunucunuzdaki Olay Görüntüleyicisi da kullanabilir.

1. **Olay Görüntüleyicisi** açın ve **uygulamalar ve hizmetler**' e gidin.
1. **Microsoft\filesync\k\t\telemetri**sayfasına gidin ve açın.
1. Tamamlanan bir eşitleme oturumuna karşılık gelen en son **olay 9102**' i arayın.
1. **Ayrıntılar** ' ı seçin ve **Syncdirection** değerinin **indirileceği**bir olaya baktığınızdan emin olun.
1. Ad uzayının sunucuya indirmesi tamamlandığında, **senaryo**, değer **FullGhostedSync** ve **HRESULT**  =  **0**olan tek bir olay olacaktır.
1. Bu olayı kaçırırsanız, **syncdirection** **9102 events**  =  **indirmesi** ve **Scenario**  =  **"RegularSync"** senaryosuyla diğer 9102 olaylarına de bakabilirsiniz. Bu olaylardan birini bulmak için aynı zamanda, ad alanının, eşitleme için bir şey olup olmadığı, diğer bir deyişle eşitleme ve eşitleme gibi normal eşitleme oturumlarına ilerlemedi ve eşitleme tamamlandığını gösterir.

### <a name="a-final-robocopy"></a>Son RoboCopy
Bu noktada, şirket içi Windows Server ve StorSimple 8100 ya da 8600 gereci arasında farklılıklar vardır:

1. Geçiş devam ederken, kullanıcıların/uygulamaların StorSimple tarafında üretilen değişikliklerle ilgili değişiklikler yapmanız gerekir.
1. Azure Dosya Eşitleme kullandığınız durumlar için: StorSimple gerecinin doldurulmuş bir önbelleği ve Windows Server 'ın Şu anda yerel olarak depolanan dosya içeriği olmayan bir ad alanı vardır. Bu nedenle, son RoboCopy, yerel olarak önbelleğe alınmış dosya içeriğini kullanıma sunarak yerel AFS önbelleğinizi hızlı bir şekilde başlatabilir ve bu sayede AFS sunucusuna uyabilirler.
1. Bazı dosyalar, geçersiz karakterler nedeniyle veri dönüştürme işinin arkasında bırakılmış olabilir. Varsa, bunları Azure Dosya Eşitleme etkin Windows Server 'a kopyalayın. Daha sonra bunları eşitlebilmeleri için ayarlayabilirsiniz. Belirli bir paylaşıma yönelik Azure Dosya Eşitleme kullanmıyorsanız, StorSimple biriminde geçersiz karakterlerle dosyaları yeniden adlandırmayı daha iyi ve ardından RoboCopy doğrudan Azure dosya paylaşımında çalıştırmalısınız. 

> [!WARNING]
> Sunucuda, tam olarak indirilen bir Azure dosya paylaşımının ad alanına sahip olacak şekilde RoboCopy başlatmamalıdır!
> Kullanıma al: "[ad alanınız sunucunuza tam olarak Indirildiğini belirleme](#determine-when-your-namespace-has-fully-synced-to-your-server)"

 Yalnızca geçiş işinin en son çalıştırıldıktan sonra değiştirilen dosyaları ve önce gelen işler aracılığıyla taşınmayan dosyaları kopyalamak istersiniz. Geçiş tamamlandıktan sonra, sunucuda daha sonra ilerlemeyen sorunları çözebilirsiniz. Bkz. [Azure dosya eşitleme sorun giderme](storage-sync-files-troubleshoot.md#how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing).

RoboCopy 'nin çeşitli parametreleri vardır. Aşağıdaki Vitrini, tamamlanmış bir komut ve bu parametreleri seçme nedenlerinin bir listesi:

```console
Robocopy /MT:16 /UNILOG:<file name> /TEE /B /MIR /COPYALL /DCOPY:DAT <SourcePath> <Dest.Path>
```

Arka plan

:::row:::
   :::column span="1":::
      /MT
   :::column-end:::
   :::column span="1":::
      RoboCopy 'nin çoklu iş parçacıklı çalıştırmasına izin verir. Varsayılan 8 ' dir, Max 128 ' dir.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /UNILOG:<file name>
   :::column-end:::
   :::column span="1":::
      GÜNLÜK dosyasının durumunu UNICODE olarak çıkış (varolan günlüğün üzerine yaz).
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /T
   :::column-end:::
   :::column span="1":::
      Konsol penceresine çıkış. Günlük dosyasına çıktılarla birlikte kullanılır.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /B
   :::column-end:::
   :::column span="1":::
      RoboCopy, bir yedekleme uygulamasının kullanacağı modda çalıştırır. RoboCopy 'nin geçerli kullanıcının izinleri olmayan dosyaları taşımasına izin verir.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /MıR
   :::column-end:::
   :::column span="1":::
      Robocopy 'nin yalnızca kaynak (StorSimple gereci) ve hedef (Windows Server dizini) arasında değişimleri 'yi kabul etmesine izin verir.
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /COPY: copyflag [s]
   :::column-end:::
   :::column span="1":::
      dosya kopyasının doğruluğu (varsayılan:/COPY: DAT), kopya bayrakları: D = veri, A = öznitelikler, T = zaman damgaları, S = güvenlik = NTFS ACL 'Leri, O = sahip bilgileri, U = denetim bilgileri
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /COPYALL
   :::column-end:::
   :::column span="1":::
      Tüm dosya bilgilerini kopyala (/COPY: DATSOU ile eşdeğer)
   :::column-end:::
:::row-end:::
:::row:::
   :::column span="1":::
      /DCOPY: copyflag [s]
   :::column-end:::
   :::column span="1":::
      dizinlerin kopyasına uygunluk (varsayılan:/DCOPY: DA), kopyalama bayrakları: D = veri, A = öznitelikler, T = zaman damgaları
   :::column-end:::
:::row-end:::

RoboCopy komutunun kaynak ve hedef konumlarını yapılandırırken, bunların eşleştiğinden emin olmak için kaynak ve hedefin yapısını gözden geçirdiğinizden emin olun. Geçiş işinin Dizin eşleme özelliğini kullandıysanız, kök dizin yapınız StorSimple biriminizin yapısıyla farklı olabilir. Bu durumda, her bir alt dizin için bir tane olmak üzere birden çok RoboCopy işine ihtiyacınız olabilir. Bu RoboCopy komutu/MıR kullandığından, aynı (örneğin katmanlı dosyalar) olan dosyaları taşımayacak, ancak kaynak ve hedef yolu yanlış alırsanız,/MıR, Windows Server/Azure dosya paylaşımınızda, StorSimple kaynak yolunda bulunmayan dizin yapılarını da temizler. Bu nedenle, yalnızca geçiş devam ederken yapılan en son değişikliklerle geçirilmiş içeriğinizi güncelleştirmek için amaçlanan hedefe ulaşmak üzere RoboCopy işi için tam olarak eşleşmesi gerekir. 

Dosyaların gerisinde bırakılmış olup olmadığını görmek için RoboCopy günlük dosyasına başvurun. Sorun varsa, bunları giderin ve RoboCopy komutunu yeniden çalıştırın. İlgilendiğiniz dosyalar veya klasörler için bekleyen sorunları giderebilmeniz için önce herhangi bir StorSimple kaynağını vermeyin.

Söz konusu Azure dosya paylaşımının önbelleğe almak için Azure Dosya Eşitleme kullanmıyorsanız, ancak doğrudan Share-Access ' i tercih ettiyseniz:
1. [Azure dosya paylaşımınızı](storage-how-to-use-files-windows.md#mount-the-azure-file-share) yerel bir Windows makinesine bir ağ sürücüsü olarak bağlayın.
1. StorSimple ile bağlı Azure dosya paylaşımınız arasındaki RoboCopy gerçekleştirin. Dosyalar kopyalamadıysanız, geçersiz karakterleri kaldırmak ve RoboCopy yeniden denemek için StorSimple tarafında adlarını düzelmeyin. Daha önce listelenen RoboCopy komutu, StorSimple 'a gereksiz geri çekmelere neden olan g/ç birden çok kez çalıştırılabilir.

### <a name="user-cut-over"></a>Kullanıcı tarafından kesilen

Azure Dosya Eşitleme kullanıyorsanız, bu AFS 'nin etkinleştirildiği Windows Server üzerinde, StorSimple birimlerinde sahip olduğunuz paylaşımlarla eşleşen SMB paylaşımlarını oluşturmanız gerekir. Bu adımı önyükleyebilir ve daha erken yapabilirsiniz. bu aşamada, hiçbir zaman Windows Server 'da değişikliklere neden olan hiçbir kimsenin erişiminin olmadığından emin olmanız gerekir.

Bir DFS-N dağıtımınız varsa, DFN-Namespaces yeni sunucu klasörü konumlarına işaret edebilirsiniz. Bir DFS-N dağıtımınız yoksa ve 8100/8600 gerecinizi bir Windows Server ile yerel olarak yaptıysanız, bu sunucuyu etki alanından alabilirsiniz. Ardından, yeni, Azure Dosya Eşitleme etkin, Windows Server 'a etki alanı ekleyin. Bu işlem sırasında, sunucuya eski sunucuyla aynı sunucu adını ve paylaşma adlarını verin; böylece, kullanıcılar, Grup ilkeniz ve betikleriniz için, kesilmeleri saydam kalır.

[DFS hakkında daha fazla bilgi edinin-N](https://aka.ms/AzureFiles/Namespaces)

## <a name="deprovision"></a>Sağlamayı kaldırma
Bir kaynak sağladığınızda, bu kaynağın yapılandırmasına ve verilerine erişiminizi kaybedersiniz. Sağlamayı kaldırma işlemi geri alınamaz, bu nedenle geçişinizin tamamlandığını ve sağlamayı kaldırmak üzere olduğunuz StorSimple dosyalarında, klasörlerinde veya birim yedeklemelerinden herhangi bir bağımlılık olmadığını onaylaana kadar devam edin.

Başlamadan önce, bir bit için yeni Azure Dosya Eşitleme dağıtımınızı üretimde gözlemleyecek en iyi uygulamadır. Bu, karşılaşabileceğiniz sorunları gidermeye yönelik seçenekler sağlar.
Azure Dosya Eşitleme dağıtımınızı en az birkaç güne kadar gözlemledikten sonra kaynakları sağlamayı şu sırada yapmaya başlayabilirsiniz:

1. StorSimple Veri Yöneticisi kaynağınızın Azure portal aracılığıyla sağlamasını kaldırma.
   Tüm DTS işleriniz onunla birlikte silinecektir. Kopyalama günlüklerini kolayca alamazsınız. Kayıtlarınız için önemliyse, sağlamadan önce bunları alın.
1. StorSimple fiziksel gereçlerinizin geçirildiğinden emin olun ve ardından bunların kaydını kaldırın.
   Geçirildiğinden tamamen emin değilseniz, devam edin. Bu kaynakları hala gerekliyse, verileri veya yapılandırmalarını kurtarmanız mümkün olmayacaktır.
1. StorSimple Aygıt Yöneticisi daha fazla kayıtlı cihaz yoksa, bu Aygıt Yöneticisi kaynağının kendisini kaldırmaya devam edebilirsiniz.
1. Artık Azure 'da StorSimple depolama hesabını silmenin zamanı. Yine de, devam etmeden önce geçişinizin tamamlandığını ve hiçbir şeyin bu verilere bağlı olmadığını durdurun ve onaylayın.
1. StorSimple fiziksel gerecini veri merkezinizden çıkarın.
1. StorSimple gerecine sahipseniz, bılgısayar geri dönüşüm kutusunu ücretsiz olarak kullanabilirsiniz. 
   Cihazınız kiralandıysanız, lessa bildirin ve cihazı uygun şekilde geri döndürün.

Geçişiniz tamamlanmıştır.

> [!NOTE]
> Hala sorularınız mı yoksa herhangi bir sorunla mi karşılaşıyoruz?</br>
> Yardım almak için buraya ihtiyacımız vardır: AzureFilesMigration@microsoft . com


## <a name="next-steps"></a>Sonraki adımlar

* Azure Dosya Eşitleme hakkında daha fazla bilgi edinin [: aka.MS/AFS](https://aka.ms/AFS).
* [Bulut katmanlama](storage-sync-cloud-tiering.md) ilkelerinin esnekliğini anlayın.
* Anlık görüntüleri zamanlamak ve yedekleme bekletme zamanlamalarını tanımlamak için Azure dosya paylaşımlarınız üzerinde [Azure Backup etkinleştirin](../../backup/backup-afs.md#configure-backup-from-the-file-share-pane) .
* Bazı dosyaların kalıcı olarak eşitlenmediğinden Azure portal görürseniz, bu sorunları gidermeye yönelik adımlar için [sorun giderme kılavuzunu](storage-sync-files-troubleshoot.md) gözden geçirin.
