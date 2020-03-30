---
title: Yönetilen geliştirici portalına erişin ve bunları özelleştirin - Azure API Yönetimi | Microsoft Dokümanlar
description: API Yönetimi'nde geliştirici portalının yönetilen sürümünü nasıl kullanacağınızı öğrenin.
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
ms.openlocfilehash: af7c995c11322a538dd9e27a905f1ddbc723e8ab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244101"
---
# <a name="access-and-customize-developer-portal"></a>Geliştirici portalına erişin ve özelleştirin

Geliştirici portalı, API'lerinizin belgeleriyle otomatik olarak oluşturulmuş, tamamen özelleştirilebilir bir web sitesidir. API tüketicilerinin API'lerinizi keşfedebileceği, bunları nasıl kullanacağınızı öğrenebileceği ve erişim isteyebileceği yerdir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Geliştirici portalının yönetilen sürümüne erişin
> * İdari arabiriminde gezinme
> * İçeriği özelleştirme
> * Değişiklikleri yayımlama
> * Yayınlanan portalı görüntüleyin

[Azure API Yönetimi geliştirici portalına genel bakış](api-management-howto-developer-portal.md)bilgi için geliştirici portalında daha fazla ayrıntı bulabilirsiniz.

![API Yönetimi geliştirici portalı - yönetici modu](media/api-management-howto-developer-portal-customize/cover.png)

## <a name="prerequisites"></a>Ön koşullar

- Aşağıdaki hızlı başlatmayı tamamlayın: [Azure API Yönetimi örneği oluşturun](get-started-create-service-instance.md)
- Azure API Yönetimi örneğini içe aktarın ve yayımlayın. Daha fazla bilgi için [Bkz. İçe Aktar ve yayımla](import-and-publish.md)

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="access-the-portal-as-an-administrator"></a>Portala yönetici olarak erişin

Portalın yönetilen sürümüne erişmek için aşağıdaki adımları izleyin.

1. Azure portalındaki API Yönetimi hizmet örneğinize gidin.
1. Üst teki gezinti çubuğundaki **Geliştirici portalı** düğmesine tıklayın. Portalın yönetimsürümüne sahip yeni bir tarayıcı sekmesi açılır.

## <a name="understand-the-portals-administrative-interface"></a>Portalın yönetim arabirimini anlama

### <a name="default-content"></a>Varsayılan içerik 

Portala ilk kez erişiyorsanız, varsayılan içerik arka planda otomatik olarak sağlanacaktır. Varsayılan içerik, portalın yeteneklerini sergilemek ve portalınızı kişiselleştirmek için gereken özelleştirme miktarını en aza indirmek üzere tasarlanmıştır. [Azure API Yönetimi geliştirici portalına genel bakışta](api-management-howto-developer-portal.md)portal içeriğinde nelerin yer aldığı hakkında daha fazla bilgi edinebilirsiniz.

### <a name="visual-editor"></a>Görsel editör

Görsel düzenleyici ile portalın içeriğini özelleştirebilirsiniz. Soldaki menü bölümleri, sayfaları, medyayı, düzenleri, menüleri, stilleri veya web sitesi ayarlarını oluşturmanıza veya değiştirmenize izin tanır. Alttaki menü öğeleri, görüntüleme noktaları arasında geçiş yapmanıza (örneğin, mobil veya masaüstü), portalın kimlik doğrulaması veya anonim kullanıcılar tarafından görülebilen öğeleri görüntülemenize veya eylemleri kaydetmenize veya geri alabilmenize izin verir.

Artı işareti olan mavi bir simgeye tıklayarak sayfaya satır ekleyebilirsiniz. Widget'lar (örneğin, metin, resim veya API listesi) artı işareti olan gri bir simgeye basılarak eklenebilir. Sürükle ve bırak etkileşimi olan bir sayfadaki öğeleri yeniden düzenleyebilirsiniz. 

### <a name="layouts-and-pages"></a>Düzenler ve sayfalar

![Sayfalar ve düzenler](media/api-management-howto-developer-portal-customize/pages-layouts.png)

Düzenler, sayfaların nasıl görüntüleneceğini tanımlar. Örneğin, varsayılan içerikte, biri ana sayfaya, diğeri kalan tüm sayfalariçin geçerli dir.

Düzen, URL şablonunu sayfanın URL'si ile eşleştirerek sayfaya uygulanır. Örneğin, URL `/wiki/*` şablonu içeren düzen, URL'de `/wiki/` segment olan her `/wiki/getting-started` `/wiki/styles`sayfaya uygulanır: , , vb.

Yukarıdaki resimde, düzene ait içerik mavi, sayfa ise kırmızı ile işaretlenmiştir. Menü bölümleri sırasıyla işaretlenir.

### <a name="styling-guide"></a>Stil kılavuzu

![Stil kılavuzu](media/api-management-howto-developer-portal-customize/styling-guide.png)

