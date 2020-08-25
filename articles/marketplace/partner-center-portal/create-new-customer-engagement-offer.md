---
title: Microsoft ticari marketi 'nde & PowerApps teklifi için Dynamics 365 for Customer Engagement oluşturun
description: Azure Marketi, AppSource veya Iş Ortağı Merkezi 'nde bulut çözümü sağlayıcısı (CSP) programı aracılığıyla listelemek veya satmaya yönelik yeni bir Dynamics 365 müşteri katılımı & oluşturma.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: navits09
ms.author: navits
ms.date: 06/17/2020
ms.openlocfilehash: 93e7a720e9d39eca6d808c8c49b3b340226997ff
ms.sourcegitcommit: d39f2cd3e0b917b351046112ef1b8dc240a47a4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88817043"
---
# <a name="create-a-dynamics-365-for-customer-engagement--powerapps-offer"></a>Dynamics 365 for Customer Engagement ve PowerApps teklifi oluşturma

Bu konuda, PowerApps teklifinin & yeni bir Dynamics 365 müşteri katılımı için nasıl oluşturulacağı açıklanmaktadır. Müşteri katılımı (PowerApps, Sales, Service, Project Service ve alan hizmeti) için Dynamics 365 için tüm uygulamalar, sertifika sürecimize gidip bir deneme deneyimini desteklemelidir. Sertifika işlemi, çözümünüzü standart gereksinimler, uyumluluk ve uygun uygulamalar için denetler. Deneme deneyimi, kullanıcıların çözümünüzü canlı bir Dynamics 365 ortamına dağıtmasını sağlar.

