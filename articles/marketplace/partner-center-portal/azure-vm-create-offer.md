---
title: Azure Market 'te Azure sanal makine teklifi oluşturma
description: Gerekli planla Azure Marketi 'nde sanal makine teklifi oluşturmayı öğrenin.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: emuench
ms.author: mingshen
ms.date: 07/17/2020
ms.openlocfilehash: 6df1620f2d1097ac23e5dfa3903fefc74d13fa65
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87012034"
---
# <a name="create-an-azure-virtual-machine-offer-on-azure-marketplace"></a>Azure Market 'te Azure sanal makine teklifi oluşturma

Bu makalede, Azure [Market 'te](https://azuremarketplace.microsoft.com/)bir Azure sanal makine teklifinin nasıl oluşturulacağı ve yayımlanacağı açıklanır. Bir işletim sistemi, bir sanal sabit disk (VHD) ve en çok 16 veri diski içeren Windows tabanlı ve Linux tabanlı sanal makinelere yöneliktir. 

Başlamadan önce [Iş Ortağı Merkezi 'nde bir ticari Market hesabı oluşturun](create-account.md). Hesabınızın ticari Market programına kayıtlı olduğundan emin olun.

## <a name="introduction"></a>Giriş

### <a name="the-benefits-of-publishing-to-azure-marketplace"></a>Azure Marketi 'ne yayımlamanın avantajları

Tekliflerinizi Azure Marketi 'nde yayımladığınızda şunları yapabilirsiniz:

- Microsoft marka 'in yardımıyla şirketinizi yükseltin.
- 100.000.000 Office 365 ve Dynamics 365 kullanıcıları ve 200.000 'den fazla kuruluşa ulaşın.
- Bu Pazar yerlerden yüksek kaliteli müşteri adayları elde edin.
- Hizmetlerinizi Microsoft alanı Sales ve Telesales ekipleri tarafından yükseltilen hizmetlerden yararlanın.

### <a name="before-you-begin"></a>Başlamadan önce

Henüz yapmadıysanız, [Sanal Makine teklifi yayımlama kılavuzunu](../marketplace-virtual-machines.md) ve bu Azure sanal makine malzemesini gözden geçirin:

- Hızlı Başlangıç Kılavuzu
  - [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/resources/templates/)
  - [GitHub Azure hızlı başlangıç şablonları](https://github.com/azure/azure-quickstart-templates)
- Öğreticiler
  - [Linux VM'leri](../../virtual-machines/linux/tutorial-manage-vm.md)
  - [Windows VM'leri](../../virtual-machines/windows/tutorial-manage-vm.md)
- Örnekler
  - [Linux VM 'Ler için Azure CLı örnekleri](../../virtual-machines/linux/cli-samples.md)
  - [Linux sanal makineleri için Azure PowerShell](../../virtual-machines/linux/powershell-samples.md)
  - [Windows VM 'Leri için Azure CLı örnekleri](../../virtual-machines/windows/cli-samples.md)
  - [Windows VM 'Leri için Azure PowerShell](../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-quick.md)

### <a name="fundamentals-in-technical-knowledge"></a>Teknik Bilgi Bankası temelleri

Teklifleri tasarlama, oluşturma ve test etme işlemi zaman alır ve hem Azure platformunda hem de teklifinizi oluşturmak için kullanılan teknolojilerde uzmanlık gerektirir.

Mühendislik ekibiniz aşağıdaki Microsoft teknolojileriyle temel bir anlama ve çalışma bilgisine sahip olmalıdır:

- [Azure hizmetleri](https://azure.microsoft.com/services/)
- [Azure uygulamalarının tasarımı ve mimarisi](https://azure.microsoft.com/solutions/architecture/)
- [Azure sanal makineleri](https://azure.microsoft.com/services/virtual-machines/), [Azure depolama](https://azure.microsoft.com/services/?filter=storage#storage)ve [Azure ağı](https://azure.microsoft.com/services/?filter=networking#networking)

## <a name="create-a-new-offer"></a>Yeni teklif oluşturma

1. [Iş Ortağı Merkezi](https://partner.microsoft.com/dashboard/home)' nde oturum açın.
2. Sol bölmede, **ticari Market**  >  **genel bakış**' ı seçin.
3. **Genel bakış** sayfasında, **yeni teklif**  >  **Azure sanal makinesi**' ni seçin.

    ![Sol bölme menü seçeneklerini ve "yeni teklif" düğmesini gösteren ekran görüntüsü.](./media/new-offer-azure-virtual-machine.png)

> [!NOTE]
> Teklifiniz yayımlandıktan sonra, Iş Ortağı Merkezi 'nde üzerinde yaptığınız herhangi bir düzenleme, yalnızca teklifi yeniden yayımladığınızda Azure Marketi 'nde görünür. Bir teklifi üzerinde değişiklik yaptıktan sonra her zaman yeniden yayımlamayı unutmayın.

## <a name="new-offer"></a>Yeni teklif

Bir **TEKLIF kimliği**girin. Bu, hesabınızdaki her teklif için benzersiz bir tanımlayıcıdır.

- Bu KIMLIK, Azure Marketi teklifinin Web adresindeki müşterilere ve varsa Azure PowerShell ve Azure CLı 'ye yönelik olarak görünür.
- Yalnızca küçük harfleri ve rakamları kullanın. KIMLIK, kısa çizgi ve alt çizgi içerebilir, ancak boşluk içeremez ve 50 karakterle sınırlıdır. Örneğin, **Test-teklif-1**girerseniz, teklif Web adresi olacaktır `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1` .
- Teklif KIMLIĞI, **Oluştur**' u seçtikten sonra değiştirilemez.

Bir **teklif diğer adı**girin. Teklif diğer adı, Iş Ortağı Merkezi 'nde teklif için kullanılan addır.

- Bu ad Azure Marketi 'nde kullanılmaz. Teklif adından ve müşterilere gösterilen diğer değerlerden farklıdır.

Teklifi oluşturmak için **Oluştur** ' u seçin ve devam edin.

## <a name="offer-setup"></a>Teklif kurulumu

### <a name="test-drive"></a>Test Sürüşü

*Test sürücüsü* , teklifinizi potansiyel müşterilere sergilemenin harika bir yoludur. "Satın almadan önce dene" seçeneğini sunar; bu, dönüştürmelerin artırılmasına ve yüksek oranda nitelikli müşteri adayları oluşturmanıza yardımcı olabilir. Daha fazla bilgi için bkz. [test sürücüsü nedir?](../what-is-test-drive.md).

Bir sınama sürücüsünü sabit bir süre etkinleştirmek için, **bir sınama sürücüsünü etkinleştir** onay kutusunu seçin. Test sürücüsünü teklifinizden kaldırmak için onay kutusunu temizleyin.

Ek test sürücüsü kaynakları:

- [En iyi teknik uygulamalar](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [En iyi pazarlama deneyimleri](../what-is-test-drive.md)
- [Test sürücüsüne genel bakış indirin](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) PDF dosyası (açılır pencere engelinizin kapalı olduğundan emin olun).

### <a name="customer-leads"></a>Müşteri liderleri

Teklifinizi Iş Ortağı Merkezi ile ticari Market 'te yayımladığınızda, Müşteri Ilişkileri yönetimi (CRM) sisteminize bağlayın. Bu, bir kişi, ürününüzü ilgilendiren veya ürünü kullandığı anda müşteri iletişim bilgilerini almanızı sağlar. Bir test sürücüsünü etkinleştirmek istiyorsanız, bir CRM 'ye bağlanmak gerekir (yukarıdaki bölüme bakın). Aksi takdirde, bir CRM 'ye bağlanmak isteğe bağlıdır.

1. Potansiyel müşterileri göndermemizi istediğiniz müşteri adayı hedefini seçin. İş Ortağı Merkezi aşağıdaki CRM sistemlerini destekler:
    - Müşteri katılımı için [Dynamics 365](commercial-marketplace-lead-management-instructions-dynamics.md)
    - [Marketo](commercial-marketplace-lead-management-instructions-marketo.md)
    - [Salesforce](commercial-marketplace-lead-management-instructions-salesforce.md)

    > [!NOTE]
    > CRM sisteminiz burada listelenmiyorsa, müşteri adayı verilerinizi depolamak için [Azure Tablo Depolaması](commercial-marketplace-lead-management-instructions-azure-table.md) 'nı veya bir [https uç noktasını](commercial-marketplace-lead-management-instructions-https.md) kullanın. Ardından verileri CRM sisteminize dışarı aktarın.

1. Teklifinizi Iş Ortağı Merkezi 'nde yayımlarken lider hedefine bağlayın.
1. Müşteri adayı hedefi bağlantısının düzgün şekilde yapılandırıldığını doğrulayın. Iş Ortağı Merkezi 'nde yayımladıktan sonra, Microsoft bağlantıyı doğrular ve size bir test lideri gönderir. Teklifi canlı olmadan önce önizlerken, teklifi önizleme ortamında dağıtmayı deneyerek lider bağlantınızı da test edebilirsiniz.
1. Müşteri adaylarını kaybetmemek için, müşteri adayı hedefi bağlantısının güncelleştirildiğinden emin olun.

1. Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="properties"></a>Özellikler

**Özellikler** sayfasında, teklifinizi Azure Marketi 'nde, uygulamanızın sürümüne ve teklifinizi destekleyen yasal sözleşmeleri gruplamak için kullanılan kategorileri tanımlarsınız.

### <a name="category"></a>Kategori

Teklifinizi uygun Market arama alanlarında yerleştirmek için kategoriler ve alt kategoriler ' i seçin. Teklifinizin teklif açıklamasında bu kategorileri nasıl desteklediğini açıkladığınızdan emin olun. Seçin:

- Birincil ve ikincil kategori dahil olmak üzere en az bir ve en fazla iki kategori (isteğe bağlı).
- Her birincil ve/veya ikincil kategori için en fazla iki alt kategori. Teklifiniz için geçerli bir alt kategori yoksa, **geçerli değil**' i seçin.

[Teklif listesi En Iyi uygulamalarında](../gtm-offer-listing-best-practices.md)kategorilerin ve alt kategorilerin tam listesini görebilirsiniz. Sanal makine teklifleri her zaman Azure Marketi 'ndeki **işlem** kategorisi altında görünür.

### <a name="legal"></a>Yasal Bildirim

Müşterilerinize teklif hüküm ve koşullar sağlamanız gerekir. İki seçeneğiniz vardır:

- **Kendi hüküm ve koşullarınızı kullanın**

   Kendi özel hüküm ve koşullarınızı sağlamak için **hüküm ve koşullar** kutusuna en fazla 10.000 karakter girin. Daha uzun bir açıklamaya ihtiyacınız varsa hüküm ve koşullarınız için tek bir Web adresi girin. Bu, müşterilere etkin bir bağlantı olarak görüntülenecektir.

   Müşterilerinizin teklifinizi deneyebilmeleri için önce bu koşulları kabul etmesi gerekir.

- **Microsoft ticari Market için standart sözleşmeyi kullanın**

   Müşteriler için satın alma sürecini basitleştirmek ve yazılım satıcılarının yasal karmaşıklığını azaltmak için, Microsoft ticari Market için standart bir sözleşme sunmaktadır. Yazılımınızı standart sözleşme kapsamında sunduktan sonra, müşterilerin onu yalnızca bir kez okuyup kabul etmesi ve özel hüküm ve koşullar oluşturmanız gerekmez.

   Standart sözleşmeyi, **Microsoft 'un ticari Market Için standart sözleşmeyi kullan** onay kutusunu seçerek ve ardından açılan pencerede **kabul et** ' i seçin (bunu görüntülemek için kaydırmanız gerekebilir).

   !["Microsoft 'un ticari Marketi için standart sözleşmeyi kullanın" onay kutusunu kullanarak Iş Ortağı Merkezi 'nde yasal bölmeyi gösteren ekran görüntüsü.](media/use-standard-contract.png)

  > [!NOTE]
  > Ticari Market için standart sözleşmeyi kullanarak bir teklifi yayımladıktan sonra, kendi özel hüküm ve koşullarınızı kullanamazsınız. Standart Sözleşme veya kendi hüküm ve koşullarınız altında bir çözüm sunabilirsiniz.

  Daha fazla bilgi için bkz. [Microsoft ticari Market Için standart sözleşme](../standard-contract.md). [Standart sözleşme](https://go.microsoft.com/fwlink/?linkid=2041178) PDF dosyasını indirin (açılır pencere engelinizin kapalı olduğundan emin olun).

  **Standart Sözleşme düzeltme açıklamaları**

  Standart anlaşma değişikliği, basitlik için standart sözleşme koşullarını seçmenizi ve ürün veya işletmenizin koşullarını oluşturmayı sağlar. Müşterilerin, Microsoft standart sözleşme 'yi zaten gözden geçirdikleri ve kabul etmiş olmaları durumunda, gereksinimleri sözleşmeye göre gözden geçirmesi gerekir. İki tür düzeltme vardır:

  * **Evrensel**değişiklik: Bu değişiklik, tüm müşteriler Için standart sözleşmeye evrensel olarak uygulanır. Satın alma akışındaki teklifin her müşterisi için gösterilir. Müşteriler, teklifinizi kullanabilmeniz için standart sözleşme ve değişiklik koşullarını kabul etmelidir. Teklif başına tek bir evrensel düzeltme sağlayabilirsiniz. Bu kutuya sınırsız sayıda karakter girebilirsiniz. Bu terimler, bulma ve satın alma akışında AppSource, Azure Marketi ve/veya Azure portal müşterilere gösterilir.

  * **Özel**değişiklik: standart sözleşmeye yönelik bu özel değişiklik, Azure kiracı kimlikleri aracılığıyla belirli müşterilere yöneliktir. Hedeflemek istediğiniz kiracıyı seçebilirsiniz. Yalnızca kiracının müşterileri, teklifin satın alma akışındaki özel değişiklik hükümleriyle sunulur. Müşteriler, teklifinizi kullanabilmeniz için standart sözleşme ve değişiklik koşullarını kabul etmelidir.

    1. **Özel düzeltme koşulları Ekle ' ye (en fazla 10)** tıklayarak başlayın. Teklif başına en fazla on özel düzeltme terimi sağlayabilirsiniz. Şunları yapın:

       a. **Özel düzeltme** koşulları kutusuna kendi değişiklik koşullarınızı girin. Sınırsız sayıda karakter girebilirsiniz. Yalnızca bu özel şartlar için belirttiğiniz kiracı kimliklerinin müşterileri, Azure portal teklifin satın alma akışında görür.

       b. Istenir **Kiracı kimlikleri**sağlayın. Her özel değişiklik, en fazla 20 Kiracı kimliğini hedefleyebilir. Özel bir düzeltme eklerseniz, Azure 'da müşterinizin tanımlandığı en az bir kiracı KIMLIĞI sağlamalısınız. Müşteriniz, **Azure Active Directory**Özellikler ' i seçerek bunu sizin için Azure 'da bulabilir  >  **Properties**. Dizin KIMLIĞI değeri kiracı KIMLIĞIDIR (örneğin, 50c464d3-4930-494c-963c-1e951d15360e). Ayrıca, [Microsoft Azure ve Office 365 KIRACı kimliği olan](https://www.whatismytenantid.com/)etki alanı adı Web adresini kullanarak KURULUŞUNUZUN Kiracı kimliğini de bulabilirsiniz.

       c. Seçim Kiracı KIMLIĞI için, değişiklik yaparken hedeflediğiniz müşteriyi belirlemenize yardımcı olan kolay bir **Açıklama** sağlayın.

        > [!NOTE]
        > Bu iki değişiklik türü birbirleriyle eşleştirilmiştir. Özel değişiklik ile hedeflenen müşteriler, satın alma sırasında standart sözleşmeye da evrensel değişiklik de sağlar.

    1. Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="offer-listing"></a>Teklif listesi

**Teklif listesi** sayfasında, teklif adı, açıklama, bağlantılar ve kişiler gibi teklif ayrıntılarını tanımlarsınız.

> [!NOTE]
> Teklif açıklaması, "Bu uygulama yalnızca içinde kullanılabilir." ifadesi ile başladığı sürece, açıklama, belgeler, ekran görüntüleri ve kullanım koşulları gibi teklif listesi içeriğinin Ingilizce olması gerekmez \<non-English language> . Ayrıca, teklif listesi içeriğinde kullanılandan farklı bir dilde içerik sunan bir siteye bağlantı sağlayan bir URL de sağlayabilirsiniz.

### <a name="marketplace-details"></a>Market ayrıntıları

#### <a name="name"></a>Name

Buraya girdiğiniz ad, müşteriler için teklif listelerinizin başlığı olarak gösterilir. Bu alan, teklifi oluştururken **teklif diğer** adı kutusuna girdiğiniz adla birlikte doldurulur. Bu adı daha sonra değiştirebilirsiniz. Ad:

- Trademarked olabilir. Ticari marka ve telif hakkı sembolleri dahil edebilirsiniz.
- 50 karakterden fazlasını içeremez.
- Emojıs dahil olamaz.

#### <a name="search-results-summary"></a>Arama sonuçları Özeti

Azure Marketi arama sonuçlarında gösterilmek üzere teklifinizin kısa bir açıklamasını sağlayın. Bu, en fazla 100 karakter içerebilir.

#### <a name="long-summary"></a>Uzun Özet

Azure Marketi arama sonuçlarında gösterilmek üzere teklifiniz için daha uzun bir açıklama sağlayın. Bu, en fazla 256 karakter içerebilir.

#### <a name="description"></a>Açıklama

[!INCLUDE [Long description-1](./includes/long-description-1.md)]

[!INCLUDE [Long description-2](./includes/long-description-2.md)]

[!INCLUDE [Long description-3](./includes/long-description-3.md)]

#### <a name="privacy-policy-link"></a>Gizlilik ilkesi bağlantısı

Kuruluşunuzun gizlilik ilkesinin Web adresini (URL) girin. Teklifinizin gizlilik yasaları ve yönetmeliklerle uyumlu olduğundan emin olun. Ayrıca, Web sitenizde geçerli bir gizlilik ilkesi de nakletmeniz gerekir.

### <a name="useful-links"></a>Yararlı bağlantılar

Teklifiniz hakkında ek çevrimiçi belgeler sağlayın. Bir bağlantı eklemek için **bağlantı ekle**' yi seçin ve ardından aşağıdaki alanları doldurun:

- **Ad**: müşteriler, ayrıntılar sayfasında adı görür.
- **Bağlantı (URL)**: müşterilerin çevrimiçi belgenizi görüntülemesine imkan tanıyan bir bağlantı girin.

### <a name="customer-support-links"></a>Müşteri desteği bağlantıları

Müşterilerin destek ekibinize ulaşabilmesi için destek Web sitesini sağlayın.

- Azure genel destek web sitesi
- Azure Kamu destek web sitesi

### <a name="partner-support-contact"></a>İş ortağı destek kişisi

Müşterileriniz bir destek bileti açtıklarında kullanılacak Microsoft iş ortakları için iletişim bilgilerini sağlayın. Bu bilgiler Azure Marketi 'nde listelenmez.

- Name
- E-posta
- Telefon

### <a name="engineering-contact"></a>Mühendislik iletişim

Microsoft 'un, Teklifinizle ilgili sorunlar olduğunda, sertifika sorunları da dahil olmak üzere iletişim bilgilerini sağlayın. Bu bilgiler Azure Marketi 'nde listelenmez.

- Name
- E-posta
- Telefon

### <a name="azure-marketplace-media"></a>Azure Market medyası

Teklifinizle birlikte kullanılacak logo ve görüntüler sağlayın. Tüm görüntülerin PNG biçiminde olması gerekir. Bulanık görüntüler gönderiminizin reddedilmesine neden olur.

>[!NOTE]
>Dosya yükleme sorununuz varsa, yerel ağınızın https://upload.xboxlive.com Iş Ortağı Merkezi tarafından kullanılan hizmeti engellemediğinden emin olun.

#### <a name="azure-marketplace-logos"></a>Azure Market logoları

Aşağıdaki dört resim boyutlarıyla teklifinizin logosunun PNG dosyalarını sağlayın:

- **Küçük** (48 &times; 48 piksel)
- **Orta** (90 &times; 90 piksel)
- **Büyük** (216 &times; 216 piksel)
- **Geniş** (255 &times; 115 piksel)

Dört logo de gereklidir ve çeşitli Azure Market listelerinde görüntülenir.

#### <a name="screenshots"></a>Ekran görüntüleri

Teklifinizin nasıl çalıştığını gösteren en fazla beş ekran görüntüsü ekleyin. Her ekran görüntüsü &times; , boyut ve PNG biçiminde 1280 720 piksel olmalıdır. Her ekran görüntüsü bir başlık içermelidir.

#### <a name="videos"></a>Videolar

Teklifinizi gösteren en fazla beş video ekleyin. Videoların bir dış video hizmetinde barındırılması gerekir. Videonun adını, Web adresini ve videonun küçük resim PNG görüntüsünü 1280 &times; 720 piksel olarak girin.

Diğer Market listeleme kaynakları için bkz. [Market teklif listeleri Için en iyi uygulamalar](../gtm-offer-listing-best-practices.md).

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="preview"></a>Önizleme

**Önizleme** sekmesini seçin ve daha geniş ticari Market hedef kitlesi için canlı yayımlamadan önce teklifinizi doğrulamak üzere sınırlı bir **Önizleme izleyiciyi** seçin.

> [!IMPORTANT]
> **Önizleme** bölmesinde teklifinizi denetledikten sonra, teklifinizi ticari Market genel hedef kitlesi için yayımlamak üzere **canlı ol** ' u seçin.

Önizleme hedef kitlesi, Azure abonelik KIMLIĞI GUID 'Leri ve her biri için isteğe bağlı bir açıklama ile tanımlanır. Bu alanlardan hiçbiri müşteriler tarafından görülenebilir. Azure abonelik KIMLIĞINIZI Azure portal **abonelikler** sayfasında bulabilirsiniz.

Tek tek (10 kimliğe kadar) veya bir CSV dosyası (100 kimliğe kadar) karşıya yükleyerek en az bir Azure abonelik KIMLIĞI ekleyin. Bu abonelik kimliklerini ekleyerek teklifinizin canlı olarak yayınlanmadan önce ne kadar önizleyebilir olduğunu tanımlarsınız. Teklifiniz zaten canlı ise, teklifinizdeki değişiklikleri veya güncelleştirmeleri test etmek için bir önizleme hedef kitlesi tanımlayabilirsiniz.

> [!NOTE]
> Önizleme hedef kitlesi, özel bir hedef kitleye göre farklılık gösterir. Önizleme hedef kitlesi, Azure Marketi 'nde canlı olarak yayımlanmadan _önce_ teklifinizi erişebilir. Önizleme hedef kitlesi, teklifiniz Azure Marketi 'ne tamamen yayımlandıktan sonra yalnızca özel bir hedef kitle tarafından kullanılabilecek olanlar da dahil olmak üzere tüm planları görebilir ve doğrulayabilir. Özel bir hedef kitle (plan **fiyatlandırma ve kullanılabilirlik** bölmesinde tanımlanan) belirli bir plana özel erişim sağlar.

Sonraki bölüme geçmeden önce **Taslağı kaydet** ' i seçin.

## <a name="plan-overview"></a>Plana genel bakış

Iş Ortağı Merkezi 'nde aynı teklif dahilinde çeşitli plan seçenekleri sağlayabilirsiniz. Bir teklif en az bir plan (daha önce SKU olarak adlandırılır) gerektirir; Bu, Monaya getirme seyirci kitlesi, Azure bölgesi, özellikleri veya VM görüntülerine göre farklılık gösterebilir.

Planlarınızı oluşturduktan sonra, görüntülenecek **plana genel bakış** sekmesini seçin:

- Plan adları
- Lisans modelleri
- Hedef kitle (genel veya özel)
- Geçerli yayımlama durumu
- Kullanılabilir eylemler

**Plana genel bakış** bölmesinde bulunan eylemlere, planınızın geçerli durumuna bağlı olarak değişiklik gösterir.

- Plan durumu bir taslak ise, **taslağı Sil**' i seçin.
- Plan durumu canlı olarak yayınlanıyorsa, **satışı durdur** veya **özel izleyiciyi Eşitle**seçeneğini belirleyin.

### <a name="create-a-new-plan"></a>Yeni bir plan oluşturun

En üstte **Yeni plan oluştur** ' u seçin. **Yeni plan** iletişim kutusu görüntülenir.

**Plan kimliği** kutusunda, bu teklifte her plan için benzersiz BIR plan kimliği oluşturun. Bu KIMLIK, ürün web adresinde müşterilere görünür olacaktır. Yalnızca küçük harf ve rakam, tire veya alt çizgi ve en fazla 50 karakter kullanın.

> [!NOTE]
> Plan KIMLIĞI, **Oluştur**' u seçtikten sonra değiştirilemez.

**Plan adı** kutusuna bu plan için bir ad girin. Müşteriler teklifiniz dahilinde hangi planın seçeceğinize karar verirken bu adı görür. Planlar arasındaki farkları açıkça gösteren benzersiz bir ad oluşturun. Örneğin, *Kullandıkça Öde*, *KLG*, *Gelişmiş*ve *Kurumsal* planlar ile **Windows Server** girebilirsiniz.

**Oluştur**’u seçin.

### <a name="plan-setup"></a>Planı ayarla

Plan türü için üst düzey yapılandırmayı ayarlayın, başka bir plandan bir teknik yapılandırmayı yeniden kullanıp kullanmayacağını belirtin ve planın kullanılabilir olması gereken Azure bölgelerini belirleyin. Burada yaptığınız seçimler, aynı plan için diğer bölmelerde hangi alanların görüntülendiğini belirlemektir.

#### <a name="reuse-a-technical-configuration"></a>Teknik yapılandırmayı yeniden kullanma

Aynı türde birden fazla planınız varsa ve paketler aralarında aynıysa, **Bu planı başka bir plandaki teknik yapılandırmayı yeniden kullanır**' ı seçebilirsiniz. Bu seçenek, bu teklif için aynı türdeki diğer planlardan birini seçmenizi sağlar ve teknik yapılandırmasını yeniden kullanmanıza olanak tanır.

> [!NOTE]
> Teknik yapılandırmayı başka bir plandan yeniden kullandığınızda, tüm **Teknik yapılandırma** sekmesi bu plandan kaybolur. Daha sonra yaptığınız tüm güncelleştirmeler dahil olmak üzere diğer plandaki teknik yapılandırma ayrıntıları, bu plan için de kullanılacaktır. Plan yayımlandıktan sonra bu ayar değiştirilemez.

#### <a name="azure-regions"></a>Azure bölgeleri

Planınız en az bir Azure bölgesinde kullanılabilir duruma getirilmelidir.

Planınızı, ticari Market tümleştirmesi olan tüm Azure genel bölgelerindeki müşterilerin kullanımına sunmak için **Azure genel** seçeneğini belirleyin. Daha fazla bilgi için bkz. [coğrafi kullanılabilirlik ve para birimi desteği](../marketplace-geo-availability-currencies.md).

Planınızı [Azure Kamu](../../azure-government/documentation-government-welcome.md) bölgesinde kullanılabilir hale getirmek Için **Azure Kamu** seçeneğini belirleyin. Bu bölge, ABD Federal, eyalet, yerel veya üç aylık varlıklardan müşterilere denetimli erişim sağlar ve bunların yanı sıra onlara hizmeti sunmaya uygun olan iş ortakları için de çalışır. Yayımcı olarak tüm uyumluluk denetimleri, güvenlik ölçüleri ve en iyi uygulamalardan sorumludur. Azure Kamu fiziksel olarak yalıtılmış veri merkezleri ve ağlar (yalnızca ABD 'de bulunur) kullanır.

[Azure Kamu](../../azure-government/documentation-government-manage-marketplace-partners.md)'da yayımlamadan önce, belirli uç noktalar farklı olabileceğinden planınızı ortamda test edin ve doğrulayın. Planınızı ayarlamak ve test etmek için [Microsoft Azure Kamu deneme](https://azure.microsoft.com/global-infrastructure/government/request/) sayfasından bir deneme hesabı isteyin.

> [!NOTE]
> Planınız yayımlandıktan ve belirli bir Azure bölgesinde kullanılabilir olduktan sonra bu bölgeyi kaldıramazsınız.

#### <a name="azure-government-certifications"></a>Azure Kamu sertifikaları

Bu seçenek yalnızca, önceki bölümde yer alan Azure bölgesi olarak **Azure Kamu** 'yu seçtiyseniz görünür.

Azure Kamu Hizmetleri, belirli kamu düzenlemelerine ve gereksinimlerine tabi olan verileri işler. Örneğin, Fedrampa, NıST 800,171 (DIB), ıTAR, ıRS 1075, DoD L4 ve CJıS. Bu programlara yönelik sertifikalarınıza yönelik olarak bir açıklama getirmek için, bunları tanımlayan 100 'e kadar bağlantı sağlayabilirsiniz. Bunlar, program üzerinde doğrudan listelemesine bağlantılar ya da kendi Web sitelerinizde uyumluluğun açıklamalarını bağlar olabilir. Bu bağlantılar yalnızca Azure Kamu müşterileri tarafından görülebilir.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

### <a name="plan-listing"></a>Planı listeleme

Bu bölümde planın listeleme ayrıntılarını yapılandırırsınız. Bu bölme, aynı teklifteki diğer planlardan farklı olabilen belirli bilgileri görüntüler.

#### <a name="plan-name"></a>Plan adı

Bu alan, oluşturduğunuz zaman planınızı verdiğiniz adla birlikte doldurulur. Bu ad, Azure Marketi 'nde bu planın başlığı olarak görünür. 100 karakterle sınırlıdır.

#### <a name="plan-summary"></a>Plan Özeti

Teklifinizi değil, planınızın kısa bir özetini sağlayın. Bu Özet 100 karakterle sınırlıdır.

#### <a name="plan-description"></a>Plan açıklaması

Bu yazılım planının benzersiz olduğunu ve teklifinizdeki planlar arasındaki farkları açıkla. Teklifi değil yalnızca planı açıklama. Plan açıklaması en fazla 2.000 karakter içerebilir.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

### <a name="pricing-and-availability"></a>Fiyatlandırma ve kullanılabilirlik

Bu bölmede şunları yapılandırırsınız:

- Bu planın kullanılabildiği pazarlar.
- Saat başına fiyat.
- Planın herkese veya yalnızca belirli müşterilere (özel bir hedef kitle) görünür hale yapılıp yapılmayacağını belirtir.

#### <a name="markets"></a>Pazar

Her plan en az bir pazarda kullanılabilir olmalıdır. Bu planın satın alma için kullanılabilir olması gereken her Pazar konumunun onay kutusunu seçin. (Bu pazarlardaki kullanıcılar teklifi ["plan kurulumu"](#plan-setup) bölümünde seçilen tüm Azure bölgelerine dağıtmaya devam edebilir.) **Havale edilen vergi** düğmesi, Microsoft remits Sales ve sizin adınıza vergi tarafından kullanılan ülkeyi/bölgeleri gösterir. Çin 'de yayımlama, *ücretsiz* veya *kendi lisansını getir* (KLG) planlarla sınırlıdır.

Planınız için ABD Doları (USD) para biriminde zaten fiyatlar ayarladıysanız ve başka bir pazar konumu eklerseniz, yeni pazar fiyatı geçerli döviz kurlarına göre hesaplanır. Yayımlamadan önce her bir pazar için fiyatı her zaman gözden geçirin. Değişikliklerinizi kaydettikten sonra **fiyatları dışarı aktar (xlsx)** seçeneğini belirleyerek fiyatlandırmanızı gözden geçirin.

Bir pazarı kaldırdığınızda, bu pazardan etkin dağıtımlar kullanan müşteriler Yeni dağıtımlar oluşturamaz veya mevcut dağıtımlarını ölçeklendiremez. Mevcut dağıtımlar etkilenmez.

#### <a name="pricing"></a>Fiyatlandırma

**Lisans modeli**için, bu plan için fiyatlandırmayı yapılandırmak üzere **Kullanım tabanlı aylık faturalandırılan plan** ' ı seçin veya müşterilerin bu planı mevcut lisansıyla kullanmasına izin vermek için **kendi lisansını getir** ' i seçin.

Kullanım tabanlı aylık faturalandırılan bir plan için aşağıdaki üç fiyatlandırma girişi seçeneğinden birini kullanın:

- **Çekirdek başına**: USD cinsinden çekirdek başına fiyatlandırma sağlar. Microsoft, çekirdek boyutu başına fiyatlandırmayı hesaplar ve geçerli döviz ücretini kullanarak yerel para birimlerine dönüştürür.
- **Çekirdek boyutu başına**: USD cinsinden çekirdek boyutu başına fiyatlandırma sağlar. Microsoft, fiyatlandırmayı hesaplar ve geçerli döviz ücretini kullanarak yerel para birimlerine dönüştürür.
- **Pazar ve çekirdek boyutu başına**: tüm pazarlar için her bir çekirdek boyutu için fiyatlandırma sağlayın. Fiyatları bir elektronik tablodan içeri aktarabilirsiniz.

> [!NOTE]
> Fiyatlandırma verilerinin dışa aktarılmasını sağlamak için fiyatlandırma değişikliklerini kaydedin. Planınızdaki bir pazar bedeli yayımlandıktan sonra, daha sonra değiştirilemez. Fiyatları yayımlamadan önce fiyatların doğru olduğundan emin olmak için, fiyatlandırma elektronik tablosunu dışarı aktarın ve her pazardaki fiyatları gözden geçirin.

#### <a name="free-trial"></a>Ücretsiz Deneme

Müşterilerinize bir aylık veya üç aylık *ücretsiz deneme sürümü* sunabilirsiniz.

#### <a name="visibility"></a>Görünürlük

Her planı herkese görünür olacak şekilde veya yalnızca önceden seçilmiş bir kitleye tasarlayabilirsiniz. Azure abonelik kimliklerini kullanarak bu kısıtlanmış hedef kitlelere üyelik atayın.

**Ortak**: planınız herkes tarafından görülebilir.

**Özel hedef kitle**: planınızı yalnızca önceden seçilmiş bir hedef kitle için görünür hale getirin. Özel bir plan olarak yayımlandıktan sonra, hedef kitleyi güncelleştirebilir veya ortak olarak değiştirebilirsiniz. Planı herkese açık hale geçirdikten sonra, genel olarak kalması gerekir. Özel bir plana geri değiştirilemez.

> [!NOTE]
> Özel veya kısıtlanmış bir hedef kitle, **Önizleme** bölmesinde tanımladığınız önizleme izleyicilerinizden farklıdır. Bir önizleme hedef kitlesi, Azure Market 'Te canlı olarak yayımlanmadan _önce_ teklifinizin erişimine açabilir. Özel hedef kitle seçimi yalnızca belirli bir plana uygulansa da, önizleme hedef kitlesi tüm özel ve genel planları doğrulama amaçlarıyla görüntüleyebilir.

**Kısıtlanmış hedef kitle (Azure abonelik kimlikleri)**: Azure abonelik kimliklerini kullanarak bu özel plana erişimi olacak hedef kitleyi atayın. İsteğe bağlı olarak, atadığınız her bir Azure abonelik KIMLIĞI için bir açıklama ekleyin. Bir CSV elektronik tablosu içeri aktarıyorsanız, en fazla 10 abonelik kimliğini el ile veya 20.000 kimlik olarak ekleyin. Azure abonelik kimlikleri GUID olarak temsil edilir ve tüm harflerin küçük harf olması gerekir.

>[!Note]
>Özel teklifler, bulut çözümü sağlayıcısı programı 'nın (CSP) satıcısı aracılığıyla oluşturulan Azure abonelikleri ile desteklenmez.

#### <a name="hide-a-plan"></a>Planı gizleme

Sanal makineniz, başka bir çözüm şablonu veya yönetilen uygulama aracılığıyla başvurulduğu zaman dolaylı olarak yalnızca dolaylı olarak kullanılacaksa, sanal makineyi yayımlamak için bu onay kutusunu işaretleyin, ancak doğrudan arama veya tarama yapmakta olabilecek müşterilerden gizleyin.

> [!NOTE]
> Gizli planlar önizleme bağlantılarını desteklemez.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

### <a name="technical-configuration"></a>Teknik yapılandırma

Bu planla ilişkili görüntüleri ve diğer teknik özellikleri sağlayın. Daha fazla bilgi için bkz. [Azure VM teknik varlığı oluşturma](create-azure-container-technical-assets.md).

> [!NOTE]
> Bu planı, **plan kurulum** sekmesindeki başka bir plandan paketleri yeniden kullanmak üzere yapılandırdıysanız **Teknik yapılandırma** sekmesi gösterilmez.

#### <a name="operating-system"></a>İşletim sistemi

**İşletim sistemi** bölmesinde şunları yapın:

- **İşletim sistemi ailesi**Için, **Windows** veya **Linux** işletim sistemini seçin.
- **Yayın** veya **satıcı**Için Windows sürümü veya Linux satıcısı ' nı seçin.
- **Işletim sistemi kolay adı**için kolay bir işletim sistemi adı girin. Bu ad, müşteriler tarafından görülebilir.

#### <a name="recommended-vm-sizes"></a>Önerilen VM boyutları

Azure Marketi 'nde görüntülenmek üzere en fazla altı önerilen sanal makine boyutu seçin.

#### <a name="open-ports"></a>Bağlantı noktalarını açma

Dağıtılmış bir sanal makinede ortak veya özel bağlantı noktalarını açın.

#### <a name="storage-option-for-deployment"></a>Dağıtım için depolama seçeneği

**Disk dağıtımı seçeneği**için, müşterilerinizin sanal makine için kullanabileceği disk dağıtımı türünü seçin. Microsoft dağıtımı yalnızca **yönetilen disk dağıtımıyla** sınırlandırmanızı önerir.

#### <a name="properties"></a>Özellikler

**Destek hızlandırılmış ağ iletişimi**IÇIN, sanal makinenizin [hızlandırılmış ağı](https://go.microsoft.com/fwlink/?linkid=2124513)destekleyip desteklemediğini seçin.

#### <a name="vm-images"></a>VM görüntüleri

Sanal makine görüntüleri için bir disk sürümü ve paylaşılan erişim imzası (SAS) URI 'SI sağlayın. Her VM görüntüsü için en fazla 16 veri diski ekleyin. Belirtilen bir gönderideki plan başına yalnızca bir yeni görüntü sürümü sağlayın. Bir görüntü yayımlandıktan sonra düzenleyemezsiniz, ancak silebilirsiniz. Bir sürümü silmek, hem yeni hem de mevcut kullanıcıların silinen sürümün yeni bir örneğini dağıtmalarını engeller.

- **Disk sürümü**: sağlaettiğiniz görüntünün sürümü.
- **SAS URI 'si**: IŞLETIM sistemi VHD 'Sini depoladığınız Azure Storage hesabınızdaki konum. SAS URI 'sini alma hakkında bilgi edinmek için bkz. [VM Görüntünüz için paylaşılan erişim imzası URI 'Si alma](get-sas-uri.md).
- Veri diski görüntüleri Ayrıca, Azure depolama hesaplarında depolanan VHD paylaşılan erişim imza URI 'leridir.
- Bir plana gönderim başına yalnızca bir resim ekleyin.

Kullandığınız işletim sisteminden bağımsız olarak, yalnızca çözüm için gereken en az sayıda veri diski ekleyin. Dağıtım sırasında, müşteriler bir görüntünün parçası olan diskleri kaldıramaz, ancak dağıtım sırasında veya sonrasında her zaman disk ekleyebilirler.

Devam etmeden önce **Taslağı kaydet** ' i seçin ve **plana genel bakış**'a geri dönün.

## <a name="resell-through-csps"></a>CSP 'Ler aracılığıyla yeniden satış

[Bulut çözümü sağlayıcısı (CSP)](https://azure.microsoft.com/offers/ms-azr-0145p/) programındaki iş ortakları tarafından kullanılabilir hale getirerek teklifinizin ulaşma durumunu genişletin. Kendi lisansını getir (KLG) planları otomatik olarak programa kabul edilir. KLG olmayan planlarınızı kabul etmek de tercih edebilirsiniz.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="test-drive"></a>Test Sürüşü

Müşterilerin teklifinizi, satın almadan önce sabit bir süre boyunca denemesini sağlayan bir tanıtım veya *test sürücüsü*ayarlayın. Müşterileriniz için bir tanıtım ortamı oluşturmak için bkz. [ticari Market 'Te sürücü tekliflerini test](test-drive.md)etme.

Bir sınama sürücüsünü etkinleştirmek için **teklif kurulum** bölmesindeki **bir test sürücüsünü etkinleştir** onay kutusunu seçin. Test sürücüsünü teklifinizden kaldırmak için onay kutusunu temizleyin.

Ek test sürücüsü kaynakları:

- [En iyi pazarlama deneyimleri](../what-is-test-drive.md)
- [En iyi teknik uygulamalar](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [Test sürücüsüne genel bakış](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) PDF dosyası (açılır pencere engelinizin kapalı olduğundan emin olun)

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="review-and-publish"></a>İnceleme ve yayımlama

Teklifin tüm gerekli bölümlerini tamamladıktan sonra, bunu İnceleme ve yayınlama için gönderebilirsiniz.

Sağ üst köşedeki **gözden geçir ve Yayımla**' yı seçin.

Bu bölmede şunları yapabilirsiniz:

- Teklifin her bir bölümü için tamamlanma durumunu görüntüleyin:

  - **Başlamadı**: Bölüm başlatılmamış ve tamamlanması gerekiyor.
  - **Tamamlanmamış**: bölümde düzeltilmesi gereken hatalar var veya daha fazla bilgi sağlamanız gerekiyor. Tamamlanmamış bilgileri güncelleştirme hakkında rehberlik için bu makalenin önceki bölümlerine bakın.
  - **Tamam**: Bölüm tamamlanmıştır ve hata yoktur. Teklifi gönderebilmeniz için teklifin tüm bölümlerinin tamamlanmış olması gerekir.
- Uygulamanızın doğru şekilde test edilmiş olduğundan emin olmak için sertifika ekibine **sertifika notları** sağlayın. Test yönergelerini ve takımın uygulamanızı anlamasına yardımcı olabilecek tüm ek notları dahil edin.

Teklifi yayımlamaya göndermek için **gözden geçir ve Yayımla**' yı seçin.

Microsoft, teklifin bir önizleme sürümünün incelenmek ve onaylanmak için kullanılabilir olduğunu size bildirmek için bir e-posta mesajı gönderir. Teklifinizi herkese yayımlamak için (veya özel bir tekliftir, özel bir hedef kitleye yayımlayın), Iş Ortağı Merkezi ' ne gidin, teklifinizin **genel bakış** sayfasına gidin ve ardından **Go-Live**' ı seçin. Sunduğumuz durum, yayımlama işlemi devam ederken sayfanın en üstünde görünür.

### <a name="errors-and-review-feedback"></a>Hatalar ve gözden geçirme geri bildirimi

Yayımlama işlemindeki **el ile doğrulama** adımı, teklifinizin ve ilişkili teknik varlıklarının kapsamlı bir incelemesini temsil eder. Bu işlem Teklifinizle ilgili hataları kapsadığından, sorunların ayrıntılarını veren bir sertifika raporu alacaksınız. Gerekli düzeltmeleri yapmanız ve teklifinizi yeniden yayımlamanız yeterlidir.

## <a name="offer-overview"></a>Teklifin genel bakış

**Teklifin genel bakış** sayfası, hem tamamlandı hem de devam eden, bu adımların, teklifinizi yayımlamak için gerekli olan ve her adımın tamamlanması gereken süre uzunluğu gibi görsel bir temsilini gösterir.

Bu sayfa, durumuna bağlı olarak teklifle çalışmanıza yardımcı olacak bağlantılar da içerir:

- Teklif bir taslak ise: [taslak teklifini sil](update-existing-offer.md#delete-a-draft-offer)
- Teklif canlı ise: [teklifi satmaya](update-existing-offer.md#stop-selling-an-offer-or-plan)
- Teklif önizlemededir: [Go-Live](publishing-status.md#publisher-approval)
- Yayımcı oturumu kapatma Işlemini tamamlamadıysanız: [yayımlamayı Iptal et](update-existing-offer.md#cancel-publishing)

## <a name="marketplace-examples"></a>Market örnekleri

Azure Marketi 'nde teklif bilgilerinin nasıl göründüğünü gösteren bir örnek aşağıda verilmiştir:

:::image type="content" source="media/example-azure-marketplace-virtual-machine-offer.png" alt-text="Bu teklifin Azure Marketi 'nde nasıl göründüğünü gösterir.":::

#### <a name="call-out-descriptions"></a>Çağrı açıklamaları

1. Büyük logo
2. Fiyat
3. Kategoriler
4. hüküm ve koşullar
5. Gizlilik ilkesi adresi (bağlantı)
6. Teklif adı
7. Açıklama
8. Yararlı bağlantılar
9. Ekran görüntüleri/videolar

<br>Azure Marketi arama sonuçlarında teklif bilgilerinin nasıl göründüğünü gösteren bir örnek aşağıda verilmiştir:

:::image type="content" source="media/example-azure-marketplace-virtual-machine-search-results.png" alt-text="Bu teklifin Azure Marketi arama sonuçlarında nasıl göründüğünü gösterir.":::

#### <a name="call-out-descriptions"></a>Çağrı açıklamaları

1. Küçük logo
2. Teklif adı
3. Arama sonuçları Özeti
4. Deneme

<br>Azure Market planı ayrıntılarına bir örnek aşağıda verilmiştir:

:::image type="content" source="media/example-azure-marketplace-virtual-machine-plan-details.png" alt-text="Azure Market planı ayrıntılarını gösterir.":::

#### <a name="call-out-descriptions"></a>Çağrı açıklamaları

1. Plan adı ve Özeti
2. VM boyutlarını öner
3. Fiyat planı

<br>Teklif bilgilerinin Azure portal nasıl göründüğünü gösteren bir örnek aşağıda verilmiştir:

:::image type="content" source="media/example-azure-portal-virtual-machine-offer.png" alt-text="Bu teklifin Azure portal nasıl göründüğünü gösterir.":::

#### <a name="call-out-descriptions"></a>Çağrı açıklamaları

1. Ad
2. Açıklama
3. Yararlı bağlantılar
4. Ekran görüntüleri/videolar

<br>Teklif bilgilerinin Azure portal arama sonuçlarında nasıl göründüğünü gösteren bir örnek aşağıda verilmiştir:

:::image type="content" source="media/example-azure-portal-virtual-machine-search-results.png" alt-text="Bu teklifin Azure portal arama sonuçlarında nasıl göründüğünü gösterir.":::

#### <a name="call-out-descriptions"></a>Çağrı açıklamaları

1. Küçük logo
2. Teklif adı
3. Arama sonuçları Özeti

<br>Azure portal planı ayrıntılarına bir örnek aşağıda verilmiştir:

:::image type="content" source="media/example-azure-portal-virtual-machine-plan-details.png" alt-text="Azure portal planı ayrıntılarını gösterir.":::

#### <a name="call-out-descriptions"></a>Çağrı açıklamaları

1. Plan adı
2. Plan açıklaması

## <a name="next-step"></a>Sonraki adım

- [Ticari Market 'te mevcut bir teklifi güncelleştirme](update-existing-offer.md)
