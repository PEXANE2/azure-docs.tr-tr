---
title: Azure sanal makine teklifi oluşturun - Azure Marketi
description: Ticari pazarda sanal bir makine teklifi oluşturmayı öğrenin.
author: emuench
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/10/2020
ms.openlocfilehash: 161fd9276427db0d0d7d56da1bfc0bb8ccf52a52
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81536667"
---
# <a name="create-an-azure-virtual-machine-offer"></a>Azure sanal makine teklifi oluşturma

> [!IMPORTANT]
> Azure VM tekliflerinizin yönetimini Bulut İş Ortağı Portalı'ndan İş Ortağı Merkezi'ne taşıyoruz. Tekliflerin izne gelene kadar, lütfen tekliflerinizi yönetmek için Cloud Partner Portal'daki [Sanal Makine Oluştur teklifindeki](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-create-offer) talimatları izlemeye devam edin.

Bu makalede, Azure Marketi'nde bir [Azure Marketplace](https://azuremarketplace.microsoft.com/)Azure sanal makine teklifinin nasıl oluşturulup yayımlandırılabildiğini açıklanmaktadır. Hem windows ve Linux tabanlı sanal makinelere hitap eder, işletim sistemi, sanal sabit disk (VHD) ve en fazla 16 veri diski içerir.

## <a name="introduction"></a>Giriş

### <a name="publishing-benefits"></a>Yararlanma avantajları

Azure Marketi'nde yayımlamanın aşağıdaki avantajları vardır:

- Microsoft markasını kullanarak şirketinizi tanıtın
- Azure Marketi aracılığıyla 100 milyondan fazla Office 365 ve Dynamics 365 kullanıcısına ve 200.000'den fazla kuruluşa ulaşın
- Bu pazarlardan yüksek kaliteli müşteri adayları alın
- Hizmetlerinizi Microsoft alanı ve tele-satış ekipleri tarafından tanıtın

### <a name="before-you-begin"></a>Başlamadan önce