Başlamadan önce, henüz yapmadıysanız [Iş Ortağı Merkezi 'nde bir ticari Market hesabı oluşturun](create-account.md) . Hesabınızın ticari Market programına kayıtlı olduğundan emin olun.

## <a name="create-a-new-offer"></a>Yeni teklif oluşturma

1. [Iş Ortağı Merkezi](https://partner.microsoft.com/dashboard/home)' nde oturum açın.
2. Sol gezinti menüsünde **ticari Market**  >  **genel bakış**' ı seçin.
3. Genel Bakış sayfasında, **+ New offer**  >  **PowerApps & müşteri katılımı için**+ yeni teklif Dynamics 365 ' i seçin.

    ![Sol gezinti menüsünü gösterir.](./media/new-offer-dynamics-365-customer-engagement-powerapps.png)

> [!NOTE]
> Bir teklif yayımlandıktan sonra, Iş Ortağı Merkezi 'nde yapılan düzenlemeler, teklif yeniden yayımlandıktan sonra yalnızca çevrimiçi mağazalarda görünür. Değişiklik yaptıktan sonra her zaman yeniden yayımlaytığınızdan emin olun.

## <a name="new-offer"></a>Yeni teklif

Bir **TEKLIF kimliği**girin. Bu, hesabınızdaki her teklif için benzersiz bir tanımlayıcıdır.

- Bu KIMLIK, varsa Market teklifi ve Azure Resource Manager şablonları için Web adresinde müşteriler tarafından görülebilir.
- Yalnızca küçük harfleri ve rakamları kullanın. Kısa çizgi ve alt çizgi içerebilir, ancak boşluk içeremez ve 50 karakterle sınırlıdır. Örneğin, **Test-teklif-1**girerseniz, teklif Web adresi olacaktır `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1` .
- Teklif KIMLIĞI, **Oluştur**' u seçtikten sonra değiştirilemez.

Bir **teklif diğer adı**girin. Bu, Iş Ortağı Merkezi 'nde teklif için kullanılan addır.

- Bu ad Market 'te kullanılmıyor ve teklif adından ve müşterilere gösterilen diğer değerlerden farklı.
- Bu, **Oluştur**' u seçtikten sonra değiştirilemez.

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

Devam etmeden önce **Taslağı kaydet** ' i seçin.

### <a name="test-drive"></a>Sınama sürücüsü

Test sürücüsü, "satın almadan önce dene" seçeneği sunarak olası müşterilere teklifinizi göstermek için harika bir yoldur. bu sayede, artan dönüştürmeye ve yüksek oranda nitelikli müşteri adaylarının oluşturulmasına neden olur. Daha fazla bilgi edinmek için, [test sürüşü](../what-is-test-drive.md)ile başlayın.

Bir sınama sürücüsünü sabit bir süre etkinleştirmek için, **bir sınama sürücüsünü etkinleştir** onay kutusunu seçin. Test sürücüsünü teklifinizden kaldırmak için bu onay kutusunu temizleyin.

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

### <a name="applicable-dynamics-365-products"></a>Geçerli Dynamics 365 ürünleri

Bu teklifin uygulandığı tüm Dynamics 365 ürünlerini seçin.

### <a name="app-version"></a>Uygulama sürümü

Teklifinizin sürüm numarasını girin. Müşteriler bu sürümü teklifin ayrıntı sayfasında listelenmiş olarak görür. Yalnızca pazarlama/tanımlayıcı değişiklikler nedeniyle sürüm numarasını güncelleştiriyorsanız **yalnızca pazarlama değişiklik** kutusunu işaretleyin. Bu seçenek, teklifin sertifika ve sağlama aşamalarını atlamasına izin verir.

### <a name="terms-and-conditions"></a>hüküm ve koşullar

Burada kendi yasal hüküm ve koşullarınızı sağlayın. Hüküm ve koşullarınızın bulunabileceği adresi de sağlayabilirsiniz. Müşterilerinizin teklifinizi deneyebilmeleri için önce bu koşulları kabul etmesi gerekir.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="offer-listing"></a>Teklif listesi

Bu sayfa, teklifinizin listelenecek dilleri görüntüler. Şu anda **İngilizce (Birleşik Devletler)** kullanılabilir tek seçenektir.

Buradaki teklif adı, açıklama ve görüntüler gibi her bir dil/Pazar için Market ayrıntılarını tanımlayın. Bu bilgileri sağlamak için dil/Pazar adını seçin.

> [!NOTE]
> Teklif açıklaması (Açıklama, belgeler, ekran görüntüleri ve kullanım koşulları gibi) sunan teklif listesinin Ingilizce olması gerekmez, "Bu uygulama yalnızca [Ingilizce olmayan dilde] kullanılabilir". Ayrıca, teklif listeleme içeriğinde kullanılandan farklı bir dilde içerik sunmak için *yararlı bir bağlantı URL 'si* sağlamak da kabul edilebilir.

Teklif bilgilerinin Microsoft AppSource nasıl göründüğünü gösteren bir örnek aşağıda verilmiştir (listelenen tüm fiyatlar yalnızca örnek amaçlıdır ve gerçek maliyetlerin yansıtılmasına yönelik değildir):

:::image type="content" source="media/example-azure-marketplace-d365-customer-engagement.png" alt-text="Bu teklifin Microsoft AppSource nasıl göründüğünü gösterir.":::

#### <a name="call-out-descriptions"></a>Çağrı açıklamaları

1. Logo
2. Ürünler
3. Kategoriler
4. Destek adresi (bağlantı)
5. Kullanım koşulları adresi (bağlantı)
6. Teklif adı
7. Açıklama
8. Ekran görüntüleri/videolar

### <a name="name"></a>Name

Buraya girdiğiniz ad, müşteriler tarafından teklif listelerinizin başlığı olarak gösterilir. Bu alan, teklifi oluştururken **teklif diğer adı** için girdiğiniz metinle önceden doldurulur, ancak bu değeri değiştirebilirsiniz. Bu ad trademarked olabilir (ve ticari marka veya telif hakkı sembolleri dahil edebilirsiniz). Ad 50 karakterden uzun olamaz ve herhangi bir emojıs içeremez.

### <a name="short-description"></a>Kısa açıklama

Teklifinizin en fazla 100 karakter uzunluğunda olması için kısa bir açıklama sağlayın. Bu açıklama Market arama sonuçlarında kullanılabilir.

### <a name="description"></a>Açıklama

[!INCLUDE [Long description-1](./includes/long-description-1.md)]

[!INCLUDE [Long description-2](./includes/long-description-2.md)]

[!INCLUDE [Rich text editor](./includes/rich-text-editor.md)]

### <a name="search-keywords"></a>Anahtar sözcükleri ara

Müşterilerin teklifinizi Market 'te bulmasına yardımcı olmak için isteğe bağlı en fazla üç arama anahtar sözcüğü girebilirsiniz. En iyi sonuçlar için, tanımlarınızda bu anahtar sözcükleri de kullanın.

### <a name="products-your-app-works-with"></a>Uygulamanızın ile birlikte çalışarak ürünler

Müşterilerinizin uygulamanızın belirli ürünlerle birlikte çalışıp çalışmadığını bilmesini istiyorsanız buraya en fazla üç ürün adı girin.

### <a name="support-urls"></a>Destek URL 'Leri

Bu bölüm, müşterilerinizin teklifiniz hakkında daha fazla bilgi sağlamasına yardımcı olacak bağlantılar sağlamanıza olanak tanır.

#### <a name="help-link"></a>Yardım bağlantısı

Müşterilerinizin teklifiniz hakkında daha fazla bilgi sağlayabilecekleri adresi girin.

#### <a name="privacy-policy-url"></a>Gizlilik ilkesi URL 'SI

Kuruluşunuzun gizlilik ilkesinin adresini girin. Uygulamanızın gizlilik yasaları ve yönetmeliklerle uyumlu olmasını sağlamaktan ve geçerli bir gizlilik ilkesi sağlamaya yönelik siz sorumlusunuz.

### <a name="contacts"></a>Kişiler

Bir **destek kişisi** ve **mühendislik ilgili kişisi**için ad, e-posta ve telefon numarası girin. Bu bilgiler müşterilere gösterilmez, ancak Microsoft tarafından kullanılabilir ve CSP iş ortakları için de sağlanıyor olabilir.

**Destek kişisi** bölümünde, CSP iş ortaklarının teklifiniz için destek BULABILECEĞI **Destek URL** 'sini de sağlamanız gerekir.

### <a name="supporting-documents"></a>Destekleyici belgeler

PDF biçiminde teknik incelemeler, broşürler, denetim listeleri veya sunular gibi en az bir (en fazla üç) ilgili pazarlama belgesi sağlayın.

### <a name="marketplace-images"></a>Market görüntüleri

Teklifinizin logolarını ve görüntülerini sağlayın. Tüm görüntülerin PNG biçiminde olması gerekir.

[!INCLUDE [logotips](../includes/graphics-suggestions.md)]

>[!NOTE]
>Dosya yükleme sorununuz varsa, yerel ağınızın https://upload.xboxlive.com Iş Ortağı Merkezi tarafından kullanılan hizmeti engellemediğinden emin olun.

#### <a name="store-logos"></a>Mağaza logoları

**Büyük** boyutlu amblem IÇIN bir PNG dosyası sağlayın. İş Ortağı Merkezi, **küçük** bir logo oluşturmak için bunu kullanacaktır. İsterseniz bunu daha sonra farklı bir görüntüyle değiştirebilirsiniz.

- **Büyük** (216 x 216-350 x 350 px, gereklidir)
- **Küçük** (48 x 48 piksel, isteğe bağlı)

Bu amblemler, listede farklı yerlerde kullanılır:

[!INCLUDE [logos-appsource-only](../includes/logos-appsource-only.md)]

[!INCLUDE [Logo tips](../includes/graphics-suggestions.md)]

#### <a name="screenshots"></a>Ekran görüntüleri

Teklifinizin nasıl çalıştığını gösteren ekran görüntüleri ekleyin. En az bir ekran görüntüsü gereklidir ve en fazla beş tane ekleyebilirsiniz. Tüm ekran görüntüleri 1280 x 720 piksel olmalıdır.

#### <a name="videos"></a>Videolar

İsteğe bağlı olarak, teklifinizi gösteren dört adede kadar video ekleyebilirsiniz. Bu videoların YouTube ve/veya Vimeo 'da barındırılması gerekir. Her biri için videonun adını, URL 'sini ve videonun küçük bir görüntüsünü girin (1280 x 720 piksel)

#### <a name="additional-marketplace-listing-resources"></a>Ek Market listeleme kaynakları

[Market teklif listeleri için en iyi uygulamalar](../gtm-offer-listing-best-practices.md)

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="availability"></a>Kullanılabilirlik

Bu sayfa, teklifinizin nerede ve nasıl kullanılabileceğini tanımlamanıza olanak sağlar.

### <a name="markets"></a>Pazar

Teklifinizin kullanılabilir olması gereken pazarları belirtin. Bunu yapmak için **Pazar seçimi** açılır penceresini görüntüleyen **pazarları Düzenle**' yi seçin.

Varsayılan olarak, bir pazar seçili değildir, ancak teklifinizi yayımlamak için en az bir pazar seçmeniz gerekir. Teklifinizin her olası pazarda kullanılabilmesini sağlamak için  **Tümünü Seç** ' i seçin veya eklemek istediğiniz belirli pazarları seçin.

Burada yaptığınız seçimler yalnızca yeni alımlar için geçerlidir; zaten belirli bir pazar ortamında uygulamanız varsa ve daha sonra bu pazarı kaldırırsanız, bu pazar ortamında zaten teklif sahibi olan kişiler bunu kullanmaya devam edebilir ancak söz konusu pazardaki yeni müşteriler teklifinizi alabilir.

> [!IMPORTANT]
> Bu gereksinimler burada veya Iş Ortağı Merkezi 'nde listelenmese de, yerel yasal gereksinimleri karşılamak sizin sorumluluğunuzdadır. Tüm pazarlar ' ı seçseniz bile, yerel yasalar ve kısıtlamalar veya diğer faktörler belirli tekliflerin bazı ülkelerde ve bölgelerde listelenmesine engel olabilir.

### <a name="preview-audience"></a>İzleyiciyi Önizle

Teklifinizi daha geniş Market teklifiyle yayımlamadan önce, önce onu sınırlı bir **Önizleme hedef kitlesi**için kullanılabilir hale getirmeniz gerekir. Burada bir **gizleme anahtarı** (yalnızca küçük harf ve/veya sayı kullanan herhangi bir dize) girin. Önizleme hedef kitlenizin üyeleri, Market 'te teklifinizin önizlemesini görüntülemek için bu anahtarı bir belirteç olarak kullanabilir.

Daha sonra, teklifinizi kullanılabilir duruma getirmek ve önizleme kısıtlamasını kaldırmak için hazır olduğunuzda, **gizleme anahtarını** kaldırmanız ve yeniden yayımlamanız gerekir.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="technical-configuration"></a>Teknik yapılandırma

Bu sayfa, teklifiniz ile bağlantı kurmak için kullanılan teknik ayrıntıları tanımlar. Bu bağlantı, teklifi edinmeyi tercih ettiklerinde, son müşteri için teklifinizi sağlamamızı sağlar.

### <a name="base-license-model"></a>Temel lisans modeli

Temel lisans modeli, müşterilerin uygulamanız için CRM Yönetim merkezinde nasıl atandığını belirler. Lisanslara kiracı başına bir tane atanırsa, örnek tabanlı lisanslama veya **Kullanıcı** için **kaynak** seçin.

### <a name="requires-s2s-outbound-and-crm-secure-store-access"></a>S2S giden ve CRM güvenli mağaza erişimi gerektirir

CRM güvenli depolama veya sunucudan sunucuya (S2S) giden erişim yapılandırmasını etkinleştirmek için bu kutuyu işaretleyin. Bu özellik, sertifika aşamasında Dynamics 365 ekibinin özelleşmiş bir şekilde dikkate alınması gerekir. Microsoft, bu özelliği desteklemeye yönelik ek adımları tamamlayabilmeniz için sizinle iletişim kuracaktır.

### <a name="application-configuration-url"></a>Uygulama yapılandırma URL 'SI

Müşterinin uygulamanızı yapılandırmasını sağlayan yapılandırma Web sayfasının URL 'sini sağlayın.

### <a name="crm-package"></a>CRM paketi

**Paket konumu URL 'si** alanına KARŞıYA yüklenen CRM paketi. zip dosyasını Içeren bir Azure Blob depolama hesabının URL 'sini girin. Microsoft 'un, doğrulamaya yönelik paketinizi görebilmesi için, URL 'ye bir salt okunurdur SAS anahtarı ekleyin.

> [!IMPORTANT]
> Bir yayımlama bloğundan kaçınmak için, blob depolamanın URL 'sindeki sona erme tarihinin süresinin dolmadığından emin olun. İlkenize erişerek tarihi gözden geçirebilir. **Süre sonu zamanının** gelecekte en az bir ay olması önerilir.

Varsa, **paket dosya içinde birden fazla CRM paketi olduğu**etiketli kutuyu seçin. Bu durumda, tüm paketleri. zip dosyanıza eklediğinizden emin olun.

Paketinizi oluşturma ve yapısını güncelleştirme hakkında ayrıntılı bilgi için bkz. [3. Adım: uygulamanız Için AppSource paketi oluşturma](https://docs.microsoft.com/powerapps/developer/common-data-service/create-package-app-appsource).

### <a name="crm-package-availability"></a>CRM paketi kullanılabilirliği

Bu bölümde, CRM paketinizin müşteriler tarafından kullanılabilir olacağı coğrafi bölgeleri belirtmek için **+ bölge Ekle** ' yi seçin. Aşağıdaki bağımsız bölgelerine dağıtım, sertifika işlemi sırasında özel izin ve doğrulama gerektirir: [Almanya](../../germany/index.yml), [ABD kamu bulutu](../../azure-government/documentation-government-welcome.md)ve ipucu.

Varsayılan olarak, yukarıda girdiğiniz **uygulama yapılandırma URL 'si** her bölge için kullanılacaktır. İsterseniz, bir veya daha fazla belirli bölge için ayrı bir uygulama yapılandırma URL 'SI girebilirsiniz. 

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="test-drive-technical-configuration"></a>Test sürüşü teknik yapılandırması

Bu sayfa, müşterilerin satın almadan önce teklifinizi denemesini sağlayan bir tanıtım ("test sürücüsü") ayarlamanıza olanak sağlar. [Test sürücüsü nedir?](../what-is-test-drive.md)bölümünde daha fazla bilgi edinin.

Bir sınama sürücüsünü etkinleştirmek için [teklif kurulumu](#test-drive) sekmesinde **bir test sürücüsünü etkinleştir** onay kutusunu işaretleyin. Test sürücüsünü teklifinizden kaldırmak için bu onay kutusunu temizleyin.

Test sürücünüzü ayarlamayı bitirdiğinizde devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="supplemental-content"></a>Ek içerik

Bu sayfa teklifinizi doğrulamamıza yardımcı olmak için teklifiniz hakkında ek bilgiler sağlamanıza olanak tanır. Bu bilgiler müşterilere gösterilmez veya Market 'te yayımlanmamıştır.

### <a name="key-usage-scenario"></a>Anahtar kullanımı senaryosu

Teklifinizin anahtar kullanımı senaryolarını listeleyen bir PDF dosyasını karşıya yükleyin. Market için teklifinizi onaylamadan önce tüm senaryolar doğrulama takımımız tarafından doğrulanabilir.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="publish"></a>Yayımlama

### <a name="submit-offer-to-preview"></a>Önizlemeye teklif Gönder

Teklifin tüm gerekli bölümlerini tamamladıktan sonra, portalın sağ üst köşesinde bulunan **gözden geçir ve Yayımla** ' yı seçin.

Bu teklifi ilk kez yayımladıysanız şunları yapabilirsiniz:

- Teklifin her bölümü için tamamlanma durumuna bakın.
    - **Başlatılmamış** -bölüm dokunulmamış ve tamamlanmalıdır.
    - **Tamamlanmamış** -bölümde düzeltilmesi gereken hatalar var veya daha fazla bilgi sağlanması gerekiyor. Bölüm (ler) e geri dönün ve güncelleştirin.
    - **Tamamlandı** -bölüm tamamlandı, tüm gerekli veriler sağlanmış ve hata yok. Teklifi gönderebilmeniz için teklifin tüm bölümlerinin tamamen bir durumda olması gerekir.
- **Sertifika notları** bölümünde, uygulamanızı anlamak için yararlı olan tüm ek notlara ek olarak, uygulamanızın doğru şekilde test edilmesini sağlamak için sertifika ekibine test yönergeleri sağlayın.
- **Gönder**' i seçerek teklifi yayımlamaya gönderebilirsiniz. Size, gözden geçirmeniz ve onaylamanız için teklifin bir önizleme sürümünün ne zaman kullanılabileceğini bilmenizi sağlayacak bir e-posta göndereceğiz. Iş Ortağı Merkezi 'ne dönün ve genel kullanıma yayımlamak üzere teklif için **Go-Live** ' ı seçin.

## <a name="next-step"></a>Sonraki adım

- [Ticari Market'te bulunan bir teklifi güncelleştirme](./update-existing-offer.md)