Stil kılavuzu, tasarımcılar düşünülde oluşturulan bir paneldir. Portalınızdaki tüm görsel öğeleri denetlemenize ve şekillendirmeye olanak tanır. Stil hiyerarşiktir - birçok öğe diğer öğelerden özellikleri devralır. Örneğin, düğme öğeleri metin ve arka plan için renkleri kullanır. Bir düğmenin rengini değiştirmek için özgün renk varyantını değiştirmeniz gerekir.

Bir varyantı yeniden yapmak için üzerine tıklayın ve üstünde görünen kalem simgesini seçin. Açılır penceredeki değişiklikleri yaptıktan sonra kapatın.

### <a name="save-button"></a>Kaydet düğmesi

![Kaydet düğmesi](media/api-management-howto-developer-portal-customize/save-button.png)

Portalda bir değişiklik yaptığınızda, alttaki menüdeki **Kaydet** düğmesine basarak el ile kaydetmeniz gerekir. Değişikliklerinizi kaydettiğinizde, değiştirilen içerik otomatik olarak API Yönetimi hizmetinize yüklenir.

## <a name="customize-the-portals-content"></a>Portalın içeriğini özelleştirme

Portalınızı ziyaretçilerin kullanımına sunmadan önce, otomatik olarak oluşturulan içeriği kişiselleştirmeniz gerekir. Önerilen değişiklikler, ana sayfanın düzenlerini, stillerini ve içeriğini içerir.

> [!NOTE]
> Entegrasyon hususları nedeniyle, aşağıdaki sayfalar kaldırılamaz veya farklı bir `/404`URL `/500` `/captcha`altında `/change-password` `/config.json`taşınamaz: `/internal-status-0123456789abcdef` `/publish`, `/signin` `/signin-sso` `/confirm/invitation` `/confirm-v2/identities/basic/signup` `/confirm-v2/password`, `/signup`, , , , , , , .

### <a name="home-page"></a>Giriş sayfası

Varsayılan **Giriş** sayfası sahte içerikle doldurulur. İçerikle birlikte tüm bölümleri kaldırabilir veya yapıyı koruyabilir ve öğeleri tek tek ayarlayabilirsiniz. Oluşturulan metin ve görüntüleri kendi metninizile değiştirin ve bağlantıların istenen konumlara işaret edeceğinden emin olun.

### <a name="layouts"></a>Düzenler

Navigasyon çubuğunda otomatik olarak oluşturulan logoyu kendi resminizle değiştirin.

### <a name="styling"></a>Stil oluşturma

Herhangi bir stilleri ayarlamanız gerekmese de, belirli öğeleri ayarlamayı düşünebilirsiniz. Örneğin, markanızın rengine uyacak şekilde birincil rengi değiştirin.

### <a name="customization-example"></a>Özelleştirme örneği

Aşağıdaki videoda portalın içeriğini nasıl değiştirebileceğimizi, web sitesinin görünümünü nasıl özelleştirebileceğimizi ve değişiklikleri nasıl yayınlayacağımızı gösteriyoruz.

> [!VIDEO https://www.youtube.com/embed/5mMtUSmfUlw]

## <a name="publish-the-portal"></a><a name="publish"> </a>Portalı yayımla

Portalınızı ve en son değişikliklerini ziyaretçilerin kullanımına açmak için yayımlamanız gerekir.

1. **Kaydet** simgesine tıklayarak değişikliklerinizi kaydettiğinizden emin olun.
1. Menünün **Operasyonlar** bölümünde **web sitesi yayın'a** tıklayın. Bu işlem birkaç dakika sürebilir.  
    ![Portalı yayımla](media/api-management-howto-developer-portal-customize/publish-portal.png)

> [!NOTE]
> Portalın, özel bir etki alanı atama, kimlik sağlayıcılarını güncelleştirme, temsilci ayarlama, oturum açma ve ürün koşullarını belirtme ve daha fazlası gibi API Yönetimi hizmet yapılandırması değişikliklerisonrasında yeniden yayımlanması gerekir.

## <a name="visit-the-published-portal"></a>Yayınlanan portalı ziyaret edin

Portalı yayımladıktan sonra, portala yönetim paneliyle aynı URL'den `https://contoso-api.developer.azure-api.net`erişebilirsiniz, örneğin. Harici bir ziyaretçi olarak ayrı bir tarayıcı oturumunda (gizli / özel tarama modu) görüntüleyin.

## <a name="apply-the-cors-policy-on-apis"></a>API'lerde CORS ilkesini uygulayın

Portalınızın ziyaretçilerinin API'leri yerleşik etkileşimli konsol aracılığıyla test etmesini sağlamak için API'lerinizde CORS'leri (çapraz kaynak paylaşımı) etkinleştirmeniz gerekir. Daha fazla ayrıntı için [bu dokümantasyon makalesine](api-management-howto-developer-portal.md#cors) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Geliştirici portalı hakkında daha fazla bilgi edinin:

- [Azure API Yönetimi geliştirici portalına genel bakış](api-management-howto-developer-portal.md)
