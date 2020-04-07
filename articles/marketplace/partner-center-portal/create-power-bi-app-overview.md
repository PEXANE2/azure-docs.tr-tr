---
title: Power BI uygulama oluşturma genel bakış - Azure Marketi
description: Bu makalede, Microsoft AppSource için bir Power BI uygulaması yayımlamak için üst düzey adımlar açıklanmaktadır. Power BI uygulamanızın ticari pazarda yayınlanması için karşılaması gereken teknik ve iş gereksinimleri de sağlanır.
author: anbene
ms.author: mingshen
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 04/02/2020
ms.openlocfilehash: c348a172b16e12334d33cf2718609694147fdce3
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80674533"
---
# <a name="power-bi-app-creation-overview"></a>Power BI uygulaması oluşturma genel bakış

> [!IMPORTANT]
> Power BI uygulama tekliflerinizin yönetimini Cloud Partner Portal'dan İş Ortağı Merkezi'ne taşıyoruz. Tekliflerin izne gelene kadar, lütfen bulut iş ortağı portalının tekliflerinizi yönetmesi için [Power BI uygulama teklifindeki](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/power-bi/cpp-power-bi-offer) talimatları izleyin.

Bu makalede, Microsoft [AppSource](https://appsource.microsoft.com/)bir Power BI uygulaması yayınlamak için nasıl açıklar. Power BI uygulaması, veri kümeleri, raporlar ve panolar da dahil olmak üzere özelleştirilebilir içerik paketler. Daha sonra Uygulamayı AppSource'u kullanarak diğer Power BI platformlarıyla kullanabilir, geliştirici tarafından izin verilen ayarlamaları ve özelleştirmeleri gerçekleştirebilir ve kendi verilerinize bağlayabilirsiniz.

## <a name="publishing-benefits"></a>Yararlanma avantajları

Ticari pazara yayıncılığın faydaları:

- Microsoft markasını kullanarak şirketinizi tanıtın.
- Azure Marketi aracılığıyla AppSource'ta 100 milyondan fazla Office 365 ve Dynamics 365 kullanıcısına ve 200.000'den fazla kuruluşa ulaşma potansiyeline sahip.
- Bu pazarlardan yüksek kaliteli müşteri adayları alın.
- Hizmetlerinizin Microsoft alanı ve tele-satış ekipleri tarafından tanıtınır.

## <a name="overview"></a>Genel Bakış

:::image type="content" source="media/power-bi-app-publishing-steps.png" alt-text="Power BI uygulamasını yayınlama adımlarına genel bakış" border="false":::

Önemli yayımlama adımları şunlardır:

1. Uygulamanızı Power BI'de oluşturun. Teklifin ana teknik varlığı olan bir paket yükleme bağlantısı alırsınız. İş Ortağı Merkezi'nde teklif oluşturmadan önce test paketini üretim öncesi pakete gönderin. Ayrıntılar için [Power BI uygulamaları nelerdir?](https://docs.microsoft.com/power-bi/service-template-apps-overview)
2. Resmi ad, açıklama ve logolar gibi pazarlama malzemelerini ekleyin.
3. Teklifin kullanım koşulları, gizlilik politikası, destek politikası ve kullanıcı yardımı gibi yasal ve destek belgelerini ekleyin.
4. Teklifi oluşturun: Teklif açıklaması, pazarlama materyalleri, yasal bilgiler, destek bilgileri ve varlık özellikleri de dahil olmak üzere ayrıntıları nızı birlikte vermek için İş Ortağı Merkezi'ni kullanın.
5. Yayımlama için gönderin.
6. AppSource'un biniş ekibi tarafından test edildiği, doğruladığı ve uygulamanızı onayladığı İş Ortağı Merkezi'ndeki işlemi izleyin.
7. Onaylandıktan sonra, uygulamayı test ortamında inceleyin ve yayınlayın. Bu AppSource üzerinde listeleyecek (bu "canlı gider").
8. Power BI'de paketi üretime gönderin. Ayrıntılar için Power [BI uygulama sürümüne](https://docs.microsoft.com/power-bi/service-template-apps-create#manage-the-template-app-release)bakın.

## <a name="before-you-begin"></a>Başlamadan önce

Şablonlar, ipuçları ve örnekler sağlayan aşağıdaki bağlantıları inceleyin.

- [Power BI uygulaması oluşturma](https://docs.microsoft.com/power-bi/service-template-apps-create)
- [Power BI uygulaması yazma ipuçları](https://docs.microsoft.com/power-bi/service-template-apps-tips)
- [Örnekler](https://docs.microsoft.com/power-bi/service-template-apps-samples)

## <a name="requirements"></a>Gereksinimler

Ticari pazarda yayınlanabilmesi için Power BI uygulama teklifinizin aşağıdaki teknik ve iş gereksinimlerini karşılaması gerekir.

### <a name="technical-requirements"></a>Teknik gereksinimler

İhtiyacınız olacak ana teknik varlık bir [Power BI uygulamasıdır.](https://go.microsoft.com/fwlink/?linkid=2028636) Bu, birincil veri kümeleri, raporlar veya panolardan oluşan bir koleksiyondur. Ayrıca isteğe bağlı bağlı hizmetler ve daha önce [içerik paketi](https://docs.microsoft.com/power-bi/service-organizational-content-pack-introduction)olarak bilinen gömülü veri kümeleri içerir. Bu tür uygulama geliştirme hakkında daha fazla bilgi için [Power BI uygulamaları nelerdir bölümüne bakın.](https://go.microsoft.com/fwlink/?linkid=2028636)

#### <a name="get-an-installation-web-address"></a>Yükleme web adresi alma

Yalnızca Power BI ortamında bir [Power BI](https://powerbi.microsoft.com/) uygulaması oluşturabilirsiniz.

1. Power BI [Pro lisansı](https://docs.microsoft.com/power-bi/service-admin-purchasing-power-bi-pro)ile oturum açın.
2. Power BI'de uygulamanızı oluşturun ve test edin.
3. Uygulama yükleme web adresini aldığınızda, İş Ortağı Merkezi'ndeki **Teknik Yapılandırma** sayfasına ekleyin.

Uygulamanız Power BI'de oluşturulduktan ve test edildikten sonra, [power bi uygulama teklifi oluşturmak](https://aka.ms/AzureCreatePBIServiceApp)için ihtiyacınız olduğunda uygulama yükleme web adresini kaydedin.

### <a name="business-requirements"></a>İş gereksinimleri

İş gereksinimleri usul, sözleşme ve yasal yükümlülükleri içerir. Şunları yapmalısın:

- Kayıtlı bir ticari pazar yeri yayıncısı olun. Kayıtlı değilseniz, [Ticari Pazar Yeri Yayıncı ol'daki](https://docs.microsoft.com/azure/marketplace/become-publisher)adımları izleyin.
- Uygulama Kaynağı'nda listelenecek teklifinizin ölçütlerini karşılayan içerik sağlayın. Daha fazla bilgi için [Bkz. AppSource'da listeleyecek bir uygulama nız var mı? İşte nasıl](https://appsource.microsoft.com/blogs/have-an-app-to-list-on-appsource-here-s-how).
- [Microsoft Gizlilik Bildirimi'ni](https://privacy.microsoft.com/privacystatement)kabul edin ve izleyin.

## <a name="next-steps"></a>Sonraki adımlar

- [İş Ortağı Merkezi'nde Power BI uygulama teklifi oluşturun](https://aka.ms/AzureCreatePBIServiceApp)