---
title: Google kimlik doğrulamasını Yapılandırma-Azure App Service
description: App Services uygulamanız için Google kimlik doğrulamasını nasıl yapılandıracağınızı öğrenin.
services: app-service
documentationcenter: ''
author: mattchenderson
manager: syntaxc4
editor: ''
ms.assetid: 2b2f9abf-9120-4aac-ac5b-4a268d9b6e2b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/19/2018
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: 7b56d4e8d179e4ff073e74b4bc5242f936dc983e
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69033738"
---
# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a>App Service uygulamanızı Google oturumu kullanacak şekilde yapılandırma
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu konuda, Azure App Service Google 'ı bir kimlik doğrulama sağlayıcısı olarak kullanmak üzere nasıl yapılandırabileceğiniz gösterilmektedir.

Bu konudaki yordamı tamamlayabilmeniz için, doğrulanmış bir e-posta adresine sahip bir Google hesabınızın olması gerekir. Yeni bir Google hesabı oluşturmak için [accounts.google.com](https://go.microsoft.com/fwlink/p/?LinkId=268302) adresine gidin.

## <a name="register"> </a>Uygulamanızı Google ile kaydedin
1. [Azure Portal]oturum açın ve uygulamanıza gidin. Daha sonra Google uygulamanızı yapılandırmak için kullandığınız **URL**'nizi kopyalayın.
2. [Google API](https://go.microsoft.com/fwlink/p/?LinkId=268303) Web sitesine gidin, Google hesabı kimlik bilgilerinizle oturum açın, **proje oluştur**' a tıklayın, proje **adı**belirtin ve ardından **Oluştur**' a tıklayın.
3. Proje oluşturulduktan sonra seçin. Proje panosunda, **API 'lere genel bakış ' a**tıklayın.
4. **API 'leri ve Hizmetleri etkinleştir '** i seçin. **Google + API**araması yapın ve bunu seçin. Ardından **Etkinleştir**' e tıklayın.
5. Sol gezinti, **kimlik bilgileri** > **OAuth onay ekranında**, **e-posta adresinizi**seçin, bir **ürün adı**girin ve **Kaydet**' e tıklayın.
6. **Kimlik bilgileri** sekmesinde **kimlik bilgileri** > **OAuth istemci kimliği**oluştur ' a tıklayın.
7. "İstemci KIMLIĞI oluştur" ekranında **Web uygulaması**' nı seçin.
8. Daha önce kopyaladığınız App Service **URL 'Sini** **yetkili JavaScript kaynaklarına**yapıştırın, sonra yeniden yönlendirme URI 'NIZI **yetkili yeniden yönlendirme URI**'sine yapıştırın. Yeniden yönlendirme URI 'si, */.Auth/login/Google/callback*yoluna eklenen uygulamanızın URL 'sidir. Örneğin: `https://contoso.azurewebsites.net/.auth/login/google/callback`. HTTPS şemasını kullandığınızdan emin olun. Sonra **Oluştur**’a tıklayın.
9. Bir sonraki ekranda, istemci KIMLIĞI ve istemci gizli anahtarı değerlerini bir yere unutmayın.

    > [!IMPORTANT]
    > İstemci parolası önemli bir güvenlik kimlik bilgileridir. Bu parolayı kimseyle paylaşmayın veya bir istemci uygulaması içinde dağıtmayın.


## <a name="secrets"> </a>Uygulamanıza Google bilgilerini ekleyin
1. [Azure Portal]geri dönüp uygulamanıza gidin. **Ayarlar**' a ve ardından **kimlik doğrulama/yetkilendirme**' ye tıklayın.
2. Kimlik doğrulama/yetkilendirme özelliği etkinleştirilmemişse, geçişi **Açık**duruma getirin.
3. **Google**'a tıklayın. Daha önce edindiğiniz uygulama KIMLIĞI ve uygulama gizli anahtarı değerlerini yapıştırın ve isteğe bağlı olarak uygulamanızın gerektirdiği kapsamları etkinleştirin. Daha sonra, **Tamam**'a tıklayın.
   
   ![][1]
   
   Varsayılan olarak, App Service kimlik doğrulaması sağlar ancak site içeriğinize ve API 'lerinize yetkili erişimi kısıtlamaz. Uygulama kodunuzda kullanıcıları yetkilendirmelisiniz.
4. Seçim Sitenize erişimi yalnızca Google tarafından kimliği doğrulanan kullanıcılarla kısıtlamak için, isteğin **Google**'da **kimlik doğrulaması olmadığında gerçekleştirilecek eylemi** ayarlayın. Bu, tüm isteklerin doğrulanmasını gerektirir ve kimliği doğrulanmamış tüm istekler kimlik doğrulaması için Google 'a yönlendirilir.

> [!CAUTION]
> Erişimin bu şekilde kısıtlanması, uygulamanıza yönelik tüm çağrılar için geçerlidir. Bu, birçok tek sayfalı uygulamalarda olduğu gibi genel kullanıma açık bir giriş sayfası gerektiren uygulamalar için istenmeyebilir. Bu tür uygulamalar için, [burada](overview-authentication-authorization.md#authentication-flow)açıklandığı gibi **anonim isteklere izin ver (eylem yok)** tercih edilebilir ve uygulamanın kendisi el ile oturum açma işlemi başlar.

5. **Kaydet**’e tıklayın.

Artık uygulamanızda kimlik doğrulaması için Google 'ı kullanmaya hazırsınız.

## <a name="related-content"> </a>İlgili içerik
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: https://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure portal]: https://portal.azure.com/

