---
title: Microsoft ticari marketi 'nde & PowerApps teklifi için Dynamics 365 for Customer Engagement oluşturun
description: Microsoft AppSource PowerApps & için Dynamics 365 for Customer Engagement oluşturma. Teklifinizi AppSource 'ta veya bulut çözümü sağlayıcısı (CSP) programı aracılığıyla listeleyin veya satın.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: how-to
author: navits09
ms.author: navits
ms.date: 12/02/2020
ms.openlocfilehash: 0c220daab0d1d9ae7d50d37d9303d6677bd52cc1
ms.sourcegitcommit: dfc4e6b57b2cb87dbcce5562945678e76d3ac7b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2020
ms.locfileid: "97360314"
---
# <a name="create-a-dynamics-365-for-customer-engagement--powerapps-offer"></a>Dynamics 365 for Customer Engagement ve PowerApps teklifi oluşturma

Bu makalede, PowerApps teklifinin & müşteri katılımı için yeni bir Dynamics 365 nasıl oluşturulacağı açıklanır. Dynamics 365 for Customer Engagement (PowerApps, Sales, Service, Project Service ve Field Service) için tüm uygulamalar, çözümünüzü standart gereksinimlere, uyumlulukla ve uygun yöntemlere denetleyen sertifika sürecimize gitmelidir. Deneme deneyimi, kullanıcıların çözümünüzü canlı bir Dynamics 365 ortamına dağıtmasını sağlar.

