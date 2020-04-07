---
title: İş Ortağı Merkezi ile Azure IoT Edge modülü teklifi oluşturun - Azure Marketi
description: İş Ortağı Merkezi'ni kullanarak Azure Market'te Nasıl IoT Edge modülü teklifi oluşturabilirsiniz öğrenin
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/03/2020
ms.openlocfilehash: 6b56b748ef31bcfd33893e55d3ea5f8d9851a3ff
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80674481"
---
# <a name="create-an-iot-edge-module-offer"></a>IoT Edge modülü teklifi oluşturma

> [!IMPORTANT]
> IoT Edge modül tekliflerinizi Bulut İş Ortağı Portalı'ndan İş Ortağı Merkezi'ne taşıyoruz. Tekliflerin izin verilene kadar, lütfen IoT Edge modülündeki talimatları izleyin, tekliflerinizi yönetmek için Bulut İş Ortağı Portalı için [genel yayın alameti sunuyor.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/iot-edge-module/cpp-offer-process-parts)

Bu makalede, Azure Marketi için Nesnelerin İnterneti (IoT) Edge modülü teklifinin nasıl oluşturulup yayımlandırılabildiğini açıklanmaktadır.

Bir IoT Edge modülü teklifi oluşturmadan önce, İş Ortağı Merkezi'nde ticari bir pazar hesabınız olmalıdır. Henüz bir tane oluşturmadıysanız, [bkz.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account)

## <a name="create-a-new-offer"></a>Yeni teklif oluşturma

1. İş Ortağı Merkezi'nde oturum açın.
2. Soldaki gezinme menüsünde **Ticari Pazara** > **Genel Bakış'ı**seçin.

    ![Sol navigasyon menüsünü gösterir.](./media/cs-menu-overview.png)

3. Select **+ Yeni teklif** > **IoT Edge modülü.** **Yeni teklif** iletişim kutusu görüntülenir.

> [!IMPORTANT]
> Bir teklif yayımlandıktan sonra, İş Ortağı Merkezi'nde yapılan yapılan lar yalnızca teklifi yeniden yayımladıktan sonra vitrinlerde görünür. Değişiklik yaptıktan sonra her zaman yeniden yayımladığınızdan emin olun.

### <a name="offer-id-and-alias"></a>Teklif kimliği ve takma ad

Teklif **Kimliği**girin. Bu, hesabınızdaki her teklif için benzersiz bir tanımlayıcıdır.

