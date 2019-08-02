---
title: Microsoft hesabı kimlik doğrulamasını Yapılandırma-Azure App Service
description: Uygulama Hizmetleri uygulamanız için Microsoft hesabı kimlik doğrulamasını yapılandırma hakkında bilgi edinin.
author: mattchenderson
services: app-service
documentationcenter: ''
manager: syntaxc4
editor: ''
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/19/2018
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: 10b661f0c4b7dc45284b907e83df3c0372f97cab
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68561533"
---
# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>App Service uygulamanızı Microsoft hesabı oturum açma bilgilerini kullanacak şekilde yapılandırma
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu konuda, Azure App Service bir kimlik doğrulama sağlayıcısı olarak Microsoft hesabı kullanmak üzere nasıl yapılandırılacağı gösterilmektedir. 

## <a name="register-microsoft-account"> </a>Uygulamanızı Microsoft hesabı ile kaydedin
1. [Azure Portal]oturum açın ve uygulamanıza gidin. Uygulamanızı Microsoft hesabıyla yapılandırmak için daha sonra kullandığınız **URL**'nizi kopyalayın.
2. [**Uygulama kayıtları**](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade)gidin ve istenirse Microsoft hesabı oturum açın.
3. **Uygulama Ekle**' ye tıklayın, ardından bir uygulama adı yazın ve **Oluştur**' a tıklayın.
4. Daha sonra ihtiyacınız olacağı için **uygulama kimliğini**bir yere unutmayın. 
5. "Platformlar" altında **Platform Ekle** ' ye tıklayın ve "Web" i seçin.
6. "Yeniden yönlendirme URI 'Leri" altında uygulamanız için uç noktayı sağlayın ve **Kaydet**' e tıklayın. 
   
   > [!NOTE]
   > Yeniden yönlendirme URI 'si, */.Auth/login/MicrosoftAccount/callback*yoluna eklenen uygulamanızın URL 'sidir. Örneğin: `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.   
   > HTTPS şemasını kullandığınızdan emin olun.
   
7. "Uygulama gizli dizileri" altında **Yeni parola oluştur**' a tıklayın. Görüntülenen değeri unutmayın. Sayfadan ayrıldığınızda, bir daha görüntülenmeyecektir.

    > [!IMPORTANT]
    > Parola, önemli bir güvenlik kimlik bilgileridir. Parolayı kimseyle paylaşmayın veya bir istemci uygulaması içinde dağıtmayın.
    
8. **Kaydet**'e tıklayın.

## <a name="secrets"> </a>App Service uygulamanıza Microsoft hesabı bilgilerini ekleme
1. [Azure Portal]geri dönüp uygulamanıza gidin, **Ayarlar** > **kimlik doğrulaması/yetkilendirme**' ye tıklayın.
2. Kimlik doğrulama/yetkilendirme özelliği etkinleştirilmemişse, **üzerinde**geçiş yapın.
3. **Microsoft hesabı**' na tıklayın. Daha önce aldığınız uygulama KIMLIĞI ve parola değerlerini yapıştırın ve isteğe bağlı olarak uygulamanızın gerektirdiği kapsamları etkinleştirin. Daha sonra, **Tamam**'a tıklayın.
   
    ![][1]
   
    Varsayılan olarak, App Service kimlik doğrulaması sağlar ancak site içeriğinize ve API 'lerinize yetkili erişimi kısıtlamaz. Uygulama kodunuzda kullanıcıları yetkilendirmelisiniz.
4. Seçim Sitenize erişimi yalnızca Microsoft hesabı tarafından kimlik doğrulaması yapılan kullanıcılarla kısıtlamak için, isteğin kimliği **Microsoft hesabına**göre **doğrulanmadığı zaman gerçekleştirilecek eylemi** ayarlayın. Bu, tüm isteklerin doğrulanmasını gerektirir ve kimliği doğrulanmamış tüm istekler kimlik doğrulaması için Microsoft hesabı yönlendirilir.
5. **Kaydet**’e tıklayın.

Artık uygulamanızda kimlik doğrulaması için Microsoft hesabı 'nı kullanmaya hazırsınız.

## <a name="related-content"> </a>İlgili içerik
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure portal]: https://portal.azure.com/