Başlamadan önce, henüz yapmadıysanız [Iş Ortağı Merkezi 'nde bir ticari Market hesabı oluşturun](create-account.md) . Hesabınızın ticari Market programına kayıtlı olduğundan emin olun.

>[!NOTE]
> Bir teklif yayımlandıktan sonra, teklifin düzenlemeleri yalnızca yayın teklifini yeniden gönderdikten sonra Iş Ortağı Merkezi 'nde ve çevrimiçi mağazada güncelleştirilir.

## <a name="create-a-new-offer"></a>Yeni teklif oluşturma

1. [Iş Ortağı Merkezi](https://partner.microsoft.com/dashboard/home)' nde oturum açın.
2. Sol gezinti menüsünde **ticari Market**  >  **genel bakış**' ı seçin.
3. Genel Bakış sayfasında,   >  **PowerApps & müşteri katılımı için**+ yeni teklif Dynamics 365 ' i seçin.

    ![Sol gezinti menüsünü gösterir.](./media/new-offer-dynamics-365-customer-engagement-powerapps.png)

## <a name="new-offer"></a>Yeni teklif

Bir **TEKLIF kimliği** girin. Bu, hesabınızdaki her teklif için benzersiz bir tanımlayıcıdır.

- Bu KIMLIK, varsa Market teklifi ve Azure Resource Manager şablonları için Web adresinde müşteriler tarafından görülebilir.
- Yayımcı KIMLIĞIYLE birleştirilmiş teklif KIMLIĞI, 40 karakter uzunluğunda olmalıdır.
- Yalnızca küçük harfleri ve rakamları kullanın. Kısa çizgi ve alt çizgi içerebilir ancak boşluk içeremez. Örneğin, yayımcı KIMLIĞINIZ ise `testpublisherid` ve **Test-teklif-1** girerseniz, teklif Web adresi olur `https://appsource.microsoft.com/product/dynamics-365/testpublisherid.test-offer-1` .
- Bu KIMLIK, **Oluştur**' u seçtikten sonra değiştirilemez.

Bir **teklif diğer adı** girin. Bu, Iş Ortağı Merkezi 'nde teklif için kullanılan addır.

- Bu ad Market 'te kullanılmıyor ve teklif adından ve müşterilere gösterilen diğer değerlerden farklı.
- Bu ad, **Oluştur**' u seçtikten sonra değiştirilemez.

Teklifi oluşturmak için **Oluştur** ' u seçin ve devam edin.

## <a name="offer-setup"></a>Teklif kurulumu

### <a name="alias"></a>Diğer ad

Bu teklifine yalnızca Iş Ortağı Merkezi içinde başvurmak için kullanacağımız açıklayıcı bir ad girin. Bu ad (teklif oluşturduğunuzda girdiğiniz değer ile önceden doldurulur) Market 'te kullanılmaz ve müşteriler tarafından gösterilen teklif adından farklıdır. Teklif adını daha sonra güncelleştirmek istiyorsanız [teklif listesi](#offer-listing) sayfasına gidin.

### <a name="setup-details"></a>Kurulum Ayrıntıları

**Potansiyel müşterilerin bu liste teklifiyle nasıl etkileşime geçmesini istiyorsunuz?**, bu teklif için kullanmak istediğiniz seçeneği belirleyin.

- **Şimdi alın (ücretsiz)** – teklifinizi ücretsiz olarak listeleyin.
- **Ücretsiz deneme (listeleme)** – teklifinizi ücretsiz bir deneme bağlantısı olan müşterilere listeleyin. Ücretsiz denemelerdeki teklif listesi, hizmetiniz tarafından oluşturulur, yönetilir ve yapılandırılır ve Microsoft tarafından yönetilen abonelikler içermez.

    > [!NOTE]
    > Uygulamanızın deneme bağlantınız aracılığıyla alacağı belirteçler, uygulamanızda hesap oluşturmayı otomatikleştirmek için yalnızca Azure Active Directory (Azure AD) aracılığıyla Kullanıcı bilgilerini almak üzere kullanılabilir. Bu belirteci kullanarak kimlik doğrulaması için Microsoft hesapları desteklenmez.

- **Benimle Iletişim kurun** – müşteri ilişkileri YÖNETIMI (CRM) sisteminizi bağlayarak müşteri iletişim bilgilerini toplayın. Müşterinin, bilgilerini paylaşması için izin istenir. Bu müşteri ayrıntıları, teklifinizin bulunduğu teklif adı, KIMLIĞI ve Market kaynağıyla birlikte, yapılandırdığınız CRM sistemine gönderilir. CRM 'nizi yapılandırma hakkında daha fazla bilgi için bkz. [müşteri adayları](#customer-leads).

### <a name="test-drive"></a>Sınama sürücüsü

Test sürücüsü, "satın almadan önce dene" seçeneği sunarak olası müşterilere teklifinizi göstermek için harika bir yoldur. bu sayede, artan dönüştürmeye ve yüksek oranda nitelikli müşteri adaylarının oluşturulmasına neden olur. Daha fazla bilgi edinmek için, [test sürüşü](../what-is-test-drive.md)ile başlayın.

Bir sınama sürücüsünü sabit bir süre etkinleştirmek için, **bir sınama sürücüsünü etkinleştir** onay kutusunu seçin. Test sürücüsünü teklifinizden kaldırmak için bu onay kutusunu temizleyin.

### <a name="customer-leads"></a>Müşteri liderleri

[!INCLUDE [Connect lead management](./includes/connect-lead-management.md)]

Daha fazla bilgi için bkz. [müşteri adayı yönetimine genel bakış](./commercial-marketplace-get-customer-leads.md).

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="properties"></a>Özellikler

Bu sayfa, teklifinizi Market 'te, uygulama sürümünüzde ve teklifinizi destekleyen yasal sözleşmelerde gruplandırmak için kullanılan kategorileri ve endüstrileri tanımlamanızı sağlar.

### <a name="categories"></a>Kategoriler

Teklifinizi uygun Market arama alanlarında yerleştirmek için kategoriler ve alt kategoriler ' i seçin. Teklifinizin teklif açıklamasında bu kategorileri nasıl desteklediğini açıkladığınızdan emin olun. Seçin:

- Birincil ve ikincil kategori dahil olmak üzere en az bir ve en fazla iki kategori (isteğe bağlı).
- Her birincil ve/veya ikincil kategori için en fazla iki alt kategori. Teklifiniz için geçerli bir alt kategori yoksa, **geçerli değil**' i seçin.

[Teklif listesi En Iyi uygulamalarında](../gtm-offer-listing-best-practices.md)kategorilerin ve alt kategorilerin tam listesini görebilirsiniz.

### <a name="industries"></a>Sektörler

[!INCLUDE [Industry Taxonomy](includes/industry-taxonomy.md)]

### <a name="applicable-dynamics-365-products"></a>Geçerli Dynamics 365 ürünleri

Bu teklifin uygulandığı tüm Dynamics 365 ürünlerini seçin.

### <a name="app-version"></a>Uygulama sürümü

Teklifinizin sürüm numarasını girin. Müşteriler bu sürümü teklifin ayrıntı sayfasında listelenmiş olarak görür.<!-- If you are only updating the version number due to marketing/descriptive changes, check the **Marketing only change** box. This option allows the offer to bypass the certification and provisioning stages.-->

### <a name="terms-and-conditions"></a>hüküm ve koşullar

Burada kendi yasal hüküm ve koşullarınızı sağlayın. Hüküm ve koşullarınızın bulunabileceği adresi de sağlayabilirsiniz. Müşterilerinizin teklifinizi deneyebilmeleri için önce bu koşulları kabul etmesi gerekir.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="offer-listing"></a>Teklif listesi

<!--This page displays the languages in which your offer will be listed. Currently, **English (United States)** is the only available option.

Define marketplace details for each language/market here, such as offer name, description, and images. Select the language/market name to provide this information.-->This page lets you define offer details such as offer name, description, links, and contacts.

> [!NOTE]
> Yalnızca bir dilde liste ayrıntılarını sunun. Teklif açıklaması "ifadesi ile başladığı sürece Ingilizce olması gerekmez," Bu uygulama yalnızca [Ingilizce olmayan dilde] kullanılabilir ". Ayrıca, teklif listeleme içeriğinde kullanılandan farklı bir dilde içerik sunmak için *yararlı bir bağlantı URL 'si* sağlamak da kabul edilebilir.

Teklif bilgilerinin Microsoft AppSource nasıl göründüğünü gösteren bir örnek aşağıda verilmiştir (listelenen tüm fiyatlar yalnızca örnek amaçlıdır ve gerçek maliyetlerin yansıtılmasına yönelik değildir):
<!-- update screen? -->
:::image type="content" source="media/example-azure-marketplace-d365-customer-engagement.png" alt-text="Bu teklifin Microsoft AppSource nasıl göründüğünü gösterir.":::

#### <a name="call-out-descriptions"></a>Çağrı açıklamaları

1. Logo
1. Ürünler
1. Kategoriler
1. Destek adresi (bağlantı)
1. Kullanım koşulları
1. Teklif adı
1. Açıklama
1. Ekran görüntüleri/videolar

### <a name="marketplace-details"></a>Market ayrıntıları

Buraya girdiğiniz **ad** , müşteriler tarafından teklif listelerinizin başlığı olarak gösterilir. Bu alan, teklifi oluştururken **teklif diğer adı** için girdiğiniz metinle önceden doldurulur, ancak bu değeri değiştirebilirsiniz. Bu ad trademarked olabilir (ve ticari marka veya telif hakkı sembolleri dahil edebilirsiniz). Ad 50 karakterden uzun olamaz ve herhangi bir emojıs içeremez.

**Arama sonuçları Özeti** için en fazla 100 karakter uzunluğunda olan teklifinizin kısa bir açıklamasını girin. Bu açıklama Market arama sonuçlarında kullanılabilir.

[!INCLUDE [Long description-1](./includes/long-description-1.md)]

[!INCLUDE [Long description-2](./includes/long-description-2.md)]

[!INCLUDE [Rich text editor](./includes/rich-text-editor.md)]

Müşterilerin teklifinizi Market 'te bulmasına yardımcı olmak için isteğe bağlı en fazla üç **arama anahtar sözcüğü** girebilirsiniz. En iyi sonuçlar için, tanımlarınızda bu anahtar sözcükleri de kullanın.

Müşterilerinizin **Uygulamanızın hangi ürünleri** yaptığını bilmesini sağlamak istiyorsanız, en fazla üç ürün adı girin.

### <a name="helpprivacy-urls"></a>Yardım/Gizlilik URL 'Leri

Müşterilerinizin teklifiniz hakkında daha fazla bilgi sağlayabilecekleri uygulamanızın (URL) **Yardım bağlantısını** girin. Yardım URL 'niz, destek URL 'niz ile aynı olamaz.

Kuruluşunuzun gizlilik ilkesine **Gizlilik ilkesi bağlantısını** (URL) girin. Uygulamanızın gizlilik yasaları ve yönetmeliklerle uyumlu olmasını sağlamaktan ve geçerli bir gizlilik ilkesi sağlamaya yönelik siz sorumlusunuz.

### <a name="contact-information"></a>İletişim bilgileri

Bir **destek kişisi** ve **mühendislik ilgili kişisi** için ad, e-posta ve telefon numarası girin. Bu bilgiler müşterilere gösterilmez, ancak Microsoft tarafından kullanılabilir ve CSP iş ortakları için de sağlanıyor olabilir.

**Destek kişisi** bölümünde, CSP iş ortaklarının teklifiniz için destek BULABILECEĞI **Destek URL** 'sini sağlayın. Destek URL 'niz yardım URL 'niz ile aynı olamaz.

### <a name="supporting-documents"></a>Destekleyici belgeler

PDF biçiminde teknik incelemeler, broşürler, denetim listeleri veya sunular gibi en az bir (en fazla üç) ilgili pazarlama belgesi sağlayın.

### <a name="marketplace-media"></a>Market medyası

Müşterilerinizin teklifinizi gösterirken kullanılacak logoları ve görüntüler sağlayın. Tüm görüntülerin PNG biçiminde olması gerekir.

[!INCLUDE [logo tips](../includes/graphics-suggestions.md)]

>[!NOTE]
>Dosya yükleme sorununuz varsa, yerel ağınızın https://upload.xboxlive.com Iş Ortağı Merkezi tarafından kullanılan hizmeti engellemediğinden emin olun.

#### <a name="logos"></a>Ların

**Büyük** boyutlu amblem IÇIN bir PNG dosyası sağlayın. İş Ortağı Merkezi bunu, gerekli diğer boyutları oluşturmak için kullanır. İsterseniz bunu daha sonra farklı bir görüntüyle değiştirebilirsiniz.

Bu amblemler, listede farklı yerlerde kullanılır:

[!INCLUDE [logos-appsource-only](../includes/logos-appsource-only.md)]

[!INCLUDE [Logo tips](../includes/graphics-suggestions.md)]

#### <a name="screenshots"></a>Ekran görüntüleri

Teklifinizin nasıl çalıştığını gösteren ekran görüntüleri ekleyin. En az bir ekran görüntüsü gereklidir ve en fazla beş tane ekleyebilirsiniz. Tüm ekran görüntülerinin 1280 x 720 piksel ve PNG biçiminde olması gerekir.

#### <a name="videos"></a>Videolar

İsteğe bağlı olarak, teklifinizi gösteren dört adede kadar video ekleyebilirsiniz. Videoların bir dış sitede barındırılması gerekir. Her biri için videonun adını, adresini ve videonun küçük bir görüntüsünü (1280 x 720 piksel) girin.

Diğer Market listeleme kaynakları için bkz. [Market teklif listeleri Için en iyi uygulamalar](../gtm-offer-listing-best-practices.md).

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="availability"></a>Kullanılabilirlik

Bu sayfa, teklifinizin nerede ve nasıl kullanılabileceğini tanımlamanıza olanak sağlar.

### <a name="markets"></a>Pazar

Teklifinizin kullanılabilir olması gereken pazarları belirtmek için **Pazar seçimi** açılır penceresini göstermek üzere **pazarları Düzenle** ' yi seçin.

En az bir pazar seçin. Teklifinizin her olası pazarda kullanılabilmesini sağlamak için Tümünü Seç ' i seçin veya yalnızca istediğiniz **pazarların** birini seçin. İşiniz bittiğinde **Kaydet**' i seçin.

Burada yaptığınız seçimler yalnızca yeni alımlar için geçerlidir; zaten belirli bir pazar ortamında uygulamanız varsa ve daha sonra bu pazarı kaldırırsanız, bu pazarda sunulan bir pazara zaten sahip olan kişiler bunu kullanmaya devam edebilir, ancak bu pazardaki yeni müşteriler teklifinizi alabilir.

> [!IMPORTANT]
> Bu gereksinimler burada veya Iş Ortağı Merkezi 'nde listelenmese de, yerel yasal gereksinimleri karşılamak sizin sorumluluğunuzdadır. Tüm pazarlar ' ı seçseniz bile, yerel yasalar, kısıtlamalar veya diğer faktörler belirli tekliflerin bazı ülkelerde ve bölgelerde listelenmesine engel olabilir.

### <a name="preview-audience"></a>İzleyiciyi Önizle

Teklifinizi daha geniş Market teklifiyle yayımlamadan önce, önce onu sınırlı bir **Önizleme hedef kitlesi** için kullanılabilir hale getirmeniz gerekir. Burada bir **gizleme anahtarı** (yalnızca küçük harf ve/veya sayı kullanan herhangi bir dize) girin. Önizleme hedef kitlenizin üyeleri, Market 'te teklifinizin önizlemesini görüntülemek için bu anahtarı bir belirteç olarak kullanabilir.

Daha sonra, teklifinizi kullanılabilir duruma getirmek ve önizleme kısıtlamasını kaldırmak için hazır olduğunuzda, **gizleme anahtarını** kaldırmanız ve yeniden yayımlamanız gerekir.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="technical-configuration"></a>Teknik yapılandırma

Bu sayfa, teklifiniz ile bağlantı kurmak için kullanılan teknik ayrıntıları tanımlar. Bu bağlantı, teklifi edinmeyi tercih ettiklerinde, son müşteri için teklifinizi sağlamamızı sağlar.

### <a name="offer-information"></a>Teklif bilgileri

**Temel lisans modeli** , müşterilerin UYGULAMANıZ Için CRM Yönetim merkezinde nasıl atandığını belirler. Lisanslara kiracı başına bir tane atanırsa, örnek tabanlı lisanslama veya **Kullanıcı** için **kaynak** seçin.

**S2S giden ve CRM güvenli depo erişimi gerektirir** onay kutusu, CRM güvenli depolama veya sunucu-sunucu (S2S) giden erişiminin yapılandırılmasını sağlar. Bu özellik, sertifika aşamasında Dynamics 365 ekibinin özelleşmiş bir şekilde dikkate alınması gerekir. Microsoft, bu özelliği desteklemeye yönelik ek adımları tamamlayabilmeniz için sizinle iletişim kuracaktır.

**Uygulama yapılandırma URL 'sini** boş bırakın; gelecekte kullanılmak üzere.

### <a name="crm-package"></a>CRM paketi

**Paket konumlarınızın URL 'si** için KARŞıYA yüklenen CRM Package. zip dosyasını Içeren bir Azure Blob depolama hesabının URL 'sini girin. Microsoft 'un, doğrulamaya yönelik paketinizi görebilmesi için, URL 'ye bir salt okunurdur SAS anahtarı ekleyin.

> [!IMPORTANT]
> Bir yayımlama bloğundan kaçınmak için, blob depolamanın URL 'sindeki sona erme tarihinin süresinin dolmadığından emin olun. İlkenize erişerek tarihi gözden geçirebilir. **Süre sonu zamanının** gelecekte en az bir ay olması önerilir.

Uygunsa, **paket dosyası içinde birden çok CRM paketi var kutusunda '** ı seçin. Bu durumda, tüm paketleri. zip dosyanıza eklediğinizden emin olun.

Paketinizi oluşturma ve yapısını güncelleştirme hakkında ayrıntılı bilgi için bkz. [3. Adım: uygulamanız Için AppSource paketi oluşturma](/powerapps/developer/common-data-service/create-package-app-appsource).

### <a name="crm-package-availability"></a>CRM paketi kullanılabilirliği

CRM paketinizin müşteriler tarafından kullanılabileceği coğrafi bölgeleri belirtmek için **+ bölge Ekle** ' yi seçin. Aşağıdaki bağımsız bölgelerine dağıtım, sertifika işlemi sırasında özel izin ve doğrulama gerektirir: [Almanya](../../germany/index.yml), [ABD kamu bulutu](../../azure-government/documentation-government-welcome.md)ve ipucu.

Varsayılan olarak, yukarıda girdiğiniz **uygulama yapılandırma URL 'si** her bölge için kullanılacaktır. İsterseniz, bir veya daha fazla belirli bölge için ayrı bir uygulama yapılandırma URL 'SI girebilirsiniz.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

<!-- ## Test drive technical configuration

This page lets you set up a demonstration ("test drive") that allows customers to try your offer before purchasing it. Learn more in [What is test drive](../what-is-test-drive.md).

To enable a test drive, select the **Enable a test drive** check box on the [Offer setup](#test-drive) tab. To remove test drive from your offer, clear this check box.

When you've finished setting up your test drive, select **Save draft** before continuing. -->

## <a name="supplemental-content"></a>Ek içerik

Bu sayfa, teklifinizi doğrulamamıza yardımcı olmak için ek bilgiler sağlamanıza olanak tanır. Bu bilgiler müşterilere gösterilmez veya Market 'te yayımlanmamıştır.

### <a name="key-usage-scenario"></a>Anahtar kullanımı senaryosu

Teklifinizin anahtar kullanımı senaryolarını listeleyen bir PDF dosyasını karşıya yükleyin. Market için teklifinizi onaylamadan önce tüm senaryolar doğrulama takımımız tarafından doğrulanabilir.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="publish"></a>Yayımlama

### <a name="submit-offer-to-preview"></a>Önizlemeye teklif Gönder

Teklifin tüm gerekli bölümlerini tamamladıktan sonra, portalın sağ üst köşesinde bulunan **gözden geçir ve Yayımla** ' yı seçin.

Bu teklifi ilk kez yayımladıysanız şunları yapabilirsiniz:

- Teklifin her bölümü için tamamlanma durumuna bakın.
    - **Başlatılmamış** -bölüm dokunulmamış ve tamamlanması gerekiyor.
    - **Tamamlanmamış** -bölüm düzeltilmesi gereken veya daha fazla bilgi gerektiren hatalar içeriyor. Bölüm (ler) e geri dönün ve güncelleştirin.
    - **Tam** bölüm tamamlandı, gerekli tüm veriler sağlanmış ve hata yok. Teklifi gönderebilmeniz için teklifin tüm bölümlerinin tamamen bir durumda olması gerekir.
- **Sertifika notları** bölümünde, uygulamanızı anlamak için yararlı olan tüm ek notlara ek olarak, uygulamanızın doğru şekilde test edilmesini sağlamak için sertifika ekibine test yönergeleri sağlayın.
- **Gönder**' i seçerek teklifi yayımlamaya gönderebilirsiniz. Teklifin bir önizleme sürümü incelenmek ve onaylanmak için kullanılabilir olduğunda size e-posta göndereceğiz. Iş Ortağı Merkezi 'ne dönün ve teklifinizi herkese açık olarak yayımlamak için **Git-Live** ' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Ticari Market'te bulunan bir teklifi güncelleştirme](./update-existing-offer.md)
