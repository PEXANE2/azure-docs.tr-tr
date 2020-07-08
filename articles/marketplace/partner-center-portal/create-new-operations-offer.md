---
title: Ticari Market 'te bir Dynamics 365 for Operations teklifi oluşturun
description: Azure Market, AppSource veya Microsoft Iş Ortağı Merkezi 'ndeki ticari Market portalını kullanan bulut çözümü sağlayıcısı (CSP) programı aracılığıyla listeleme veya satma için yeni bir Dynamics 365 Işlem teklifi oluşturma.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 06/17/2020
ms.openlocfilehash: 0a2696da70a785fc1da6bfd2c7b0cabae1a89197
ms.sourcegitcommit: 93462ccb4dd178ec81115f50455fbad2fa1d79ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2020
ms.locfileid: "85980704"
---
# <a name="create-a-dynamics-365-for-operations-offer"></a>Dynamics 365 for Operations teklifi oluşturma

Bu konuda, yeni bir Dynamics 365 for Operations teklifinin nasıl oluşturulduğu açıklanmaktadır. [Finans ve işlemler Için Microsoft Dynamics 365](https://dynamics.microsoft.com/finance-and-operations) , gelişmiş finans, işlemler, üretim ve tedarik zinciri yönetimini destekleyen bir kurumsal kaynak planlama (ERP) hizmetidir. Operations için Dynamics 365 için tüm teklifler, sertifika sürecimize gitmelidir.

Başlamadan önce, henüz yapmadıysanız [Iş Ortağı Merkezi 'nde bir ticari Market hesabı oluşturun](create-account.md) . Hesabınızın ticari Market programına kayıtlı olduğundan emin olun.

>[!NOTE]
> Bir teklif yayımlandıktan sonra, Iş Ortağı Merkezi 'nde yapılan teklifle ilgili düzenlemeler yalnızca sistem sırasında ve yeniden yayımlamadan sonra mağazaların ön yüzlerinin güncelleştirilmesini sağlar. Değişiklikleri yaptıktan sonra teklifi yayın için gönderdiğinizden emin olun.

## <a name="create-a-new-offer"></a>Yeni teklif oluşturma

1. [Iş Ortağı Merkezi](https://partner.microsoft.com/dashboard/home)' nde oturum açın.
2. Sol gezinti menüsünde **ticari Market**  >  **genel bakış**' ı seçin.
3. Genel Bakış sayfasında, işlemler için **+ yeni teklif**  >  **Dynamics 365**' i seçin.

    ![Sol gezinti menüsünü gösterir.](./media/new-offer-dynamics-365-operations.png)

> [!NOTE]
> Bir teklif yayımlandıktan sonra, Iş Ortağı Merkezi 'nde yapılan düzenlemeler, teklif yeniden yayımlandıktan sonra yalnızca stoporonts 'de görüntülenir. Değişiklik yaptıktan sonra her zaman yeniden yayımlaytığınızdan emin olun.

## <a name="new-offer"></a>Yeni teklif

Bir **TEKLIF kimliği**girin. Bu, hesabınızdaki her teklif için benzersiz bir tanımlayıcıdır.

- Bu KIMLIK, varsa Market teklifi ve Azure Resource Manager şablonları için Web adresinde müşteriler tarafından görülebilir.
- Yalnızca küçük harfleri ve rakamları kullanın. Kısa çizgi ve alt çizgi içerebilir, ancak boşluk içeremez ve 50 karakterle sınırlıdır. Örneğin, burada **Test-teklif-1** girerseniz, teklif Web adresi olacaktır `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1` .
- Teklif KIMLIĞI, **Oluştur**' u seçtikten sonra değiştirilemez.

Bir **teklif diğer adı**girin. Bu, Iş Ortağı Merkezi 'nde teklif için kullanılan addır.

- Bu ad Market 'te kullanılmıyor ve teklif adından ve müşterilere gösterilen diğer değerlerden farklı.

Teklifi oluşturmak için **Oluştur** ' u seçin ve devam edin.

## <a name="offer-setup"></a>Teklif kurulumu

### <a name="how-do-you-want-potential-customers-to-interact-with-this-listing-offer"></a>Potansiyel müşterilerin bu liste teklifiyle nasıl etkileşime geçmesini istiyorsunuz?

Bu teklif için kullanmak istediğiniz seçeneği belirleyin.

#### <a name="get-it-now-free"></a>Şimdi alın (ücretsiz)

Uygulamanıza erişebilecekleri geçerli bir URL ( *http* veya *https*ile başlayarak) sağlayarak müşterilerinizin teklifinizi ücretsiz olarak listeleyin.  Örneğin, `https://contoso.com/my-app`

#### <a name="free-trial-listing"></a>Ücretsiz deneme (listeleme)

Ücretsiz bir deneme bağlantısı olan müşterilere teklifinizi `http` `https` , bir deneme sunabileceği GEÇERLI bir URL (veya ' den başlayarak) sunarak listeleyin. Örneğin, `https://contoso.com/trial/my-app`. Ücretsiz denemelerdeki teklif listesi, hizmetiniz tarafından oluşturulur, yönetilir ve yapılandırılır ve Microsoft tarafından yönetilen abonelikler içermez.

> [!NOTE]
> Uygulamanızın deneme bağlantınız aracılığıyla alacağı belirteçler, uygulamanızda hesap oluşturmayı otomatikleştirmek için yalnızca Azure Active Directory (Azure AD) aracılığıyla Kullanıcı bilgilerini almak üzere kullanılabilir. Bu belirteci kullanarak kimlik doğrulaması için Microsoft hesapları desteklenmez.

#### <a name="contact-me"></a>Benimle iletişim kurun

Müşteri Ilişkileri yönetimi (CRM) sisteminizi bağlayarak müşteri iletişim bilgilerini toplayın. Müşterinin, bilgilerini paylaşması için izin istenir. Bu müşteri ayrıntıları, teklifinizin bulunduğu teklif adı, KIMLIĞI ve Market kaynağıyla birlikte, yapılandırdığınız CRM sistemine gönderilir. CRM 'nizi yapılandırma hakkında daha fazla bilgi için bkz. [müşteri adayları](#customer-leads).

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

### <a name="app-version"></a>Uygulama sürümü

Teklifinizin sürüm numarasını girin. Müşteriler bu sürümü teklifin ayrıntı sayfasında listelenmiş olarak görür.

### <a name="terms-and-conditions"></a>hüküm ve koşullar

**Hüküm ve koşullar** alanında kendi yasal hüküm ve koşullarınızı belirtin. Ayrıca, hüküm ve koşullarınızın bulunabileceği URL 'YI de sağlayabilirsiniz. Müşterilerinizin teklifinizi deneyebilmeleri için önce bu koşulları kabul etmesi gerekir.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="offer-listing"></a>Teklif listesi

Bu sayfa, teklifinizin listelenecek dilleri görüntüler. Şu anda **İngilizce (Birleşik Devletler)** kullanılabilir tek seçenektir.

Her dil/Pazar için Market ayrıntılarını (teklif adı, açıklama, görüntüler vb.) tanımlamanız gerekir. Bu bilgileri sağlamak için dil/Pazar adını seçin.

> [!NOTE]
> Teklif açıklaması (örneğin, açıklama, belgeler, ekran görüntüleri, kullanım koşulları vb.) için teklif listesi içeriği, "Bu uygulama yalnızca [Ingilizce olmayan dil] içinde kullanılabilir" olarak, Ingilizce olması gerekmez. Ayrıca, teklif listeleme içeriğinde kullanılandan farklı bir dilde içerik sunmak için *yararlı bir bağlantı URL 'si* sağlamak da kabul edilebilir.

Teklif bilgilerinin Microsoft AppSource ' de nasıl göründüğünü gösteren bir örnek aşağıda verilmiştir:

:::image type="content" source="media/example-azure-marketplace-d365-operations.png" alt-text="Bu teklifin Microsoft AppSource nasıl göründüğünü gösterir.":::

#### <a name="call-out-descriptions"></a>Çağrı açıklamaları

1. Logo
2. Ürünler
3. Kategoriler
4. Sektörler
5. Destek adresi (bağlantı)
6. Kullanım koşulları
7. Gizlilik ilkesi
8. Teklif adı
9. Ekran görüntüleri/videolar
10. Description

### <a name="name"></a>Name

Buraya girdiğiniz ad, müşteriler tarafından teklif listelerinizin başlığı olarak gösterilir. Bu alan, teklifi oluştururken **teklif diğer adı** için girdiğiniz metinle önceden doldurulur, ancak bu değeri değiştirebilirsiniz. Bu ad trademarked olabilir (ve ticari marka veya telif hakkı sembolleri dahil edebilirsiniz). Ad 50 karakterden uzun olamaz ve herhangi bir emojıs içeremez.

### <a name="short-description"></a>Kısa açıklama

Teklifinizin en fazla 100 karakter uzunluğunda olması için kısa bir açıklama sağlayın. Bu açıklama Market arama sonuçlarında kullanılabilir.

### <a name="description"></a>Description

[!INCLUDE [Long description-1](./includes/long-description-1.md)]

[!INCLUDE [Long description-2](./includes/long-description-2.md)]

[!INCLUDE [Rich text editor](./includes/rich-text-editor.md)]

### <a name="search-keywords"></a>Anahtar sözcükleri ara

Müşterilerin teklifinizi Market 'te bulmasına yardımcı olmak için isteğe bağlı en fazla üç arama anahtar sözcüğü girebilirsiniz. En iyi sonuçlar için, tanımlarınızda bu anahtar sözcükleri de kullanmayı deneyin.

### <a name="products-your-app-works-with"></a>Uygulamanızın ile birlikte çalışarak ürünler

Müşterilerinizin uygulamanızın belirli ürünlerle birlikte çalışıp çalışmadığını bilmesini istiyorsanız buraya en fazla üç ürün adı girin.

### <a name="support-urls"></a>Destek URL 'Leri

Bu bölüm, müşterilerinizin teklifiniz hakkında daha fazla bilgi sağlamasına yardımcı olacak bağlantılar sağlamanıza olanak tanır.

#### <a name="help-link"></a>Yardım bağlantısı

Müşterilerinizin teklifiniz hakkında daha fazla bilgi sağlayabilecekleri URL 'YI girin.

#### <a name="privacy-policy-url"></a>Gizlilik ilkesi URL 'SI

Kuruluşunuzun gizlilik ilkesinin URL 'sini girin. Uygulamanızın gizlilik yasaları ve yönetmeliklerle uyumlu olmasını sağlamaktan ve geçerli bir gizlilik ilkesi sağlamaya yönelik siz sorumlusunuz.

### <a name="contacts"></a>Kişiler

Bu bölümde, bir **destek kişisi** ve **mühendislik ilgili kişisi**için ad, e-posta ve telefon numarası girin. Bu bilgiler müşterilere gösterilmez, ancak Microsoft tarafından kullanılabilir ve CSP iş ortakları için de sağlanıyor olabilir.

**Destek kişisi** bölümünde, CSP iş ortaklarının teklifiniz için destek BULABILECEĞI **Destek URL** 'sini sağlayın.

### <a name="supporting-documents"></a>Destekleyici belgeler

Burada, teknik incelemeler, broşürler, denetim listeleri veya sunular gibi en az bir (en fazla üç) ilgili pazarlama belgesi sağlayın. Bu belgeler PDF biçiminde olmalıdır.

### <a name="marketplace-images"></a>Market görüntüleri

Bu bölümde, müşteri teklifinizi gösterirken kullanılacak logo ve görüntüler sağlayabilirsiniz. Tüm görüntülerin. png biçiminde olması gerekir.

>[!Note]
>Dosya yükleme sorununuz varsa, yerel ağınızın https://upload.xboxlive.com Iş Ortağı Merkezi tarafından kullanılan hizmeti engellemediğinden emin olun.

#### <a name="store-logos"></a>Mağaza logoları

Teklifinizin logosunu iki piksel boyutunda sağlar:

- **Küçük** (48 x 48)
- **Büyük** (216 x 216)


#### <a name="screenshots"></a>Ekran görüntüleri

Teklifinizin nasıl çalıştığını gösteren ekran görüntüleri ekleyin. En az bir ekran görüntüsü gereklidir ve en fazla beş tane ekleyebilirsiniz. Tüm ekran görüntüleri 1280 x 720 piksel olmalıdır.

#### <a name="videos"></a>Videolar

İsteğe bağlı olarak, teklifinizi gösteren dört adede kadar video ekleyebilirsiniz. Bu videoların YouTube ve/veya Vimeo 'da barındırılması gerekir. Her biri için videonun adını, URL 'sini ve videonun küçük bir görüntüsünü girin (1280 x 720 piksel)

#### <a name="additional-marketplace-listing-resources"></a>Ek Market listeleme kaynakları

[Market teklif listeleri için en iyi uygulamalar](../gtm-offer-listing-best-practices.md)

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="availability"></a>Kullanılabilirlik

Bu sayfa, teklifinizin nerede ve nasıl kullanılabileceğini gösteren seçenekler sağlar.

### <a name="markets"></a>Pazar

Bu bölüm, teklifinizin kullanılabilir olması gereken pazarları belirtmenize olanak tanır. Bunu yapmak için **Pazar seçimi** açılır penceresini görüntüleyen **pazarları Düzenle '** yi seçin.

Varsayılan olarak, bir piyasa seçili değildir. Teklifinizi yayımlamak için en az bir pazar seçin. Teklifinizin her olası pazarda kullanılabilmesini sağlamak için **Tümünü Seç** ' e tıklayın veya eklemek istediğiniz belirli pazarları seçin. İşiniz bittiğinde **Kaydet**' i seçin.

Burada yaptığınız seçimler yalnızca yeni alımlar için geçerlidir; zaten belirli bir pazar ortamında uygulamanız varsa ve daha sonra bu pazarı kaldırırsanız, bu pazarda zaten sunulan bir pazara sahip kişiler bunu kullanmaya devam edebilir, ancak bu pazardaki yeni müşteriler teklifinizi alabilir.

> [!IMPORTANT]
> Bu gereksinimler burada veya Iş Ortağı Merkezi 'nde listelenmese de, yerel yasal gereksinimleri karşılamak sizin sorumluluğunuzdadır.

Tüm pazarlar ' i seçtiğinizde, yerel yasalar ve kısıtlamalar ya da diğer faktörler bazı ülkelerde ve bölgelerde belirli tekliflerin listelenmesini engelleyebilse de aklınızda bulundurun.

### <a name="preview-audience"></a>İzleyiciyi Önizle

Teklifinizi daha geniş Market teklifiyle yayımlamadan önce, önce onu sınırlı bir **Önizleme hedef kitlesi**için kullanılabilir hale getirmeniz gerekir. Burada bir **gizleme anahtarı** (yalnızca küçük harf ve/veya sayı kullanan herhangi bir dize) girin. Önizleme hedef kitlenizin üyeleri, Market 'te teklifinizin önizlemesini görüntülemek için bu anahtarı bir belirteç olarak kullanabilir.

Daha sonra, teklifinizi kullanılabilir duruma getirmek ve önizleme kısıtlamasını kaldırmak için hazır olduğunuzda, **gizleme anahtarını** kaldırmanız ve yeniden yayımlamanız gerekir.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="technical-configuration"></a>Teknik yapılandırma

Bu sayfa, teklifiniz ile bağlantı kurmak için kullanılan teknik ayrıntıları tanımlar. Bu bağlantı, teklifi edinmeyi tercih ettiklerinde, son müşteri için teklifinizi sağlamamızı sağlar.

### <a name="solution-identifier"></a>Çözüm tanımlayıcısı

Çözümünüz için çözüm tanımlayıcısı (GUID) sağlayın.

Çözüm tanımlarınızı bulmak için:

1. Microsoft Dynamics yaşam döngüsü Hizmetleri 'nde (LCS) **Çözüm Yönetimi**' ni seçin.
2. Çözümünüzü seçin, sonra **pakete genel bakış**bölümünde **çözüm tanımlayıcısı** ' nı arayın. Tanımlayıcı boşsa, **düzenleme** yapın ve paketinizi yeniden yayımlayın, sonra yeniden deneyin.

### <a name="release-version"></a>Yayın sürümü

Finans için Dynamics 365 sürümünü ve bu çözümün birlikte çalıştığına yönelik Işlemleri seçin.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="test-drive-technical-configuration"></a>Test sürüşü teknik yapılandırması

Bu sayfa, müşterilerin satın almadan önce teklifinizi denemesini sağlayan bir tanıtım ("test sürücüsü") ayarlamanıza olanak sağlar. [Test sürücüsü nedir?](../what-is-test-drive.md)bölümünde daha fazla bilgi edinin.

Bir sınama sürücüsünü etkinleştirmek için [teklif kurulumu](#test-drive) sekmesinde **bir test sürücüsünü etkinleştir** onay kutusunu işaretleyin. Test sürücüsünü teklifinizden kaldırmak için bu onay kutusunu temizleyin.

Test sürücünüzü ayarlamayı bitirdiğinizde devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="supplemental-content"></a>Ek içerik

Bu sayfa teklifinizi doğrulamamıza yardımcı olmak için teklifiniz hakkında ek bilgiler sağlamanıza olanak tanır. Bu bilgiler müşterilere gösterilmez veya Market 'te yayımlanmamıştır.

### <a name="validation-assets"></a>Doğrulama varlıkları

Bu bölüme bir [Özelleştirme analizi raporu (otomobil)](https://docs.microsoft.com/dynamics365/unified-operations/dev-itpro/dev-tools/customization-analysis-report) yükleyin. Bu rapor, özelleştirme ve uzantı modelleriniz analiz edilirken, önceden tanımlanmış en iyi yöntem kuralları kümesine göre oluşturulur.

Bu dosya. xls veya. xlsx biçiminde olmalıdır. Birden fazla raporunuz varsa, tüm raporları içeren bir. zip dosyasını karşıya yükleyebilirsiniz.

### <a name="does-solution-include-localizations"></a>Çözüm yerelleştirmeler içeriyor mu?

Çözüm yerel standartlar ve ilkelerin kullanımına izin verirseniz (örneğin, farklı ülkeler/bölgeler için gereken farklı Bordro kurallarına uygunsa) **Evet** ' i seçin. Gerekmiyorsa **Hayır**'ı seçin.

### <a name="does-solution-enable-translations"></a>Çözüm çevirileri etkinleştirir mi?

Çözümünüzdeki metin diğer dillere çevrilebilmesi için **Evet** yanıtını verin. Gerekmiyorsa **Hayır**'ı seçin.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="publish"></a>Yayımlama

### <a name="submit-offer-to-preview"></a>Önizlemeye teklif Gönder

Teklifin tüm gerekli bölümlerini tamamladıktan sonra, portalın sağ üst köşesinde bulunan **gözden geçir ve Yayımla** ' yı seçin.

Bu teklifi ilk kez yayımladıysanız şunları yapabilirsiniz:

- Teklifin her bölümü için tamamlanma durumuna bakın.
    - **Başlatılmamış** – bölüm dokunulmamış ve tamamlanmalıdır.
    - **Tamamlanmamış** – bölümde düzeltilmesi gereken hatalar var veya daha fazla bilgi sağlanması gerekiyor. Bölüm (ler) e geri dönün ve güncelleştirin.
    - **Tamamlandı** – bölüm tamamlandı, tüm gerekli veriler sağlanmış ve hata yok. Teklifi gönderebilmeniz için teklifin tüm bölümlerinin tamamen bir durumda olması gerekir.
- **Sertifika notları** bölümünde, uygulamanızı anlamak için yararlı olan tüm ek notlara ek olarak, uygulamanızın doğru şekilde test edilmesini sağlamak için sertifika ekibine test yönergeleri sağlayın.
- **Gönder**' i seçerek teklifi yayımlamaya gönderebilirsiniz. Size, gözden geçirmeniz ve onaylamanız için teklifin bir önizleme sürümünün ne zaman kullanılabileceğini bilmenizi sağlayacak bir e-posta göndereceğiz. Iş Ortağı Merkezi 'ne dönün ve teklifinizi herkese açık bir şekilde **yayımlamak için teklif ' i seçin.**

## <a name="next-step"></a>Sonraki adım

- [Ticari Market'te bulunan bir teklifi güncelleştirme](./update-existing-offer.md)
