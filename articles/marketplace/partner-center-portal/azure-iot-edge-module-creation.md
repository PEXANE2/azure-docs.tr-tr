---
title: Iş Ortağı Merkezi ile Azure IoT Edge bir modül teklifi oluşturma-Azure Marketi
description: Iş Ortağı Merkezi 'ni kullanarak Azure Marketi 'nde IoT Edge modül teklifi oluşturmayı öğrenin
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/03/2020
ms.openlocfilehash: cca54e4e456fe766b190f64657cd1aca1d9520e0
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81869137"
---
# <a name="create-an-iot-edge-module-offer"></a>IoT Edge modül teklifi oluşturma

> [!IMPORTANT]
> IoT Edge modülü tekliflerinizin yönetimini Bulut İş Ortağı Portalı Iş ortağı merkezine taşıdık. Tekliflerinizi geçirene kadar, tekliflerinizi yönetmek için, lütfen IoT Edge modülündeki yönergeleri izleyerek Bulut İş Ortağı Portalı için [yayımlamaya genel yayımlama](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/iot-edge-module/cpp-offer-process-parts) konusuna göz atın.

Bu makalede, Azure Marketi için bir Nesnelerin İnterneti (IoT) Edge modülü teklifinin nasıl oluşturulacağı ve yayımlanacağı açıklanır.

IoT Edge bir modül teklifi oluşturabilmeniz için önce Iş Ortağı Merkezi 'nde bir ticari Market hesabınız olmalıdır. Henüz bir tane oluşturmadıysanız [Iş Ortağı Merkezi 'nde ticari Market hesabı oluşturma](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account)konusuna bakın.

## <a name="create-a-new-offer"></a>Yeni teklif oluşturma

1. İş Ortağı Merkezi'nde oturum açın.
2. Sol gezinti menüsünde **ticari Market** > **genel bakış**' ı seçin.

    ![Sol gezinti menüsünü gösterir.](./media/cs-menu-overview.png)

3. **+ Yeni teklif** > **IoT Edge modülünü**seçin. **Yeni teklif** iletişim kutusu görüntülenir.

> [!IMPORTANT]
> Bir teklif yayımlandıktan sonra, Iş Ortağı Merkezi 'nde yapılan düzenlemeler, teklif yeniden yayımlandıktan sonra yalnızca stoporonts 'de görüntülenir. Değişiklik yaptıktan sonra her zaman yeniden yayımlaytığınızdan emin olun.

### <a name="offer-id-and-alias"></a>Teklif KIMLIĞI ve diğer ad

Bir **TEKLIF kimliği**girin. Bu, hesabınızdaki her teklif için benzersiz bir tanımlayıcıdır.

