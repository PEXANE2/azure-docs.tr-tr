---
title: Yönetilen geliştirici portalına erişme ve bunları özelleştirme-Azure API Management | Microsoft Docs
description: API Management 'de geliştirici portalının yönetilen sürümünü nasıl kullanacağınızı öğrenin.
services: api-management
documentationcenter: API Management
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 03/05/2020
ms.author: apimpm
ms.openlocfilehash: 345b4950e25f48eb1a48c5e583908d0f771771ba
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86206236"
---
# <a name="access-and-customize-developer-portal"></a>Geliştirici portalına erişme ve özelleştirme

Geliştirici portalı, API 'lerinizin belgelerinde otomatik olarak oluşturulan ve tamamen özelleştirilebilir bir Web sitesidir. API tüketicilerinin API 'lerinizi keşfedebileceği, nasıl kullanılacağını ve nasıl erişim isteyeceğini burada bulabilirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Geliştirici portalının yönetilen sürümüne erişin
> * Yönetim arabiriminde gezin
> * İçeriği özelleştirme
> * Değişiklikleri Yayımla
> * Yayınlanan portalı görüntüleme

Geliştirici Portalında daha fazla ayrıntı için [Azure API Management geliştirici portalına genel bakış ' a](api-management-howto-developer-portal.md)ulaşabilirsiniz.

![API Management Geliştirici Portalı-yönetici modu](media/api-management-howto-developer-portal-customize/cover.png)

## <a name="prerequisites"></a>Önkoşullar

- Şu hızlı başlangıcı doldurun: [Azure API Management örneği oluşturma](get-started-create-service-instance.md)
- Azure API Management örneğini içeri aktarın ve yayımlayın. Daha fazla bilgi için bkz. [Içeri aktarma ve yayımlama](import-and-publish.md)

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="access-the-portal-as-an-administrator"></a>Portala yönetici olarak erişin

Portalın yönetilen sürümüne erişmek için aşağıdaki adımları izleyin.

1. Azure portal API Management hizmet örneğinize gidin.
1. Üst gezinti çubuğundaki **Geliştirici Portalı** düğmesine tıklayın. Portalın yönetici sürümüne sahip yeni bir tarayıcı sekmesi açılır.

## <a name="understand-the-portals-administrative-interface"></a>Portalın yönetim arabirimini anlayın

### <a name="default-content"></a>Varsayılan içerik 

Portala ilk kez erişiyorsanız, varsayılan içerik arka planda otomatik olarak sağlanır. Varsayılan içerik, portalın yeteneklerini göstermek ve portalınızı kişiselleştirmek için gereken özelleştirme miktarını en aza indirmek için tasarlanmıştır. [Azure API Management geliştirici portalına genel bakış](api-management-howto-developer-portal.md)bölümünde Portal içeriğine nelerin dahil olduğu hakkında daha fazla bilgi edinebilirsiniz.

### <a name="visual-editor"></a>Görsel düzenleyici

Portalın içeriğini görsel düzenleyiciyle özelleştirebilirsiniz. Sol taraftaki menü bölümleri; sayfaları, medyayı, düzenleri, menüleri, stilleri veya Web sitesi ayarlarını oluşturmanıza veya değiştirmenize olanak sağlar. Alt kısımdaki menü öğeleri, görünüm pencereleri (örneğin, mobil veya masaüstü) arasında geçiş yapmanızı, portalın öğelerini kimliği doğrulanmış veya anonim kullanıcılarla görüntülemeyi veya eylemleri kaydetmenizi veya geri almanızı sağlar.

Bir sayfaya, artı işareti ile mavi simgesine tıklayarak satır ekleyebilirsiniz. Pencere öğeleri (örneğin, metin, görüntüler veya API listesi), artı işaretiyle bir gri simgeye basılarak eklenebilir. Sürükle ve bırak etkileşimi ile bir sayfadaki öğeleri yeniden düzenleyebilirsiniz. 

### <a name="layouts-and-pages"></a>Düzenler ve sayfalar

![Sayfalar ve düzenler](media/api-management-howto-developer-portal-customize/pages-layouts.png)

Düzenler sayfaların nasıl görüntüleneceğini tanımlar. Örneğin, varsayılan içerikte iki düzen vardır-diğeri giriş sayfasına, diğeri de kalan tüm sayfalara uygulanır.

Bir düzen, URL şablonuyla sayfanın URL 'siyle eşleştirerek bir sayfaya uygulanır. Örneğin, URL şablonuna sahip bir düzen, `/wiki/*` `/wiki/` URL 'deki segmentle birlikte her sayfaya uygulanır: `/wiki/getting-started` , `/wiki/styles` vb.

Yukarıdaki görüntüde, sayfa kırmızı olarak işaretlenirken düzene ait içerikler mavi olarak işaretlenir. Menü bölümleri sırasıyla işaretlenir.

### <a name="styling-guide"></a>Stil Kılavuzu

![Stil Kılavuzu](media/api-management-howto-developer-portal-customize/styling-guide.png)

Stil Kılavuzu, tasarımcılara göz önünde bulundurularak oluşturulan bir paneldir. Portalınızdaki tüm görsel öğeleri fazla görmeye ve stillendirilmesini sağlar. Stil hiyerarşik bir öğelerdir; birçok öğe diğer öğelerden özellikleri miras alır. Örneğin düğme öğeleri metin ve arka plan renklerini kullanır. Bir düğmenin rengini değiştirmek için özgün renk türevini değiştirmeniz gerekir.

Bir varyantı düzenlemek için, üzerine tıklayın ve sayfanın üstünde görünen kalem simgesini seçin. Açılır penceredeki değişiklikleri yaptıktan sonra kapatın.

### <a name="save-button"></a>Kaydet düğmesi

![Kaydet düğmesi](media/api-management-howto-developer-portal-customize/save-button.png)

Portalda her değişiklik yaptığınızda, alt kısımdaki menüdeki **Kaydet** düğmesine basarak el ile kaydetmeniz gerekir. Değişikliklerinizi kaydettiğinizde, değiştirilen içerik API Management hizmetinize otomatik olarak yüklenir.

## <a name="customize-the-portals-content"></a>Portalın içeriğini özelleştirme

Portalınızı ziyaretçi tarafından kullanılabilir yapmadan önce, otomatik olarak oluşturulan içeriği kişiselleştirmelisiniz. Önerilen değişiklikler, ana sayfanın düzenlerini, stillerini ve içeriğini içerir.

> [!NOTE]
> Tümleştirme konuları nedeniyle, aşağıdaki sayfalar farklı bir URL altında kaldırılamaz veya taşınamaz: `/404` , `/500` ,, `/captcha` `/change-password` , `/config.json` , `/confirm/invitation` , `/confirm-v2/identities/basic/signup` , `/confirm-v2/password` , `/internal-status-0123456789abcdef` , `/publish` , `/signin` `/signin-sso` `/signup` ,,,,,,,,,,,.

### <a name="home-page"></a>Giriş sayfası

Varsayılan **giriş** sayfası, kukla içerikle doldurulmuştur. Tüm bölümleri içeriğe göre kaldırabilir ya da yapıyı tutabilir ve öğeleri tek tek düzenleyebilirsiniz. Oluşturulan metin ve görüntüleri kendi kendinize değiştirin ve bağlantıların istenen konumlara işaret ettiğinizden emin olun.

### <a name="layouts"></a>Düzenler

Gezinti çubuğundaki otomatik olarak oluşturulan logoyu kendi görüntünüz ile değiştirin.

### <a name="styling"></a>Stil oluşturma

Herhangi bir stili ayarlamanız gerekmez, ancak belirli öğeleri ayarlamayı düşünebilirsiniz. Örneğin, birincil rengi markaınızın rengine uyacak şekilde değiştirin.

### <a name="customization-example"></a>Özelleştirme örneği

Aşağıdaki videoda portalın içeriğini düzenlemeyi, Web sitesinin görünümünü özelleştirmeyi ve değişiklikleri yayımlamayı göstereceğiz.

> [!VIDEO https://www.youtube.com/embed/5mMtUSmfUlw]

## <a name="publish-the-portal"></a><a name="publish"> </a>Portalı yayımlama

Portalınızın ve en son değişikliklerinin ziyaretçi tarafından kullanılabilmesini sağlamak için, onu yayımlamanız gerekir.

1. **Kaydet** simgesine tıklayarak değişikliklerinizi kaydettiğinizden emin olun.
1. Menüdeki **işlemler** bölümünde **Web sitesine yayımla** ' ya tıklayın. Bu işlem birkaç dakika sürebilir.  
    ![Portalı Yayımla](media/api-management-howto-developer-portal-customize/publish-portal.png)

> [!NOTE]
> Portalın, özel bir etki alanı atama, kimlik sağlayıcılarını güncelleştirme, temsili ayarlama, oturum açma ve ürün koşulları ve daha fazlası gibi API Management hizmet yapılandırma değişikliklerinden sonra yeniden yayımlanması gerekir.

## <a name="visit-the-published-portal"></a>Yayınlanan portalı ziyaret edin

Portalı yayımladıktan sonra, yönetim paneliyle aynı URL 'de buna erişebilirsiniz (örneğin,) `https://contoso-api.developer.azure-api.net` . Bunu, dış ziyaretçi olarak ayrı bir tarayıcı oturumunda (bilito/Private gözatma modu) görüntüleyin.

## <a name="apply-the-cors-policy-on-apis"></a>API 'lerde CORS ilkesini uygulama

Portalınızın ziyaretçilerinin yerleşik etkileşimli konsol aracılığıyla API 'Leri test etmek için API 'lerinizin CORS 'yi (Cross-Origin kaynak paylaşımı) etkinleştirmeniz gerekir. Daha fazla bilgi için [Bu belge makalesine](api-management-howto-developer-portal.md#cors) başvurun.

## <a name="next-steps"></a>Sonraki adımlar
- [Bulut harcamalarınızı iyileştirin ve kaydedin](https://docs.microsoft.com/azure/cost-management-billing/costs/quick-acm-cost-analysis?WT.mc_id=costmanagementcontent_docsacmhorizontal_-inproduct-learn)

Geliştirici portalı hakkında daha fazla bilgi edinin:

- [Azure API Management geliştirici portalına genel bakış](api-management-howto-developer-portal.md)