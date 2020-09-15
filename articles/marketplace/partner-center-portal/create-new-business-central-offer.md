---
title: Dynamics 365 Iş Merkezi teklifi oluşturma-Microsoft ticari Market
description: İş Ortağı Merkezi 'nde Microsoft ticari marketi 'nde yeni bir Dynamics 365 Iş Merkezi teklifi oluşturmaya yönelik adımları ve konuları öğrenin. Teklifinizi Azure Marketi 'nde veya bulut çözümü sağlayıcısı (CSP) programı aracılığıyla listeleyebilir veya satın alabilirsiniz.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: navits09
ms.author: navits
ms.date: 08/07/2020
ms.openlocfilehash: 1e2af29779c93175ccc4c7f6964862b79febd8e6
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90527097"
---
# <a name="create-a-dynamics-365-business-central-offer"></a>Dynamics 365 Business Central teklifi oluşturma

Bu makalede, yeni bir Dynamics 365 Iş Merkezi teklifinin nasıl oluşturulacağı açıklanmaktadır. [Microsoft Dynamics 365 Business Central](https://dynamics.microsoft.com/business-central) , finans, işlemler, tedarik ZINCIRI, CRM ve proje yönetimi ve elektronik ticaret dahil olmak üzere çok çeşitli iş süreçlerini işleyen bir kurumsal kaynak planlama (ERP) sistemidir. Premium paketler klasik dağıtım modelini ve üretimi de destekler. Dynamics 365 Business Central için tüm teklifler, sertifika sürecimize gitmelidir.

Başlamadan önce, henüz yapmadıysanız [Iş Ortağı Merkezi 'nde bir ticari Market hesabı oluşturun](create-account.md) . Hesabınızın ticari Market programına kayıtlı olduğundan emin olun.

## <a name="create-a-new-offer"></a>Yeni teklif oluşturma

1. [Iş Ortağı Merkezi](https://partner.microsoft.com/dashboard/home)' nde oturum açın.
2. Sol gezinti menüsünde **ticari Market**  >  **genel bakış**' ı seçin.
3. Genel Bakış sayfasında **+ yeni teklif**  >  **Dynamics 365 Business Central**' ı seçin.

    ![Sol gezinti menüsünü gösterir.](./media/new-offer-dynamics-365-business-central.png)

> [!NOTE]
> Bir teklif yayımlandıktan sonra, Iş Ortağı Merkezi 'nde yapılan düzenlemeler, teklif yeniden yayımlandıktan sonra yalnızca çevrimiçi mağazalarda görünür. Değişiklik yaptıktan sonra her zaman yeniden yayımlaytığınızdan emin olun.

## <a name="new-offer"></a>Yeni teklif

Bir **TEKLIF kimliği**girin. Bu, hesabınızdaki her teklif için benzersiz bir tanımlayıcıdır.

- Bu KIMLIK, varsa Market teklifi ve Azure Resource Manager şablonları için Web adresinde müşteriler tarafından görülebilir.
- Yalnızca küçük harfleri ve rakamları kullanın. Kısa çizgi ve alt çizgi içerebilir, ancak boşluk içeremez ve 50 karakterle sınırlıdır. Örneğin, **Test-teklif-1**girerseniz, teklif Web adresi olacaktır `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1` .
- Teklif KIMLIĞI, **Oluştur**' u seçtikten sonra değiştirilemez.

Bir **teklif diğer adı**girin. Bu, Iş Ortağı Merkezi 'nde teklif için kullanılan addır.

- Bu ad Market 'te kullanılmıyor ve teklif adından ve müşterilere gösterilen diğer değerlerden farklı.
- Teklif diğer adı, **Oluştur**' u seçtikten sonra değiştirilemez.

Teklifi oluşturmak için **Oluştur** ' u seçin ve devam edin.

## <a name="offer-setup"></a>Teklif kurulumu

Teklifinizi ayarlamak için bu adımları izleyin.

### <a name="how-do-you-want-potential-customers-to-interact-with-this-listing-offer"></a>Potansiyel müşterilerin bu liste teklifiyle nasıl etkileşime geçmesini istiyorsunuz?

Bu teklif için kullanmak istediğiniz seçeneği belirleyin.

#### <a name="get-it-now-free"></a>Şimdi alın (ücretsiz)

Uygulamanıza erişebilecekleri geçerli bir URL ( *http* veya *https*ile başlayarak) sağlayarak müşterilerinizin teklifinizi ücretsiz olarak listeleyin.  Örneğin, `https://contoso.com/my-app`.

#### <a name="free-trial-listing"></a>Ücretsiz deneme (listeleme)

Ücretsiz bir deneme bağlantısı olan müşterilere teklifinizi `http` `https` , bir deneme sunabileceği GEÇERLI bir URL (veya ' den başlayarak) sunarak listeleyin.  Örneğin, `https://contoso.com/trial/my-app`. Ücretsiz denemelerdeki teklif listesi, hizmetiniz tarafından oluşturulur, yönetilir ve yapılandırılır ve Microsoft tarafından yönetilen abonelikler içermez.

> [!NOTE]
> Uygulamanızın deneme bağlantınız aracılığıyla alacağı belirteçler, uygulamanızda hesap oluşturmayı otomatikleştirmek için yalnızca Azure Active Directory (Azure AD) aracılığıyla Kullanıcı bilgilerini almak üzere kullanılabilir. Bu belirteci kullanarak kimlik doğrulaması için Microsoft hesapları desteklenmez.

#### <a name="contact-me"></a>Benimle iletişim kurun

Müşteri Ilişkileri yönetimi (CRM) sisteminizi bağlayarak müşteri iletişim bilgilerini toplayın. Müşterinin, bilgilerini paylaşması için izin istenir. Bu müşteri ayrıntıları, teklifinizin bulunduğu teklif adı, KIMLIĞI ve Market kaynağıyla birlikte, yapılandırdığınız CRM sistemine gönderilir. CRM 'nizi yapılandırma hakkında daha fazla bilgi için bkz. [müşteri adayları](#customer-leads).

### <a name="test-drive"></a>Sınama sürücüsü

Bu noktada, Dynamics 365 Iş Merkezi teklifleri test sürücüsünü desteklemez. Test sürücüsünü teklifinizden kaldırmak için, **sınama sürücüsünü etkinleştir** onay kutusunu temizleyin.

### <a name="customer-leads"></a>Müşteri liderleri

[!INCLUDE [Connect lead management](./includes/connect-lead-management.md)]

Daha fazla bilgi için bkz. [müşteri adayı yönetimine genel bakış](./commercial-marketplace-get-customer-leads.md).

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="properties"></a>Özellikler

Bu sayfa, teklifinizi Market 'te, uygulama sürümünüzde ve teklifinizi destekleyen yasal sözleşmelerde gruplandırmak için kullanılan kategorileri ve endüstrileri tanımlamanızı sağlar.

### <a name="category"></a>Kategori

Teklifinizi uygun Market arama alanlarında yerleştirmek için kategoriler ve alt kategoriler ' i seçin. Teklifinizin teklif açıklamasında bu kategorileri nasıl desteklediğini açıkladığınızdan emin olun. Seçin:

- Birincil ve ikincil kategori dahil olmak üzere en az bir ve en fazla iki kategori (isteğe bağlı).
- Her birincil ve/veya ikincil kategori için en fazla iki alt kategori. Teklifiniz için geçerli bir alt kategori yoksa, **geçerli değil**' i seçin.

[Teklif listesi En Iyi uygulamalarında](../gtm-offer-listing-best-practices.md)kategorilerin ve alt kategorilerin tam listesini görebilirsiniz.

### <a name="industry"></a>Sektör

[!INCLUDE [Industry Taxonomy](./includes/industry-taxonomy.md)]

### <a name="app-version"></a>Uygulama sürümü

Teklifinizin sürüm numarasını girin. Müşteriler bu sürümü teklifin ayrıntı sayfasında listelenmiş olarak görür.

### <a name="terms-and-conditions"></a>hüküm ve koşullar

**Hüküm ve koşullar** alanında kendi yasal hüküm ve koşullarınızı belirtin. Ayrıca, hüküm ve koşullarınızın bulunabileceği URL 'YI de sağlayabilirsiniz. Müşterilerinizin teklifinizi deneyebilmeleri için önce bu koşulları kabul etmesi gerekir.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="offer-listing"></a>Teklif listesi

Bu, teklifiniz için ad, açıklama ve görüntü gibi ayrıntıları tanımladığınız yerdir.

> [!NOTE]
> Yalnızca bir dilde teklif listesi ayrıntılarını sağlayabilirsiniz. Teklif açıklaması "ifadesi ile başladığı sürece Ingilizce olması gerekmez," Bu uygulama yalnızca [Ingilizce olmayan dilde] kullanılabilir ". Ayrıca, teklif listeleme içeriğinde kullanılandan farklı bir dilde içerik sunmak için bir *Yardım bağlantısı URL 'si* sağlamak da kabul edilebilir.

Teklif bilgilerinin Microsoft AppSource nasıl göründüğünü gösteren bir örnek aşağıda verilmiştir (listelenen tüm fiyatlar yalnızca örnek amaçlıdır ve gerçek maliyetlerin yansıtılmasına yönelik değildir):

:::image type="content" source="media/example-d365-business-central.png" alt-text="Bu teklifin Microsoft AppSource nasıl göründüğünü gösterir.":::

#### <a name="call-out-descriptions"></a>Çağrı açıklamaları

1. Logo
2. Ürünler
3. Kategoriler
4. Destek adresi (bağlantı)
5. Kullanım koşulları
6. Gizlilik ilkesi
7. Teklif adı
8. Özet
9. Açıklama
10. Ekran görüntüleri/videolar

### <a name="name"></a>Name

Buraya girdiğiniz ad, müşteriler tarafından teklif listelerinizin başlığı olarak gösterilir. Bu alan, teklifi oluştururken **teklif diğer adı** için girdiğiniz metinle önceden doldurulur, ancak bu değeri değiştirebilirsiniz. Bu ad trademarked olabilir (ve ticari marka veya telif hakkı sembolleri dahil edebilirsiniz). Ad 50 karakterden uzun olamaz ve herhangi bir emojıs içeremez.

### <a name="short-description"></a>Kısa açıklama

Teklifinizin en fazla 100 karakter uzunluğunda olması için kısa bir açıklama sağlayın. Bu açıklama Market arama sonuçlarında kullanılabilir.

### <a name="description"></a>Açıklama

[!INCLUDE [Long description-1](./includes/long-description-1.md)]

[!INCLUDE [Long description-2](./includes/long-description-2.md)]

[!INCLUDE [Rich text editor](./includes/rich-text-editor.md)]

### <a name="search-keywords"></a>Anahtar sözcükleri ara

Müşterilerin teklifinizi Market 'te bulmasına yardımcı olmak için isteğe bağlı en fazla üç arama anahtar sözcüğü girebilirsiniz. En iyi sonuçlar için, tanımlarınızda bu anahtar sözcükleri de kullanmayı deneyin.

### <a name="products-your-app-works-with"></a>Uygulamanızın ile birlikte çalışarak ürünler

Müşterilerinizin uygulamanızın belirli ürünlerle birlikte çalışıp çalışmadığını bilmesini istiyorsanız buraya en fazla üç ürün adı girin.

### <a name="helpprivacy-urls"></a>Yardım/Gizlilik URL 'Leri

Bu bölüm, müşterilerinizin teklifiniz hakkında daha fazla bilgi sağlamasına yardımcı olacak bağlantılar sağlamanıza olanak tanır.

#### <a name="help-link"></a>Yardım bağlantısı

Müşterilerinizin teklifiniz hakkında daha fazla bilgi sağlayabilecekleri URL 'YI girin. **Yardım bağlantınız** , **Destek URL** 'niz ile aynı olamaz (aşağıda açıklanmıştır).

#### <a name="privacy-policy-link"></a>Gizlilik ilkesi bağlantısı

Kuruluşunuzun gizlilik ilkesinin URL 'sini girin. Uygulamanızın gizlilik yasaları ve yönetmeliklerle uyumlu olmasını sağlamaktan ve geçerli bir gizlilik ilkesi sağlamaya yönelik siz sorumlusunuz.

### <a name="contact-information"></a>İletişim Bilgileri

Bu bölümde, bir **destek kişisi** ve **mühendislik ilgili kişisi**için ad, e-posta ve telefon numarası sağlamanız gerekir. Bu bilgiler müşterilere gösterilmez, ancak Microsoft tarafından kullanılabilir ve CSP iş ortakları için de sağlanıyor olabilir.

**Destek kişisi** bölümünde, CSP iş ortaklarının teklifiniz için destek BULABILECEĞI **Destek URL** 'sini de sağlamanız gerekir. Destek URL 'niz **Yardım bağlantılarınız**ile aynı olamaz.

### <a name="supporting-documents"></a>Destekleyici belgeler

Burada, teknik incelemeler, broşürler, denetim listeleri veya sunular gibi en az bir (en fazla üç) ilgili pazarlama belgesi sağlayın. Bu belgeler. PDF biçiminde olmalıdır.

### <a name="marketplace-images"></a>Market görüntüleri

**Büyük** boyutlu amblem IÇIN bir PNG dosyası sağlayın. İş Ortağı Merkezi, **küçük** bir logo oluşturmak için bunu kullanacaktır. İsterseniz bunu daha sonra farklı bir görüntüyle değiştirebilirsiniz.

- **Büyük** (216 x 216-350 x 350 px, gereklidir)
- **Küçük** (48 x 48 piksel, isteğe bağlı)

Bu amblemler, listede farklı yerlerde kullanılır:

[!INCLUDE [logos-appsource-only](../includes/logos-appsource-only.md)]

[!INCLUDE [Logo tips](../includes/graphics-suggestions.md)]

>[!NOTE]
>Dosya yükleme sorununuz varsa, yerel ağınızın `https://upload.xboxlive.com` Iş Ortağı Merkezi tarafından kullanılan hizmeti engellemediğinden emin olun.

#### <a name="screenshots"></a>Ekran görüntüleri

Teklifinizin nasıl çalıştığını gösteren ekran görüntüleri ekleyin. En az üç ekran görüntüsü gerekir ve beş adede kadar ekleyebilirsiniz. Tüm ekran görüntüleri 1280 x 720 piksel olmalıdır.

#### <a name="videos"></a>Videolar

İsteğe bağlı olarak, teklifinizi gösteren en fazla beş video ekleyebilirsiniz. Bu videoların YouTube ve/veya Vimeo 'da barındırılması gerekir. Her biri için videonun adını, URL 'sini ve videonun küçük bir görüntüsünü (1280 x 720 piksel) girin.

#### <a name="additional-marketplace-listing-resources"></a>Ek Market listeleme kaynakları

[Market teklif listeleri için en iyi uygulamalar](../gtm-offer-listing-best-practices.md)

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="availability"></a>Kullanılabilirlik

Bu sayfa, teklifinizin nerede ve nasıl kullanılabilir hale sunulabileceği hakkında seçenekler sağlar.

### <a name="markets"></a>Pazar

Bu bölüm, teklifinizin kullanılabilir olması gereken pazarları belirtmenize olanak tanır. Bunu yapmak için **Pazar seçimi** açılır penceresini görüntüleyen **pazarları Düzenle**' yi seçin.

Teklifinizi yayımlamak için en az bir pazar seçin. Teklifinizin her olası pazarda kullanılabilmesini sağlamak için **Tümünü Seç** ' i seçin veya eklemek istediğiniz belirli pazarları seçin.

Burada yaptığınız seçimler yalnızca yeni alımlar için geçerlidir; zaten belirli bir pazar ortamında uygulamanız varsa ve daha sonra bu pazarı kaldırırsanız, bu pazarda zaten sunulan bir pazara sahip kişiler bunu kullanmaya devam edebilir, ancak bu pazardaki yeni müşteriler teklifinizi alabilir.

> [!IMPORTANT]
> Bu gereksinimler burada veya Iş Ortağı Merkezi 'nde listelenmese de, yerel yasal gereksinimleri karşılamak sizin sorumluluğunuzdadır.

Tüm pazarlar, yerel yasalar, kısıtlamalar veya diğer faktörler, bazı ülkelerde ve bölgelerde belirli tekliflerin listelenmesini engelleyebilse de aklınızda bulundurun.

### <a name="preview-audience"></a>İzleyiciyi Önizle

Teklifinizi daha geniş Market teklifiyle yayımlamadan önce, önce onu sınırlı bir **Önizleme hedef kitlesi**için kullanılabilir hale getirmeniz gerekir. Burada bir **gizleme anahtarı** (yalnızca küçük harf ve/veya sayı kullanan herhangi bir dize) girin. Önizleme hedef kitlenizin üyeleri, Market 'te teklifinizin önizlemesini görüntülemek için bu anahtarı bir belirteç olarak kullanabilir.

Daha sonra, teklifinizi kullanılabilir duruma getirmek ve önizleme kısıtlamasını kaldırmak için hazır olduğunuzda, **gizleme anahtarını** kaldırmanız ve yeniden yayımlamanız gerekir.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="technical-configuration"></a>Teknik yapılandırma

Bu sayfa, teklifiniz ile bağlantı kurmak için kullanılan teknik ayrıntıları tanımlar. Bu bağlantı, teklifi edinmeyi tercih ettiklerinde, son müşteri için teklifinizi sağlamamızı sağlar.

### <a name="package-type"></a>Paket türü

Teklifiniz için geçerli olan seçeneği belirleyin:

* **Ekleme** – eklenti uygulaması, Dynamics 365 Business Central 'ın deneyimini ve mevcut işlevselliğini genişletir. Ayrıntılar için bkz. [eklenti uygulamaları](https://docs.microsoft.com/dynamics365/business-central/dev-itpro/developer/readiness/readiness-add-on-apps).
* **Connect** – bir Connect uygulaması, Dynamics 365 İş Merkezi ile bir üçüncü taraf çözüm veya hizmet arasında noktadan noktaya bağlantı kurulması gereken senaryolarda kullanılabilir. Ayrıntılar için bkz. [uygulamaları bağlama](https://docs.microsoft.com/dynamics365/business-central/dev-itpro/developer/readiness/readiness-connect-apps).

### <a name="file-upload"></a>Karşıya dosya yükleme

Yukarıdaki **Ekle** ' yi seçtiyseniz, teklifin paket dosyasını, bağımlılıkları olan herhangi bir uzantıya ait paket dosyaları ile birlikte karşıya yükleyeceksiniz.

#### <a name="extensions-package-file"></a>Uzantılar paket dosyası

Teklifiniz için Uzantı paketi dosyası (. app) dosyasını karşıya yükleyin.

#### <a name="library-package-file"></a>Kitaplık paketi dosyası

Teklifinizin Market 'e yayımlanmayacak başka bir uzantıyla birlikte yüklenmesi gerekiyorsa gerekir. Öyleyse. app dosyasını buraya yükleyin.

#### <a name="dependency-package-file"></a>Bağımlılık paketi dosyası

Teklifinizin Market 'te zaten yayımlanmış başka bir uzantıyla birlikte yüklenmesi gerekiyorsa gereklidir. Bu durumda, `.app` veya `.zip` dosyasını buraya yükleyin.

### <a name="url-to-app-installation"></a>Uygulama yüklemesinin URL 'SI

Yukarıdaki **Bağlan** ' ı seçtiyseniz, uygulama yüklemenizin adresini buraya girin. Yüklemesi gerektirmeyen bağlı hizmetler için, hizmet giriş sayfanız veya kaydolma sayfanız için adresi sağlayın.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="test-drive-technical-configuration"></a>Test sürüşü teknik yapılandırması

Bu sayfa, müşterilerin satın almadan önce teklifinizi denemesini sağlayan bir tanıtım ("test sürücüsü") ayarlamanıza olanak sağlar. [Test sürücüsü nedir?](../what-is-test-drive.md)bölümünde daha fazla bilgi edinin.

Bir sınama sürücüsünü etkinleştirmek için [teklif kurulumu](#test-drive) sekmesinde **bir test sürücüsünü etkinleştir** onay kutusunu işaretleyin. Test sürücüsünü teklifinizden kaldırmak için bu onay kutusunu temizleyin.

Test sürücünüzü ayarlamayı bitirdiğinizde devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="supplemental-content"></a>Ek içerik

Bu sayfa teklifinizi doğrulamamıza yardımcı olmak için teklifiniz hakkında ek bilgiler sağlamanıza olanak tanır. Bu bilgiler müşterilere gösterilmez veya Market 'te yayımlanmamıştır.

### <a name="target-release"></a>Hedef yayın sürümü

Çözümünüzün hedeflediği Microsoft Dynamics Business 'ın hangi sürümü olduğunu belirtin: **geçerli**, **sonraki büyük**veya **sonraki küçük**. Bu bilgiler, çözümünüzü uygun şekilde test etmemize olanak sağlar.

### <a name="supported-editions"></a>Desteklenen sürümler

Teklifiniz Microsoft Dynamics 365 Business Central 'ın Premium sürümünü gerektiriyorsa yalnızca **Premium** ' u seçin. Aksi takdirde, hem **Essentials** hem de **Premium**' u seçin.

### <a name="key-usage-scenario"></a>Anahtar kullanımı senaryosu

Teklifinizin bir belge (. PDF biçiminde) içinde listelenen anahtar kullanımı senaryolarını listeleyen bir PDF dosyası yüklemeniz gerekir. Burada listelenen tüm senaryolar, Market için teklifinizi onaylamadan önce doğrulama takımımız tarafından doğrulanabilir.

### <a name="app-tests-automation"></a>Uygulama testleri Otomasyonu

Teklifiniz bir eklenti uygulaması ise, bir **uygulama testleri Otomasyon** dosyası (. app) yüklemeniz gerekir. Bu dosya, uygulama bağlama için geçerli değildir.

### <a name="test-accounts"></a>Test hesapları

Sertifika ekibimizin teklifinizi doğru bir şekilde gözden geçirmesi için bir test hesabı gerekiyorsa, **test hesabı** bilgilerinizi içeren bir. PDF,. doc veya. docx dosyasını yükleyin.

## <a name="publish"></a>Yayımlama

### <a name="submit-offer-to-preview"></a>Önizlemeye teklif Gönder

Teklifin tüm gerekli bölümlerini tamamladıktan sonra portalın sağ üst köşesinde **Yayımla** ' yı seçin. **İnceleme ve yayımlama** sayfasına yönlendirilirsiniz. 

Bu teklifi ilk kez yayımladıysanız şunları yapabilirsiniz:

- Teklifin her bölümü için tamamlanma durumuna bakın.
    - *Başlatılmamış* -bölüm dokunulmamış ve tamamlanması gereken anlamına gelir.
    - *Tamamlanmamış* -bölümde düzeltilmesi gereken hatalar olduğu veya daha fazla bilgi sağlanması gerekir. Bölüm (ler) e geri dönün ve güncelleştirin.
    - *Tam* -bölümün tamamlandığı, tüm gerekli verilerin sağlandığı ve hata olmadığı anlamına gelir. Teklifi gönderebilmeniz için teklifin tüm bölümlerinin tamamen bir durumda olması gerekir.
- **Sertifika notları** bölümünde, uygulamanızı anlamak için yararlı olan tüm ek notlara ek olarak, uygulamanızın doğru şekilde test edilmesini sağlamak için sertifika ekibine test yönergeleri sağlayın.
- **Gönder**' i seçerek teklifi yayımlamaya gönderebilirsiniz. Teklifin önizleme sürümü gözden geçirmeniz ve onaylamanız için kullanılabilir olduğunda size bir e-posta göndereceğiz. Iş Ortağı Merkezi 'ne dönün ve teklifinizi herkese açık bir şekilde **yayımlamak için teklif ' i seçin.**

## <a name="next-steps"></a>Sonraki adımlar

- [Ticari Market'te bulunan bir teklifi güncelleştirme](./update-existing-offer.md)