- Bu KIMLIK, varsa Market teklifi ve Azure Resource Manager şablonları için Web adresinde müşteriler tarafından görülebilir.
- Yalnızca küçük harfleri ve rakamları kullanın. Kısa çizgi ve alt çizgi içerebilir, ancak boşluk içeremez ve 50 karakterle sınırlıdır. Örneğin, **Test-teklif-1**girerseniz, teklif Web adresi olacaktır `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.
- Teklif KIMLIĞI, Oluştur ' u seçtikten sonra değiştirilemez.

Bir **teklif diğer adı**girin. Bu, Iş Ortağı Merkezi 'nde teklifi ifade etmek için kullanılan addır.

- Bu ad Market 'te kullanılmıyor ve teklif adından ve müşterilere gösterilen diğer değerlerden farklı.
- Bu, **Oluştur**' u seçtikten sonra değiştirilemez.

Bu iki değeri girdikten sonra, bir sonraki sayfaya geçmeden önce **Oluştur** ' u seçin, teklif genel bakış ' a tıklayın.

## <a name="offer-overview"></a>Teklifin genel bakış

**Teklif genel bakış** sayfasında, bu teklifi yayımlamak için gereken adımların görsel bir gösterimi gösterilir (hem tamamlandı hem de yakında) ve her adımın tamamlanması gereken süre.

Bu sayfa, yaptığınız seçime bağlı olarak bu teklif üzerinde işlem gerçekleştirme bağlantılarını içerir. Örneğin:

- Teklif bir taslak- [silme taslağı tekliftir](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#delete-a-draft-offer)
- Teklif canlı ise, [teklifi satmaya](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#stop-selling-an-offer-or-plan)
- Teklif önizleme sürümündedir- [canlı](https://docs.microsoft.com/azure/marketplace/partner-center-portal/publishing-status#publisher-approval)
- Yayımcı oturumu kapatma Işlemini tamamlamadıysanız [yayımlamayı Iptal edin.](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#cancel-publishing)

## <a name="offer-setup"></a>Teklif kurulumu

Teklifinizi ayarlamak için bu adımları izleyin.

### <a name="connect-lead-management"></a>Müşteri adayı yönetimini bağlama

Teklifinizi Iş Ortağı Merkezi ile Market 'te yayımlarken, isteğe bağlı olarak Müşteri Ilişkileri yönetimi (CRM) sisteminize bağlayabilirsiniz. Bu, bir kişi, ürününüzü ilgilendiren veya ürünü kullandığı anda müşteri iletişim bilgilerini almanızı sağlar.

1. Potansiyel müşterileri göndermemizi istediğiniz müşteri adayı hedefini seçin. İş Ortağı Merkezi aşağıdaki CRM sistemlerini destekler:

    - Müşteri katılımı için [Dynamics 365](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics)
    - [Marketo](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-marketo)
    - [Salesforce](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce)

    > [!NOTE]
    > CRM sisteminiz yukarıda listelenmiyorsa, müşteri adayı verilerini depolamak için [Azure tablosu](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table) veya [https uç noktasını](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-https) kullanın, ardından verileri CRM sisteminize dışarı aktarın.

2. Teklifinizi Iş Ortağı Merkezi 'nde yayımlarken lider hedefine bağlayın.
3. Müşteri adayı hedefi bağlantısının düzgün şekilde yapılandırıldığını doğrulayın. Iş Ortağı Merkezi 'nde yayımladıktan sonra, bağlantıyı doğrulayacağız ve size bir test lideri göndereceğiz. Teklifi canlı olmadan önce önizlerken, teklifi önizleme ortamında satın almayı deneyerek lider bağlantınızı da test edebilirsiniz.
4. Müşteri adayı hedefine olan bağlantının güncelleştirildiğinden emin olun, böylece müşteri adaylarını kaybetmezsiniz.

Bazı ek müşteri yönetimi kaynakları aşağıda verilmiştir:

- [Müşteri adayı yönetimine genel bakış](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-get-customer-leads)
- [Müşteri adayı yönetimi hakkında SSS](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [Sık karşılaşılan müşteri adayı yapılandırma hataları](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#common-lead-configuration-errors-during-publishing-on-cloud-partner-portal)
- [Müşteri adayı yönetimine genel bakış](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf) PDF (açılır pencere engelleyicinizin kapalı olduğundan emin olun).

Sonraki bölüme devam etmeden önce **Taslağı kaydet** ' i seçin.

### <a name="properties"></a>Özellikler

Bu sayfa, teklifinizi Market 'te gruplandırmak için kullanılan kategorileri ve teklifinizi destekleyen yasal sözleşmeleri tanımlamanızı sağlar.

#### <a name="category"></a>Kategori

En az bir ve en fazla beş kategori seçin. Bu kategoriler, teklifinizi uygun Market arama alanına yerleştirmek için kullanılır ve teklif ayrıntıları sayfasında gösterilir. Teklif açıklamasında, teklifinizin bu kategorileri nasıl desteklediğini açıklayın. Tarayıcı sayfalarında, tüm IoT Edge modülleri **nesnelerin interneti > IoT Edge modül** kategorisi altında gösterilir.

#### <a name="legal"></a>Yasal Bildirim

Teklif için hüküm ve koşullar sağlamanız gerekir. İki seçeneğiniz vardır:

- Microsoft ticari Market için standart sözleşmeyi kullanın.
- Kendi hüküm ve koşullarınızı belirtin.

##### <a name="standard-contract-for-the-microsoft-commercial-marketplace"></a>Microsoft ticari Market için standart sözleşme

Ticari Market 'teki işlemleri kolaylaştırmaya yardımcı olmak için standart bir sözleşme şablonu sunuyoruz. Çözümünüzü standart sözleşme altında sunmayı tercih edebilirsiniz; Bu, müşterilerin yalnızca bir kez denetlemesi ve kabul etmesi gereken bir çözümdür. Özel hüküm ve koşullar yapmak istemiyorsanız bu iyi bir seçenektir.

Standart Sözleşme hakkında daha fazla bilgi edinmek için bkz. [Microsoft ticari Market Için standart sözleşme](https://docs.microsoft.com/azure/marketplace/standard-contract). Ayrıca [standart sözleşme](https://go.microsoft.com/fwlink/?linkid=2041178) PDF 'sini de indirebilirsiniz (açılır pencere engelleyicisinin kapalı olduğundan emin olun).

Standart sözleşmeyi kullanmak için **Microsoft 'un ticari Market Için standart sözleşmeyi kullan** onay kutusunu işaretleyin ve ardından **kabul et**' e tıklayın.

> [!NOTE]
> Microsoft ticari Market için standart sözleşmeyi kullanarak bir teklifi yayımladıktan sonra, kendi özel hüküm ve koşullarınızı kullanamazsınız. Çözümünüzü standart sözleşme kapsamında ya da kendi hüküm ve koşullarınızın altına sunun.

![Microsoft 'un ticari Market onay kutusu için standart sözleşmeyi kullanmayı gösterir.](./media/iot-edge-module-creation/iot-edge-module-standard-contract-checkbox.png)

##### <a name="your-own-terms-and-conditions"></a>Kendi hüküm ve koşullarınız

Kendi özel hüküm ve koşullarınızı sağlamak için **hüküm ve koşullar** kutusuna girin. Bu kutuya metnin sınırsız miktarda karakterini girebilirsiniz. Müşterilerinizin teklifinizi deneyebilmeleri için önce bu koşulları kabul etmesi gerekir.

Bir sonraki bölüme devam etmeden önce **Taslağı kaydet** ' i seçin, teklif listesi.

## <a name="offer-listing"></a>Teklif listesi

Burada, Market 'te görüntülenen teklif ayrıntılarını tanımlayacaksınız. Buna teklif adı, açıklama, görüntüler vb. dahildir. Bu teklifi yapılandırırken Microsoft 'un ilke sayfasında ayrıntılı ilkeleri izlediğinizden emin olun.

> [!NOTE]
> Teklif açıklaması, "Bu uygulama yalnızca [Ingilizce olmayan dil] bölümünde kullanılabilir" ifadesi ile başlıyorsa, teklif ayrıntılarının Ingilizce olması gerekmez. Ayrıca, teklif listesi ayrıntılarından farklı bir dilde içerik sunmak için faydalı bir bağlantı sağlamak da yararlı olabilir.

### <a name="name"></a>Adı

Buraya girdiğiniz ad teklifinizin başlığı olarak görüntülenir. Bu alan, teklifi oluştururken **teklif diğer adı** kutusuna girdiğiniz metin ile önceden doldurulur. Bu adı daha sonra değiştirebilirsiniz.

Ad:

- Trademarked olabilir (ve ticari marka veya telif hakkı sembolleri dahil edebilirsiniz).
- 50 karakterden uzun olamaz.
- Emojıs dahil olamaz.

### <a name="search-results-summary"></a>Arama sonuçları Özeti

Teklifinizin kısa bir açıklamasını sağlayın. Bu, 100 karakter uzunluğunda olabilir ve Market arama sonuçlarında kullanılır.

### <a name="long-summary"></a>Uzun Özet

Teklifinizin daha ayrıntılı bir açıklamasını sağlayın. Bu, 256 karakter uzunluğunda olabilir ve Market arama sonuçlarında kullanılır.

### <a name="description"></a>Açıklama

Teklifinizin en fazla 3.000 karakter uzunluğunda olması için daha uzun bir açıklama sağlayın. Bu, Market dökümüne Genel Bakış bölümündeki müşterilere görüntülenir.

Açıklamasında aşağıdakilerden birini veya birkaçını ekleyin:

- Teklifinizin sağladığı değer ve önemli avantajlar
- Kategori veya sektör ilişkilendirmeleri veya her ikisi
- Uygulama içi satın alma fırsatları
- Gerekli tüm kaldırlar

IoT Edge modül teklifleri, açıklamanın en altında bulunan en düşük donanım gereksinimleri paragrafını içermelidir. Örneğin:

*En düşük donanım gereksinimleri: Linux x64 ve ARM32 OS, 1 GB RAM, 500 MB depolama*

Tanımlarınızı yazmak için bazı ipuçları aşağıda verilmiştir:

- Açıklayamlarınızın ilk birkaç cümlede teklifinizin değerini açıkça açıklamalıdır. Aşağıdaki öğeleri ekleyin:
    - Teklifin açıklaması.
    - Tekliften faydalanan Kullanıcı türü.
    - Müşteri ihtiyaçları veya teklif adreslerini yayınlar.
- İlk birkaç tümcenin arama sonuçlarında görüntülenebileceğini unutmayın.
- Ürününüzü satmaya yönelik özelliklere ve işlevlere güvenmeyin. Bunun yerine, teklifinizin sağladığı değere odaklanın.
- Sektöre özgü sözlük veya avantaj tabanlı bir ifade kullanmayı deneyin.

Teklif **açıklamanızı** daha ilgi çekici hale getirmek için, tanımlarınızı biçimlendirmek için zengin metin düzenleyicisini kullanın. Zengin metin Düzenleyicisi, tanımlarınızın daha okunaklı olması için sayılar, madde işaretleri, kalın, italik ve girintiler eklemenize olanak tanır.

:::image type="content" source="media/text-editor2.png" alt-text="Zengin metin düzenleyicisini gösterir." border="false":::

- İçeriğinizin biçimini değiştirmek için, biçimlendirmek istediğiniz metni vurgulayın ve bu ekran görüntüsünde gösterildiği gibi bir metin stili seçin:

     :::image type="content" source="media/text-editor3.png" alt-text="Zengin metin düzenleyicisinde metin stili denetimini gösterir." border="false":::

- Metne bir madde işaretli veya numaralı liste eklemek için, bu ekran görüntüsünde gösterilen seçenekleri kullanın:
  
    :::image type="content" source="media/text-editor4.png" alt-text="Zengin metin düzenleyicisinde madde işaretli ve sayı listesi denetimlerini gösterir." border="false":::

- Metne girintileme eklemek veya kaldırmak için, bu ekran görüntüsünde gösterilen seçenekleri kullanın:

    :::image type="content" source="media/text-editor5.png" alt-text="Zengin metin düzenleyicisinde girintileme denetimlerini gösterir." border="false":::

#### <a name="privacy-policy-url"></a>Gizlilik ilkesi URL 'SI

Kuruluşunuzun gizlilik ilkesinin Web adresini girin. Teklifinizin gizlilik yasaları ve yönetmeliklerle uyumlu olmasını sağlamaktan sorumlu olursunuz. Ayrıca, Web sitenizde geçerli bir gizlilik ilkesi göndermekten de sorumlusunuz.

#### <a name="useful-links"></a>Yararlı bağlantılar

Teklifiniz hakkında ek çevrimiçi belgeler sağlayın. En fazla 25 bağlantı ekleyebilirsiniz. Bir bağlantı eklemek için **+ bağlantı ekle** ' yi seçin ve ardından aşağıdaki alanları doldurun:

- **Başlık** -müşteriler, teklifin ayrıntılar sayfasında başlığı görür.
- **Bağlantı (URL)** -müşterilerin çevrimiçi belgenizi görüntülemesi için bir bağlantı girin. Bağlantı, http://veya https://ile başlamalıdır.

Belgelerinize en az bir bağlantı ve [Azure IoT cihaz kataloğundan](https://catalog.azureiotsolutions.com/)uyumlu IoT Edge cihazlara bir bağlantı eklediğinizden emin olun.

### <a name="contact-information"></a>İletişim bilgileri

Bir **destek kişisi** ve **mühendislik ilgili kişisi** için ad, e-posta ve telefon numarası sağlamanız gerekir. Bu bilgiler müşterilere gösterilmez. Microsoft tarafından kullanılabilir ve bulut çözümü sağlayıcısı (CSP) iş ortakları için sağlanmış olabilir.

- Destek kişisi (gerekli): genel destek soruları Için.
- Mühendislik ilgili kişisi (gerekli): Teknik sorular ve sertifika sorunları Için.
- CSP program kişisi (isteğe bağlı): CSP programıyla ilgili satıcı soruları Için.

**Destek kişisi** bölümünde, ortağın küresel Azure 'Da, Azure Kamu 'da veya her ikisinde de kullanılabilir olup olmadığına bağlı olarak iş ortaklarının teklif desteğini bulabileceği **Destek Web sitesinin** Web adresini sağlayın.

**CSP program kişisi** bölümünde, CSP iş ortaklarının teklifiniz için pazarlama malzemeleri bulabileceği bağlantıyı (**CSP program pazarlama malzemeleri**) sağlayın.

#### <a name="additional-marketplace-listing-resources"></a>Ek Market listeleme kaynakları

Teklif listeleri oluşturma hakkında daha fazla bilgi için bkz. [teklif listesi en iyi uygulamaları](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices).

### <a name="marketplace-images"></a>Market görüntüleri

Teklifinizle birlikte kullanılacak logo ve görüntüler sağlayın. Tüm görüntülerin. png biçiminde olması gerekir. Bulanık görüntüler reddedilir.

>[!Note]
>Dosya yükleme sorununuz varsa, yerel ağınızın Iş Ortağı Merkezi tarafından kullanılan https://upload.xboxlive.com hizmeti engellemediğinden emin olun.

#### <a name="store-logos"></a>Mağaza logoları

Aşağıdaki dört piksel boyutunda, teklifinizin logosunun. png dosyalarını sağlayın:

- **Küçük (48 x 48)**
- **Orta (90 x 90)**
- **Büyük (216 x 216)**
- **Geniş (255 x 115)**

Dört logo gereklidir ve Market listesindeki farklı yerlerde kullanılır.

#### <a name="screenshots-optional"></a>Ekran görüntüleri (isteğe bağlı)

Teklifinizin nasıl çalıştığını gösteren en fazla beş ekran görüntüsü ekleyin. Her biri, boyut ve. png biçiminde 1280 x 720 piksel olmalıdır.

#### <a name="videos-optional"></a>Videolar (isteğe bağlı)

Teklifinizi gösteren en fazla beş video ekleyin. Videonun adını, Web adresini ve videonun küçük resim. png görüntüsünü 1280 x 720 piksel boyutunda girin.

#### <a name="offer-examples"></a>Teklif örnekleri

Aşağıdaki örneklerde teklif listeleme alanlarının teklifin farklı konumlarında nasıl göründüğü gösterilmektedir.

Bu ekran görüntüsünde, Azure Marketi 'ndeki **teklif listesi** sayfası gösterilmektedir.

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-offer-listing-page.png" alt-text="Azure Marketi 'ndeki teklif listesi sayfasını gösterir.":::

Bu ekran görüntüsünde Azure Marketi 'ndeki arama sonuçları gösterilmektedir:

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-marketplace-search-results.png" alt-text="Azure Marketi 'ndeki arama sonuçlarını gösterir.":::

Bu ekran görüntüsünde Azure portal **teklif listesi** sayfası gösterilmektedir.

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-listing-page-azure-portal.png" alt-text="Azure portal teklif listesi sayfasını gösterir.":::

Bu ekran görüntüsünde Azure portal arama sonuçları gösterilmektedir.

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-azure-portal-search-results.png" alt-text="Azure portal teklif listesi sayfasını gösterir.":::

Sonraki bölüme devam etmeden önce **Taslağı kaydet** ' i seçin, önizleme.

## <a name="preview"></a>Önizleme

**Önizleme sekmesinde**, daha geniş Market hedef kitlesi için canlı yayımlamadan önce teklifinizi doğrulamak üzere sınırlı bir **Önizleme hedef kitlesi** seçebilirsiniz.

> [!IMPORTANT]
> Teklifinizi önizlemede güncelleştirdikten sonra teklifinizi herkese açık bir şekilde yayımlamak için **canlı git** ' i seçmeniz gerekir.

Azure abonelik KIMLIĞI GUID 'Lerini kullanarak önizleme Dinleyicilerinizi, her biri için isteğe bağlı bir açıklama ile birlikte belirtin. Bu alanlardan hiçbiri müşteriler tarafından görülenebilir.

> [!NOTE]
> Azure abonelik KIMLIĞINIZI Azure portal abonelikler sayfasında bulabilirsiniz.

Tek tek (10 ' a kadar) veya bir CSV dosyası karşıya yükleyerek (100 'e kadar) en az bir Azure abonelik KIMLIĞI ekleyin. Bu abonelik kimliklerini ekleyerek teklifinizin canlı olarak yayınlanmadan önce ne kadar önizleyebilir tanımlar. Teklifiniz zaten canlı ise, teklifinizdeki değişiklikleri veya güncelleştirmeleri test etmek için bir önizleme izleyiciyi tanımlayabilirsiniz.

> [!NOTE]
> Önizleme hedef kitlesi, özel bir hedef kitleye göre farklılık gösterir. Bir **Önizleme** hedef kitlesi, yalnızca **özel** bir hedef kitleye yayımlanmaları (kullanılabilirlik sekmesinde ayarlanır) dahil olmak üzere Market 'te canlı olmadan önce tüm teklif planlarını görebilir ve doğrulayabilirler.

Sonraki bölüme geçmeden önce **Taslağı kaydet** ' i seçin, plana genel bakış ' a tıklayın.

### <a name="plan-overview"></a>Plana genel bakış

Bu sekme, Iş Ortağı Merkezi 'nde aynı teklif dahilinde farklı plan seçenekleri sağlamanıza olanak tanır. Bu planlar, daha önce SKU 'Ları veya stok tutma birimleri olarak adlandırılmıştı. Planlar, genel bulutlar, kamu bulutları ve plan tarafından başvurulan görüntü gibi bulutların kullanılabildiği koşullara göre farklılık gösterebilir. Teklifinizi Market 'te listelemek için en az bir plan ayarlamanız gerekir.

Planlarınızı oluşturduktan sonra **plana genel bakış** sekmesi şunları gösterir:

- Plan adları
- Fiyatlandırma modeli
- Bulut kullanılabilirliği (küresel veya kamu)
- Geçerli yayımlama durumu
- Kullanılabilir eylemler

Plana genel bakışta sunulan eylemler planınızın geçerli durumuna bağlı olarak farklılık gösterir. Şunları içerir:

- **Taslağı Sil**: plan durumu bir tasladır.
- **Satışı durdur plan**: plan durumu canlı olarak yayımlandı.

#### <a name="create-new-plan"></a>Yeni plan oluştur

**Yeni plan oluştur**' u seçin. **Yeni plan** iletişim kutusu görüntülenir.

**Plan kimliği** kutusunda, bu teklifte her plan için benzersiz BIR plan kimliği oluşturun. Bu KIMLIK, ürün web adresinde müşterilere görünür olacaktır. Yalnızca küçük harf ve rakam, tire veya alt çizgi ve en fazla 50 karakter kullanın.

**Plan adı** kutusuna bu plan için bir ad girin. Müşteriler, teklifiniz dahilinde hangi planın seçeceğinize karar verirken bu adı görür. Bu teklifte her plan için benzersiz bir ad oluşturun. Örneğin, Windows Server **2016** ve **Windows Server 2019**planlarına **sahip bir** teklif adı kullanabilirsiniz.

> [!NOTE]
> Plan KIMLIĞI, **Oluştur**' u seçtikten sonra değiştirilemez.

**Oluştur**’u seçin.

### <a name="plan-setup"></a>Planı ayarla

Bu sekme, planın hangi bulutlarda kullanılabilir olduğunu yapılandırmanızı sağlar. Bu sekmedeki yanıtlarınız, diğer sekmelerde hangi alanların görüntülendiğini etkiler.

#### <a name="cloud-availability"></a>Bulut kullanılabilirliği

Planınız Azure IoT Hub kullanılarak en az bir bulutta kullanılabilir olmalıdır.

Planınızın, Market kullanan tüm genel Azure bölgelerinde müşteriler tarafından kullanılabilmesi için **Azure genel** seçeneğini belirleyin. Ayrıntılar için bkz. [coğrafi kullanılabilirlik ve para birimi desteği](https://docs.microsoft.com/azure/marketplace/marketplace-geo-availability-currencies).

Çözümünüzü burada görünmesini sağlamak için [Azure Kamu Bulutu](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome) seçeneğini belirleyin. Bu, ABD Federal, eyalet ve yerel ya da üç devlet kurumlarının yanı sıra onlara hizmeti sunmaya uygun iş ortakları için denetimli erişime sahip bir kamu topluluk bulutuna sahiptir. Yayımcı olarak, bu bulut topluluğu için tüm uyumluluk denetimleri, güvenlik ölçüleri ve en iyi uygulamalardan siz sorumlusunuz. Azure Kamu fiziksel olarak yalıtılmış veri merkezleri ve ağlar (yalnızca ABD 'de bulunur) kullanır. Azure Kamu 'da [yayımlamadan](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-marketplace-partners) önce, sonuçlar farklı olabileceğinden çözümünüzü test edin ve onaylayın. Çözümünüzü hazırlamak ve test etmek için [Microsoft Azure Kamu deneme](https://azure.microsoft.com/global-infrastructure/government/request/)sürümünden bir deneme hesabı isteyin.

> [!NOTE]
> Planınız yayımlandıktan ve belirli bir bulutta kullanılabilir olduktan sonra, bu bulutu kaldıramazsınız.

#### <a name="azure-government-cloud-certifications"></a>Azure Kamu bulut sertifikaları

Bu seçenek yalnızca **Azure Kamu Bulutu** 'nın **bulut kullanılabilirliği**altında seçilmesi durumunda görünür.

Azure Kamu Hizmetleri, belirli kamu düzenlemelerine ve gereksinimlerine tabi olan verileri işler. Örneğin, Fedrampa, NıST 800,171 (DIB), ıTAR, ıRS 1075, DoD L4 ve CJıS. Bu programların sertifikalarına yönelik olarak bir açıklama getirmek için, sertifikalarınızı tanımlayan en fazla 100 bağlantı sağlayabilirsiniz. Bunlar program üzerinde doğrudan veya kendi web sitenizde yer alan dökümlerinizin bağlantısı olabilir. Bu bağlantılar yalnızca Azure Kamu müşterileri tarafından görülebilir.

## <a name="plan-listing"></a>Planı listeleme

Bu sekme, aynı teklif içindeki her farklı plan için belirli bilgileri görüntüler.

### <a name="plan-name"></a>Plan adı

Bu, planınızı oluşturduğunuz sırada verdiğiniz adla önceden doldurulmuştur. Bu adı gerektiği şekilde değiştirebilirsiniz. En fazla 50 karakter uzunluğunda olabilir. Bu ad, Azure Marketi 'nde bu planın başlığı olarak görünür ve Azure portal. Plan kullanılmak üzere hazır olduktan sonra varsayılan modül adı olarak kullanılır.

### <a name="plan-summary"></a>Plan Özeti

Planınızın kısa bir özetini sağlayın (teklif değil). Bu Özet, Azure Marketi arama sonuçlarında görünür ve en fazla 100 karakter içerebilir.

### <a name="plan-description"></a>Plan açıklaması

Bu planın ne olduğunu ne yaptığını ve teklifinizdeki planlar arasındaki farkları betimleyebilirsiniz. Teklifi değil, yalnızca plan. Bu açıklama, Azure Marketi 'nde ve teklif listesi sayfasındaki Azure portal görüntülenir. Plan özetinde girdiğiniz içerik aynı olabilir ve en fazla 2.000 karakter içerebilir.

Bu alanları tamamladıktan sonra **Taslağı kaydet** ' i seçin.

#### <a name="plan-examples"></a>Plan örnekleri

Aşağıdaki örneklerde plan listeleme alanlarının farklı görünümlerde nasıl göründüğü gösterilmektedir.

Plan ayrıntılarını görüntülerken Azure Marketi 'ndeki alanlar şunlardır:

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-marketplace-plan-details.png" alt-text="Azure Marketi 'nde plan ayrıntılarını görüntülerken gördüğünüz alanları gösterir.":::

Bunlar Azure portal plan ayrıntılardır:

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-azure-portal-plan-details.png" alt-text="Azure portal plan ayrıntılarını gösterir.":::

## <a name="availability"></a>Kullanılabilirlik

Müşterilerin Market 'te arayabilmesi, gözatmasını veya satın almasını sağlamak için yayınlanmış teklifinizi gizlemek istiyorsanız, kullanılabilirlik sekmesinde **planı Gizle** onay kutusunu seçin.

Bu alan genellikle şu durumlarda kullanılır:

- Teklif, başka bir uygulama olarak başvuruluyorsa yalnızca dolaylı olarak kullanılmak üzere tasarlanmıştır.
- Teklif ayrı olarak satın alınmamalıdır.
- Plan ilk test için kullanıldı ve artık ilgili değil.
- Plan, geçici veya dönemsel teklifler için kullanıldı ve artık sunulmamalıdır.

## <a name="technical-configuration"></a>Teknik yapılandırma

**IoT Edge modülü** teklif türü, bir IoT Edge cihazında çalışan belirli bir kapsayıcı türüdür. **Teknik yapılandırma** sekmesinde, [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)içinde kapsayıcı görüntü deponuz için başvuru bilgileri ve müşterilerin modülü kolayca kullanmasına izin veren yapılandırma ayarlarını sağlarsınız.

Teklif yayımlandıktan sonra, IoT Edge kapsayıcı resminiz belirli bir ortak kapsayıcı kayıt defterinde Azure Marketi 'ne kopyalanır. Azure kullanıcılarından modülünüzü kullanmaya yönelik tüm istekler, özel kapsayıcı Kayıt defterinizden değil, Azure Marketi ortak kapsayıcı kayıt defterinden sunulur.

Birden çok platformu hedefleyebilir ve Etiketler kullanarak modül kapsayıcı resminizin çeşitli sürümlerini sağlayabilirsiniz. Etiketler ve sürüm oluşturma hakkında daha fazla bilgi için bkz. [IoT Edge modülünüzü hazırlama teknik varlıkları](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-iot-edge-module-asset).

### <a name="image-repository-details"></a>Görüntü deposu ayrıntıları

**Görüntü deposu ayrıntıları** sekmesinde aşağıdaki bilgileri sağlarız.

**Görüntü kaynağını seçin**: **Azure Container Registry** seçeneğini belirleyin.

**Azure ABONELIK kimliği**: kaynak kullanımının BILDIRILDIĞI abonelik kimliğini ve kapsayıcı görüntünüzü içeren Azure Container Registry için hizmetlerin faturalandırıldığını belirtin. Bu KIMLIĞI, Azure portal [abonelikler sayfasında](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) bulabilirsiniz.

**Azure Kaynak grubu adı**: kapsayıcı yansımanıza Azure Container Registry içeren [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-portal) adını sağlayın. Kaynak grubuna abonelik KIMLIĞINDE erişilebilir olması gerekir (yukarıda). Adı Azure portal [kaynak grupları](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceGroups) sayfasında bulabilirsiniz.

**Azure Container kayıt defteri adı**: kapsayıcı görüntünüzü içeren [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/container-registry-intro) adını belirtin. Kapsayıcı kayıt defterinin, daha önce belirttiğiniz Azure Kaynak grubunda mevcut olması gerekir. Tam oturum açma sunucusu adını değil yalnızca kayıt defteri adını sağlayın. Adından **azurecr.io** attığınızdan emin olun. Kayıt defteri adını Azure portal [kapsayıcı kayıt defterleri sayfasında](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.ContainerRegistry%2Fregistries) bulabilirsiniz.

**Azure Container Registry Için Yönetici Kullanıcı adı**: kapsayıcı görüntünüzü içeren Azure Container Registry ilişkili [Yönetici Kullanıcı adını](https://docs.microsoft.com/azure/container-registry/container-registry-authentication#admin-account) sağlayın. Şirketinizin kayıt defterine erişiminin olduğundan emin olmak için Kullanıcı adı ve parola gereklidir. Yönetici Kullanıcı adını ve parolasını almak için, Azure komut satırı arabirimi 'ni (CLı) kullanarak **yönetici özellikli** özelliği **true** olarak ayarlayın. İsteğe bağlı olarak, Azure portal **yönetici kullanıcıyı** **etkinleştirmek** için ayarlayabilirsiniz.

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-admin-user.png" alt-text="Kapsayıcı kayıt defterini Güncelleştir iletişim kutusunu gösterir.":::

**Azure Container Registry parolası**: Azure Container Registry ilişkili Yönetici Kullanıcı adı için parola belirtin ve kapsayıcı görüntünüzü içerir. Şirketinizin kayıt defterine erişiminin olduğundan emin olmak için Kullanıcı adı ve parola gereklidir. Azure Portal parolayı **Container Registry** > **erişim anahtarlarına** giderek veya [göster komutunu](https://docs.microsoft.com/cli/azure/acr/credential?view=azure-cli-latest#az-acr-credential-show) kullanarak Azure CLI ile öğrenebilirsiniz.

:::image type="content" source="media/iot-edge-module-creation/iot-edge-module-username-password.png" alt-text="Azure portal erişim tuşu ekranını gösterir.":::

**Azure Container Registry Içinde Depo adı**. Yansımanıza sahip Azure Container Registry deposunun adını sağlayın. Görüntüyü kayıt defterine gönderdiğinizde deponun adını belirtirsiniz. Deponun adını [Container Registry](https://azure.microsoft.com/services/container-registry/) > **depoları sayfasına**giderek bulabilirsiniz. Daha fazla bilgi için bkz. [Azure Portal kapsayıcı kayıt defteri depolarını Görüntüleme](https://docs.microsoft.com/azure/container-registry/container-registry-repositories). Ad ayarlandıktan sonra, bunun değiştirilemeyeceğini unutmayın. Hesabınızdaki her teklif için benzersiz bir ad kullanın.

### <a name="image-tags-for-new-versions-of-your-offer"></a>Teklifinizin yeni sürümleri için görüntü etiketleri

Bir güncelleştirme yayımladığınızda müşterilerin Azure Marketi 'nden güncelleştirmeleri otomatik olarak alabilmesi gerekir. Güncelleştirmek istemlerse, görüntünüzün belirli bir sürümünde kalabilmeleri gerekir. Görüntüde her güncelleştirme yaptığınızda bunu yeni resim etiketleri ekleyerek yapabilirsiniz.

**Resim etiketi**. Bu alan, tüm desteklenen platformlarda görüntünüzün en son sürümüne işaret eden **en son** bir etiketi içermelidir. Ayrıca, sürüm etiketi de içermelidir (örneğin, xx. xx. xx ile başlayarak xx bir sayıdır). Müşteriler birden çok platformu hedeflemek için [bildirim etiketlerini](https://github.com/estesp/manifest-tool) kullanmalıdır. Bir bildirim etiketi tarafından başvurulan tüm Etiketler de karşıya yüklenebilmemiz için eklenmelidir. Tüm bildirim etiketleri (en son etiketi hariç), X. Y-veya X. Y. Z ile başlamalı; burada X, Y ve Z tamsayılardır. Örneğin, en son bir etiket 1.0.1-Linux-x64, 1.0.1-Linux-ARM32 ve 1.0.1-Windows-ARM32 ' y i gösteriyorsa, bu altı etiket bu alana eklenmelidir. Etiketler ve sürüm oluşturma hakkında daha fazla bilgi için bkz [. IoT Edge modülünüzü hazırlama teknik varlıkları.](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/iot-edge-module/cpp-create-technical-assets)

### <a name="default-deployment-settings-optional"></a>Varsayılan dağıtım ayarları (isteğe bağlı)

IoT Edge modülünüzü dağıtmak için en yaygın ayarları tanımlayın. Bu varsayılan ayarlarla IoT Edge modülünüzü kullanıma hazır hale getirerek müşteri dağıtımlarını iyileştirin.

**Varsayılan yollar**. IoT Edge hub, modüller, IoT Hub ve cihazlar arasındaki iletişimi yönetir. Modüller ve IoT Hub arasında veri girişi ve çıkış için yollar ayarlayabilir ve bu, iletileri işlemek veya ek kod yazmak için ek hizmetlere gerek kalmadan gitmeleri gereken yerlere ileti gönderme esnekliği sağlar. Yollar ad/değer çiftleri kullanılarak oluşturulur. En fazla beş varsayılan yol adı tanımlayabilirsiniz, her biri 512 karakter uzunluğunda olabilir.

Rota değerindeki doğru [yol sözdizimini](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes) kullandığınızdan emin olun (genellikle from/Message/* $upstream olarak tanımlanmıştır). Diğer bir deyişle, herhangi bir modül tarafından gönderilen tüm iletiler IoT Hub gider. Modülize başvurmak için, varsayılan modül adını kullanın, bu ad, boşluk veya özel karakterler olmadan **teklif adınız**olacaktır. Henüz bilinen diğer modüllere başvurmak için, <FROM_MODULE_NAME> kuralını kullanarak müşterilerinizin bu bilgileri güncelleştirmesi gerektiğini bilmesini sağlayın. IoT Edge yolları hakkında daha fazla bilgi için bkz. [yolları bildirme](https://docs.microsoft.com/azure/iot-edge/module-composition#declare-routes).

Örneğin, ContosoModule, ContosoOutput 'da Contosoınput ve Output verilerinde giriş yapıyorsa, aşağıdaki iki varsayılan yolu tanımlamak mantıklı olur:

- Ad #1: ToContosoModule
- Değer #1:/messages/modules/<FROM_MODULE_NAME>/Outputs/* BrokeredEndpoint ("/modules/ContosoModule/inputs/ContosoInput")
- Ad #2: FromContosoModuleToCloud
- Değer #2:/messages/modules/ContonsoModule/outputs/ContosoOutput 'den $upstream

**Varsayılan modül ikizi istenen özellikleri**. Modül ikizi, istenen özellikler dahil olmak üzere bir modül örneği için durum bilgilerini depolayan IoT Hub JSON belgesidir. İstenen özellikler, modül yapılandırmalarını veya koşullarını eşitlemeye yönelik bildirilen özelliklerle birlikte kullanılır. Çözüm arka ucu istenen özellikleri ayarlayabilir ve modül bunları okuyabilir. Modül, istenen özelliklerde değişiklik bildirimleri de alabilir. İstenen özellikler en fazla beş ad/değer çifti kullanılarak oluşturulur ve her varsayılan değer 512 karakterden az olmalıdır. En fazla beş ad/değer ikizi istenen özellikleri tanımlayabilirsiniz. İkizi Desired özelliklerinin değerleri, en yüksek iç içe geçmiş dört düzey hiyerarşisine sahip diziler olmadan geçerli bir JSON, kaçışsız olmalıdır. Varsayılan değer için gereken bir parametrenin anlamlı olmadığı bir senaryoda (örneğin, bir müşterinin sunucusunun IP adresi), varsayılan değer olarak bir parametre ekleyebilirsiniz. İkizi istenen özellikler hakkında daha fazla bilgi edinmek için bkz. [istenen özellikleri tanımlama veya güncelleştirme](https://docs.microsoft.com/azure/iot-edge/module-composition#define-or-update-desired-properties).

Örneğin, bir modül ikizi istenen özellikleri kullanarak dinamik olarak yapılandırılabilir yenileme oranını destekliyorsa, aşağıdaki varsayılan ikizi istenen özelliği tanımlamak mantıklı olur:

- Ad #1: RefreshRate
- Değer #1:60

**Varsayılan ortam değişkenleri**. Ortam değişkenleri, yapılandırma işlemine yardımcı olan bir modüle ek bilgiler sağlar. Ortam değişkenleri ad/değer çiftleri kullanılarak oluşturulur. Her varsayılan ortam değişkeni adı ve değeri 512 karakterden az olmalıdır ve en fazla beş karakter tanımlayabilirsiniz. Varsayılan bir değer için gereken bir parametre anlamlı olmadığında (örneğin, bir müşterinin sunucusunun IP adresi), varsayılan değer olarak bir parametre ekleyebilirsiniz.

Örneğin, bir modülün başlamadan önce kullanım koşullarını kabul etmesi gerekiyorsa, aşağıdaki ortam değişkenini tanımlayabilirsiniz:

- Ad #1: ACCEPT_EULA
- Değer #1: Y

**Varsayılan kapsayıcı oluşturma seçenekleri**. Kapsayıcı oluşturma seçenekleri IoT Edge modülü Docker kapsayıcısının oluşturulmasını doğrudan yönlendirir. IoT Edge Docker Engine API 'SI kapsayıcı oluşturma seçeneklerini destekler. [Liste kapsayıcılarındaki](https://docs.docker.com/engine/api/v1.30/#operation/ContainerList) tüm seçeneklere bakın. Oluşturma seçenekleri alanı geçerli bir JSON, kaçışsız ve 512 karakterden az olmalıdır.

Örneğin, bir modül bağlantı noktası bağlamayı gerektiriyorsa, aşağıdaki oluşturma seçeneklerini tanımlayın:

"HostConfig": {"PortBindings": {"5012/TCP": [{"HostPort": "5012"}]}

## <a name="review-and-publish"></a>İnceleme ve yayımlama

Teklifin tüm gerekli bölümlerini tamamladıktan sonra, bunu gözden geçirmek ve yayımlamak için gönderebilirsiniz.

Portalın sağ üst köşesinde, **gözden geçir ve Yayımla**' yı seçin.

İnceleme sayfasında yayımlama durumunu görebilirsiniz:

- Teklifin her bölümü için tamamlanma durumuna bakın. Teklifin tüm bölümleri Tamam olarak işaretlenene kadar yayımlayamazsınız.
    - **Başlatılmamış** -bölüm başlatılmamış ve tamamlanması gerekiyor.
    - **Tamamlanmamış** -bölümde düzeltilmesi gereken hatalar var veya daha fazla bilgi sağlamanız gerekiyor. Rehberlik için bu belgenin önceki bölümlerine bakın.
    - **Tamamlandı** -bölümde tüm gerekli veriler var ve hata yok. Teklifi gönderebilmeniz için teklifin tüm bölümlerinin tamamlanmış olması gerekir.
- Teklifinizin doğru şekilde test edilmesini sağlamak için sertifika ekibine test yönergeleri sağlayın. Ayrıca, teklifinizi anlamak için yararlı olan tüm ek notları sağlayın.

Teklifi yayımlamaya göndermek için **Yayımla**' yı seçin.

Teklifin bir önizleme sürümünün gözden geçirilmesi ve onaylanmak üzere kullanılabilir olduğunu size bildirmek için size bir e-posta göndereceğiz. Teklifinizi herkese (veya özel bir teklif, özel bir hedef kitleye) yayımlamak için Iş Ortağı Merkezi ' ne gidin ve **Git-Live**' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

- [Ticari Market 'te mevcut bir teklifi güncelleştirme](https://docs.microsoft.com//azure/marketplace/partner-center-portal/update-existing-offer)