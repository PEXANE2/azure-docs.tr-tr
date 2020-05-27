---
title: Azure Uygulama teklifi oluşturma-Microsoft ticari Market
description: Iş Ortağı Merkezi 'nde ticari Market portalında yeni bir Azure uygulaması teklifi oluşturmaya yönelik adımları ve konuları öğrenin. Azure uygulama teklifinizi Azure Market 'te veya bulut çözümü sağlayıcısı (CSP) programı aracılığıyla listeleyebilir veya satın alabilirsiniz.
author: qianw211
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/19/2020
ms.openlocfilehash: ace85727680ecf6d62860ac2239a8c0b68ae6e0e
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83848796"
---
# <a name="create-an-azure-application-offer"></a>Azure uygulama teklifi oluşturma

Bu makalede, ticari Market 'te yeni bir Azure uygulaması teklifi oluşturmaya yönelik adımlar ve noktalar açıklanmaktadır. Yeni bir Azure Uygulama teklifi oluşturmadan önce bu kavramları tanımanız gerekir.

Yeni bir Azure Uygulama teklifi yayımlayabilmeniz için önce [Iş Ortağı Merkezi 'nde bir ticari Market hesabı oluşturun](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account) ve hesabınızın ticari Market programına kayıtlı olduğundan emin olun.

## <a name="before-you-begin"></a>Başlamadan önce

Azure Uygulama tekliflerini tasarlama, derleme ve test etme, hem Azure platformu hem de teklifi oluşturmak için kullanılan teknolojiler hakkında teknik bilgi gerektirir. Mühendislik ekibiniz aşağıdaki Microsoft teknolojileri hakkında bilgi sahibi olmalıdır:

* [Azure hizmetlerinin](https://azure.microsoft.com/services/)temel olarak anlaşılmasıdır.
* [Azure uygulamaları tasarlama ve mimari](https://azure.microsoft.com/solutions/architecture/)oluşturma.
* [Azure sanal makineleri](https://azure.microsoft.com/services/virtual-machines/), [Azure depolama](https://azure.microsoft.com/services/?filter=storage#storage)ve [Azure ağ](https://azure.microsoft.com/services/?filter=networking#networking)ile çalışma hakkında bilgi.
* [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)çalışma bilgileri.
* [JSON](https://www.json.org/)hakkında çalışma bilgisi.

### <a name="technical-documentation-and-resources"></a>Teknik belgeler ve kaynaklar

Ticari Market için Azure uygulama teklifinizi hazırlarken aşağıdaki kaynakları gözden geçirin.

* [Azure Resource Manager şablonlarını anlama](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)

* Hızlı Başlangıçlar:

    * [Azure Hızlı Başlangıç şablonları](https://azure.microsoft.com/documentation/templates/)
    * [GitHub Azure hızlı başlangıç şablonları](https://github.com/azure/azure-quickstart-templates)
    * [Uygulama tanımını yayımlama](https://docs.microsoft.com/azure/managed-applications/publish-service-catalog-app)
    * [Hizmet kataloğu uygulaması dağıtma](https://docs.microsoft.com/azure/managed-applications/deploy-service-catalog-quickstart)

* Öğreticiler:

    * [Tanım dosyaları oluşturma](https://docs.microsoft.com/azure/managed-applications/publish-service-catalog-app)
    * [Market uygulaması yayımlama](https://docs.microsoft.com/azure/managed-applications/publish-marketplace-app)

* Lerinizi

    * [Azure CLI](https://docs.microsoft.com/azure/managed-applications/cli-samples)
    * [Azure PowerShell](https://docs.microsoft.com/azure/managed-applications/powershell-samples)
    * [Yönetilen uygulama çözümleri](https://docs.microsoft.com/azure/managed-applications/sample-projects)

[Azure Market 'e yönelik çözüm şablonları ve yönetilen uygulamalar için](https://channel9.msdn.com/Events/Build/2018/BRK3603) sunulan video, Azure Uygulama teklifi türüne kapsamlı bir giriş sağlar:

* Hangi teklif türleri kullanılabilir?
* Hangi teknik varlıkların gerekli olduğu;
* Azure Resource Manager şablonu yazma;
* Uygulama kullanıcı arabirimini geliştirme ve test etme;
* Uygulama teklifini yayımlama;
* Uygulama İnceleme işlemi.

### <a name="suggested-tools"></a>Önerilen araçlar

Azure uygulamanızın yönetilmesine yardımcı olmak için aşağıdaki komut dosyası ortamlarının birini veya her ikisini birden seçin:

* [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
* [Azure CLI](https://docs.microsoft.com/cli/azure)

Geliştirme ortamınıza aşağıdaki araçları eklemeniz önerilir:

* [Azure Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
* Aşağıdaki uzantılara sahip [Visual Studio Code](https://code.visualstudio.com/) :
    * Uzantı: [Azure Resource Manager araçları](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
    * Uzantı: [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
    * Uzantı: [prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

Mevcut araçları [Azure Geliştirici Araçları](https://azure.microsoft.com/tools/) sayfasında inceleyebilirsiniz. Ayrıca, Visual Studio kullanıyorsanız [Visual Studio Market](https://marketplace.visualstudio.com/).

## <a name="types-of-azure-application-plans"></a>Azure uygulama planlarının türleri

İki tür Azure uygulama planı vardır: çözüm şablonları ve yönetilen uygulamalar.

* **Çözüm şablonu** , Market 'te çözüm yayımlamanın ana yöntemlerinden biridir. Çözümünüz tek bir sanal makinenin (VM) ötesinde ek dağıtım ve yapılandırma Otomasyonu gerektirdiğinde bu plan türünü kullanın. Bir çözüm şablonuyla, karmaşık IaaS çözümleri sağlamak üzere VM 'Ler, ağ oluşturma ve depolama kaynakları dahil olmak üzere birden fazla kaynağın sağlanması otomatik hale getirebilirsiniz.  Çözüm şablonları oluşturma hakkında daha fazla bilgi için bkz. [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).

* **Yönetilen uygulama** , tek bir anahtar farklılığı ile çözüm şablonlarına benzerdir. Yönetilen bir uygulamada kaynaklar, uygulamanın yayımcısı tarafından yönetilen bir kaynak grubuna dağıtılır. Kaynak grubu, tüketicinin aboneliğinde mevcuttur ancak yayımcının kiracısındaki bir kimlik, kaynak grubuna erişime sahiptir. Yayımcı olarak, çözümün sürekli desteği için maliyeti belirtirsiniz. Yönetilen uygulamaları kullanarak, müşterilerinize tam olarak yönetilen, anahtar kullanan uygulamaları kolayca oluşturup sunun.  Yönetilen uygulamaların avantajları ve türleri hakkında daha fazla bilgi için bkz. [Azure yönetilen uygulamalarına genel bakış](https://docs.microsoft.com/azure/managed-applications/overview).

## <a name="technical-requirements"></a>Teknik gereksinimler

Tüm Azure uygulamaları, bir arşiv kök klasöründe en az iki dosya içerir `.zip` :

* [Maintemplate. JSON](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)adlı bir kaynak yöneticisi şablon dosyası.  Bu şablon, müşterinin Azure aboneliğine dağıtılacak kaynakları tanımlar.  Kaynak Yöneticisi şablonlarının örnekleri için bkz. [Azure hızlı başlangıç şablonları Galerisi](https://azure.microsoft.com/resources/templates/) veya ilgili [GitHub: Azure Resource Manager hızlı başlangıç şablonları](https://github.com/azure/azure-quickstart-templates) deposu.

* [Createuıdefinition. JSON](https://docs.microsoft.com/azure/managed-applications/create-uidefinition-overview)adlı Azure uygulama oluşturma deneyimi için bir kullanıcı arabirimi tanımı.  Kullanıcı arayüzünde tüketicilerin parametre değerleri sağlamasına olanak tanıyan öğeleri belirlersiniz.

Tüm yeni Azure Uygulama teklifleri bir [Azure iş ortağı müşteri kullanımı attributıon GUID 'i](https://docs.microsoft.com/azure/marketplace/azure-partner-customer-usage-attribution)içermelidir. 

Her uygulama planına ilişkin gereksinimleri yayımlama hakkında bilgi edinmek için bkz. [çözüm şablonu teklif yayımlama gereksinimleri](../marketplace-solution-templates.md) ve [yönetilen uygulama teklifi yayımlama gereksinimleri](../marketplace-managed-apps.md).

## <a name="create-a-new-offer"></a>Yeni teklif oluşturma

>[!NOTE]
>Bir teklif yayımlandıktan sonra, teklifi yeniden yayınlana kadar Iş Ortağı Merkezi 'nde bunun üzerinde yaptığınız düzenlemeler durdurulmaz. Değişiklikleri yaptıktan sonra teklifi her zaman yeniden yayımlaytığınızdan emin olun.

1. [Iş Ortağı Merkezi](https://partner.microsoft.com/dashboard/home)' nde oturum açın.

1. Sol taraftaki menüden **ticari Market**  >  **genel bakış**' ı seçin.

1. Genel Bakış sayfasında **+ yeni teklif**  >  **Azure uygulaması**' nı seçin.

    ![Sol gezinti menüsünü gösterir.](./media/new-offer-azure-app.png)

1. **Yeni teklif** sayfasında, BIR **teklif kimliği**girin. Bu, hesabınızdaki her teklif için benzersiz bir tanımlayıcıdır.

     * Bu KIMLIK, varsa Market teklifi ve Azure Resource Manager şablonları için Web adresinde müşteriler tarafından görülebilir.
     * Yalnızca küçük harfleri ve rakamları kullanın. Kısa çizgi ve alt çizgi içerebilir, ancak boşluk içeremez ve 50 karakterle sınırlıdır. Örneğin, **Test-teklif-1**girerseniz, teklif Web adresi olacaktır `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1` .
     * Teklif KIMLIĞI, **Oluştur**' u seçtikten sonra değiştirilemez.

1. Bir **teklif diğer adı**girin. Bu, Iş Ortağı Merkezi 'nde teklif için kullanılan addır.

     * Bu ad Market 'te kullanılmıyor ve teklif adından ve müşterilere gösterilen diğer değerlerden farklı.
     * Teklif diğer adı, **Oluştur**' u seçtikten sonra değiştirilemez.

1. Teklifi oluşturmak için **Oluştur** ' u seçin ve devam edin.

## <a name="offer-setup"></a>Teklif kurulumu

**Teklif kurulumu** sayfası, teklifiniz için bir test sürücüsü ve lider yönetimi yapılandırabileceğiniz yerdir.

### <a name="test-drive"></a>Sınama sürücüsü

Test sürücüsü, "satın almadan önce dene" seçeneği sunarak olası müşterilere teklifinizi göstermek için harika bir yoldur. bu sayede, artan dönüştürmeye ve yüksek oranda nitelikli müşteri adaylarının oluşturulmasına neden olur. [Sınama sürücüleri hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive).

Bir sınama sürücüsünü sabit bir süre etkinleştirmek için, **bir sınama sürücüsünü etkinleştir** onay kutusunu seçin. Test sürücüsünü teklifinizden kaldırmak için bu onay kutusunu temizleyin. Bu konunun ilerleyen kısımlarında, [test sürücüsü teknik yapılandırma](#test-drive-technical-configuration) bölümünde test sürücüsü ortamını yapılandırın.

Daha fazla bilgi için bkz. [ticari Market 'te teklifinizin test](https://docs.microsoft.com/azure/marketplace/partner-center-portal/test-drive)etme. Ayrıca, [test sürücüsü en iyi uygulamaları](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices) hakkında bilgi alabilir ve [Test SÜRÜCÜLERINE genel bakış PDF](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) 'sini indirebilirsiniz (açılır pencere engelinizin kapalı olduğundan emin olun)

>[!Note]
>Tüm Azure uygulamaları Azure Resource Manager şablonu kullanılarak uygulandığından, bir Azure uygulaması için kullanılabilen tek test sürücüsü türü, [Azure Resource Manager tabanlı bir test sürücüsüdür](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive).

### <a name="customer-leads"></a>Müşteri liderleri

[!INCLUDE [Connect lead management](./includes/connect-lead-management.md)]

Daha fazla bilgi için bkz. [müşteri adayı yönetimine genel bakış](./commercial-marketplace-get-customer-leads.md).

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="properties"></a>Özellikler

**Özellikler** sayfası, teklifinizi Market 'te, uygulama sürümünüzde ve teklifinizi destekleyen yasal sözleşmelerde gruplandırmak için kullanılan kategorileri ve endüstrileri tanımladığınız yerdir.

Teklifinizi uygun Market arama alanlarında yerleştirmek için en az bir ve en fazla üç kategori seçin. Teklifinizin teklif açıklamasında bu kategorileri nasıl desteklediğini açıkladığınızdan emin olun.

### <a name="legal"></a>Yasal Bildirim

[!INCLUDE [Commercial marketplace Standard Contract](./includes/marketplace-contract-text.md)]

## <a name="offer-listing"></a>Teklif listesi

Bu sayfa, ticari Market teklifinizin kopyasını ve resimlerini yönettiğiniz yerdir. 

### <a name="marketplace-details"></a>Market ayrıntıları

> [!NOTE]
> Teklif açıklaması (Açıklama, belgeler, ekran görüntüleri ve kullanım koşulları gibi) sunan teklif listesinin Ingilizce olması gerekmez, "Bu uygulama yalnızca [Ingilizce olmayan dilde] kullanılabilir". Ayrıca, teklif listeleme içeriğinde kullanılandan farklı bir dilde içerik sunmak için *yararlı bir bağlantı URL 'si* sağlamak da kabul edilebilir.

#### <a name="name"></a>Name

Buraya girdiğiniz ad, müşteriler tarafından teklif listelerinizin başlığı olarak gösterilir. Bu alan, teklifi oluştururken **teklif diğer adı** için girdiğiniz metin ile önceden doldurulur, ancak bu değeri değiştirebilirsiniz. Bu ad trademarked olabilir (ve ticari marka veya telif hakkı sembolleri dahil edebilirsiniz). Ad 50 karakterden uzun olamaz ve herhangi bir emojıs içeremez.

#### <a name="search-results-summary"></a>Arama sonuçları Özeti

Teklifinizin en fazla 100 karakter uzunluğunda olması için kısa bir açıklama sağlayın. Bu açıklama, arama sonuçlarında kullanılabilir.

#### <a name="long-summary"></a>Uzun Özet

Teklifinizin en fazla 256 karakter uzunluğunda olması için daha uzun bir açıklama sağlayın. Bu açıklama, arama sonuçlarında kullanılabilir.

#### <a name="description"></a>Açıklama

[!INCLUDE [Long description-1](./includes/long-description-1.md)]

[!INCLUDE [Long description-2](./includes/long-description-2.md)]

[!INCLUDE [Rich text editor](./includes/rich-text-editor.md)]

#### <a name="search-keywords"></a>Anahtar sözcükleri ara

Müşterilerin teklifinizi Market 'te bulmasına yardımcı olmak için isteğe bağlı en fazla üç arama anahtar sözcüğü girebilirsiniz. En iyi sonuçlar için, tanımlarınızda bu anahtar sözcükleri de kullanın.

#### <a name="privacy-policy-link"></a>Gizlilik ilkesi bağlantısı

Kuruluşunuzun gizlilik ilkesinin URL 'sini girin. Uygulamanızın gizlilik yasaları ve yönetmeliklerle uyumlu olmasını sağlamaktan ve geçerli bir gizlilik ilkesi sağlamaya yönelik siz sorumlusunuz.

### <a name="useful-links"></a>Yararlı bağlantılar

**+ Bağlantı ekle**seçeneğini belirleyerek çözümünüz hakkında isteğe bağlı ek çevrimiçi belgelere bağlantılar ekleyin.

### <a name="contact-information"></a>İletişim bilgileri

Bir **destek kişisi**, **mühendislik Iletişim**ve **CSP program kişisi**için ad, e-posta ve telefon numarası girin. Bu bilgiler müşterilere gösterilmez, ancak Microsoft tarafından kullanılabilir ve CSP iş ortakları için de sağlanıyor olabilir. Bazı kişiler için ek bilgi gerekebilir.

### <a name="marketplace-media"></a>Market medyası

Teklifinizle birlikte kullanılacak logo ve görüntüler sağlayın. Tüm görüntülerin PNG biçiminde olması gerekir. Bulanık görüntüler gönderiminizin reddedilmesine neden olur.

>[!Note]
>Dosya yükleme sorununuz varsa, yerel ağınızın https://upload.xboxlive.com Iş Ortağı Merkezi tarafından kullanılan hizmeti engellemediğinden emin olun.

#### <a name="store-logos"></a>Mağaza logoları

Aşağıdaki üç piksel boyutunda teklifinizin logosunun PNG dosyalarını sağlayın:

- **Küçük** (48 x 48)
- **Orta** (90 x 90)
- **Büyük** (216 x 216)
- **Geniş** (255 x 115)

Üç logo de gereklidir ve listede farklı yerlerde kullanılır.

#### <a name="screenshots"></a>Ekran görüntüleri

Teklifinizin nasıl çalıştığını gösteren en fazla beş ekran görüntüsü ekleyin. Her ekran görüntüsü, boyut ve PNG biçiminde 1280 x 720 piksel olmalıdır. Her ekran görüntüsü bir başlık içermelidir.

#### <a name="videos"></a>Videolar

Teklifinizi gösteren en fazla beş video ekleyin. Bunların bir dış video hizmetinde barındırılması gerekir. Videonun adını, Web adresini ve videonun küçük resim PNG görüntüsünü 1280 x 720 piksel olarak girin.

#### <a name="additional-marketplace-listing-resources"></a>Ek Market listeleme kaynakları

- [Market teklif listeleri için en iyi uygulamalar](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="preview-audience"></a>İzleyiciyi Önizle

Önizleme sekmesinde, daha geniş Market hedef kitlesi için canlı yayımlamadan önce teklifinizi doğrulamak üzere sınırlı bir **Önizleme izleyiciyi** seçin.

> [!IMPORTANT]
> Teklifinizi önizlemede denetledikten sonra, teklifinizi ticari Market genel kitlelerine yayımlamak için **canlı ol** ' u seçin.

Önizleme hedef kitlesi, Azure abonelik KIMLIĞI GUID 'Leri ve her biri için isteğe bağlı bir açıklama ile tanımlanır. Bu alanlardan hiçbiri müşteriler tarafından görülenebilir. Azure abonelik KIMLIĞINIZI Azure portal içindeki **abonelikler** sayfasında bulabilirsiniz.

Tek tek (10 ' a kadar) veya bir CSV dosyası karşıya yükleyerek (100 'e kadar) en az bir Azure abonelik KIMLIĞI ekleyin. Bu abonelik kimliklerini ekleyerek teklifinizin canlı olarak yayınlanmadan önce ne kadar önizleyebilir olduğunu tanımlarsınız. Teklifiniz zaten canlı ise, teklifinizdeki değişiklikleri veya güncelleştirmeleri test etmek için bir önizleme izleyiciyi yine de tanımlayabilirsiniz.

> [!NOTE]
> Önizleme hedef kitlesi, özel bir hedef kitleye göre farklılık gösterir. Bir önizleme hedef kitlesi, Market 'te canlı olarak yayımlanmadan _önce_ teklifinizin erişimine erişebilir. Teklifinizin Market 'e tamamen yayımlandıktan sonra yalnızca özel bir hedef kitle için kullanılabilir olacağını da içeren tüm planları görebilir ve doğrulayabilir. Özel bir hedef kitle (plan **fiyatlandırması ve kullanılabilirliği** sekmesinde tanımlanmıştır) belirli bir plana özel erişim sağlar.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="technical-configuration"></a>Teknik yapılandırma

Bu bölümü yalnızca teklifiniz Market ölçüm hizmeti API 'sini kullanarak ölçüm olaylarını yayan bir yönetilen uygulama içeriyorsa doldurun. Hizmetin ölçüm olaylarını yayırken kullanacağı **Azure Active Directory KIRACı kimliğini** ve **Azure Active Directory uygulama kimliğini** girin.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="technical-configuration-offer-level"></a>Teknik yapılandırma (teklif düzeyi)

>[!Note]
>Teklif düzeyi teknik ayrıntıları isteğe bağlıdır.  Bu ayrıntıları yalnızca ölçülen faturalandırma ile yönetilen bir uygulama yayımlıyorsanız ve bir Azure AD güvenlik belirteciyle kimlik doğrulaması yapılacak bir hizmete sahipseniz yapılandırmanız gerekir. Daha fazla bilgi için bkz. farklı kimlik doğrulama seçeneklerinde [kimlik doğrulama stratejileri](./marketplace-metering-service-authentication.md) .

Teknik yapılandırma, [Market ölçüm hizmeti API 'lerini](./marketplace-metering-service-apis.md)kullanarak yönetilen bir uygulama için ölçüm olaylarını yayan hizmetinize kimlik sağlamak için kullanılan ayrıntıları (Kiracı kimliği ve uygulama kimliği) tanımlar.  Hizmetinizin, ölçüm olaylarını yayırken kullanacağı kimliği girin.

* **Azure AD KIRACı kimliği** (gerekli): Azure Portal içinde, iki hizmetimiz arasındaki bağlantının kimliği doğrulanmış bir iletişimin arkasında olduğunu doğrulayabilmemiz için [BIR Azure Active Directory (ad) uygulaması oluşturmanız](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal) gerekir. [KIRACı kimliğini](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)bulmak için Azure Active Directory gidin ve **Özellikler**' i seçin, ardından listelenen **dizin kimliği** numarasını arayın (örneğin, 50c464d3-4930-494c-963c-1e951d15360e).
* **Azure AD uygulama kimliği** (gerekli): Ayrıca, [Uygulama Kimliğiniz](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) ve bir kimlik doğrulama anahtarınız olması gerekir. Bu değerleri almak için Azure Active Directory gidin ve **uygulama kayıtları**' i seçin, ardından LISTELENEN **uygulama kimliği** numarasını arayın (örneğin, 50c464d3-4930-494c-963c-1e951d15360e). Kimlik doğrulama anahtarını bulmak için **Ayarlar** ' a gidin ve **anahtarlar**' ı seçin. Bir açıklama ve süre sağlamanız gerekir ve ardından bir sayı değeri sağlanacaktır.

>[!Note]
>Azure uygulama KIMLIĞI, yayımcı KIMLIĞINIZLE ilişkilendirilecektir ve bu yayımcı hesabında yalnızca yeniden kullanılabilir.

>[!Note]
>[Toplu kullanım olayını](https://docs.microsoft.com/azure/marketplace/partner-center-portal/marketplace-metering-service-apis#batch-usage-event)kullanmak istiyorsanız bu yapılandırma gereklidir.  [Kullanım olayı](https://docs.microsoft.com/azure/marketplace/partner-center-portal/marketplace-metering-service-apis#usage-event)göndermek Isterseniz, [JSON Web belirteci (JWT) taşıyıcı belirtecini](https://docs.microsoft.com/azure/marketplace/partner-center-portal/pc-saas-registration#get-a-token-based-on-the-azure-ad-app)almak için [örnek meta verileri hizmetini](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) de kullanabilirsiniz.

## <a name="plan-overview"></a>Plana genel bakış

Bu sekme, aynı teklif içinde farklı plan seçenekleri sağlamanıza olanak sağlar. Bu planlar (Bulut İş Ortağı Portalı SKU 'Lar olarak adlandırılır), plan türü (çözüm şablonu ve yönetilen uygulama), paraya getirme veya hedef kitle bakımından farklı olabilir.  Teklifinizi Market 'te listelemek için en az bir plan yapılandırın.

Oluşturulduktan sonra, plan adları, kimlikleri, plan türü, kullanılabilirliği (genel veya özel), geçerli yayımlama durumu ve bu sekmede kullanılabilir tüm eylemler görüntülenir.

**Plana genel bakış** bakımından sunulan **Eylemler** planınızın geçerli durumuna bağlı olarak farklılık gösterir ve şunlar olabilir:

* Plan durumu **taslak** ise, taslağı silin.
* Plan durumu **canlı** ise, planı satmayı veya özel izleyiciyi eşitlemeyi durdurun.

### <a name="create-new-plan"></a>Yeni plan oluştur

***Plan kimliği*** – bu teklifte her plan için benzersiz BIR plan kimliği oluşturun. Bu KIMLIK, ürün URL 'sindeki müşterilere görünür olacaktır.  Yalnızca küçük harf, alfasayısal karakter, kısa çizgi veya alt çizgi kullanın. Bu plan KIMLIĞI için en fazla 50 karakter kullanılabilir. Bu KIMLIK, oluştur seçildikten sonra değiştirilemez.

***Plan adı*** -müşteriler, teklifiniz dahilinde hangi planın seçeceğinize karar verirken bu adı görür. Bu teklifteki her plan için benzersiz bir teklif adı oluşturun. Plan adı, aynı teklifin bir parçası olabilecek yazılım planlarını ayırt etmek için kullanılır (örneğin, teklif adı: Windows Server; planlar: Windows Server 2016, Windows Server 2019).

### <a name="plan-setup"></a>Planı ayarla

Bu sekme, plan türü için üst düzey yapılandırmayı, başka bir plandan gelen paketleri yeniden kullanıp kullanmayacağını ve planın hangi bulutlarda kullanılabilir olması gerektiğini ayarlamanıza olanak sağlar. Bu sekmede verdiğiniz yanıtlar aynı plan için diğer sekmelerde görüntülenecek alanları etkileyecektir.

#### <a name="plan-type"></a>Plan türü
Teklifiniz için plan türünü seçin. Bir **çözüm şablonu** planı tamamen müşteri tarafından yönetilir. **Yönetilen bir uygulama** planı, yayımcıların uygulamayı müşteri adına yönetmesine olanak sağlar. Ayrıntılar için bkz. [Azure Uygulama planlarına yönelik türler](#types-of-azure-application-plans).

#### <a name="re-use-technical-configuration"></a>Teknik yapılandırmayı yeniden kullanma

Aynı türde birden fazla planınız varsa ve paketler aralarında özdeş ise, **Bu planı başka bir plandaki paketleri yeniden kullanır**' i seçebilirsiniz.  Bu seçeneği belirlediğinizde, bu teklif için aynı türdeki diğer planlardan birini seçerek paketleri ' den yeniden kullanabilirsiniz. 

>[!Note]
>Paketleri başka bir plandan yeniden kullandığınızda, tüm teknik Yapılandırma sekmesi bu plandan kaybolur. İleride yaptığınız tüm güncelleştirmeler de dahil olmak üzere diğer plandaki teknik yapılandırma ayrıntıları, bu plan için de kullanılacaktır.<br><br>Bu plan yayımlandıktan sonra bu ayar değiştirilemez.

#### <a name="azure-regions-cloud"></a>Azure bölgeleri (bulut)

Planınız en az bir Azure bölgesinde kullanılabilir duruma getirilmelidir.

Planınızı, ticari Market tümleştirmesi olan tüm Azure genel bölgelerindeki müşterilerin kullanımına sunmak için **Azure genel** seçeneğini belirleyin. Ayrıntılar için bkz. [coğrafi kullanılabilirlik ve para birimi desteği](https://docs.microsoft.com/azure/marketplace/marketplace-geo-availability-currencies).

Planınızı [Azure Kamu](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome) bölgesinde kullanılabilir hale getirmek Için **Azure Kamu** seçeneğini belirleyin. Bu bölge, ABD Federal, eyalet, yerel veya üç aylık varlıklardan müşterilere yönelik denetimli erişim sağlar ve bu da onlara sunmaya uygun iş ortakları sağlar. Yayımcı olarak tüm uyumluluk denetimleri, güvenlik ölçüleri ve en iyi uygulamalardan sorumludur. Azure Kamu fiziksel olarak yalıtılmış veri merkezleri ve ağlar (yalnızca ABD 'de bulunur) kullanır.

[Azure Kamu](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners)'da yayımlamadan önce, belirli uç noktalar farklı olabileceğinden planınızı ortamda test edin ve doğrulayın. Planınızı ayarlamak ve test etmek için [Microsoft Azure Kamu deneme](https://azure.microsoft.com/global-infrastructure/government/request/)sürümünden bir deneme hesabı isteyin.

>[!NOTE]
>Planınız yayımlandıktan ve belirli bir Azure bölgesinde kullanılabilir olduktan sonra bu bölgeyi kaldıramazsınız.

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

### <a name="availability-solution-template-plans-only"></a>Kullanılabilirlik (yalnızca çözüm şablonu planları)

Planı herkese, yalnızca belirli müşterilere (özel bir hedef kitleye) görünür hale getirebilirsiniz ve planın yalnızca diğer çözüm şablonu veya yönetilen uygulamalar tarafından kullanılması için gizli hale getirme yapıp yapmayacağı.

#### <a name="plan-visibility"></a>Plan görünürlüğü

Her planı herkese görünür olacak şekilde veya yalnızca seçtiğiniz belirli bir kitlede olacak şekilde yapılandırabilirsiniz. Azure abonelik kimliklerini kullanarak bu kısıtlanmış hedef kitleye üyelik atayabilirsiniz.

Planınızı özel ve yalnızca seçtiğiniz kısıtlanmış kitlele görünür hale getirmek için **özel bir plan olan bu bir plandır** . Özel bir plan olarak yayımlandıktan sonra izleyiciyi güncelleştirebilir veya planı herkes için kullanılabilir hale getirebilirsiniz. Bir plan herkese görünür olarak yayımlandıktan sonra herkese görünür olmalıdır; özel bir plan olarak yeniden yapılandırılamaz.

Planı özel yaparsanız, bir **Azure abonelik kimlikleri** ve açıklamasını girin. Her biri, bu özel plana erişimi olacak olan bir hedef kitledir. Atanan her bir Azure abonelik KIMLIĞININ açıklamasını ekleme seçeneğiyle Azure abonelik kimlikleri kullanılarak erişim atanır. 10 adede kadar müşterinin abonelik kimliğini tek tek veya bir CSV dosyasını içeri aktararak 20.000 ekleyin. Azure abonelik kimlikleri, GUID 'Ler olarak temsil edilir ve harflerin küçük harf olması gerekir.

>[!Note]
>Özel veya kısıtlanmış bir hedef kitle, **Önizleme** sekmesinde tanımladığınız önizleme izleyicilerinizden farklıdır. Bir önizleme hedef kitlesi, Market 'te canlı olarak yayımlanmadan _önce_ teklifinizin erişimine açabilir. Özel hedef kitle seçimi yalnızca belirli bir plana uygulansa da, önizleme hedef kitlesi doğrulama amacıyla tüm planları (özel veya değil) görüntüleyebilir.

#### <a name="hide-plan"></a>Planı gizle

Çözüm şablonunuz, başka bir çözüm şablonu veya yönetilen uygulama başvurulduğu sırada yalnızca dolaylı olarak dağıtılmak üzere tasarlanıyorsa, çözüm şablonunuzu yayımlamak için bu kutuyu işaretleyin, ancak doğrudan arama yaparak ve göz atarak bu kutuyu gizleyin.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

### <a name="pricing-and-availability-managed-application-plans-only"></a>Fiyatlandırma ve kullanılabilirlik (yalnızca yönetilen uygulama planları)

Bu planın kullanılabileceği **pazarları** yapılandırmak için bu planı kullanın, çözüm yönetimine ait aylık **fiyatlandırma** ve yalnızca belirli müşteriler tarafından görüleye (özel bir hedef kitle) varsa **plan görünürlüğü** .

Devam etmeden önce **Taslağı kaydet** ' i seçin.

#### <a name="markets"></a>Pazar

Her plan en az bir pazarda kullanılabilir olmalıdır. Bu planı kullanılabilir yapmak istediğiniz tüm pazar konumları için onay kutusunu seçin. Microsoft remits Sales ve kullanım vergisinin sizin adınıza, yardım 'a dahil olduğu "vergi havalesi" ülkelerinden oluşan bir arama kutusu ve düğmesi.

Planınız için Birleşik Devletler dolar (USD) cinsinden zaten fiyatlar ayarladıysanız ve başka bir pazar konumu eklerseniz, yeni pazar fiyatı geçerli döviz kurlarına göre hesaplanır. Yayımlamadan önce her bir pazar için fiyatı her zaman gözden geçirin. Fiyatlandırma, değişiklikleriniz kaydedildikten sonra "fiyatları dışarı aktar (xlsx)" bağlantısı kullanılarak incelenebilir.

#### <a name="pricing"></a>Fiyatlandırma

Bu plan için aylık fiyat fiyatını belirtin.  Bu fiyat, bu çözüm tarafından dağıtılan kaynaklar tarafından tahakkuk eden tüm Azure altyapısına veya kullandıkça öde yazılım maliyetlerine ek olarak yapılır.

Aylık fiyata ek olarak, [ölçülen faturalandırmayı](./azure-app-metered-billing.md)kullanarak standart olmayan birimlerin tüketimine yönelik fiyatlar da ayarlayabilirsiniz.  Aylık fiyat fiyatını sıfır olarak ayarlayabilir ve isterseniz tarifeli faturalandırma kullanarak özel olarak ücretlendirme yapabilirsiniz. 

ABD Doları cinsinden ayarlanan fiyatlar (USD = Birleşik Devletler dolar), kaydedildiği sırada geçerli döviz kurları kullanılarak tüm seçili piyasaların yerel para birimine dönüştürülür. Fiyatlandırma elektronik tablosunu dışarı aktarıp her bir pazardaki fiyatı inceleyerek yayımlamadan önce bu fiyatları doğrulayın. Tek bir pazarda özel fiyatlar ayarlamak isterseniz, fiyatlandırma elektronik tablosunu değiştirin ve içeri aktarın. 

>[!Note]
>Fiyatlandırma verilerinin dışarı aktarılmasını sağlamak için öncelikle fiyatlandırma değişikliklerinizi kaydetmeniz gerekir.

Bir plan yayımlandıktan sonra nelerin değiştirebilecekleri hakkında bazı kısıtlamalar olduğundan, yayımlamadan önce fiyatlarınızı dikkatlice gözden geçirin.  

>[!Note]
>Planınızdaki bir pazar bedeli yayımlandığında, daha sonra değiştirilemez.

#### <a name="plan-visibility"></a>Plan görünürlüğü

Her planı herkese görünür olacak şekilde veya yalnızca seçtiğiniz belirli bir kitlede olacak şekilde yapılandırabilirsiniz. Azure abonelik kimliklerini kullanarak bu kısıtlanmış hedef kitleye üyelik atayabilirsiniz.

Planınızı özel ve yalnızca seçtiğiniz kısıtlanmış kitlele görünür hale getirmek için **özel bir plan olan bu bir plandır** . Özel bir plan olarak yayımlandıktan sonra izleyiciyi güncelleştirebilir veya planı herkes için kullanılabilir hale getirebilirsiniz. Bir plan herkese görünür olarak yayımlandıktan sonra herkese görünür olmalıdır; özel bir plan olarak yeniden yapılandırılamaz.

Planı özel yaparsanız, bir **Azure abonelik kimlikleri** ve açıklamasını girin. Her biri, bu özel plana erişimi olacak olan bir hedef kitledir. Atanan her bir Azure abonelik KIMLIĞININ açıklamasını ekleme seçeneğiyle Azure abonelik kimlikleri kullanılarak erişim atanır. 10 adede kadar müşterinin abonelik kimliğini tek tek veya bir CSV dosyasını içeri aktararak 20.000 ekleyin. Azure abonelik kimlikleri, GUID 'Ler olarak temsil edilir ve harflerin küçük harf olması gerekir.

>[!Note]
>Özel veya kısıtlanmış bir hedef kitle, **Önizleme** sekmesinde tanımladığınız önizleme izleyicilerinizden farklıdır. Bir önizleme hedef kitlesi, Market 'te canlı olarak yayımlanmadan _önce_ teklifinizin erişimine açabilir. Özel hedef kitle seçimi yalnızca belirli bir plana uygulansa da, önizleme hedef kitlesi doğrulama amacıyla tüm planları (özel veya değil) görüntüleyebilir.

### <a name="technical-configuration"></a>Teknik yapılandırma 

Bu sekme, müşterilerin planınızı dağıtmasına olanak sağlayacak dağıtım paketini karşıya yüklemenize olanak sağlar.

>[!Note]
>Bu plan, farklı bir plandaki paketleri **plan kurulumu** sekmesinde yeniden kullanmak üzere yapılandırdıysanız bu sekme görünür olmayacaktır.

#### <a name="package-details"></a>Paket ayrıntıları

Bu sekme, teknik yapılandırmanızın taslak sürümünü düzenlemenize olanak tanır.

**Sürüm** – teknik yapılandırmanın güncel sürümünü atayın.  Bu sayfada her değişiklik yayımlaışınızda bu sürümü artırın. Sürüm biçimde olmalıdır `{integer}.{integer}.{integer}` .

**Paket dosyası** (. zip) – Bu paket, bu plan için gereken tüm şablon dosyalarının yanı sıra dosya olarak paketlenmiş ek kaynaklar içerir `.zip` .

Tüm Azure uygulama planı paketleri, bir arşiv kök klasöründe bu iki dosyayı içermelidir `.zip` :

* [Maintemplate. JSON](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)adlı bir kaynak yöneticisi şablon dosyası. Bu şablon, müşterilerin Azure aboneliğine kaynak dağıtımını otomatikleştirir.  Kaynak Yöneticisi şablonlarının örnekleri için bkz. [Azure hızlı başlangıç şablonları Galerisi](https://azure.microsoft.com/documentation/templates/) veya ilgili [GitHub: Azure Resource Manager hızlı başlangıç şablonları](https://github.com/azure/azure-quickstart-templates) deposu.
* [Createuıdefinition. JSON](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview)adlı Azure uygulama oluşturma deneyimi için bir kullanıcı arabirimi tanımı.

Desteklenen en büyük dosya boyutu:

* Toplam sıkıştırılmış arşiv boyutunda en fazla 1 GB `.zip`
* Arşiv içindeki her bir sıkıştırılmamış dosya için 1 GB 'a kadar `.zip`  

Tüm yeni Azure Uygulama teklifleri Ayrıca bir [Azure iş ortağı müşteri kullanımı attributıon](https://docs.microsoft.com/azure/marketplace/azure-partner-customer-usage-attribution) GUID 'i de içermelidir.

>[!Note]
>Dosya yükleme sorununuz varsa, yerel ağınızın https://upload.xboxlive.com Iş Ortağı Merkezi tarafından kullanılan hizmeti engellemediğinden emin olun.

Yönetilen uygulama planları bu sekmede ek bilgi gerektirir.

#### <a name="previously-published-packages"></a>Önceden yayımlanmış paketler

**Daha önce yayınlanan paketler** alt sekmesi, teknik yapılandırmanızın tüm yayınlanan sürümlerini görüntülemenize olanak sağlar.

#### <a name="enable-just-in-time-jit-access"></a>Tam zamanında (JıT) erişimi etkinleştir

Bu plan için tam zamanında (JıT) erişimi etkinleştirmek üzere bu seçeneği belirleyin.  JıT erişimi, sorun giderme ve bakım için yönetilen bir uygulamanın kaynaklarına yükseltilmiş erişim isteme imkanı sağlar. Kaynaklara her zaman salt okuma erişiminizin olması gerekir, ancak belirli bir süre için daha fazla erişime sahip olabilirsiniz.  Daha fazla bilgi için bkz. [Azure yönetilen uygulamalar için tam zamanında erişimi etkinleştirme ve isteme](https://docs.microsoft.com/azure/managed-applications/request-just-in-time-access).  Yönetilen uygulamanızın tüketicilerinin hesabınıza kalıcı erişim vermesini gerektirmek için, bu seçeneği işaretlenmemiş olarak bırakın.

>[!Note]
>`createUiDefinition.json`Bu özelliği desteklemek için dosyanızı güncelleştirdiğinizden emin olun.  

#### <a name="deployment-mode"></a>Dağıtım modu

Bu planı dağıttığınızda, **tamamlanmış** veya **artımlı dağıtım modunun** yapılandırıp yapılandırmadığınızı seçin: 

* **Tüm modda**, kaynak içinde tanımlanmazsa, uygulamanın müşteri tarafından yeniden dağıtımı yönetilen kaynak grubundaki kaynakların kaldırılmasına neden olur `mainTemplate.json` . 
* **Artımlı modda**, uygulamanın yeniden dağıtımı mevcut kaynakları değişmeden bırakır.

Dağıtım modları hakkında daha fazla bilgi için bkz. [Azure Resource Manager Dağıtım modları](https://docs.microsoft.com/azure/azure-resource-manager/deployment-modes).

#### <a name="notification-endpoint-url"></a>Bildirim uç noktası URL 'SI

Bu plan sürümünün yönetilen uygulama örneklerinde bulunan tüm CRUD işlemlerine ilişkin bildirimleri almak için bir HTTPS Web kancası uç noktası belirtin.

#### <a name="customize-allowed-customer-actions"></a>İzin verilen müşteri eylemlerini özelleştirme

Müşterilerin, `*/read` Varsayılan olarak kullanılabilir olan "" eylemlerine ek olarak yönetilen kaynaklarda gerçekleştirebileceği eylemleri belirtmek için bu seçeneği belirleyin. 

Müşterinizin bu şekilde gerçekleştirmesini sağlamak istediğiniz ek eylemleri, noktalı virgülle ayırarak listeleyin.  Daha fazla bilgi için bkz. [Azure kaynakları için reddedilen atamaları anlama](https://docs.microsoft.com/azure/role-based-access-control/deny-assignments).  Kullanılabilir eylemler için bkz. [Azure Resource Manager kaynak sağlayıcısı işlemleri](https://docs.microsoft.com/azure/role-based-access-control/resource-provider-operations). Örneğin, tüketicilerin sanal makineleri yeniden başlatmasına izin vermek için `Microsoft.Compute/virtualMachines/restart/action` izin verilen eylemlere ekleyin.

#### <a name="global-azure--azure-government-cloud"></a>Küresel Azure/Azure Kamu Bulutu

Desteklenen her bulutta bu yönetilen uygulamaya yönetim erişiminin olması gereken kişileri belirtin. Yönetilen kaynak grubu için izin verilmesini istediğiniz kullanıcılar, gruplar veya uygulamalar Azure Active Directory (AAD) kimlikleri kullanılarak tanımlanır.

**Azure Active Directory KIRACı kimliği** : izin vermek istediğiniz kullanıcıların, grupların veya uygulamaların KIMLIKLERINI Içeren AAD Kiracı kimliği (dizin kimliği olarak da bilinir). Azure portal [Azure Active Directory ÖZELLIKLERINDE](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Properties)AAD kiracı kimliğinizi bulabilirsiniz.

**Yetkilendirmeler** – yönetilen kaynak grubu için izin verilmesini istediğiniz kullanıcı, Grup veya uygulamanın Azure ACTIVE DIRECTORY nesne kimliğini ekleyin. Kullanıcıyı, [Azure portal Azure Active Directory Kullanıcıları dikey penceresinde](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UsersManagementMenuBlade/AllUsers)bulunan birincil kimliğine göre belirler.

Her sorumlu için, listeden (sahip veya katkıda bulunan) Azure AD yerleşik rollerinden birini seçin. Seçtiğiniz rol, sorumlunun müşteri aboneliğindeki kaynaklarda sahip olacağı izinleri anlatmaktadır. Daha fazla bilgi için bkz. [Azure kaynakları için yerleşik roller](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles). Rol tabanlı erişim denetimi (RBAC) hakkında daha fazla bilgi için bkz. [Azure Portal RBAC ile çalışmaya başlama](https://docs.microsoft.com/azure/role-based-access-control/overview).

>[!Note]
>Bulut başına en çok 100 yetkilendirmeler ekleyebilseniz de, genellikle bir Active Directory Kullanıcı grubu oluşturmak ve KIMLIĞINI "asıl KIMLIK" içinde belirtmek daha kolay olur. Bu, plan dağıtıldıktan sonra yönetim grubuna daha fazla kullanıcı eklemenize ve daha fazla yetkilendirmeler eklemek için planı güncelleştirme ihtiyacını azaltmasına imkan sağlar.

#### <a name="policy-settings"></a>İlke ayarları

Dağıtılan çözüme yönelik uyumluluk gereksinimlerini belirtmek için yönetilen uygulamanıza [Azure ilkeleri](https://docs.microsoft.com/azure/governance/policy/overview) uygulayın. İlke tanımları ve parametre değerlerinin biçimi için bkz. [Azure İlke Örnekleri](https://docs.microsoft.com/azure/governance/policy/samples/index). En fazla beş ilke ve her Ilke seçeneğinin yalnızca bir örneğini yapılandırabilirsiniz. Bazı ilkeler ek parametreler gerektirir. Denetim ilkeleri için standart SKU gereklidir. İlke adı 50 karakterle sınırlıdır.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="co-sell-with-microsoft"></a>Microsoft ile ortak satış yapma

Ortak satış sekmesi hakkında bilgi sağlamak, teklifinizi yayımlamak için tamamen isteğe bağlıdır. Ortak satış için hazırlık ve IP ortak satışı için hazırlık durumu elde etmek gerekir. Sağladığınız bilgiler, Microsoft Sales ekipleri tarafından müşteri gereksinimlerine uygun olarak değerlendirme yaparken çözümünüz hakkında daha fazla bilgi edinmek için kullanılacaktır. Doğrudan müşteriler tarafından kullanılamaz.

Bu sekmeye ilişkin ayrıntılar için bkz. [Partner Center 'Da ortak satış seçeneği](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-co-sell).

## <a name="resell-through-csps"></a>CSP 'Ler aracılığıyla yeniden satış

[Bulut çözüm sağlayıcıları](https://azure.microsoft.com/offers/ms-azr-0145p/) (CSP) programındaki iş ortakları için kullanılabilir hale getirerek teklifinizin erişimini genişletin. Bu, satıcıların teklifinizi müşterilere satmasını ve paketlenmiş çözümler oluşturmasını sağlar.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="test-drive"></a>Sınama sürücüsü

Müşterilerin teklifinizi satın almadan önce denemesini sağlayan bir tanıtım (test sürücüsü) ayarlayın. Müşterilerin teklifinizi sabit bir süre için denemesini sağlayan bir tanıtım ortamı oluşturmak için, bkz. [ticari Market 'te teklifinizin test](https://docs.microsoft.com/azure/marketplace/partner-center-portal/test-drive)etme.

Bir sınama sürücüsünü etkinleştirmek için [teklif kurulumu](#test-drive) sekmesinde **bir test sürücüsünü etkinleştir** onay kutusunu işaretleyin. Test sürücüsünü teklifinizden kaldırmak için bu onay kutusunu temizleyin.

### <a name="test-drive-technical-configuration"></a>Sınama sürücüsü teknik yapılandırma

- **Azure AD uygulama kimliği** (gerekli): Azure ACTIVE DIRECTORY (ad) [uygulama kimliğinizi](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)girin. Bu KIMLIĞI bulmak için [Azure Portal](https://portal.azure.com/)oturum açın, sol menüden Active Directory sekmesini seçin, **uygulama kayıtları**' yı seçin, ardından listelenen **uygulama kimliği** numarasını arayın (örneğin, 50c464d3-4930-494c-963c-1e951d15360e).

#### <a name="deployment-subscription-details"></a>Dağıtım aboneliği ayrıntıları

Test sürücüsünün sizin adınıza dağıtılmasına izin vermek için, ayrı ve benzersiz bir Azure aboneliği oluşturun ve sağlayın (Power BI test sürücüleri için gerekli değildir).

* **Azure ABONELIK kimliği** (Azure Resource Manager ve Logic Apps için gereklidir) – Azure hesap hizmetlerinizi kaynak kullanımı raporlama ve faturalama için erişim izni vermek üzere abonelik kimliğini girin. Henüz bir tane yoksa, test sürücüleri için kullanmak üzere [ayrı bir Azure aboneliği oluşturmayı](https://docs.microsoft.com/azure/billing/billing-create-subscription) düşünmeniz önerilir. [Azure Portal](https://portal.azure.com/) oturum açarak ve sol taraftaki menüdeki **abonelikler** SEKMESINE giderek Azure abonelik kimliğinizi bulabilirsiniz. Sekmeyi seçtiğinizde, abonelik KIMLIĞINIZ görüntülenir (örneğin, "a83645ac-1234-5AB6-345-1h234g764ghty").
* **Azure AD KIRACı kimliği** (gerekli) – Azure ACTIVE DIRECTORY (ad) [kiracı kimliğinizi](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)girin. Bu KIMLIĞI bulmak için [Azure Portal](https://portal.azure.com/)oturum açın, sol taraftaki menüden Active Directory sekmesini seçin, **Özellikler**' i seçin, sonra listelenen **dizin kimliği** numarasını (örneğin, 50c464d3-4930-494c-963c-1e951d15360e) arayın. Ayrıca, şu adreste bulunan etki alanı adı URL 'nizi kullanarak kuruluşunuzun kiracı KIMLIĞINI de arayabilirsiniz: [https://www.whatismytenantid.com](https://www.whatismytenantid.com) .
* **Azure AD kiracı adı** (dinamik 365 için gereklidir) – Azure ACTIVE DIRECTORY (ad) adınızı girin. Bu adı bulmak için sağ üst köşedeki [Azure Portal](https://portal.azure.com/)oturum açın, kiracı adınız hesap adınızın altında listelenecektir.
* **Azure AD uygulama kimliği** (gerekli) – Azure ACTIVE DIRECTORY (ad) [uygulama kimliğinizi](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)girin. Bu KIMLIĞI bulmak için [Azure Portal](https://portal.azure.com/)oturum açın, sol gezinti menüsünde Active Directory sekmesini seçin, **uygulama kayıtları**' i seçin, ardından listelenen **uygulama kimliği** numarasını (örneğin, 50c464d3-4930-494c-963c-1e951d15360e) arayın.
* **Azure Active Directory uygulama istemci parolası** (gerekli) – Azure AD uygulamanızın [Istemci gizli](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#certificates-and-secrets)anahtarını girin. Bu değeri bulmak için [Azure Portal](https://portal.azure.com/)oturum açın. Sol gezinti menüsünde **Azure Active Directory** sekmesini seçin, **uygulama kayıtları**' i seçin ve ardından test sürücüsü uygulamanızı seçin. Sonra **Sertifikalar ve gizlilikler**' ı seçin, **yeni istemci parolası**' nı seçin, bir açıklama girin, **süresi dolmasın**' ı seçin ve ardından **Ekle**' yi **Never** Bu sayfadan çıkmadan önce değeri kopyalamadığınızdan emin olun.)

Devam etmeden önce **Taslağı kaydet** ' i seçin.

### <a name="marketplace-listing-optional"></a>Market listeleme (isteğe bağlı)

Sınama sürücüsü deneyimini açıklama.

<!-- The **Test Drive listings** option found under the **Test drive** tab displays the languages (and markets) where your test drive is available (currently English (United States) is the only location available). Additionally, this page displays the status of the language-specific listing and the date/time that it was added. You will need to define the test drive details (description, user manual, videos, etc.) for each language/market. -->

* **Açıklama** (gerekli) – test sürücünüzü, neyi gösterdiklerinizi, kullanıcının deneme amaçlarını, araştırılacak özellikleri ve kullanıcının teklifinizi elde edip etmeyeceğinizi belirlemesine yardımcı olmak için Ilgili bilgileri tanıtın. Bu alana en fazla 3.000 karakter metin girilebilir. 
* **Erişim bilgileri** (Azure Resource Manager ve mantıksal test sürücüleri için gereklidir) – Bu test sürücüsüne erişmek ve bu sınama sürücüsüne erişmek için müşterinin neleri bilmesi gerektiğini açıklayın. Teklifinizi kullanmaya yönelik bir senaryoya ve müşterinin test sürüşü genelinde özelliklere erişmek için bilmeleri gereken tam olarak ne olduğunu öğrenin. Bu alana en fazla 10.000 karakter metin girilebilir.
* **Kullanıcı el ile** (gerekli) – test sürücünüzün deneyiminizdeki derinlemesine bir adım adım. Kullanıcı el kitabı, müşterinin test sürücüsüyle karşılaşmasını istediğiniz şekilde tam olarak kapsamalıdır ve sahip olabileceği sorulara başvuru olarak görev yapar. Dosya, karşıya yüklendikten sonra PDF biçiminde ve adlandırılmalıdır (en fazla 255 karakter) olmalıdır.
* **Videolar: video ekleme** (isteğe bağlı) – videolar, YouTube veya Vimeo 'a karşıya yüklenebilir ve burada bir bağlantı ve küçük resim görüntüsü (533 x 324 piksel) ile birlikte başvuruluyorsa, bir müşterinin test sürücüsünü daha iyi anlamasına yardımcı olmak için, teklifin özelliklerini ve avantajlarının vurgulanmasını sağlayan senaryoları anlamak da dahil olmak üzere, daha fazla bilgi edinmek için,
  * **Ad** (gerekli)
  * **Adres** (yalnızca YouTube veya Vimeo) gereklidir)
  * **Küçük resim** (görüntü dosyası png biçiminde ve 533 x 324 piksel) olmalıdır).

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="publish"></a>Yayımlama

Teklifin tüm gerekli bölümlerini tamamladığınızda, portalın sağ üst köşesinde bulunan **gözden geçir ve Yayımla** ' yı seçin.

Teklifin her bir bölümü için tamamlanma durumunu gözden geçirin.
    - *Başlatılmamış* -bölüm dokunulmamış ve tamamlanması gereken anlamına gelir.
    - *Tamamlanmamış* -bölümde düzeltilmesi gereken hatalar olduğu veya daha fazla bilgi sağlanması gerekir. Bölüm (ler) e geri dönün ve güncelleştirin.
    - *Tam* -bölümün tamamlandığı, tüm gerekli verilerin sağlandığı ve hata olmadığı anlamına gelir. Teklifi gönderebilmeniz için teklifin tüm bölümlerinin tamamen bir durumda olması gerekir.

Bu teklifi ilk kez yayımlıyorsanız, uygulamanızı anlamak için yararlı olan tüm ek notlara ek olarak, uygulamanızın doğru şekilde test edilmesini sağlamak için sertifika ekibine test yönergeleri sağlayabilirsiniz.

Teklifinizi yayımlamaya göndermek için **Gönder** ' i seçin. Size, gözden geçirmeniz ve onaylamanız için teklifin bir önizleme sürümünün ne zaman kullanılabileceğini bilmenizi sağlayacak bir e-posta göndereceğiz.

Iş Ortağı Merkezi 'ne dönün ve teklifinizi herkese açık bir şekilde (veya özel bir teklif olarak özel hedef kitleye) yayımlama teklifi için **Go-Live** ' ı seçin.

### <a name="errors-and-review-feedback"></a>Hatalar ve gözden geçirme geri bildirimi

Yayımlama işlemindeki **el ile doğrulama** adımı, teklifinizin ve ilişkili teknik varlıklarının (özellikle Azure Resource Manager şablonu) kapsamlı bir incelemesini temsil eder, sorunlar genellikle çekme ISTEğI (PR) bağlantıları olarak sunulur. Bu PR 'ler nasıl görüntüleneceği ve yanıtlanmasına ilişkin bir açıklama, bkz. [İnceleme geri bildirimini işleme](./azure-apps-review-feedback.md).

Yayımlama adımlarında bir veya daha fazla hata varsa, teklifinizi yeniden yayımlamadan önce bunları düzeltin.

## <a name="next-steps"></a>Sonraki adımlar

* [Ticari Market'te bulunan bir teklifi güncelleştirme](./update-existing-offer.md)
