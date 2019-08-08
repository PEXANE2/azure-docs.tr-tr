---
title: Yeni geliştirici portalına erişme ve bunları özelleştirme-Azure API Management | Microsoft Docs
description: Yeni geliştirici portalını API Management nasıl kullanacağınızı öğrenin.
services: api-management
documentationcenter: API Management
author: mikebudzynski
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2019
ms.author: apimpm
ms.openlocfilehash: 27d5dcc99db040036ba296911aa33d8a312bb23f
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68851521"
---
# <a name="access-and-customize-the-new-developer-portal-in-azure-api-management"></a>Azure API Management yeni geliştirici portalına erişin ve özelleştirin

Bu makalede, yeni Azure API Management geliştirici portalına nasıl erişebileceğiniz gösterilmektedir. Görsel düzenleyici deneyiminde içerik ekleme ve düzenleme ile Web sitesinin görünümünü özelleştirme konusunda size kılavuzluk eder.

![Yeni API Management geliştirici portalı](media/api-management-howto-developer-portal/cover.png)

## <a name="prerequisites"></a> Önkoşullar

- Aşağıdaki hızlı başlangıcı doldurun: [Azure API Management örneği oluşturun](get-started-create-service-instance.md).
- Azure API Management örneğini içeri aktarın ve yayımlayın. Daha fazla bilgi için bkz. [Içeri aktarma ve yayımlama](import-and-publish.md).

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

> [!NOTE]
> Yeni geliştirici portalı şu anda önizleme aşamasındadır.

## <a name="managed-vs-self-hosted"></a>Yönetilen ve şirket içinde barındırılan sürümler

Geliştirici portalınızı iki şekilde oluşturabilirsiniz:

- **Yönetilen sürüm** -API Management örneğiniz içinde yerleşik olan ve URL `<your-api-management-instance-name>.developer.azure-api.net`aracılığıyla erişilebilen portalı düzenleyerek ve özelleştirerek.
- **Şirket içinde barındırılan sürüm** -portalınızın bir API Management örneği dışında dağıtıp barındırılmasına göre. Bu yaklaşım, portalın kod temelini düzenlemenizi ve sunulan çekirdek işlevselliği genişletmenizi sağlar. Ayrıntılar ve yönergeler için, [portalın kaynak kodu Ile GitHub deposuna][1]bakın.

## <a name="managed-access"></a>Portalın yönetilen sürümüne erişin

Portalın yönetilen sürümüne erişmek için aşağıdaki adımları izleyin.

1. Azure portal API Management hizmet örneğinize gidin.
1. Üst gezinti çubuğundaki **Yeni geliştirici portalı (Önizleme)** düğmesine tıklayın. Portalın yönetici sürümüne sahip yeni bir tarayıcı sekmesi açılır. Portala ilk kez erişiyorsanız, varsayılan içerik otomatik olarak sağlanır.

## <a name="managed-tutorial"></a>Portalın yönetilen sürümünü düzenleyin ve özelleştirin

Aşağıdaki videoda portalın içeriğini düzenlemeyi, Web sitesinin görünümünü özelleştirmeyi ve değişiklikleri yayımlamayı göstereceğiz.

