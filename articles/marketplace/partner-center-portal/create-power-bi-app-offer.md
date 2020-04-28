---
title: Power BI uygulama teklifi oluşturun - Azure Marketi
description: Microsoft AppSource'a bir Power BI uygulaması teklifi oluşturma ve yayımlama hakkında bilgi edinin.
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/02/2020
ms.openlocfilehash: dc036ae3cba6aa4d3a689562afffb991fadc8c0a
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81867591"
---
# <a name="create-a-power-bi-app"></a>Power BI uygulaması oluşturma

> [!IMPORTANT]
> Power BI uygulama tekliflerinizin yönetimini Cloud Partner Portal'dan İş Ortağı Merkezi'ne taşıyoruz. Tekliflerin izne gelene kadar, lütfen tekliflerinizi yönetmek için Bulut İş Ortağı Portalı için [Power BI uygulama teklifi oluştur'daki](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/power-bi/cpp-create-offer) yönergeleri izleyin.

Bu makalede, Microsoft [AppSource'a](https://appsource.microsoft.com/)bir Power BI uygulaması teklifinin nasıl oluşturulup yayımlandırılabildiğini açıklanmaktadır.

Power BI uygulama teklifi oluşturmadan önce, İş Ortağı Merkezi'nde ticari bir pazar hesabınız olması gerekir. Henüz bir tane oluşturmadıysanız, İş Ortağı Merkezi hesabında [Ticari Pazar Hesabı Oluşturma'ya](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account) bakın.

## <a name="create-a-new-offer"></a>Yeni teklif oluşturma

1. [Ortak Merkezi'nde](https://partner.microsoft.com/dashboard/home)oturum açın.
2. Soldaki gezinme menüsünde **Ticari Pazara** > **Genel Bakış'ı**seçin.

    :::image type="content" source="media/power-bi-menu-overview.png" alt-text="ticari pazar menüsüne genel bakış" border="false":::

3. Select **+ Yeni teklif** > **Power BI App**. **Yeni teklif** iletişim kutusu görüntülenir.

> [!IMPORTANT]
> Power **BI App** seçeneği&#39;gösterilmemişse veya&#39;etkin değilse, hesabınızın bu teklif türünü oluşturma izni&#39;. Lütfen bu teklif türü için geliştirici hesabı için kayıt dahil olmak üzere tüm [gereksinimleri](create-power-bi-app-overview.md) karşıladığınızdan&#39;kontrol edin

### <a name="offer-id-and-alias"></a>Teklif kimliği ve takma ad

Teklif **Kimliği**girin. Bu, hesabınızdaki her teklif için benzersiz bir tanımlayıcıdır.

- Bu kimlik, pazar teklifi ve varsa Azure Kaynak Yöneticisi şablonlarının web adresindeki müşteriler tarafından görülebilir.
- Yalnızca küçük harfleri ve rakamları kullanın. Tireler ve alt çizerler içerebilir, ancak boşluk yoktur ve 50 karakterle sınırlıdır. Örneğin, **test-teklif-1** girerseniz, teklif web adresi `https://azuremarketplace.microsoft.com/marketplace/../test-offer-1`.

- **Teklif Kimliği, Oluştur'u**seçtikten sonra değiştirilemez.

Teklif **takma adını**girin. Bu, Ortak Merkezi'ndeki teklifi ifade etmek için kullanılan addır.

- Bu ad pazarda kullanılmaz ve teklif adı ve müşterilere gösterilen diğer değerlerden farklıdır.
- **Oluştur'u**seçtikten sonra bu değişiklik edilemez.

Bu iki değeri girdikten sonra Teklif'e genel bakış sayfasına devam etmek için **Oluştur'u** seçin.

## <a name="offer-overview"></a>Teklife genel bakış

**Teklife genel bakış** sayfası, bu teklifi yayımlamak için gereken adımların (hem tamamlanmış hem de yaklaşan) ve her adımın tamamlanmasının ne kadar sürmesi gerektiğinin görsel bir gösterimini gösterir.

Yaptığınız seçime bağlı olarak bu teklifle ilgili işlemleri gerçekleştirmek için bağlantılar içerir. Örneğin:

- Teklif taslaksa - [Taslak teklifi sil](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#delete-a-draft-offer)
- Teklif canlıise - [Teklifi satmayı bırakın](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#stop-selling-an-offer-or-plan)
- Teklif önizlemedeyse - [Canlı Yayına Geçin](https://docs.microsoft.com/azure/marketplace/partner-center-portal/publishing-status#publisher-approval)
- Yayımcı oturum açmayı tamamlamadıysanız - [Yayımlamayı iptal et](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#cancel-publishing)

## <a name="offer-setup"></a>Teklif kurulumu

Teklifinizi ayarlamak için aşağıdaki adımları izleyin.

### <a name="connect-lead-management"></a>Müşteri adayı yönetimini bağlayın

Teklifinizi İş Ortağı Merkezi ile pazara yayınlarken, teklifi Müşteri İlişkileri Yönetimi (CRM) sisteminize bağlamanız gerekir. Bu, birisi ürününüzün ilgisini ifade eder etmez veya ürününüzü kullanır kullanmaz müşteri iletişim bilgilerini almanızı sağlar.

1. Potansiyel müşterileri göndermemizi istediğiniz müşteri adayı hedefini seçin. İş Ortağı Merkezi aşağıdaki CRM sistemlerini destekler:

    - Müşteri Katılımı için [Dynamics 365](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-dynamics)
    - [Marketo](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-marketo)
    - [Salesforce](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-salesforce)

    > [!NOTE]
    > CRM sisteminiz yukarıda listelenmemişse, müşteri müşteri adayı verilerini depolamak için [Azure Tablosu](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-azure-table) veya [Https Endpoint'i](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-lead-management-instructions-https) kullanın. Ardından verileri CRM sisteminize aktarın.

2. İş Ortağı Merkezi'nde yayın yaparken teklifinizi müşteri adayı hedefe bağlayın.
3. Müşteri adayı hedefine bağlantının düzgün şekilde yapılandırıldığından onaylayın. İş Ortağı Merkezi'nde yayımladıktan sonra bağlantıyı doğrular ve size bir test ipucu göndeririz. Teklifi yayına girmeden önce önizlemenize kadar, teklifi önizleme ortamında kendiniz satın almaya çalışarak müşteri adayı bağlantınızı da test edebilirsiniz.
4. Müşteri adayını kaybetmemek için müşteri adayı hedefine bağlantının güncel kaldığından emin olun.

Aşağıda bazı ek müşteri adayı yönetim kaynakları vereme

- [Müşteri adayı yönetimine genel bakış](https://docs.microsoft.com/azure/marketplace/partner-center-portal/commercial-marketplace-get-customer-leads)
- [Müşteri adayı yönetimi hakkında SSS](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#frequently-asked-questions)
- [Sık karşılaşılan müşteri adayı yapılandırma hataları](https://docs.microsoft.com/azure/marketplace/lead-management-for-cloud-marketplace#common-lead-configuration-errors-during-publishing-on-cloud-partner-portal)
- [Müşteri Adayı Yönetimine Genel Bakış](https://assetsprod.microsoft.com/mpn/cloud-marketplace-lead-management.pdf) PDF (açılır pencere engelleyicinizin kapalı olduğundan emin olun)

Özellikler adlı bir sonraki bölüme devam etmeden önce **taslağı kaydet'i** seçin.

## <a name="properties"></a>Özellikler

Bu sayfa, teklifinizi pazarda, uygulama sürümünüzde ve teklifinizi destekleyen yasal sözleşmelerde gruplandırmak için kullanılan kategorileri ve sektörleri tanımlamanıza olanak tanır.

### <a name="category"></a>Kategori

En az bir ve en fazla üç kategori seçin. Bu kategoriler, teklifinizi uygun pazar yeri arama alanlarına yerleştirmek için kullanılır ve teklif ayrıntıları sayfanızda gösterilir. Teklif açıklamasında, teklifinizin bu kategorileri nasıl desteklediğini açıklayın.

### <a name="industry"></a>Endüstri

İsteğe bağlı olarak, her endüstrinin altında en fazla iki endüstri ve iki dikey seçin. Teklifinizi görüntülemek için kategoriler kullanılırken, arama filtrelerinde endüstri ve dikey ler kullanılır ve Storefront'da uygulanır. Teklifiniz belirli bir endüstriyi ve/veya dikeyi hedefliyorsa, teklifinizin seçilen sektörleri veya dikeyleri nasıl desteklediğini açıklamak için teklif açıklamasını kullanın. Teklifiniz sektöre özel&#39;değilse, bu bölümü boş bırakın.

> [!NOTE]
> Biz teklif keşif deneyimini geliştirmek için yeni sanayi ve dikey tanıtmak için çalışırken, bazı sanayi veya dikey henüz Storefront görünür olmayabilir. (*) ile işaretlenmiş endüstriler ve dikeyler ileribir tarihte kullanıma sunulacaktır. Yayınlanan tüm teklifler anahtar kelime araması ile keşfedilebilir.
<p>&nbsp;

| **Endüstri** | **Alt Sanayi** |
| --- | --- |
| *Otomotiv | *Otomotiv |
| Tarım | *Diğer - Segmentsiz |
| Dağıtım | *Toptan Satış<br>Koli ve Paket Nakliye |
| Eğitim | *Yüksek<br> Öğretim* İlk ve Orta Öğretim / K-12<br>*Kütüphaneler ve Müzeler |
| Finansal Hizmetler | *Bankacılık ve<br> Sermaye Piyasaları* Sigorta |
| Devlet | *Savunma ve İstihbarat (Ulusal ve Kamu<br> Güvenliği olarak adlandırılır)* Kamu Güvenliği ve Adalet<br>*Sivil Hükümet |
| Sağlık (Sağlık denir) | *Sağlık Payor<br> * Sağlık Kuruluşu<br>*İlaç |
| Üretim ve Kaynaklar (Eskiden İmalat olarak adlandırılır) | *Kimyasal ve Tarımsal<br> Kimyasal* Ayrık İmalat<br>*Enerji |
| Perakende ve Tüketim Malları (eskiden Perakende olarak adlandırılır) | *Tüketim<br> Malları* Perakendeci |
| *Medya ve İletişim (Medya ve Eğlence olarak adlandırılır) | *Medya ve<br> Eğlence* Telekomünikasyon |
| Profesyonel Hizmetler | *Yasal<br> * Ortak Profesyonel Hizmetler |
| *Mimarlık ve İnşaat (Mimarlık Mühendisliği olarak adlandırılır) | *Diğer - Segmentsiz |
| *Misafirperverlik ve Seyahat | *Oteller ve<br> Eğlence* Seyahat ve Ulaşım<br>*Restoranlar ve Yemek Hizmetleri |
| *Diğer Kamu Sektörü Sektörleri | *Ormancılık ve<br> Balıkçılık* Kar amacı gütme -yen |
| *Emlak | *Diğer - Segmentsiz |

### <a name="legal"></a>Yasal Bildirim

#### <a name="terms-and-conditions"></a>hüküm ve koşullar

Kendi özel hüküm ve koşullarınızı sağlamak **için, Şartlar ve Koşullar** kutusuna en fazla 10.000 karakter girin. Hüküm ve koşullarınız daha uzun bir açıklama gerektiriyorsa, bulunabilecekleri yerlere tek bir web bağlantısı girin. Bu aktif bir bağlantı olarak müşterilere görüntülenir.

Müşteriler teklifinizi denemeden önce bu koşulları kabul etmelidir.

Bir sonraki bölüme, Teklif listesine devam etmeden önce **taslağı Kaydet'i** seçin.

## <a name="offer-listing"></a>Teklif listesi

Burada pazarda görüntülenen teklif ayrıntılarını tanımlarsınız. Bu teklif adı, açıklama, görüntüler ve benzeri içerir.

### <a name="language"></a>Dil

Teklifinizin listeleneceği dili seçin. Şu anda, **İngilizce (AMERIKA Birleşik Devletleri)** tek seçenektir.

Her dil/pazar için pazar ayrıntılarını (teklif adı, açıklama ve resimler gibi) tanımlayın. Bu bilgileri sağlamak için dil/pazar adını seçin.

> [!NOTE]
> Teklif açıklaması ifadeyle başlarsa teklif ayrıntılarının İngilizce olması &quot;gerekmez, bu uygulama yalnızca [İngilizce olmayan dilde] kullanılabilir. &quot; Teklif listesinde kullanılandan farklı&#39;bir dilde içerik sunmak için Yararlı Bir Bağlantı sağlamanın da sakınca&#39;.

### <a name="name"></a>Adı

Buraya girdiğiniz ad, teklifinizin başlığı olarak görüntülenir. Bu alan, teklifi oluşturduğunuzda Teklif **takma adı** kutusuna girdiğiniz metinle önceden doldurulmuştur. Bu adı daha sonra değiştirebilirsiniz.

Adı:

- Ticari marka olabilir (ve ticari marka veya telif hakkı sembolleri dahil edebilirsiniz).
- 50 karakterden uzun olamaz.
- Emojileri içeremiyorum.

### <a name="search-results-summary"></a>Arama sonuçları özeti

Teklifinizin kısa bir açıklamasını sağlayın. Bu, 100 karaktere kadar uzun olabilir ve pazar yeri arama sonuçlarında kullanılır.

### <a name="description"></a>Açıklama

Teklifinizin 3.000 karaktere kadar daha uzun bir açıklamasını sağlayın. Bu, pazar yeri giriş özetinde müşterilere görüntülenir.

Açıklamanıza aşağıdakilerden birini veya birkaçını ekleyin:

- Teklifinizin sağladığı değer ve önemli avantajlar.
- Kategori veya endüstri dernekleri veya her ikisi.
- Uygulama içi satın alma fırsatları.
- Gerekli açıklamalar.

Açıklamanızı yazmak için bazı ipuçları aşağıda verilmiştir:

- Teklifinizin değerini açıklamanızın ilk birkaç cümlesinde açıkça açıklayın. Aşağıdaki öğeleri ekleyin:
  - Teklifin açıklaması.
  - Tekliften yararlanan kullanıcı türü.
  - Müşteri ihtiyaçları veya teklif adreslerini sorunları.
- İlk birkaç cümlenin arama sonuçlarında görüntülenebileceğini unutmayın.
- Ürününüzü satmak için özelliklere ve işlevselliğe güvenmeyin. Bunun yerine, teklifinizin sağladığı değere odaklanın.
- Sektöre özgü kelime dağarcığı veya fayda tabanlı ifadeler kullanmayı deneyin.

Teklifaçıklamanızı **Description** daha ilgi çekici hale getirmek için, açıklamanızı biçimlendirmek için zengin metin düzenleyicisini kullanın. Zengin metin düzenleyicisi, açıklamanızı daha okunabilir hale getirmek için sayılar, madde işaretleri, kalın, italik ler ve girintiler eklemenizi sağlar.

:::image type="content" source="media/power-bi-rich-text-editor.png" alt-text="zengin metin editörü gösterir" border="false":::

### <a name="search-keywords"></a>Anahtar kelimeleri ara

Müşterilerin teklifinizi pazarda bulmasına yardımcı olmak için en fazla üç isteğe bağlı arama anahtar kelimesini girin. En iyi sonuçlar için, bu anahtar kelimeleri açıklamanızda da kullanın.

### <a name="helpprivacy-web-addresses"></a>Yardım/Gizlilik Web adresleri

Müşterilerin teklifinizi daha iyi anlamalarına yardımcı olacak bağlantılar sağlayın.

#### <a name="help-link"></a>Yardım bağlantısı

Müşterilerin teklifiniz hakkında daha fazla bilgi edinebileceği web adresini girin.

#### <a name="privacy-policy-url"></a>Gizlilik ilkesi URL'si

Kuruluşunuzun gizlilik ilkesine web adresini girin. Teklifinizin gizlilik yasalarına ve yönetmeliklerine uygun olmasını sağlamaksizin sorumlusunuz. Ayrıca web sitenizde geçerli bir gizlilik politikası yayınlamaksizin de sorumlusunuz.

### <a name="contact-information"></a>İletişim Bilgileri

**Destek kişisi** ve Mühendislik **kişisi**için adı, e-postayı ve telefon numarasını sağlamanız gerekir. Bu bilgiler müşterilere gösterilmeden&#39;. Microsoft tarafından kullanılabilir ve Bulut Çözüm Sağlayıcısı (CSP) iş ortaklarına sağlanabilir.

- Destek kişisi (gerekli): Genel destek soruları için.
- Mühendislik iletişimi (gerekli): Teknik sorular ve belgelendirme sorunları için.
- CSP Programı iletişim bilgileri (isteğe bağlı): CSP programıyla ilgili bayi soruları için.

Destek **iletişim** bölümünde, ortakların teklifiniz için destek bulabilecekleri **Destek web sitesinin** web adresini girin.

### <a name="supporting-documents"></a>Destekleyici belgeler

PDF formatında en az bir ve en fazla üç ilgili pazarlama belgesi sağlayın. Örneğin, teknik incelemeler, broşürler, denetim listeleri veya sunular.

### <a name="marketplace-images"></a>Pazar yeri görüntüleri

Teklifinizle birlikte kullanılacak logolar ve resimler sağlayın. Tüm görüntüler .png formatında olmalıdır. Bulanık görüntüler reddedilir.

>[!Note]
>Dosya yükleme sorunu yaşıyorsanız, yerel ağınızın İş Ortağı https://upload.xboxlive.com Merkezi tarafından kullanılan hizmeti engellemediğinden emin olun.

#### <a name="store-logos"></a>Mağaza logoları

Küçük (48 x 48 piksel) ve **Small** **Büyük** (216 x 216 piksel) olmak üzere iki boyutta&#39;teklifinizin .png dosyalarını sağlayın.

Her iki logo da gereklidir ve pazar yeri listesinde farklı yerlerde kullanılır.

#### <a name="screenshots"></a>Ekran görüntüleri

Teklifinizin nasıl çalıştığını gösteren en az bir ve en fazla beş ekran görüntüsü ekleyin. Her biri 1280 x 720 piksel boyutunda ve .png formatında olmalıdır.

#### <a name="videos-optional"></a>Videolar (isteğe bağlı)

Teklifinizi gösteren en fazla beş video ekleyin. Video&#39;nun adını, web adresini ve videonun küçük resmi .png görüntüsünü 1280 x 720 piksel boyutunda girin.

#### <a name="additional-marketplace-listing-resources"></a>Ek pazar yeri listeleme kaynakları

Teklif listeleri oluşturma hakkında daha fazla bilgi edinmek için, [en iyi uygulamaları listele'ye](https://docs.microsoft.com/azure/marketplace/gtm-offer-listing-best-practices)bakın.

## <a name="technical-configuration"></a>Teknik yapılandırma

Power BI Service'de uygulamanızı üretime yükseltin ve müşterilerin uygulamanızı yüklemesini sağlayan Power BI uygulama yükleyici bağlantısını sağlayın. Daha fazla bilgi için bkz. [Power BI'de panolar ve raporlar içeren uygulamaları yayımla.](https://docs.microsoft.com/power-bi/service-create-distribute-apps)

## <a name="supplemental-content"></a>Ek içerik

Teklifiniz hakkında, teklifi doğrulamamıza yardımcı olacak ek bilgiler sağlayın. Bu bilgiler müşterilere gösterilmeden&#39;veya pazara yayınlanmaz.

### <a name="validation-assets"></a>Doğrulama varlıkları

İsteğe bağlı olarak, Microsoft doğrulama ekibinin uygulamanızı yapılandırmasına, bağlamasına ve test etmesini sağlamak için yönergeler (en fazla 3.000 karakter) ekleyin. Veri Bağla seçeneğini sınamak için kullanılabilecek tipik yapılandırma ayarlarını, hesapları, parametreleri veya diğer bilgileri ekleyin. Bu bilgiler yalnızca doğrulama ekibi tarafından görülebilir ve yalnızca doğrulama amacıyla kullanılır.

## <a name="review-and-publish"></a>İnceleme ve yayınlama

Teklifin gerekli tüm bölümlerini&#39;tamamladıktan sonra, teklifinizi incelemek ve yayınlamak için gönderebilirsiniz.

Portalın sağ üst köşesinde, **Gözden Geçir'i seçin ve yayınlayın.**

İnceleme sayfasında şunları yapabilirsiniz:

- Teklifin her bölümü için tamamlanma durumuna bakın. Teklifin tüm bölümleri tamamlanana kadar yayımlama&#39;yapamazsınız.
  - **Başlatılmadı** - Bölüm başlatılmadı ve tamamlanması gerekiyor.
  - **Eksik** - Bölümde düzeltilmesi gereken veya daha fazla bilgi sağlamanızı gerektiren hatalar vardır. Rehberlik için bu belgedeki önceki bölümlere bakın.
  - **Tam** - Bölümde gerekli tüm veriler vardır ve herhangi bir hata yoktur. Teklifi sunamadan önce teklifin tüm bölümlerinin tamamlanmış olması gerekmektedir.
- Uygulamanızın doğru test edildiğinden emin olmak için sertifika ekibine test talimatları sağlayın. Ayrıca, teklifinizi anlamak için yararlı olan ek notlar sağlayın.

Yayımlama teklifini göndermek için **Yayımla'yı**seçin.

Teklifin önizleme sürümünün gözden geçirilip onaylamayacağını bildirmek için size bir e-posta göndeririz. Teklifinizi halka (veya özel bir teklifte özel bir hedef kitleye) yayınlamak için Ortak Merkezi'ne gidin ve **Go-live'ı**seçin.
