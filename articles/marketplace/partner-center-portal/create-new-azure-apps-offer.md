---
title: Ticari Market 'te yeni bir Azure uygulamaları teklifi oluşturun
description: Azure Market, AppSource veya Microsoft Iş Ortağı Merkezi 'ndeki ticari Market portalını kullanan bulut çözümü sağlayıcısı (CSP) programı aracılığıyla listeleme veya satma için yeni bir Azure uygulamaları teklifi oluşturma.
author: MaggiePucciEvans
manager: evansma
ms.author: evansma
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/21/2019
ms.openlocfilehash: 69d7bc81f5adc4797be86f946fdb656b4b6cc682
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79283374"
---
# <a name="create-an-azure-application-offer"></a>Azure uygulama teklifi oluşturma

Ticari Market 'te bir Azure uygulaması teklifi yayımlama adımları burada özetlenmiştir.

## <a name="azure-application-offer-type"></a>Azure uygulaması teklif türü

Bu konu başlığı altında, Azure Uygulama teklifleri hakkında temel bilgiler verilmektedir.  Market 'te yeni bir Azure Uygulama teklifi yayımlama işlemini başlatmadan önce bu kavramları öğrenmeniz gerekir.

### <a name="publishing-overview"></a>Yayımlamaya genel bakış