- Bu kimlik, pazar teklifi ve varsa Azure Kaynak Yöneticisi şablonlarının web adresindeki müşteriler tarafından görülebilir.
- Yalnızca küçük harfleri ve rakamları kullanın. Tireler ve alt çizerler içerebilir, ancak boşluk yoktur ve 50 karakterle sınırlıdır. Örneğin, **test-teklif-1**girerseniz, teklif web adresi `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
- Teklif Kimliği, Oluştur'u seçtikten sonra değiştirilemez.

Teklif **takma adını**girin. Bu, Ortak Merkezi'ndeki teklifi ifade etmek için kullanılan addır.

- Bu ad pazarda kullanılmaz ve teklif adı ve müşterilere gösterilen diğer değerlerden farklıdır.
- **Oluştur'u**seçtikten sonra bu değişiklik edilemez.

Bu iki değeri girdikten sonra, bir sonraki sayfaya devam etmeden önce **Oluştur'u** seçin, Teklif genel bakış.

## <a name="offer-overview"></a>Teklife genel bakış

**Teklife genel bakış** sayfası, bu teklifi yayımlamak için gereken adımların (hem tamamlanmış hem de yaklaşan) ve her adımın tamamlanmasının ne kadar sürmesi gerektiğinin görsel bir gösterimini gösterir.

Bu sayfa, yaptığınız seçime dayalı olarak bu teklifle ilgili işlemleri gerçekleştirmek için bağlantılar içerir. Örnek:

- Teklif taslaksa - [Taslak teklifi sil](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#delete-a-draft-offer)
- Teklif canlıise - [Teklifi satmayı bırakın](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#stop-selling-an-offer-or-plan)
- Teklif önizlemedeyse - [Canlı Yayına Geçin](https://docs.microsoft.com/azure/marketplace/partner-center-portal/publishing-status#publisher-approval)
- Yayımcı oturum açma yı tamamlamadıysanız - [Yayımlamayı iptal edin.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#cancel-publishing)

## <a name="offer-setup"></a>Teklif kurulumu

Teklifinizi ayarlamak için aşağıdaki adımları izleyin.

### <a name="connect-lead-management"></a>Müşteri adayı yönetimini bağlayın

Teklifinizi İş Ortağı Merkezi ile pazara yayınlarken, isteğe bağlı olarak Müşteri İlişkileri Yönetimi (CRM) sisteminize bağlayabilirsiniz. Bu, birisi ürününüzün ilgisini ifade eder etmez veya ürününüzü kullanır kullanmaz müşteri iletişim bilgilerini almanızı sağlar.

1. Potansiyel müşterileri göndermemizi istediğiniz müşteri adayı hedefini seçin. İş Ortağı Merkezi aşağıdaki CRM sistemlerini destekler:

    - Müşteri Katılımı için [Dynamics 365](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics)
    - [Marketo](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-marketo)
    - [Salesforce](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce)

    > [!NOTE]
    > CRM sisteminiz yukarıda listelenmemişse, müşteri müşteri adayı verilerini depolamak için [Azure Tablosu](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table) veya [Https Endpoint'i](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-https) kullanın ve verileri CRM sisteminize aktarın.

2. İş Ortağı Merkezi'nde yayın yaparken teklifinizi müşteri adayı hedefe bağlayın.
3. Müşteri adayı hedefine bağlantının düzgün şekilde yapılandırıldığından onaylayın. İş Ortağı Merkezi'nde yayımladıktan sonra bağlantıyı doğrular ve size bir test ipucu göndeririz. Teklifi yayına girmeden önce önizlemenize kadar, teklifi önizleme ortamında kendiniz satın almaya çalışarak müşteri adayı bağlantınızı da test edebilirsiniz.
4. Müşteri adayını kaybetmemek için müşteri adayı hedefine bağlantının güncel kaldığından emin olun.

Aşağıda bazı ek müşteri adayı yönetim kaynakları vereme

- [Müşteri adayı yönetimine genel bakış](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-get-customer-leads)
- [Müşteri adayı yönetimi hakkında SSS](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [Sık karşılaşılan müşteri adayı yapılandırma hataları](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#common-lead-configuration-errors-during-publishing-on-cloud-partner-portal)
- [Müşteri Adayı Yönetimine Genel Bakış](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf) PDF (Açılır pencere engelleyicinizin kapalı olduğundan emin olun).

Özellikler adlı bir sonraki bölüme devam etmeden önce **taslağı kaydet'i** seçin.

### <a name="properties"></a>Özellikler

Bu sayfa, teklifinizi pazarda gruplandırmak için kullanılan kategorileri ve teklifinizi destekleyen yasal sözleşmeleri tanımlamanızı sağlar.

#### <a name="category"></a>Kategori

En az bir ve en fazla beş kategori seçin. Bu kategoriler, teklifinizi uygun pazar yeri arama alanlarına yerleştirmek için kullanılır ve teklif ayrıntıları sayfanızda gösterilir. Teklif açıklamasında, teklifinizin bu kategorileri nasıl desteklediğini açıklayın. Gözatma sayfalarında, tüm IoT Edge modülleri **Nesnelerin İnterneti > IoT Edge modülü** kategorisi altında gösterilir.

#### <a name="legal"></a>Yasal Bildirim

Teklif için şart ve koşulları sağlamalısınız. İki seçeneğiniz vardır:

- Microsoft Ticari Pazar Yeri için Standart Sözleşme'yi kullanın.
- Kendi şart ve koşullarınızı sağlayın.

##### <a name="standard-contract-for-the-microsoft-commercial-marketplace"></a>Microsoft Ticari Pazar Yeri için standart sözleşme

Ticari pazardaki işlemleri kolaylaştırmaya yardımcı olmak için standart sözleşme şablonu sunuyoruz. Çözümünüzü, müşterilerin yalnızca bir kez kontrol edip kabul etmesi gereken Standart Sözleşme kapsamında sunmayı seçebilirsiniz. Bu, özel şartlar ve koşullar zanaat istemiyorsanız iyi bir seçenektir.

Standart Sözleşme hakkında daha fazla bilgi edinmek [için Microsoft Ticari Pazarı için Standart Sözleşme'ye](https://docs.microsoft.com/azure/marketplace/standard-contract)bakın. [Standart Sözleşme](https://go.microsoft.com/fwlink/?linkid=2041178) PDF'sini de indirebilirsiniz (açılır pencere engelleyicinizin kapalı olduğundan emin olun).

Standart Sözleşme'yi kullanmak **için, Microsoft'un ticari pazar yeri** onay kutusunu için Standart Sözleşmeyi Kullan'ı seçin ve sonra **Kabul Et'i**tıklatın.

> [!NOTE]
> Microsoft ticari pazarı için Standart sözleşmeyi kullanarak bir teklif yayımladıktan sonra, kendi özel hüküm ve koşullarınızı kullanamazsınız. Çözümünüzü Standart Sözleşme kapsamında veya kendi hüküm ve koşullarınız altında sunun.

![Microsoft'un ticari pazar onay kutusu için Standart Sözleşme'yi kullanarak gösteriş.](./media/iot-edge-module-creation/iot-edge-module-standard-contract-checkbox.png)

##### <a name="your-own-terms-and-conditions"></a>Kendi hüküm ve koşullarınız

Kendi özel hüküm ve koşullarınızı sağlamak **için, bunları Şartlar ve Koşullar** kutusuna girin. Bu kutuya sınırsız miktarda metin karakteri girebilirsiniz. Müşteriler teklifinizi denemeden önce bu koşulları kabul etmelidir.

Bir sonraki bölüme, Teklif listesine devam etmeden önce **taslağı Kaydet'i** seçin.

## <a name="offer-listing"></a>Teklif listesi

Burada pazarda görüntülenen teklif ayrıntılarını tanımlarsınız. Bu teklif adı, açıklama, görüntüler ve benzeri içerir. Bu teklifi yapılandırırken Microsoft'un ilke sayfasında ayrıntılı olarak belirtilen ilkeleri uyguladığından emin olun.

> [!NOTE]
> Teklif açıklaması "Bu uygulama yalnızca [İngilizce olmayan dilde] şeklinde başlıyorsa, teklif ayrıntılarının İngilizce olması gerekmez." Teklif listesi ayrıntılarında kullanılandan farklı bir dilde içerik sunmak için yararlı bir bağlantı sağlamak da sorun değildir.

### <a name="name"></a>Adı

Buraya girdiğiniz ad, teklifinizin başlığı olarak görüntülenir. Bu alan, teklifi oluşturduğunuzda Teklif **takma adı** kutusuna girdiğiniz metinle önceden doldurulmuştur. Bu adı daha sonra değiştirebilirsiniz.

Adı:

- Ticari marka olabilir (ve ticari marka veya telif hakkı sembolleri dahil edebilirsiniz).
- 50 karakterden uzun olamaz.
- Emojileri içeremiyorum.

### <a name="search-results-summary"></a>Arama sonuçları özeti

Teklifinizin kısa bir açıklamasını sağlayın. Bu, 100 karaktere kadar uzun olabilir ve pazar yeri arama sonuçlarında kullanılır.

### <a name="long-summary"></a>Uzun özet

Teklifinizin daha ayrıntılı bir açıklamasını sağlayın. Bu, 256 karakter uzunluğunda olabilir ve pazar yeri arama sonuçlarında kullanılır.

### <a name="description"></a>Açıklama

Teklifinizin 3.000 karaktere kadar daha uzun bir açıklamasını sağlayın. Bu, pazar yeri giriş özetinde müşterilere görüntülenir.

Açıklamanıza aşağıdakilerden birini veya birkaçını ekleyin:

- Teklifinizin sağladığı değer ve önemli avantajlar
- Kategori veya endüstri dernekleri veya her ikisi
- Uygulama içi satın alma fırsatları
- Gerekli açıklamalar

IoT Edge modülü teklifleri, açıklamanın altındaki minimum donanım gereksinimleri paragrafını içermelidir. Örnek:

*Minimum donanım gereksinimleri: Linux x64 ve arm32 işletim sistemi, 1 GB RAM, 500 Mb depolama*

Açıklamanızı yazmak için bazı ipuçları aşağıda verilmiştir:

- Teklifinizin değerini açıklamanızın ilk birkaç cümlesinde açıkça açıklayın. Aşağıdaki öğeleri ekleyin:
    - Teklifin açıklaması.
    - Tekliften yararlanan kullanıcı türü.
    - Müşteri ihtiyaçları veya teklif adreslerini sorunları.
- İlk birkaç cümlenin arama sonuçlarında görüntülenebileceğini unutmayın.
- Ürününüzü satmak için özelliklere ve işlevselliğe güvenmeyin. Bunun yerine, teklifinizin sağladığı değere odaklanın.
- Sektöre özgü kelime dağarcığı veya fayda tabanlı ifadeler kullanmayı deneyin.

Teklifaçıklamanızı **Description** daha ilgi çekici hale getirmek için, açıklamanızı biçimlendirmek için zengin metin düzenleyicisini kullanın. Zengin metin düzenleyicisi, açıklamanızı daha okunabilir hale getirmek için sayılar, madde işaretleri, kalın, italik ler ve girintiler eklemenizi sağlar.

:::image type="content" source="media/text-editor2.png" alt-text="Zengin metin düzenleyicisini gösterir." border="false":::

- İçeriğinizin biçimini değiştirmek için, biçimlendirmek istediğiniz metni vurgulayın ve bu ekran görüntüsünde gösterildiği gibi bir metin stili seçin:

     :::image type="content" source="media/text-editor3.png" alt-text="Zengin metin düzenleyicisinde metin stili denetimini gösterir." border="false":::

- Metne madde işaretli veya numaralanmış bir liste eklemek için bu ekran görüntüsünde gösterilen seçenekleri kullanın:
  
    :::image type="content" source="media/text-editor4.png" alt-text="Zengin metin düzenleyicisinde madde işaretli ve numara listesi denetimlerini gösterir." border="false":::

- Metne girintiyi eklemek veya kaldırmak için bu ekran görüntüsünde gösterilen seçenekleri kullanın:

    :::image type="content" source="media/text-editor5.png" alt-text="Zengin metin düzenleyicisindeki girinti denetimlerini gösterir." border="false":::

#### <a name="privacy-policy-url"></a>Gizlilik ilkesi URL'si

Kuruluşunuzun gizlilik ilkesinin web adresini girin. Teklifinizin gizlilik yasalarına ve yönetmeliklerine uygun olmasını sağlamaksizin sorumlusunuz. Ayrıca web sitenizde geçerli bir gizlilik politikası yayınlamaksizin de sorumlusunuz.

#### <a name="useful-links"></a>Yararlı bağlantılar

Teklifiniz hakkında ek çevrimiçi belgeler sağlayın. En fazla 25 bağlantı ekleyebilirsiniz. Bağlantı eklemek için **+ Bağlantı ekle'yi** seçin ve ardından aşağıdaki alanları tamamlayın:

- **Başlık** - Müşteriler teklifinizin ayrıntılar sayfasında başlığı göreceksiniz.
- **Link (URL)** - Müşterilerin çevrimiçi belgenizi görüntülemesi için bir bağlantı girin. Bağlantı http:// veya https:// ile başlamalıdır.

 [Azure IoT aygıt kataloğundan](https://catalog.azureiotsolutions.com/)belgelerinize en az bir bağlantı ve uyumlu IoT Edge aygıtlarına bir bağlantı eklediğinizden emin olun.

### <a name="contact-information"></a>İletişim bilgileri

**Destek kişisi** ve Mühendislik kişisi için adı, e-postayı ve telefon numarasını sağlamanız **gerekir.** Bu bilgiler müşterilere gösterilmez. Microsoft tarafından kullanılabilir ve Bulut Çözüm Sağlayıcısı (CSP) iş ortaklarına sağlanabilir.

- Destek kişisi (gerekli): Genel destek soruları için.
- Mühendislik iletişimi (gerekli): Teknik sorular ve belgelendirme sorunları için.
- CSP Programı iletişim bilgileri (isteğe bağlı): CSP programıyla ilgili bayi soruları için.

Destek **iletişim** bölümünde, teklifin genel Azure, Azure Resmi veya her ikisinde de kullanılabilir olup olmadığına bağlı olarak iş ortaklarının teklifiniz için destek bulabilecekleri **Destek web sitesinin** web adresini sağlayın.

**CSP Programı iletişim** bölümünde, CSP iş ortaklarının teklifiniz için pazarlama malzemeleri bulabilecekleri bağlantıyı **(CSP Program Pazarlama Malzemeleri)** sağlayın.

#### <a name="additional-marketplace-listing-resources"></a>Ek pazar yeri listeleme kaynakları

Teklif listeleri oluşturma hakkında daha fazla bilgi edinmek için, [en iyi uygulamaları listele'ye](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)bakın.

### <a name="marketplace-images"></a>Pazar yeri görüntüleri

Teklifinizle birlikte kullanılacak logolar ve resimler sağlayın. Tüm görüntüler .png formatında olmalıdır. Bulanık görüntüler reddedilir.

#### <a name="store-logos"></a>Mağaza logoları

Aşağıdaki dört piksel boyutun her birinde teklifinizin logosunun .png dosyalarını sağlayın:

- **Küçük (48 x 48)**
- **Orta (90 x 90)**
- **Büyük (216 x 216)**
- **Geniş (255 x 115)**

Dört logo gereklidir ve pazar listesinde farklı yerlerde kullanılır.

#### <a name="screenshots-optional"></a>Ekran görüntüleri (isteğe bağlı)

Teklifinizin nasıl çalıştığını gösteren en fazla beş ekran görüntüsü ekleyin. Her biri 1280 x 720 piksel boyutunda ve .png formatında olmalıdır.

#### <a name="videos-optional"></a>Videolar (isteğe bağlı)

Teklifinizi gösteren en fazla beş video ekleyin. Videonun adını, web adresini ve videonun 1280 x 720 piksel boyutunda küçük resmi .png görüntüsünü girin.

#### <a name="offer-examples"></a>Teklif örnekleri

Aşağıdaki örnekler, teklif listeleme alanlarının teklifin farklı yerlerinde nasıl göründüğünü gösterir.

Bu ekran görüntüsü, Azure Marketi'ndeki **Teklif listeleme** sayfasını gösterir.

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-offer-listing-page.png" alt-text="Azure Marketi'ndeki Teklif giriş sayfasını gösterir.":::

Bu ekran görüntüsü Azure Marketi'ndeki arama sonuçlarını gösterir:

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-marketplace-search-results.png" alt-text="Azure Marketi'ndeki arama sonuçlarını gösterir.":::

Bu ekran görüntüsü, Azure portalındaki **Teklif listeleme** sayfasını gösterir.

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-listing-page-azure-portal.png" alt-text="Azure portalındaki Teklif giriş sayfasını gösterir.":::

Bu ekran görüntüsü, Azure portalındaki arama sonuçlarını gösterir.

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-azure-portal-search-results.png" alt-text="Azure portalındaki Teklif giriş sayfasını gösterir.":::

Bir sonraki bölüme geçmeden önce **taslak kaydet'i** seçin, Önizleme.

## <a name="preview"></a>Önizleme

Önizleme **sekmesinde,** teklifinizi daha geniş pazar kitlesine canlı olarak yayınlamadan önce doğrulamak için sınırlı bir **Önizleme Hedef Kitlesi** seçebilirsiniz.

> [!IMPORTANT]
> Teklifinizi Önizleme'de görüntüledikten sonra, teklifinizi herkese yayınlamak için **Canlı Yayına Gönder'i** seçmeniz gerekir.

Azure abonelik kimliği GUID'lerini ve her biri için isteğe bağlı bir açıklama kullanarak önizleme hedef kitlenizi belirtin. Bu alanların hiçbiri müşteriler tarafından görülemez.

> [!NOTE]
> Azure abonelik kimliğinizi Azure portalında Abonelikler sayfasında bulabilirsiniz.

Tek tek (10'a kadar) veya bir CSV dosyası yükleyerek (100'e kadar) en az bir Azure abonelik kimliği ekleyin. Bu abonelik eklerini ekleyerek, teklifinizi canlı olarak yayınlanmadan önce kimlerin önizgösterip izleyebileceğini tanımlarsınız. Teklifiniz zaten yayındaysa, teklifinizdeki değişiklikleri veya güncellemeleri test etmek için bir önizleme hedef kitlesi tanımlayabilirsiniz.

> [!NOTE]
> Önizleme kitlesi özel bir hedef kitleden farklıdır. Önizleme **preview** kitlesi, yalnızca **özel** bir hedef kitleye (Kullanılabilirlik sekmesinde ayarlanmış) yayınlanacak olanlar da dahil olmak üzere, tüm teklif planlarını pazarda yayınlanmadan önce görebilir ve onaylayabilir.

Bir sonraki bölüme devam etmeden önce **taslağı Kaydet'i** seçin, Genel Bakış Planı' nı planlayın.

### <a name="plan-overview"></a>Plana genel bakış

Bu sekme, İş Ortağı Merkezi'nde aynı teklif içinde farklı plan seçenekleri sunmanızı sağlar. Bu planlar, daha önce SK'ler veya stok tutma birimleri olarak anılırdı. Planlar, küresel bulutlar, Devlet bulutları ve planın atıfta bulunduğu görüntü gibi bulutların nelerin mevcut olduğu açısından farklılık görebilir. Teklifinizi pazarda listelemek için en az bir plan ayarlamanız gerekir.

Planlarınızı oluşturduktan sonra, **Plana genel bakış** sekmesi şunları gösterir:

- Plan adları
- Fiyatlandırma modeli
- Bulut kullanılabilirliği (Genel veya Devlet)
- Geçerli yayımlama durumu
- Kullanılabilir eylemler

Plan'a genel bakışta bulunan eylemler, planınızın geçerli durumuna bağlı olarak değişir. Şunları içerir:

- **Taslak Sil**: Plan durumu taslaksa.
- **Stop sell plan**: Plan durumu canlı olarak yayınlanırsa.

#### <a name="create-new-plan"></a>Yeni plan oluşturma

**Yeni plan Oluştur'u**seçin. **Yeni plan** iletişim kutusu görüntülenir.

Plan **Kimliği** kutusunda, bu teklifteki her plan için benzersiz bir plan kimliği oluşturun. Bu kimlik, ürün web adresindeki müşteriler tarafından görülebilir. Yalnızca küçük harfleri ve sayıları, tireleri veya alt çiziyi ve en fazla 50 karakter kullanın.

Plan **adı** kutusuna, bu plan için bir ad girin. Müşteriler, teklifiniz içinde hangi planı seçiniz gerekip tespin edindiğinde bu adı görür. Bu teklifteki her plan için benzersiz bir ad oluşturun. Örneğin, Windows Server 2016 ve **Windows Server 2019**planlarını içeren bir **teklif** adı Kullanabilirsiniz. **Windows Server 2016**

> [!NOTE]
> **Oluştur'u**seçtikten sonra plan kimliği değiştirilemez.

**Oluştur'u**seçin.

### <a name="plan-setup"></a>Kurulum planı

Bu sekme, planın hangi bulutlarda kullanılabildiği üzerinde yapılandırmanızı sağlar. Bu sekmedeki yanıtlarınız, diğer sekmelerde hangi alanların görüntüleneceğini etkiler.

#### <a name="cloud-availability"></a>Bulut kullanılabilirliği

Planınız Azure IoT Hub'ını kullanarak en az bir bulutta kullanılabilir olmalıdır.

Planınızın pazar yerini kullanan tüm küresel Azure bölgelerindeki müşteriler tarafından kullanılabilmesi için **Azure Global** seçeneğini seçin. Ayrıntılar için [Bkz. Coğrafi kullanılabilirlik ve para birimi desteği.](https://aka.ms/AzureGovCurrencies)

Çözümünüzün burada görünmesini sağlamak için [Azure Kamu Bulutu](https://aka.ms/WhatIsAzureGovernment) seçeneğini belirleyin. Bu, ABD federal, eyalet ve yerel veya kabile devlet kurumlarının yanı sıra onlara hizmet vermeye uygun iş ortaklarından müşteriler için kontrollü erişime sahip bir devlet topluluğu bulutudur. Yayımcı olarak, bu bulut topluluğu için tüm uyumluluk denetimlerinden, güvenlik önlemlerinden ve en iyi uygulamalardan siz sorumlusunuz. Azure Kamu fiziksel olarak yalıtılmış veri merkezlerini ve ağlarını kullanır (yalnızca ABD'de bulunur). Azure Kamu'da [yayımlamadan](https://aka.ms/azuregovpublish) önce, sonuçlar farklı olabileceğinden, bu alandaki çözümünüzü test edin ve onaylayın. Çözümünüzü aşamalı olarak düzenlemek ve test etmek için [Microsoft Azure Kamu deneme sürümünden](https://aka.ms/AzureGovernmentTrial)bir deneme hesabı isteyin.

> [!NOTE]
> Planınız yayınlandıktan ve belirli bir bulutta kullanılabilir hale sunulduktan sonra, bu bulutu kaldıramazsınız.

#### <a name="azure-government-cloud-certifications"></a>Azure Kamu Bulutu sertifikaları

Bu seçenek yalnızca **Azure Kamu Bulutu** **Bulut kullanılabilirliği**altında seçilirse görünür.

Azure Kamu hizmetleri, belirli devlet yönetmeliklerine ve gereksinimlerine tabi olan verileri işler. Örneğin, FedRAMP, NIST 800.171 (DIB), ITAR, IRS 1075, DoD L4 ve CJIS. Bu programlar için sertifikalarınıza farkındalık getirmek için, sertifikalarınızı açıklayan en fazla 100 bağlantı sağlayabilirsiniz. Bunlar, doğrudan programdaki girişlerinize veya kendi web sitenize bağlantılar olabilir. Bu bağlantılar yalnızca Azure Kamu müşterileri tarafından görülebilir.

## <a name="plan-listing"></a>Plan listesi

Bu sekme, aynı teklif içindeki her farklı plan için belirli bilgileri görüntüler.

### <a name="plan-name"></a>Plan adı

Bu, planı oluşturduğunuzda vermiş olduğunuz adla önceden doldurulmuştur. Gerektiğinde bu adı değiştirebilirsiniz. En fazla 50 karakter uzunluğunda olabilir. Bu ad, Azure Marketi ve Azure portalında bu planın başlığı olarak görünür. Plan kullanıma hazır olduktan sonra varsayılan modül adı olarak kullanılır.

### <a name="plan-summary"></a>Plan özeti

Planınızın kısa bir özetini sağlayın (teklifi değil). Bu özet Azure Marketi arama sonuçlarında görünür ve en fazla 100 karakter içerebilir.

### <a name="plan-description"></a>Plan açıklaması

Bu planı benzersiz kılan ın yanı sıra teklifinizdeki planlar arasındaki farkları açıklayın. Teklifi tarif etme, sadece planı. Bu açıklama Azure Marketi'nde ve Teklif giriş sayfasındaki Azure portalında görünür. Plan özetinde sağladığınız içeriğin aynısı olabilir ve en fazla 2.000 karakter içerebilir.

Bu alanları tamamladıktan sonra **taslağı Kaydet'i** seçin.

#### <a name="plan-examples"></a>Plan örnekleri

Aşağıdaki örnekler, plan listeleme alanlarının farklı görünümlerde nasıl göründüğünü gösterir.

Plan ayrıntılarını görüntülerken Azure Marketi'ndeki alanlar şunlardır:

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-marketplace-plan-details.png" alt-text="Azure Marketi'nde plan ayrıntılarını görüntülerken gördüğünüz alanları gösterir.":::

Azure portalındaki plan ayrıntıları şunlardır:

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-azure-portal-plan-details.png" alt-text="Azure portalındaki plan ayrıntılarını gösterir.":::

## <a name="availability"></a>Kullanılabilirlik

Müşterilerin pazarda arama yapamaması, göz atmaması veya satın almaması için yayımlanmış teklifinizi gizlemek istiyorsanız, Kullanılabilirlik sekmesindeki **Planı Gizle** onay kutusunu seçin.

Bu alan genellikle şu zaman kullanılır:

- Teklif, başka bir uygulama yla başvurulduğunda yalnızca dolaylı olarak kullanılmak üzere tasarlanmıştır.
- Teklif tek tek satın alınmamalıdır.
- Plan ilk test için kullanıldı ve artık alakalı değil.
- Plan geçici veya mevsimlik teklifler için kullanıldı ve artık sunulmamalıdır.

## <a name="technical-configuration"></a>Teknik yapılandırma

**IoT Edge modül** teklif türü, Bir IoT Edge aygıtında çalışan belirli bir kapsayıcı türüdür. Teknik **Yapılandırma** sekmesinde, [Azure Kapsayıcı Kayıt Defteri'ndeki](https://aka.ms/ContainerRegistry)kapsayıcı resim deponuz için referans bilgilerinin yanı sıra müşterilerin modülü kolayca kullanmasına izin veren yapılandırma ayarları sağlarsınız.

Teklif yayımlandıktan sonra, IoT Edge kapsayıcı resminiz belirli bir genel kapsayıcı kayıt defterinde Azure Marketi'ne kopyalanır. Azure kullanıcılarından modülünüzü kullanmaları için gelen tüm istekler, özel konteyner kayıt defterinizden değil, Azure Marketi genel konteyner kayıt defterinden sunulur.

Etiketleri kullanarak birden çok platformu hedefleyebilir ve modül kapsayıcı resminizin çeşitli sürümlerini sağlayabilirsiniz. Etiketler ve sürümler hakkında daha fazla bilgi edinmek için [bkz.](https://aka.ms/AzureIoTTechAsset)

### <a name="image-repository-details"></a>Resim deposu ayrıntıları

**Resim deposu ayrıntıları** sekmesinde aşağıdaki bilgileri sağlarsınız.

**Resim kaynağını seçin**: **Azure Kapsayıcı Kayıt Defteri** seçeneğini seçin.

**Azure abonelik kimliği**: Kaynak kullanımının raporlandığı ve hizmetlerin kapsayıcı resminizi içeren Azure Kapsayıcı Kayıt Defteri için faturalandırıldığı abonelik kimliğini sağlayın. Bu kimliği Azure portalında [Abonelikler sayfasında](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) bulabilirsiniz.

**Azure kaynak grubu adı**: Kapsayıcı resminizle Azure Kapsayıcı Kayıt Defteri'ni içeren [kaynak grubu](https://aka.ms/ResourceManagerAzurePortal) adını sağlayın. Kaynak grubuna abonelik kimliğinde (üstte) erişilebilir olmalıdır. Adı Azure portalındaki [Kaynak grupları](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceGroups) sayfasında bulabilirsiniz.

**Azure kapsayıcı kayıt defteri adı**: Kapsayıcı resminizin olduğu Azure Kapsayıcı Kayıt [Defteri'nin](https://aka.ms/DockerContainerRegistriesAzure) adını sağlayın. Kapsayıcı kayıt defteri, daha önce sağladığınız Azure kaynak grubunda bulunmalıdır. Tam oturum açma sunucusu adını değil, yalnızca kayıt defteri adını sağlayın. Adından **azurecr.io** atlayıp emin olun. Kayıt defteri adını Azure portalında [Konteyner Kayıt Defterleri sayfasında](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.ContainerRegistry%2Fregistries) bulabilirsiniz.

**Azure Kapsayıcı Kayıt Defteri için yönetici kullanıcı adı**: Kapsayıcı resminizin olduğu Azure Kapsayıcı Kayıt Defteri ile ilişkili yönetici kullanıcı [adını](https://aka.ms/AdminAccountContainerRegistry) sağlayın. Şirketinizin kayıt defterine erişebilmesini sağlamak için kullanıcı adı ve parola gereklidir. Yönetici kullanıcı adı ve parolasını almak **için, yönetici etkin özelliğiAzure** Komut Satırı Arabirimi (CLI) kullanarak **True** olarak ayarlayın. **Yönetici kullanıcısını** isteğe bağlı olarak Azure portalında **Etkinleştir'e** ayarlayabilirsiniz.

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-admin-user.png" alt-text="Kapsayıcı kayıt defteri iletişim kutusunu güncelleştirin.":::

**Azure Kapsayıcı Kayıt Defteri için Parola**: Azure Kapsayıcı Kayıt Defteri ile ilişkili ve kapsayıcı resminiz olan yönetici kullanıcı adının parolasını sağlayın. Şirketinizin kayıt defterine erişebilmesini sağlamak için kullanıcı adı ve parola gereklidir. Parolayı Azure portalından Konteyner Kayıt **Defteri** > **Erişim Anahtarları'na** giderek veya azure CLI ile [göster komutunu](https://aka.ms/azacrcredentialshow) kullanarak alabilirsiniz.

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-username-password.png" alt-text="Azure portalındaki erişim anahtarı ekranını gösterir.":::

**Azure Kapsayıcı Kayıt Defteri'nde depo adı.** Resminize sahip Azure Kapsayıcı Kayıt Defteri deposunun adını sağlayın. Resmi kayıt defterine itdiğinizde deponun adını belirtirsiniz. [Konteyner Kayıt Defteri](https://aka.ms/ContainerRegistry) > **Depoları sayfasına**giderek deponun adını bulabilirsiniz. Daha fazla bilgi için Azure [portalındaki kapsayıcı kayıt defteri depolarını görüntüleyin.](https://aka.ms/ContainerRegistryRepositoriesAzure) Ad ayarlandıktan sonra değiştirilemediğini unutmayın. Hesabınızdaki her teklif için benzersiz bir ad kullanın.

### <a name="image-tags-for-new-versions-of-your-offer"></a>Teklifinizin yeni sürümleri için resim etiketleri

Müşteriler, bir güncelleştirme yayımladığınızda Azure Marketi'nden otomatik olarak güncelleştirme alabilmeli. Güncelleştirmek istemiyorlarsa, resminizin belirli bir sürümünde kalabilmeleri gerekir. Bunu, görüntüye her güncelleştirme yaptığınızda yeni görüntü etiketleri ekleyerek yapabilirsiniz.

**Resim etiketi**. Bu alan, desteklenen tüm platformlarda görüntünüzün en son sürümüne işaret eden **en son** bir etiket içermelidir. Ayrıca bir sürüm etiketi içermelidir (örneğin, xx.xx.xx ile başlayan, xx bir sayı olduğu). Müşteriler birden çok platformu hedeflemek için [bildirim etiketleri](https://aka.ms/GitHubmanifest-tool) kullanmalıdır. Bir bildirim etiketiyle başvurulan tüm etiketlerde de eklenmelidir, böylece bunları yükleyebiliriz. Tüm manifesto etiketleri (en son etiket hariç) X, Y ve Z'nin sondaolduğu X.Y- veya X.Y.Z- ile başlamalıdır. Örneğin, en son etiket 1.0.1-linux-x64, 1.0.1-linux-arm32 ve 1.0.1-windows-arm32'ye işaret ederse, bu altı etiketin bu alana eklenmesi gerekir. Etiketler ve sürümler hakkında ayrıntılı bilgi için [IoT Edge modülü teknik varlıklarınızı hazırlayın'](https://aka.ms/PrepareIoTEdgeModTechAssets) a bakın.

### <a name="default-deployment-settings-optional"></a>Varsayılan dağıtım ayarları (isteğe bağlı)

IoT Edge modülünüzü dağıtmak için en yaygın ayarları tanımlayın. Bu varsayılan ayarlarla IoT Edge modülünüzü kullanıma sunmasına izin vererek müşteri dağıtımlarını optimize edin.

**Varsayılan yollar**. IoT Edge Hub, modüller, IoT Hub'ı ve aygıtlar arasındaki iletişimi yönetir. Modüller ve IoT Hub arasında veri girişi ve çıktı yolları ayarlayabilirsiniz, bu da size iletileri işlemek veya ek kod yazmak için ek hizmetlere gerek kalmadan gitmeleri gereken yerlerde ileti gönderme esnekliği sağlar. Rotalar ad/değer çiftleri kullanılarak oluşturulur. Her biri en fazla 512 karakter uzunluğunda beş varsayılan rota adı tanımlayabilirsiniz.

Rota değerinizde doğru [rota sözdizimini](https://aka.ms/DeclareRoutesAzureIoT) kullandığınızdan emin olun (genellikle FROM/message/* INTO $upstream olarak tanımlanır). Bu, herhangi bir modül tarafından gönderilen iletilerin IoT Hub'ınıza gittiği anlamına gelir. Modülünüze başvurmak için, boşluk veya özel karakter olmadan **Teklif Adınız**olacak varsayılan modül adını kullanın. Henüz tanınmayan diğer modüllere başvurmak için, müşterilerinizin bu bilgileri güncelleştirmeleri gerektiğini bildirmek için <> sözleşmesi FROM_MODULE_NAME kullanın. IoT Edge rotaları hakkında daha fazla bilgi [için, Bildir rotalarını](https://aka.ms/DeclareRoutesAzureIoT)görün.

Örneğin, contosoinput ve çıktı verilerindeki contosoinoutput'daki girdileri modül ContosoInoutput'da modül dinliyorsa, aşağıdaki iki varsayılan yolu tanımlamak mantıklıdır:

- Adı #1: ToContosoModule
- Değer #1: FROM /messages/modules/<FROM_MODULE_NAME>/outputs/* INTO BrokeredEndpoint("/modules/ContosoModule/inputs/ContosoInput")
- Ad #2: FromContosoModuleToCloud
- Değer #2: FROM /messages/modules/ContonsoModule/outputs/ContosoOutput INTO $upstream

**Varsayılan modül ikiz istenilen özellikleri**. Modül ikizi, IoT Hub'ında istenen özellikler de dahil olmak üzere durum bilgilerini bir modül örneği için depolayan bir JSON belgesidir. İstenilen özellikler, modül yapılandırmasını veya koşullarını eşitlemek için bildirilen özelliklerle birlikte kullanılır. Çözüm arka uç istenilen özellikleri ayarlayabilirsiniz ve modül bunları okuyabilirsiniz. Modül ayrıca istenen özelliklerde değişiklik bildirimleri de alabilir. İstenen özellikler en fazla beş ad/değer çifti kullanılarak oluşturulur ve her varsayılan değer 512 karakterden az olmalıdır. En fazla beş ad/değer ikizi istenen özellikleri tanımlayabilirsiniz. İkiz istenilen özelliklerin değerleri, dört düzeyden oluşan en yüksek iç içe hiyerarşiye sahip diziler olmadan, kaçamayan JSON geçerli olmalıdır. Varsayılan değer için gereken parametrenin (örneğin, müşterinin sunucusunun IP adresi) anlamlı olmadığı bir senaryoda, varsayılan değer olarak bir parametre ekleyebilirsiniz. İstenilen ikiz özellikler hakkında daha fazla bilgi edinmek için, [bkz.](https://aka.ms/DefineUpdateProperties)

Örneğin, bir modül, istenen ikiz özellikleri kullanarak dinamik olarak yapılandırılabilir yenileme hızını destekliyorsa, aşağıdaki varsayılan ikiz istenen özelliği tanımlamak mantıklıdır:

- Ad #1: RefreshRate
- Değer #1: 60

**Varsayılan ortam değişkenleri.** Ortam değişkenleri, yapılandırma sürecine yardımcı olan bir modüle ek bilgi sağlar. Ortam değişkenleri ad/değer çiftleri kullanılarak oluşturulur. Her varsayılan ortam değişken adı ve değeri 512 karakterden az olmalıdır ve en fazla beş karakter tanımlayabilirsiniz. Varsayılan değer için gerekli bir parametre mantıklı gelmiyorsa (örneğin, müşterinin sunucusunun IP adresi), varsayılan değer olarak bir parametre ekleyebilirsiniz.

Örneğin, bir modül başlatılmadan önce kullanım koşullarını kabul etmeyi gerektiriyorsa, aşağıdaki ortam değişkenini tanımlayabilirsiniz:

- Ad #1: ACCEPT_EULA
- Değer #1: Y

**Varsayılan kapsayıcı seçenekleri oluşturun.** Konteyner oluşturma seçenekleri, IoT Edge modülü Docker konteynerinin oluşturulmasını yönlendirir. IoT Edge, Docker engine API Create Container seçeneklerini destekler. [Liste kapsayıcılarında](https://aka.ms/ContainerList) tüm seçenekleri görün. Oluşturma seçenekleri alanı geçerli JSON, kaçamayan ve 512 karakterden az olmalıdır.

Örneğin, bir modül bağlantı noktası bağlama gerektiriyorsa, aşağıdaki oluşturma seçeneklerini tanımlayın:

"HostConfig":{"PortBindings":{"5012/tcp":[{"HostPort":"5012"}]}

## <a name="review-and-publish"></a>İnceleme ve yayınlama

Teklifin gerekli tüm bölümlerini tamamladıktan sonra, bunu incelemek ve yayınlamak için gönderebilirsiniz.

Portalın sağ üst köşesinde, **Gözden Geçir'i seçin ve yayınlayın.**

İnceleme sayfasında yayımlama durumunu görebilirsiniz:

- Teklifin her bölümü için tamamlanma durumuna bakın. Teklifin tüm bölümleri tamamlanana kadar yayımlayamazsınız.
    - **Başlatılmadı** - Bölüm başlatılmadı ve tamamlanması gerekiyor.
    - **Eksik** - Bölümde düzeltilmesi gereken veya daha fazla bilgi sağlamanızı gerektiren hatalar vardır. Rehberlik için bu belgedeki önceki bölümlere bakın.
    - **Tam** - Bölümde gerekli tüm veriler vardır ve herhangi bir hata yoktur. Teklifi sunamadan önce teklifin tüm bölümlerinin tamamlanmış olması gerekmektedir.
- Teklifinizin doğru test edildiğinden emin olmak için belgelendirme ekibine test talimatları sağlayın. Ayrıca, teklifinizi anlamak için yararlı olan ek notlar sağlayın.

Yayımlama teklifini göndermek için **Yayımla'yı**seçin.

Teklifin önizleme sürümünün gözden geçirilip onaylamayacağını bildirmek için size bir e-posta göndeririz. Teklifinizi halka (veya özel bir teklifte özel bir hedef kitleye) yayınlamak için Ortak Merkezi'ne gidin ve **Go-live'ı**seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Ticari pazarda mevcut bir teklifi güncelleştirme](https://docs.microsoft.com//azure/marketplace/partner-center-portal/update-existing-offer)