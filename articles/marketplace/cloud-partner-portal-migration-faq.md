---
title: Iş Ortağı Merkezi 'ne geçişle ilgili sık sorulan sorular-Microsoft ticari Market
description: Bulut İş Ortağı Portalı tekliflerini Iş ortağı merkezine geçirme hakkında sık sorulan soruların yanıtları.
author: anbene
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 05/19/2020
ms.author: mingshen
ms.openlocfilehash: c4fdde94ea703d194e2de27a8df429f62ea374f4
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83727672"
---
# <a name="common-questions-about-migrating-from-the-cloud-partner-portal-to-partner-center"></a>Bulut İş Ortağı Portalı Iş ortağı merkezine geçirme hakkında sık sorulan sorular

Bu makalede, Bulut İş Ortağı Portalı tekliflerinin Iş Ortağı Merkezi 'ne geçirilmesi hakkında sıkça sorulan sorular ele alınmaktadır.

## <a name="what-does-offer-migration-mean"></a>Geçiş teklifi ne anlama geliyor?

Teklif verilerini, teklif yayımlama ve yönetim deneyimindeki değişikliklerle Iş ortağı merkezine Bulut İş Ortağı Portalı taşıdık.

| Alan  | Değişiklikler  |
|-------|----------|
| **Yayımlama ve teklif yönetimi deneyimi** | Iş Ortağı Merkezi 'nde sezgisel bir arabirim ile geliştirilmiş bir kullanıcı deneyimi görürsünüz. Daha fazla ayrıntı için bkz [. Iş ortağı merkezi ve bulut iş ortağı portalı arasındaki farklar nelerdir?](#what-are-the-differences-between-partner-center-and-the-cloud-partner-portal) |
| **Market 'teki tekliflerinizin kullanılabilirliği** | Değişiklik yok. Teklifiniz Market 'te canlı ise, geçiş işlemi tamamlandıktan sonra ve sonrasında canlı olmaya devam edecektir. |
| **Yeni satın almalar ve dağıtımlar** | Değişiklik yok. Müşterileriniz, kesintiler olmadan tekliflerinizi satın alıp dağıtmaya devam eder. |
| **Ödemeler** | Geçiş sırasında veya sonrasında gerçekleşen tüm satın almalar ve dağıtımlar sizin için normal olarak ücretlendirilecektir. |
|**Mevcut [bulut iş ortağı portalı API 'leri](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-api-overview) ile API tümleştirmeleri** | Mevcut Bulut İş Ortağı Portalı API 'Leri geçişten sonra desteklenecek ve mevcut tümleştirmeleriniz çalışmaya devam edecektir. Daha fazla ayrıntı için [bulut iş ortağı PORTALı REST API 'lerinin geçiş sonrası desteklenecek mi?](#will-the-cloud-partner-portal-rest-apis-be-supported-post-migration) |
| | |

## <a name="can-i-still-access-the-cloud-partner-portal-and-manage-my-offers-during-migration"></a>Bulut İş Ortağı Portalı erişip geçiş sırasında tekliflerimi yönetebilirim miyim?

Tüm teklifler Iş Ortağı Merkezi 'ne geçirildi. Her teklifin geçiş süresi 24 saatten az olmalıdır. Teklifiniz geçirildikten sonra bir e-posta bildirimi almış olmanız gerekir.

Tekliflerinizi geçirdikten sonra, Bulut İş Ortağı Portalı **sınırlı bir süre için** salt okuma modunda olacaktır. Bu durumda "Iş Ortağı Merkezi 'ne taşınır" görüntülenir ve aşağıdaki ekran görüntülerinde gösterildiği gibi Iş Ortağı Merkezi 'nde teklifinizin bir bağlantısını içerir. Bu noktada, tüm tekliflerinizin güncelleştirmelerini yönetecek veya özel olarak Iş Ortağı Merkezi aracılığıyla yeni teklifler oluşturacaksınız.

:::image type="content" source="media/migration-faq/all-offers-2.png" alt-text="Iş Ortağı Merkezi 'ne geçirilmiş teklifler için verilen iletiyi gösterir":::

:::image type="content" source="media/migration-faq/offer-has-moved.png" alt-text="Geçirilmiş bir teklifin Bulut İş Ortağı Portalı sayfasını gösterir.":::

## <a name="how-will-i-create-new-offers"></a>Yeni teklifleri nasıl oluşturabilirim?

Bulut İş Ortağı Portalı, Iş Ortağı Merkezi 'nde yeni teklifler oluşturmaya yönlendirilirsiniz

:::image type="content" source="media/migration-faq/create-new-offer-1.png" alt-text="Bulut İş Ortağı Portalı yeni teklif oluşturma menüsünü gösterir":::

Yeni bir teklif seçtikten sonra, aşağıdaki gibi bir ileti görürsünüz.

:::image type="content" source="media/migration-faq/create-new-offer-2.png" alt-text="CPP içinde yeni bir teklif oluşturulurken alınan iletiyi gösterir":::

## <a name="do-i-need-to-create-a-new-account-to-manage-offers-in-partner-center"></a>Iş Ortağı Merkezi 'nde teklifleri yönetmek için yeni bir hesap oluşturmak mı gerekiyor?

Hayır. Temel hesabınız korunacaktır ve Iş Ortağı Merkezi 'nde zaten yönetiyorsunuz. Yani, mevcut bir iş ortağıysanız, Iş ortağı merkezi geçiş sonrasında oturum açmak için mevcut Bulut İş Ortağı Portalı hesabı kimlik bilgilerinizi kullanabilirsiniz. Yalnızca teklifler ve ilişkili yönetim deneyimi Bulut İş Ortağı Portalı Iş ortağı merkezine taşınıyor. Tekliflerinizin yeni hesapla ilişkilendirilmeyeceği yeni hesap oluşturmayacağız.

## <a name="i-see-a-message-in-the-cloud-partner-portal-to-activate-my-account-what-does-this-mean"></a>Hesabımı etkinleştirmek için Bulut İş Ortağı Portalı bir ileti görüyorum, bu ne anlama geliyor?

Hesabınızı Iş ortağı merkezine doğru bir şekilde geçirmek ve teklif geçişi tamamlandıktan sonra Iş Ortağı Merkezi 'nde tekliflerinizi yönetmenizi sağlamak için daha fazla ayrıntı yapmanız gerekir. Hesap etkinleştirme hakkında daha fazla bilgi için bkz. [bulut iş ortağı portalı 'Dan Iş ortağı merkezine hesap geçişi](https://docs.microsoft.com/azure/marketplace/partner-center-portal/account-migration-from-cpp-to-pc).

Hesap etkinleştirmeyi tamamlaması için gereken adımlar, hesap rolünüze göre farklılık gösterir.

| Hesap rolü | Etkinleştirme adımları |
|--------------|----------------|
|Sahip | Bulut İş Ortağı Portalı [Yayımcı profili](https://cloudpartner.azure.com/#profile) sayfasına gidin ve sonra etkinleştirmek için başlıktaki bağlantıya tıklayın. Hesap etkinleştirmeyi tamamlaması için Iş Ortağı Merkezi 'ne yönlendirilirsiniz. |
| Katılımcı | Hesabı yalnızca sahip rolü olan bir Kullanıcı etkinleştirebilir. Hesap etkinleştirmeyi tamamlayacak hesap sahipleriyle iletişim kurun. Hesap sahiplerinizin başlık iletisinde listelenmesi gerekir. |
| | |

## <a name="im-having-trouble-logging-in-to-my-account-and-opening-a-support-ticket"></a>Hesabım 'da oturum açarken ve bir destek bileti açarken sorun yaşıyorum

Hesabınızda oturum açamazsınız bir [destek bileti](https://partner.microsoft.com/support/v2/?stage=1)açabilirsiniz.

## <a name="where-can-i-find-documentation-on-the-new-partner-center-publishing-experience"></a>Yeni Iş Ortağı Merkezi yayımlama deneyimiyle ilgili belgeleri nereden bulabilirim?

[Ticari Market belgelerine](https://docs.microsoft.com/azure/marketplace/)gidin. Ardından, **iş ortağı merkezi 'nde ticari Market Portalı '** nı genişletin   >  , her bir teklif türünü oluşturmaya yönelik yardım konularını görmek için**Yeni bir teklif oluşturun** .

:::image type="content" source="media/migration-faq/marketplace-help-topics.png" alt-text="Iş Ortağı Merkezi için yardım konularını gösterir":::

### <a name="what-are-the-differences-between-partner-center-and-the-cloud-partner-portal"></a>Iş ortağı merkezi ve Bulut İş Ortağı Portalı arasındaki farklar nelerdir?

Bulut İş Ortağı Portalı ve Iş ortağı merkezi arasında aşağıdaki farklılıklar olduğunu fark edebilirsiniz.

### <a name="modular-publishing-capabilities"></a>Modüler yayımlama özellikleri

İş Ortağı Merkezi, tüm güncelleştirmeleri her zaman tek seferde yayımlamak yerine yayımlamak istediğiniz değişiklikleri seçmenizi sağlayan modüler bir yayımlama seçeneği sunar. Örneğin, aşağıdaki ekran görüntüsünde, **özelliklerin** ve **teklif listelemesi**için yapılan değişiklikler aşağıda verilmiştir.

:::image type="content" source="media/migration-faq/review-and-publish-migration.png" alt-text="Önizleme ve yayımlama sayfasını gösterir":::

Yayımlayacağınız güncelleştirmeler taslak olarak kaydedilir. Teklifinizi herkese açık hale getirmeden önce teklifinizin doğrulanması için size devam edin.

### <a name="rich-text-format"></a>Zengin metin biçimi

Teklif listesi ve plan listeleme sayfasında zengin metin Düzenleyicisi 'Ni kullanarak teklif ve plan açıklamanızı geliştirin.

:::image type="content" source="media/migration-faq/rich-text-editor-migration.png" alt-text="Zengin metin düzenleyicisini gösterir":::

### <a name="enhanced-preview-options"></a>Gelişmiş önizleme seçenekleri

İş Ortağı Merkezi, Gelişmiş filtreleme seçenekleriyle bir [karşılaştırma özelliği](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#compare-changes-to-marketplace-offers) içerir. Bu, teklifin önizleme ve canlı sürümleriyle karşılaştırmanıza olanak sağlar.

:::image type="content" source="media/migration-faq/enhanced-preview.png" alt-text="Karşılaştırma özelliğini gösterir":::

### <a name="branding-and-navigation-changes"></a>Marka ve gezinti değişiklikleri

Bazı marka değişiklikleri olduğunu fark edeceksiniz. Örneğin, "SKU 'Lar" Iş Ortağı Merkezi 'nde "planlar" olarak markalı değildir.

:::image type="content" source="media/migration-faq/plan-overview-migration.png" alt-text="Plana genel bakış gösterir.":::

Ayrıca, Bulut İş Ortağı Portalı **Market** veya S**torefront Ayrıntılar** (danışmanlık hizmeti, Power BI uygulaması) sayfalarında sağladığınız bilgiler, Iş Ortağı Merkezi 'nde **teklif listesi** sayfasında toplanır.

:::image type="content" source="media/migration-faq/offer-listing-migration.png" alt-text="Teklif listesi sayfasını gösterir.":::

Bulut İş Ortağı Portalı tek bir sayfada SKU 'Lar için sağlamak üzere kullandığınız bilgiler artık Iş Ortağı Merkezi 'nin çeşitli sayfalarında toplanabilir:

* Plan ayarlama sayfası
* Plan listeleme sayfası
* Plan kullanılabilirlik sayfası
* Bu ekran görüntüsünde gösterildiği gibi teknik yapılandırma planlayın sayfası.

:::image type="content" source="media/migration-faq/tech-config-migration.png" alt-text="Teknik yapılandırma planı sayfasını gösterir.":::

Teklif KIMLIĞINIZ artık teklifin sol gezinti çubuğunda gösteriliyor.

:::image type="content" source="media/migration-faq/offer-id-offer-overview.png" alt-text="Teklif KIMLIĞIYLE birlikte sol gezinti menüsünü gösterir.":::

### <a name="stop-selling-an-offer"></a>Teklif satışı durdur

Market 'teki bir teklifi doğrudan Iş Ortağı Merkezi portalından [satmaya](https://docs.microsoft.com/azure/marketplace/partner-center-portal/update-existing-offer#stop-selling-an-offer-or-plan) yönelik istek yapabilirsiniz. Bu seçenek, teklifiniz için **teklif genel bakış** sayfasında bulunur.

:::image type="content" source="media/migration-faq/stop-selling-migration.png" alt-text="Satışı Durdur seçeneğinin bulunduğu teklif genel bakış sayfasını gösterir.":::

## <a name="what-pages-in-partner-center-correspond-to-pages-i-used-in-the-cloud-partner-portal"></a>Iş Ortağı Merkezi 'ndeki sayfalar Bulut İş Ortağı Portalı kullandığım sayfalara karşılık geliyor?

Aşağıdaki tabloda, iki Portal arasındaki karşılık gelen bağlantılar gösterilmektedir.

| Sayfa | Bulut İş Ortağı Portalı bağlantısı | İş Ortağı Merkezi bağlantısı |
|------|---------------------------|---------------------|
| **Tüm teklifler sayfası** | https://cloudpartner.azure.com/#alloffers | https://partner.microsoft.com/dashboard/commercial-marketplace/overview |
| **Tüm yayımcılar sayfası** | https://cloudpartner.azure.com/#publishers | https://partner.microsoft.com/dashboard/account/v3/publishers/list |
| **Yayımcı profili** | https://cloudpartner.azure.com/#profile | https://partner.microsoft.com/dashboard/account/management |
| **Kullanıcılar sayfası** | https://cloudpartner.azure.com/#users | https://partner.microsoft.com/dashboard/account/usermanagement |
| **Geçmiş sayfası** | https://cloudpartner.azure.com/#history | Geçmiş özelliği henüz Iş Ortağı Merkezi 'nde desteklenmiyor. |
| **Öngörüler panosu** | https://cloudpartner.azure.com/#insights | https://partner.microsoft.com/dashboard/commercial-marketplace/analytics/summary |
| **Ödeme raporu** | https://cloudpartner.azure.com/#insights/payout | https://partner.microsoft.com/dashboard/payouts/reports/incentivepayments |
| | |

## <a name="will-the-cloud-partner-portal-rest-apis-be-supported-post-migration"></a>Bulut İş Ortağı Portalı REST API 'Leri geçiş sonrası desteklenecek mi?

Bulut İş Ortağı Portalı API 'Leri iş ortağı merkezi ile tümleşiktir ve teklifleriniz iş ortağı merkezi 'ne geçirildikten sonra çalışmaya devam edecektir. Tümleştirme küçük değişiklikler sunar. İş Ortağı Merkezi 'ne geçişten sonra kodunuzun çalışmaya devam ettiğinden emin olmak için aşağıdaki tablodaki değişiklikleri gözden geçirin.

| **API** | **Açıklamayı Değiştir** | **Etki** |
| ------- | ---------------------- | ---------- |
| Yayınlama SONRASı, GoLive, Iptal | Geçirilen tekliflere, yanıt üst bilgisi farklı bir biçime sahip olur ancak işlem durumunu almak için göreli bir yol belirten aynı şekilde çalışmaya devam edecektir. | Bir teklif için karşılık gelen POST isteklerinin herhangi birini gönderirken, teklifin geçiş durumuna bağlı olarak konum üstbilgisi iki biçimden birine sahip olur:<ul><li>Geçirilmeyen teklifler<br>`/api/operations/{PublisherId}${offerId}$2$preview?api-version=2017-10-31`</li><li>Geçirilmiş teklifler<br>`/api/publishers/{PublisherId}/offers/{offereId}/operations/408a4835-0000-1000-0000-000000000000?api-version=2017-10-31`</li> |
| Işlemi al | Yanıtta daha önce ' bildirim-e-posta ' alanının desteklendiği teklifler için, bu alan kullanım dışı bırakılır ve artık geçirilmiş teklifler için döndürülmez. | Geçirilmiş teklifleriniz için, isteklerde belirtilen e-posta listesine artık bildirim gönderemeyecektir. Bunun yerine, API hizmeti, e-posta göndermek için Iş Ortağı Merkezi 'nde bildirim e-postası işlemiyle hizalanır. Özellikle, işlem ilerleme durumunu bildirmek için Iş ortağı merkezindeki hesap ayarlarınızın satıcı iletişim bilgileri bölümünde bulunan e-posta adresine bildirim gönderilir.<br><br>Bildirimler için doğru e-postanın sağlandığından emin olmak için Iş Ortağı Merkezi 'ndeki [Hesap ayarları](https://partner.microsoft.com/dashboard/account/management) bölümündeki satıcı iletişim bilgileri bölümünde bulunan e-posta adresini gözden geçirin.  |
| | | |
