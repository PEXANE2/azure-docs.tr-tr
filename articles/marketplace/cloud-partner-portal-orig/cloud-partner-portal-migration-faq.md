---
title: Ortak Merkezi'ne geçiş hakkında sık sorulan sorular | Azure Marketi
description: Bu makalede, Bulut İş Ortağı Portalı'ndan İş Ortağı Merkezi'ne tekliflerin geçişi yle ilgili sık sorulan sorular ele alilmektedir.
author: anbene
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/10/2020
ms.author: mingshen
ms.openlocfilehash: 672153eba4aa2b739b67694f939c4796b39ac4c6
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81274386"
---
# <a name="frequently-asked-questions-for-migrating-from-the-cloud-partner-portal-to-partner-center"></a>Bulut İş Ortağı Portalı'ndan İş Ortağı Merkezi'ne geçiş için sık sorulan sorular

Bu makalede, Bulut İş Ortağı Portalı'ndan İş Ortağı Merkezi'ne tekliflerin geçişi yle ilgili sık sorulan sorular ele alilmektedir.

## <a name="what-does-offer-migration-mean"></a>Teklif göçü ne anlama gelir?

Teklif yayımlama ve yönetim deneyimindeki değişikliklerle teklif verilerinizi Bulut İş Ortağı Portalı'ndan İş Ortağı Merkezi'ne taşıyoruz.

