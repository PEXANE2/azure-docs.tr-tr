---
title: Azure Container teklifi oluşturma-Azure Marketi
description: Azure Marketi 'ne bir kapsayıcı teklifi oluşturmayı ve yayımlamayı öğrenin.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
author: keferna
ms.author: keferna
ms.date: 06/17/2020
ms.openlocfilehash: 9196bea663d037f269316dc108f99d3f2c282ec7
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88606356"
---
# <a name="create-an-azure-container-offer-in-azure-marketplace"></a>Azure Market'te Azure kapsayıcısı teklifi oluşturma

Bu makalede, Azure Marketi için bir kapsayıcı teklifinin nasıl oluşturulacağı ve yayımlanacağı açıklanır. Başlamadan önce, henüz yapmadıysanız [Iş Ortağı Merkezi 'nde bir ticari Market hesabı oluşturun](create-account.md) . Hesabınızın ticari Market programına kayıtlı olduğundan emin olun.

## <a name="create-a-new-offer"></a>Yeni teklif oluşturma

1. [Iş Ortağı Merkezi](https://partner.microsoft.com/dashboard/home)' nde oturum açın.

2. Sol gezinti menüsünde **ticari Market**  >  **genel bakış**' ı seçin.

3. Genel Bakış sayfasında **+ yeni teklif**  >  **Azure kapsayıcısı**' nı seçin.

   ![Sol gezinti menüsünü gösterir.](./media/new-offer-azure-container.png)

> [!TIP]
> Bir teklif yayımlandıktan sonra, Iş Ortağı Merkezi 'nde yapılan düzenlemeler, teklif yeniden yayımlandıktan sonra yalnızca çevrimiçi mağazalarda görünür. Değişiklik yaptıktan sonra her zaman yeniden yayımlaytığınızdan emin olun.

### <a name="offer-id-and-alias"></a>Teklif KIMLIĞI ve diğer ad

Bir **TEKLIF kimliği**girin. Bu, hesabınızdaki her teklif için benzersiz bir tanımlayıcıdır.

- Bu KIMLIK, varsa Market teklifi ve Azure Resource Manager şablonları için Web adresinde müşteriler tarafından görülebilir.
- Yalnızca küçük harfleri ve rakamları kullanın. Kısa çizgi ve alt çizgi içerebilir, ancak boşluk içeremez ve 50 karakterle sınırlıdır. Örneğin, **Test-teklif-1**girerseniz, teklif Web adresi olacaktır `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1` .
- Teklif KIMLIĞI, **Oluştur**' u seçtikten sonra değiştirilemez.

Bir **teklif diğer adı**girin. Bu, Iş Ortağı Merkezi 'nde teklif için kullanılan addır.

- Bu ad Market 'te kullanılmıyor ve teklif adından ve müşterilere gösterilen diğer değerlerden farklı.
- Bu, **Oluştur**' u seçtikten sonra değiştirilemez.

Teklifi oluşturmak için **Oluştur** ' u seçin ve devam edin.

## <a name="offer-overview"></a>Teklifin genel bakış

**Teklif genel bakış** sayfasında, bu teklifi yayımlamak için gereken adımların görsel bir gösterimi gösterilir (hem tamamlandı hem de yakında) ve her adımın tamamlanması gereken süre.

Bu sayfa, teklifin geçerli durumuna bağlı olarak farklı bağlantıları gösterir. Örneğin:

- Teklif bir taslak- [silme taslağı tekliftir](update-existing-offer.md#delete-a-draft-offer)
- Teklif canlı ise, [teklifi satmaya](update-existing-offer.md#stop-selling-an-offer-or-plan)
- Teklif önizleme sürümündedir- [canlı](publishing-status.md#publisher-approval)
- Yayımcı oturumu kapatma Işlemini tamamlamadıysanız [yayımlamayı Iptal edin](update-existing-offer.md#cancel-publishing)

## <a name="offer-setup"></a>Teklif kurulumu

Teklifinizi ayarlamak için bu adımları izleyin.

### <a name="customer-leads--optional"></a>Müşteri liderleri – isteğe bağlı

Teklifinizi Iş Ortağı Merkezi ile ticari Market 'te yayımlarken, Müşteri Ilişkileri yönetimi (CRM) sisteminize bağlayabilirsiniz. Bu, bir kişi, ürününüzü ilgilendiren veya ürünü kullandığı anda müşteri iletişim bilgilerini almanızı sağlar.

1. **Müşteri adaylarını göndermemizi istediğiniz bir müşteri adayı hedefi seçin**. İş Ortağı Merkezi aşağıdaki CRM sistemlerini destekler:

   - Müşteri katılımı için [Dynamics 365](commercial-marketplace-lead-management-instructions-dynamics.md)
   - [Marketo](commercial-marketplace-lead-management-instructions-marketo.md)
   - [Salesforce](commercial-marketplace-lead-management-instructions-salesforce.md)

   > [!NOTE]
   > CRM sisteminiz yukarıda listelenmiyorsa, müşteri adayı verilerini depolamak için [Azure tablosu](commercial-marketplace-lead-management-instructions-azure-table.md) veya [https uç noktasını](commercial-marketplace-lead-management-instructions-https.md) kullanın, ardından verileri CRM sisteminize dışarı aktarın.

2. Teklifinizi Iş Ortağı Merkezi 'nde yayımlarken lider hedefine bağlayın.
3. Lider hedefi bağlantısının düzgün yapılandırıldığını onaylayın. Iş Ortağı Merkezi 'nde yayımladıktan sonra, bağlantıyı doğrulayacağız ve size bir test lideri göndereceğiz. Teklifi canlı olmadan önce önizlerken, teklifi önizleme ortamında satın almayı deneyerek lider bağlantınızı da test edebilirsiniz.
4. Müşteri adayı hedefine olan bağlantının güncelleştirildiğinden emin olun, böylece müşteri adaylarını kaybetmezsiniz.

Bazı ek müşteri yönetimi kaynakları aşağıda verilmiştir:

- [Müşteri adayı yönetimine genel bakış](commercial-marketplace-get-customer-leads.md)
- [Müşteri adayı yönetimi hakkında SSS](../lead-management-for-cloud-marketplace.md#frequently-asked-questions)
- [Sık karşılaşılan müşteri adayı yapılandırma hataları](../lead-management-for-cloud-marketplace.md#publishing-config-errors)
- [Müşteri adayı yönetimine genel bakış](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf) PDF (açılır pencere engelleyicinizin kapalı olduğundan emin olun)

Devam etmeden önce **Taslağı kaydet** ' i seçin.

### <a name="properties"></a>Özellikler

Bu sayfa, teklifinizi Market 'te gruplandırmak için kullanılan kategorileri ve teklifinizi destekleyen yasal sözleşmeleri tanımlamanızı sağlar.

#### <a name="category"></a>Kategori

Teklifinizi uygun Market arama alanlarında yerleştirmek için kategoriler ve alt kategoriler ' i seçin. Teklifinizin teklif açıklamasında bu kategorileri nasıl desteklediğini açıkladığınızdan emin olun. Seçin:

- Birincil ve ikincil kategori dahil olmak üzere en az bir ve en fazla iki kategori (isteğe bağlı).
- Her birincil ve/veya ikincil kategori için en fazla iki alt kategori. Teklifiniz için geçerli bir alt kategori yoksa, **geçerli değil**' i seçin.

[Teklif listesi En Iyi uygulamalarında](../gtm-offer-listing-best-practices.md)kategorilerin ve alt kategorilerin tam listesini görebilirsiniz. Kapsayıcılar her zaman **kapsayıcılar** altında ve sonra **kapsayıcı görüntüleri** kategorisi ' nde görünür.

#### <a name="legal"></a>Yasal Bildirim

Teklif için hüküm ve koşullar sağlamanız gerekir. İki seçenek vardır:

- Microsoft ticari Market için standart sözleşmeyi kullanın.
- Kendi hüküm ve koşullarınızı belirtin.

#### <a name="standard-contract-for-the-microsoft-commercial-marketplace"></a>Microsoft ticari Market için standart sözleşme

Ticari Market 'teki işlemleri kolaylaştırmaya yardımcı olmak için standart bir sözleşme şablonu sunuyoruz. Çözümünüzü standart sözleşme altında sunmayı tercih edebilirsiniz; Bu, müşterilerin yalnızca bir kez denetlemesi ve kabul etmesi gereken bir çözümdür. Özel hüküm ve koşullar oluşturmak istemiyorsanız bu iyi bir seçenektir.

Standart Sözleşme hakkında daha fazla bilgi edinmek için bkz. [Microsoft ticari Market Için standart sözleşme](../standard-contract.md). Ayrıca [standart sözleşme](https://go.microsoft.com/fwlink/?linkid=2041178) PDF 'sini de indirebilirsiniz (açılır pencere engelleyicisinin kapalı olduğundan emin olun).

Standart sözleşmeyi kullanmak için * * Microsoft 'un ticari Marketi için standart sözleşmeyi kullanın] (.. /standard-contract.md)

> [!NOTE]
> Microsoft ticari Market için standart sözleşmeyi kullanarak bir teklifi yayımladıktan sonra, kendi özel hüküm ve koşullarınızı kullanamazsınız. Çözümünüzü standart sözleşme kapsamında ya da kendi hüküm ve koşullarınızın altına sunun.

:::image type="content" source="media/azure-create-container-offer-images/azure-create-1-standard-contract.png" alt-text="Microsoft 'un ticari Market 'e yönelik standart sözleşmeyi kullanma onay kutusunu gösterir.":::

##### <a name="your-own-terms-and-conditions"></a>Kendi hüküm ve koşullarınız

Kendi özel hüküm ve koşullarınızı sağlamak için **hüküm ve koşullar** kutusuna girin. Bu kutuya metnin sınırsız miktarda karakterini girebilirsiniz. Müşterilerinizin teklifinizi deneyebilmeleri için önce bu koşulları kabul etmesi gerekir.

Bir sonraki bölüme devam etmeden önce **Taslağı kaydet** ' i seçin, teklif listesi.

## <a name="offer-listing"></a>Teklif listesi

Bu sayfa, ticari Market 'te görüntülenen teklif ayrıntılarını tanımlamanıza olanak sağlar. Bu teklif adını, açıklamasını ve görüntülerini içerir.

> [!NOTE]
> Teklif açıklaması, "Bu uygulama yalnızca [Ingilizce olmayan dil] ' de kullanılabilir" ifadesi ile başlıyorsa, teklif ayrıntılarının Ingilizce olması gerekmez. Ayrıca, teklif listesi ayrıntılarından farklı bir dilde içerik sunmak için faydalı bir bağlantı sağlamak da yararlı olabilir.

### <a name="name"></a>Adı

Buraya girdiğiniz ad teklifinizin başlığı olarak görüntülenir. Bu alan, teklifi oluştururken **teklif diğer adı** kutusuna girdiğiniz metin ile önceden doldurulur. Bu adı daha sonra değiştirebilirsiniz.

Ad:

- Trademarked olabilir (ve ticari marka ve telif hakkı sembolleri dahil edebilirsiniz).
- 50 karakterden uzun olamaz.
- Emojıs dahil olamaz.

### <a name="search-results-summary"></a>Arama sonuçları Özeti

Teklifinizin kısa bir açıklaması. Bu, 100 karakter uzunluğunda olabilir ve Market arama sonuçlarında kullanılır.

### <a name="long-summary"></a>Uzun Özet

Teklifinizin daha ayrıntılı bir açıklaması. Bu, 256 karakter uzunluğunda olabilir ve Market arama sonuçlarında kullanılır.

### <a name="description"></a>Açıklama

[!INCLUDE [Long description-1](./includes/long-description-1.md)]

[!INCLUDE [Long description-2](./includes/long-description-2.md)]

[!INCLUDE [Long description-3](./includes/long-description-3.md)]

#### <a name="privacy-policy-link"></a>Gizlilik ilkesi bağlantısı

Kuruluşunuzun gizlilik ilkesinin Web adresini girin. Teklifinizin gizlilik yasaları ve yönetmeliklerle uyumlu olmasını sağlamaktan sorumlu olursunuz. Ayrıca, Web sitenizde geçerli bir gizlilik ilkesi göndermekten de sorumlusunuz.

#### <a name="useful-links"></a>Yararlı bağlantılar

Teklifiniz hakkında ek çevrimiçi belgeler sağlayın. En fazla 25 bağlantı ekleyebilirsiniz. Bir bağlantı eklemek için **+ bağlantı ekle** ' yi seçin ve ardından aşağıdaki alanları doldurun:

- **Başlık** : müşteriler bunu teklifinizin ayrıntılar sayfasında görür.
- **Bağlantı (URL)** – müşterilerin çevrimiçi belgenizi görüntülemesi için bir bağlantı girin. Bağlantı, http://veya https://ile başlamalıdır.

### <a name="contact-information"></a>İletişim Bilgileri

Bir **destek kişisi** ve **mühendislik ilgili kişisi**için ad, e-posta ve telefon numarası sağlamanız gerekir. Bu bilgiler müşterilere gösterilmez, ancak Microsoft tarafından kullanılabilir. Ayrıca, bulut çözümü sağlayıcısı (CSP) iş ortakları için de kullanılabilir.

- Destek kişisi (gerekli): genel destek soruları Için.
- Mühendislik ilgili kişisi (gerekli): Teknik sorular ve sertifika sorunları Için.
- CSP program kişisi (isteğe bağlı): CSP programıyla ilgili satıcı soruları Için.

**Destek kişisi** bölümünde, ortağın küresel Azure 'Da, Azure Kamu 'da veya her ikisinde de kullanılabilir olup olmadığına bağlı olarak, teklif için iş ortaklarının destek bulabileceği **Destek Web sitesini** sağlayın.

**CSP program kişisi** bölümünde, CSP iş ortaklarının teklifiniz için pazarlama malzemeleri bulabileceği bağlantıyı ( **CSP program pazarlama malzemeleri** ) sağlayın.

#### <a name="additional-marketplace-listing-resources"></a>Ek Market listeleme kaynakları

Teklif listeleri oluşturma hakkında daha fazla bilgi için bkz. [teklif listesi en iyi uygulamaları](../gtm-offer-listing-best-practices.md)

### <a name="marketplace-images"></a>Market görüntüleri

Teklifinizle birlikte kullanılacak logo ve görüntüler sağlayın. Tüm görüntülerin PNG biçiminde olması gerekir. Bulanık görüntüler reddedilir.

[!INCLUDE [logotips](../includes/graphics-suggestions.md)]

>[!Note]
>Dosya yükleme sorununuz varsa, yerel ağınızın https://upload.xboxlive.com Iş Ortağı Merkezi tarafından kullanılan hizmeti engellemediğinden emin olun.

#### <a name="store-logos"></a>Mağaza logoları

 Aşağıdaki dört piksel boyutundan her biri için teklifinizin logosunun PNG dosyalarını sağlayın:

- **Küçük** (48 x 48)
- **Orta** (90 X 90)
- **Büyük** (216 x 216)
- **Geniş** (255 X 115)

Dört logo gereklidir ve Market listesindeki farklı yerlerde kullanılır.

[!INCLUDE [logos-azure-marketplace-only](../includes/logos-azure-marketplace-only.md)]

#### <a name="screenshots-optional"></a>Ekran görüntüleri (isteğe bağlı)

Teklifinizin nasıl çalıştığını gösteren en fazla beş ekran görüntüsü ekleyin. Her biri, boyut ve PNG biçiminde 1280 x 720 piksel olmalıdır.

#### <a name="videos-optional"></a>Videolar (isteğe bağlı)

Teklifinizi gösteren en fazla beş video ekleyin. Videonun adını, Web adresini ve videonun küçük resim PNG görüntüsünü 1280 x 720 piksel boyutunda girin.

#### <a name="offer-examples"></a>Teklif örnekleri

Aşağıdaki örneklerde teklif listeleme alanlarının teklifin farklı konumlarında nasıl göründüğü gösterilmektedir.

Bu, Azure Marketi 'ndeki **teklif listesi** sayfasını gösterir:

:::image type="content" source="media/azure-create-container-offer-images/azure-create-6-offer-listing-mkt-plc.png" alt-text="Azure Marketi 'ndeki teklif listesi sayfasını gösterir." :::

Bu, Azure Marketi 'nde arama sonuçlarını gösterir:

:::image type="content" source="media/azure-create-container-offer-images/azure-create-7-search-results-mkt-plc.png" alt-text="Azure Marketi 'ndeki arama sonuçlarını gösterir.":::

Bu, Azure portal **teklif listesi** sayfasını gösterir:

:::image type="content" source="media/azure-create-container-offer-images/azure-create-8-offer-listing-portal.png" alt-text="Azure portal teklif listesi sayfasını gösterir.":::

Bu, arama sonuçlarını Azure portal gösterir:

:::image type="content" source="media/azure-create-container-offer-images/azure-create-9-search-results-portal.png" alt-text="Azure portal arama sonuçlarını gösterir.":::

## <a name="preview"></a>Önizleme

Önizleme sekmesinde, canlı yayımlamadan önce teklifinizi doğrulamak için sınırlı bir **Önizleme hedef kitlesi** seçebilirsiniz.

> [!IMPORTANT]
> Teklifinizi **önizlemede**güncelleştirdikten sonra teklifinizi herkese açık bir şekilde yayımlamak Için **canlı git** ' i seçmeniz gerekir.

Azure abonelik KIMLIĞI GUID 'Lerini kullanarak önizleme Dinleyicilerinizi, her biri için isteğe bağlı bir açıklama ile birlikte belirtin. Bu alanlardan hiçbiri müşteriler tarafından görülenebilir.

> [!NOTE]
> Azure abonelik KIMLIĞINIZI Azure portal içindeki abonelikler sayfasında bulabilirsiniz.

Tek tek (10 ' a kadar) veya bir CSV dosyası karşıya yükleyerek (100 'e kadar) en az bir Azure abonelik KIMLIĞI ekleyin. Bu abonelik kimliklerini ekleyerek teklifinizin canlı olarak yayınlanmadan önce ne kadar önizleyeceğini belirlersiniz. Teklifiniz zaten canlı ise, teklifinizdeki değişiklikleri veya güncelleştirmeleri test etmek için bir önizleme izleyiciyi seçebilirsiniz.

Devam etmeden önce **Taslağı kaydet** ' i seçin.

## <a name="plan-overview"></a>Plana genel bakış

Bu sekme, aynı teklif içinde farklı plan seçenekleri sağlamanıza olanak tanır. Planlar (eskiden SKU olarak adlandırılır), genel bulutlar, kamu bulutları ve plan tarafından başvurulan görüntü gibi hangi bulutların kullanılabildiği konusunda farklılık gösterebilir. Teklifinizi ticari Market 'te listelemek için en az bir plan ayarlamanız gerekir.

Her teklif için en fazla 100 plan oluşturabilirsiniz: bunların en fazla 45 ' e kadar özel olabilir. [Microsoft ticari Market 'Teki özel tekliflerle](../private-offers.md)ilgili özel planlar hakkında daha fazla bilgi edinin.

Planlarınızı oluşturduktan sonra **plana genel bakış** sekmesi şunları gösterir:

- Plan adları
- Fiyatlandırma modeli
- Azure bölgeleri (küresel veya kamu)
- Geçerli yayımlama durumu
- Kullanılabilir eylemler

Plana genel bakışta sunulan eylemler planınızın geçerli durumuna bağlı olarak farklılık gösterir. Bu ölçümler şunlardır:

- **Taslağı Sil** – plan durumu bir tasladır.
- **Satış planını durdur** – plan durumu canlı olarak yayımlandı.

### <a name="create-new-plan"></a>Yeni plan oluştur

**Yeni plan oluştur**' u seçin. **Yeni plan** iletişim kutusu görüntülenir.

**Plan kimliği** kutusunda, bu teklifteki her plan için benzersiz bir plan tanımlayıcısı oluşturun. Bu KIMLIK, ürünün Web adresindeki müşteriler tarafından görülebilir. Yalnızca küçük harf ve rakam, tire veya alt çizgi ve en fazla 50 karakter kullanın.

> [!NOTE]
> Plan KIMLIĞI, **Oluştur**' u seçtikten sonra değiştirilemez.

**Plan adı** kutusuna bu plan için bir ad girin. Müşteriler, teklifiniz dahilinde hangi planın seçeceğinize karar verirken bu adı görür. Bu teklifte her plan için benzersiz bir ad oluşturun. Örneğin, Windows Server **2016** ve **Windows Server 2019**planlarına **sahip bir** teklif adı kullanabilirsiniz.

### <a name="plan-setup"></a>Planı ayarla

Bu sekme, planın kullanılabildiği bulutu seçmenize olanak sağlar. Bu sekmedeki yanıtlarınız, diğer sekmelerde hangi alanların görüntülendiğini etkiler.

#### <a name="azure-regions"></a>Azure bölgeleri

Azure Container teklifleri için tüm planlar otomatik olarak **Azure genel**'de kullanıma sunulmuştur.  Planınız, ticari Market kullanan tüm genel Azure bölgelerinde bulunan müşteriler tarafından kullanılabilir. Ayrıntılar için bkz. [coğrafi kullanılabilirlik ve para birimi desteği](../marketplace-geo-availability-currencies.md).

Çözümünüzü burada görünmesini sağlamak için [Azure Kamu](../../azure-government/documentation-government-welcome.md) seçeneğini belirleyin. Bu, ABD Federal, eyalet ve yerel ya da üç devlet kurumlarının yanı sıra onlara hizmeti sunmaya uygun iş ortakları için denetimli erişime sahip bir kamu topluluk bulutuna sahiptir. Yayımcı olarak, bu bulut topluluğu için tüm uyumluluk denetimleri, güvenlik ölçüleri ve en iyi uygulamalardan siz sorumlusunuz. Azure Kamu fiziksel olarak yalıtılmış veri merkezleri ve ağlar (yalnızca ABD 'de bulunur) kullanır. Azure Kamu 'da [yayımlamadan](../../azure-government/documentation-government-manage-marketplace-partners.md) önce, sonuçlar farklı olabileceğinden çözümünüzü test edin ve onaylayın. Çözümünüzü oluşturmak ve test etmek için [Microsoft Azure Kamu deneme](https://azure.microsoft.com/global-infrastructure/government/request/)sürümünden bir deneme hesabı isteyin.

> [!NOTE]
> Planınız yayımlandıktan ve belirli bir bölgede kullanılabilir olduktan sonra bu bölgeyi kaldıramazsınız.

#### <a name="azure-government-certifications"></a>Azure Kamu sertifikaları

Bu seçenek, yalnızca Azure **kamu** , **Azure bölgeleri**altında seçilirse görülebilir.

Azure Kamu Hizmetleri, belirli kamu düzenlemelerine ve gereksinimlerine tabi olan verileri işler. Örneğin, Fedrampa, NıST 800,171 (DIB), ıTAR, ıRS 1075, DoD L4 ve CJıS.

Bu programlara yönelik sertifikalarınızı göstermek için, bunları tanımlayan 100 'e kadar bağlantı sağlayabilirsiniz. Bunlar program üzerinde doğrudan veya kendi web sitenizde yer alan dökümlerinizin bağlantısı olabilir. Bu bağlantılar yalnızca Azure Kamu müşterileri tarafından görülebilir.

### <a name="plan-listing"></a>Planı listeleme

Bu sekme, geçerli teklifin içindeki her farklı plan için belirli bilgileri görüntüler.

### <a name="plan-name"></a>Plan adı

Bu, planınızı oluşturduğunuz sırada verdiğiniz adla önceden doldurulmuştur. Bu adı gerektiği gibi değiştirebilirsiniz. En fazla 50 karakter uzunluğunda olabilir. Bu ad, Azure Marketi 'nde bu planın başlığı olarak görünür ve Azure portal. Plan kullanılmak üzere hazır olduktan sonra varsayılan modül adı olarak kullanılır.

### <a name="plan-summary"></a>Plan Özeti

Yazılım planınızın kısa bir özeti (teklif değildir). Bu Özet, Azure Marketi arama sonuçlarında görünür ve en fazla 100 karakter içerebilir.

### <a name="plan-description"></a>Plan açıklaması

Bu yazılım planının ne olduğunu ve teklifinizdeki planlar arasındaki farkları betimleyebilirsiniz. Teklifi değil, yalnızca plan. Bu açıklama, Azure Marketi 'nde ve **teklif listesi** sayfasındaki Azure Portal görüntülenir. Plan özetinde girdiğiniz içerik aynı olabilir ve en fazla 2.000 karakter içerebilir.

Bu alanları tamamladıktan sonra **Kaydet** ' i seçin.

#### <a name="plan-examples"></a>Plan örnekleri

Aşağıdaki örneklerde plan listeleme alanlarının farklı görünümlerde nasıl göründüğü gösterilmektedir.

Plan ayrıntılarını görüntülerken Azure Marketi 'ndeki alanlar şunlardır:

:::image type="content" source="media/azure-create-container-offer-images/azure-create-10-plan-details-mtplc.png" alt-text="Azure Marketi 'nde plan ayrıntılarını görüntülerken gördüğünüz alanları gösterir.":::

Bunlar Azure portal plan ayrıntılardır:

:::image type="content" source="media/azure-create-container-offer-images/azure-create-11-plan-details-portal.png" alt-text="Azure portal plan ayrıntılarını gösterir.":::

### <a name="plan-availability"></a>Plan kullanılabilirliği

Müşterilerin Market 'te arayabilmesi, gözatmasını veya satın almasını sağlamak için yayınlanmış teklifinizi gizlemek istiyorsanız, **kullanılabilirlik** sekmesinde **planı Gizle** onay kutusunu seçin.

Bu alan şu durumlarda kullanılır:

- Teklif, başka bir uygulama üzerinden başvuruluyorsa dolaylı olarak kullanılmak üzere tasarlanmıştır.
- Teklif ayrı olarak satın alınmamalıdır.
- Plan ilk test için kullanıldı ve artık ilgili değil.
- Plan, geçici veya dönemsel teklifler için kullanıldı ve artık sunulmamalıdır.

## <a name="technical-configuration"></a>Teknik yapılandırma

Kapsayıcı görüntülerinin özel bir [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)barındırılması gerekir. **Teknik yapılandırma** sekmesinde, Azure Container Registry içindeki kapsayıcı görüntüsü deponuzun başvuru bilgilerini girin.

Teklif yayımlandıktan sonra, kapsayıcı görüntünüz belirli bir ortak kapsayıcı kayıt defterinde Azure Marketi 'ne kopyalanır. Kapsayıcı görüntünüzü kullanmaya yönelik tüm istekler, özel bir hizmet değil, Azure Marketi ortak kapsayıcı kayıt defterinden sunulur. Ayrıntılar için bkz. [Azure Container teknik varlıklarınızı hazırlama](create-azure-container-technical-assets.md).

### <a name="image-repository-details"></a>Görüntü deposu ayrıntıları

**Görüntü deposu ayrıntıları** sekmesinde aşağıdaki bilgileri sağlayın.

**Azure ABONELIK kimliği** – kullanım rapor edildiği ve kapsayıcı görüntünüzü içeren Azure Container Registry için hizmetlerin FATURALANDıRıLAN abonelik kimliğini belirtin. Bu KIMLIĞI, Azure portal [abonelikler sayfasında](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) bulabilirsiniz.

**Azure Kaynak grubu adı** – kapsayıcı yansımanıza Azure Container Registry içeren [kaynak grubu](../../azure-resource-manager/management/manage-resource-groups-portal.md) adını belirtin. Kaynak grubuna abonelik KIMLIĞINDE erişilebilir olması gerekir (yukarıda). Adı Azure portal [kaynak grupları](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceGroups) sayfasında bulabilirsiniz.

**Azure Container Registry Name** : kapsayıcı yansımanıza sahip [Azure Container Registry](../../container-registry/container-registry-intro.md) adını belirtin. Kapsayıcı kayıt defteri, daha önce belirttiğiniz Azure Kaynak grubunda olmalıdır. Tam oturum açma sunucusu adını değil, yalnızca kayıt defteri adını dahil edin. Adından **azurecr.io** attığınızdan emin olun. Kayıt defteri adını Azure portal [kapsayıcı kayıt defterleri sayfasında](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.ContainerRegistry%2Fregistries) bulabilirsiniz.

**Azure Container Registry Için Yönetici Kullanıcı adı** -kapsayıcı yansımanıza sahip Azure Container Registry bağlantılı [Yönetici Kullanıcı adı](../../container-registry/container-registry-authentication.md#admin-account)belirtin. Şirketinizin kayıt defterine erişiminin olduğundan emin olmak için Kullanıcı adı ve parola gereklidir. Yönetici Kullanıcı adını ve parolasını almak için, Azure komut satırı arabirimi 'ni (CLı) kullanarak **yönetici özellikli** özelliği **true** olarak ayarlayın. **Yönetici kullanıcıyı** Azure Portal ' de **etkinleştirmek** üzere isteğe bağlı olarak ayarlayabilirsiniz.

 :::image type="content" source="media/azure-create-container-offer-images/azure-create-12-update-container-registry-edit.png" alt-text="Kapsayıcı kayıt defterini Güncelleştir iletişim kutusunu gösterir.":::

**Azure Container Registry parolası** – Azure Container Registry ilişkili Yönetici Kullanıcı adı için parola belirtin ve kapsayıcı görüntünüzü içerir. Şirketinizin kayıt defterine erişiminin olduğundan emin olmak için Kullanıcı adı ve parola gereklidir. Azure Portal parolayı **Container Registry**  >  **erişim anahtarlarına** giderek veya [göster komutunu](https://docs.microsoft.com/cli/azure/acr/credential?view=azure-cli-latest#az-acr-credential-show)kullanarak Azure CLI ile öğrenebilirsiniz.

:::image type="content" source="media/azure-create-container-offer-images/azure-create-13-access-keys.png" alt-text="Erişim tuşu menüsünü gösterir.":::

**Azure Container Registry Içinde Depo adı**. Yansımanıza sahip Azure Container Registry deposunun adını sağlayın. Görüntüyü kayıt defterine gönderdiğinizde deponun adını ekleyin. Deponun adını [Container Registry](https://azure.microsoft.com/services/container-registry/)  >  **depoları** sayfasına giderek bulabilirsiniz. Daha fazla bilgi için bkz. [Azure Portal kapsayıcı kayıt defteri depolarını Görüntüleme](../../container-registry/container-registry-repositories.md).

> [!NOTE]
> Ad ayarlandıktan sonra değiştirilemez. Hesabınızdaki her teklif için benzersiz bir ad kullanın.

### <a name="image-tags-for-new-versions-of-your-offer"></a>Teklifinizin yeni sürümleri için görüntü etiketleri

Bir güncelleştirme yayımladığınızda müşterilerin Azure Marketi 'nden güncelleştirmeleri otomatik olarak alabilmesi gerekir. Güncelleştirmek istemlerse, görüntünüzün belirli bir sürümünde kalabilmeleri gerekir. Görüntüde her güncelleştirme yaptığınızda bunu yeni resim etiketleri ekleyerek yapabilirsiniz.

### <a name="image-tag"></a>Resim etiketi

Bu alan, tüm desteklenen platformlarda görüntünüzün en son sürümüne işaret eden **en son** bir etiketi içermelidir. Ayrıca, sürüm etiketi de içermelidir (örneğin, xx. xx. xx ile başlayarak xx bir sayıdır). Müşteriler birden çok platformu hedeflemek için [bildirim etiketlerini](https://github.com/estesp/manifest-tool) kullanmalıdır. Bir bildirim etiketi tarafından başvurulan tüm Etiketler de karşıya yüklenebilmemiz için eklenmelidir.

Tüm bildirim etiketleri (en son etiket hariç), X. Y **-** veya x. y. z ile başlamalı; burada x, y ve z tamsayılardır. Örneğin, **en son** bir etiket 1.0.1-Linux-x64, 1.0.1-Linux-ARM32 ve 1.0.1-Windows-ARM32 ' y i gösteriyorsa, bu altı etiket bu alana eklenmelidir. Ayrıntılar için bkz. [Azure Container teknik varlıklarınızı hazırlama](create-azure-container-technical-assets.md).

> [!NOTE]
> Test sırasında görüntüyü tanımlayabilmeniz için yansımanıza bir sınama etiketi eklemeyi unutmayın.

## <a name="review-and-publish"></a>İnceleme ve yayımlama

Teklifin tüm gerekli bölümlerini tamamladıktan sonra, bunu gözden geçirmek ve yayımlamak için gönderebilirsiniz.

Portalın sağ üst köşesinde, **gözden geçir ve** **Yayımla**' yı seçin.

İnceleme sayfasında şunları yapabilirsiniz:

- Teklifin her bölümü için tamamlanma durumuna bakın. Teklifin tüm bölümleri Tamam olarak işaretlenene kadar yayımlayamazsınız.
  - **Başlatılmamış** – başlatılmamış ve tamamlanması gerekiyor.
  - **Tamamlanmamış** – düzeltilmesi gereken hatalar var veya daha fazla bilgi sağlamanızı gerektirir. Yardım için bu belgenin önceki bölümlerine bakın.
  - **Tamamlanmış** – hatasız tüm gerekli verileri içerir. Teklifi gönderebilmeniz için teklifin tüm bölümlerinin tamamlanmış olması gerekir.
- Teklifinizin doğru şekilde test edilmesini sağlamak için sertifika ekibine test yönergeleri sağlayın. Ayrıca, teklifinizi anlamak için yararlı olan tüm ek notları sağlayın.

Teklifi yayımlamaya göndermek için **Yayımla**' yı seçin.

Teklifin bir önizleme sürümünün gözden geçirilmesi ve onaylanmak üzere kullanılabilir olduğunu size bildirmek için size bir e-posta göndereceğiz.

Teklifinizi herkese açık bir şekilde yayımlamak için Iş Ortağı Merkezi 'ne gidin ve **Git-Live**' ı seçin.

## <a name="next-step"></a>Sonraki adım

- [Ticari Market 'te mevcut bir teklifi güncelleştirme](update-existing-offer.md)
