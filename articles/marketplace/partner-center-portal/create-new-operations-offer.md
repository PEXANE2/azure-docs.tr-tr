---
title: Ticari Pazar Da operasyonlar için yeni bir Dynamics 365 oluşturma teklifi
description: Microsoft İş Ortağı Merkezi'ndeki Ticari Pazar Portalı portalını kullanarak Azure Marketi, AppSource veya Bulut Çözüm Sağlayıcısı (CSP) programı aracılığıyla listelemek veya satmak için Operasyonlar için yeni bir Dynamics 365 teklifi oluşturma.
author: dsindona
ms.author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: fbd8d9bdfb356ff9816056b85b1dc5688d17b58c
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81869763"
---
# <a name="create-a-new-dynamics-365-for-operations-offer"></a>Operasyonlar için yeni bir Dynamics 365 oluşturma teklifi

Bu konu, Operasyonlar için yeni bir Dynamics 365 teklifinin nasıl oluşturulabildiğini açıklar. [Microsoft Dynamics 365 for Finance and Operations,](https://dynamics.microsoft.com/finance-and-operations) gelişmiş finans, operasyon, üretim ve tedarik zinciri yönetimini destekleyen bir kurumsal kaynak planlama (ERP) hizmetidir. Dynamics 365 for Operations için tüm teklifler belgelendirme sürecimizden geçmelidir.

Operasyonlar için Dynamics 365'i oluşturmaya başlamak için, ilk önce [Bir İş Ortağı Merkezi hesabı oluşturduğunuzdan](./create-account.md) ve seçilen Genel **Bakış** sayfasıyla Ticari [Pazar panosunu](https://partner.microsoft.com/dashboard/commercial-marketplace/offers)açtığınızı sağlayın.

![İş Ortağı Merkezi'nde Ticari Pazar panosu](./media/new-offer-overview.png)

>[!Note]
> Bir teklif yayımlandıktan sonra, Ortak Merkezi'nde yapılan teklife yönelik yapılan güncellemeler yalnızca yeniden yayımlandıktan sonra sistemde ve mağaza önlerinde güncellenir. Değişiklik yaptıktan sonra teklifi yayınlanmak üzere gönderdiğinizi belirtin.


## <a name="create-a-new-offer"></a>Yeni teklif oluşturma

+ **Yeni teklif** düğmesini seçin ve ardından **İşlemler için Dynamics 365** menü öğesini seçin. **Yeni teklif** iletişim kutusu görüntülenir.

### <a name="offer-id-and-alias"></a>Teklif kimliği ve takma ad

- **Teklif Kimliği**: Hesabınızdaki her teklif için benzersiz tanımlayıcı. Bu kimlik, pazar teklifi ve Azure Kaynak Yöneticisi şablonlarının (varsa) URL adresindeki müşteriler tarafından görülebilir. Teklif kimliği küçük alfasayısal karakterler olmalıdır (tireler ve alt çizerler dahil, ancak beyaz boşluk yoktur). Bu kimlik 50 karakterle sınırlıdır ve **Oluştur'u**seçtikten sonra değiştirilemez.  Örneğin, *burada test-teklif-1* girerseniz, teklif URL'si `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.

- **Teklif diğer adı**: Ortak Merkezi'ndeki teklife atıfta bulunmak için kullanılan ad. Bu ad pazarda kullanılmaz ve teklif adı ve müşterilere gösterilecek diğer değerlerden farklıdır. **Oluştur'u**seçtikten sonra bu değer değiştirilemez.

**Teklif Kimliği** nizi ve **Teklif takma**adını girdikten sonra **Oluştur'u**seçin. Daha sonra teklifinizin tüm farklı kısımları üzerinde çalışabileceksiniz.

## <a name="offer-setup"></a>Teklif kurulumu

**Teklif kurulum** sayfası aşağıdaki bilgileri ister. Bu alanları tamamladıktan sonra **Kaydet'i** seçtiğinizden emin olun.

### <a name="how-do-you-want-potential-customers-to-interact-with-this-listing-offer"></a>Potansiyel müşterilerin bu giriş teklifiyle nasıl etkileşimde bulunmasını istiyorsunuz?

Bu teklif için kullanmak istediğiniz seçeneği seçin.

#### <a name="get-it-now-free"></a>Şimdi alın (ücretsiz)

Uygulamanıza erişebilecekleri geçerli bir URL *(http* veya *https*ile başlayan) sağlayarak müşterilerinize teklifinizi ücretsiz olarak listeleyebilirsiniz.  Örneğin, `https://contoso.com/my-app`

#### <a name="free-trial-listing"></a>Ücretsiz deneme (listeleme)

Teklifinizi, deneme alabilecekleri geçerli bir URL *(http* veya *https*ile başlayan) sağlayarak ücretsiz deneme sürümü bağlantısıyla müşterilere listele.  Örneğin: `https://contoso.com/trial/my-app`. Teklif listesi ücretsiz denemeler oluşturulur, yönetilir ve hizmetiniz tarafından yapılandırılır ve Microsoft tarafından yönetilen abonelikleri yoktur.

> [!NOTE]
> Uygulamanızın deneme bağlantınız aracılığıyla alacağı belirteçler, yalnızca uygulamanızdaki hesap oluşturmayı otomatikleştirmek için Azure Active Directory (Azure AD) aracılığıyla kullanıcı bilgilerini elde etmek için kullanılabilir. Microsoft hesapları bu belirteç kullanılarak kimlik doğrulaması için desteklenmez.

#### <a name="contact-me"></a>Bana ulaşın

Müşteri İlişkileri Yönetimi (CRM) sisteminizi bağlayarak müşteri iletişim bilgilerini toplayın. Müşteriden bilgilerini paylaşmak için izin istenecektir. Bu müşteri bilgileri, teklif inizi buldukları teklif adı, kimlik ve pazar yeri kaynağıyla birlikte, yapılandırdığınız CRM sistemine gönderilir. CRM'nizi yapılandırma hakkında daha fazla bilgi için [müşteri adayı yönetimini bağla'ya](#connect-lead-management)bakın. 

### <a name="test-drive"></a>Test sürüşü

Test sürüşü, potansiyel müşterilere teklifinizi 'satın almadan önce deneme' seçeneği sunarak, dönüşümün artması ve yüksek nitelikli müşteri adaylarının üretilmesiyle sonuçlanan harika bir yoldur. [Test sürücüleri hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive)

Test sürüşü etkinleştirmek **için, test sürüşü etkinleştir** kutusunu işaretleyin. Daha sonra, müşterilerin teklifinizi belirli bir süre boyunca denemesine izin verecek şekilde Test [sürücüsü teknik yapılandırmasında](#test-drive-technical-configuration) bir gösteri ortamı yapılandırmanız gerekir.

#### <a name="type-of-test-drive"></a>Test sürüşü türü

Aşağıdaki seçeneklerarasından seçim:

- **[Azure Kaynak Yöneticisi](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive)**: Çözümünüzü oluşturan tüm Azure kaynaklarını içeren dağıtım şablonu. Bu senaryoya uyan ürünler yalnızca Azure kaynaklarını kullanır.
- **[Business Central için Dynamics 365](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cpp-business-central-offer)**: Microsoft, Business Central kurumsal kaynak planlama sistemi (finans, operasyonlar, tedarik zinciri, CRM, vb.) için test sürüşü hizmetini (sağlama ve dağıtım dahil) barındırır ve korur.  
- **[Müşteri Etkileşimi için Dynamics 365](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/dyn365ce/cpp-customer-engagement-offer)**: Microsoft, Müşteri Etkileşimi sistemi (satış, servis, proje hizmeti, saha hizmeti, vb.) için test sürüşü hizmetini (sağlama ve dağıtım dahil) barındırır ve tutar.  
- **[Operasyonlar için Dinamikler 365](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cpp-dynamics-365-operations-offer)**: Microsoft, Finans ve Operasyonlar kurumsal kaynak planlama sistemi (finans, operasyonlar, üretim, tedarik zinciri, vb.) için test sürüşü hizmetini (sağlama ve dağıtım dahil) barındırır ve korur. 
- **[Mantık uygulaması](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/logic-app-test-drive)**: Tüm karmaşık çözüm mimarilerini kapsayan dağıtım şablonu. Herhangi bir özel ürün bu test sürüşü türünü kullanmalıdır.
- **[Güç BI](https://docs.microsoft.com/power-bi/service-template-apps-overview)**: Özel olarak oluşturulmuş bir panoya gömülü bağlantı. Etkileşimli bir Power BI görseli göstermek isteyen ürünler bu test sürüşü türünü kullanmalıdır. Burada yüklemek için gereken tek şey gömülü Güç BI URL'dir.

#### <a name="additional-test-drive-resources"></a>Ek test sürüşü kaynakları

- [Test Sürüşü Teknik En İyi Uygulamalar](https://github.com/Azure/AzureTestDrive/wiki/Test-Drive-Best-Practices)
- [Test Drive Pazarlama En İyi Uygulamalar](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/marketing-and-best-practices)
- [Test Sürüşüne Genel Bakış Bir Çağrı Cihazı](https://assetsprod.microsoft.com/mpn/azure-marketplace-appsource-test-drives.pdf)

## <a name="connect-lead-management"></a>Müşteri adayı yönetimini bağlayın

[!INCLUDE [Connect lead management](./includes/connect-lead-management.md)]

Daha fazla bilgi için Müşteri [Adayı yönetimine genel bakış](./commercial-marketplace-get-customer-leads.md)adabına bakın.

Bir sonraki bölüme geçmeden önce **Kaydetmeyi** unutmayın.

## <a name="properties"></a>Özellikler

**Özellikler** sayfası, teklifinizi pazarda, uygulama sürümünüzde ve teklifinizi destekleyen yasal sözleşmelerde gruplandırmak için kullanılan kategorileri ve sektörleri tanımlamanıza olanak tanır. Bu sayfayı tamamladıktan sonra **Kaydet'i** seçin.

### <a name="category"></a>Kategori

En az bir ve en fazla üç kategori seçin. Bu kategoriler, teklifinizi uygun pazar yeri arama alanlarına yerleştirmek için kullanılacaktır. Teklifinizin teklif açıklamasında bu kategorileri nasıl desteklediğini mutlaka unutmayın.

### <a name="industry"></a>Endüstri

[!INCLUDE [Industry Taxonomy](./includes/industry-taxonomy.md)]

### <a name="app-version"></a>Uygulama sürümü

Teklifinizin sürüm numarasını girin. Müşteriler bu sürümü teklifin ayrıntı sayfasında görür.

### <a name="terms-and-conditions"></a>hüküm ve koşullar

**Şartlar ve koşullar** alanında kendi yasal hüküm ve koşullarınızı sağlayın. Ayrıca, şart ve koşullarınızın bulunabileceği URL'yi de sağlayabilirsiniz. Müşterilerin teklifinizi denemeden önce bu koşulları kabul etmeleri gerekmektedir.

## <a name="offer-listing"></a>Teklif listesi

Teklif listesi sayfası, teklifinizin listeleneceği dilleri görüntüler. Şu anda, **İngilizce (AMERIKA Birleşik Devletleri)** tek seçenektir.

Her dil/pazar için pazar ayrıntıları (teklif adı, açıklama, resim, vb.) tanımlamanız gerekir. Bu bilgileri sağlamak için dil/pazar adını seçin.

> [!NOTE]
> Teklif listesi içeriği (açıklama, belge, ekran görüntüsü, kullanım koşulları, vb.) "Bu uygulama yalnızca [İngilizce olmayan dilde] ifadesiyle başladığı sürece İngilizce olması gerekmez." Teklif listesi içeriğinde kullanılandan başka bir dilde içerik sunmak için yararlı bir *bağlantı URL'si* sağlamak da kabul edilebilir.

### <a name="name"></a>Adı

Buraya girdiğiniz ad, teklif listenizin başlığı olarak müşterilere gösterilir. Bu alan, teklifi oluşturduğunuzda **Teklif takma adı** için girdiğiniz metinle önceden doldurulmuştur, ancak bu değeri değiştirebilirsiniz. Bu ad ticari marka olabilir (ve ticari marka veya telif hakkı sembolleri içerebilir). Ad 50 karakterden fazla olamaz ve herhangi bir emoji içeremez.

### <a name="short-description"></a>Kısa açıklama

Teklifinizin kısa bir açıklamasını sağlayın (en fazla 100 karakter). Bu açıklama, pazar yeri arama sonuçlarında kullanılabilir.

### <a name="description"></a>Açıklama

Teklifinizin daha uzun bir açıklamasını sağlayın (en fazla 3.000 karakter). Bu açıklama, pazar yeri giriş özetinde müşterilere görüntülenir. Teklifinizin değer teklifini, temel avantajlarını, kategorive/veya endüstri derneklerini, uygulama içi satın alma fırsatlarını ve gerekli açıklamaları ekleyin.

Açıklamanızı yazmak için bazı ipuçları:  

- Teklifinizin değer teklifini açıklamanızın ilk birkaç cümlesinde açıkça açıklayın. Değer teklifinize aşağıdaki bilgileri ekleyin:
  - Ürünün tanımı
  - Üründen yararlanan kullanıcı türü
  - Ürünün hitap ettiği müşteri ihtiyaçları veya acısı
- İlk birkaç cümlenin arama motoru sonuçlarında görüntülenebileceğini unutmayın.  
- Ürününüzü satmak için özelliklere ve işlevselliğe güvenmeyin. Bunun yerine, sağladığınız değere odaklanın.  
- Sektöre özgü kelime dağarcığı veya fayda tabanlı ifadeler mümkün olduğunca kullanın.
- Açıklamanızı biçimlendirmek ve daha ilgi çekici hale getirmek için HTML etiketlerini kullanmayı düşünün.

Teklif açıklamanızı daha ilgi çekici hale getirmek için, açıklamanızı biçimlendirmek için zengin metin düzenleyicisini kullanın.

![Zengin metin düzenleyicisini kullanma](./media/text-editor2.png)

Zengin metin düzenleyicisini kullanmak için aşağıdaki yönergeleri kullanın:

- İçeriğinizin biçimini değiştirmek için, biçimlendirmek istediğiniz metni vurgulayın ve aşağıda gösterildiği gibi bir metin stili seçin:

     ![Metin biçimini değiştirmek için zengin metin düzenleyicisini kullanma](./media/text-editor3.png)

- Metne madde işaretli veya numaralanmış bir liste eklemek için aşağıdaki seçenekleri kullanın:

     ![Liste eklemek için zengin metin düzenleyicisini kullanma](./media/text-editor4.png)

- Metne girintiyi eklemek veya kaldırmak için aşağıdaki seçenekleri kullanın:

     ![Girinti için zengin metin düzenleyicisini kullanma](./media/text-editor5.png)

### <a name="search-keywords"></a>Anahtar kelimeleri ara

Müşterilerin teklifinizi pazarda bulmasına yardımcı olmak için isteğe bağlı olarak en fazla üç arama anahtar kelimesini girebilirsiniz. En iyi sonuçlar için, bu anahtar kelimeleri açıklamanızda da kullanmayı deneyin.

### <a name="products-your-app-works-with"></a>Uygulamanızın birlikte çalıştığı ürünler

Müşterilerinize uygulamanızın belirli ürünlerle çalıştığını bildirmek istiyorsanız, buraya en fazla üç ürün adı girin.

### <a name="support-urls"></a>Destek URL'leri

Bu bölüm, müşterilerin teklifiniz hakkında daha fazla bilgi edinmelerine yardımcı olacak bağlantılar sağlamanıza olanak tanır.

#### <a name="help-link"></a>Yardım bağlantısı

Müşterilerin teklifiniz hakkında daha fazla bilgi edinebileceği URL'yi girin.

#### <a name="privacy-policy-url"></a>Gizlilik ilkesi URL'si

Kuruluşunuzun gizlilik politikasına URL girin. Uygulamanızın gizlilik yasalarına ve yönetmeliklerine uygun olmasını sağlamaktan ve geçerli bir gizlilik politikası sağlamaktan siz sorumlusunuz.

### <a name="contacts"></a>Kişiler

Bu bölümde, bir Destek kişi ve **mühendislik** **kişi** için ad, e-posta ve telefon numarası sağlayın. Bu bilgiler müşterilere gösterilmez, ancak Microsoft tarafından kullanılabilir ve CSP iş ortaklarına sağlanabilir.

Destek **iletişim** bölümünde, CSP iş ortaklarının teklifiniz için destek bulabilecekleri **Destek URL'sini** sağlayın.

### <a name="supporting-documents"></a>Destekleyici belgeler

Burada teknik incelemeler, broşürler, denetim listeleri veya sunular gibi en az bir (ve en fazla üç) ilgili pazarlama belgesi sağlayın. Bu belgeler .pdf formatında olmalıdır.

### <a name="marketplace-images"></a>Pazar yeri görüntüleri

Bu bölümde, teklifinizi müşteriye gösterirken kullanılacak logolar ve resimler sağlayabilirsiniz. Tüm görüntüler .png formatında olmalıdır.

>[!Note]
>Dosya yükleme sorunu yaşıyorsanız, yerel ağınızın İş Ortağı https://upload.xboxlive.com Merkezi tarafından kullanılan hizmeti engellemediğinden emin olun.

#### <a name="store-logos"></a>Mağaza logoları

Teklifinizin logosunu iki boyutta sağlayın: **Küçük (48 x 48)** ve **Büyük (216 x 216)**.

#### <a name="hero"></a>Kahraman

Kahraman görüntüsü isteğe bağlıdır. Bir tane sağlarsanız, 815 x 290 piksel ölçmeniz gerekir.

#### <a name="screenshots"></a>Ekran görüntüleri

Teklifinizin nasıl çalıştığını gösteren ekran görüntüleri ekleyin. En az bir ekran görüntüsü gereklidir ve en fazla beş ekran ekleyebilirsiniz. Tüm ekran görüntüleri 1280 x 720 piksel olmalıdır.

#### <a name="videos"></a>Videolar

İsteğe bağlı olarak, teklifinizi gösteren en fazla dört video ekleyebilirsiniz. Bu videolar YouTube ve/veya Vimeo'da barındırılmalıdır. Her biri için videonun adını, URL'sini ve videonun küçük resmini (1280 x 720 piksel) girin

#### <a name="additional-marketplace-listing-resources"></a>Ek pazar yeri listeleme kaynakları

- [Pazar teklifi listeleri için en iyi uygulamalar](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)

## <a name="availability"></a>Kullanılabilirlik

**Kullanılabilirlik** sayfası, teklifinizi nerede ve nasıl sunacağınız hakkında seçenekler sunar.

### <a name="markets"></a>Piyasa

Bu bölümde, teklifinizin sunulması gereken pazarları belirtmenize olanak tanır. Bunu yapmak için, **Market seçimi** açılır penceresini görüntüleyen **Pazarları Edit'i** seçin.

Varsayılan olarak, hiçbir pazar seçili değildir. Teklifinizi yayınlamak için en az bir pazar seçin. Teklifinizi olası her pazarda kullanılabilir hale getirmek için **Tümünü Seç'i** tıklatın veya eklemek istediğiniz belirli pazarları seçin. Bitirdikten sonra **Kaydet'i**seçin.

Buradaki seçimleriniz yalnızca yeni satın almalar için geçerlidir; birisi uygulamanızı belirli bir pazarda zaten kullanıyorsa ve daha sonra bu pazarı kaldırırsanız, bu pazarda zaten teklif veren kişiler uygulamayı kullanmaya devam edebilir, ancak bu pazardaki hiçbir yeni müşteri teklifinizi alabilir.

> [!IMPORTANT]
> Bu gereksinimler burada veya İş Merkezi'nde listelenmemiş olsa bile, yerel yasal gereklilikleri yerine getirmek sizin sorumluluğunuzdadır.

Tüm pazarları seçseniz bile, yerel yasalar ve kısıtlamalar veya diğer etkenler bazı ülkelerde ve bölgelerde belirli tekliflerin listelenmesini engelleyebilir.

### <a name="preview-audience"></a>Hedef kitleyi önizleme

Teklifinizi daha geniş bir pazar yeri teklifine canlı olarak yayınlamadan önce, önce teklifi sınırlı bir **Önizleme kitlesi**tarafından kullanılabilir hale getirmeniz gerekir. Buraya bir **Gizle tuşu** (yalnızca küçük harfleri ve/veya sayıları kullanan herhangi bir dize) girin. Önizleme hedef kitlenizin üyeleri, teklifinizin pazardaki önizlemesini görüntülemek için bu gizleme anahtarını belirteç olarak kullanabilir.

Ardından, teklifinizi kullanıma sunmaya ve önizleme kısıtlamasını kaldırmaya hazır olduğunuzda, **Gizle anahtarını** kaldırmanız ve yeniden yayımlamanız gerekir.

## <a name="technical-configuration"></a>Teknik yapılandırma

**Teknik yapılandırma** sayfası, teklifinize bağlanmak için kullanılan teknik ayrıntıları tanımlar. Bu bağlantı, teklifinizi satın almayı seçerlerse son müşteriye sunmamızı sağlar.

### <a name="solution-identifier"></a>Çözüm tanımlayıcısı

Çözümünüz için çözüm tanımlayıcısını (GUID) sağlayın.

Çözüm tanımlayıcınızı bulmak için:
1. Microsoft Dynamics Yaşam Döngüsü Hizmetleri'nde (LCS) **Çözüm Yönetimi'ni**seçin.
2. Çözümünüzü seçin ve **pakete genel bakışta** **Çözüm Tanımlayıcısını** arayın. Tanımlayıcı boşsa, **Edit'i** seçin ve paketinizi yeniden yayımlayın, ardından yeniden deneyin.

### <a name="release-version"></a>Sürüm sürümü

Bu çözümün birlikte çalıştığı Finans ve Operasyonlar için Dynamics 365 sürümünü seçin.

## <a name="test-drive-technical-configuration"></a>Test sürüşü teknik yapılandırması

[Teklif kurulum](#offer-setup) sayfasında bir **test sürüşü etkinleştir'i** seçtiyseniz, müşterilerin teklifinizin test sürüşlerini deneyimlemesini sağlamak için burada ayrıntılı bilgi vermeniz gerekir.

**Test sürüşü** sayfası, müşterilerin teklifinizi satın almayı taahhüt etmeden önce denemelerini sağlayacak bir gösteri (veya "test sürüşü") ayarlamanızı sağlar. Makalede daha fazla bilgi Test [Drive nedir?](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/what-is-test-drive) Teklifiniz için artık bir test sürüşü sağlamak istemiyorsanız, **[Teklif kurulum](#offer-setup)** sayfasına dönün ve **Test sürücüsüne etkin**bir şekilde etkinleştir'in denetimini kaldırın.

Aşağıdaki test sürücüleri, her biri kendi teknik yapılandırma gereksinimlerine sahip kullanılabilir.

- [Azure Resource Manager](#technical-configuration-for-azure-resource-manager-test-drive)
- [Dynamics 365](#technical-configuration-for-dynamics-365-test-drive)
- [Mantık uygulaması](#technical-configuration-for-logic-app-test-drive)
- [Güç BI](#technical-configuration-not-required-for-power-bi-test-drives) (Teknik yapılandırma gerekli değildir)

### <a name="technical-configuration-for-azure-resource-manager-test-drive"></a>Azure Kaynak Yöneticisi test sürüşü için teknik yapılandırma

Çözümünüzü oluşturan tüm Azure kaynaklarını içeren bir dağıtım şablonu. Bu senaryoya uyan ürünler yalnızca Azure kaynaklarını kullanır. Azure Kaynak Yöneticisi [test sürücüsü](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/azure-resource-manager-test-drive)ayarlama hakkında daha fazla bilgi edinin.

- **Bölgeler** (gerekli): Şu anda test sürüşünüzün kullanılabildiği 26 Azure destekli bölge bulunmaktadır. Genellikle, test sürüşünüzü en iyi performans için en yakın bölgeyi seçebilmeleri için en fazla müşteri sayısını tahmin ettiğiniz bölgelerde kullanılabilir hale getirmek istersiniz. Aboneliğinizin seçtiğiniz bölgelerin her birinde gereken tüm kaynakları dağıtmasına izin verildiğinden emin olmanız gerekir.

- **Örnekler**: Teklifinizin mevcut olduğu bölge sayısıyla çarpılarak bulunacak tür (sıcak veya soğuk) ve kullanılabilir örnek sayısını seçin.

**Hot**: Bu tür bir örnek dağıtılır ve seçili bölge başına erişim beklenilir. Müşteriler dağıtım beklemek yerine test sürücüsünün *Sıcak* örneklerine anında erişebilir. Bunun amacı, bu örneklerin Azure aboneliğinizde her zaman çalışıyor olmasıdır, bu nedenle daha büyük bir çalışma süresi maliyetine neden olurlar. Çoğu müşteri tam dağıtımları beklemek istemediğinden, en az bir *Sıcak* örneğinin olması önerilir ve bu da *Hot* instance yoksa müşteri kullanımında bir düşüşe neden olur.

**Soğuk**: Bu tür bir örnek, bölge başına dağıtılabilir toplam örnek sayısını temsil eder. Soğuk durumlar, bir müşteri test sürüşü istediğinde tüm Test Sürücüsü Kaynak Yöneticisi şablonuna dağıtılmasını gerektirir, bu nedenle *Soğuk* örneklerin yüklenmesi *Sıcak* örneklerden çok daha yavaştır. Bunun yanı sıra, yalnızca test sürüşü süresi için ödeme niz olması, her zaman Sıcak *örneğinde* olduğu gibi Azure aboneliğinizde *çalışmamasıdır.*

- **Test sürüşü Azure Kaynak Yöneticisi şablonu**: Azure Kaynak Yöneticisi şablonunuzu içeren .zip'i yükleyin.  Azure portalını kullanarak Azure [Kaynak Yöneticisi şablonları oluştur ve dağıtıla](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal)başlıklı hızlı başlangıç makalesinde Bir Azure Kaynak Yöneticisi şablonu oluşturma hakkında daha fazla bilgi edinin.

- **Test sürüşü süresi** (gerekli): Test Sürüşünün etkin kalacağı süreyi saat sayısı olarak girin. Bu süre sona erdikten sonra Test Sürüşü otomatik olarak sona erer. Bu süre yalnızca tam saat sayısına göre ayarlanabilir (örneğin, "2" saat; "1.5" geçerli değildir).

### <a name="technical-configuration-for-dynamics-365-test-drive"></a>Dynamics 365 test sürüşü için teknik yapılandırma

Microsoft, bu tür bir test sürücüsükullanarak hizmet sağlama ve dağıtımını barındırarak ve sürdürerek bir test sürücüsü ayarlamanın karmaşıklığını kaldırabilir. Bu tür barındırılan test sürüşü yapılandırması, test sürücüsünün Bir İş Merkezi, Müşteri Etkileşimi veya Operasyonlar hedef kitlesini hedefleyip hedeflemediğine bakılmaksızın aynıdır.

- **Maksimum eşzamanlı test sürücüleri** (gerekli): Test sürüşünüzü aynı anda kullanabilecek maksimum müşteri sayısını ayarlayın. Her eşzamanlı kullanıcı test sürüşü etkinken bir Dynamics 365 lisansı tüketir, bu nedenle maksimum sınır kümesini desteklemek için yeterli lisansa sahip olduğunuzdan emin olmanız gerekir. Önerilen değer 3-5.

- **Test sürüşü süresi** (gerekli): Saat sayısını tanımlayarak Test Sürüşünün etkin kalacağı süreyi girin. Bu kadar saatten sonra oturum sona erer ve artık lisanslarınızdan birini tüketmeyecek. Teklifinizin karmaşıklığına bağlı olarak 2-24 saat lik bir değer öneriyoruz. Bu süre yalnızca tam saat sayısına göre ayarlanabilir (örneğin, "2" saat; "1.5" geçerli değildir).  Kullanıcı, zamanı biterse ve test sürücüsüne yeniden erişmek isterse yeni bir oturum isteyebilir.

- **Örnek URL** (gerekli): Müşterinin test sürüşüne başlayacağı URL. Genellikle dynamics 365 örneğinizin URL'si uygulamanızı örnek veriler `https://testdrive.crm.dynamics.com`yüklü olarak çalıştırıyor (örneğin, ).

- **Örnek Web API URL'si** (gerekli): Microsoft 365 hesabınızda oturum açarak ve **Ayarlar** \&gt'de gezinerek Dynamics 365 örneğiniz için Web API URL'sini alın; **Özelleştirme** \&gt; **Geliştirici Kaynakları** \&gt; **Örnek Web API (Hizmet Kökü URL)**, burada `https://testdrive.crm.dynamics.com/api/data/v9.0`bulunan URL'yi kopyalayın (örneğin, ).

- **Rol adı** (gerekli): Özel Dynamics 365 test sürüşünüzde tanımladığınız güvenlik rol adını sağlayın. Bu, test sürüşü sırasında kullanıcıya atanır (örneğin, test sürüşü rolü).

### <a name="technical-configuration-for-logic-app-test-drive"></a>Logic uygulaması test sürüşü için teknik yapılandırma

Herhangi bir özel ürün, çeşitli karmaşık çözüm mimarilerini kapsayan bu test sürücüsü dağıtım şablonu türünü kullanmalıdır. Logic App test sürücüleri ayarlama hakkında daha fazla bilgi için GitHub'da [Operasyonlar](https://github.com/Microsoft/AppSource/blob/master/Setup-your-Azure-subscription-for-Dynamics365-Operations-Test-Drives.md) ve [Müşteri Katılımı'nı](https://github.com/Microsoft/AppSource/wiki/Setting-up-Test-Drives-for-Dynamics-365-app) ziyaret edin.

- **Bölge** (gerekli, tek seçimaçılır liste): Şu anda test sürüşünüzün kullanılabildiği 26 Azure destekli bölge bulunmaktadır. Mantık uygulamanızın kaynakları seçtiğiniz bölgede dağıtılır. Mantık Uygulamanızda belirli bir bölgede depolanan özel kaynaklar varsa, o bölgenin burada seçildiğinden emin olun. Bölgeniz için özel kaynaklara sahip olmasını sağlamanın en iyi yolu, Logic App'inizi portaldaki Azure aboneliğinizde yerel olarak tam olarak dağıtmak ve bu seçimi yapmadan önce doğru şekilde çalıştığını doğrulamaktır.

- **Maksimum eşzamanlı test sürücüleri** (gerekli): Test sürüşünüzü aynı anda kullanabilecek maksimum müşteri sayısını ayarlayın. Bu test sürücüleri zaten dağıtıldı ve müşterilerin dağıtım beklemeden bunlara anında erişmesini sağladı.

- **Test sürüşü süresi** (gerekli): Test Sürüşünün etkin kalacağı süreyi saat sayısı olarak girin. Bu süre sona erdikten sonra test sürüşü otomatik olarak sona erer.

- **Azure kaynak grubu adı** (gerekli): Logic App test sürücünüzün kaydedildiği [Azure kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) adını girin.

- **Azure mantık uygulaması adı** (gerekli): Test sürücüsünu kullanıcıya atayan Mantık uygulamasının adını girin. Bu Mantık uygulaması yukarıdaki Azure kaynakları grubuna kaydedilmelidir.

- **Deprovision logic app name** (gerekli): Müşteri tamamlandıktan sonra test sürücüsünün hükmünü bozan Mantık uygulamasının adını girin. Bu Mantık uygulaması yukarıdaki Azure kaynakları grubuna kaydedilmelidir.

### <a name="technical-configuration-not-required-for-power-bi-test-drives"></a>Power BI test sürücüleri için teknik yapılandırma gerekli değildir

Etkileşimli power bi görselgöstermek isteyen ürünler, test sürüşü olarak özel olarak oluşturulmuş bir panopaylaşmak için gömülü bir bağlantı kullanabilir, başka teknik yapılandırma gerekmez. [Power BI](https://docs.microsoft.com/power-bi/service-template-apps-overview) şablon uyrama uygulamalarını ayarlama hakkında daha fazla bilgi edinin.

### <a name="deployment-subscription-details"></a>Dağıtım abonelik ayrıntıları

Test Sürüşü'ni sizin adınıza dağıtmak için ayrı, benzersiz bir Azure Aboneliği oluşturun ve sağlayın. (Power BI test sürücüleri için gerekli değildir).

- **Azure abonelik kimliği** (Azure Kaynak Yöneticisi ve Mantık uygulamaları için gereklidir): Kaynak kullanımı raporlaması ve faturalandırma için Azure hesap hizmetlerinize erişim izni vermek için abonelik kimliğigirin girin. Zaten test sürücüleri için kullanmak üzere [ayrı bir Azure aboneliği oluşturmayı](https://docs.microsoft.com/azure/billing/billing-create-subscription) düşünmenizi öneririz. [Azure portalında](https://portal.azure.com/) oturum açarak ve sol taraftaki menüdeki **Abonelikler** sekmesinde gezinerek Azure abonelik kimliğinizi bulabilirsiniz. Sekmeyi seçmek abonelik kimliğinizi görüntüler (örneğin, "a83645ac-1234-5ab6-6789-1h234g764ghty").

- **Azure AD kiracı kimliği** (gerekli): Azure Etkin Dizini (AD) [kiracı kimliğinizi](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)girin. Bu kimliği bulmak için [Azure portalında](https://portal.azure.com/)oturum açın, sol menüdeki Etkin Dizin sekmesini seçin, **Özellikler'i**seçin ve listelenen **Dizin Kimliği** numarasını arayın (örneğin, 50c464d3-4930-494c-963c-1e951d15360e). Ayrıca, alan adınızın URL'sini kullanarak kuruluşunuzun [https://www.whatismytenantid.com](https://www.whatismytenantid.com)kiracı kimliğine de bakabilirsiniz: .

- **Azure AD kiracı adı** (Dinamik 365 için gereklidir): Azure Etkin Dizini (AD) adınızı girin. Bu adı bulmak için, sağ üst köşedeki [Azure portalında](https://portal.azure.com/)oturum açın, kiracı adınız hesap adınız altında listelenir.

- **Azure AD uygulama kimliği** (gerekli): Azure Etkin Dizini (AD) [uygulama kimliğinizi](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)girin. Bu kimliği bulmak için [Azure portalında](https://portal.azure.com/)oturum açın, sol menüdeki Etkin Dizin sekmesini seçin, **Uygulama kayıtlarını**seçin, ardından listelenen Uygulama **Kimliği** numarasını arayın (örneğin, 50c464d3-4930-494c-963c-1e951d15360e).

- **Azure AD uygulaması istemcisi gizli** (gerekli): Azure AD uygulama [istemcinizi gizli](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#certificates-and-secrets)girin. Bu değeri bulmak için [Azure portalında](https://portal.azure.com/)oturum açın. Sol menüdeki **Azure Active Directory** sekmesini seçin, **Uygulama kayıtlarını**seçin ve ardından test sürüşü uygulamanızı seçin. Sonra, **Sertifikalar ve sırlar**seçin, **Yeni istemci gizli**seçin, bir açıklama girin, Bitiş **Dolması**Altında **Asla** seçin, sonra **Ekle**seçin . Değeri kopyaladığından emin olun. (Değere not vermeden önce sayfadan uzağa gitmez veya değere erişemezsinuz.)

Bir sonraki bölüme geçmeden önce **Kaydet'i** unutmayın!

### <a name="test-drive-marketplace-listings"></a>Test sürüşü pazar yeri listeleri

**Test sürüşü** sekmesinin altında bulunan **Market giriş** seçeneği, test sürücünüzün kullanılabildiği dilleri görüntüler. Şu anda, **İngilizce (AMERIKA Birleşik Devletleri)** mevcut tek yerdir. Test sürüşü deneyimini açıklayan bilgileri girmek için dil adını seçin.

- **Açıklama** (gerekli): Test sürüşünüzü, gösterilecek leri, kullanıcının deney yapacağı hedefleri, araştırılacak özellikleri ve kullanıcının teklifinizi alıp almayacağını belirlemesine yardımcı olacak ilgili bilgileri açıklayın. Bu alana en fazla 3.000 metin karakteri girilebilir.

- **Erişim bilgileri** (Azure Kaynak Yöneticisi ve Mantık test sürücüleri için gereklidir): Bu test sürüşüne erişmek ve bunları kullanmak için müşterinin bilmesi gerekenleri açıklayın. Teklifinizi ve test sürüşü boyunca özelliklere erişmek için müşterinin tam olarak bilmesi gerekenleri kullanmak için bir senaryoda yürüyün. Bu alana en fazla 10.000 karakter metin girilebilir.

- **Kullanım Kılavuzu** (gerekli): Test sürüşü deneyiminizi derinlemesine inceler. Kullanım Kılavuzu, müşterinin test sürüşünü deneyimlemekten elde etmesini tam olarak istediğiniz şeyi kapsamalı ve sahip olabilecekleri sorular için bir referans görevi göremelidir. Dosya PDF formatında olmalı ve yükledikten sonra (en fazla 255 karakter) adlandırılmalıdır.

- **Videolar: Video ekleme** (isteğe bağlı): Videolar YouTube veya Vimeo'ya yüklenebilir ve burada bir bağlantı ve küçük resim görüntüsüyle (533 x 324 piksel) başvurulabilir, böylece bir müşteri, teklifinizin özelliklerini başarıyla nasıl kullanacağınız ve avantajlarıvurgulayan senaryoları nasıl başarılı bir şekilde anlayacağınızı da dahil olmak üzere test sürüşünü daha iyi anlamalarına yardımcı olacak bir bilgi yürüyüşünü görüntüleyebilir.
  - **Ad** (gerekli)
  - **URL (yalnızca YouTube veya Vimeo)** (gerekli)
  - **Küçük resim (533 x 324px)**: Resim dosyası PNG formatında olmalıdır.

## <a name="supplemental-content"></a>Ek içerik

Bu sayfa, teklifinizi doğrulamamıza yardımcı olması için teklifiniz hakkında ek bilgi sağlamanıza olanak tanır. Bu bilgiler müşterilere gösterilmez veya pazara yayınlanmaz.

### <a name="validation-assets"></a>Doğrulama varlıkları

Bu bölüme bir [Özelleştirme Analizi Raporu (CAR)](https://docs.microsoft.com/dynamics365/unified-operations/dev-itpro/dev-tools/customization-analysis-report) yükleyin. Bu rapor, önceden tanımlanmış en iyi uygulama kuralları kümesini temel alarak özelleştirme ve uzantı modellerinizi analiz ederek oluşturulur.

Bu dosya .xls veya .xlsx formatında olmalıdır. Birden fazla raporunuz varsa, tüm raporları içeren bir .zip dosyası yükleyebilirsiniz.

### <a name="does-solution-include-localizations"></a>Çözüm yerelleştirmeleri içeriyor mu?

Çözüm yerel standartların ve politikaların kullanımına olanak sağlıyorsa **Evet'i** seçin (örneğin, farklı ülkeler/bölgeler tarafından gerekli olan farklı bordro kurallarını barındırıyorsa). Gerekmiyorsa **Hayır**'ı seçin.

### <a name="does-solution-enable-translations"></a>Çözüm çevirilere olanak sağlıyor mu?

Çözümünüzdeki metin başka dillere çevrilebiliyorsa **Evet'e** yanıt verin. Gerekmiyorsa **Hayır**'ı seçin.

## <a name="publish"></a>Yayımlama

### <a name="submit-offer-to-preview"></a>Teklifönizlemeye gönderme

Teklifin gerekli tüm bölümlerini tamamladıktan **publish** sonra, portalın sağ üst köşesinde yayınla'yı seçin. **İnceleme ve yayımlama** sayfasına yönlendirileceksiniz.

Bu teklifi ilk kez yayınlıyorsanız, şunları yapabilirsiniz:

- Teklifin her bölümü için tamamlanma durumuna bakın.
    - *BaşlatılmamasA* da, bölüme dokunulmamış ve tamamlanmalıdır.
    - *Eksik* - bölümün düzeltilmesi gereken veya daha fazla bilgi sağlanması nı gerektiren hataları olduğu anlamına gelir. Bölüme geri dön ve güncelleştir.
    - *Tam* - bölüm tamamlandı, gerekli tüm veriler sağlandı ve hiçbir hata yok demektir. Teklifi sunamadan önce teklifin tüm bölümlerinin tam bir durumda olması gerekir.
- Sertifika **notları** bölümünde, uygulamanızın anlaşılmasına yardımcı olan ek notlara ek olarak uygulamanızın doğru test edildiğinden emin olmak için sertifika ekibine test talimatları sağlayın.
- **Gönder'i**seçerek yayımlama için teklif gönderin. Teklifin önizleme sürümünün gözden geçirip onaylamanız için kullanılabilir olduğunda size bildirmek için size bir e-posta göndeririz. Ortak Merkezi'ne dönün ve teklifinizi halka (veya özel bir teklifte özel bir hedef kitleye) yayınlamak için teklif için **Go-live'ı** seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Ticari Market'te bulunan bir teklifi güncelleştirme](./update-existing-offer.md)
