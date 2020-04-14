---
title: İş Ortağı Merkezi'nde Azure kapsayıcı teklifi oluşturma - Azure Marketi
description: Bu makalede, Azure Marketi için bir Kapsayıcı teklifinin nasıl oluşturulup yayımlandırılabildiğini açıklanmaktadır.
author: mingshen
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/07/2020
ms.openlocfilehash: b96fe12ccc0292bb5f689fbecabd53d2af54846e
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81266324"
---
# <a name="create-an-azure-container-offer"></a>Azure Kapsayıcı teklifi oluşturma

> [!IMPORTANT]
> Azure Konteyner tekliflerinizin yönetimini Bulut İş Ortağı Portalı'ndan İş Ortağı Merkezi'ne taşıyoruz. Teklifleriniz geçirilene kadar, tekliflerinizi yönetmek için lütfen Bulut İş Ortağı Portalı [için Kapsayıcılar'daki](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/containers/cpp-containers-offer) yönergeleri izleyin.

Bu makalede, Azure Marketi için bir Kapsayıcı teklifinin nasıl oluşturulup yayımlandırılabildiğini açıklanmaktadır. Başlamadan [önce, İş Merkezi'nde Ticari Pazar Yeri hesabı oluşturun.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account)

## <a name="create-a-new-offer"></a>Yeni teklif oluşturma

1. [Ortak Merkezi'nde](https://partner.microsoft.com/dashboard/home)oturum açın ve ardından üst menüden **Pano'yu**seçin.
2. Sol menüde, **Commercial Marketplace'i**seçin, ardından **Genel Bakış'** ı seçin.
3. Genel **Bakış** sayfasında + **Yeni teklif ,** ardından Azure **Kapsayıcı'yı**seçin. **Yeni teklif** iletişim kutusu görüntülenir.

:::image type="content" source="media/azure-create-container-offer-images/azure-create-new-container.png" alt-text="İş Ortağı Merkezi'ndeki Genel Bakış sayfasını gösterilmiştir. Yeni teklif düğmesi ve Danışmanlık hizmet teklifi vurgulanır.":::

> [!TIP]
> Bir teklif yayımlandıktan sonra, İş Ortağı Merkezi'nde yapılan yapılan lar yalnızca teklifi yeniden yayımladıktan sonra vitrinlerde görünür. Değişiklik yaptıktan sonra her zaman yeniden yayımladığınızdan emin olun.

### <a name="offer-id-and-alias"></a>Teklif kimliği ve takma ad

Teklif **Kimliği**girin. Bu, hesabınızdaki her teklif için benzersiz bir tanımlayıcıdır.

- Bu kimlik, müşteriler tarafından pazar teklifi ve varsa Azure Kaynak Yöneticisi şablonlarının web adresinde görülebilir.
- Yalnızca küçük harfleri ve rakamları kullanın. Tireler ve alt çizerler içerebilir, ancak boşluk yoktur ve 50 karakterle sınırlıdır. Örneğin, **test-teklif-1**girerseniz, teklif web adresi `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
- **Teklif Kimliği, Oluştur'u**seçtikten sonra değiştirilemez.

**Teklif takma adını** **girin.** Bu, Ortak Merkezi'ndeki teklifi ifade etmek için kullanılan addır.

- Bu ad pazarda kullanılmaz ve teklif adı ve müşterilere gösterilen diğer değerlerden farklıdır.
- **Oluştur'u**seçtikten sonra bu değişiklik edilemez.

Devam etmeden önce **Oluştur'u** seçin.

## <a name="offer-overview"></a>Teklife genel bakış

**Teklife genel bakış** sayfası, bu teklifi yayımlamak için gereken adımların (hem tamamlanmış hem de yaklaşan) ve her adımın tamamlanmasının ne kadar sürmesi gerektiğinin görsel bir gösterimini gösterir.

Bu sayfa, teklifin geçerli durumuna göre farklı bağlantılar gösterir. Örneğin:

- Teklif taslaksa - [Taslak teklifi sil](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#delete-a-draft-offer)
- Teklif canlıise - [Teklifi satmayı bırakın](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#stop-selling-an-offer-or-plan)
- Teklif önizlemedeyse - [Canlı Yayına Geçin](https://docs.microsoft.com/azure/marketplace/partner-center-portal/publishing-status#publisher-approval)
- Yayımcı oturum açmayı tamamlamadıysanız - [Yayımlamayı iptal et](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#cancel-publishing)

## <a name="offer-setup"></a>Teklif kurulumu

Teklifinizi ayarlamak için aşağıdaki adımları izleyin.

### <a name="connect-lead-management--optional"></a>Müşteri adayı yönetimini bağlayın – isteğe bağlı

Teklifinizi İş Ortağı Merkezi ile pazara yayınlarken, müşteri ilişkileri yönetimi (CRM) sisteminize bağlayabilirsiniz. Bu, birisi ürününüzün ilgisini ifade eder etmez veya ürününüzü kullanır kullanmaz müşteri iletişim bilgilerini almanızı sağlar.

1. **Müşteri müşteri adaylarını göndermemizi istediğiniz bir müşteri adayı seçin.** İş Ortağı Merkezi aşağıdaki CRM sistemlerini destekler:

- Müşteri Katılımı için [Dynamics 365](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics)
- [Marketo](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-marketo)
- [Salesforce](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce)

> [!NOTE]
> CRM sisteminiz yukarıda listelenmemişse, müşteri müşteri adayı verilerini depolamak için [Azure Tablosu](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table) veya [Https Endpoint'i](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-https) kullanın ve verileri CRM sisteminize aktarın.

2. İş Ortağı Merkezi'nde yayın yaparken teklifinizi müşteri adayı hedefe bağlayın.
3. Müşteri adayı hedefine olan bağlantının düzgün şekilde yapılandırıldığı doğrulayın. İş Ortağı Merkezi'nde yayımladıktan sonra bağlantıyı doğrular ve size bir test ipucu göndeririz. Teklifi yayına girmeden önce önizlemenize kadar, teklifi önizleme ortamında kendiniz satın almaya çalışarak müşteri adayı bağlantınızı da test edebilirsiniz.
4. Müşteri adayını kaybetmemek için müşteri adayı hedefine bağlantının güncel kaldığından emin olun.

Aşağıda bazı ek müşteri adayı yönetim kaynakları vereme

- [Müşteri adayı yönetimine genel bakış](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-get-customer-leads)
- [Müşteri adayı yönetimi hakkında SSS](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [Sık karşılaşılan müşteri adayı yapılandırma hataları](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#common-lead-configuration-errors-during-publishing-on-cloud-partner-portal)
- [Müşteri Adayı Yönetimine Genel Bakış](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf) PDF (Açılır pencere engelleyicinizin kapalı olduğundan emin olun)

Özellikler adlı bir sonraki bölüme devam etmeden önce **taslağı kaydet'i** seçin.

### <a name="properties"></a>Özellikler

Bu sayfa, teklifinizi pazarda gruplandırmak için kullanılan kategorileri ve teklifinizi destekleyen yasal sözleşmeleri tanımlamanızı sağlar.

#### <a name="category"></a>Kategori

En az bir ve en fazla beş kategori seçin. Bu kategoriler, teklifinizi uygun pazar yeri arama alanlarına yerleştirmek için kullanılır ve teklif ayrıntıları sayfanızda gösterilir. Teklif açıklamasında, teklifinizin bu kategorileri nasıl desteklediğini açıklayın. **Kapsayıcılar Kapsayıcılar** ve ardından **Kapsayıcı Görüntüler** kategorisi altında görünür.

#### <a name="legal"></a>Yasal Bildirim

Teklif için şart ve koşulları sağlamalısınız. İki seçenek vardır:

- Microsoft ticari pazarı için Standart Sözleşme'yi kullanın.
- Kendi şart ve koşullarınızı sağlayın.

##### <a name="standard-contract-for-the-microsoft-commercial-marketplace"></a>Microsoft ticari pazarı için standart sözleşme

Ticari pazardaki işlemleri kolaylaştırmaya yardımcı olmak için standart sözleşme şablonu sunuyoruz. Çözümünüzü, müşterilerin yalnızca bir kez kontrol edip kabul etmesi gereken Standart Sözleşme kapsamında sunmayı seçebilirsiniz. Özel hüküm ve koşullar oluşturmak istemiyorsanız, bu iyi bir seçenektir.

Standart Sözleşme hakkında daha fazla bilgi edinmek [için Microsoft ticari pazarı için Standart Sözleşme'ye](https://docs.microsoft.com/azure/marketplace/standard-contract)bakın. [Standart Sözleşme](https://go.microsoft.com/fwlink/?linkid=2041178) PDF'sini de indirebilirsiniz (açılır pencere engelleyicinizin kapalı olduğundan emin olun).

Standart Sözleşme'yi kullanmak **için, Microsoft'un ticari pazar yeri** onay kutusunu için Standart Sözleşmeyi Kullan'ı seçin ve sonra **Kabul Et'i**tıklatın.

> [!NOTE]
> Microsoft ticari pazarı için Standart sözleşmeyi kullanarak bir teklif yayımladıktan sonra, kendi özel hüküm ve koşullarınızı kullanamazsınız. Çözümünüzü Standart Sözleşme kapsamında veya kendi hüküm ve koşullarınız altında sunun.

:::image type="content" source="media/azure-create-container-offer-images/azure-create-1-standard-contract.png" alt-text="Microsoft'un ticari pazar onay kutusu için Standart Sözleşmekullanma'yı gösterir.":::

##### <a name="your-own-terms-and-conditions"></a>Kendi hüküm ve koşullarınız

Kendi özel hüküm ve koşullarınızı sağlamak için, bunları **Şartlar ve Koşullar** kutusuna girin. Bu kutuya sınırsız miktarda metin karakteri girebilirsiniz. Müşteriler teklifinizi denemeden önce bu koşulları kabul etmelidir.

Bir sonraki bölüme, Teklif listesine devam etmeden önce **taslağı Kaydet'i** seçin.

## <a name="offer-listing"></a>Teklif listesi

Bu sayfa, ticari pazarda görüntülenen teklif ayrıntılarını tanımlamanızı sağlar. Bu teklif adı, açıklama ve görüntüleri içerir.

> [!NOTE]
> Teklif açıklaması "Bu uygulama yalnızca [İngilizce olmayan dilde] olarak sunulur" ifadesiyle başlarsa, teklif ayrıntılarının İngilizce olması gerekmez. Teklif listesi ayrıntılarında kullanılandan farklı bir dilde içerik sunmak için yararlı bir bağlantı sağlamak da sorun değildir.

### <a name="name"></a>Adı

Buraya girdiğiniz ad, teklifinizin başlığı olarak görüntülenir. Bu alan, teklifi oluşturduğunuzda Teklif **takma adı** kutusuna girdiğiniz metinle önceden doldurulmuştur. Bu adı daha sonra değiştirebilirsiniz.

Adı:

- Ticari marka olabilir (ve ticari marka ve telif hakkı sembolleri dahil edebilirsiniz).
- 50 karakterden uzun olamaz.
- Emojileri içeremiyorum.

### <a name="search-results-summary"></a>Arama sonuçları özeti

Teklifinizin kısa bir açıklaması. Bu, 100 karaktere kadar uzun olabilir ve pazar yeri arama sonuçlarında kullanılır.

### <a name="long-summary"></a>Uzun özet

Teklifinizin daha ayrıntılı bir açıklaması. Bu, 256 karakter uzunluğunda olabilir ve pazar yeri arama sonuçlarında kullanılır.

### <a name="description"></a>Açıklama

Teklifinizin 3.000 karaktere kadar daha uzun bir açıklamasını sağlayın. Bu, pazar yeri giriş özetinde müşterilere görüntülenir.

Açıklamanıza aşağıdakilerden birini veya birkaçını ekleyin:

- Teklifinizin sağladığı değer ve önemli avantajlar
- Kategori veya endüstri dernekleri veya her ikisi
- Uygulama içi satın alma fırsatları
- Gerekli açıklamalar

Açıklamanızı yazmak için bazı ipuçları aşağıda verilmiştir:

- Teklifinizin değerini açıklamanızın ilk birkaç cümlesinde açıkça açıklayın. Aşağıdaki öğeleri ekleyin:
  - Teklifin açıklaması.
  - Tekliften yararlanan kullanıcı türü
  - Müşteri ihtiyaçları veya teklif adreslerini sorunları.
- İlk birkaç cümlenin arama sonuçlarında görüntülenebileceğini unutmayın.
- Ürününüzü satmak için özelliklere ve işlevselliğe güvenmeyin. Bunun yerine, teklifinizin sağladığı değere odaklanın.
- Sektöre özgü kelime dağarcığı veya fayda tabanlı ifadeler kullanmayı deneyin.

Teklif **açıklamanızı** daha ilgi çekici hale getirmek için, açıklamanızı biçimlendirmek için zengin metin düzenleyicisini kullanın. numaralandırma, madde işaretleri, kalın, italik ve girintilerle açıklamanızı daha okunabilir hale getirmek için.

:::image type="content" source="media/text-editor2.png" alt-text="Zengin metin düzenleyicisini gösterir." border="false" :::

- Metne paragraf stili uygulamak için bu açılır uygulamayı kullanın.

    :::image type="content" source="media/text-editor3.png" alt-text="Zengin metin düzenleyicisinde metin stili denetimini gösterir." border="false":::

- Metne numaralandırma veya madde işaretleri uygulamak için bu simgeleri kullanın.

     :::image type="content" source="media/text-editor4.png" alt-text="Zengin metin düzenleyicisinde madde işaretli ve numara listesi denetimlerini gösterir." border="false":::

- Girintiyi metine veya metinden eklemek veya kaldırmak için bu simgeleri kullanın.

    :::image type="content" source="media/text-editor5.png" alt-text="Zengin metin düzenleyicisindeki girinti denetimlerini gösterir." border="false":::

#### <a name="privacy-policy-link"></a>Gizlilik politikası bağlantısı

Kuruluşunuzun gizlilik ilkesinin web adresini girin. Teklifinizin gizlilik yasalarına ve yönetmeliklerine uygun olmasını sağlamaksizin sorumlusunuz. Ayrıca web sitenizde geçerli bir gizlilik politikası yayınlamaksizin de sorumlusunuz.

#### <a name="useful-links"></a>Yararlı bağlantılar

Teklifiniz hakkında ek çevrimiçi belgeler sağlayın. En fazla 25 bağlantı ekleyebilirsiniz. Bağlantı eklemek için **+ Bağlantı ekle'yi** seçin ve ardından aşağıdaki alanları tamamlayın:

- **Başlık** - Müşteriler bunu teklifinizin ayrıntılar sayfasında göreceksiniz.
- **Link (URL)** – Müşterilerin çevrimiçi belgenizi görüntülemesi için bir bağlantı girin. Bağlantı http:// veya https:// ile başlamalıdır.

### <a name="contact-information"></a>İletişim Bilgileri

**Destek kişisi** ve Mühendislik **kişisi**için adı, e-postayı ve telefon numarasını sağlamanız gerekir. Bu bilgiler müşterilere gösterilmez, ancak Microsoft tarafından kullanılabilir. Bulut Çözüm Sağlayıcısı (CSP) iş ortaklarına da sağlanabilir.

- Destek kişisi (gerekli): Genel destek soruları için.
- Mühendislik iletişimi (gerekli): Teknik sorular ve belgelendirme sorunları için.
- CSP Programı iletişim bilgileri (isteğe bağlı): CSP programıyla ilgili bayi soruları için.

Destek **iletişim** bölümünde, teklifin genel Azure, Azure Resmi veya her ikisinde de kullanılabilir olup olmadığına bağlı olarak iş ortaklarının teklifiniz için destek bulabilecekleri **Destek web sitesini** sağlayın.

**CSP Programı iletişim** bölümünde, CSP iş ortaklarının teklifiniz için pazarlama malzemeleri bulabilecekleri bağlantıyı **(CSP Program Pazarlama Malzemeleri)** sağlayın.

#### <a name="additional-marketplace-listing-resources"></a>Ek pazar yeri listeleme kaynakları

Teklif listeleri oluşturma hakkında daha fazla bilgi edinmek için, [en iyi uygulamaları listele'ye](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices) bakın

### <a name="marketplace-images"></a>Pazar yeri görüntüleri

Teklifinizle birlikte kullanılacak logolar ve resimler sağlayın. Tüm görüntüler PNG formatında olmalıdır. Bulanık görüntüler reddedilir.

#### <a name="store-logos"></a>Mağaza logoları

 Teklifinizin logosunun PNG dosyalarını aşağıdaki dört piksel boyutlarının her birinde sağlayın:

- **Küçük** (48 x 48)
- **Orta** (90 x 90)
- **Büyük** (216 x 216)
- **Geniş** (255 x 115)

Dört logo gereklidir ve pazar listesinde farklı yerlerde kullanılır.

#### <a name="screenshots-optional"></a>Ekran görüntüleri (isteğe bağlı)

Teklifinizin nasıl çalıştığını gösteren en fazla beş ekran görüntüsü ekleyin. Her biri 1280 x 720 piksel boyutunda ve PNG formatında olmalıdır.

#### <a name="videos-optional"></a>Videolar (isteğe bağlı)

Teklifinizi gösteren en fazla beş video ekleyin. Videonun adını, web adresini ve videonun 1280 x 720 piksel boyutunda küçük resim png görüntüsünü girin.

#### <a name="offer-examples"></a>Teklif örnekleri

Aşağıdaki örnekler, teklif listeleme alanlarının teklifin farklı yerlerinde nasıl göründüğünü gösterir.

Bu, Azure Marketi'ndeki **Teklif giriş** sayfasını gösterir:

:::image type="content" source="media/azure-create-container-offer-images/azure-create-6-offer-listing-mkt-plc.png" alt-text="Azure Marketi'ndeki Teklif giriş sayfasını gösterir." :::

Bu, Azure Marketi'ndeki arama sonuçlarını gösterir:

:::image type="content" source="media/azure-create-container-offer-images/azure-create-7-search-results-mkt-plc.png" alt-text="Azure Marketi'ndeki arama sonuçlarını gösterir.":::

Bu, Azure portalındaki **Teklif giriş** sayfasını gösterir:

:::image type="content" source="media/azure-create-container-offer-images/azure-create-8-offer-listing-portal.png" alt-text="Azure portalındaki Teklif giriş sayfasını gösterir.":::

Bu, Azure portalındaki arama sonuçlarını gösterir:

:::image type="content" source="media/azure-create-container-offer-images/azure-create-9-search-results-portal.png" alt-text="Azure portalındaki arama sonuçlarını gösterir.":::

## <a name="preview"></a>Önizleme

Önizleme sekmesinde, teklifinizi canlı yayınlamadan önce doğrulamak için sınırlı bir **Önizleme Hedef Kitlesi** seçebilirsiniz.

> [!IMPORTANT]
> Teklifinizi **Önizleme'de**görüntüledikten sonra, teklifinizi herkese yayınlamak için **Canlı Yayına Gönder'i** seçmeniz gerekir.

Azure abonelik kimliği GUID'lerini ve her biri için isteğe bağlı bir açıklama kullanarak önizleme hedef kitlenizi belirtin. Bu alanların hiçbiri müşteriler tarafından görülemez.

> [!NOTE]
> Azure abonelik kimliğinizi Azure portalındaki Abonelikler sayfasında bulabilirsiniz.

Tek tek (10'a kadar) veya bir CSV dosyası yükleyerek (100'e kadar) en az bir Azure abonelik kimliği ekleyin. Bu abonelik adlarını ekleyerek, teklifinizi canlı yayınlanmadan önce kimlerin önizgösterip izleyebileceğini belirlersiniz. Teklifiniz zaten yayındaysa, teklifinizdeki değişiklikleri veya güncellemeleri test etmek için bir önizleme hedef kitlesi seçebilirsiniz.

> [!NOTE]
> Önizleme kitlesi özel bir hedef kitleden farklıdır. Önizleme **preview** kitlesi, yalnızca **özel** bir hedef kitleye (Kullanılabilirlik sekmesinde ayarlanmış) yayınlanacak olanlar da dahil olmak üzere, tüm teklif planlarını pazarda yayınlanmadan önce görebilir ve onaylayabilir.

Devam etmeden önce **taslağı Kaydet'i** seçin.

### <a name="plan-overview"></a>Plana genel bakış

Bu sekme, aynı teklif içinde farklı plan seçenekleri sağlamanızı sağlar. Bu planlar daha önce SK'ler veya stok tutma birimleri olarak anılırdı. Planlar, küresel bulutlar, Devlet bulutları ve planın atıfta bulunduğu görüntü gibi bulutların nelerin mevcut olduğu açısından farklılık görebilir. Teklifinizi ticari pazarda listelemek için en az bir plan ayarlamanız gerekir.

Planlarınızı oluşturduktan sonra, **Plana genel bakış** sekmesi şunları gösterir:

- Plan adları
- Fiyatlandırma modeli
- Bulut kullanılabilirliği (Genel veya Devlet)
- Geçerli yayımlama durumu
- Kullanılabilir eylemler

Plan'a genel bakışta bulunan eylemler, planınızın geçerli durumuna bağlı olarak değişir. Şunları içerir:

- **Taslağı sil** – Plan durumu taslaksa.
- **Satış planını durdurun** – Plan durumu canlı olarak yayınlanırsa.

#### <a name="create-new-plan"></a>Yeni plan oluşturma

**Yeni plan Oluştur'u**seçin. **Yeni plan** iletişim kutusu görüntülenir.

Plan **Kimliği** kutusunda, bu teklifteki her plan için benzersiz bir plan tanımlayıcısı oluşturun. Bu kimlik, ürünün web adresindeki müşteriler tarafından görülebilir. Yalnızca küçük harfleri ve sayıları, tireleri veya alt çiziyi ve en fazla 50 karakter kullanın.

> [!NOTE]
> **Oluştur'u**seçtikten sonra plan kimliği değiştirilemez.

Plan **adı** kutusuna, bu plan için bir ad girin. Müşteriler, teklifiniz içinde hangi planı seçiniz gerekip tespin edindiğinde bu adı görür. Bu teklifteki her plan için benzersiz bir ad oluşturun. Örneğin, Windows Server 2016 ve **Windows Server 2019**planlarını içeren bir **teklif** adı Kullanabilirsiniz. **Windows Server 2016**

### <a name="plan-setup"></a>Kurulum planı

Bu sekme, planın hangi bulutlarda kullanılabileceğini seçmenize olanak tanır. Bu sekmedeki yanıtlarınız, diğer sekmelerde hangi alanların görüntüleneceğini etkiler.

#### <a name="cloud-availability"></a>Bulut kullanılabilirliği

Planınız en az bir bulutta kullanılabilir olmalıdır.

Planınızın ticari pazarı kullanan tüm küresel Azure bölgelerindeki müşteriler tarafından kullanılabilmesi için **Azure Global** seçeneğini seçin. Ayrıntılar için [Bkz. Coğrafi kullanılabilirlik ve para birimi desteği.](https://docs.microsoft.com/azure/marketplace/marketplace-geo-availability-currencies)

Çözümünüzün burada görünmesini sağlamak için [**Azure Kamu Bulutu**](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome) seçeneğini belirleyin. Bu, ABD federal, eyalet ve yerel veya kabile devlet kurumlarının yanı sıra onlara hizmet vermeye uygun iş ortaklarından müşteriler için kontrollü erişime sahip bir devlet topluluğu bulutudur. Yayımcı olarak, bu bulut topluluğu için tüm uyumluluk denetimlerinden, güvenlik önlemlerinden ve en iyi uygulamalardan siz sorumlusunuz. Azure Kamu fiziksel olarak yalıtılmış veri merkezlerini ve ağlarını kullanır (yalnızca ABD'de bulunur).

Azure Kamu'da [yayımlamadan](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners) önce, sonuçlar farklı olabileceğinden, bu alandaki çözümünüzü test edin ve onaylayın. Çözümünüzü oluşturmak ve test etmek için [Microsoft Azure Kamu deneme sürümünden](https://azure.microsoft.com/global-infrastructure/government/request/)bir deneme hesabı isteyin.

> [!NOTE]
> Planınız yayınlandıktan ve belirli bir bulutta kullanılabilir hale sunulduktan sonra, bu bulutu kaldıramazsınız.

#### <a name="azure-government-cloud-certifications"></a>Azure Kamu Bulutu sertifikaları

Bu seçenek yalnızca **Azure Kamu Bulutu** **Bulut kullanılabilirliği**altında seçilirse görülebilir.

Azure Kamu hizmetleri, belirli devlet yönetmeliklerine ve gereksinimlerine tabi olan verileri işler. Örneğin, FedRAMP, NIST 800.171 (DIB), ITAR, IRS 1075, DoD L4 ve CJIS.

Bu programlara ait sertifikalarınızı göstermek için, bunları açıklayan en fazla 100 bağlantı sağlayabilirsiniz. Bunlar, doğrudan programdaki girişlerinize veya kendi web sitenize bağlantılar olabilir. Bu bağlantılar yalnızca Azure Kamu müşterileri tarafından görülebilir.

## <a name="plan-listing"></a>Plan listesi

Bu sekme, geçerli teklifteki her farklı plan için belirli bilgileri görüntüler.

### <a name="plan-name"></a>Plan adı

Bu, planı oluşturduğunuzda vermiş olduğunuz adla önceden doldurulmuştur. Bu adı gerektiği gibi değiştirebilirsiniz. En fazla 50 karakter uzunluğunda olabilir. Bu ad, Azure Marketi ve Azure portalında bu planın başlığı olarak görünür. Plan kullanıma hazır olduktan sonra varsayılan modül adı olarak kullanılır.

### <a name="plan-summary"></a>Plan özeti

Yazılım planınızın kısa bir özeti (teklif değil). Bu özet Azure Marketi arama sonuçlarında görünür ve en fazla 100 karakter içerebilir.

### <a name="plan-description"></a>Plan açıklaması

Bu yazılım planını benzersiz kılan ın yanı sıra teklifinizdeki planlar arasındaki farkları açıklayın. Teklifi tarif etme, sadece planı. Bu açıklama Azure Marketi'nde ve **Teklif giriş** sayfasındaki Azure portalında görünür. Plan özetinde sağladığınız içeriğin aynısı olabilir ve en fazla 2.000 karakter içerebilir.

Bu alanları tamamladıktan sonra **Kaydet'i** seçin.

#### <a name="plan-examples"></a>Plan örnekleri

Aşağıdaki örnekler, plan listeleme alanlarının farklı görünümlerde nasıl göründüğünü gösterir.

Plan ayrıntılarını görüntülerken Azure Marketi'ndeki alanlar şunlardır:

:::image type="content" source="media/azure-create-container-offer-images/azure-create-10-plan-details-mtplc.png" alt-text="Azure Marketi'nde plan ayrıntılarını görüntülerken gördüğünüz alanları gösterir.":::

Azure portalındaki plan ayrıntıları şunlardır:

:::image type="content" source="media/azure-create-container-offer-images/azure-create-11-plan-details-portal.png" alt-text="Azure portalındaki plan ayrıntılarını gösterir.":::

## <a name="plan-availability"></a>Plan kullanılabilirliği

Müşterilerin pazarda arama yapamaması, göz atmaması veya satın almaması için yayımlanmış teklifinizi gizlemek istiyorsanız, **Kullanılabilirlik** sekmesindeki **Planı Gizle** onay kutusunu seçin.

Bu alan şu anda kullanılır:

- Teklif, başka bir uygulama yla başvurulduğunda dolaylı olarak kullanılmak üzere tasarlanmıştır.
- Teklif tek tek satın alınmamalıdır.
- Plan ilk test için kullanıldı ve artık alakalı değil.
- Plan geçici veya mevsimlik teklifler için kullanıldı ve artık sunulmamalıdır.

## <a name="technical-configuration"></a>Teknik yapılandırma

Kapsayıcı görüntüleri özel bir [Azure Kapsayıcı Kayıt Defteri'nde](https://azure.microsoft.com/services/container-registry/)barındırılmalıdır. Teknik **Yapılandırma** sekmesinde, Azure Kapsayıcı Kayıt Defteri'ndeki kapsayıcı resim deponuz için başvuru bilgilerini sağlayın.

Teklif yayımlandıktan sonra, kapsayıcı resminiz belirli bir genel kapsayıcı kayıt defterinde Azure Marketi'ne kopyalanır. Konteyner resminizi kullanmak için tüm istekler, özel resminizden değil, Azure Marketi genel konteyner kayıt defterinden sunulur. Ayrıntılar için azure [kapsayıcı teknik varlıklarınızı hazırlayın'](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-azure-container-technical-assets)a bakın.

### <a name="image-repository-details"></a>Resim deposu ayrıntıları

**Resim deposu ayrıntıları** sekmesinde aşağıdaki bilgileri sağlayın.

**Azure abonelik kimliği** – Kullanım bildirilen ve kapsayıcı resminizi içeren Azure Kapsayıcı Kayıt Defteri için hizmetlerin faturalandırıldığı abonelik kimliğini sağlayın. Bu kimliği Azure portalında [Abonelikler sayfasında](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) bulabilirsiniz.

**Azure kaynak grubu adı** – Kapsayıcı resminizle Azure Kapsayıcı Kayıt Defteri'ni içeren [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-portal) adını sağlayın. Kaynak grubuna abonelik kimliğinde (üstte) erişilebilir olmalıdır. Adı Azure portalındaki [Kaynak grupları](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceGroups) sayfasında bulabilirsiniz.

**Azure Kapsayıcı Kayıt Defteri adı** – Kapsayıcı resminizin olduğu Azure Kapsayıcı Kayıt [Defteri'nin](https://docs.microsoft.com/azure/container-registry/container-registry-intro) adını sağlayın. Kapsayıcı kayıt defteri, daha önce sağladığınız Azure kaynak grubunda olmalıdır. Tam oturum açma sunucusu adını değil, yalnızca kayıt defteri adını ekleyin. Adından **azurecr.io** atlayıp emin olun. Kayıt defteri adını Azure portalında [Konteyner Kayıt Defterleri sayfasında](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.ContainerRegistry%2Fregistries) bulabilirsiniz.

**Azure Kapsayıcı Kayıt Defteri için yönetici kullanıcı adı** – Kapsayıcı resminizin olduğu Azure Kapsayıcı Kayıt Defteri'ne bağlı yönetici kullanıcı [adı](https://docs.microsoft.com/azure/container-registry/container-registry-authentication#admin-account) sağlayın. Şirketinizin kayıt defterine erişebilmesini sağlamak için kullanıcı adı ve parola gereklidir. Yönetici kullanıcı adı ve parolasını almak **için, yönetici etkin özelliğiAzure** Komut Satırı Arabirimi (CLI) kullanarak **True** olarak ayarlayın. **Yönetici kullanıcısını** isteğe bağlı olarak Azure portalında **Etkinleştir'e** ayarlayabilirsiniz.

 :::image type="content" source="media/azure-create-container-offer-images/azure-create-12-update-container-registry-edit.png" alt-text="Kapsayıcı kayıt defteri iletişim kutusunu güncelleştirin.":::

**Azure Kapsayıcı Kayıt Defteri için Parola** – Azure Kapsayıcı Kayıt Defteri ile ilişkili ve kapsayıcı resminiz olan yönetici kullanıcı adının parolasını sağlayın. Şirketinizin kayıt defterine erişebilmesini sağlamak için kullanıcı adı ve parola gereklidir. Parolayı Azure portalından Konteyner Kayıt **Defteri** > **Erişim Anahtarları'na** giderek veya azure CLI ile [göster komutunu](https://docs.microsoft.com/cli/azure/acr/credential?view=azure-cli-latest#az-acr-credential-show)kullanarak alabilirsiniz.

:::image type="content" source="media/azure-create-container-offer-images/azure-create-13-access-keys.png" alt-text="Access Key menüsünü gösterir.":::

**Azure Kapsayıcı Kayıt Defteri'nde depo adı.** Resminize sahip Azure Kapsayıcı Kayıt Defteri deposunun adını sağlayın. Resmi kayıt defterine iterken deponun adını ekleyin. [Konteyner Kayıt Defteri](https://azure.microsoft.com/services/container-registry/) > **Depoları** sayfasına giderek deponun adını bulabilirsiniz. Daha fazla bilgi için azure [portalındaki kapsayıcı kayıt defteri depolarını görüntüleyin.](https://docs.microsoft.com/azure/container-registry/container-registry-repositories)

> [!NOTE]
> Ad ayarlandıktan sonra değiştirilemez. Hesabınızdaki her teklif için benzersiz bir ad kullanın.

### <a name="image-tags-for-new-versions-of-your-offer"></a>Teklifinizin yeni sürümleri için resim etiketleri

Müşteriler, bir güncelleştirme yayımladığınızda Azure Marketi'nden otomatik olarak güncelleştirme alabilmeli. Güncelleştirmek istemiyorlarsa, resminizin belirli bir sürümünde kalabilmeleri gerekir. Bunu, görüntüye her güncelleştirme yaptığınızda yeni görüntü etiketleri ekleyerek yapabilirsiniz.

### <a name="image-tag"></a>Resim etiketi

Bu alan, desteklenen tüm platformlarda görüntünüzün en son sürümüne işaret eden **en son** bir etiket içermelidir. Ayrıca bir sürüm etiketi içermelidir (örneğin, xx.xx.xx ile başlayan, xx bir sayı olduğu). Müşteriler birden çok platformu hedeflemek için [bildirim etiketleri](https://github.com/estesp/manifest-tool) kullanmalıdır. Bir bildirim etiketiyle başvurulan tüm etiketlerde de eklenmelidir, böylece bunları yükleyebiliriz.

Tüm manifesto etiketleri (en son etiket hariç) **-** X, Y ve Z'nin sondaolduğu X.Y veya X.Y.Z ile başlamalıdır. Örneğin, en **son** etiket 1.0.1-linux-x64, 1.0.1-linux-arm32 ve 1.0.1-windows-arm32'ye işaret ederse, bu altı etiketin bu alana eklenmesi gerekir. Ayrıntılar için azure [kapsayıcı teknik varlıklarınızı hazırlayın'](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-azure-container-technical-assets)a bakın.

> [!NOTE]
> Test sırasında görüntüyü tanımlayabilmek için resminize bir test etiketi eklemeyi unutmayın.

## <a name="review-and-publish"></a>İnceleme ve yayınlama

Teklifin gerekli tüm bölümlerini tamamladıktan sonra, bunu incelemek ve yayınlamak için gönderebilirsiniz.

Portalın sağ üst köşesinde, **Gözden Geçir'i** seçin ve **yayınlayın.**

İnceleme sayfasında şunları yapabilirsiniz:

- Teklifin her bölümü için tamamlanma durumuna bakın. Teklifin tüm bölümleri tamamlanana kadar yayımlayamazsınız.
  - **Başlatılmadı** – Henüz başlatılmadı ve tamamlanması gerekiyor.
  - **Eksik** – Düzeltilmesi gereken veya daha fazla bilgi sağlamanızı gerektiren hatalar vardır. Yardım için bu belgedeki önceki bölümlere bakın.
  - **Komple** – Hatasız gerekli tüm verileri içerir. Teklifi sunamadan önce teklifin tüm bölümlerinin tamamlanmış olması gerekmektedir.
- Teklifinizin doğru test edildiğinden emin olmak için belgelendirme ekibine test talimatları sağlayın. Ayrıca, teklifinizi anlamak için yararlı olan ek notlar sağlayın.

Yayımlama teklifini göndermek için **Yayımla'yı**seçin.

Teklifin önizleme sürümünün gözden geçirilip onaylamayacağını bildirmek için size bir e-posta göndeririz.

Teklifinizi halka (veya özel bir teklifte özel bir hedef kitleye) yayınlamak için Ortak Merkezi'ne gidin ve **Go-live'ı**seçin.

## <a name="next-step"></a>Sonraki adım

- [Ticari Market'te bulunan bir teklifi güncelleştirme](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer)
