---
title: Facebook kimlik doğrulamasını Yapılandırma-Azure App Service
description: App Services uygulamanız için Facebook kimlik doğrulamasını nasıl yapılandıracağınızı öğrenin.
services: app-service
documentationcenter: ''
author: mattchenderson
manager: syntaxc4
editor: ''
ms.assetid: b6b4f062-fcb4-47b3-b75a-ec4cb51a62fd
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/06/2019
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: f4d26572dc21e2c7454fb739cb4b5fb5665bd1db
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70098572"
---
# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a>App Service uygulamanızı Facebook oturum açma bilgilerini kullanacak şekilde yapılandırma
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu konuda, Azure App Service bir kimlik doğrulama sağlayıcısı olarak Facebook kullanmak üzere nasıl yapılandırılacağı gösterilmektedir.

Bu konudaki yordamı tamamlayabilmeniz için, doğrulanmış bir e-posta adresine ve cep telefonu numarasına sahip bir Facebook hesabınızın olması gerekir. Yeni bir Facebook hesabı oluşturmak için [Facebook.com]adresine gidin.

## <a name="register"> </a>Uygulamanızı Facebook ile kaydetme
1. [Facebook geliştiricileri] Web sitesine gidin ve Facebook hesabı kimlik bilgilerinizle oturum açın.
3. Seçim Geliştiriciler için Facebook hesabınız yoksa, **kullanmaya** başlayın ' a tıklayın ve kayıt adımlarını izleyin.
4. **Uygulamalarım** > **Yeni uygulama ekle**' ye tıklayın.
5. **Görünen ad**alanına uygulamanız için benzersiz bir ad yazın. Ayrıca, **Iletişim e-postanızı**girin ve ardından **uygulama kimliği oluştur** ' a tıklayın ve güvenlik denetimini doldurun. Bu, sizi yeni Facebook uygulamanızın geliştirici panosuna götürür.
6. **Pano** > Facebook oturumaçma > Web 'i ayarla ' ya tıklayın. > 
1. **Facebook oturum açma**altındaki sol taraftaki gezinmede **Ayarlar**' a tıklayın.
1. **Geçerli OAuth yeniden yönlendirme URI 'lerinde**, `https://<app-name>.azurewebsites.net/.auth/login/facebook/callback`  *\<app-name >* yazın ve Azure App Service uygulamanızın adıyla değiştirin. Tıklayın **değişiklikleri kaydetmek**.
8. Sol taraftaki gezinmede **Ayarlar** > **temel**' ya tıklayın. **Uygulama gizli** alanından **göster**' e tıklayın. **Uygulama kimliği** ve **uygulama gizli anahtarı**değerlerini kopyalayın. Azure 'da App Service uygulamanızı yapılandırmak için bunları daha sonra kullanırsınız.
   
   > [!IMPORTANT]
   > Uygulama gizli anahtarı önemli bir güvenlik kimlik bilgileridir. Bu parolayı kimseyle paylaşmayın veya bir istemci uygulaması içinde dağıtmayın.
   > 
   > 
9. Uygulamayı kaydetmek için kullanılan Facebook hesabı, uygulamanın bir yöneticisidir. Bu noktada, yalnızca Yöneticiler bu uygulamada oturum açabilir. Diğer Facebook hesaplarının kimliğini doğrulamak için, **uygulama gözden geçirmesi** ' na tıklayın ve Facebook kimlik doğrulamasını kullanarak genel genel erişimi etkinleştirmek üzere uygulamanızın **adını \<> Genel yapın** .

## <a name="secrets"> </a>Uygulamanıza Facebook bilgilerini ekleyin
1. [Azure Portal] oturum açın ve App Service uygulamanıza gidin. Ayarlar > **kimlik doğrulaması/yetkilendirme**' ye tıklayın ve **App Service kimlik doğrulamasının** **Açık**olduğundan emin olun.
2. Daha önce aldığınız uygulama KIMLIĞI ve uygulama gizli anahtarı ' nda **Facebook**' a tıklayın, isteğe bağlı olarak uygulamanızın gerektirdiği kapsamları etkinleştirin ve **Tamam**' a tıklayın.
   
    ![][0]
   
    Varsayılan olarak, App Service kimlik doğrulaması sağlar ancak site içeriğinize ve API 'lerinize yetkili erişimi kısıtlamaz. Uygulama kodunuzda kullanıcıları yetkilendirmelisiniz.
3. Seçim Sitenize erişimi yalnızca Facebook tarafından kimliği doğrulanan kullanıcılarla kısıtlamak için, isteğin **Facebook 'ta** **kimlik doğrulaması olmadığında gerçekleştirilecek eylemi** ayarlayın. Bu, tüm isteklerin doğrulanmasını ve kimliği doğrulanmamış tüm isteklerin kimlik doğrulaması için Facebook 'a yönlendirilmesini gerektirir.
 
> [!CAUTION]
> Erişimin bu şekilde kısıtlanması, uygulamanıza yönelik tüm çağrılar için geçerlidir. Bu, birçok tek sayfalı uygulamalarda olduğu gibi genel kullanıma açık bir giriş sayfası gerektiren uygulamalar için istenmeyebilir. Bu tür uygulamalar için, [burada](overview-authentication-authorization.md#authentication-flow)açıklandığı gibi **anonim isteklere izin ver (eylem yok)** tercih edilebilir ve uygulamanın kendisi el ile oturum açma işlemi başlar.

4. Kimlik doğrulamasını yapılandırmayı tamamladığınızda **Kaydet**' e tıklayın.

Artık uygulamanızda kimlik doğrulaması için Facebook kullanmaya hazırsınız.

## <a name="related-content"> </a>İlgili içerik
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook geliştiricileri]: https://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: https://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure portal]: https://portal.azure.com/
