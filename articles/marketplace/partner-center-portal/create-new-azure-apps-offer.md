---
title: Ticari Pazar'da yeni bir Azure Uygulamaları teklifi oluşturun
description: Microsoft İş Ortağı Merkezi'ndeki Ticari Pazar Portalı'nı kullanarak Azure Marketi, AppSource veya Bulut Çözüm Sağlayıcısı (CSP) programı aracılığıyla listeleme veya satış için yeni bir Azure Apps teklifi oluşturma.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 11/21/2019
ms.openlocfilehash: 0ff1bbd976273a7d0cbfb22effebdf45c84d2f41
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80277253"
---
# <a name="create-an-azure-application-offer"></a>Azure uygulama teklifi oluşturma

Azure uygulama teklifini ticari pazarda yayımlama adımları burada özetlenmiştir.

## <a name="azure-application-offer-type"></a>Azure uygulama teklif türü

Bu konu, Azure uygulama teklifleri ile ilgili temel konuları özetler.  Market'te yeni bir Azure uygulama teklifi yayımlama işlemine başlamadan önce bu kavramlara aşina olmalısınız.

### <a name="publishing-overview"></a>Yayımlama genel bakış

Video [Oluşturma Çözümü Şablonları ve Azure Marketi için Yönetilen Uygulamalar,](https://channel9.msdn.com/Events/Build/2018/BRK3603) Azure uygulama teklif türüne bir giriştir:

* Hangi teklif türleri mevcuttur;
* Hangi teknik varlıklar gereklidir;
* Azure Kaynak Yöneticisi şablonu nasıl yazar;
* Uygulama UI'sinin geliştirilmesi ve test edilmesi;
* Uygulama teklifi nasıl yayınlanır;
* Uygulama gözden geçirme süreci.

### <a name="types-of-azure-application-plans"></a>Azure uygulama planları türleri

İki tür Azure uygulama planı, yönetilen uygulamalar ve çözüm şablonları vardır.

* **Çözüm şablonu,** Çözüm'ü Market'te yayımlamanın ana yollarından biridir. Bu plan türü, çözümünüz tek bir sanal makinenin (VM) ötesinde ek dağıtım ve yapılandırma otomasyonu gerektirdiğinde kullanılır.  Çözüm şablonuyla, karmaşık IaaS çözümleri sağlamak için VM'ler, ağ oluşturma ve depolama kaynakları da dahil olmak üzere birden fazla kaynağıotomatikleştirebilirsiniz.  Çözüm şablonları oluşturma hakkında daha fazla bilgi için [Azure Kaynak Yöneticisi](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) belgelerine bakın.

* **Yönetilen uygulama,** tek bir önemli farkla çözüm şablonlarına benzer. Yönetilen bir uygulamada kaynaklar, uygulamanın yayımcısı tarafından yönetilen bir kaynak grubuna dağıtılır. Kaynak grubu, tüketicinin aboneliğinde mevcuttur ancak yayımcının kiracısındaki bir kimlik, kaynak grubuna erişime sahiptir. Yayımcı olarak, çözümün sürekli desteği için maliyeti belirtirsiniz. Müşterilerinize tam olarak yönetilen anahtar teslim uygulamalar oluşturmak ve sunmak için Yönetilen uygulamaları kullanın.  Yönetilen uygulamaların avantajları ve türleri hakkında daha fazla bilgi için [Azure yönetilen uygulamalara genel bakış](https://docs.microsoft.com/azure/managed-applications/overview)bölümüne bakın.

Tüm Azure uygulamaları, arşivin `.zip` kök klasöründe en az iki dosya içerir:

* [mainTemplate.json](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)adlı bir Kaynak Yöneticisi şablon dosyası .  Bu şablon, müşterinin Azure aboneliğinde dağıtılacak kaynakları tanımlayan şablondur.  Kaynak Yöneticisi şablonları örnekleri için Azure [Hızlı Başlangıç Şablonları galerisine](https://azure.microsoft.com/resources/templates/) veya ilgili [GitHub: Azure Kaynak Yöneticisi Hızlı Başlangıç Şablonları](https://github.com/azure/azure-quickstart-templates) repo'ya bakın.

* [createUiDefinition.json](https://docs.microsoft.com/azure/managed-applications/create-uidefinition-overview)adlı Azure uygulama oluşturma deneyimi için kullanıcı arabirimi tanımı.  Kullanıcı arayüzünde tüketicilerin parametre değerleri sağlamasına olanak tanıyan öğeleri belirlersiniz.

Tüm yeni Azure uygulama teklifleri bir [Azure iş ortağı müşteri kullanımı atıf GUID](https://docs.microsoft.com/azure/marketplace/azure-partner-customer-usage-attribution)içermelidir.

### <a name="before-you-begin"></a>Başlamadan önce

Hızlı Başlangıçlar, Öğreticiler ve Örnekler sağlayan aşağıdaki Azure uygulama belgelerini gözden geçirin.

* [Azure Kaynak Yöneticisi Şablonlarını Anlama](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)
* Hızlı Başlangıçlar:

    * [Azure Quickstart şablonları](https://azure.microsoft.com/documentation/templates/)
    * [GitHub Azure Quickstart şablonları](https://github.com/azure/azure-quickstart-templates)
    * [Uygulama tanımını yayımlama](https://docs.microsoft.com/azure/managed-applications/publish-managed-app-definition-quickstart)
    * [Hizmet kataloğu uygulaması dağıtma](https://docs.microsoft.com/azure/managed-applications/deploy-service-catalog-quickstart)

* Öğreticiler:

    * [Tanım dosyaları oluşturma](https://docs.microsoft.com/azure/managed-applications/publish-service-catalog-app)
    * [Market uygulaması yayımlama](https://docs.microsoft.com/azure/managed-applications/publish-marketplace-app)

* Örnekleri:

    * [Azure CLI](https://docs.microsoft.com/azure/managed-applications/cli-samples)
    * [Azure PowerShell](https://docs.microsoft.com/azure/managed-applications/powershell-samples)
    * [Yönetilen uygulama çözümleri](https://docs.microsoft.com/azure/managed-applications/sample-projects)

### <a name="fundamentals-in-technical-knowledge"></a>Teknik bilginin temelleri

Bu varlıkların tasarlanması, oluşturulması ve test edilmesi zaman alır ve hem Azure platformu hem de teklifi oluşturmak için kullanılan teknolojiler hakkında teknik bilgi gerektirir.

Mühendislik ekibiniz aşağıdaki Microsoft teknolojileri hakkında bilgi sahibi olmalıdır:

* [Azure Hizmetlerinin](https://azure.microsoft.com/services/)temel anlayışı.
* Azure uygulamaları nasıl [tasarlar ve tasarlar.](https://azure.microsoft.com/solutions/architecture/)
* [Azure Sanal Makineler,](https://azure.microsoft.com/services/virtual-machines/) [Azure Depolama](https://azure.microsoft.com/services/?filter=storage#storage)ve [Azure Ağı'nın](https://azure.microsoft.com/services/?filter=networking#networking)çalışma bilgileri.
* [Azure Kaynak Yöneticisi'nin](https://azure.microsoft.com/features/resource-manager/)çalışma bilgisi.
* [JSON](https://www.json.org/)çalışma bilgisi .

### <a name="suggested-tools"></a>Önerilen araçlar

Azure uygulamanızı yönetmenize yardımcı olmak için aşağıdaki komut dosyası ortamlarından birini veya her ikisini seçin:

* [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)
* [Azure CLI](https://docs.microsoft.com/cli/azure)

Geliştirme ortamınıza aşağıdaki araçları eklemenizi öneririz:

* [Azure Storage Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer)
* Aşağıdaki uzantıları ile [Görsel Studio Kodu:](https://code.visualstudio.com/)
    * Uzantı: [Azure Kaynak Yöneticisi Araçları](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools)
    * Uzantısı: [Güzelleştirmek](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
    * Uzatma: [Prettify JSON](https://marketplace.visualstudio.com/items?itemName=mohsen1.prettify-json)

Kullanılabilir araçları Azure Geliştirici [Araçları](https://azure.microsoft.com/tools/) sayfasında inceleyebilirsiniz.  Ayrıca Visual Studio kullanıyorsanız, [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

## <a name="create-an-azure-application-offer"></a>Azure uygulama teklifi oluşturma

Azure uygulama teklifi oluşturmadan önce, öncelikle [bir İş Ortağı Merkezi hesabı oluşturmanız](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account) ve Seçili Genel **Bakış** sekmesiyle Ticari [Pazar panosunu](https://partner.microsoft.com/dashboard/commercial-marketplace/offers)açmanız gerekir.

>[!Note]
>Bir teklif yayımlandıktan sonra, Ortak Merkezi'nde yapılan teklife yönelik yapılan lar yalnızca yeniden yayımlandıktan sonra sistemde ve vitrinlerde güncellenir.  Lütfen değişiklik yapıldıktan sonra teklifi yayınlanmak üzere gönderdiğinden emin olun.

### <a name="create-a-new-offer"></a>Yeni teklif oluşturma

+ **Yeni teklif** düğmesini seçin ve ardından **Azure Uygulaması** menü öğesini seçin. **Yeni teklif** iletişim kutusu görüntülenir.

### <a name="offer-id-and-alias"></a>Teklif kimliği ve takma ad

* **Teklif Kimliği**: Hesabınızdaki her teklif için benzersiz bir tanımlayıcı. Bu kimlik, pazar teklifi ve Azure Kaynak Yöneticisi şablonlarının (varsa) URL adresindeki müşteriler tarafından görülebilir. <br> <br> Teklif Kimliğiniz daha küçük alfasayısal karakterler olmalıdır (tireler ve alt çizerler dahil, ancak beyaz boşluk yoktur). 50 karakterle sınırlıdır ve Oluştur'u seçtikten sonra değiştirilemez. <br> <br> Örneğin, buraya girerseniz, `test-offer-1` teklif URL'si . `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1` 

* **Teklif diğer adı**: Ortak Merkezi'ndeki teklife atıfta bulunmak için kullanılan ad. Bu ad pazarda kullanılmaz ve teklif adından ve müşterilere gösterilecek diğer değerlerden farklıdır. **Oluştur'u**seçtikten sonra bu değer değiştirilemez.

**Teklif Kimliği** nizi ve **Teklif takma**adınıgirdikten sonra **Oluştur'u**seçin. Daha sonra teklifinizin diğer tüm kısımları üzerinde çalışabileceksiniz.

## <a name="offer-setup"></a>Teklif kurulumu

**Teklif kurulum** sayfası aşağıdaki bilgileri ister. Bu alanları tamamladıktan sonra **Kaydet'i** seçtiğinizden emin olun.

### <a name="test-drive"></a>Test sürüşü

Test sürüşü, potansiyel müşterilere teklifinizi 'satın almadan önce deneme' seçeneği sunarak, dönüşümün artması ve yüksek nitelikli müşteri adaylarının üretilmesiyle sonuçlanan harika bir yoldur. [Test sürücüleri hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)

Test sürüşü etkinleştirmek **için, test sürüşü etkinleştir** kutusunu işaretleyin. Daha sonra, müşterilerin teklifinizi belirli bir süre boyunca denemesine izin verecek şekilde Test [sürücüsü teknik yapılandırmasında](#types-of-azure-application-plans) bir gösteri ortamı yapılandırmanız gerekir. 

>[!Note]
>Tüm Azure uygulamaları Azure Kaynak Yöneticisi şablonu kullanılarak uygulandığından, Azure Uygulaması için yapılandırılabilen tek test sürücüsü türü [Azure Kaynak Yöneticisi tabanlı test sürücüsüdür.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive)

#### <a name="additional-test-drive-resources"></a>Ek test sürüşü kaynakları

- [Test Sürüşü Teknik En İyi Uygulamalar](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [Test Drive Pazarlama En İyi Uygulamalar](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [Test Sürüşüne Genel Bakış Bir Çağrı Cihazı](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf)

## <a name="connect-lead-management"></a>Müşteri adayı yönetimini bağlayın

[!INCLUDE [Connect lead management](./includes/connect-lead-management.md)]

Daha fazla bilgi için Müşteri [Adayı yönetimine genel bakış](./commercial-marketplace-get-customer-leads.md)adabına bakın.

Bir sonraki bölüme geçmeden önce **Kaydet'i** unutmayın!

## <a name="properties"></a>Özellikler

**Özellikler** sayfası, teklifinizi pazarda, uygulama sürümünüzde ve teklifinizi destekleyen yasal sözleşmelerde gruplandırmak için kullanılan kategorileri ve sektörleri tanımlamanıza olanak tanır. Bu sayfayı tamamladıktan sonra **Kaydet'i** seçin.

### <a name="category"></a>Kategori

Teklifinizi uygun pazar yeri arama alanlarına yerleştirmek için kullanılacak en az bir ve en fazla üç kategori seçin. Teklifinizin teklif açıklamasında bu kategorileri nasıl desteklediğini mutlaka unutmayın. 

### <a name="standard-contract-for-the-microsoft-commercial-marketplace"></a>Microsoft ticari pazar için Standart Sözleşme

[!INCLUDE [Commercial marketplace Standard Contract](./includes/marketplace-contract-text.md)]

## <a name="offer-listing"></a>Teklif listesi

Teklif listesi sayfası, teklifinizin listeleneceği dilleri görüntüler. Şu anda, **İngilizce (AMERIKA Birleşik Devletleri)** tek seçenektir.

Her dil/pazar için pazar ayrıntıları (teklif adı, açıklama, resim, vb.) tanımlamanız gerekir. Bu bilgileri sağlamak için dil/pazar adını seçin.

> [!NOTE]
> Teklif listesi içeriği (açıklama, belge, ekran görüntüsü, kullanım koşulları, vb.) "Bu uygulama yalnızca [İngilizce olmayan dilde] ifadesiyle başladığı sürece İngilizce olması gerekmez." Teklif listesi içeriğinde kullanılandan başka bir dilde içerik sunmak için yararlı bir *bağlantı URL'si* sağlamak da kabul edilebilir.

### <a name="name"></a>Adı

Buraya girdiğiniz ad, teklif listenizin başlığı olarak müşterilere gösterilir. Bu alan, teklifi oluşturduğunuzda **Teklif takma adı** için girdiğiniz metinle önceden doldurulmuştur, ancak bu değeri değiştirebilirsiniz. Bu ad ticari marka olabilir (ve ticari marka veya telif hakkı sembolleri içerebilir). Ad 50 karakterden fazla olamaz ve herhangi bir emoji içeremez.

### <a name="summary"></a>Özet

Teklifinizin kısa bir açıklamasını (en fazla 100 karakter) sağlayın ve bu açıklamanızı pazar yeri arama sonuçlarında da kullanılabilir.

### <a name="long-summary"></a>Uzun özet

Teklifinizin daha uzun bir açıklamasını sağlayın (en fazla 256 karakter). Açıklama, pazar yeri arama sonuçlarında kullanılabilir.

### <a name="description"></a>Açıklama

Teklifinizin daha uzun bir açıklamasını sağlayın (en fazla 3.000 karakter). Bu açıklama, pazar yeri giriş özetinde müşterilere görüntülenir. Teklifinizin değer teklifini, temel avantajlarını, kategorive/veya endüstri derneklerini, uygulama içi satın alma fırsatlarını ve gerekli açıklamaları ekleyin. 

Açıklamanızı yazmak için bazı ipuçları:  

- Teklifinizin değer teklifini açıklamanızın ilk birkaç cümlesinde açıkça açıklayın. Değer teklifinize aşağıdaki öğeleri ekleyin:
  - Ürünün tanımı
  - Üründen yararlanan kullanıcı türü
  - Ürünün hitap ettiği müşteri ihtiyaçları veya acısı
- İlk birkaç cümlenin arama motoru sonuçlarında görüntülenebileceğini unutmayın.  
- Ürününüzü satmak için özelliklere ve işlevselliğe güvenmeyin. Bunun yerine, sağladığınız değere odaklanın.  
- Sektöre özgü kelime dağarcığı veya fayda tabanlı ifadeler mümkün olduğunca kullanın. 
- Açıklamanızı biçimlendirmek ve daha ilgi çekici hale getirmek için HTML etiketlerini kullanmayı düşünün.

### <a name="search-keywords"></a>Anahtar kelimeleri ara

Müşterilerin teklifinizi pazarda bulmasına yardımcı olmak için isteğe bağlı olarak en fazla üç arama anahtar kelimesini girebilirsiniz. En iyi sonuçlar için, bu anahtar kelimeleri açıklamanızda da kullanmayı deneyin.

### <a name="support-urls"></a>Destek URL'leri

Bu bölüm, müşterilerin teklifiniz hakkında daha fazla bilgi edinmelerine yardımcı olacak bağlantılar sağlamanıza olanak tanır.

#### <a name="privacy-policy-url"></a>Gizlilik ilkesi URL'si

Kuruluşunuzun gizlilik politikasına URL girin. Uygulamanızın gizlilik yasalarına ve yönetmeliklerine uygun olmasını sağlamaktan ve geçerli bir gizlilik politikası sağlamaktan siz sorumlusunuz.

#### <a name="useful-links"></a>Yararlı bağlantılar

Çözümünüz le ilgili isteğe bağlı tamamlayıcı çevrimiçi belgeler sağlayın.  + **Link ekle'ye**tıklayarak ek yararlı bağlantılar ekleyin.

### <a name="contacts"></a>Kişiler

Bu bölümde, bir **Destek kişi** ve mühendislik **kişi**için ad, e-posta ve telefon numarası sağlamanız gerekir. Bu bilgiler müşterilere gösterilmez, ancak Microsoft tarafından kullanılabilir ve CSP iş ortaklarına sağlanabilir.

Destek **iletişim** bölümünde, CSP iş ortaklarının teklifiniz için destek bulabilecekleri **Destek URL'sini** de sağlamanız gerekir.

### <a name="marketplace-images"></a>Pazar yeri görüntüleri

Bu bölümde, teklifinizi müşteriye gösterirken kullanılacak logolar ve resimler sağlayabilirsiniz. Tüm görüntüler .png formatında olmalıdır.

#### <a name="store-logos"></a>Mağaza logoları

Teklifinizin logosunu üç boyutta sağlayın: **Küçük (48 x 48)**, **Orta (90 X 90)** ve **Büyük (216 x 216).**

#### <a name="hero"></a>Kahraman

Kahraman görüntüsü isteğe bağlıdır. Bir tane sağlarsanız, 815 x 290 piksel ölçmeniz gerekir.

#### <a name="screenshots"></a>Ekran görüntüleri

Teklifinizin nasıl çalıştığını gösteren ekran görüntüleri ekleyin. En fazla beş ekran görüntüsü ekleyebilirsiniz. Tüm ekran görüntüleri 1280 x 720 piksel olmalıdır.

#### <a name="videos"></a>Videolar

İsteğe bağlı olarak, teklifinizi gösteren en fazla beş video ekleyebilirsiniz. Bu videolar YouTube ve/veya Vimeo'da barındırılmalıdır. Her biri için videonun adını, URL'sini ve videonun küçük resmini (1280 x 720 piksel) girin.

#### <a name="additional-marketplace-listing-resources"></a>Ek pazar yeri listeleme kaynakları

- [Pazar teklifi listeleri için en iyi uygulamalar](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)

## <a name="preview"></a>Önizleme

**Önizleme** sekmesi, teklifinizi daha geniş pazar kitleye canlı olarak yayınlamadan önce teklifinizi doğrulamak için sınırlı bir **Önizleme Hedef Kitlesi** tanımlamanıza olanak tanır.

> [!IMPORTANT]
> Teklifiniz Önizleme'de teklifinizi kontrol ettikten sonra pazardaki herkese açık izleyicilere canlı olarak yayınlanmadan önce **Canlı Yayınla'yı** seçmelisiniz.

Önizleme hedef kitleniz, her biri için isteğe bağlı bir açıklamayla eşleştirilmiş Azure abonelik kimliği GUID'leri tarafından tanımlanır.  Bu alanların hiçbiri müşteriler tarafından görülemez.

El ile en fazla 10 Azure abonelik tonu veya bir CSV dosyası yüklerken 100'e kadar ekleme.  Bu abonelikleri ekleyerek, teklifiniz tam olarak yayınlanmadan önce önizleme erişimine izin verilecek bir hedef kitle tanımlıyorsunuz.  Teklifiniz zaten yayındaysa, teklifinizdeki değişiklikleri veya güncellemeleri test etmek için bir önizleme hedef kitlesi tanımlayabilirsiniz.

>[!Note]
>Önizleme kitlesi özel bir hedef kitleden farklıdır. Önizleme kitlesi, pazarlarda canlı olarak yayınlanmadan *önce* teklifinize erişebilir. Ayrıca, bir plan oluşturmayı ve planı yalnızca özel bir hedef kitle için kullanılabilir hale getirmeyi de seçebilirsiniz (Plan Kullanılabilirlik sekmesini kullanarak).  Önizleme kitleniz, teklifiniz pazaryerinde tam olarak yayınlandığında yalnızca özel bir hedef kitletarafından kullanılabilecek planlar da dahil olmak üzere tüm planları görebilir ve doğrulayacaktır.

## <a name="plan-overview"></a>Plana genel bakış

**Plan'a genel bakış** sekmesi, aynı teklif içinde farklı plan seçenekleri sağlamanıza olanak tanır. Bu planlar (Bulut İş Ortağı Portalında SCO olarak anılacaktır) plan türü (çözüm şablonu yönetilen uygulamaya karşı yönetilen uygulama), para kazanma veya hedef kitle açısından farklılık görebilir.  Teklifinizi pazarda listelemek için en az bir planı yapılandırın.

Oluşturulduktan sonra, plan adlarınızı, tünüzü, plan türünü, kullanılabilirliği (Genel veya Özel), geçerli yayımlama durumunu ve bu sekmedeki kullanılabilir eylemleri görürsünüz.

Plan'a **genel bakışta** bulunan **eylemler,** planınızın geçerli durumuna bağlı olarak değişir ve şunları içerebilir:

* Plan durumu **Taslak** ise – Taslağı silin.
* Plan durumu **Canlı** ise – Satış planını durdurun veya özel hedef kitleyi eşitle.

### <a name="create-new-plan"></a>Yeni plan oluşturma

***Plan ID*** - Bu teklifteki her plan için benzersiz bir plan kimliği oluşturun. Bu kimlik, ürün URL'sinde müşteriler tarafından görülebilir.  Yalnızca küçük, alfasayısal karakterleri, tireleri veya alt çizgilerini kullanın. Bu plan kimliği için en fazla 50 karaktere izin verilir. Bu kimlik, oluştur'u seçtikten sonra değiştirilemez.

***Plan adı*** - Müşteriler teklifiniz içinde hangi planı seçeceğinize karar verirken bu adı görür. Bu teklifteki her plan için benzersiz bir teklif adı oluşturun. Plan adı, aynı teklifin bir parçası olabilecek yazılım planlarını ayırt etmek için kullanılır (örn. Teklif adı: Windows Server; planlar: Windows Server 2016, Windows Server 2019).

### <a name="plan-setup"></a>Kurulum planı

**Plan kurulumu** sekmesi, plan türüiçin üst düzey yapılandırmayı, paketleri başka bir plandan yeniden kullanıp kullanmadığını ve planın hangi bulutlarda bulunması gerektiğini ayarlamanızı sağlar.  Bu sekmedeki yanıtlarınız, aynı plan için diğer sekmelerde hangi alanların görüntüleneceğini etkiler.

#### <a name="plan-type"></a>Plan türü

Azure uygulama [planları türlerinde](#types-of-azure-application-plans)belirtildiği gibi, planınızın bir çözüm şablonu mu yoksa yönetilen bir uygulama mı içereceğini seçin.

#### <a name="this-plan-reuses-packages"></a>Bu plan paketleri yeniden kullanır

Aynı türden birden fazla planınız varsa ve paketler aralarında aynıysa, **bu planı başka bir plandan paketleri yeniden kullanır'ı**seçebilirsiniz.  Bu seçeneği seçtiğinizde, paketleri yeniden kullanmak için bu teklif için aynı türdeki diğer planlardan birini seçebilirsiniz. 

>[!Note]
>Paketleri başka bir plandan yeniden kullandığınızda, tüm Teknik yapılandırma sekmesi bu plandan kaybolur.  Gelecekte yaptığınız güncelleştirmeler de dahil olmak üzere diğer plandaki Teknik yapılandırma ayrıntıları da bu plan için kullanılacaktır. <br> <br> Ayrıca, bu plan yayımlandıktan sonra bu ayar değiştirilemez.

#### <a name="cloud-availability"></a>Bulut kullanılabilirliği

Bu plan en az bir bulutta kullanılabilir hale getirilmelidir. 

Çözümünüzü, Market tümleştirmesi olan tüm ortak Azure bölgelerindeki müşteriler için dağıtılabilir hale getirmek için **Herkese Açık Azure** seçeneğini seçin.  [Coğrafi kullanılabilirlik](https://docs.microsoft.com/azure/marketplace/marketplace-geo-availability-currencies)hakkında daha fazla bilgi edinin.

Çözümünüzü, ABD Federal, Eyalet, yerel veya bu kuruluşlardan müşteriler için kontrollü erişime sahip bir devlet topluluğu bulutu olan [Azure Kamu Bulutu'nda](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome)dağıtılabilir hale getirmek için Azure Kamu **Bulutu** seçeneğini belirleyin.  Yayıncı olarak, bu bulut topluluğuna hizmet etmek için tüm uyumluluk denetimlerinden, güvenlik önlemlerinden ve en iyi uygulamalardan siz sorumlusunuz.  Azure Kamu fiziksel olarak yalıtılmış veri merkezlerini ve ağlarını kullanır (yalnızca ABD'de bulunur).  Azure [Hükümeti'nde](https://aka.ms/azuregovpublish)yayımlamadan önce Microsoft, belirli uç noktalar farklı olabileceğinden çözümünüzü ortamda sınayın ve doğrulamanızı önerir. Çözümünüzü aşamalı olarak düzenlemek ve test etmek için bu [bağlantıdan](https://azure.microsoft.com/global-infrastructure/government/request/)bir deneme hesabı isteyin.

>[!Note]
>Bir plan belirli bir bulutta kullanılabilir olarak yayımlandıktan sonra, bu bulut kaldırılamaz.

**Azure Kamu Bulutu sertifikaları**

Bu seçenek yalnızca **Azure Kamu Bulutu** **Bulut kullanılabilirliği**altında seçilirse görünür.

Azure Devlet hizmetleri, FedRAMP, NIST 800.171 (DIB), ITAR, IRS 1075, DoD L4 ve CJIS gibi belirli devlet yönetmeliklerine ve gereksinimlerine tabi olan verileri işler.  Bu programlar için sertifikalarınıza farkındalık getirmek için, sertifikalarınızı açıklayan en fazla 100 bağlantı sağlayabilirsiniz.  Bu bağlantılar, doğrudan programdaki girişinize bağlantılar veya kendi web sitelerinizde bunlarla uyumluluğunun açıklamalarına bağlantılar olabilir.  Bu bağlantılar yalnızca Azure Kamu Bulutu müşterileri tarafından görülebilir.

## <a name="plan-listing"></a>Plan listesi

**Plan listeleme** sekmesi, aynı teklifiçin farklı planlar arasında farklı olabilecek plana özgü listeleme bilgilerini görüntüler.

### <a name="name"></a>Adı

Planınızı oluşturduğunuzda atadığınız ada göre önceden doldurulmuştur.  Bu ad, pazarda görüntülenen bu "Yazılım planının" başlığı olarak görünür.  En fazla 100 karakter içerebilir.

### <a name="summary"></a>Özet

Yazılım planınızın kısa bir özetini sağlayın.  En fazla 100 karakter içerebilir.

### <a name="description"></a>Açıklama

Bu açıklama, bu yazılım planını benzersiz kılan şeyleri ve teklifiniz dahilindeki diğer yazılım planlarından herhangi bir farkı açıklamak için bir fırsattır. 2.000 karaktere kadar olabilir.

Bu alanları tamamladıktan sonra **Kaydet'i** seçin.

## <a name="availability"></a>Kullanılabilirlik

**Kullanılabilirlik** sekmesi yalnızca çözüm şablonu planları için görünür.  Planı yalnızca belirli müşteriler (özel bir hedef kitle) ve planın yalnızca diğer çözüm şablonu veya yönetilen uygulamalar tarafından kullanılmak üzere gizli hale getirip getirmeyeceğiniz herkes tarafından görülebilir hale getirebilirsiniz.

### <a name="plan-audience"></a>Hedef Kitleyi Planla

Her planı herkes tarafından görülebilen veya yalnızca seçtiğiniz belirli bir hedef kitleye göre yapılandırma seçeneğiniz var. Azure abonelik teşekklerini kullanarak bu kısıtlı hedef kitleye üyelik atayabilirsiniz.

**Gizlilik / Bu özel bir plandır** (İsteğe bağlı onay kutusu) - Planınızı yalnızca seçtiğiniz kısıtlı hedef kitleye özel ve görünür hale getirmek için bu kutuyu işaretleyin. Özel bir plan olarak yayımlandıktan sonra, hedef kitleyi güncelleyebilir veya planı herkesin kullanımına sunmayı seçebilirsiniz. Bir plan herkes tarafından görülebilen olarak yayımlandıktan sonra, herkes tarafından görülebilir olarak kalmalıdır. (Plan yeniden özel plan olarak yapılandırılamaz).

**Kısıtlı Hedef Kitle (Azure abonelik teşekali)** - Bu özel plana erişimi olan hedef kitleyi atayın. Access, atanan her Azure abonelik kimliğinin açıklamasını ekleme seçeneğiyle birlikte Azure abonelik kimlikleri kullanılarak atanır. Bir .csv elektronik tablo dosyası içe aktarıyorsanız en fazla 10 abonelik tonu veya 20.000 müşteri abonelik ii eklenebilir.  Azure abonelik teşekkülleri GUID olarak gösterilir ve harfler küçük harfle ifade edilmelidir.

>[!Note]
>Özel hedef kitle (veya kısıtlı hedef kitle) [**Önizleme**](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-saas-offer#preview) sekmesinde tanımladığınız önizleme hedef kitlesinden farklıdır.  Teklifin pazarda canlı olarak yayınlanmasından *önce* bir önizleme kitlesinin teklifinize erişmesine izin verilir. Özel hedef kitle ataması yalnızca belirli bir plan için geçerli olsa da, önizleme hedef kitlesi doğrulama amacıyla tüm planları (özel veya özel veya değil) görüntüleyebilir.

### <a name="hide-plan"></a>Planı gizle

Çözüm şablonunuz, başka bir çözüm şablonu veya yönetilen bir uygulama yla başvurulduğunda yalnızca dolaylı olarak dağıtılmak üzere tasarlanmışsa, çözüm şablonunuzu yayımlamak için bu kutuyu işaretleyin, ancak bu şablonu doğrudan arayan ve doğrudan gezen müşterilerden gizleyin.

## <a name="pricing-and-availability"></a>Fiyatlandırma ve kullanılabilirlik

**Fiyatlandırma ve kullanılabilirlik** sekmesi yalnızca yönetilen uygulama planları için görülebilir.  Bu planın mevcut olacağı pazarları, çözümün yönetiminin aylık fiyatını ve planı herkes veya yalnızca belirli müşteriler (özel bir hedef kitle) tarafından görünür hale getirip getirmeyeceğini görebilirsiniz.

### <a name="markets"></a>Piyasa

Her plan en az bir pazarda mevcut olmalıdır. Bu planı kullanıma açmak istediğiniz herhangi bir pazar konumu için onay kutusunu seçin. Microsoft'un satışları sizin adınıza havale ettiği ve vergi yi kullandığı "Vergi Havalesi" ülkeleri seçmek için bir arama kutusu ve düğmesi yardımcı olur.

Planınız için fiyatları Abd Doları (USD) olarak belirlediyseniz ve başka bir piyasa konumu eklediyseniz, yeni piyasa fiyatı geçerli döviz kurlarına göre hesaplanacaktır. Her zaman yayınlamadan önce her piyasa için fiyat gözden geçirin. Fiyatlandırma, değişikliklerinizi kaydettikten sonra "İhracat fiyatları (xlsx)" bağlantısı kullanılarak gözden geçirilebilir.

### <a name="pricing"></a>Fiyatlandırma

Bu plan için aylık fiyatı sağlayın.  Bu fiyat, bu çözüm tarafından dağıtılan kaynaklartarafından tahakkuk ettirilen herhangi bir Azure altyapısına veya istediğiniz kadar öde yazılım maliyetlerine ek olarak sunulur.

Yerel para birimi cinsinden belirlenen fiyatlar (USD = Abd Doları) kurulum sırasında mevcut mevcut döviz kurları kullanılarak seçilen tüm piyasaların yerel para birimine dönüştürülür. Fiyatlandırma elektronik tablosunu dışa aktararak ve her pazardaki fiyatı gözden geçirerek yayımlamadan önce bu fiyatları doğrulayın. Özel fiyatları tek bir pazarda ayarlamak istiyorsanız, fiyatlandırma elektronik tablosunu değiştirin ve içe aktarın. 

>[!Note]
>Fiyatlandırma verilerinin dışa aktarılmasını etkinleştirmek için öncelikle fiyatlandırma değişikliklerinizi kaydetmeniz gerekir.

Bir plan yayımlandıktan sonra nelerin değişebileceğine ilişkin bazı kısıtlamalar olduğundan, yayımlanmadan önce fiyatlarınızı dikkatle inceleyin.  

>[!Note]
>Planınızdaki bir piyasa nın fiyatı yayımlandıktan sonra değiştirilemez.

### <a name="plan-audience"></a>Hedef Kitleyi Planla

Her planı herkes tarafından görülebilen veya yalnızca seçtiğiniz belirli bir hedef kitleye göre yapılandırma seçeneğiniz var. Azure abonelik teşekklerini kullanarak bu kısıtlı hedef kitleye üyelik atayabilirsiniz.

**Gizlilik / Bu özel bir plandır** (İsteğe bağlı onay kutusu) - Planınızı yalnızca seçtiğiniz kısıtlı hedef kitleye özel ve görünür hale getirmek için bu kutuyu işaretleyin. Özel bir plan olarak yayımlandıktan sonra, hedef kitleyi güncelleyebilir veya planı herkesin kullanımına sunmayı seçebilirsiniz. Bir plan herkes tarafından görülebilen olarak yayımlandıktan sonra, herkes tarafından görülebilir olarak kalmalıdır. (Plan yeniden özel plan olarak yapılandırılamaz).

**Kısıtlı Hedef Kitle (Azure abonelik teşekali)** - Bu özel plana erişimi olan hedef kitleyi atayın. Access, atanan her Azure abonelik kimliğinin açıklamasını ekleme seçeneğiyle birlikte Azure abonelik kimlikleri kullanılarak atanır. Bir .csv elektronik tablo dosyası içe aktarıyorsanız en fazla 10 abonelik tonu veya 20.000 müşteri abonelik ii eklenebilir.  Azure abonelik teşekkülleri GUID olarak gösterilir ve harfler küçük harfle ifade edilmelidir.

>[!Note]
>Özel hedef kitle (veya kısıtlı hedef kitle) [**Önizleme**](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-saas-offer#preview) sekmesinde tanımladığınız önizleme hedef kitlesinden farklıdır.  Teklifin pazarda canlı olarak yayınlanmasından *önce* bir önizleme kitlesinin teklifinize erişmesine izin verilir. Özel hedef kitle ataması yalnızca belirli bir plan için geçerli olsa da, önizleme hedef kitlesi doğrulama amacıyla tüm planları (özel veya özel veya değil) görüntüleyebilir.

## <a name="technical-configuration"></a>Teknik yapılandırma 

**Teknik yapılandırma** sekmesi, müşterilerin planınızı dağıtmasına olanak tanıyan dağıtım paketini yüklemenize olanak tanır.

>[!Note]
>Bu planı **Plan kurulumu** sekmesindeki başka bir plandan paketleri yeniden kullanacak şekilde yapılandırırsanız bu sekme görünmez.

### <a name="package-details"></a>Paket detayları

**Paket ayrıntıları** alt sekmesi, teknik yapılandırmanızın taslak sürümünü düzenlemenize olanak tanır.

***Sürüm*** - Teknik yapılandırmanın geçerli sürümünü atayın.  Bu sayfada her değişiklik yayımladığınızda bu sürümü artım. Sürüm biçiminde `{integer}.{integer}.{integer}`olmalıdır.

***Paket*** dosyası`.zip`( ) - Bu paket, bu plan için gerekli olan tüm şablon `.zip` dosyalarının yanı sıra dosya olarak paketlenmiş ek kaynakları da içerir.

Tüm Azure uygulama planı paketleri, bu iki dosyayı bir `.zip` arşivin kök klasörüne içermelidir:

* [mainTemplate.json](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)adlı bir Kaynak Yöneticisi şablon dosyası .  Bu şablon, kaynakların müşterilerazure aboneliğine dağıtımını otomatikleştirir.  Kaynak Yöneticisi şablonları örnekleri için Azure [Hızlı Başlangıç Şablonları galerisine](https://azure.microsoft.com/documentation/templates/) veya ilgili [GitHub: Azure Kaynak Yöneticisi Hızlı Başlangıç Şablonları](https://github.com/azure/azure-quickstart-templates) repo'ya bakın.

* [createUiDefinition.json](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview)adlı Azure uygulama oluşturma deneyimi için kullanıcı arabirimi tanımı.

Tüm yeni Azure uygulama teklifleri, Azure [iş ortağı müşteri kullanımı atıf GUID'i](https://docs.microsoft.com/azure/marketplace/azure-partner-customer-usage-attribution) de içermelidir.

### <a name="previously-published-packages"></a>Daha önce yayınlanmış paketler 

**Önceden yayımlanmış paketler** alt sekmesi, teknik yapılandırmanızın tüm yayımlanmış sürümlerini görüntülemenizi sağlar.

## <a name="technical-configuration-managed-application-plans-only"></a>Teknik yapılandırma (yalnızca yönetilen uygulama planları)

Yönetilen uygulama planları, teknik **yapılandırma** sekmesinde yukarıda açıklanan **Sürüm** ve **Paket dosya** alanlarının ötesinde ek bir karmaşıklığa sahiptir. 

### <a name="enable-just-in-time-jit-access"></a>Tam zamanında (JIT) erişimi etkinleştirme

Bu plan için tam zamanında (JIT) erişimi etkinleştirmek için bu seçeneği belirleyin.  JIT erişimi, sorun giderme veya bakım için yönetilen bir uygulamanın kaynaklarına yüksek erişim isteğinde bulunmanızı sağlar. Kaynaklara her zaman salt okunur erişiminiz vardır, ancak belirli bir süre için daha fazla erişime sahip olabilirsiniz.  Daha fazla bilgi için Azure [Yönetilen Uygulamalar için Etkinleştir ve tam zamanında erişim isteğinde](https://docs.microsoft.com/azure/managed-applications/request-just-in-time-access)bulunun.  Yönetilen uygulamanızın tüketicilerinin hesabınıza kalıcı erişim vermesini sağlamak için, bu seçeneği işaretsiz bırakın.

>[!Note]
>Bu özelliği desteklemek `createUiDefinition.json` için dosyanızı güncelleştirdiğinden emin olun.  

### <a name="deployment-mode"></a>Dağıtım modu

Bu planı dağıtırken **Tam** veya **Artımlı dağıtım modunu** yapılandırıp yapılandırmayacağınızı seçin: 

* **Tam modda,** uygulamanın müşteri tarafından yeniden dağıtılması, kaynaklar tanımlanmamışsa yönetilen kaynak grubundaki `mainTemplate.json`kaynakların kaldırılmasına neden olur. 
* **Artımlı modda,** uygulamanın yeniden dağıtımı varolan kaynakları değiştirmeden bırakır.

Dağıtım modları hakkında daha fazla bilgi edinmek için [Azure Kaynak Yöneticisi dağıtım modlarına](https://docs.microsoft.com/azure/azure-resource-manager/deployment-modes)bakın.

### <a name="notification-endpoint-url"></a>Bildirim bitiş noktası URL'si

Bu plan sürümünün yönetilen uygulama örneklerindeki tüm CRUD işlemleri hakkında bildirimler almak için bir HTTPS Webhook bitiş noktası belirtin.

### <a name="customize-allowed-customer-actions"></a>İzin verilen müşteri eylemlerini özelleştirme

Varsayılan olarak kullanılabilir " "`*/read`eylemlere ek olarak, müşterilerin yönetilen kaynaklarda hangi eylemleri gerçekleştirebileceğini belirtmek için bu seçeneği belirleyin. 

Müşterinizin burada gerçekleştirmesini sağlamak istediğiniz ek eylemleri, yarı iki nokta ile ayrılmış olarak listele.  Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/role-based-access-control/deny-assignments)  Kullanılabilir eylemler için Azure [Kaynak Yöneticisi kaynak sağlayıcısı işlemlerine](https://docs.microsoft.com/azure/role-based-access-control/resource-provider-operations)bakın. Örneğin, tüketicilerin sanal makineleri yeniden başlatmasına `Microsoft.Compute/virtualMachines/restart/action` izin vermek için, izin verilen eylemlere ekleyin.

### <a name="global-azure--azure-government-cloud"></a>Global Azure / Azure Genel Bulutu

Desteklenen her bulutta yönetilen bu uygulamaya kimlerin yönetim erişimine sahip olması gerektiğini belirtin.  Yönetilen kaynak grubuna izin vermek istediğiniz kullanıcılar, gruplar veya uygulamalar Azure Etkin Dizin (AAD) kimlikleri kullanılarak tanımlanır.

***Azure Etkin Dizin Kiracı Kimliği*** - İzin vermek istediğiniz kullanıcıların, grupların veya uygulamaların kimliklerini içeren AAD Kiracı Kimliği (dizin kimliği olarak da bilinir).  AAD Kiracı Kimliğinizi Azure portalında, [Azure Etkin Dizin özelliklerinde](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Properties)bulabilirsiniz.

***Yetkilendirmeler*** - Yönetilen kaynak grubuna izin verilmesini istediğiniz kullanıcının, grubun veya uygulamanın Azure Etkin Dizin nesne kimliğini ekleyin. Kullanıcıyı, [Azure portalındaki Azure Active Directory kullanıcılarında](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UsersManagementMenuBlade/AllUsers)bulunan Asıl Kimliğiyle tanımlayın.

Her müdür için, listeden Azure AD yerleşik rollerinden birini (Sahibi veya Katılımcısı) seçin. Seçtiğiniz rol, asıl yöneticinin müşteri aboneliğindeki kaynaklar üzerinde sahip olacağı izinleri açıklar. Daha fazla bilgi için bkz. [Azure kaynakları için yerleşik roller](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).  Rol tabanlı erişim denetimi (RBAC) hakkında daha fazla bilgi için Azure [portalında RBAC ile başlayın'](https://docs.microsoft.com/azure/role-based-access-control/overview)a bakın.

>[!Note]
>Bulut başına en fazla 100 yetkilendirme ekleyebilirseniz de, Active Directory kullanıcı grubu oluşturmak ve kimliğini "Asıl Kimlik"te belirtmek genellikle daha kolaydır.  Bu, plan dağıtıldıktan sonra yönetim grubuna daha fazla kullanıcı eklemenize ve daha fazla yetkilendirme eklemek için planı güncelleştirme gereksinimini azaltmanıza olanak tanır.

### <a name="policy-settings"></a>İlke ayarları

Dağıtılan çözüm için uyumluluk gereksinimlerini belirtmek için yönetilen uygulamanıza [Azure İlkeleri](https://docs.microsoft.com/azure/governance/policy/overview) uygulayın.  İlke tanımları ve parametre değerlerinin biçimi için bkz. [Azure İlke Örnekleri](https://docs.microsoft.com/azure/governance/policy/samples/index).  En fazla beş ilke ve her İlke seçeneğinin yalnızca bir örneğini yapılandırabilirsiniz.  Bazı ilkeler ek parametreler gerektirir.  Standart SKU denetim politikaları için gereklidir.  İlke Adı 50 karakterle sınırlıdır.

## <a name="co-sell"></a>Ortak satış

Cosell sekmesinde bilgi sağlamak, teklifinizi yayınlamak için tamamen isteğe bağlıdır. Co-sell Ready ve IP Co-sell Ready durumunu elde etmek için gereklidir. Sağladığınız bilgiler, müşteri gereksinimlerine uygun unu değerlendirirken çözümünüz hakkında daha fazla bilgi edinmek için Microsoft satış ekipleri tarafından kullanılacaktır. Doğrudan müşteriler tarafından kullanılamaz.

Bu sekmeyi tamamlama hakkında daha fazla bilgi [için, Ortak Merkezi'nde Ortak satış seçeneğine](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-co-sell)bakın.

## <a name="test-drive"></a>Test sürüşü

**Test sürüşü** sekmesi, müşterilerin teklifinizi satın almayı taahhüt etmeden önce denemelerini sağlayacak bir gösteri (veya "test sürüşü") ayarlamanızı sağlar. Makalede daha fazla bilgi Test [Drive nedir?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)  Teklifiniz için artık bir test sürüşü sağlamak istemiyorsanız, **Teklif kurulum** sayfasına dönün ve **Test sürücüsüne etkin**bir şekilde etkinleştir'in denetimini kaldırın.

### <a name="technical-configuration"></a>Teknik yapılandırma

Azure Uygulamaları doğal olarak Azure Kaynak Yöneticisi test sürüşü türünü kullanır.  Daha fazla bilgi [için Azure Kaynak Yöneticisi test sürüşü için Teknik yapılandırmaya](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-new-customer-engagement-offer#technical-configuration-for-azure-resource-manager-test-drive) bakın.

### <a name="deployment-subscription-details"></a>Dağıtım abonelik ayrıntıları

Test Sürüşü'ni sizin adınıza dağıtmak için lütfen ayrı, benzersiz bir Azure Aboneliği oluşturun ve sağlayın. (Power BI test sürücüleri için gerekli değildir).

- **Azure abonelik kimliği** (Azure Kaynak Yöneticisi ve Mantık uygulamaları için gereklidir): Kaynak kullanımı raporlaması ve faturalandırma için Azure hesap hizmetlerinize erişim izni vermek için abonelik kimliğigirin girin. Zaten test sürücüleri için kullanmak üzere [ayrı bir Azure aboneliği oluşturmayı](https://docs.microsoft.com/azure/billing/billing-create-subscription) düşünmenizi öneririz. [Azure portalında](https://portal.azure.com/) oturum açarak ve sol taraftaki menüdeki **Abonelikler** sekmesinde gezinerek Azure abonelik kimliğinizi bulabilirsiniz. Sekmeyi seçmek abonelik kimliğinizi görüntüler (örn. "a83645ac-1234-5ab6-6789-1h234g764ghty").

- **Azure AD kiracı kimliği** (gerekli): Azure Etkin Dizini (AD) [kiracı kimliğinizi](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)girin. Bu kimliği bulmak için [Azure portalında](https://portal.azure.com/)oturum açın, sol menüdeki Etkin Dizin sekmesini seçin, **Özellikler'i seçin ve listelenen **Dizin Kimliği** numarasını arayın (örn. 50c464d3-4930-494c-963c-1e951d15360e). Ayrıca, alan adınızın URL'sini kullanarak kuruluşunuzun [https://www.whatismytenantid.com](https://www.whatismytenantid.com)kiracı kimliğine de bakabilirsiniz: .

- **Azure AD kiracı adı** (Dinamik 365 için gereklidir): Azure Etkin Dizini (AD) adınızı girin. Bu adı bulmak için, sağ üst köşedeki [Azure portalında](https://portal.azure.com/)oturum açın, kiracı adınız hesap adınız altında listelenir.

- **Azure AD uygulama kimliği** (gerekli): Azure Etkin Dizini (AD) [uygulama kimliğinizi](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)girin. Bu kimliği bulmak için [Azure portalında](https://portal.azure.com/)oturum açın, sol menüdeki Active Directory sekmesini seçin, **Uygulama kayıtlarını**seçin, ardından listelenen Uygulama **Kimliği** numarasını arayın (örn. 50c464d3-4930-494c-963c-1e951d15360e).

- **Azure AD uygulaması istemcisi gizli** (gerekli): Azure AD uygulama [istemcinizi gizli](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#certificates-and-secrets)girin. Bu değeri bulmak için [Azure portalında](https://portal.azure.com/)oturum açın. Sol menüdeki **Azure Active Directory** sekmesini seçin, **Uygulama kayıtlarını**seçin ve ardından test sürüşü uygulamanızı seçin. Sonra, **Sertifikalar ve sırlar**seçin, **Yeni istemci gizli**seçin, bir açıklama girin, Bitiş **Dolması**Altında **Asla** seçin, sonra **Ekle**seçin . Değeri kopyaladığından emin olun. (Değeri kopyalamadan önce sayfadan uzağa gitmez, aksi takdirde değere erişemezsin.)

Bir sonraki bölüme geçmeden önce **Kaydet'i** unutmayın!

### <a name="test-drive-listings-optional"></a>Test sürüşü listeleri (isteğe bağlı)

**Test sürüşü** sekmesinin altında bulunan **Test Sürüşü listeleri** seçeneği, test sürücünüzün mevcut olduğu dilleri (ve pazarları) görüntüler ve şu anda İngilizce (ABD) kullanılabilir tek konumdur. Ayrıca, bu sayfa dile özgü girişin durumunu ve eklenme tarihi/saati görüntüler. Her dil/pazar için test sürüşü ayrıntılarını (açıklama, kullanım kılavuzu, videolar, vb.) tanımlamanız gerekir.

- **Açıklama** (gerekli): Test sürüşünüzü, gösterilecek leri, kullanıcının deney yapacağı hedefleri, araştırılacak özellikleri ve kullanıcının teklifinizi alıp almayacağını belirlemesine yardımcı olacak ilgili bilgileri açıklayın. Bu alana en fazla 3.000 metin karakteri girilebilir. 

- **Erişim bilgileri** (Azure Kaynak Yöneticisi ve Mantık test sürücüleri için gereklidir): Bu test sürüşüne erişmek ve bunları kullanmak için müşterinin bilmesi gerekenleri açıklayın. Teklifinizi ve test sürüşü boyunca özelliklere erişmek için müşterinin tam olarak bilmesi gerekenleri kullanmak için bir senaryoda yürüyün. Bu alana en fazla 10.000 karakter metin girilebilir.

- **Kullanım Kılavuzu** (gerekli): Test sürüşü deneyiminizi derinlemesine inceler. Kullanım Kılavuzu, müşterinin test sürüşünü deneyimlemekten elde etmesini tam olarak istediğiniz şeyi kapsamalı ve sahip olabilecekleri sorular için bir referans görevi göremelidir. Dosya PDF formatında olmalı ve yükledikten sonra (en fazla 255 karakter) adlandırılmalıdır.

- **Videolar: Video ekleme** (isteğe bağlı): Videolar YouTube veya Vimeo'ya yüklenebilir ve burada bir bağlantı ve küçük resim görüntüsüyle (533 x 324 piksel) başvurulabilir, böylece bir müşteri, teklifinizin özelliklerini başarıyla nasıl kullanacağınız ve avantajlarıvurgulayan senaryoları nasıl başarılı bir şekilde anlayacağınızı da dahil olmak üzere test sürüşünü daha iyi anlamalarına yardımcı olacak bir bilgi yürüyüşünü görüntüleyebilir.
  - **Ad** (gerekli)
  - **URL (yalnızca YouTube veya Vimeo)** (gerekli)
  - **Küçük resim (533 x 324 px)**: Resim dosyası PNG formatında olmalıdır.

Bu alanları tamamladıktan sonra **Kaydet'i** seçin.

## <a name="publish"></a>Yayımlama

### <a name="submit-offer-to-preview"></a>Teklifönizlemeye gönderme

Teklifin gerekli tüm bölümlerini tamamladıktan sonra, portalın sağ üst köşesinde **yayımla'yı** seçin. **İnceleme ve yayımlama** sayfasına yönlendirileceksiniz. 

Bu teklifi ilk kez yayınlıyorsanız, şunları yapabilirsiniz:

- Teklifin her bölümü için tamamlanma durumuna bakın.
    - *Başlatılmamamıştır* - bölüme dokunulmadığı ve tamamlanması gerektiği anlamına gelir.
    - *Eksik* - bölümün düzeltilmesi gereken veya daha fazla bilgi sağlanması nı gerektiren hataları olduğu anlamına gelir. Lütfen bölüme geri dön ve güncelle.
    - *Tam* - bölüm tamamlandı, gerekli tüm veriler sağlandı ve hiçbir hata yok demektir. Teklifi sunamadan önce teklifin tüm bölümlerinin tam bir durumda olması gerekir.
- Uygulamanızın anlaşılmasına yardımcı olan ek notlara ek olarak uygulamanızın doğru test edildiğinden emin olmak için sertifika ekibine test talimatları sağlayın.
- **Gönder'i**seçerek yayımlama için teklif gönderin. Teklifin önizleme sürümünün gözden geçirip onaylamanız için kullanılabilir olduğunda size bildirmek için size bir e-posta göndeririz. Ortak Merkezi'ne dönün ve teklifinizi halka (veya özel bir teklifte özel bir hedef kitleye) yayınlamak için teklif için **Go-live'ı** seçin.

### <a name="errors-and-review-feedback"></a>Hatalar ve gözden geçirme geri bildirimi

Yayımlama işlemindeki **El Ile doğrulama** adımı, teklifinizin ve ilişkili teknik varlıklarının (özellikle Azure Kaynak Yöneticisi şablonu) kapsamlı bir incelemesini temsil eder ve sorunlar genellikle çekme isteği (PR) bağlantıları olarak sunulur. Bu PR'lerin nasıl görüntülenilip yanıtverileceklerine ilişkin bir açıklama, [bkz.](./azure-apps-review-feedback.md)

Yayımlama adımlarından birinde veya daha fazlasında hatalarla karşılaştıysanız, bunları düzeltmeniz ve teklifinizi yeniden yayımlamalısınız.

## <a name="next-steps"></a>Sonraki adımlar

- [Ticari Market'te bulunan bir teklifi güncelleştirme](./update-existing-offer.md)
