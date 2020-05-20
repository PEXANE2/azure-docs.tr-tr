---
title: Azure Marketi 'nde Azure sanal makine teklifi oluşturma
description: Azure Marketi 'nde gerekli SKU ile sanal makine teklifi oluşturmayı öğrenin.
author: emuench
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/19/2020
ms.openlocfilehash: 25b7a619f6d3e308de966ead5925133f6094d9c8
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83701085"
---
# <a name="create-an-azure-virtual-machine-offer-in-the-azure-marketplace"></a>Azure Marketi 'nde Azure sanal makine teklifi oluşturma

> [!IMPORTANT]
> Azure VM tekliflerinizin yönetimini, Bulut İş Ortağı Portalı 'ten Iş ortağı merkezine taşıdık. Tekliflerinizi geçirene kadar, tekliflerinizi yönetmek için lütfen Bulut İş Ortağı Portalı [Sanal Makine teklifi oluşturma](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/virtual-machine/cpp-create-offer) bölümündeki yönergeleri izlemeye devam edin.

Bu makalede, Azure [Market 'te](https://azuremarketplace.microsoft.com/)bir Azure sanal makine teklifinin nasıl oluşturulacağı ve yayımlanacağı açıklanır. Bir işletim sistemi, sanal sabit disk (VHD) ve en çok 16 veri diski içeren Windows ve Linux tabanlı sanal makinelere yöneliktir. Başlamadan önce, henüz yapmadıysanız [Iş Ortağı Merkezi 'nde bir ticari Market hesabı oluşturun](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account) . Hesabınızın ticari Market programına kayıtlı olduğundan emin olun.

## <a name="introduction"></a>Giriş

### <a name="publishing-benefits"></a>Yayımlama avantajları

Azure Marketi 'Nde yayımlama aşağıdaki avantajlara sahiptir:

- Microsoft markasını kullanarak şirketinizi yükseltin
- 100.000.000 Office 365 ve Dynamics 365 kullanıcıları ile Azure Marketi aracılığıyla 200.000 'ten daha fazla kuruluşa ulaşın
- Bu Pazar yerlerden yüksek kaliteli müşteri adayları alın
- Microsoft alanı ve Telesales ekipleri tarafından yükseltilen hizmetlerinizi alın

### <a name="before-you-begin"></a>Başlamadan önce