| Alan  | Değişiklikler  |
|-------|----------|
| **Yayıncılık ve teklif yönetimi deneyimi** | İş Ortağı Merkezi'nde sezgisel bir arayüzle gelişmiş bir kullanıcı deneyimiyaşayacaksınız. Daha fazla bilgi için İş [Ortağı Merkezi ile Bulut İş Ortağı Portalı arasındaki farklar nelerdir?](#what-are-the-differences-between-partner-center-and-the-cloud-partner-portal) |
| **Tekliflerinizin pazarda kullanılabilirliği** | Değişiklik yok. Teklifiniz pazarda canlı ysa, geçiş sırasında ve sonrasında canlı olmaya devam edecektir. |
| **Yeni satın almalar ve dağıtımlar** | Değişiklik yok. Müşterileriniz tekliflerinizi kesintisiz olarak satın almaya ve dağıtmaya devam edecektir. |
| **Ödeme** | Geçiş sırasında veya sonrasında gerçekleşen tüm satın almalar ve dağıtımlar size normal şekilde ödenmeye devam eder. |
|**Mevcut Bulut İş [Ortağı Portal API'leri](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) ile API tümleştirmeleri** | Mevcut Bulut İş Ortağı Portalı API'leri geçişten sonra desteklenmeye devam eder ve varolan tümleştirmeleriniz çalışmaya devam eder. Daha fazla bilgi için bulut [iş ortağı portalı REST API'leri geçiş sonrası desteklenecek mi?](#will-the-cloud-partner-portal-rest-apis-be-supported-post-migration) |
| | |

## <a name="can-i-still-access-the-cloud-partner-portal-and-manage-my-offers-during-migration"></a>Bulut İş Ortağı Portalı'na erişebilir ve geçiş sırasında tekliflerimi yönetebilir miyim?

Teklifler, Mayıs ayı boyunca 13 Nisan'dan itibaren Ortak Merkezi'ne iletilir. Bu süre zarfında, sunduğunuz geçiş için zamanlanmış olması dışında Bulut İş Ortağı Portalı'na her zamanki gibi erişebilirsiniz.
Tekliflerin izlenirken, aşağıdaki ekran görüntüsünde görüldüğü gibi "Kilitli – İş Ortağı Merkezi'ne taşınma" durumuna sahip olurlar. Bu geçiş süresi 24 saatten az olmalıdır. Bu süre zarfında, tekliflerinizi güncellenemezsiniz. Tekliflerinizi ilettikten sonra bir e-posta bildirimi alırsınız.

:::image type="content" source="media/migration-faq/all-offers-1.png" alt-text="Geçirilen tekliflerin durumunu gösterir.":::

Teklifleriniz geçirildikten sonra, Bulut İş Ortağı Portalı'nda **sınırlı bir süre için** salt okunur modunda olurlar. Durumları "İş Ortağı Merkezi'ne Taşınır" gösterilir ve aşağıdaki ekran görüntülerinde gösterildiği gibi İş Ortağı Merkezi'ndeki teklifinize bir bağlantı içerir. Bu noktadan itibaren, tüm tekliflerinizi yönetecek veya yalnızca İş Ortağı Merkezi aracılığıyla yeni teklifler oluşturacaksınız,

:::image type="content" source="media/migration-faq/all-offers-2.png" alt-text="Ortak Merkezi'ne geçirilen teklifler için verilen iletiyi gösterir":::

:::image type="content" source="media/migration-faq/offer-has-moved.png" alt-text="Geçirilen bir teklif için Bulut İş Ortağı Portalı sayfasını gösterir.":::

## <a name="how-will-i-create-new-offers"></a>Yeni teklifleri nasıl oluşturacağım?

Bulut İş Ortağı Portalı'ndan, İş Ortağı Merkezi'nde yeni teklifler oluşturmaya yönlendirileceksiniz

:::image type="content" source="media/migration-faq/create-new-offer-1.png" alt-text="Bulut İş Ortağı Portalı'nda yeni bir teklif oluşturmak için menüyü gösterir":::

Yeni bir teklif seçtikten sonra aşağıdaki gibi bir ileti görürsünüz.

:::image type="content" source="media/migration-faq/create-new-offer-2.png" alt-text="CPP'de yeni bir teklif oluştururken alınan iletiyi gösterir":::

## <a name="do-i-need-to-create-a-new-account-to-manage-offers-in-partner-center"></a>İş Ortağı Merkezi'ndeki teklifleri yönetmek için yeni bir hesap oluşturmam gerekiyor mu?

Hayır. Temel hesabınız korunacaktır ve zaten Ortak Merkezi'nde yönetiyor olmalısınız. Bu, varolan bir iş ortağıysanız, geçiş sonrası İş Ortağı Merkezi'nde oturum açmak için mevcut Cloud İş Ortağı Portalı hesap kimlik bilgilerinizi kullanabileceğiniz anlamına gelir. Yalnızca teklifler ve ilişkili yönetim deneyimi Bulut İş Ortağı Portalı'ndan İş Ortağı Merkezi'ne taşınıyor. Teklifleriniz yeni hesapla ilişkilendirilmeyeceğinden, yeni hesap oluşturmamanızı rica ediyoruz.

## <a name="i-see-a-message-in-the-cloud-partner-portal-to-activate-my-account-what-does-this-mean"></a>Hesabımı etkinleştirmek için Bulut İş Ortağı Portalı'nda bir ileti görüyorum, bu ne anlama geliyor?

Hesabınızı İş Merkezi'ne düzgün bir şekilde geçirmek ve teklif geçişi tamamlandıktan sonra İş Ortağı Merkezi'ndeki tekliflerinizi yönetmenize olanak sağlamak için sizden birkaç ayrıntı ya da bilgi almamız gerekir. Hesap etkinleştirme hakkında daha fazla bilgi için [Bulut İş Ortağı Portalı'ndan İş Ortağı Merkezi'ne Hesap geçişi](https://docs.microsoft.com/azure/marketplace/partner-center-portal/account-migration-from-cpp-to-pc)ne resleyiği ne resit.

Hesap etkinleştirme sini tamamlamak için gereken adımlar hesap rolünüze bağlı olarak değişir.

| Hesap rolü | Etkinleştirme adımları |
|--------------|----------------|
|Sahip | Bulut İş Ortağı Portalı'ndaki [Publisher Profile](https://cloudpartner.azure.com/#profile) sayfasına gidin ve ardından etkinleştirmek için banner'daki bağlantıyı tıklatın. Hesap etkinleştirmesini tamamlamak için Ortak Merkezi'ne yönlendirilirsiniz. |
| Katılımcı | Hesabı yalnızca sahip rolü olan bir kullanıcı etkinleştirebilir. Hesap etkinleştirme sini tamamlamak için hesap sahiplerine başvurun. Hesap sahipleriniz banner iletisinde listelenmelidir. |
| | |

## <a name="im-having-trouble-logging-in-to-my-account-and-opening-a-support-ticket"></a>Hesabıma giriş yaparken ve destek bileti açmakta sorun yaşıyorum

Hesabınızda oturum açamıyorsanız, bir [destek bileti](https://partner.microsoft.com/support/v2/?stage=1)açabilirsiniz.

## <a name="where-can-i-find-documentation-on-the-new-partner-center-publishing-experience"></a>Yeni İş Ortağı Merkezi yayımlama deneyimiyle ilgili belgeleri nereden bulabilirim?

[Ticari pazar belgeleri](https://docs.microsoft.com/azure/marketplace/)gidin. Ardından **İş Ortağı Merkezi'ndeki**  > Ticari Pazar Portalı'nı genişletin Her teklif türünü oluşturmak için yardım konularını görmek için**yeni bir teklif** oluşturun.

:::image type="content" source="media/migration-faq/marketplace-help-topics.png" alt-text="İş Ortağı Merkezi için yardım konularını gösterir":::

### <a name="what-are-the-differences-between-partner-center-and-the-cloud-partner-portal"></a>İş Ortağı Merkezi ile Bulut İş Ortağı Portalı arasındaki farklar nelerdir?

Bulut İş Ortağı Portalı ve İş Ortağı Merkezi arasındaki aşağıdaki farkları fark edebilirsiniz.

### <a name="modular-publishing-capabilities"></a>Modüler yayın özellikleri

İş Ortağı Merkezi, tüm güncelleştirmeleri her zaman aynı anda yayımlamak yerine yayımlamak istediğiniz değişiklikleri seçmenize olanak tanıyan modüler bir yayımlama seçeneği sağlar. Örneğin, aşağıdaki ekran görüntüsü, yayımlanmak üzere seçilen yalnızca **Özellikler** ve **Teklif Listesi**değişiklikleri olduğunu gösterir.

:::image type="content" source="media/migration-faq/review-and-publish-migration.png" alt-text="Önizleme ve Yayımlama sayfasını gösterir":::

Yayımlamadığınız güncelleştirmeler taslak olarak kaydedilir. Teklifinizi halka duyurmadan önce doğrulamak için teklif önizlemenizi kullanmaya devam edin.

### <a name="rich-text-format"></a>Zengin metin biçimi

Teklif Listesi ve Plan Listeleme sayfasındaki Zengin Metin Düzenleyicisi'ni kullanarak teklifinizi geliştirin ve açıklamanızı planlayın.

:::image type="content" source="media/migration-faq/rich-text-editor-migration.png" alt-text="Zengin metin düzenleyicisini gösterir":::

### <a name="enhanced-preview-options"></a>Geliştirilmiş önizleme seçenekleri

İş Ortağı Merkezi, geliştirilmiş filtreleme seçenekleriyle [karşılaştırma özelliği](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#compare-changes-to-marketplace-offers) içerir. Bu, teklifin önizleme ve canlı sürümleriyle karşılaştırma olanağı sağlar.

:::image type="content" source="media/migration-faq/enhanced-preview.png" alt-text="Karşılaştırma özelliğini gösterir":::

### <a name="branding-and-navigation-changes"></a>Marka ve navigasyon değişiklikleri

Bazı marka değişiklikleri fark edeceksiniz. Örneğin, "SK'ler" Ortak Merkezi'nde "Planlar" olarak damgalanır.

:::image type="content" source="media/migration-faq/plan-overview-migration.png" alt-text="Plana genel bakışı gösterir.":::

Ayrıca, Cloud Partner Portal'daki **Market** veya S**torefront Details** (Danışmanlık Hizmeti, Power BI uygulaması) sayfalarında sağladığınız bilgiler, İş Ortağı Merkezi'ndeki Teklif **listeleme** sayfasında toplanır.

:::image type="content" source="media/migration-faq/offer-listing-migration.png" alt-text="Teklif listeleme sayfasını gösterir.":::

Bulut İş Ortağı Portalı'ndaki tek bir sayfada SNU'lara sağladığınız bilgiler artık İş Ortağı Merkezi'ndeki birkaç sayfada toplanabilir:

* Sayfayı ayarlama yı planla
* Liste sayfasını planla
* Kullanılabilirlik sayfasını planla
* Bu ekran görüntüsünde gösterildiği gibi teknik yapılandırma sayfasını planlayın.

:::image type="content" source="media/migration-faq/tech-config-migration.png" alt-text="Plan teknik yapılandırma sayfasını gösterir.":::

Teklif kimliğiniz artık teklifin sol navigasyon çubuğunda gösterilir.

:::image type="content" source="media/migration-faq/offer-id-offer-overview.png" alt-text="Teklif kimliğiyle sol daki gezinme menüsünü gösterir.":::

### <a name="stop-selling-an-offer"></a>Teklif satmayı durdur

Pazardaki bir teklifi doğrudan İş Ortağı Portalı'ndan [satmayı durdurmayı](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#stop-selling-an-offer-or-plan) talep edebilirsiniz. Bu seçenek, teklifiniz için **Teklif genel bakış** sayfasında mevcuttur.

:::image type="content" source="media/migration-faq/stop-selling-migration.png" alt-text="Teklifi Durdur seçeneğiyle Teklife Genel Bakış sayfasını gösterir.":::

## <a name="what-pages-in-partner-center-correspond-to-pages-i-used-in-the-cloud-partner-portal"></a>İş Ortağı Merkezi'ndeki hangi sayfalar Bulut İş Ortağı Portalı'nda kullandığım sayfalarla karşılık gelir?

Aşağıdaki tablo, iki portal arasındaki ilgili bağlantıları gösterir.

| Sayfa | Bulut İş Ortağı Portalı bağlantısı | İş Ortağı Merkezi bağlantısı |
|------|---------------------------|---------------------|
| **Tüm teklifler sayfası** | https://cloudpartner.azure.com/#alloffers | https://partner.microsoft.com/dashboard/commercial-marketplace/overview |
| **Tüm yayımcılar sayfası** | https://cloudpartner.azure.com/#publishers | https://partner.microsoft.com/dashboard/account/v3/publishers/list |
| **Yayıncı profili** | https://cloudpartner.azure.com/#profile | https://partner.microsoft.com/dashboard/account/management |
| **Kullanıcılar sayfası** | https://cloudpartner.azure.com/#users | https://partner.microsoft.com/dashboard/account/usermanagement |
| **Geçmiş sayfası** | https://cloudpartner.azure.com/#history | Geçmiş özelliği, İş Ortağı Merkezi'nde henüz desteklenmedi. |
| **Öngörüler panosu** | https://cloudpartner.azure.com/#insights | https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary |
| **Ödeme raporu** | https://cloudpartner.azure.com/#insights/payout | https://partner.microsoft.com/dashboard/payouts/reports/incentivepayments |
| | |

## <a name="will-the-cloud-partner-portal-rest-apis-be-supported-post-migration"></a>Bulut İş Ortağı Portalı REST API'leri geçiş sonrası desteklenecek mi?

Bulut İş Ortağı Portalı API'leri İş Ortağı Merkezi ile entegre edilmiştir ve teklifleriniz İş Ortağı Merkezi'ne geçtikten sonra çalışmaya devam edecektir. Entegrasyon küçük değişiklikler sunar. Kodunuzu Ortak Merkezi'ne geçişten sonra çalışmaya devam etmesini sağlamak için aşağıdaki tablodaki değişiklikleri gözden geçirin.

| **API** | **Açıklamayı değiştir** | **Etki** |
| ------- | ---------------------- | ---------- |
| POST Yayınla, GoLive, İptal Et | Geçirilen teklifler için yanıt üstbilgisi farklı bir biçime sahip olur, ancak işlem durumunu almak için göreli bir yolu göstererek aynı şekilde çalışmaya devam edecektir. | İlgili POST isteklerinden herhangi birini teklif için gönderirken, Konum üstbilgisi teklifin geçiş durumuna bağlı olarak iki biçimden birine sahip olacaktır:<ul><li>Geçirilmeyen teklifler<br>`/api/operations/{PublisherId}${offerId}$2$preview?api-version=2017-10-31`</li><li>Geçirilen teklifler<br>`/api/publishers/{PublisherId}/offers/{offereId}/operations/408a4835-0000-1000-0000-000000000000?api-version=2017-10-31`</li> |
| GET Operasyonu | Yanıtta daha önce desteklenen 'bildirim-e-posta' alanı olan teklifler için, bu alan amortismana dahil edilir ve artık geçirilen teklifler için iade edilmez. | Geçirilen teklifler için artık isteklerde belirtilen e-postalar listesine bildirim göndermeyiz. Bunun yerine, API hizmeti e-posta göndermek için İş Ortağı Merkezi'ndeki bildirim e-posta süreciyle uyumlu olacaktır. Özellikle, bildirimler İş Ortağı Merkezi'ndeki Hesap ayarlarınızın Satıcı iletişim bilgileri bölümünde ayarlanan e-posta adresine gönderilecektir.<br><br>Bildirimler için doğru e-postanın sağlandığından emin olmak için İş Ortağı Merkezi'ndeki [Hesap ayarlarındaki](https://partner.microsoft.com/dashboard/account/management) Satıcı iletişim bilgileri bölümünde ayarlanan e-posta adresini gözden geçirin.  |
| | | |