Azure [Market 'e yönelik çözüm şablonları ve yönetilen uygulamalar için](https://channel9.msdn.com/Events/Build/2018/BRK3603) sunulan video, Azure uygulaması teklif türü 'ne bir giriş niteliğindedir:

* Hangi teklif türleri kullanılabilir?
* Hangi teknik varlıkların gerekli olduğu;
* Azure Resource Manager şablonu yazma;
* Uygulama kullanıcı arabirimini geliştirme ve test etme;
* Uygulama teklifini yayımlama;
* Uygulama İnceleme işlemi.

### <a name="types-of-azure-application-plans"></a>Azure uygulama planlarının türleri

İki tür Azure uygulama planı, yönetilen uygulamalar ve çözüm şablonları vardır.

* **Çözüm şablonu** , Market 'te çözüm yayımlamanın ana yöntemlerinden biridir. Bu plan türü, çözümünüz tek bir sanal makinenin (VM) ötesinde ek dağıtım ve yapılandırma Otomasyonu gerektirdiğinde kullanılır.  Bir çözüm şablonuyla, karmaşık IaaS çözümleri sağlamak üzere VM 'Ler, ağ oluşturma ve depolama kaynakları dahil olmak üzere birden fazla kaynağın sağlanması otomatik hale getirebilirsiniz.  Çözüm şablonları oluşturma hakkında daha fazla bilgi için [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) belgelerine bakın.

* **Yönetilen uygulama** , tek bir anahtar farklılığı ile çözüm şablonlarına benzerdir. Yönetilen bir uygulamada kaynaklar, uygulamanın yayımcısı tarafından yönetilen bir kaynak grubuna dağıtılır. Kaynak grubu, tüketicinin aboneliğinde mevcuttur ancak yayımcının kiracısındaki bir kimlik, kaynak grubuna erişime sahiptir. Yayımcı olarak, çözümün sürekli desteği için maliyeti belirtirsiniz. Yönetilen uygulamaları kullanarak, müşterilerinize tam olarak yönetilen, anahtar kullanan uygulamaları kolayca oluşturup sunun.  Yönetilen uygulamaların avantajları ve türleri hakkında daha fazla bilgi için bkz. [Azure yönetilen uygulamalarına genel bakış](https://docs.microsoft.com/azure/managed-applications/overview).

Tüm Azure uygulamaları `.zip` arşivi kök klasöründe en az iki dosya içerir:

* [Maintemplate. JSON](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)adlı bir kaynak yöneticisi şablon dosyası.  Bu, müşterinin Azure aboneliğine dağıtılacak kaynakları tanımlayan şablondur.  Kaynak Yöneticisi şablonlarının örnekleri için bkz. [Azure hızlı başlangıç şablonları Galerisi](https://azure.microsoft.com/resources/templates/) veya ilgili [GitHub: Azure Resource Manager hızlı başlangıç şablonları](https://github.com/azure/azure-quickstart-templates) deposu.

* [Createuıdefinition. JSON](https://docs.microsoft.com/azure/managed-applications/create-uidefinition-overview)adlı Azure uygulama oluşturma deneyimi için bir kullanıcı arabirimi tanımı.  Kullanıcı arayüzünde tüketicilerin parametre değerleri sağlamasına olanak tanıyan öğeleri belirlersiniz.

Tüm yeni Azure Uygulama teklifleri bir [Azure iş ortağı müşteri kullanımı attributıon GUID 'i](https://docs.microsoft.com/azure/marketplace/azure-partner-customer-usage-attribution)içermelidir.

### <a name="before-you-begin"></a>Başlamadan önce

Hızlı başlangıçler, öğreticiler ve örnekler sağlayan aşağıdaki Azure uygulaması belgelerini gözden geçirin.

* [Azure Resource Manager şablonlarını anlama](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)
* Hızlı Başlangıçlar:

    * [Azure hızlı başlangıç şablonları](https://azure.microsoft.com/documentation/templates/)
    * [GitHub Azure hızlı başlangıç şablonları](https://github.com/azure/azure-quickstart-templates)
    * [Uygulama tanımını Yayımla](https://docs.microsoft.com/azure/managed-applications/publish-managed-app-definition-quickstart)
    * [Hizmet kataloğu uygulaması dağıtma](https://docs.microsoft.com/azure/managed-applications/deploy-service-catalog-quickstart)

* Öğreticiler:

    * [Tanım dosyaları oluştur](https://docs.microsoft.com/azure/managed-applications/publish-service-catalog-app)
    * [Market uygulaması yayımlama](https://docs.microsoft.com/azure/managed-applications/publish-marketplace-app)

* Lerinizi

    * [Azure CLI](https://docs.microsoft.com/azure/managed-applications/cli-samples)
    * [Azure PowerShell](https://docs.microsoft.com/azure/managed-applications/powershell-samples)
    * [Yönetilen uygulama çözümleri](https://docs.microsoft.com/azure/managed-applications/sample-projects)

### <a name="fundamentals-in-technical-knowledge"></a>Teknik Bilgi Bankası temelleri

Bu varlıkları tasarlama, oluşturma ve test etme zaman alır ve hem Azure platformu hem de teklifi oluşturmak için kullanılan teknolojiler hakkında teknik bilgi gerektirir.

Mühendislik ekibiniz aşağıdaki Microsoft teknolojileri hakkında bilgi sahibi olmalıdır:

* [Azure hizmetlerinin](https://azure.microsoft.com/services/)temel olarak anlaşılmasıdır.
* [Azure uygulamaları tasarlama ve mimari](https://azure.microsoft.com/solutions/architecture/)oluşturma.
* [Azure sanal makineleri](https://azure.microsoft.com/services/virtual-machines/), [Azure depolama](https://azure.microsoft.com/services/?filter=storage#storage)ve [Azure ağ](https://azure.microsoft.com/services/?filter=networking#networking)ile çalışma hakkında bilgi.
* [Azure Resource Manager](https://azure.microsoft.com/features/resource-manager/)çalışma bilgileri.
* [JSON](https://www.json.org/)hakkında çalışma bilgisi.

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

Mevcut araçları [Azure Geliştirici Araçları](https://azure.microsoft.com/tools/) sayfasında inceleyebilirsiniz.  Ayrıca, Visual Studio kullanıyorsanız [Visual Studio Market](https://marketplace.visualstudio.com/).

## <a name="create-an-azure-application-offer"></a>Azure uygulama teklifi oluşturma

Bir Azure uygulaması teklifi oluşturabilmeniz için önce [bir Iş Ortağı Merkezi hesabı oluşturmanız](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account) ve **genel bakış** sekmesi seçili olarak [ticari Market panosunu](https://partner.microsoft.com/dashboard/commercial-marketplace/offers)açmanız gerekir.

>[!Note]
>Bir teklif yayımlandıktan sonra, Iş Ortağı Merkezi 'nde yapılan teklifle ilgili düzenlemeler yalnızca sistem ve yeniden yayımlama sonrasında, sistem ve storets 'ler tarafından güncelleştirilir.  Lütfen değişiklikler yapıldıktan sonra teklifi yayın için gönderdiğinizden emin olun.

### <a name="create-a-new-offer"></a>Yeni teklif oluşturma

**+ Yeni teklif** düğmesini seçin ve ardından **Azure Uygulama** menü öğesini seçin. **Yeni teklif** iletişim kutusu görüntülenir.

### <a name="offer-id-and-alias"></a>Teklif KIMLIĞI ve diğer ad

* **TEKLIF kimliği**: hesabınızdaki her bir teklif için benzersiz bir tanımlayıcı. Bu KIMLIK, Market teklifi ve Azure Resource Manager şablonları (varsa) için URL adresindeki müşterilere görünür olacaktır. <br> <br> Teklif KIMLIĞINIZ küçük harfli alfasayısal karakterler (tireler ve alt çizgiler dahil ancak boşluk yok) olmalıdır. 50 karakterle sınırlıdır ve Oluştur ' u seçtikten sonra değiştirilemez. <br> <br> Örneğin, buraya `test-offer-1` girerseniz, teklif URL 'SI `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`olur. 

* **Teklif diğer adı**: Iş Ortağı Merkezi içindeki teklifine başvurmak için kullanılan ad. Bu ad Market 'te kullanılmayacak ve teklif adından ve müşterilere gösterilecek diğer değerlerden farklı. Bu değer, **Oluştur**' u seçtikten sonra değiştirilemez.

**TEKLIF kimliği** ve **teklif diğer adınızı**girdikten sonra **Oluştur**' u seçin. Daha sonra teklifinizin diğer tüm bölümleri üzerinde çalışabilirsiniz.

## <a name="offer-setup"></a>Teklif kurulumu

**Teklif kurulum** sayfası, aşağıdaki bilgileri ister. Bu alanları tamamladıktan sonra **Kaydet** ' i seçtiğinizden emin olun.

### <a name="test-drive"></a>Sınama sürücüsü

Test sürücüsü, bu kullanıcılara teklifinizi "satın almadan önce dene" seçeneği sunarak, daha fazla dönüştürmeye ve yüksek oranda nitelikli müşteri adaylarının oluşturulmasına neden olacak şekilde sergilemenin harika bir yoludur. [Sınama sürücüleri hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)

Bir sınama sürücüsünü etkinleştirmek için, **sınama sürücüsünü etkinleştir** kutusunu işaretleyin. Daha sonra, müşterilerin teklifinizi sabit bir süre için denemesini sağlamak üzere teknik yapılandırma Yapılandır ' da [Test sürücüsünde](#types-of-azure-application-plans) bir tanıtım ortamı yapılandırmanız gerekir. 

>[!Note]
>Tüm Azure uygulamaları Azure Resource Manager şablonu kullanılarak uygulandığından, bir Azure uygulaması için yapılandırılabilecek tek test sürücüsü türü, [Azure Resource Manager tabanlı bir test sürücüsüdür](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive).

#### <a name="additional-test-drive-resources"></a>Ek test sürücüsü kaynakları

- [Test sürücüsü teknik En Iyi uygulamaları](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [Test sürüşü Pazarlama En Iyi yöntemleri](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [Test sürücüsüne genel bakış bir sayfalayıcı](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf)

## <a name="connect-lead-management"></a>Müşteri adayı yönetimini bağlama

[!INCLUDE [Connect lead management](./includes/connect-lead-management.md)]

Daha fazla bilgi için bkz. [müşteri adayı yönetimine genel bakış](./commercial-marketplace-get-customer-leads.md).

Sonraki bölüme geçmeden önce **kaydetmeyi** unutmayın!

## <a name="properties"></a>Özellikler

**Özellikler** sayfası, teklifinizi Market 'te, uygulama sürümünüzde ve teklifinizi destekleyen yasal sözleşmelerde gruplandırmak için kullanılan kategorileri ve endüstrileri tanımlamanızı sağlar. Bu sayfayı tamamladıktan sonra **Kaydet** ' i seçin.

### <a name="category"></a>Kategori

Teklifinizi uygun Market arama alanına yerleştirmek için kullanılacak en az bir ve en fazla üç kategori seçin. Teklifinizin teklif açıklamasında bu kategorileri nasıl desteklediğine dikkat edin. 

### <a name="standard-contract-for-the-microsoft-commercial-marketplace"></a>Microsoft ticari Market için standart sözleşme

[!INCLUDE [Commercial marketplace Standard Contract](./includes/marketplace-contract-text.md)]

## <a name="offer-listing"></a>Teklif listesi

Teklif listesi sayfası, teklifinizin listelenecek dilleri görüntüler. Şu anda **İngilizce (Birleşik Devletler)** kullanılabilir tek seçenektir.

Her dil/Pazar için Market ayrıntılarını (teklif adı, açıklama, görüntüler vb.) tanımlamanız gerekir. Bu bilgileri sağlamak için dil/Pazar adını seçin.

> [!NOTE]
> Teklif açıklaması (örneğin, açıklama, belgeler, ekran görüntüleri, kullanım koşulları vb.) için teklif listesi içeriği, "Bu uygulama yalnızca [Ingilizce olmayan dil] içinde kullanılabilir" olarak, Ingilizce olması gerekmez. Ayrıca, teklif listeleme içeriğinde kullanılandan farklı bir dilde içerik sunmak için *yararlı bir bağlantı URL 'si* sağlamak da kabul edilebilir.

### <a name="name"></a>Adı

Buraya girdiğiniz ad, müşteriler tarafından teklif listelerinizin başlığı olarak gösterilir. Bu alan, teklifi oluştururken **teklif diğer adı** için girdiğiniz metin ile önceden doldurulur, ancak bu değeri değiştirebilirsiniz. Bu ad trademarked olabilir (ve ticari marka veya telif hakkı sembolleri dahil edebilirsiniz). Ad 50 karakterden uzun olamaz ve herhangi bir emojıs içeremez.

### <a name="summary"></a>Özet

Sunabileceğiniz (en fazla 100 karakter) kısa bir açıklama sağlayın ve bu işlem Market arama sonuçlarında kullanılabilir.

### <a name="long-summary"></a>Uzun Özet

Teklifiniz için daha uzun bir açıklama sağlayın (en fazla 256 karakter). Açıklama Market arama sonuçlarında kullanılıyor olabilir.

### <a name="description"></a>Açıklama

Teklifiniz için daha uzun bir açıklama sağlayın (en fazla 3.000 karakter). Bu açıklama Market dökümüne Genel Bakış bölümündeki müşterilere görüntülenecektir. Teklifinizin değer teklifini, anahtar avantajlarını, kategori ve/veya sektör ilişkilendirmelerini, uygulama içi satın alma fırsatlarını ve gerekli tüm teklifleri dahil edin. 

Tanımlarınızı yazmak için bazı ipuçları:  

- Tanımınızın ilk birkaç cümlede teklifinizin değer teklifini açık bir şekilde açıklamalıdır. Değer teklifinde aşağıdaki öğeleri ekleyin:
  - Ürünün açıklaması
  - Üründen faydalanan Kullanıcı türü
  - Müşteri ihtiyaçları veya ürün adresleriyle ilgili bir sorun
- İlk birkaç tümcenin arama motoru sonuçlarında görüntülenebileceğini aklınızda bulundurun.  
- Ürününüzü satmaya yönelik özelliklere ve işlevlere güvenmeyin. Bunun yerine, teslim ettiğiniz değere odaklanın.  
- Sektöre özgü sözlük veya avantaj temelli bir ifade mümkün olduğunca yararlanın. 
- Açıklamanızı biçimlendirmek ve daha ilgi çekici hale getirmek için HTML etiketleri kullanmayı düşünün.

### <a name="search-keywords"></a>Anahtar sözcükleri ara

Müşterilerin teklifinizi Market 'te bulmasına yardımcı olmak için isteğe bağlı en fazla üç arama anahtar sözcüğü girebilirsiniz. En iyi sonuçlar için, tanımlarınızda bu anahtar sözcükleri de kullanmayı deneyin.

### <a name="support-urls"></a>Destek URL 'Leri

Bu bölüm, müşterilerinizin teklifiniz hakkında daha fazla bilgi sağlamasına yardımcı olacak bağlantılar sağlamanıza olanak tanır.

#### <a name="privacy-policy-url"></a>Gizlilik ilkesi URL 'SI

Kuruluşunuzun gizlilik ilkesinin URL 'sini girin. Uygulamanızın gizlilik yasaları ve yönetmeliklerle uyumlu olmasını sağlamaktan ve geçerli bir gizlilik ilkesi sağlamaya yönelik siz sorumlusunuz.

#### <a name="useful-links"></a>Yararlı bağlantılar

Çözümünüz hakkında isteğe bağlı ek çevrimiçi belgeler sağlayın.  **+ Bağlantı ekle**' ye tıklayarak ek yararlı bağlantılar ekleyin.

### <a name="contacts"></a>Kişiler

Bu bölümde, bir **destek kişisi** ve **mühendislik ilgili kişisi**için ad, e-posta ve telefon numarası sağlamanız gerekir. Bu bilgiler müşterilere gösterilmez, ancak Microsoft tarafından kullanılabilir ve CSP iş ortakları için de sağlanıyor olabilir.

**Destek kişisi** bölümünde, CSP iş ortaklarının teklifiniz için destek BULABILECEĞI **Destek URL** 'sini de sağlamanız gerekir.

### <a name="marketplace-images"></a>Market görüntüleri

Bu bölümde, müşteri teklifinizi gösterirken kullanılacak logo ve görüntüler sağlayabilirsiniz. Tüm görüntülerin. png biçiminde olması gerekir.

#### <a name="store-logos"></a>Mağaza logoları

Teklifinizin logosunu üç boyutta sağlayın: **küçük (48 x 48)** , **Orta (90 x 90)** ve **büyük (216 x 216)** .

#### <a name="hero"></a>Hero

Hero görüntüsü isteğe bağlıdır. Bir tane sağlarsanız, 815 x 290 pikseli ölçmelidir.

#### <a name="screenshots"></a>Ekran görüntüleri

Teklifinizin nasıl çalıştığını gösteren ekran görüntüleri ekleyin. En fazla beş ekran görüntüsü ekleyebilirsiniz. Tüm ekran görüntüleri 1280 x 720 piksel olmalıdır.

#### <a name="videos"></a>Videolar

İsteğe bağlı olarak, teklifinizi gösteren en fazla beş video ekleyebilirsiniz. Bu videoların YouTube ve/veya Vimeo 'da barındırılması gerekir. Her biri için videonun adını, URL 'sini ve videonun küçük bir görüntüsünü (1280 x 720 piksel) girin.

#### <a name="additional-marketplace-listing-resources"></a>Ek Market listeleme kaynakları

- [Market teklif listeleri için en iyi uygulamalar](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)

## <a name="preview"></a>Önizleme

**Önizleme** sekmesi, teklifinizi daha geniş Market hedef kitlelerine yayımlamadan önce teklifinizi doğrulamak için sınırlı bir **Önizleme hedef kitlesi** tanımlamanızı sağlar.

> [!IMPORTANT]
> Teklifinizi önizlemede denetledikten sonra Market ortak hedef kitlesi için canlı **çalış** ' ı seçmeniz gerekir.

Önizleme hedef kitlesi, Azure abonelik KIMLIĞI GUID 'Leri tarafından tanımlanır ve her biri için isteğe bağlı bir açıklama ile eşleştirilir.  Bu alanlardan hiçbiri müşteriler tarafından görülemez.

10 adede kadar Azure abonelik kimliğini el ile veya bir CSV dosyası karşıya yüklerken 100 'e kadar ekleyin.  Bu abonelikleri ekleyerek, tam olarak yayımlanmadan önce teklifiniz için önizlemeye erişim izni verilecek bir hedef kitle tanımlamanız gerekir.  Teklifiniz zaten canlı ise, teklifiniz için herhangi bir değişikliği veya güncelleştirmeyi test etmek üzere bir önizleme hedef kitlesi de tanımlayabilirsiniz.

>[!Note]
>Önizleme hedef kitlesi, özel bir hedef kitleye göre farklılık gösterir. Bir önizleme hedef kitlesi, Market 'te canlı olarak yayımlanmadan *önce* teklifinizin erişimine izin verilir. Ayrıca, bir plan oluşturmayı ve bunu yalnızca özel bir hedef kitle için kullanılabilir hale getirebilirsiniz (plan kullanılabilirliği sekmesini kullanarak).  Önizleme izleyicileriniz, teklifinizin Market 'e tamamen yayımlandıktan sonra yalnızca özel bir hedef kitle tarafından kullanılabilecek planlar dahil olmak üzere tüm planları görebilir ve doğrulayabilecektir.

## <a name="plan-overview"></a>Plana genel bakış

**Plana genel bakış** sekmesi, aynı teklif içinde farklı plan seçenekleri sağlamanıza olanak sağlar. Bu planlar (Bulut İş Ortağı Portalı SKU 'Lar olarak adlandırılır), plan türü (çözüm şablonu ve yönetilen uygulama), paraya getirme veya hedef kitle bakımından farklı olabilir.  Teklifinizi Market 'te listelemek için en az bir plan yapılandırın.

Oluşturulduktan sonra, plan adları, kimlikleri, plan türü, kullanılabilirliği (genel veya özel), geçerli yayımlama durumu ve bu sekmede kullanılabilir tüm eylemler görüntülenir.

**Plana genel bakış** bakımından sunulan **Eylemler** planınızın geçerli durumuna bağlı olarak farklılık gösterir ve şunlar olabilir:

* Plan durumu **taslak** ise, taslağı silin.
* Plan durumu **canlı** ise, planı satmayı veya özel izleyiciyi eşitlemeyi durdurun.

### <a name="create-new-plan"></a>Yeni plan oluştur

***Plan kimliği*** -bu teklifteki her plan için benzersiz BIR plan kimliği oluşturun. Bu KIMLIK, ürün URL 'sindeki müşterilere görünür olacaktır.  Yalnızca küçük harf, alfasayısal karakter, kısa çizgi veya alt çizgi kullanın. Bu plan KIMLIĞI için en fazla 50 karakter kullanılabilir. Bu KIMLIK, oluştur seçildikten sonra değiştirilemez.

***Plan adı*** -müşteriler, teklifiniz dahilinde hangi planın seçeceğinize karar verirken bu adı görür. Bu teklifteki her plan için benzersiz bir teklif adı oluşturun. Plan adı, aynı teklifin bir parçası olabilecek yazılım planlarını ayırt etmek için kullanılır (ör. Teklif adı: Windows Server; planlar: Windows Server 2016, Windows Server 2019).

### <a name="plan-setup"></a>Planı ayarla

**Plan kurulumu** sekmesi, plan türü için üst düzey yapılandırmayı, başka bir plandan gelen paketleri yeniden kullanıp kullanmayacağını ve planın hangi bulutlarda kullanılabilir olması gerektiğini ayarlamanıza olanak sağlar.  Bu sekmede verdiğiniz yanıtlar aynı plan için diğer sekmelerde görüntülenecek alanları etkileyecektir.

#### <a name="plan-type"></a>Plan türü

[Azure uygulama planlarının türlerinde](#types-of-azure-application-plans)özetlenen şekilde, planınızın bir çözüm şablonu mı yoksa yönetilen bir uygulama mı olacağını seçin.

#### <a name="this-plan-reuses-packages"></a>Bu plan paketleri yeniden kullanır

Aynı türde birden fazla planınız varsa ve paketler aralarında özdeş ise, **Bu planı başka bir plandaki paketleri yeniden kullanır**' i seçebilirsiniz.  Bu seçeneği belirlediğinizde, bu teklif için aynı türdeki diğer planlardan birini seçerek paketleri ' den yeniden kullanabilirsiniz. 

>[!Note]
>Paketleri başka bir plandan yeniden kullandığınızda, tüm teknik Yapılandırma sekmesi bu plandan kaybolur.  İleride yaptığınız tüm güncelleştirmeler de dahil olmak üzere diğer plandaki teknik yapılandırma ayrıntıları, bu plan için de kullanılacaktır. <br> <br> Ayrıca, bu plan yayımlandıktan sonra bu ayar değiştirilemez.

#### <a name="cloud-availability"></a>Bulut kullanılabilirliği

Bu plan, en az bir bulutta kullanılabilir hale getirilmelidir. 

Çözümünüzü Market tümleştirmesi olan tüm genel Azure bölgelerinde müşterilere dağıtılabilir hale getirmek için **Genel Azure** seçeneğini belirleyin.  [Coğrafi kullanılabilirlik](https://docs.microsoft.com/azure/marketplace/marketplace-geo-availability-currencies)hakkında daha fazla bilgi edinin.

Çözümünüzü [Azure Kamu bulutunda](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome)dağıtılabilir hale getirmek için **Azure** kamu bulutu seçeneğini belirleyin. ABD Federal, eyalet, yerel veya üç aylık ve iş ortakları tarafından bu varlıklara uygun olan müşteriler için denetimli erişime sahip bir devlet topluluğu bulutu.  Yayımcı olarak, bu bulut Community 'yi sunmaya yönelik tüm uyumluluk denetimleri, güvenlik ölçüleri ve en iyi uygulamalardan sorumludur.  Azure Kamu fiziksel olarak yalıtılmış veri merkezleri ve ağlar (yalnızca ABD 'de bulunur) kullanır.  Microsoft, [Azure Kamu](https://aka.ms/azuregovpublish)'ya yayımlamadan önce, belirli uç noktalar farklı olabileceğinden çözümünüzü ortamınızda test etmeniz ve doğrulamanız önerilir. Çözümünüzü hazırlamak ve test etmek için bu [bağlantıdan](https://azure.microsoft.com/global-infrastructure/government/request/)bir deneme hesabı isteyin.

>[!Note]
>Bir plan belirli bir bulutta kullanılabilir olarak yayımlandıktan sonra, bu bulut kaldırılamaz.

**Azure Kamu bulut sertifikaları**

Bu seçenek yalnızca **Azure Kamu Bulutu** 'nın **bulut kullanılabilirliği**altında seçilmesi durumunda görünür.

Azure Kamu Hizmetleri, Fedrampa, NıST 800,171 (DIB), ıTAR, ıRS 1075, DoD L4 ve CJıS gibi belirli kamu düzenlemelerine ve gereksinimlerine tabi olan verileri işler.  Bu programların sertifikalarına yönelik olarak bir açıklama getirmek için, sertifikalarınızı açıklayan en fazla 100 bağlantı sağlayabilirsiniz.  Bu bağlantılar programın doğrudan listelemesine yönelik bağlantılar ya da kendi Web sitelerinizde uyumluluğun açıklamalarına yönelik bağlantılar olabilir.  Bu bağlantılar yalnızca Azure Kamu bulut müşterileri tarafından görülebilir.

## <a name="plan-listing"></a>Planı listeleme

**Plan listeleme** sekmesi, aynı teklifin farklı planları arasında farklı olabilecek plana özgü liste bilgilerini görüntüler.

### <a name="name"></a>Adı

Adınızı temel alarak, sizin oluşturduğunuz zaman planınızı atadığınız şekilde önceden doldurulmuş olursunuz.  Bu ad, Market 'te görüntülenen bu "yazılım planı" başlığı olarak görünür.  En fazla 100 karakter içerebilir.

### <a name="summary"></a>Özet

Yazılım planınızın kısa bir özetini sağlayın.  En fazla 100 karakter içerebilir.

### <a name="description"></a>Açıklama

Bu açıklama, bu yazılım planının ne olduğunu ve teklifinizdeki diğer yazılım planlarından herhangi bir farklılık yaptığını açıklayan bir fırsattır. En fazla 2.000 karakter içerebilir.

Bu alanları tamamladıktan sonra **Kaydet** ' i seçin.

## <a name="availability"></a>Kullanılabilirlik

**Kullanılabilirlik** sekmesi yalnızca çözüm şablonu planları için görülebilir.  Planı herkese, yalnızca belirli müşterilere (özel bir hedef kitleye) görünür hale getirebilirsiniz ve planın yalnızca diğer çözüm şablonu veya yönetilen uygulamalar tarafından kullanılması için gizli hale getirme yapıp yapmayacağı.

### <a name="plan-audience"></a>Hedef kitle planlayın

Her planı herkese görünür olarak veya yalnızca seçtiğiniz belirli bir kitlele yapılandırma seçeneğiniz vardır. Azure abonelik kimliklerini kullanarak bu kısıtlanmış hedef kitleye üyelik atayabilirsiniz.

**Gizlilik/bu özel bir plandır** (isteğe bağlı onay kutusu)-planınızı özel ve yalnızca seçtiğiniz kısıtlanmış kitlele görünür hale getirmek için bu kutuyu işaretleyin. Özel bir plan olarak yayımlandıktan sonra izleyiciyi güncelleştirebilir veya planı herkes için kullanılabilir hale getirebilirsiniz. Bir plan herkese görünür olarak yayımlandıktan sonra herkese görünür kalması gerekir. (Plan bir özel plan olarak yeniden yapılandırılamaz).

**Kısıtlanmış hedef kitle (Azure abonelik kimlikleri)** -bu özel plana erişimi olacak hedef kitleyi atayın. Atanan her bir Azure abonelik KIMLIĞININ açıklamasını ekleme seçeneğiyle Azure abonelik kimlikleri kullanılarak erişim atanır. Bir. csv elektronik tablo dosyası içeri aktarıldıysanız, en fazla 10 abonelik kimliği eklenebilir veya 20.000 müşteri abonelik kimliği olabilir.  Azure abonelik kimlikleri, GUID 'Ler olarak temsil edilir ve harflerin küçük harfle oluşturulması gerekir.

>[!Note]
>Özel hedef kitle (veya kısıtlı kitleci) [**Önizleme**](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-saas-offer#preview) sekmesinde tanımladığınız önizleme izleyicilerinden farklıdır.  Bir önizleme hedef kitlesi, Market 'te canlı olarak yayımlanmadan *önce* teklifinizin erişimine izin verilir. Özel izleyici ataması yalnızca belirli bir plana uygulansa da, önizleme hedef kitlesi doğrulama amacıyla tüm planları (özel veya değil) görüntüleyebilir.

### <a name="hide-plan"></a>Planı gizle

Çözüm şablonunuz, başka bir çözüm şablonu veya yönetilen uygulama başvurulduğu sırada yalnızca dolaylı olarak dağıtılmak üzere tasarlanıyorsa, çözüm şablonunuzu yayımlamak için bu kutuyu işaretleyin, ancak doğrudan arama yaparak ve göz atarak bu kutuyu gizleyin.

## <a name="pricing-and-availability"></a>Fiyatlandırma ve kullanılabilirlik

**Fiyatlandırma ve kullanılabilirlik** sekmesi yalnızca yönetilen uygulama planları tarafından görülebilir.  Bu planın kullanılabileceği pazarları, çözümün yönetimi için aylık fiyat ve planı herkese veya yalnızca belirli müşterilere (özel bir hedef kitle) görünür hale getirmek isteyip istemediğinizi yapılandırabilirsiniz.

### <a name="markets"></a>Pazar

Her plan en az bir pazarda kullanılabilir olmalıdır. Bu planı kullanılabilir yapmak istediğiniz tüm pazar konumları için onay kutusunu seçin. Microsoft remits Sales ve kullanım vergisinin sizin adınıza yardımına dahil olduğu "vergi havalesi" ülkelerinden oluşan bir arama kutusu ve düğmesi.

Planınız için Birleşik Devletler dolar (USD) cinsinden zaten fiyatlar ayarladıysanız ve başka bir pazar konumu eklerseniz, yeni pazar fiyatı geçerli döviz kurlarına göre hesaplanır. Yayımlamadan önce her bir pazar için fiyatı her zaman gözden geçirin. Fiyatlandırma, değişiklikleriniz kaydedildikten sonra "fiyatları dışarı aktar (xlsx)" bağlantısı kullanılarak incelenebilir.

### <a name="pricing"></a>Fiyatlandırma

Bu plan için aylık fiyat fiyatını belirtin.  Bu fiyat, bu çözüm tarafından dağıtılan kaynaklar tarafından tahakkuk eden tüm Azure altyapısına veya kullandıkça öde yazılım maliyetlerine ek olarak yapılır.

Yerel para birimi cinsinden ayarlanan fiyatlar (USD = Birleşik Devletler dolar), kurulum sırasında kullanılabilir olan geçerli döviz kurlarını kullanarak tüm seçili piyasaların yerel para birimine dönüştürülür. Fiyatlandırma elektronik tablosunu dışarı aktarıp her bir pazardaki fiyatı inceleyerek yayımlamadan önce bu fiyatları doğrulayın. Tek bir pazarda özel fiyatlar ayarlamak isterseniz, fiyatlandırma elektronik tablosunu değiştirin ve içeri aktarın. 

>[!Note]
>Fiyatlandırma verilerinin dışarı aktarılmasını sağlamak için öncelikle fiyatlandırma değişikliklerinizi kaydetmeniz gerekir.

Bir plan yayımlandıktan sonra nelerin değiştirebilecekleri hakkında bazı kısıtlamalar olduğundan, yayımlamadan önce fiyatlarınızı dikkatlice gözden geçirin.  

>[!Note]
>Planınızdaki bir pazar bedeli yayımlandığında, daha sonra değiştirilemez.

### <a name="plan-audience"></a>Hedef kitle planlayın

Her planı herkese görünür olarak veya yalnızca seçtiğiniz belirli bir kitlele yapılandırma seçeneğiniz vardır. Azure abonelik kimliklerini kullanarak bu kısıtlanmış hedef kitleye üyelik atayabilirsiniz.

**Gizlilik/bu özel bir plandır** (isteğe bağlı onay kutusu)-planınızı özel ve yalnızca seçtiğiniz kısıtlanmış kitlele görünür hale getirmek için bu kutuyu işaretleyin. Özel bir plan olarak yayımlandıktan sonra izleyiciyi güncelleştirebilir veya planı herkes için kullanılabilir hale getirebilirsiniz. Bir plan herkese görünür olarak yayımlandıktan sonra herkese görünür kalması gerekir. (Plan bir özel plan olarak yeniden yapılandırılamaz).

**Kısıtlanmış hedef kitle (Azure abonelik kimlikleri)** -bu özel plana erişimi olacak hedef kitleyi atayın. Atanan her bir Azure abonelik KIMLIĞININ açıklamasını ekleme seçeneğiyle Azure abonelik kimlikleri kullanılarak erişim atanır. Bir. csv elektronik tablo dosyası içeri aktarıldıysanız, en fazla 10 abonelik kimliği eklenebilir veya 20.000 müşteri abonelik kimliği olabilir.  Azure abonelik kimlikleri, GUID 'Ler olarak temsil edilir ve harflerin küçük harfle oluşturulması gerekir.

>[!Note]
>Özel hedef kitle (veya kısıtlı kitleci) [**Önizleme**](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-saas-offer#preview) sekmesinde tanımladığınız önizleme izleyicilerinden farklıdır.  Bir önizleme hedef kitlesi, Market 'te canlı olarak yayımlanmadan *önce* teklifinizin erişimine izin verilir. Özel izleyici ataması yalnızca belirli bir plana uygulansa da, önizleme hedef kitlesi doğrulama amacıyla tüm planları (özel veya değil) görüntüleyebilir.

## <a name="technical-configuration"></a>Teknik yapılandırma 

**Teknik yapılandırma** sekmesi, müşterilerin planınızı dağıtmasını sağlayacak dağıtım paketini karşıya yüklemenize olanak sağlar.

>[!Note]
>Bu plan, farklı bir plandaki paketleri **plan kurulumu** sekmesinde yeniden kullanmak üzere yapılandırdıysanız bu sekme görünür olmayacaktır.

### <a name="package-details"></a>Paket ayrıntıları

**Paket ayrıntıları** alt sekmesi, teknik yapılandırmanızın taslak sürümünü düzenlemenize olanak sağlar.

***Sürüm*** -teknik yapılandırmanın güncel sürümünü atayın.  Bu sayfada her değişiklik yayımlaışınızda bu sürümü artırın. Sürüm `{integer}.{integer}.{integer}`biçiminde olmalıdır.

***Paket dosyası*** (`.zip`)-Bu paket, bu plan için gereken tüm şablon dosyalarını ve bir `.zip` dosyası olarak paketlenmiş ek kaynakları içerir.

Tüm Azure uygulama planı paketleri, bir `.zip` arşivi kök klasöründe bu iki dosyayı içermelidir:

* [Maintemplate. JSON](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)adlı bir kaynak yöneticisi şablon dosyası.  Bu şablon, müşterilerin Azure aboneliğine kaynak dağıtımını otomatikleştirir.  Kaynak Yöneticisi şablonlarının örnekleri için bkz. [Azure hızlı başlangıç şablonları Galerisi](https://azure.microsoft.com/documentation/templates/) veya ilgili [GitHub: Azure Resource Manager hızlı başlangıç şablonları](https://github.com/azure/azure-quickstart-templates) deposu.

* [Createuıdefinition. JSON](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview)adlı Azure uygulama oluşturma deneyimi için bir kullanıcı arabirimi tanımı.

Tüm yeni Azure Uygulama teklifleri Ayrıca bir [Azure iş ortağı müşteri kullanımı attributıon](https://docs.microsoft.com/azure/marketplace/azure-partner-customer-usage-attribution) GUID 'i de içermelidir.

### <a name="previously-published-packages"></a>Önceden yayımlanmış paketler 

**Daha önce yayımlanmış paketler** alt sekmesi, teknik yapılandırmanızın tüm yayınlanan sürümlerini görüntülemenize olanak sağlar.

## <a name="technical-configuration-managed-application-plans-only"></a>Teknik yapılandırma (yalnızca yönetilen uygulama planları)

Yönetilen uygulama planlarının, yukarıda açıklanan **Sürüm** ve **paket dosyası** alanlarının ötesinde **Teknik yapılandırma** sekmesinde ek karmaşıklık vardır. 

### <a name="enable-just-in-time-jit-access"></a>Tam zamanında (JıT) erişimi etkinleştir

Bu plan için tam zamanında (JıT) erişimi etkinleştirmek üzere bu seçeneği belirleyin.  JıT erişimi, sorun giderme ve bakım için yönetilen bir uygulamanın kaynaklarına yükseltilmiş erişim isteme imkanı sağlar. Kaynaklara her zaman salt okuma erişiminizin olması gerekir, ancak belirli bir süre için daha fazla erişime sahip olabilirsiniz.  Daha fazla bilgi için bkz. [Azure yönetilen uygulamalar için tam zamanında erişimi etkinleştirme ve isteme](https://docs.microsoft.com/azure/managed-applications/request-just-in-time-access).  Yönetilen uygulamanızın tüketicilerinin hesabınıza kalıcı erişim vermesini gerektirmek için, bu seçeneği işaretlenmemiş olarak bırakın.

>[!Note]
>Bu özelliği desteklemek için `createUiDefinition.json` dosyanızı güncelleştirdiğinizden emin olun.  

### <a name="deployment-mode"></a>Dağıtım modu

Bu planı dağıttığınızda, **tamamlanmış** veya **artımlı dağıtım modunun** yapılandırıp yapılandırmadığınızı seçin: 

* **Tüm modda**, kaynak `mainTemplate.json`tanımlanmamışsa, uygulamanın müşteri tarafından yeniden dağıtımı yönetilen kaynak grubundaki kaynakların kaldırılmasına neden olur. 
* **Artımlı modda**, uygulamanın yeniden dağıtımı mevcut kaynakları değişmeden bırakır.

Dağıtım modları hakkında daha fazla bilgi için bkz. [Azure Resource Manager Dağıtım modları](https://docs.microsoft.com/azure/azure-resource-manager/deployment-modes).

### <a name="notification-endpoint-url"></a>Bildirim uç noktası URL 'SI

Bu plan sürümünün yönetilen uygulama örneklerinde bulunan tüm CRUD işlemlerine ilişkin bildirimleri almak için bir HTTPS Web kancası uç noktası belirtin.

### <a name="customize-allowed-customer-actions"></a>İzin verilen müşteri eylemlerini özelleştirme

Müşterilerin varsayılan olarak kullanılabilir olan "`*/read`" eylemlerine ek olarak, yönetilen kaynaklarda hangi eylemlerin gerçekleştirebileceği belirlemek için bu seçeneği belirleyin. 

Müşterinizin bu şekilde gerçekleştirmesini sağlamak istediğiniz ek eylemleri, noktalı virgülle ayırarak listeleyin.  Daha fazla bilgi için bkz. [Azure kaynakları için reddedilen atamaları anlama](https://docs.microsoft.com/azure/role-based-access-control/deny-assignments).  Kullanılabilir eylemler için bkz. [Azure Resource Manager kaynak sağlayıcısı işlemleri](https://docs.microsoft.com/azure/role-based-access-control/resource-provider-operations). Örneğin, tüketicilerin sanal makineleri yeniden başlatmasına izin vermek için izin verilen eylemlere `Microsoft.Compute/virtualMachines/restart/action` ekleyin.

### <a name="global-azure--azure-government-cloud"></a>Küresel Azure/Azure Kamu Bulutu

Desteklenen her bulutta bu yönetilen uygulamaya yönetim erişiminin olması gereken kişileri belirtin.  Yönetilen kaynak grubu için izin verilmesini istediğiniz kullanıcılar, gruplar veya uygulamalar Azure Active Directory (AAD) kimlikleri kullanılarak tanımlanır.

***Azure Active Directory KIRACı kimliği*** -izin vermek istediğiniz kullanıcıların, grupların veya uygulamaların KIMLIKLERINI Içeren AAD Kiracı kimliği (dizin kimliği olarak da bilinir).  Azure portal [Azure Active Directory ÖZELLIKLERINDE](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Properties)AAD kiracı kimliğinizi bulabilirsiniz.

***Yetkilendirmeler*** -yönetilen kaynak grubu için izin verilmesini istediğiniz kullanıcı, Grup veya uygulamanın Azure ACTIVE DIRECTORY nesne kimliğini ekleyin. Kullanıcıyı, [Azure portal Azure Active Directory Kullanıcıları dikey penceresinde](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UsersManagementMenuBlade/AllUsers)bulunan birincil kimliğine göre belirler.

Her sorumlu için, listeden (sahip veya katkıda bulunan) Azure AD yerleşik rollerinden birini seçin. Seçtiğiniz rol, sorumlunun müşteri aboneliğindeki kaynaklarda sahip olacağı izinleri anlatmaktadır. Daha fazla bilgi için bkz. [Azure kaynakları için yerleşik roller](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).  Rol tabanlı erişim denetimi (RBAC) hakkında daha fazla bilgi için bkz. [Azure Portal RBAC ile çalışmaya başlama](https://docs.microsoft.com/azure/role-based-access-control/overview).

>[!Note]
>Bulut başına en çok 100 yetkilendirmeler ekleyebilseniz de, genellikle bir Active Directory Kullanıcı grubu oluşturmak ve KIMLIĞINI "asıl KIMLIK" içinde belirtmek daha kolay olur.  Bu, plan dağıtıldıktan sonra yönetim grubuna daha fazla kullanıcı eklemenizi ve daha fazla yetkilendirmeler eklemek için planı güncelleştirme ihtiyacını azaltmanızı sağlar.

### <a name="policy-settings"></a>İlke ayarları

Dağıtılan çözüme yönelik uyumluluk gereksinimlerini belirtmek için yönetilen uygulamanıza [Azure ilkeleri](https://docs.microsoft.com/azure/governance/policy/overview) uygulayın.  İlke tanımları ve parametre değerlerinin biçimi için bkz. [Azure İlke Örnekleri](https://docs.microsoft.com/azure/governance/policy/samples/index).  En fazla beş ilke ve her Ilke seçeneğinin yalnızca bir örneğini yapılandırabilirsiniz.  Bazı ilkeler ek parametreler gerektirir.  Denetim ilkeleri için standart SKU gereklidir.  İlke adı 50 karakterle sınırlıdır.

## <a name="co-sell"></a>Ortak satış

Cosatm sekmesi hakkında bilgi sağlamak, teklifinizi yayımlamak için tamamen isteğe bağlıdır. Ortak satış için hazırlık ve IP ortak satışı için hazırlık durumu elde etmek gerekir. Sağladığınız bilgiler, Microsoft Sales ekipleri tarafından müşteri gereksinimlerine uygun olarak değerlendirme yaparken çözümünüz hakkında daha fazla bilgi edinmek için kullanılacaktır. Doğrudan müşteriler tarafından kullanılamaz.

Bu sekmeyi tamamlama hakkında daha fazla bilgi için bkz. [Partner Center 'Da ortak satış seçeneği](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-co-sell).

## <a name="test-drive"></a>Sınama sürücüsü

**Test sürücüsü** sekmesi, müşterilerin satın alma işlemine geçmeden önce teklifinizi denemesini sağlayacak bir tanıtım (veya "test sürücüsü") ayarlamanıza olanak sağlar. [Test sürücüsü nedir](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive) makalesinde daha fazla bilgi edinin  Teklifiniz için artık bir test sürücüsü sağlamak istemiyorsanız **teklif kurulumu** sayfasına dönün ve **Test sürücüsünü etkinleştir**seçeneğinin işaretini kaldırın.

### <a name="technical-configuration"></a>Teknik yapılandırma

Azure uygulamaları kendiliğinden Azure Resource Manager test sürücü türünü kullanır.  Daha fazla bilgi için bkz. [Azure Resource Manager test sürücüsü Için Teknik yapılandırma](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-customer-engagement-offer#technical-configuration-for-azure-resource-manager-test-drive) .

### <a name="deployment-subscription-details"></a>Dağıtım aboneliği ayrıntıları

Test sürücüsünü sizin adınıza dağıtmak için lütfen benzersiz ve benzersiz bir Azure aboneliği oluşturun ve sağlayın. (Power BI test sürücüleri için gerekli değildir).

- **Azure ABONELIK kimliği** (Azure Resource Manager ve Logic Apps için gereklidir): Azure hesap hizmetlerinizi kaynak kullanımı raporlama ve faturalama için erişim izni vermek üzere abonelik kimliğini girin. Henüz bir tane yoksa, test sürücüleri için kullanmak üzere [ayrı bir Azure aboneliği oluşturmayı](https://docs.microsoft.com/azure/billing/billing-create-subscription) düşünmeniz önerilir. [Azure Portal](https://portal.azure.com/) oturum açarak ve sol taraftaki menüdeki **abonelikler** SEKMESINE giderek Azure abonelik kimliğinizi bulabilirsiniz. Sekmeyi seçtiğinizde, abonelik KIMLIĞINIZ görüntülenir (örneğin, "a83645ac-1234-5AB6-345-1h234g764ghty").

- **Azure AD KIRACı kimliği** (gerekli): Azure ACTIVE DIRECTORY (ad) [kiracı kimliğinizi](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)girin. Bu KIMLIĞI bulmak için [Azure Portal](https://portal.azure.com/)oturum açın, sol taraftaki menüden Active Directory sekmesini seçin, * * Özellikler ' i seçin, sonra LISTELENEN **dizin kimliği** numarasını (ör. 50c464d3-4930-494c-963c-1e951d15360e) arayın. Kuruluşunuzun kiracı KIMLIĞINI Ayrıca, etki alanı adı URL 'nizi kullanarak da arayabilirsiniz: [https://www.whatismytenantid.com](https://www.whatismytenantid.com).

- **Azure AD kiracı adı** (dinamik 365 için gereklidir): Azure ACTIVE DIRECTORY (ad) adınızı girin. Bu adı bulmak için sağ üst köşedeki [Azure Portal](https://portal.azure.com/)oturum açın, kiracı adınız hesap adınızın altında listelenecektir.

- **Azure AD uygulama kimliği** (gerekli): Azure ACTIVE DIRECTORY (ad) [uygulama kimliğinizi](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)girin. Bu KIMLIĞI bulmak için [Azure Portal](https://portal.azure.com/)oturum açın, sol menüden Active Directory sekmesini seçin, **uygulama kayıtları**' yı seçin, ardından listelenen **uygulama kimliği** numarasını (ör. 50c464d3-4930-494c-963c-1e951d15360e) arayın.

- **Azure AD uygulama istemci parolası** (gerekli): Azure AD uygulama [Istemci gizli](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#certificates-and-secrets)anahtarını girin. Bu değeri bulmak için [Azure Portal](https://portal.azure.com/)oturum açın. Sol menüdeki **Azure Active Directory** sekmesini seçin, **uygulama kayıtları**' i seçin ve ardından test sürücüsü uygulamanızı seçin. Sonra **Sertifikalar ve gizlilikler**' ı seçin, **yeni istemci parolası**' nı seçin, bir açıklama girin, **süresi dolmasın**' ı seçin ve ardından **Ekle**' yi Değeri kopyalamadığınızdan emin olun. (Değeri kopyalamadan önce sayfadan uzaklaşmayın, aksi takdirde değere erişemezsiniz.)

Sonraki bölüme geçmeden önce **kaydetmeyi** unutmayın!

### <a name="test-drive-listings-optional"></a>Sürücü listelerini test etme (isteğe bağlı)

**Test sürücüsü sekmesinde bulunan** **Test sürücü listeleri** seçeneği, test sürücünüzün kullanılabildiği dilleri (ve pazarlar) görüntüler; şu anda İngilizce (Birleşik Devletler) kullanılabilir tek konumdur. Ayrıca, Bu sayfa dile özgü listenin durumunu ve eklendiği tarih/saati görüntüler. Her dil/Pazar için test sürücüsü ayrıntılarını (Açıklama, Kullanıcı el ile, videolar vb.) tanımlamanız gerekir.

- **Açıklama** (gerekli): test sürücünüzü, neyi gösterdiklerinizi, kullanıcının deneme amaçlarını, araştırılacak özellikleri ve kullanıcının teklifinizi elde edip etmeyeceğinizi belirlemesine yardımcı olmak için Ilgili bilgileri tanıtın. Bu alana en fazla 3.000 karakter metin girilebilir. 

- **Erişim bilgileri** (Azure Resource Manager ve mantıksal test sürücüleri için gereklidir): Bu test sürücüsüne erişmek ve kullanmak için müşterinin neleri bilmesi gerektiğini açıklayın. Teklifinizi kullanmaya yönelik bir senaryoya ve müşterinin test sürüşü genelinde özelliklere erişmek için bilmeleri gereken tam olarak ne olduğunu öğrenin. Bu alana en fazla 10.000 karakter metin girilebilir.

- **Kullanıcı el ile** (gerekli): test sürücünüzün deneyiminizin derinlemesine bir yolu. Kullanıcı el kitabı, müşterinin test sürücüsüyle karşılaşmasını istediğiniz şekilde tam olarak kapsamalıdır ve sahip olabileceği sorulara başvuru olarak görev yapar. Dosya, karşıya yüklendikten sonra PDF biçiminde ve adlandırılmalıdır (en fazla 255 karakter) olmalıdır.

- **Videolar: video ekleme** (isteğe bağlı): videolar, YouTube veya Vimeo 'a yüklenebilir ve burada bir bağlantı ve küçük resim görüntüsü (533 x 324 piksel) ile birlikte başvuruda bulunabilir. böylece, bir müşterinin test sürücüsünü daha iyi anlamasına yardımcı olmak için, teklifin özelliklerini nasıl kullanacağınızla ilgili bilgi ve avantajları vurgulayan senaryolar hakkında bilgi
  - **Ad** (gerekli)
  - **URL (yalnızca YouTube veya Vimeo)** (gerekli)
  - **Küçük resim (533 x 324 piksel)** : görüntü dosyası png biçiminde olmalıdır.

Bu alanları tamamladıktan sonra **Kaydet** ' i seçin.

## <a name="publish"></a>Yayımla

### <a name="submit-offer-to-preview"></a>Önizlemeye teklif Gönder

Teklifin tüm gerekli bölümlerini tamamladıktan sonra portalın sağ üst köşesinde **Yayımla** ' yı seçin. **İnceleme ve yayımlama** sayfasına yönlendirilirsiniz. 

Bu teklifi ilk kez yayımlıyorsanız şunları yapabilirsiniz:

- Teklifin her bölümü için tamamlanma durumuna bakın.
    - *Başlatılmamış* -bölüm dokunulmamış ve tamamlanması gereken anlamına gelir.
    - *Tamamlanmamış* -bölümde düzeltilmesi gereken hatalar olduğu veya daha fazla bilgi sağlanması gerekir. Lütfen bölüm (ler) e geri dönün ve güncelleştirin.
    - *Tam* -bölümün tamamlandığı, tüm gerekli verilerin sağlandığı ve hata olmadığı anlamına gelir. Teklifi gönderebilmeniz için teklifin tüm bölümlerinin tamamen bir durumda olması gerekir.
- Uygulamanızı anlamak için yararlı olan tüm ek notlara ek olarak, uygulamanızın doğru şekilde test edilmesini sağlamak için sertifika ekibine test yönergeleri sağlayın.
- **Gönder**' i seçerek teklifi yayımlamaya gönderebilirsiniz. Size, gözden geçirmeniz ve onaylamanız için teklifin bir önizleme sürümünün ne zaman kullanılabileceğini bilmenizi sağlayacak bir e-posta göndereceğiz. Iş Ortağı Merkezi 'ne dönün ve teklifinizi herkese açık bir şekilde (veya özel bir teklif olarak özel hedef kitleye) yayımlama teklifi için **Go-Live** ' ı seçin.

### <a name="errors-and-review-feedback"></a>Hatalar ve gözden geçirme geri bildirimi

Yayımlama işlemindeki **el ile doğrulama** adımı, teklifinizin ve ilişkili teknik varlıklarının (özellikle Azure Resource Manager şablonu) kapsamlı bir incelemesini temsil eder, sorunlar genellikle çekme ISTEğI (PR) bağlantıları olarak sunulur. Bu PR 'ler nasıl görüntüleneceği ve yanıtlanmasına ilişkin bir açıklama, bkz. [İnceleme geri bildirimini işleme](./azure-apps-review-feedback.md).

Yayımlama adımlarında bir veya daha fazla hatayla karşılaşırsanız, bunları düzeltmeniz ve teklifinizi yeniden yayımlamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Ticari Market'te bulunan bir teklifi güncelleştirme](./update-existing-offer.md)