Henüz yapmadıysanız, [Sanal Makine teklifi yayımlama kılavuzunu](https://docs.microsoft.com/azure/marketplace/marketplace-virtual-machines) ve bu Azure sanal makine malzemesini gözden geçirin:

- Hızlı Başlangıç Kılavuzu
  - [Azure Hızlı Başlangıç şablonları](https://azure.microsoft.com/resources/templates/)
  - [GitHub Azure hızlı başlangıç şablonları](https://github.com/azure/azure-quickstart-templates)
- Öğreticiler
  - [Linux VM'leri](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
  - [Windows VM’leri](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-manage-vm)
- Örnekler
  - [Linux VM 'Ler için Azure CLı örnekleri](https://docs.microsoft.com/azure/virtual-machines/linux/cli-samples)
  - [Linux sanal makineleri için Azure PowerShell](https://docs.microsoft.com/azure/virtual-machines/linux/powershell-samples)
  - [Windows VM 'Leri için Azure CLı örnekleri](https://docs.microsoft.com/azure/virtual-machines/windows/cli-samples)
  - [Windows VM 'Leri için Azure PowerShell](https://docs.microsoft.com/azure/virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-quick)

### <a name="fundamentals-in-technical-knowledge"></a>Teknik Bilgi Bankası temelleri

Bu varlıkların tasarlanmasına, oluşturulmasına ve test edilmesine zaman alır ve hem Azure platformu hem de teklifi oluşturmak için kullanılan teknolojiler hakkında teknik bilgi gerektirir.

Mühendislik ekibiniz aşağıdaki Microsoft teknolojilerini anlamalıdır:

- [Azure hizmetlerini](https://azure.microsoft.com/services/) temel olarak anlama
- [Azure uygulamalarını tasarlama ve mimarın](https://azure.microsoft.com/solutions/architecture/)
- [Azure sanal makineler](https://azure.microsoft.com/services/virtual-machines/), [Azure depolama](https://azure.microsoft.com/services/?filter=storage#storage)ve [Azure ağı](https://azure.microsoft.com/services/?filter=networking#networking) ile çalışma hakkında bilgi

## <a name="create-a-new-offer"></a>Yeni teklif oluşturma

1. [Iş Ortağı Merkezi](https://partner.microsoft.com/dashboard/home)' nde oturum açın.
2. Sol gezinti menüsünde **ticari Market**  >  **genel bakış**' ı seçin.
3. Genel Bakış sayfasında **+ yeni teklif**  >  **Azure sanal makinesi**' ni seçin.

    ![Sol gezinti menüsünü gösterir.](./media/new-offer-azure-vm.png)

> [!NOTE]
> Bir teklif yayımlandıktan sonra, Iş Ortağı Merkezi 'nde yapılan düzenlemeler, teklif yeniden yayımlandıktan sonra yalnızca stoporonts 'de görüntülenir. Değişiklik yaptıktan sonra her zaman yeniden yayımlaytığınızdan emin olun.

## <a name="new-offer"></a>Yeni teklif

Bir **TEKLIF kimliği**girin. Bu, hesabınızdaki her teklif için benzersiz bir tanımlayıcıdır.

- Bu KIMLIK, Market teklifinin Web adresindeki müşterilere ve varsa Azure PowerShell ve Azure CLı 'ye yönelik olarak görünür.
- Yalnızca küçük harfleri ve rakamları kullanın. Kısa çizgi ve alt çizgi içerebilir, ancak boşluk içeremez ve 50 karakterle sınırlıdır. Örneğin, burada **Test-teklif-1** girerseniz, teklif Web adresi olacaktır `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1` .
- Teklif KIMLIĞI, **Oluştur**' u seçtikten sonra değiştirilemez.

Bir **teklif diğer adı**girin. Bu, Iş Ortağı Merkezi 'nde teklif için kullanılan addır.

- Bu ad Market 'te kullanılmıyor ve teklif adından ve müşterilere gösterilen diğer değerlerden farklı.

Teklifi oluşturmak için **Oluştur** ' u seçin ve devam edin.

## <a name="offer-setup"></a>Teklif kurulumu

### <a name="test-drive"></a>Sınama sürücüsü

Test sürücüsü, "satın almadan önce dene" seçeneği sunarak olası müşterilere teklifinizi göstermek için harika bir yoldur. bu sayede, artan dönüştürmeye ve yüksek oranda nitelikli müşteri adaylarının oluşturulmasına neden olur. [Sınama sürücüleri hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive).

Bir sınama sürücüsünü sabit bir süre etkinleştirmek için, **bir sınama sürücüsünü etkinleştir** onay kutusunu seçin. Test sürücüsünü teklifinizden kaldırmak için bu onay kutusunu temizleyin.

Ek test sürücüsü kaynakları:

- [En iyi teknik uygulamalar](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [En iyi pazarlama deneyimleri](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [Sınama sürücülerine genel bakış](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) PDF (açılır pencere engelleyicinizin kapalı olduğundan emin olun)

### <a name="lead-management"></a>Müşteri adayı yönetimi

Teklifinizi Iş Ortağı Merkezi ile ticari Market 'te yayımlarken, Müşteri Ilişkileri yönetimi (CRM) sisteminize bağlayın. Bu, bir kişi, ürününüzü ilgilendiren veya ürünü kullandığı anda müşteri iletişim bilgilerini almanızı sağlar. **Test sürücüsünü** etkinleştirebiliyorsanız (önceki bölüme bakın), isteğe bağlı olarak, bir CRM 'ye bağlanma gerekir.

1. Potansiyel müşterileri göndermemizi istediğiniz müşteri adayı hedefini seçin. İş Ortağı Merkezi aşağıdaki CRM sistemlerini destekler:
    - Müşteri katılımı için [Dynamics 365](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics)
    - [Marketo](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-marketo)
    - [Salesforce](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce)

    > [!NOTE]
    > CRM sisteminiz yukarıda listelenmiyorsa, müşteri adayı verilerini depolamak için [Azure tablosu](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table) veya [https uç noktasını](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-https) kullanın. Ardından verileri CRM sisteminize dışarı aktarın.

2. Teklifinizi Iş Ortağı Merkezi 'nde yayımlarken lider hedefine bağlayın.
3. Müşteri adayı hedefi bağlantısının düzgün şekilde yapılandırıldığını doğrulayın. Iş Ortağı Merkezi 'nde yayımladıktan sonra, bağlantıyı doğrulayacağız ve size bir test lideri göndereceğiz. Teklifi canlı olmadan önce önizlerken, teklifi önizleme ortamında dağıtmayı deneyerek lider bağlantınızı da test edebilirsiniz.
4. Müşteri adaylarını kaybetmemeniz için lider hedefle bağlantının güncelleştirildiğinden emin olun.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="properties"></a>Özellikler

Bu sayfa, teklifinizi Market 'te, uygulama sürümünüzde ve teklifinizi destekleyen yasal sözleşmelerde gruplandırmak için kullanılan kategorileri ve endüstrileri tanımlamanızı sağlar.

### <a name="categories"></a>Kategoriler

En az bir ve en fazla beş kategori seçin. Bu kategoriler, teklifinizi uygun Market arama alanlarında yerleştirmek için kullanılır. Teklif açıklamasında, teklifinizin bu kategorileri nasıl desteklediğini açıklayın. Sanal makine teklifleri, Azure Marketi 'ndeki **işlem** kategorisi altında görünür.

### <a name="legal"></a>Yasal Bildirim

Teklif için hüküm ve koşullar sağlamanız gerekir. İki seçeneğiniz vardır:

- Kendi hüküm ve koşullarınızı kullanın
- Microsoft ticari Market için standart sözleşmeyi kullanın

#### <a name="use-your-own-terms-and-conditions"></a>Kendi hüküm ve koşullarınızı kullanın

Kendi özel hüküm ve koşullarınızı sağlamak için **hüküm ve koşullar** kutusuna en fazla 10.000 karakter girin. Daha uzun bir açıklamaya ihtiyacınız varsa, hüküm ve koşullarınızın nerede bulunmalarından işaret eden tek bir Web adresi girin. Bu, müşterilere etkin bir bağlantı olarak görüntülenecektir.

Müşterilerinizin teklifinizi deneyebilmeleri için önce bu koşulları kabul etmesi gerekir.

#### <a name="use-the-standard-contract-for-the-microsoft-commercial-marketplace"></a>Microsoft ticari Market için standart sözleşmeyi kullanın

Müşteriler için satın alma sürecini basitleştirmek ve yazılım satıcılarının yasal karmaşıklığını azaltmak için, Microsoft ticari Market için standart bir sözleşme sunmaktadır. Yazılımınızı standart sözleşme kapsamında sunabileceğiniz müşterilerin yalnızca bir kez okuyup kabul etmesi ve özel hüküm ve koşullar oluşturmanız gerekmez.

Standart sözleşmeyi, **Microsoft 'un ticari Market Için standart sözleşmeyi kullan** onay kutusunu seçerek ve ardından açılır pencerede **kabul edin** (bunu görmek için yukarı kaydırmanız gerekebilir).

![Yeni teklif düğmesi ve danışmanlık hizmeti teklifi seçiliyken Iş Ortağı Merkezi 'ndeki genel bakış sayfasını gösterir.](media/use-standard-contract.png)

> [!NOTE]
> Ticari Market için standart sözleşmeyi kullanarak bir teklifi yayımladıktan sonra, kendi özel hüküm ve koşullarınızı kullanamazsınız. Çözümünüzü standart sözleşme kapsamında **ya** da kendi hüküm ve koşullarınızın altına sunun.

Standart Sözleşme hakkında daha fazla bilgi edinmek için bkz. Microsoft [ticari Market](https://docs.microsoft.com/azure/marketplace/standard-contract)Için standart sözleşme. [Standart sözleşmeyi](https://go.microsoft.com/fwlink/?linkid=2041178) bir PDF olarak indirebilirsiniz (açılır pencere engelleyicisinin kapalı olduğundan emin olun).

##### <a name="standard-contract-amendments"></a>Standart Sözleşme düzeltme açıklamaları

Standart anlaşma değişikliği, basitlik için standart sözleşme koşullarını seçmenizi ve ürün veya işletmenizin koşullarını oluşturmayı sağlar. Müşterilerin, Microsoft standart sözleşmesini gözden geçirmiş ve kabul etmiş olmaları durumunda yalnızca, sözleşmeyi gözden geçirmesi gerekir. İki çeşit değişiklik bulunur: evrensel ve özel.

**Evrensel** değişiklik: Bunlar, tüm müşteriler Için standart sözleşmeye evrensel olarak uygulanır. Satın alma akışındaki teklifin her müşterisi için gösterilir. Müşterilerin teklifinizi kullanabilmesi için önce standart sözleşme ve değişiklik koşullarını kabul etmesi gerekir. Teklif başına tek bir evrensel düzeltme sağlayabilirsiniz. Bu kutuya sınırsız sayıda karakter girebilirsiniz. Bu terimler, bulma ve satın alma akışında AppSource, Azure Marketi 'nde müşterilere ve/veya Azure portal görüntülenir.

**Özel değişiklik açıklamaları** : Bunlar, Azure kiracı kimlikleri aracılığıyla belirli müşterileri hedefleyen standart sözleşmeye yönelik özel adımlardır. Hedeflemek istediğiniz kiracıyı seçebilirsiniz. Yalnızca kiracının müşterilerine, teklifin satın alma akışındaki özel değişiklik koşulları sunulur. Müşterilerin teklifinizi kullanabilmesi için önce standart sözleşme ve değişiklik koşullarını kabul etmesi gerekir.

**Özel düzeltme koşulları Ekle ' ye (en fazla 10)** tıklayarak başlayın. Teklif başına en fazla on özel düzeltme terimi sağlayabilirsiniz.

- **Özel değişiklik koşulları** : özel değişiklik koşulları kutusunda kendi değişiklik koşullarınızı girin. Sınırsız sayıda karakter girebilirsiniz. Yalnızca bu özel şartlar için belirttiğiniz kiracı kimliklerinin müşterileri, Azure portal teklifin satın alma akışında görüntülenir.
- **Kiracı kimlikleri** (gerekli) – her bir özel değişiklik, en fazla 20 Kiracı kimliğini hedefleyebilir. Özel bir düzeltme eklerseniz, Azure 'da müşterinizin tanımlandığı en az bir kiracı KIMLIĞI sağlamalısınız. Müşteriniz, altında Azure 'da, sonra Özellikler ' i bulabilir. Dizin KIMLIĞI değeri kiracı KIMLIĞIDIR (örneğin, 50c464d3-4930-494c-963c-1e951d15360e). Ayrıca, [Microsoft Azure ve Office 365 KIRACı kimliği olan](https://www.whatismytenantid.com/)etki alanı adı Web adresini kullanarak KURULUŞUNUZUN Kiracı kimliğini de bulabilirsiniz.
- **Açıklama** (isteğe bağlı) – Kiracı kimliği için, değişiklik yaparken hedeflediğiniz müşteriyi belirlemenize yardımcı olan kolay bir açıklama sağlayın.

> [!NOTE]
> Bu iki tür değişiklik yığını birbirlerinin üzerine. Özel değişiklik ile hedeflenen müşteriler, satın alma sırasında standart sözleşmeye evrensel değişiklik de alır.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="offer-listing"></a>Teklif listesi

Bu sayfa, teklif adı, açıklama, bağlantılar ve kişiler gibi teklif ayrıntıları tanımlamanızı sağlar.

> [!NOTE]
> Teklif açıklaması (Açıklama, belgeler, ekran görüntüleri ve kullanım koşulları gibi) sunan teklif listesinin Ingilizce olması gerekmez, "Bu uygulama yalnızca [Ingilizce olmayan dilde] kullanılabilir". Ayrıca, teklif listeleme içeriğinde kullanılandan farklı bir dilde içerik sunmak için _yararlı bir bağlantı Web adresi_ sağlayabilirsiniz.

### <a name="marketplace-details"></a>Market ayrıntıları

#### <a name="name"></a>Name

Buraya girdiğiniz ad, müşteriler için teklif listelerinizin başlığı olarak gösterilir. Bu alan, teklifi oluştururken **teklif diğer adı** kutusuna girdiğiniz metin ile önceden doldurulur. Bu adı daha sonra değiştirebilirsiniz.

Ad:

- Trademarked olabilir (ve ticari marka ve telif hakkı sembolleri dahil edebilirsiniz)
- 50 karakterden uzun olamaz
- Emojıs dahil olamaz.

#### <a name="search-results-summary"></a>Arama sonuçları Özeti

Teklifinizin kısa bir açıklaması. Bu, 100 karakter uzunluğunda olabilir ve Market arama sonuçlarında kullanılır.

#### <a name="long-summary"></a>Uzun Özet

Teklifinizin daha uzun bir açıklamasını sağlayın. Bu, 256 karakter uzunluğunda olabilir ve Market arama sonuçlarında kullanılır.

#### <a name="description"></a>Açıklama

[!INCLUDE [Long description-1](./includes/long-description-1.md)]

[!INCLUDE [Long description-2](./includes/long-description-2.md)]

[!INCLUDE [Long description-3](./includes/long-description-3.md)]

#### <a name="privacy-policy-link"></a>Gizlilik ilkesi bağlantısı

Kuruluşunuzun gizlilik ilkesine Web adresini (URL) girin. Teklifinizin gizlilik yasaları ve yönetmeliklerle uyumlu olduğundan emin olun. Ayrıca, Web sitenizde geçerli bir gizlilik ilkesi de nakletmeniz gerekir.

### <a name="useful-links"></a>Yararlı bağlantılar

Teklifiniz hakkında ek çevrimiçi belgeler sağlayın. Bir bağlantı eklemek için **+ bağlantı ekle** ' yi seçin ve ardından aşağıdaki alanları doldurun:

- **Ad** – müşteriler, ayrıntılar sayfasında adı görür.
- **Bağlantı (URL)** – müşterilerin çevrimiçi belgenizi görüntülemesi için bir bağlantı girin.

### <a name="customer-support-links"></a>Müşteri desteği bağlantıları

Müşterilerin destek ekibinize ulaşabilmesi için destek Web sitesini sağlayın.

- Azure genel destek web sitesi
- Azure Kamu destek web sitesi

### <a name="partner-support-contact"></a>İş ortağı destek kişisi

Müşterileriniz bir destek bileti açtıklarında kullanılacak Microsoft iş ortakları için iletişim bilgilerini sağlayın. Bu, Market 'te listelenmeyecektir.

- Name
- E-posta
- Telefon

### <a name="engineering-contact"></a>Mühendislik iletişim

Microsoft 'un, Teklifinizle ilgili sorunlar olduğunda, sertifika sorunları da dahil olmak üzere iletişim bilgilerini sağlayın. Bu, Market 'te listelenmeyecektir.

- Name
- E-posta
- Telefon

### <a name="marketplace-media"></a>Market medyası

Teklifinizle birlikte kullanılacak logo ve görüntüler sağlayın. Tüm görüntülerin PNG biçiminde olması gerekir. Bulanık görüntüler gönderiminizin reddedilmesine neden olur.

>[!Note]
>Dosya yükleme sorununuz varsa, yerel ağınızın https://upload.xboxlive.com Iş Ortağı Merkezi tarafından kullanılan hizmeti engellemediğinden emin olun.

#### <a name="marketplace-logos"></a>Market logoları

Aşağıdaki dört piksel boyutunda teklifinizin logosunun PNG dosyalarını sağlayın:

- **Küçük** (48 x 48)
- **Orta** (90 x 90)
- **Büyük** (216 x 216)
- **Geniş** (255 x 115)

Dört logo gereklidir ve Market listesindeki farklı yerlerde kullanılır.

#### <a name="screenshots"></a>Ekran görüntüleri

Teklifinizin nasıl çalıştığını gösteren en fazla beş ekran görüntüsü ekleyin. Her ekran görüntüsü, boyut ve PNG biçiminde 1280 x 720 piksel olmalıdır. Her ekran görüntüsü bir başlık içermelidir.

#### <a name="videos"></a>Videolar

Teklifinizi gösteren en fazla beş video ekleyin. Bunların bir dış video hizmetinde barındırılması gerekir. Videonun adını, Web adresini ve videonun küçük resim PNG görüntüsünü 1280 x 720 piksel olarak girin.

Diğer Market listeleme kaynakları için bkz. [Market teklif listeleri Için en iyi uygulamalar](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices).

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="preview"></a>Önizleme

Önizleme sekmesinde, daha geniş Market hedef kitlesi için canlı yayımlamadan önce teklifinizi doğrulamak üzere sınırlı bir **Önizleme izleyiciyi** seçin.

> [!IMPORTANT]
> Teklifinizi önizlemede denetledikten sonra, teklifinizi ticari Market genel kitlelerine yayımlamak için **canlı ol** ' u seçin.

Önizleme hedef kitlesi, Azure abonelik KIMLIĞI GUID 'Leri ve her biri için isteğe bağlı bir açıklama ile tanımlanır. Bu alanlardan hiçbiri müşteriler tarafından görülenebilir. Azure abonelik KIMLIĞINIZI Azure portal içindeki **abonelikler** sayfasında bulabilirsiniz.

Tek tek (10 ' a kadar) veya bir CSV dosyası karşıya yükleyerek (100 'e kadar) en az bir Azure abonelik KIMLIĞI ekleyin. Bu abonelik kimliklerini ekleyerek teklifinizin canlı olarak yayınlanmadan önce ne kadar önizleyebilir olduğunu tanımlarsınız. Teklifiniz zaten canlı ise, teklifinizdeki değişiklikleri veya güncelleştirmeleri test etmek için bir önizleme izleyiciyi yine de tanımlayabilirsiniz.

> [!NOTE]
> Önizleme hedef kitlesi, özel bir hedef kitleye göre farklılık gösterir. Bir önizleme hedef kitlesi, Market 'te canlı olarak yayımlanmadan _önce_ teklifinizin erişimine erişebilir. Teklifinizin Market 'e tamamen yayımlandıktan sonra yalnızca özel bir hedef kitle için kullanılabilir olacağını da içeren tüm planları görebilir ve doğrulayabilir. Özel bir hedef kitle (plan **fiyatlandırması ve kullanılabilirliği** sekmesinde tanımlanmıştır) belirli bir plana özel erişim sağlar.

Sonraki bölüme geçmeden önce **Taslağı kaydet** ' i seçin, plana genel bakış ' a tıklayın.

## <a name="plan-overview"></a>Plana genel bakış

Iş Ortağı Merkezi 'nde aynı teklif içinde farklı plan seçenekleri sağlayabilirsiniz. Bu planlar daha önce SKU 'Lar olarak adlandırılmıştı. Bir teklif en az bir plan gerektirir ve bu da, Azure bölgeleri, özellikleri veya VM görüntüleri açısından farklılık gösterebilir.

Planlarınızı oluşturduktan sonra **plana genel bakış** sekmesi şunları gösterir:

- Plan adları
- Lisans modelleri
- Hedef kitle (genel veya özel)
- Geçerli yayımlama durumu
- Kullanılabilir eylemler

Plan genel görünümünde bulunan eylemlere, planınızın geçerli durumuna bağlı olarak değişiklik gösterir. Şunları içerir:

- **Taslağı Sil** – plan durumu taslak ise
- Planı **satmayı** veya **özel izleyiciyi eşitlemeyi** durdur – plan durumu canlı olarak yayınlanıyorsa

### <a name="create-new-plan"></a>Yeni plan oluştur

En üstte **+ Yeni plan oluştur** ' u seçin. **Yeni plan** iletişim kutusu görüntülenir.

**Plan kimliği** kutusunda, bu teklifte her plan için benzersiz BIR plan kimliği oluşturun. Bu KIMLIK, ürün web adresinde müşterilere görünür olacaktır. Yalnızca küçük harf ve rakam, tire veya alt çizgi ve en fazla 50 karakter kullanın.

> [!NOTE]
> Plan KIMLIĞI, **Oluştur**' u seçtikten sonra değiştirilemez.

**Plan adı** kutusuna bu plan için bir ad girin. Müşteriler, teklifiniz dahilinde hangi planın seçeceğinize karar verirken bu adı görür. Her planın farklarını açıkça gösteren benzersiz bir ad oluşturun. Örneğin, **Kullandıkça Öde**, **KLG**, **Gelişmiş**ve **Kurumsal**planlar ile **Windows Server** 'ı kullanabilirsiniz.

**Oluştur**’u seçin.

### <a name="plan-setup"></a>Planı ayarla

Plan türü için üst düzey yapılandırmayı, başka bir plandan teknik yapılandırmayı yeniden kullanıp kullanmadığını ve planın hangi Azure bölgelerinde kullanılabilir olacağını belirleyin. Burada yaptığınız seçimler, aynı plan için diğer sekmelerde görüntülenen alanları tespit edilir.

#### <a name="re-use-technical-configuration"></a>Teknik yapılandırmayı yeniden kullanma

Aynı türde birden fazla planınız varsa ve paketler aralarında aynıysa, **Bu planı başka bir plandaki teknik yapılandırmayı yeniden kullanır**' ı seçebilirsiniz. Bu seçenek, bu teklif için aynı türdeki diğer planlardan birini seçmenizi ve teknik yapılandırmasını yeniden kullanmanızı sağlar.

> [!NOTE]
> Teknik yapılandırmayı başka bir plandan yeniden kullandığınızda, tüm **Teknik yapılandırma** sekmesi bu plandan kaybolur. Daha sonra yaptığınız tüm güncelleştirmeler dahil olmak üzere diğer plandaki teknik yapılandırma ayrıntıları, bu plan için de kullanılacaktır. Bu plan yayımlandıktan sonra bu ayar değiştirilemez.

#### <a name="azure-regions"></a>Azure bölgeleri

Planınız en az bir Azure bölgesinde kullanılabilir duruma getirilmelidir.

Planınızı, ticari Market tümleştirmesi olan tüm Azure genel bölgelerindeki müşterilerin kullanımına sunmak için **Azure genel** seçeneğini belirleyin. Ayrıntılar için bkz. [coğrafi kullanılabilirlik ve para birimi desteği](https://docs.microsoft.com/azure/marketplace/marketplace-geo-availability-currencies).

Planınızı [Azure Kamu](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome) bölgesinde kullanılabilir hale getirmek Için **Azure Kamu** seçeneğini belirleyin. Bu bölge, ABD Federal, eyalet, yerel veya üç aylık varlıklardan müşterilere yönelik denetimli erişim sağlar ve bu da onlara sunmaya uygun iş ortakları sağlar. Yayımcı olarak tüm uyumluluk denetimleri, güvenlik ölçüleri ve en iyi uygulamalardan sorumludur. Azure Kamu fiziksel olarak yalıtılmış veri merkezleri ve ağlar (yalnızca ABD 'de bulunur) kullanır.

[Azure Kamu](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners)'da yayımlamadan önce, belirli uç noktalar farklı olabileceğinden planınızı ortamda test edin ve doğrulayın. Planınızı ayarlamak ve test etmek için [Microsoft Azure Kamu deneme](https://azure.microsoft.com/global-infrastructure/government/request/)sürümünden bir deneme hesabı isteyin.

> [!NOTE]
> Planınız yayımlandıktan ve belirli bir Azure bölgesinde kullanılabilir olduktan sonra bu bölgeyi kaldıramazsınız.

#### <a name="azure-government-certifications"></a>Azure Kamu sertifikaları

Bu seçenek yalnızca **Azure Kamu**'yu seçtiyseniz görünür.

Azure Kamu Hizmetleri, belirli kamu düzenlemelerine ve gereksinimlerine tabi olan verileri işler. Örneğin, Fedrampa, NıST 800,171 (DIB), ıTAR, ıRS 1075, DoD L4 ve CJıS. Bu programlara yönelik sertifikalarınıza yönelik olarak bir açıklama getirmek için, bunları tanımlayan 100 'e kadar bağlantı sağlayabilirsiniz. Bunlar, program üzerinde doğrudan listelemesine bağlantılar ya da kendi Web sitelerinizde uyumluluğun açıklamalarını bağlar olabilir. Bu bağlantılar yalnızca Azure Kamu müşterilerine görünür.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

### <a name="plan-listing"></a>Planı listeleme

Plana ilişkin liste ayrıntılarını yapılandırdığınız yerdir. Bu sekme, aynı teklifteki planlar arasında farklılık gösteren belirli bilgileri görüntüler.

#### <a name="plan-name"></a>Plan adı

Bu, planınızı oluşturduğunuz sırada verdiğiniz adla önceden doldurulmuştur. Bu ad Market 'te bu planın başlığı olarak görünür ve 100 karakterle sınırlıdır.

#### <a name="plan-summary"></a>Plan Özeti

Planınızın kısa bir özetini sağlayın (teklif değil). Bu Özet 100 karakterle sınırlıdır.

#### <a name="plan-description"></a>Plan açıklaması

Bu yazılım planının ne olduğunu ve teklifinizdeki planlar arasındaki farkları betimleyebilirsiniz. Teklifi değil, yalnızca plan. Plan açıklaması en fazla 2.000 karakter içerebilir.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

### <a name="pricing-and-availability"></a>Fiyatlandırma ve kullanılabilirlik

Bu sekmede, aşağıdakileri yapılandıracaksınız:

- Bu planın pazarları şu şekilde kullanılabilir olacaktır
- Saat başına fiyat
- Planın herkese veya yalnızca belirli müşterilere (özel bir hedef kitle) görünür hale yapılıp yapılmayacağını belirtir

#### <a name="markets"></a>Pazar

Her plan en az bir pazarda kullanılabilir olmalıdır. Bu planın satın alma için kullanılabilir olması gereken her Pazar konumunun onay kutusunu seçin (bu pazarlardaki kullanıcılar teklifi **[plan kurulumunda](#plan-setup)** seçilen tüm Azure bölgelerine dağıtmaya devam edebilir). **Havale edilen vergi** düğmesi, Microsoft remits Sales ve sizin adınıza kullanım vergisini gösteren ülkeleri gösterir. Çin 'de yayımlama, **ücretsiz** olan veya **kendi lisansınızı** getiren planlarla sınırlıdır (KLG).

Planınız için Birleşik Devletler dolar (USD) cinsinden zaten fiyatlar ayarladıysanız ve başka bir pazar konumu eklerseniz, yeni pazar fiyatı geçerli döviz kurlarına göre hesaplanır. Yayımlamadan önce her bir pazar için fiyatı her zaman gözden geçirin. Değişikliklerinizi kaydettikten sonra **fiyatları dışarı aktar (xlsx)** bağlantısını kullanarak fiyatları gözden geçirin.

Bir pazarı kaldırdığınızda, etkin dağıtımlar kullanılarak söz konusu pazardan müşteriler Yeni dağıtımlar oluşturamaz veya mevcut dağıtımlarını ölçeklendiremez. Mevcut dağıtımlar etkilenmeyecektir.

#### <a name="pricing"></a>Fiyatlandırma

**Lisans modeli** – bu plan için fiyatlandırmayı yapılandırmak üzere **Kullanım tabanlı aylık faturalandırılan planı** seçin veya müşterilerin bu planı mevcut lisansıyla kullanmasına izin vermek için **kendi lisansınızı getirin** .

Kullanım tabanlı aylık faturalandırılan bir plan için aşağıdaki üç fiyatlandırma girişi seçeneğinden birini kullanın:

- **Çekirdek başına** – Birleşik Devletler dolar (USD) cinsinden çekirdek başına fiyat sağlayın. Çekirdek boyutu başına fiyatlandırmayı hesaplayacaktır ve geçerli döviz ücretini kullanarak yerel para birimlerine dönüştürüyoruz.
- **Çekirdek boyutu başına** – USD cinsinden çekirdek boyutuna göre fiyat sağlayın. Geçerli döviz ücretini kullanarak fiyatları yerel para birimlerine dönüştürüyoruz.
- **Pazar ve çekirdek boyutu başına** – tüm pazarlar için her bir çekirdek boyutu için fiyatlar sağlayın. Bir elektronik tablodan fiyatları içeri aktarabilirsiniz.

> [!NOTE]
> Fiyatlandırma verilerinin dışa aktarılmasını sağlamak için fiyatlandırma değişikliklerini kaydedin. Planınızdaki bir pazar bedeli yayımlandıktan sonra, daha sonra değiştirilemez. Fiyatlandırma elektronik tablosunu dışarı aktarıp her bir pazardaki fiyatı inceleyerek, bu fiyatların yayınlanmadan önce olduğundan emin olun.

#### <a name="free-trial"></a>Ücretsiz deneme sürümü

Müşterilerinize bir aylık veya üç aylık ücretsiz deneme sürümü sunabilirsiniz.

#### <a name="visibility"></a>Görünürlük

Her planı herkese görünür olacak şekilde veya yalnızca önceden seçilmiş bir hedef kitleye tasarlayabilirsiniz. Azure abonelik kimliklerini kullanarak bu kısıtlanmış hedef kitleye üyelik atayın.

**Genel** – planınız herkes tarafından görülebilir.

**Özel hedef kitle** – planınızı yalnızca önceden seçilmiş bir hedef kitle için görünür hale getirin. Özel bir plan olarak yayımlandıktan sonra, hedef kitleyi güncelleştirebilir veya ortak olarak değiştirebilirsiniz. Planı herkese açık hale geçirdikten sonra, genel olarak kalması gerekir; Bunu özel olarak değiştiremezsiniz.

**Kısıtlanmış hedef kitle (Azure abonelik kimlikleri)** – Azure abonelik kimliklerini kullanarak bu özel plana erişimi olacak hedef kitleyi atayın. İsteğe bağlı olarak, atadığınız her bir Azure abonelik KIMLIĞI için bir açıklama ekleyin. 10 adede kadar abonelik kimliği el ile veya bir CSV elektronik tablosu içeri aktarıldıysanız 20.000 ekleyin. Azure abonelik kimlikleri, GUID 'Ler olarak temsil edilir ve harflerin küçük harf olması gerekir.

> [!NOTE]
> Özel veya kısıtlanmış bir hedef kitle, **Önizleme** sekmesinde tanımladığınız önizleme izleyicilerinizden farklıdır. Bir önizleme hedef kitlesi, Market 'te canlı olarak yayımlanmadan _önce_ teklifinizin erişimine açabilir. Özel hedef kitle seçimi yalnızca belirli bir plana uygulansa da, önizleme hedef kitlesi doğrulama amacıyla tüm planları (özel veya değil) görüntüleyebilir.

#### <a name="hide-plan"></a>Planı gizle

Sanal makineniz, başka bir çözüm şablonu veya yönetilen uygulama aracılığıyla başvuruluyorsa yalnızca dolaylı olarak kullanılması için bu kutuyu seçerek sanal makinenizi yayımlayın, ancak doğrudan arama yapın ve BT 'ye göz atarak bu kutuyu gizleyin.

> [!NOTE]
> Gizli planlar önizleme bağlantılarını desteklemez.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

### <a name="technical-configuration"></a>Teknik yapılandırma

Bu planla ilişkili görüntüleri ve diğer teknik özellikleri sağlayın. Ayrıntılar için bkz. [Azure VM teknik varlığı oluşturma](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-azure-container-technical-assets).

> [!NOTE]
> Bu plan, bu planı, **plan kurulum** sekmesindeki başka bir plandan paketleri yeniden kullanmak üzere yapılandırdıysanız gösterilmez.

#### <a name="operating-system"></a>İşletim Sistemi

- **İşletim sistemi ailesi** – **Windows** veya **Linux** işletim sisteminden seçim yapın
- **Yayın** veya **satıcı** – Windows sürümü veya Linux satıcıyı seçin
- **OS kolay adı** – kolay bir işletim sistemi adı seçin. Bu ad müşteriler tarafından görülebilir

#### <a name="recommended-vm-sizes"></a>Önerilen VM boyutları

Azure Marketi 'nde görüntülenmek üzere en fazla altı önerilen sanal makine boyutu seçin.

#### <a name="open-ports"></a>Bağlantı noktalarını açma

Dağıtılmış bir sanal makinede ortak veya özel bağlantı noktalarını açın.

#### <a name="storage-option-for-deployment"></a>Dağıtım için depolama seçeneği

**Disk dağıtım seçeneği** – kullanıcılarınızın sanal makineyi kullanırken ne tür bir disk dağıtımı yapabileceğini seçin. Microsoft dağıtımı yalnızca yönetilen disk dağıtımıyla sınırlandırmanızı önerir.

#### <a name="properties"></a>Özellikler

**Hızlandırılmış ağ desteği** – sanal makinenizin [hızlandırılmış ağı](https://go.microsoft.com/fwlink/?linkid=2124513)destekleyip desteklemediğini seçin.

#### <a name="vm-images"></a>VM Görüntüleri

Sanal makine görüntüleri için bir disk sürümü ve SAS URI 'SI sağlayın. Her VM görüntüsü için en fazla 16 veri diski ekleyin. Belirli bir gönderim için plan başına yalnızca bir yeni görüntü sürümü sağlayın. Bir görüntü yayımlandıktan sonra düzenleyemezsiniz, ancak silebilirsiniz. Bir sürümü silmek, yeni ve mevcut kullanıcıların silinen sürümün yeni bir örneğini dağıtmasını engeller.

- **Disk sürümü** , sağlaettiğiniz görüntünün sürümüdür.
- **SAS URI 'si** , IŞLETIM sistemi VHD 'Sini depoladığınız Azure Storage konumudur.
- Veri diski görüntüleri de Azure depolamada depolanan VHD SAS URI 'leridir.
- Bir plana gönderim başına yalnızca bir resim ekleyin.

Kullandığınız işletim sisteminden bağımsız olarak yalnızca çözüm için gereken en az sayıda veri diski ekleyin. Müşteriler, dağıtım sırasında bir görüntünün parçası olan diskleri kaldıramaz, ancak dağıtım sırasında veya sonrasında her zaman disk ekleyebilirler.

Devam etmeden önce **Taslağı kaydet** ' i seçin ve **plana genel bakış**'a dönün.

## <a name="resell-through-csps"></a>CSP 'Ler aracılığıyla yeniden satış

[Bulut çözüm sağlayıcıları](https://azure.microsoft.com/offers/ms-azr-0145p/) (CSP) programındaki iş ortakları için kullanılabilir hale getirerek teklifinizin erişimini genişletin. Kendi lisansını getir (KLG) planları otomatik olarak kabul edilir; KLG olmayan planlarınızı kabul edebilirsiniz.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="test-drive"></a>Sınama sürücüsü

Müşterilerin teklifinizi satın almadan önce denemesini sağlayan bir tanıtım (test sürücüsü) ayarlayın. Müşterilerin teklifinizi sabit bir süre için denemesini sağlayan bir tanıtım ortamı oluşturmak için, bkz. [ticari Market 'te teklifinizin test](https://docs.microsoft.com/azure/marketplace/partner-center-portal/test-drive)etme.

Bir sınama sürücüsünü etkinleştirmek için [teklif kurulumu](#test-drive) sekmesinde bir test sürücüsünü etkinleştir onay kutusunu işaretleyin. Test sürücüsünü teklifinizden kaldırmak için bu onay kutusunu temizleyin.

Ek test sürücüsü kaynakları:

- [En iyi pazarlama deneyimleri](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [En iyi teknik uygulamalar](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [Genel bakış](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) (PDF; açılır pencere engelleyicinizin kapalı olduğundan emin olun)

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="review-and-publish"></a>İnceleme ve yayımlama

Teklifin tüm gerekli bölümlerini tamamladıktan sonra, bunu gözden geçirmek ve yayımlamak için gönderebilirsiniz.

Portalın sağ üst köşesinde, **gözden geçir ve Yayımla**' yı seçin.

Bu sayfada şunları yapabilirsiniz:

- Teklifin her bölümü için tamamlanma durumuna bakın.
  - **Başlatılmamış** – bölüm başlatılmamış ve tamamlanması gerekiyor.
  - **Tamamlanmamış** – bölümde düzeltilmesi gereken hatalar var veya daha fazla bilgi sağlamanız gerekiyor. Bu bölümü güncelleştirme hakkında yönergeler için bu belgenin önceki bölümlerine bakın.
  - **Tamam** – bölüm tamamlanmıştır ve hata yoktur. Teklifi gönderebilmeniz için teklifin tüm bölümlerinin tamamlanmış olması gerekir.
- **Sertifika notları** – uygulamanızın doğru şekilde test edilmesini sağlamak için sertifika ekibine test yönergeleri sağlar. Uygulamanızı anlamak için yardımcı olan tüm ek notları sağlayın.

Teklifi yayımlamaya göndermek için **gözden geçir ve Yayımla**' yı seçin.

Teklifin bir önizleme sürümünün gözden geçirilmesi ve onaylanmak üzere kullanılabilir olduğunu size bildirmek için size bir e-posta göndereceğiz. Teklifinizi herkese (veya özel bir teklif, özel bir hedef kitleye) yayımlamak için Iş Ortağı Merkezi ' ne gidin, teklifinizin **genel bakış** sayfasını bulun ve **Go-Live**' ı seçin. Sunduğumuz durum, yayımlama işlemi devam ederken sayfanın üst kısmında görünür.

### <a name="errors-and-review-feedback"></a>Hatalar ve gözden geçirme geri bildirimi

Yayımlama işlemindeki **el ile doğrulama** adımı, teklifinizin ve ilişkili teknik varlıklarının kapsamlı bir incelemesini temsil eder. Bu işlem Teklifinizle ilgili hataları kapsadığından, sorunların ayrıntılarını veren bir sertifika raporu alacaksınız. Gerekli düzeltmeleri yapmanız ve teklifinizi yeniden yayımlamanız yeterlidir.

## <a name="offer-overview"></a>Teklifin genel bakış

**Teklif genel bakış** sayfasında, bu teklifi yayımlamak için gereken adımların görsel temsili (hem tamamlandı hem de devam ediyor) ve her adımın tamamlanması için gereken süre gösterilir.

Bu sayfa, yaptığınız seçime bağlı olarak bu teklif üzerinde işlem gerçekleştirme bağlantılarını içerir. Örnek:

- Teklif bir taslak- [silme taslağı tekliftir](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#delete-a-draft-offer)
- Teklif canlı ise, [teklifi satmaya](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#stop-selling-an-offer-or-plan)
- Teklif önizleme sürümündedir- [canlı](https://docs.microsoft.com/azure/marketplace/partner-center-portal/publishing-status#publisher-approval)
- Yayımcı oturumu kapatma Işlemini tamamlamadıysanız [yayımlamayı Iptal edin](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#cancel-publishing)

## <a name="marketplace-examples"></a>Market örnekleri

Bu örneklerde, teklifin Azure Marketi 'nde nasıl göründüğü gösterilmektedir.

### <a name="azure-marketplace-offer-details"></a>Azure Market teklif ayrıntıları

![Azure Marketi teklif ayrıntıları ekranı örneği](media/avm-create1.png)

### <a name="azure-marketplace-search-results"></a>Azure Marketi arama sonuçları

![Azure Marketi arama ayrıntıları ekranı örneği](media/avm-create2.png)

### <a name="azure-marketplace-plan-details"></a>Azure Market planı Ayrıntıları

![Azure Market plan ayrıntıları ekranı örneği](media/avm-create3.png)

### <a name="azure-portal-offer-details"></a>Azure portal teklif ayrıntıları

![Azure portal teklif ayrıntıları ekranı örneği](media/avm-create4.png)

### <a name="azure-portal-search-results"></a>Azure portal arama sonuçları

![Azure portal arama sonuçları ekranı örneği](media/avm-create5.png)

### <a name="azure-portal-plan-details"></a>Azure portal planı Ayrıntıları

![Azure portal planı ayrıntıları ekranı örneği](media/avm-create6.png)

## <a name="next-step"></a>Sonraki adım

- [Ticari Market 'te mevcut bir teklifi güncelleştirme](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer)