Henüz yapmadıysanız, [Sanal makine teklif yayımlama kılavuzunu](https://aka.ms/Virtualmachineofferpublishingguide) ve bu Azure sanal makine malzemesini inceleyin:

- Quickstart kılavuzları
  - [Azure Quickstart şablonları](https://azure.microsoft.com/resources/templates/)
  - [GitHub Azure Quickstart şablonları](https://github.com/azure/azure-quickstart-templates)
- Öğreticiler
  - [Linux VM'leri](https://aka.ms/LinuxVMtutorial)
  - [Windows VM’leri](https://aka.ms/windowsvms)
- Örnekler
  - [Linux VM'leri için Azure CLI Örnekleri](https://aka.ms/linuxclisamples)
  - [Linux VM'leri için Azure PowerShell](https://aka.ms/linuxpowershellsamples)
  - [Windows VM'ler için Azure CLI Örnekleri](https://aka.ms/windowsclisamples)
  - [Windows VM'ler için Azure PowerShell](https://aka.ms/windowspowershellvmsamples)

### <a name="fundamentals-in-technical-knowledge"></a>Teknik bilginin temelleri

Bu varlıkların tasarlanması, oluşturulması ve test edilmesi zaman alır ve hem Azure platformu hem de teklifi oluşturmak için kullanılan teknolojiler hakkında teknik bilgi gerektirir.

Mühendislik ekibiniz aşağıdaki Microsoft teknolojilerini anlamalıdır:

- [Azure Hizmetlerinin](https://azure.microsoft.com/services/) temel anlayışı
- Azure uygulamaları nasıl [tasarlar ve tasarlar](https://azure.microsoft.com/solutions/architecture/)
- [Azure Sanal Makineleri,](https://azure.microsoft.com/services/virtual-machines/) [Azure Depolama](https://azure.microsoft.com/services/?filter=storage#storage)ve [Azure Ağı](https://azure.microsoft.com/services/?filter=networking#networking) çalışma bilgisi

## <a name="create-an-azure-virtual-machine-offer"></a>Azure sanal makine teklifi oluşturma

Azure sanal makine teklifi oluşturabiliyor sanız, İş Ortağı Merkezi'nde ticari bir pazar hesabınız olması gerekir. Henüz bir tane oluşturmadıysanız, [bkz.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account)

1. [Ortak Merkezi'nde](https://partner.microsoft.com/dashboard/home)oturum açın ve ardından üst menüden **Pano'yu**seçin.
2. Sol navigasyon çubuğunda, **Commercial Marketplace'i**seçin, ardından **Genel Bakış'** ı seçin.
3. Genel **Bakış** sayfasında + **Yeni teklif ,** ardından Azure Sanal **Makine'yi**seçin. **Yeni teklif** iletişim kutusu görüntülenir.

![Yeni teklif düğmesi ve azure sanal makine teklifi seçilerek İş Ortağı Merkezi'ndeki Genel Bakış sayfasını gösterir.](media/avm-create7.png)

## <a name="offer-id-and-alias"></a>Teklif kimliği ve takma ad

Teklif **Kimliği**girin. Bu, hesabınızdaki her teklif için benzersiz bir tanımlayıcıdır.

- Bu kimlik, pazar teklifi için web adresinde ve varsa Azure PowerShell ve Azure CLI'de müşteriler tarafından görülebilir.
- Yalnızca küçük harfleri ve rakamları kullanın. Tireler ve alt çizerler içerebilir, ancak boşluk yoktur ve 50 karakterle sınırlıdır. Örneğin, **burada test-teklif-1** girerseniz, teklif web `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`adresi .
- **Teklif Kimliği, Oluştur'u**seçtikten sonra değiştirilemez.

Teklif **takma adını**girin. Bu, Ortak Merkezi'ndeki teklif için kullanılan addır. Bu ad pazarda kullanılmaz ve teklif adı ve müşterilere gösterilen diğer değerlerden farklıdır.

Teklifi oluşturmak ve devam etmek için **Oluştur'u** seçin.

## <a name="offer-setup"></a>Teklif kurulumu

### <a name="test-drive"></a>Test sürüşü

Müşterilerin teklifinizi satın almadan önce denemelerini sağlayan bir gösteri (test sürüşü) ayarlayın. Müşterilerin teklifinizi belirli bir süre boyunca denemelerine olanak tanıyan bir gösteri ortamı oluşturmak için, [teklifinizi ticari pazarda Test Sürüşü'ne](https://docs.microsoft.com/azure/marketplace/partner-center-portal/test-drive)bakın.

Test sürüşünü etkinleştirmek **için, test sürüşünü etkinleştir** onay kutusunu seçin. Test sürüşü teklifinizden kaldırmak için bu onay kutusunu temizleyin.

Ek test sürüşü kaynakları:

- [Teknik en iyi uygulamalar](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [En iyi pazarlama deneyimleri](https://aka.ms/TestDriveMarketingBestPractices)
- [Test Sürücüleri genel bakış](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) PDF (açılır pencere engelleyicinizin kapalı olduğundan emin olun).

### <a name="lead-management"></a>Müşteri adayı yönetimi

Teklifinizi İş Ortağı Merkezi ile ticari pazara yayınlarken, müşteri ilişkileri yönetimi (CRM) sisteminize bağlayın. Bu, birisi ürününüzün ilgisini ifade eder etmez veya ürününüzü kullanır kullanmaz müşteri iletişim bilgilerini almanızı sağlar. **Test Sürüşünü** etkinleştirecekseniz (önceki bölüme bakın), aksi takdirde isteğe bağlıdır.

1. Potansiyel müşterileri göndermemizi istediğiniz müşteri adayı hedefini seçin. İş Ortağı Merkezi aşağıdaki CRM sistemlerini destekler:
    - Müşteri katılımı için [Dynamics 365](https://aka.ms/Dyn365LeadMgmt)
    - [Marketo](https://aka.ms/LeadMgmtMarketo)
    - [Salesforce](https://aka.ms/LeadMgmtSalesforce)

    > [!NOTE]
    > CRM sisteminiz yukarıda listelenmemişse, müşteri müşteri adayı verilerini depolamak için [Azure Tablosu](https://aka.ms/AzureTableLeadMgmt) veya [Https Endpoint'i](https://aka.ms/LeadMgmtHTTPS) kullanın. Ardından verileri CRM sisteminize aktarın.

2. İş Ortağı Merkezi'nde yayın yaparken teklifinizi müşteri adayı hedefe bağlayın.
3. Müşteri adayı hedefine bağlantının düzgün şekilde yapılandırıldığından onaylayın. İş Ortağı Merkezi'nde yayımladıktan sonra bağlantıyı doğrular ve size bir test ipucu göndeririz. Teklifi yayına girmeden önce önizlemenize kadar, teklifi önizleme ortamında kendiniz dağıtmaya çalışarak müşteri adayı bağlantınızı da test edebilirsiniz.
4. Müşteri adayını kaybetmemek için müşteri adayı yla bağlantının güncel kalmasını sağlayın.

Devam etmeden önce **taslağı Kaydet'i** seçin.

## <a name="properties"></a>Özellikler

Bu sayfa, teklifinizi pazarda, uygulama sürümünüzde ve teklifinizi destekleyen yasal sözleşmelerde gruplandırmak için kullanılan kategorileri ve sektörleri tanımlamanıza olanak tanır.

### <a name="categories"></a>Kategoriler

En az bir ve en fazla beş kategori seçin. Bu kategoriler, teklifinizi uygun pazar yeri arama alanlarına yerleştirmek için kullanılır. Teklif açıklamasında, teklifinizin bu kategorileri nasıl desteklediğini açıklayın. Sanal makine teklifleri Azure Marketi'nde **Bilgi İşlem** kategorisi altında görünür.

### <a name="legal"></a>Yasal Bildirim

Teklif için şart ve koşulları sağlamalısınız. İki seçeneğiniz vardır:

- Kendi şart ve koşullarınızı kullanın
- Microsoft ticari pazarı için Standart Sözleşme'yi kullanma

#### <a name="use-your-own-terms-and-conditions"></a>Kendi şart ve koşullarınızı kullanın

Kendi özel hüküm ve koşullarınızı sağlamak **için, Şartlar ve Koşullar** kutusuna en fazla 10.000 karakter metin girin. Daha uzun bir açıklama gerekiyorsa, şart ve koşullarınızın nerede bulunabileceğini gösteren tek bir web adresi girin. Bu aktif bir bağlantı olarak müşterilere görüntülenir.

Müşteriler teklifinizi denemeden önce bu koşulları kabul etmelidir.

#### <a name="use-the-standard-contract-for-the-microsoft-commercial-marketplace"></a>Microsoft ticari pazarı için Standart Sözleşme'yi kullanma

Müşteriler için satın alma işlemini kolaylaştırmak ve yazılım satıcıları için yasal karmaşıklığı azaltmak için Microsoft, ticari pazar için standart bir sözleşme sunar. Yazılımınızı Standart Sözleşme kapsamında sunduğunuzda, müşterilerin yazılımı yalnızca bir kez okuması ve kabul etmesi gerekir ve özel hüküm ve koşullar oluşturmanız gerekmez.

**Microsoft'un ticari pazar yeri** onay kutusunu kullanarak Standart Sözleşmeyi Kullan'ı seçin ve ardından açılır pencerede **Kabul** Et 'i (görmek için yukarı kaydırmanız gerekebilir).

![Yeni teklif düğmesi ve Seçilen Danışmanlık hizmeti teklifiyle İş Ortağı Merkezi'ndeki Genel Bakış sayfasını gösterir.](media/use-standard-contract.png)

> [!NOTE]
> Ticari pazar için Standart Sözleşme'yi kullanarak bir teklif yayınladıktan sonra, kendi özel hüküm ve koşullarınızı kullanamazsınız. Çözümünüzü Standart Sözleşme kapsamında **veya** kendi hüküm ve koşullarınız altında sunun.

Standart Sözleşme hakkında daha fazla bilgi edinmek için Microsoft [ticari pazarı](https://docs.microsoft.com/azure/marketplace/standard-contract)için Standart Sözleşme'ye bakın. [Standart Sözleşmeyi](https://go.microsoft.com/fwlink/?linkid=2041178) PDF olarak indirebilirsiniz (açılır pencere engelleyicinizin kapalı olduğundan emin olun).

##### <a name="standard-contract-amendments"></a>Standart Sözleşme Değişiklikleri

Standart Sözleşme Değişiklikleri, basitlik için Standart Sözleşme koşullarını seçmenize ve ürününizin veya işletmenizin koşullarını oluşturmanıza izin vermez. Müşterilerin sözleşmedeğişikliklerini yalnızca Microsoft Standart Sözleşmesini gözden geçirip kabul etmişlerse gözden geçirmeleri gerekir. Evrensel ve özel olmak için iki tür değişiklik mevcuttur.

**Evrensel Değişiklikler** – Bunlar tüm müşteriler için Standart Sözleşme'ye evrensel olarak uygulanır. Bunlar, teklifin her müşterisine satın alma akışında gösterilir. Müşteriler, teklifinizi kullanabilmeleri için Standart Sözleşme'nin ve değişiklik koşullarını kabul etmelidir. Teklif başına tek bir evrensel değişiklik sağlayabilirsiniz. Bu kutuya sınırsız sayıda karakter girebilirsiniz. Bu koşullar, bulma ve satın alma akışı sırasında AppSource, Azure Marketplace ve/veya Azure portalında müşterilere görüntülenir.

**Özel Değişiklikler** – Bunlar, Azure kiracı sıdamaları aracılığıyla belirli müşterileri hedefleyen Standart Sözleşme'deki özel değişikliklerdir. Hedeflemek istediğiniz kiracıyı seçebilirsiniz. Yalnızca kiracının müşterilerine teklifin satın alma akışında özel değişiklik koşulları sunulacaktır. Müşteriler, teklifinizi kullanabilmeleri için Standart Sözleşme'nin ve değişiklik koşullarını kabul etmelidir.

Özel değişiklik **terimleri ekle 'yi (Max 10)** seçerek başlayın. Teklif başına en fazla on özel değişiklik şartı sağlayabilirsiniz.

- **Özel değişiklik koşulları** – Özel değişiklik terimleri kutusuna kendi değişiklik terimlerinizi girin. Sınırsız sayıda karakter girebilirsiniz. Bunları yalnızca bu özel koşullar için belirttiğiniz kiracı lı müşteriler, teklifin Azure portalındaki satın alma akışında görür.
- **Kiracı adları** (gerekli) – Her özel değişiklik en fazla 20 kiracı lı ekine hedeflenebilir. Özel bir değişiklik eklerseniz, Azure'da müşterinizi tanımlayan en az bir kiracı kimliği sağlamanız gerekir. müşteriniz sizin için Azure altında, ardından Özellikler'de bulabilir. Dizin kimliği değeri kiracı kimliğidir (örneğin, 50c464d3-4930-494c-963c-1e951d15360e). [Microsoft Azure ve Office 365 kiracı kimliğim nedir?](https://www.whatismytenantid.com/).
- **Açıklama** (isteğe bağlı) – Kiracı kimliği için, değişiklikle birlikte hedeflediğiniz müşteriyi belirlemenize yardımcı olan samimi bir açıklama sağlayın.

> [!NOTE]
> Bu iki değişiklik türü üst üste yığ. Özel değişikliklerle hedeflenen müşteriler, satın alma sırasında Standart Sözleşme'de evrensel değişiklik de elde edeceklerdir.

Devam etmeden önce **taslağı Kaydet'i** seçin.

## <a name="offer-listing"></a>Teklif listesi

Bu sayfa, teklif adı, açıklama, bağlantılar ve kişiler gibi teklif ayrıntılarını tanımlamanıza olanak tanır.

> [!NOTE]
> Teklif kaydı içeriği (açıklama, belge, ekran görüntüsü ve kullanım koşulları gibi) "Bu uygulama yalnızca [İngilizce olmayan dilde] ifadesiyle başladığı sürece İngilizce olması gerekmez." Ayrıca, Teklif listesi içeriğinde kullanılandan başka bir dilde içerik sunmak için yararlı bir _bağlantı web adresi_ de sağlayabilirsiniz.

### <a name="marketplace-details"></a>Pazar yeri detayları

#### <a name="name"></a>Adı

Buraya girdiğiniz ad, teklif listenizin başlığı olarak müşterilere gösterilir. Bu alan, teklifi oluşturduğunuzda Teklif **takma adı** kutusuna girdiğiniz metinle önceden doldurulmuştur. Bu adı daha sonra değiştirebilirsiniz.

Adı:

- Ticari marka olabilir (ve ticari marka ve telif hakkı sembolleri ekleyebilirsiniz)
- 50 karakterden uzun olamaz
- Emojileri içeremiyorum.

#### <a name="search-results-summary"></a>Arama sonuçları özeti

Teklifinizin kısa bir açıklaması. Bu, 100 karaktere kadar uzun olabilir ve pazar yeri arama sonuçlarında kullanılır.

#### <a name="long-summary"></a>Uzun özet

Teklifinizin daha uzun bir açıklamasını sağlayın. Bu, 256 karakter uzunluğunda olabilir ve pazar yeri arama sonuçlarında kullanılır.

#### <a name="description"></a>Açıklama

Teklifinizin 3.000 karaktere kadar ayrıntılı bir açıklamasını sağlayın. Bu, ticari pazar yeri listesine genel bakışta müşterilere görüntülenir.

Açıklamanıza aşağıdakilerden birini veya birkaçını ekleyin:

- Teklifinizin değeri ve temel avantajları
- Kategori veya endüstri dernekleri veya her ikisi
- Uygulama içi satın alma fırsatları
- Gerekli açıklamalar

Açıklamanızı yazmak için bazı ipuçları aşağıda verilmiştir:

- Teklifinizin değer teklifini açıklamanızın ilk birkaç cümlesinde açıkça açıklayın. Aşağıdaki öğeleri ekleyin:
  - Teklifinizin açıklaması.
  - Teklifinizden yararlanan kullanıcı türü.
  - Müşteri ihtiyaçları veya teklifin ele verdiği sorunlar.
- İlk birkaç cümlenin arama motoru sonuçlarında görüntülenebileceğini unutmayın.
- Teklifinizi satmak için özelliklere ve işlevselliğe güvenmeyin. Bunun yerine, teklifinizin sağladığı değere odaklanın.
- Sektöre özgü veya fayda tabanlı sözcükler kullanın.

Teklif açıklamanızı daha ilgi çekici hale getirmek için, açıklamanızı biçimlendirmek için zengin metin düzenleyicisini kullanın. Zengin metin düzenleyicisi, açıklamanızı daha okunabilir hale getirmek için sayılar, madde işaretleri, kalın, italik ler ve girintiler eklemenizi sağlar.

![Yeni teklif düğmesi ve Seçilen Danışmanlık hizmeti teklifiyle İş Ortağı Merkezi'ndeki Genel Bakış sayfasını gösterir.](media/power-bi-rich-text-editor.png)

#### <a name="privacy-policy-link"></a>Gizlilik politikası bağlantısı

Kuruluşunuzun gizlilik ilkesine web adresini (URL) girin. Teklifinizin gizlilik yasalarına ve yönetmeliklerine uygun olduğundan emin olun. Ayrıca web sitenizde geçerli bir gizlilik politikası yayınlamanız gerekir.

### <a name="useful-links"></a>Yararlı bağlantılar

Teklifiniz hakkında ek çevrimiçi belgeler sağlayın. Bağlantı eklemek için **+ Bağlantı ekle'yi** seçin ve ardından aşağıdaki alanları tamamlayın:

- **Ad** – Müşteriler detaylar sayfasında ismi görür.
- **Link (URL)** – Müşterilerin çevrimiçi belgenizi görüntülemesi için bir bağlantı girin.

### <a name="customer-support-links"></a>Müşteri destek bağlantıları

Müşterilerin destek ekibinize ulaşabileceği destek web sitesini sağlayın.

- Azure Global destek web sitesi
- Azure Devlet destek web sitesi

### <a name="partner-support-contact"></a>İş ortağı destek kişisi

Müşterileriniz bir destek bileti açtığında Microsoft iş ortaklarının kullanması için kişi bilgileri sağlayın. Bu pazarda listelenmez.

- Adı
- Email
- Telefon

### <a name="engineering-contact"></a>Mühendislik iletişim

Teklifinizde sertifikayla ilgili sorunlar da dahil olmak üzere sorunlar olduğunda Microsoft'un kullanması için kişi bilgileri sağlayın. Bu pazarda listelenmez.

- Adı
- Email
- Telefon

### <a name="marketplace-media"></a>Pazar yeri ortamı

Teklifinizle birlikte kullanılacak logolar ve resimler sağlayın. Tüm görüntüler PNG formatında olmalıdır. Bulanık görüntüler gönderinin reddedilmesine neden olur.

#### <a name="marketplace-logos"></a>Pazar yeri logoları

Teklifinizin logosunun PNG dosyalarını aşağıdaki dört piksel boyutunda sağlayın:

- **Küçük** (48 x 48)
- **Orta** (90 x 90)
- **Büyük** (216 x 216)
- **Geniş** (255 x 115)

Dört logo gereklidir ve pazar listesinde farklı yerlerde kullanılır.

#### <a name="screenshots"></a>Ekran görüntüleri

Teklifinizin nasıl çalıştığını gösteren en fazla beş ekran görüntüsü ekleyin. Her ekran görüntüsü 1280 x 720 piksel boyutunda ve PNG formatında olmalıdır. Ekran görüntüsünüze açıklamak için bir resim yazısı da eklemeniz gerekir.

#### <a name="videos"></a>Videolar

Teklifinizi gösteren en fazla beş video ekleyin. Bunlar harici bir video hizmetinde barındırılmalıdır. Her videonun adını, web adresini ve videonun küçük resim PNG görüntüsünü 1280 x 720 piksel boyutunda girin.

Ek pazar yeri listeleme kaynakları [için, pazar teklifi listeleri için en iyi uygulamalara](https://aka.ms/LdMgmtOfferListingBestPractices)bakın.

Devam etmeden önce **taslağı Kaydet'i** seçin.

## <a name="preview"></a>Önizleme

Önizleme sekmesinde, teklifinizi daha geniş pazar kitlesine canlı olarak yayınlamadan önce doğrulamak için sınırlı bir **Önizleme Hedef Kitlesi** seçin.

> [!IMPORTANT]
> Teklifinizi Preview'da kontrol ettikten sonra, teklifinizi ticari pazar daki herkese açık kitleye yayınlamak için **Canlı Yayınla'yı** seçin.

Önizleme hedef kitleniz Azure abonelik kimliği GUID'leri ve her biri için isteğe bağlı bir açıklama yla tanımlanır. Bu alanların hiçbiri müşteriler tarafından görülemez. Azure abonelik kimliğinizi Azure portalındaki **Abonelikler** sayfasında bulabilirsiniz.

Tek tek (10'a kadar) veya bir CSV dosyası yükleyerek (100'e kadar) en az bir Azure abonelik kimliği ekleyin. Bu abonelik eklerini ekleyerek, teklifinizi canlı olarak yayınlanmadan önce kimlerin önizebileceğinizi tanımlarsınız. Teklifiniz zaten yayındaysa, teklif değişikliklerini veya teklifinizi güncelleştirmeleri test etmek için bir önizleme hedef kitlesi tanımlayabilirsiniz.

> [!NOTE]
> Önizleme kitlesi özel hedef kitleden farklıdır. Önizleme kitlesi teklifinize pazarlarda canlı yayınlanmadan _önce_ erişebilir. Teklifiniz pazara tam olarak yayınlandıktan sonra yalnızca özel bir hedef kitletarafından kullanılabilecek planlar da dahil olmak üzere tüm planları görebilir ve doğrulayabilirler. Özel bir hedef kitle **(fiyatlandırma ve kullanılabilirlik** planında tanımlanan) belirli bir plana özel erişime sahiptir.

Bir sonraki bölüme devam etmeden önce **taslağı Kaydet'i** seçin, Genel Bakış Planı' nı planlayın.

## <a name="plan-overview"></a>Plana genel bakış

İş Ortağı Merkezi'nde aynı teklif dahilinde farklı plan seçenekleri sunabilirsiniz. Bu planlar daha önce SUS olarak anılırdı. Bir teklif, para kazanma hedef kitlesi, Azure bölgeleri, özellikler veya VM görüntüleri açısından farklılık gösteren en az bir plan gerektirir.

Planlarınızı oluşturduktan sonra, **Plana genel bakış** sekmesi şunları gösterir:

- Plan adları
- Lisans modelleri
- Hedef Kitle (genel veya özel)
- Geçerli yayımlama durumu
- Kullanılabilir eylemler

Plana genel bakış'ta bulunan eylemler, planınızın geçerli durumuna bağlı olarak değişir. Şunları içerir:

- **Taslağı sil** – Plan durumu taslaksa
- **Satış planını durdurun** veya **özel hedef kitleyi eşitle** - Plan durumu canlı olarak yayınlanırsa

### <a name="create-new-plan"></a>Yeni plan oluşturma

Seç + En üstte **yeni plan oluşturun.** **Yeni plan** iletişim kutusu görüntülenir.

Plan **Kimliği** kutusunda, bu teklifteki her plan için benzersiz bir plan kimliği oluşturun. Bu kimlik, ürün web adresindeki müşteriler tarafından görülebilir. Yalnızca küçük harfleri ve sayıları, tireleri veya alt çiziyi ve en fazla 50 karakter kullanın.

> [!NOTE]
> **Oluştur'u**seçtikten sonra plan kimliği değiştirilemez.

Plan **adı** kutusuna, bu plan için bir ad girin. Müşteriler, teklifiniz içinde hangi planı seçiniz gerekip tespin edindiğinde bu adı görür. Her planın farklılıklarını açıkça gösteren benzersiz bir ad oluşturun. Örneğin, **Windows Server'ı** **kullandıkça öde**, **BYOL**, **Advanced**ve **Enterprise**planlarını kullanabilirsiniz.

**Oluştur**’u seçin.

### <a name="plan-setup"></a>Kurulum planı

Plan türü için üst düzey yapılandırmayı, başka bir plandaki teknik yapılandırmayı yeniden kullanıp kullanmadığını ve planın hangi Azure bölgelerinde kullanılabileceğini ayarlayın. Buradaki seçimleriniz, aynı plan için diğer sekmelerde hangi alanların görüntüleneceğini belirler.

#### <a name="reuse-technical-configuration"></a>Teknik yapılandırmayı yeniden kullanma

Aynı türden birden fazla planınız varsa ve paketler aralarında aynıysa, **bu planı başka bir plandan teknik yapılandırmayı yeniden kullanır'ı**seçebilirsiniz. Bu seçenek, bu teklif için aynı türdeki diğer planlardan birini seçmenize ve teknik yapılandırmasını yeniden kullanmanıza olanak tanır.

> [!NOTE]
> Teknik yapılandırmayı başka bir plandan yeniden kullandığınızda, tüm **Teknik yapılandırma** sekmesi bu plandan kaybolur. Gelecekte yaptığınız güncelleştirmeler de dahil olmak üzere diğer plandaki Teknik yapılandırma ayrıntıları da bu plan için kullanılacaktır. Bu plan yayımlandıktan sonra bu ayar değiştirilemez.

#### <a name="azure-regions"></a>Azure bölgeleri

Planınız en az bir Azure bölgesinde kullanılabilir hale getirilmelidir.

Planınızı ticari pazar tümleştirmesi olan tüm genel Azure bölgelerindeki müşterilerin kullanımına açmak için **Azure Global** seçeneğini belirleyin. Ayrıntılar için [Bkz. Coğrafi kullanılabilirlik ve para birimi desteği.](https://docs.microsoft.com/azure/marketplace/marketplace-geo-availability-currencies)

Planınızı [Azure Kamu](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome) bölgesinde kullanılabilir hale getirmek için Azure **Kamu** seçeneğini belirleyin. Bu bölge, ABD federal, eyalet, yerel veya kabile kuruluşlarından gelen müşterilerin yanı sıra onlara hizmet vermeye uygun ortaklar için kontrollü erişim sağlar. Yayıncı olarak, uyumluluk denetimlerinden, güvenlik önlemlerinden ve en iyi uygulamalardan siz sorumlusunuz. Azure Kamu fiziksel olarak yalıtılmış veri merkezlerini ve ağlarını kullanır (yalnızca ABD'de bulunur).

[Azure Kamu'da](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners)yayımlamadan önce, belirli uç noktalar farklı olabileceğinden, planınızı ortamda test edin ve doğrulayın. Planınızı ayarlamak ve test etmek için [Microsoft Azure Kamu deneme sürümünden](https://azure.microsoft.com/global-infrastructure/government/request/)bir deneme hesabı isteyin.

> [!NOTE]
> Planınız yayımlandıktan ve belirli bir Azure bölgesinde kullanılabilir hale geldikten sonra, bu bölgeyi kaldıramazsınız.

#### <a name="azure-government-certifications"></a>Azure Devlet sertifikaları

Bu seçenek yalnızca **Azure bölgeleri**altında **Azure Kamu'yı** seçerseniz görünür.

Azure Kamu hizmetleri, belirli devlet yönetmeliklerine ve gereksinimlerine tabi olan verileri işler. Örneğin, FedRAMP, NIST 800.171 (DIB), ITAR, IRS 1075, DoD L4 ve CJIS. Bu programlar ait sertifikalarınıza farkındalık getirmek için, bunları açıklayan en fazla 100 bağlantı sağlayabilirsiniz. Bunlar doğrudan programdaki girişinize bağlantılar veya kendi web sitelerinizde bunlarla uyumluluğunun açıklamalarına bağlantılar olabilir. Bu bağlantılar yalnızca Azure Kamu müşterileri tarafından görülebilir.

Devam etmeden önce **taslağı Kaydet'i** seçin.

### <a name="plan-listing"></a>Plan listesi

Bu, planın giriş ayrıntılarını yapılandırdığınız yerdir. Bu sekme, aynı teklifteki planlar arasında farklılık gösteren belirli bilgileri görüntüler.

#### <a name="plan-name"></a>Plan adı

Bu, planı oluşturduğunuzda vermiş olduğunuz adla önceden doldurulmuştur. Bu ad pazarda bu planın başlığı olarak görünür ve 100 karakterle sınırlıdır.

#### <a name="plan-summary"></a>Plan özeti

Planınızın kısa bir özetini sağlayın (teklifi değil). Bu özet 100 karakterle sınırlıdır.

#### <a name="plan-description"></a>Plan açıklaması

Bu yazılım planını benzersiz kılan ın yanı sıra teklifinizdeki planlar arasındaki farkları açıklayın. Teklifi tarif etme, sadece planı. Plan açıklaması en fazla 2.000 karakter içerebilir.

Devam etmeden önce **taslağı Kaydet'i** seçin.

### <a name="pricing-and-availability"></a>Fiyatlandırma ve kullanılabilirlik

Bu sekmede, aşağıdakileri yapılandıracaksınız:

- Bu planın mevcut olacağı pazarlar
- Saat başına fiyat
- Planı herkes veya yalnızca belirli müşteriler (özel hedef kitle) tarafından görünür hale getirip getirmeyeceği

#### <a name="markets"></a>Piyasa

Her plan en az bir pazarda mevcut olmalıdır. Bu planın satın alınabileceği her pazar konumu için onay kutusunu seçin (bu pazarlardaki kullanıcılar teklifi **[Plan kurulumunda](#plan-setup)** seçilen tüm Azure bölgelerine dağıtmaya devam edebilir). **Vergi Havalesi** düğmesi, Microsoft'un satışları sizin adınıza havale ettiği ve vergi kullandığı ülkeleri gösterir. Çin'de yayıncılık ya **Ücretsiz** ya da kendi lisans (BYOL) **getir** planları ile sınırlıdır.

Planınız için fiyatları Abd Doları (USD) olarak belirlediyseniz ve başka bir pazar konumu eklediyseniz, yeni piyasa fiyatı geçerli döviz kurlarına göre hesaplanacaktır. Her zaman yayınlamadan önce her piyasa için fiyat gözden geçirin. Değişikliklerinizi kaydettikten sonra **İhracat fiyatları (xlsx)** bağlantısını kullanarak fiyatları gözden geçirin.

Bir pazarı kaldırdığınızda, etkin dağıtımları kullanan müşteriler yeni dağıtımlar oluşturamaz veya varolan dağıtımlarını ölçeklendiremez. Varolan dağıtımlar etkilenmez.

#### <a name="pricing"></a>Fiyatlandırma

**Lisans modeli** – Bu plan için fiyatlandırmayı yapılandırmak için **Kullanım tabanlı aylık faturalı planı** seçin veya müşterilerin bu planı mevcut lisanslarıyla kullanmalarına izin vermek için kendi **lisansınızı getirin.**

Kullanım tabanlı aylık faturalı plan için aşağıdaki üç fiyatlandırma giriş seçeneğinden birini kullanın:

- **Çekirdek başına** – Abd Doları (USD) cinsinden çekirdek başına fiyatı sağlayın. Çekirdek boyutu başına fiyatlandırmayı hesaplar ve geçerli döviz kuru kullanarak yerel para birimlerine dönüştürüriz.
- **Çekirdek boyutu başına** - USD çekirdek boyutu başına fiyatları sağlayın. Geçerli döviz kurunu kullanarak fiyatları yerel para birimlerine dönüştüreceğiz.
- **Pazar ve çekirdek boyutu başına** - Tüm pazarlar için her çekirdek boyutu için fiyat sağlayın. Fiyatları elektronik tablodan içe aktarabilirsiniz.

> [!NOTE]
> Fiyatlandırma verilerinin dışa aktarılmasını etkinleştirmek için fiyatlandırma değişikliklerini kaydedin. Planınızdaki bir piyasa fiyatı yayınlandıktan sonra, daha sonra değiştirilemez. Fiyatlandırma elektronik tablosunu dışa aktararak ve her pazardaki fiyatı gözden geçirerek bu fiyatların yayımlanmadan hemen önce olduğundan emin olun.

#### <a name="free-trial"></a>Ücretsiz deneme sürümü

Müşterilerinize bir aylık veya üç aylık ücretsiz deneme sürümü sunabilirsiniz.

#### <a name="visibility"></a>Görünürlük

Her planı herkes tarafından görülebilecek veya yalnızca önceden seçilmiş bir hedef kitleye göre tasarlayabilirsiniz. Azure abonelik teşekklerini kullanarak bu kısıtlı hedef kitleye üyelik atayın.

**Kamu** - Planınız herkes tarafından görülebilir.

**Özel hedef kitle** – Planınızı yalnızca önceden seçilmiş bir hedef kitle tarafından görünür hale getirin. Özel bir plan olarak yayımlandıktan sonra, hedef kitleyi güncelleyebilir veya herkese açık olarak değiştirebilirsiniz. Bir planı halka açıklandıktan sonra, halka açık lanmalıdır; Özel olarak değiştiremezsin.

**Kısıtlı Hedef Kitle (Azure abonelik teşekali)** – Azure abonelik teşekküllerini kullanarak bu özel plana erişebilecek hedef kitleyi atayın. İsteğe bağlı olarak, atadığınız her Azure abonelik kimliğinin açıklamasını ekleyin. Bir CSV elektronik tablosu içe aktarıyorsanız el ile 10'a kadar abonelik tonu veya 20.000'e kadar ekleme. Azure abonelik teşekkünleri GUID olarak gösterilir ve harfler küçük olmalıdır.

> [!NOTE]
> Özel veya kısıtlı hedef kitle, **Önizleme** sekmesinde tanımladığınız önizleme hedef kitlesinden farklıdır. Önizleme kitlesi teklifinize pazarda yayınlanmadan _önce_ erişebilir. Özel hedef kitle seçimi yalnızca belirli bir plan için geçerli olsa da, önizleme hedef kitlesi doğrulama amacıyla tüm planları (özel veya özel veya değil) görüntüleyebilir.

#### <a name="hide-plan"></a>Planı gizle

Sanal makineniz başka bir çözüm şablonu veya yönetilen uygulama aracılığıyla başvurulduğunda yalnızca dolaylı olarak kullanılacaksa, sanal makinenizi yayımlamak için bu kutuyu seçin, ancak bu kutuyu doğrudan arayan ve doğrudan gezen müşterilerden gizleyin.

> [!NOTE]
> Gizli planlar önizleme bağlantılarını desteklemez.

Devam etmeden önce **taslağı Kaydet'i** seçin.

### <a name="technical-configuration"></a>Teknik yapılandırma

Bu planla ilişkili görüntüleri ve diğer teknik özellikleri sağlayın. Ayrıntılar için [bkz.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-azure-container-technical-assets)

> [!NOTE]
> Plan **kurulumu** sekmesindeki başka bir plandaki paketleri yeniden kullanmak üzere bu planı yapılandırırsanız bu sekme gösterilmez.

#### <a name="operating-system"></a>İşletim Sistemi

- **İşletim sistemi ailesi** – **Windows** veya **Linux** işletim sisteminden seçin
- **Release** or **Vendor** – Windows sürümü veya Linux satıcısını seçin
- **İşletim sistemi dostu ad** – Kolay bir işletim sistemi adı seçin. Bu ad müşteriler tarafından görülebilir

#### <a name="recommended-vm-sizes"></a>Önerilen VM Boyutları

Azure Marketi'nde görüntülenmek üzere önerilen en fazla altı sanal makine boyutu seçin.

#### <a name="open-ports"></a>Bağlantı noktalarını açma

Dağıtılan bir sanal makinede ortak veya özel bağlantı noktalarını açın.

#### <a name="storage-option-for-deployment"></a>Dağıtım için depolama seçeneği

**Disk dağıtımı seçeneği** – Kullanıcılarınızın sanal makineyi kullanırken ne tür disk dağıtımı kullanabileceğini seçin. Microsoft, dağıtımı yalnızca Yönetilen disk dağıtımıyla sınırlamanızı önerir.

#### <a name="properties"></a>Özellikler

**Hızlandırılmış ağ desteğini destekleyin** – VM'niz [hızlandırılmış ağ iletişimini](https://go.microsoft.com/fwlink/?linkid=2124513)destekliyorsa seçin.

#### <a name="vm-images"></a>VM Görüntüleri

Sanal makine görüntüleri için bir disk sürümü ve SAS URI sağlayın. Her VM görüntü için en fazla 16 veri diski ekleyin. Belirli bir gönderimde plan başına yalnızca bir yeni resim sürümü sağlayın. Bir resim yayımlandıktan sonra onu düzenleyebilirsiniz, ancak silebilirsiniz. Sürümü silerken, hem yeni hem de varolan kullanıcıların silinen sürümün yeni bir örneğini dağıtmasını engeller.

- **Disk sürümü,** sağladığınız görüntünün sürümüdür.
- **SAS URI,** Azure Depolama'da işletim sistemi VHD'yi depoladığınız konumdur.
- Veri diski görüntüleri, Azure depolamalarında depolanan VHD SAS URI'leri dir.
- Bir planda gönderim başına yalnızca bir görüntü ekleyin.

Hangi işletim sistemini kullandığınızdan bağımsız olarak, çözümün gerektirdiği en az sayıda veri diski ekleyin. Müşteriler dağıtım sırasında görüntünün parçası olan diskleri kaldıramaz, ancak dağıtım sırasında veya sonrasında her zaman disk ekleyebilirler.

Devam etmeden önce **taslağı Kaydet'i** seçin ve **Plan'a genel bakış'a**dönün.

## <a name="resell-through-csps"></a>CSP'ler aracılığıyla yeniden sat

[Bulut Çözüm Sağlayıcıları](https://azure.microsoft.com/offers/ms-azr-0145p/) (CSP) programındaki iş ortaklarının kullanımına sunarak teklifinizin erişimini genişletin. Tüm Bring Your Own Lisans (BYOL) planları otomatik olarak tercih edilir; BYOL dışı planlarınızı tercih edebilirsiniz.

Devam etmeden önce **taslağı Kaydet'i** seçin.

## <a name="test-drive"></a>Test sürüşü

Müşterilerin teklifinizi satın almadan önce denemelerini sağlayan bir gösteri (test sürüşü) ayarlayın. Müşterilerin teklifinizi belirli bir süre boyunca denemelerine olanak tanıyan bir gösteri ortamı oluşturmak için, [teklifinizi ticari pazarda Test Sürüşü'ne](https://docs.microsoft.com/azure/marketplace/partner-center-portal/test-drive)bakın.

Test sürüşünü etkinleştirmek için [Teklif kurulumu](#test-drive) sekmesinde test sürücüsü onay kutusunu etkinleştir'i seçin. Test sürüşü teklifinizden kaldırmak için bu onay kutusunu temizleyin.

Ek test sürüşü kaynakları:

- Teknik en iyi uygulamalar
- En iyi pazarlama deneyimleri
- Test Sürüşleri genel bakışı (PDF; açılır pencere engelleyicinizin kapalı olduğundan emin olun).

Devam etmeden önce **taslağı Kaydet'i** seçin.

## <a name="review-and-publish"></a>İnceleme ve yayınlama

Teklifin gerekli tüm bölümlerini tamamladıktan sonra, bunu incelemek ve yayınlamak için gönderebilirsiniz.

Portalın sağ üst köşesinde, **Gözden Geçir'i seçin ve yayınlayın.**

Bu sayfada şunları yapabilirsiniz:

- Teklifin her bölümü için tamamlanma durumuna bakın.
  - **Başlatılmamasın** – Bölüm başlatılmamamıştır ve tamamlanması gerekir.
  - **Eksik** – Bölümde düzeltilmesi gereken veya daha fazla bilgi vermenizi gerektiren hatalar vardır. Bu bölümün nasıl güncelleştirilene ilişkin rehberlik için bu belgedeki önceki bölümlere bakın.
  - **Tam** - Bölüm tamamlandı ve herhangi bir hata yok. Teklifi sunamadan önce teklifin tüm bölümlerinin tamamlanmış olması gerekmektedir.
- **Sertifika notları** – Uygulamanızın doğru test edildiğinden emin olmak için sertifika ekibine test talimatları sağlayın. Uygulamanızı anlamak için yararlı olan ek notlar sağlayın.

Yayımlama teklifini göndermek için **Gözden Geçir'i seçin ve yayımlayın.**

Teklifin önizleme sürümünün gözden geçirilip onaylamayacağını bildirmek için size bir e-posta göndeririz. Teklifinizi herkese (veya özel bir hedef kitleye özel bir teklifte) yayınlamak için İş Ortağı Merkezi'ne gidin, teklifinizin **Genel Bakış** sayfasını bulun ve **Go-live'ı**seçin. Yayımlama devam ederken teklifinizin durumu sayfanın üst kısmında görünür.

### <a name="errors-and-review-feedback"></a>Hatalar ve gözden geçirme geri bildirimi

Yayımlama işlemindeki **Manuel doğrulama** adımı, teklifinizin ve ilişkili teknik varlıklarının kapsamlı bir incelemesini temsil eder. Bu işlem teklifinizdeki hataları ortaya çıkarırsa, sorunları ayrıntılarıyla anlatan bir sertifika raporu alırsınız. Sadece gerekli düzeltmeleri yapın ve teklifinizi yeniden yayınlayın.

## <a name="offer-overview"></a>Teklife genel bakış

**Teklife genel bakış** sayfası, bu teklifi yayımlamak için gereken adımların (hem tamamlanmış hem de devam etmekte olan) ve her adımın tamamlanmasının ne kadar sürmesi gerektiğinin görsel bir gösterimini gösterir.

Bu sayfa, yaptığınız seçime dayalı olarak bu teklifle ilgili işlemleri gerçekleştirmek için bağlantılar içerir. Örneğin:

- Teklif taslaksa - [Taslak teklifi sil](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#delete-a-draft-offer)
- Teklif canlıise - [Teklifi satmayı bırakın](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#stop-selling-an-offer-or-plan)
- Teklif önizlemedeyse - [Canlı Yayına Geçin](https://docs.microsoft.com/azure/marketplace/partner-center-portal/publishing-status#publisher-approval)
- Yayımcı oturumkapatmasını tamamlamadıysanız - [Yayımlamayı iptal et](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#cancel-publishing)

## <a name="marketplace-examples"></a>Pazar yeri örnekleri

Bu örnekler, teklifin Azure Marketi'nde nasıl göründüğünü gösterir.

### <a name="azure-marketplace-offer-details"></a>Azure Marketi ayrıntıları sunar

![Azure Marketi ayrıntılar ekran örneği sunar](media/avm-create1.png)

### <a name="azure-marketplace-search-results"></a>Azure Marketi arama sonuçları

![Azure Marketi arama ayrıntıları ekran örneği](media/avm-create2.png)

### <a name="azure-marketplace-plan-details"></a>Azure Marketi planı ayrıntıları

![Azure Marketplace planı ayrıntıları ekran örneği](media/avm-create3.png)

### <a name="azure-portal-offer-details"></a>Azure portalı ayrıntıları sunar

![Azure portalı, ayrıntılar ekran örneği sunar](media/avm-create4.png)

### <a name="azure-portal-search-results"></a>Azure portalı arama sonuçları

![Azure portal arama sonuçları ekran örneği](media/avm-create5.png)

### <a name="azure-portal-plan-details"></a>Azure portal planı ayrıntıları

![Azure portal planı ayrıntıları ekran örneği](media/avm-create6.png)

## <a name="next-step"></a>Sonraki adım

- [Ticari pazarda mevcut bir teklifi güncelleştirme](https://aka.ms/UpdateOfferCM)
