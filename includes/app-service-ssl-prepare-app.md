---
title: include dosyası
description: include dosyası
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 10/15/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 4c5c0197ab6cecbba4bac4c0bff5ef76de24b6de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78301020"
---
## <a name="prepare-your-web-app"></a>Web uygulamanızı hazırlama

Özel güvenlik bağlayıcıları oluşturmak veya Uygulama Hizmeti uygulamanız için istemci sertifikalarını etkinleştirmek için [Uygulama Hizmeti planınızın](https://azure.microsoft.com/pricing/details/app-service/) **Temel,** **Standart,** **Premium**veya **İzole** katmanında olması gerekir. Bu adımda, web uygulamanızın desteklenen bir fiyatlandırma katmanında olduğundan emin olacaksınız.

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure [portalını](https://portal.azure.com)açın.

### <a name="navigate-to-your-web-app"></a>Web uygulamanıza gidin

**App Services'ı**arayın ve seçin.

![Uygulama Hizmetlerini Seçin](./media/app-service-ssl-prepare-app/app-services.png)

Uygulama **Hizmetleri** sayfasında web uygulamanızın adını seçin.

![Azure uygulamasına portal gezintisi](./media/app-service-ssl-prepare-app/select-app.png)

Web uygulamanızın yönetim sayfasına indiniz.  

### <a name="check-the-pricing-tier"></a>Fiyatlandırma katmanını denetleme

Web uygulaması sayfasının sol gezinti bölmesinde **Ayarlar** bölümüne kaydırın ve **Ölçeği artır (App Service planı)** öğesini seçin.

![Ölçeği artır menüsü](./media/app-service-ssl-prepare-app/scale-up-menu.png)

Web uygulamanızın **F1** veya **D1** katmanında olmadığından emin olun. Web uygulamanızın geçerli katmanı koyu mavi bir kutuyla vurgulanır.

![Fiyatlandırma katmanını denetleyin](./media/app-service-ssl-prepare-app/check-pricing-tier.png)

**F1** veya **D1** katmanında özel SSL desteklenmez. Ölçeği artırmanız gerekirse sonraki bölümde verilen adımları izleyin. Aksi takdirde, **Sayfayı Ölçekle'yi** kapatın ve Uygulama Hizmeti planı nızı [ölçeklendir'i](#scale-up-your-app-service-plan) atlayın.

### <a name="scale-up-your-app-service-plan"></a>App Service planınızın ölçeğini artırma

Ücretsiz olmayan katmanlardan birini seçin (**B1**, **B2**, **B3**, veya **Üretim** kategorisindeki herhangi bir katmanı). Ek seçenekler için **Ek seçeneklere bakın**’a tıklayın.

**Uygula**’ya tıklayın.

![Fiyatlandırma katmanı seçme](./media/app-service-ssl-prepare-app/choose-pricing-tier.png)

Aşağıdaki bildirimi gördüğünüzde, ölçeklendirme işlemi tamamlanmıştır.

![Ölçek artırma bildirimi](./media/app-service-ssl-prepare-app/scale-notification.png)

