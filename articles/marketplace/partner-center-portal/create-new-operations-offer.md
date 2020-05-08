---
title: Ticari Market 'te bir Dynamics 365 for Operations teklifi oluşturun
description: Azure Market, AppSource veya Microsoft Iş Ortağı Merkezi 'ndeki ticari Market portalını kullanan bulut çözümü sağlayıcısı (CSP) programı aracılığıyla listeleme veya satma için yeni bir Dynamics 365 Işlem teklifi oluşturma.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: df618e508590f047fb015a8be6c4934d05c04fa9
ms.sourcegitcommit: 366e95d58d5311ca4b62e6d0b2b47549e06a0d6d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82692573"
---
# <a name="create-a-dynamics-365-for-operations-offer"></a>Dynamics 365 for Operations teklifi oluşturma

Bu konuda, yeni bir Dynamics 365 for Operations teklifinin nasıl oluşturulduğu açıklanmaktadır. [Finans ve işlemler Için Microsoft Dynamics 365](https://dynamics.microsoft.com/finance-and-operations) , gelişmiş finans, işlemler, üretim ve tedarik zinciri yönetimini destekleyen bir kurumsal kaynak planlama (ERP) hizmetidir. Operations için Dynamics 365 için tüm teklifler, sertifika sürecimize gitmelidir.

Başlamadan önce, henüz yapmadıysanız [Iş Ortağı Merkezi 'nde bir ticari Market hesabı oluşturun](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account) . Hesabınızın ticari Market programına kayıtlı olduğundan emin olun.

>[!NOTE]
> Bir teklif yayımlandıktan sonra, Iş Ortağı Merkezi 'nde yapılan teklifle ilgili düzenlemeler yalnızca sistem sırasında ve yeniden yayımlamadan sonra mağazaların ön yüzlerinin güncelleştirilmesini sağlar. Değişiklikleri yaptıktan sonra teklifi yayın için gönderdiğinizden emin olun.

## <a name="create-a-new-offer"></a>Yeni teklif oluşturma

1. [Iş Ortağı Merkezi](https://partner.microsoft.com/dashboard/home)' nde oturum açın.
2. Sol gezinti menüsünde **ticari Market** > **genel bakış**' ı seçin.
3. Genel Bakış sayfasında, işlemler için **+ yeni teklif** > **Dynamics 365**' i seçin.

    ![Sol gezinti menüsünü gösterir.](./media/new-offer-dynamics-365-ops.png)

> [!NOTE]
> Bir teklif yayımlandıktan sonra, Iş Ortağı Merkezi 'nde yapılan düzenlemeler, teklif yeniden yayımlandıktan sonra yalnızca stoporonts 'de görüntülenir. Değişiklik yaptıktan sonra her zaman yeniden yayımlaytığınızdan emin olun.

## <a name="new-offer"></a>Yeni teklif

Bir **TEKLIF kimliği**girin. Bu, hesabınızdaki her teklif için benzersiz bir tanımlayıcıdır.

- Bu KIMLIK, varsa Market teklifi ve Azure Resource Manager şablonları için Web adresinde müşteriler tarafından görülebilir.
- Yalnızca küçük harfleri ve rakamları kullanın. Kısa çizgi ve alt çizgi içerebilir, ancak boşluk içeremez ve 50 karakterle sınırlıdır. Örneğin, burada **Test-teklif-1** girerseniz, teklif Web adresi olacaktır `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
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

Ücretsiz bir deneme bağlantısı olan müşterilere teklifinizi, bir deneme sunabileceği geçerli bir URL ( *http* veya *https*ile başlayarak) sunarak listeleyin. Örneğin, `https://contoso.com/trial/my-app`. Ücretsiz denemelerdeki teklif listesi, hizmetiniz tarafından oluşturulur, yönetilir ve yapılandırılır ve Microsoft tarafından yönetilen abonelikler içermez.

> [!NOTE]
> Uygulamanızın deneme bağlantınız aracılığıyla alacağı belirteçler, uygulamanızda hesap oluşturmayı otomatikleştirmek için yalnızca Azure Active Directory (Azure AD) aracılığıyla Kullanıcı bilgilerini almak üzere kullanılabilir. Bu belirteci kullanarak kimlik doğrulaması için Microsoft hesapları desteklenmez.

#### <a name="contact-me"></a>Benimle iletişim kurun

Müşteri Ilişkileri yönetimi (CRM) sisteminizi bağlayarak müşteri iletişim bilgilerini toplayın. Müşterinin, bilgilerini paylaşması için izin istenir. Bu müşteri ayrıntıları, teklifinizin bulunduğu teklif adı, KIMLIĞI ve Market kaynağıyla birlikte, yapılandırdığınız CRM sistemine gönderilir. CRM 'nizi yapılandırma hakkında daha fazla bilgi için bkz. [Connect lider yönetimi](#connect-lead-management).

### <a name="test-drive"></a>Sınama sürücüsü

Test sürücüsü, "satın almadan önce dene" seçeneği sunarak olası müşterilere teklifinizi göstermek için harika bir yoldur. bu sayede, artan dönüştürmeye ve yüksek oranda nitelikli müşteri adaylarının oluşturulmasına neden olur. [Sınama sürücüleri hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive).

Bir sınama sürücüsünü sabit bir süre etkinleştirmek için, **bir sınama sürücüsünü etkinleştir** onay kutusunu seçin. Test sürücüsünü teklifinizden kaldırmak için bu onay kutusunu temizleyin. Bu konunun ilerleyen kısımlarında, [test sürücüsü teknik yapılandırma](#test-drive-technical-configuration) bölümünde test sürücüsü ortamını yapılandırın.

Daha fazla bilgi için bkz. [ticari Market 'te teklifinizin test](https://docs.microsoft.com/azure/marketplace/partner-center-portal/test-drive)etme.

#### <a name="type-of-test-drive"></a>Test sürücüsünün türü

Aşağıdaki seçeneklerden seçim yapın:

- **[Azure Resource Manager](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive)** : çözümünüzü oluşturan tüm Azure kaynaklarını içeren bir dağıtım şablonu. Bu senaryoya uyan ürünler yalnızca Azure kaynaklarını kullanır.
- **[İş Merkezi Için Dynamics 365](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cpp-business-central-offer)** – Microsoft, bir iş merkezi kurumsal kaynak planlama sistemi (Finans, işlemler, tedarik ZINCIRI, CRM vb.) için test sürücüsü hizmetini (sağlama ve dağıtım dahil) barındırır ve saklar.  
- **[Müşteri katılımı Için Dynamics 365](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/dyn365ce/cpp-customer-engagement-offer)** – Microsoft, bir müşteri katılımı sistemi (Sales, Service, Project Service, alan hizmeti vb.) için test sürücüsü hizmetini (sağlama ve dağıtım dahil) barındırır ve saklar.  
- **[Operations Için Dynamics 365](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cpp-dynamics-365-operations-offer)** – Microsoft, finans ve operasyon kurumsal kaynak planlama sistemi (Finans, işlemler, üretim, tedarik zinciri vb.) için test sürücüsü hizmetini (sağlama ve dağıtım dahil) barındırır ve saklar.
- **[Mantıksal uygulama](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/logic-app-test-drive)** : tüm karmaşık çözüm mimarilerini çevreleyen bir dağıtım şablonu. Özel ürünlerin bu tür test sürücüsü kullanması gerekir.
- **[Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview)** – özel olarak oluşturulmuş bir panoya eklenmiş bir bağlantı. Etkileşimli bir Power BI görseli göstermek isteyen ürünlerin bu tür test sürücüsünü kullanması gerekir. Buraya yüklemeniz gereken, katıştırılmış Power BI URL 'sidir.

#### <a name="additional-test-drive-resources"></a>Ek test sürücüsü kaynakları

- [En iyi pazarlama deneyimleri](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [En iyi teknik uygulamalar](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [Genel bakış](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf) (PDF; açılır pencere engelleyicinizin kapalı olduğundan emin olun)

## <a name="connect-lead-management"></a>Müşteri adayı yönetimini bağlama

[!INCLUDE [Connect lead management](./includes/connect-lead-management.md)]

Daha fazla bilgi için bkz. [müşteri adayı yönetimine genel bakış](./commercial-marketplace-get-customer-leads.md).

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="properties"></a>Özellikler

Bu sayfa, teklifinizi Market 'te, uygulama sürümünüzde ve teklifinizi destekleyen yasal sözleşmelerde gruplandırmak için kullanılan kategorileri ve endüstrileri tanımlamanızı sağlar.

### <a name="category"></a>Kategori

En az bir ve en fazla üç kategori seçin. Bu kategoriler, teklifinizi uygun Market arama alanına yerleştirmek için kullanılacaktır. Teklifinizin teklif açıklamasında bu kategorileri nasıl desteklediğine dikkat edin.

### <a name="industry"></a>Endüstri

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

### <a name="name"></a>Name

Buraya girdiğiniz ad, müşteriler tarafından teklif listelerinizin başlığı olarak gösterilir. Bu alan, teklifi oluştururken **teklif diğer adı** için girdiğiniz metin ile önceden doldurulur, ancak bu değeri değiştirebilirsiniz. Bu ad trademarked olabilir (ve ticari marka veya telif hakkı sembolleri dahil edebilirsiniz). Ad 50 karakterden uzun olamaz ve herhangi bir emojıs içeremez.

### <a name="short-description"></a>Kısa açıklama

Teklifinizin kısa bir açıklamasını sağlayın (en fazla 100 karakter). Bu açıklama Market arama sonuçlarında kullanılabilir.

### <a name="description"></a>Açıklama

Teklifiniz için daha uzun bir açıklama sağlayın (en fazla 3.000 karakter). Bu açıklama Market dökümüne Genel Bakış bölümündeki müşterilere görüntülenecektir. Teklifinizin değer teklifini, anahtar avantajlarını, kategori ve/veya sektör ilişkilendirmelerini, uygulama içi satın alma fırsatlarını ve gerekli tüm teklifleri dahil edin.

Tanımlarınızı yazmak için bazı ipuçları:

- Tanımınızın ilk birkaç cümlede teklifinizin değer teklifini açık bir şekilde açıklamalıdır. Değer teklifinde aşağıdaki bilgileri ekleyin:
  - Ürünün açıklaması
  - Üründen faydalanan Kullanıcı türü
  - Müşteri ihtiyaçları veya ürün adresleriyle ilgili bir sorun
- İlk birkaç tümcenin arama motoru sonuçlarında görüntülenebileceğini aklınızda bulundurun.  
- Ürününüzü satmaya yönelik özelliklere ve işlevlere güvenmeyin. Bunun yerine, teslim ettiğiniz değere odaklanın.  
- Sektöre özgü sözlük veya avantaj temelli bir ifade mümkün olduğunca yararlanın.

Teklif açıklamanızı daha ilgi çekici hale getirmek için, biçimlendirme uygulamak için zengin metin düzenleyicisini kullanın.

![Zengin metin düzenleyicisini kullanma](./media/rich-text-editor.png)

| <center>Metin biçimini değiştir | <center>Madde işaretleri veya numaralandırma ekleme | <center>Metin girintisi ekleme veya kaldırma |
| --- | --- | --- |
| <center>![Metin biçimini değiştirmek için zengin metin düzenleyicisini kullanma](./media/text-editor3.png) |  <center>![Liste eklemek için zengin metin düzenleyicisini kullanma](./media/text-editor4.png) |  <center>![Girintilemek için zengin metin düzenleyicisini kullanma](./media/text-editor5.png) |

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
>Dosya yükleme sorununuz varsa, yerel ağınızın Iş Ortağı Merkezi tarafından kullanılan https://upload.xboxlive.com hizmeti engellemediğinden emin olun.

#### <a name="store-logos"></a>Mağaza logoları

Teklifinizin logosunu iki piksel boyutunda sağlar:

- **Küçük** (48 x 48)
- **Büyük** (216 x 216)

#### <a name="hero"></a>Hero

Hero görüntüsü isteğe bağlıdır. Bir tane sağlarsanız, 815 x 290 pikseli ölçmelidir.

#### <a name="screenshots"></a>Ekran görüntüleri

Teklifinizin nasıl çalıştığını gösteren ekran görüntüleri ekleyin. En az bir ekran görüntüsü gereklidir ve en fazla beş tane ekleyebilirsiniz. Tüm ekran görüntüleri 1280 x 720 piksel olmalıdır.

#### <a name="videos"></a>Videolar

İsteğe bağlı olarak, teklifinizi gösteren dört adede kadar video ekleyebilirsiniz. Bu videoların YouTube ve/veya Vimeo 'da barındırılması gerekir. Her biri için videonun adını, URL 'sini ve videonun küçük bir görüntüsünü girin (1280 x 720 piksel)

#### <a name="additional-marketplace-listing-resources"></a>Ek Market listeleme kaynakları

[Market teklif listeleri için en iyi uygulamalar](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)

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

## <a name="test-drive-technical-configuration"></a>Sınama sürücüsü teknik yapılandırma

Bu sayfa, müşterilerin satın almadan önce teklifinizi denemesini sağlayan bir tanıtım ("test sürücüsü") ayarlamanıza olanak sağlar. [Test sürücüsü nedir?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)makalesinde daha fazla bilgi edinin.

Bir sınama sürücüsünü etkinleştirmek için [teklif kurulumu](#test-drive) sekmesinde **bir test sürücüsünü etkinleştir** onay kutusunu işaretleyin. Test sürücüsünü teklifinizden kaldırmak için bu onay kutusunu temizleyin.

Aşağıdaki test sürücüsü türleri, her biri kendi teknik yapılandırma gereksinimlerine sahip kullanılabilir.

- [Azure Resource Manager](#technical-configuration-for-azure-resource-manager-test-drive)
- [Dynamics 365](#technical-configuration-for-dynamics-365-test-drive)
- [Mantıksal uygulama](#technical-configuration-for-logic-app-test-drive)
- [Power BI](#technical-configuration-not-required-for-power-bi-test-drives) (Teknik yapılandırma gerekli değil)

### <a name="technical-configuration-for-azure-resource-manager-test-drive"></a>Azure Resource Manager test sürücüsü için teknik yapılandırma

Çözümünüzü oluşturan tüm Azure kaynaklarını içeren bir dağıtım şablonu. Bu senaryoya uyan ürünler yalnızca Azure kaynaklarını kullanır. [Azure Resource Manager test sürücüsü](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive)ayarlama hakkında daha fazla bilgi edinin.

- **Bölgeler** (gerekli) – Şu anda, test sürücünüzün kullanılabilir hale getirilbileceği 26 Azure tarafından desteklenen bölge vardır. Genellikle, en iyi performans için en yakın bölgeyi seçebilmeniz için, en fazla sayıda müşteriyi tahmin ettiğiniz bölgelerde test sürücünüzü kullanılabilir hale getirmek isteyeceksiniz. Aboneliğinizin seçtiğiniz her bölgede gereken tüm kaynakları dağıtmasına izin verildiğinden emin olmanız gerekir.

- **Örnekler** : teklifin kullanılabildiği bölge sayısıyla çarpılacak olan türü (sıcak veya soğuk) ve kullanılabilir örneklerin sayısını seçin.

    **Sık** erişimli – bu örnek türü dağıtılır ve seçilen bölge başına erişim bekleniyor. Müşteriler, bir dağıtımı beklemek yerine bir test sürücüsünün *sık* erişimli örneklerine anında erişebilir. Zorunluluğunu getirir, bu örneklerin her zaman Azure aboneliğinizde çalışıyor olması, bu nedenle daha büyük bir çalışma süresi maliyeti doğuracaktır. En az bir *sıcak* örnek olması önemle önerilir, çünkü çoğu müşteri tam dağıtımları beklemek Istemelidir ve *etkin* örnek yoksa, müşteri kullanımındaki bir bırakmaya neden olur.

    **Soğuk** – Bu örnek türü, bölge başına büyük olasılıkla dağıtılabilecek örneklerin toplam sayısını temsil eder. Soğuk örnekler, bir müşteri test sürücüsünü istediğinde tüm test sürücüsü Kaynak Yöneticisi şablonun dağıtılmasını gerektirir, böylece *soğuk* örnekler, *etkin* örneklere göre yüklenmeye çok daha yavaştır. Zorunluluğunu getirir, yalnızca test sürücüsünün süresi için ödeme yapmanız gereken *bir deyişle, Azure aboneliğinizde her zaman* bir *sıcak* örnekle birlikte çalışmıyor.

- **Test sürücüsü Azure Resource Manager şablonu** – Azure Resource Manager şablonunuzu içeren. zip ' i karşıya yükleyin.  Hızlı başlangıç makalesinde Azure Resource Manager şablonu oluşturma hakkında daha fazla bilgi edinin [Azure Portal kullanarak Azure Resource Manager şablonları oluşturun ve dağıtın](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal).

- **Sınama sürücüsü süresi** (zorunlu) – test sürücüsünün, saat sayısı içinde etkin kalacağı sürenin uzunluğunu girin. Bu süre dolduktan sonra test sürücüsü otomatik olarak sona erer. Bu süre yalnızca birkaç saat (örneğin, "2" saat) ile ayarlanabilir. "1,5" geçerli değil).

### <a name="technical-configuration-for-dynamics-365-test-drive"></a>Dynamics 365 test sürücüsü için teknik yapılandırma

Microsoft, bu test sürücüsü türünü kullanarak hizmet sağlamayı ve dağıtımını barındırarak ve tutarak bir test sürücüsü ayarlamanın karmaşıklığını ortadan kaldırabilir. Bu barındırılan test sürücüsü türünün yapılandırması, test sürücüsünün bir Iş Merkezi, müşteri katılımı veya Işlem kitlesini hedeflediğinden bağımsız olarak aynıdır.

- **Maksimum eşzamanlı test sürücüleri** (gerekli) – test sürücünüzü tek seferde kullanılabilecek maksimum müşteri sayısını ayarlayın. Her eşzamanlı kullanıcı, test sürücüsü etkinken bir Dynamics 365 lisansını kullanır, bu nedenle maksimum sınır kümesini destekleyecek yeterli lisansa sahip olduğunuzdan emin olmanız gerekir. Önerilen 3-5 değeri.

- **Sınama sürücüsü süresi** (zorunlu) – saat sayısını tanımlayarak test sürücüsünün etkin kalacağı sürenin uzunluğunu girin. Bu süre dolduktan sonra, oturum sona erdir ve artık lisanslarınızdan birini tüketmez. Teklifinizin karmaşıklığına bağlı olarak 2-24 saat arasında bir değer öneririz. Bu süre yalnızca birkaç saat (örneğin, "2" saat) ile ayarlanabilir. "1,5" geçerli değil).  Kullanıcı zaman aşımına uğrar ve test sürücüsüne yeniden erişmek istediğinizde yeni bir oturum isteyebilir.

- **Örnek URL** (gerekli) – müşterinin test sürücüsüne başlayacağı URL. Genellikle uygulamanızı örnek verilerle çalıştıran Dynamics 365 örneğinizin URL 'SI (örneğin, `https://testdrive.crm.dynamics.com`).

- **Örnek Web API URL 'si** (gerekli) – Microsoft 365 hesabınızda oturum açarak ve **Ayarlar** \&gt; üzerinde gezinerek Dynamics 365 örneğinizin Web API URL 'sini alın. **Özelleştirme** \&gt; **Geliştirici kaynakları** \&gt; **Örnek Web API 'si (hizmet kök URL 'si)**, burada bulunan URL 'yi kopyalayın (örneğin `https://testdrive.crm.dynamics.com/api/data/v9.0`,).

- **Rol adı** (gerekli) – özel Dynamics 365 test sürücünüzde tanımladığınız güvenlik rolü adını belirtin. Bu, kullanıcıya test sürücüleri sırasında atanır (örneğin, Test-Drive-Role).

### <a name="technical-configuration-for-logic-app-test-drive"></a>Mantıksal uygulama sınama sürücüsü için teknik yapılandırma

Herhangi bir özel ürün, çeşitli karmaşık çözüm mimarilerini kapsayan bu tür test sürücüsü Dağıtım şablonunu kullanmalıdır. Mantıksal uygulama test sürücüleri ayarlama hakkında daha fazla bilgi için bkz. GitHub üzerinde [işlemler](https://github.com/Microsoft/AppSource/blob/master/Setup-your-Azure-subscription-for-Dynamics365-Operations-Test-Drives.md) ve [müşteri katılımı](https://github.com/Microsoft/AppSource/wiki/Setting-up-Test-Drives-for-Dynamics-365-app) .

- **Bölge** (gerekli, tek seçim açılan listesi) – Şu anda, test sürücünüzün kullanılabilir hale getirilbileceği 26 Azure tarafından desteklenen bölge vardır. Mantıksal uygulamanızın kaynakları seçtiğiniz bölgede dağıtılır. Mantıksal uygulamanızda belirli bir bölgede depolanan özel kaynaklar varsa bölgenin burada seçildiğinden emin olun. Bölgeniz için özel kaynaklarınızın mevcut olduğundan emin olmanın en iyi yolu, mantıksal uygulamanızı portalda Azure aboneliğinizde yerel olarak dağıtmaktır ve bu seçimi yapmadan önce doğru çalıştığını doğrular.

- **Maksimum eşzamanlı test sürücüleri** (gerekli) – test sürücünüzü tek seferde kullanılabilecek maksimum müşteri sayısını ayarlayın. Bu test sürücüleri zaten dağıtılır ve müşterilerin bir dağıtımı beklemeden bunlara anında erişmelerini sağlar.

- **Sınama sürücüsü süresi** (zorunlu) – test sürücüsünün, saat sayısı içinde etkin kalacağı sürenin uzunluğunu girin. Bu süre dolduktan sonra test sürücüsü otomatik olarak sona erer.

- **Azure Kaynak grubu adı** (gerekli) – mantıksal uygulama test sürücünüzün kaydedildiği [Azure Kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) adını girin.

- **Azure mantıksal uygulama adı** (gerekli) – test sürücüsünü kullanıcıya atayan mantıksal uygulamanın adını girin. Bu mantıksal uygulama, yukarıdaki Azure kaynakları grubuna kaydedilmelidir.

- **Mantıksal uygulama adı sağlamasını kaldırma** (gerekli) – müşteri bittikten sonra test sürücüsünü sağlayan mantıksal uygulamanın adını girin. Bu mantıksal uygulama, yukarıdaki Azure kaynakları grubuna kaydedilmelidir.

### <a name="technical-configuration-not-required-for-power-bi-test-drives"></a>Power BI test sürücüleri için teknik yapılandırma gerekli değildir

Etkileşimli bir Power BI görseli göstermek isteyen ürünler, özel olarak oluşturulmuş bir panoyu test sürücüleri olarak paylaşmak için, daha fazla teknik yapılandırma gerekmeden gömülü bir bağlantı kullanabilir. [Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview) şablonu uygulamalarını ayarlama hakkında daha fazla bilgi edinin.

### <a name="deployment-subscription-details"></a>Dağıtım aboneliği ayrıntıları

Test sürücüsünü sizin adınıza dağıtmak için, ayrı ve benzersiz bir Azure aboneliği oluşturun ve sağlayın. (Power BI test sürücüleri için gerekli değildir).

- **Azure ABONELIK kimliği** (Azure Resource Manager ve Logic Apps için gereklidir) – Azure hesap hizmetlerinizi kaynak kullanımı raporlama ve faturalama için erişim izni vermek üzere abonelik kimliğini girin. Henüz bir tane yoksa, test sürücüleri için kullanmak üzere [ayrı bir Azure aboneliği oluşturmayı](https://docs.microsoft.com/azure/billing/billing-create-subscription) düşünmeniz önerilir. [Azure Portal](https://portal.azure.com/) oturum açarak ve sol taraftaki menüdeki **abonelikler** SEKMESINE giderek Azure abonelik kimliğinizi bulabilirsiniz. Sekmeyi seçtiğinizde, abonelik KIMLIĞINIZ görüntülenir (örneğin, "a83645ac-1234-5AB6-345-1h234g764ghty").

- **Azure AD KIRACı kimliği** (gerekli) – Azure ACTIVE DIRECTORY (ad) [kiracı kimliğinizi](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)girin. Bu KIMLIĞI bulmak için [Azure Portal](https://portal.azure.com/)oturum açın, sol taraftaki menüden Active Directory sekmesini seçin, **Özellikler**' i seçin, sonra listelenen **dizin kimliği** numarasını (örneğin, 50c464d3-4930-494c-963c-1e951d15360e) arayın. Ayrıca, etki alanı adı adresinizi kullanarak kuruluşunuzun kiracı KIMLIĞINI de arayabilirsiniz [https://www.whatismytenantid.com](https://www.whatismytenantid.com).

- **Azure AD kiracı adı** (dinamik 365 için gereklidir) – Azure ACTIVE DIRECTORY (ad) adınızı girin. Bu adı bulmak için sağ üst köşedeki [Azure Portal](https://portal.azure.com/)oturum açın, kiracı adınız hesap adınızın altında listelenecektir.

- **Azure AD uygulama kimliği** (gerekli) – Azure ACTIVE DIRECTORY (ad) [uygulama kimliğinizi](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)girin. Bu KIMLIĞI bulmak için [Azure Portal](https://portal.azure.com/)oturum açın, sol menüden Active Directory sekmesini seçin, **uygulama kayıtları**' yı seçin, ardından listelenen **uygulama kimliği** numarasını arayın (örneğin, 50c464d3-4930-494c-963c-1e951d15360e).

- **Azure AD uygulama istemci parolası** (gerekli) – Azure AD uygulama [Istemci gizli](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#certificates-and-secrets)anahtarını girin. Bu değeri bulmak için [Azure Portal](https://portal.azure.com/)oturum açın. Sol menüdeki **Azure Active Directory** sekmesini seçin, **uygulama kayıtları**' i seçin ve ardından test sürücüsü uygulamanızı seçin. Sonra **Sertifikalar ve gizlilikler**' ı seçin, **yeni istemci parolası**' nı seçin, bir açıklama girin, **süresi dolmasın**' ı seçin ve ardından **Ekle**' yi **Never** Değeri kopyalamadığınızdan emin olun. (Değeri bir yere göz önünde yapmadan önce sayfadan uzaklaşmayın veya değere erişemezsiniz.)

Devam etmeden önce **Taslağı kaydet** ' i seçin.

### <a name="test-drive-marketplace-listings"></a>Sınama sürücüsü Market listeleri

**Test sürücüsü** sekmesinde bulunan **Market listeleme** seçeneği, test sürücünüzün kullanılabildiği dilleri görüntüler. Şu anda **İngilizce (Birleşik Devletler)** kullanılabilir tek konumdur. Sınama sürücüsü deneyimini açıklayan bilgileri girmek için dil adını seçin.

- **Açıklama** (gerekli) – test sürücünüzü, neyi gösterdiklerinizi, kullanıcının deneme amaçlarını, araştırılacak özellikleri ve kullanıcının teklifinizi elde edip etmeyeceğinizi belirlemesine yardımcı olmak için Ilgili bilgileri tanıtın. Bu alana en fazla 3.000 karakter metin girilebilir.

- **Erişim bilgileri** (Azure Resource Manager ve mantıksal test sürücüleri için gereklidir) – Bu test sürücüsüne erişmek ve bu sınama sürücüsüne erişmek için müşterinin neleri bilmesi gerektiğini açıklayın. Teklifinizi kullanmaya yönelik bir senaryoya ve müşterinin test sürüşü genelinde özelliklere erişmek için bilmeleri gereken tam olarak ne olduğunu öğrenin. Bu alana en fazla 10.000 karakter metin girilebilir.

- **Kullanıcı el ile** (gerekli) – test sürücünüzün deneyiminizdeki derinlemesine bir adım adım. Kullanıcı el kitabı, müşterinin test sürücüsüyle karşılaşmasını istediğiniz şekilde tam olarak kapsamalıdır ve sahip olabileceği sorulara başvuru olarak görev yapar. Dosya, karşıya yüklendikten sonra PDF biçiminde ve adlandırılmalıdır (en fazla 255 karakter) olmalıdır.

- **Videolar** (isteğe bağlı) – videolar, YouTube veya Vimeo 'a yüklenebilir ve burada bir bağlantı ve küçük resim görüntüsü (533 x 324 piksel) ile birlikte başvuruda bulunabilir. böylece, bir müşterinin test sürücüsünü daha iyi anlamasına yardımcı olmak için bir bağlantı ve küçük resim görüntüsü (x piksel), bu sayede teklifinizin özelliklerinin başarıyla nasıl kullanılacağı ve avantajlarının vurgulanmasını sağlayan senaryolar hakkında daha fazla bilgi edinebilirsiniz.
  - **Ad** (gerekli)
  - **URL** (gerekli; Yalnızca YouTube veya Vimeo)
  - **Küçük resim** (görüntü, PNG biçiminde ve 533 x 324 piksel olmalıdır)

Devam etmeden önce **Taslağı kaydet** ' i seçin.

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
- **Gönder**' i seçerek teklifi yayımlamaya gönderebilirsiniz. Size, gözden geçirmeniz ve onaylamanız için teklifin bir önizleme sürümünün ne zaman kullanılabileceğini bilmenizi sağlayacak bir e-posta göndereceğiz. Iş Ortağı Merkezi 'ne dönün ve teklifinizi herkese açık bir şekilde (veya özel bir teklif olarak özel hedef kitleye) yayımlama teklifi için **Go-Live** ' ı seçin.

## <a name="next-step"></a>Sonraki adım

- [Ticari Market'te bulunan bir teklifi güncelleştirme](./update-existing-offer.md)