> [!VIDEO https://www.youtube.com/embed/5mMtUSmfUlw]

## <a name="faq"></a>Sık sorulan sorular

Bu bölümde, genel doğası olan yeni geliştirici portalı hakkında sık sorulan sorulara yanıt veriyoruz. Şirket içinde barındırılan sürüme özgü sorular için [GitHub deposunun wiki bölümüne](https://github.com/Azure/api-management-developer-portal/wiki)bakın.

### <a name="how-can-i-migrate-content-from-the-old-developer-portal-to-the-new-one"></a>İçeriği eski geliştirici portalından yenisine nasıl geçirebilirim?

Yapamazsınız. Portallar uyumsuzdur.

### <a name="when-will-the-portal-become-generally-available"></a>Portal ne zaman genel olarak kullanılabilir olacaktır?

Portal Şu anda önizleme aşamasındadır ve takvim yılının (2019) sonunda genel kullanıma sunulacaktır. Önizleme sürümü üretim amaçları için kullanılmamalıdır.

### <a name="will-the-old-portal-be-deprecated"></a>Eski portalın kullanım dışı olması gerekir mi?

Evet, yeni bir tane genel kullanıma sunulduğunda kullanım dışı olacaktır. Endişeleriniz varsa, bunları [özel bir GitHub sorunuyla](https://github.com/Azure/api-management-developer-portal/issues/121)yükseltin.

### <a name="does-the-new-portal-have-all-the-features-of-the-old-portal"></a>Yeni Portal eski portalın tüm özelliklerine sahip mi?

Genel kullanım amacı, eski portalla senaryo tabanlı bir özellik eşliği sağlamaktır. Bu durumda, önizleme sürümünde uygulanan özellikler uygulanmamış olabilir.

Özel durumlar, eski portaldan yeni portalda kullanılamayacak *uygulamalar* ve *sorunlardır* . Eski portalda *sorunlar* ve yeni bir tane varsa, [özel bir GitHub sorununa](https://github.com/Azure/api-management-developer-portal/issues/122)yorum gönderin.

### <a name="ive-found-bugs-andor-id-like-to-request-a-feature"></a>Hata buldum ve/veya bir özellik istemek istiyorum.

Harika! [GitHub deposunun sorunlar bölümünde](https://github.com/Azure/api-management-developer-portal/issues)bize geri bildirim verebilir, bir özellik isteği gönderebilir veya hata raporu dosyalayabilir. Bu sırada, `community` etiketle işaretlenmiş sorunlar hakkında geri bildirimlerinizi de beğeneceğiz.

### <a name="i-want-to-move-the-content-of-the-new-portal-between-environments-how-can-i-do-that-and-do-i-need-to-go-with-the-self-hosted-version"></a>Yeni portalın içeriğini ortamlar arasında taşımak istiyorum. Bunu nasıl yapabilirim ve şirket içinde barındırılan sürüme gitmem gerekir mi?

Bunu hem Portal sürümlerinde hem de şirket içinde barındırılan bir şekilde yapabilirsiniz. Yeni geliştirici portalı, API Management hizmetinizin yönetim API 'SI aracılığıyla içerik ayıklamayı destekler. API 'Ler [GitHub deposunun wiki bölümünde](https://github.com/Azure/api-management-developer-portal/wiki/)belgelenmiştir. Ayrıca, başlamanıza yardımcı olabilecek [bir komut dosyası](https://github.com/Azure/api-management-developer-portal/blob/master/scripts/migrate.bat)da yazdık.

Bu işlemi hala API Management DevOps kaynak seti ile hizalamak için çalışıyoruz.

### <a name="how-can-i-select-a-layout-when-creating-a-new-page"></a>Yeni bir *sayfa*oluştururken nasıl *Düzen* seçirim?

Bir *Düzen* , URL şablonuyla *sayfanın* URL 'siyle eşleştirerek bir sayfaya uygulanır. Örneğin, URL şablonuna `/wiki/*` sahip bir düzen, `/wiki/` segment ile her *sayfaya* uygulanır: `/wiki/getting-started`, `/wiki/styles`vb.

### <a name="why-doesnt-the-interactive-developer-console-work"></a>Etkileşimli Geliştirici Konsolu neden çalışmıyor?

Bu, büyük olasılıkla CORS ile ilgilidir. Etkileşimli konsol tarayıcıdan istemci tarafı API isteği yapar. API 'lerinize [BIR CORS ilkesi](https://docs.microsoft.com/azure/api-management/api-management-cross-domain-policies#CORS) ekleyerek CORS sorununu çözebilirsiniz. Tüm parametreleri el ile belirtebilirsiniz (örneğin, Origin as https://contoso.com) veya joker karakter `*` değeri kullanabilirsiniz).

## <a name="next-steps"></a>Sonraki adımlar

Yeni geliştirici portalı hakkında daha fazla bilgi edinin:

- [Kaynak kodlu GitHub deposu][1]
- [Portalı kendi kendine barındırma hakkındaki yönergeler][2]
- [Projenin ortak yol haritası][3]

[1]: https://aka.ms/apimdevportal
[2]: https://github.com/Azure/api-management-developer-portal/wiki
[3]: https://github.com/Azure/api-management-developer-portal/projects