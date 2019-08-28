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
ms.topic: article
ms.date: 08/08/2019
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: 5ef0cb2da26fcc00d1daf4b2dd0faf8bde8b743b
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70098534"
---
# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>App Service uygulamanızı Microsoft hesabı oturum açma bilgilerini kullanacak şekilde yapılandırma
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu konuda, Azure App Service bir kimlik doğrulama sağlayıcısı olarak Microsoft hesabı kullanmak üzere nasıl yapılandırılacağı gösterilmektedir. 

## <a name="register-microsoft-account"> </a>Uygulamanızı Microsoft hesabı ile kaydedin
1. [Azure Portal]oturum açın ve uygulamanıza gidin. 

<!-- Copy your **URL**, which you will use later to configure your app with Microsoft Account. -->
1. [**Uygulama kayıtları**](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade)gidin ve istenirse Microsoft hesabı oturum açın.

1. **Yeni kayıt**' ye tıklayın, ardından bir uygulama adı yazın.

1. **Yeniden yönlendirme URI 'lerinde**, **Web**' i seçin ve `https://<app-domain-name>/.auth/login/microsoftaccount/callback supply the endpoint for your application`yazın. *\<App-Domain-Name >* değerini uygulamanızın etki alanı adıyla değiştirin.  Örneğin: `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`. 

   > [!NOTE]
   > URL 'de HTTPS şemasını kullanın.

1. **Kaydol**' u seçin. 

1. **Uygulama (istemci) kimliğini**kopyalayın. Daha sonra ihtiyacınız olacak. 
   
7. Yeni uygulama kaydının sol gezinti bölmesinde, **Sertifikalar & gizli** > **anahtar istemci parolası**' nı seçin. Bir açıklama girin, geçerlilik süresini seçin ve **Ekle**' yi seçin.

1. **Sertifikalar & gizlilikler** sayfasında görüntülenen değeri kopyalayın. Sayfadan ayrıldığınızda, bir daha görüntülenmeyecektir.

    > [!IMPORTANT]
    > Parola, önemli bir güvenlik kimlik bilgileridir. Parolayı kimseyle paylaşmayın veya bir istemci uygulaması içinde dağıtmayın.

## <a name="secrets"> </a>App Service uygulamanıza Microsoft hesabı bilgilerini ekleme
1. [Azure Portal]uygulamanıza gidin. Sol gezinmeden **kimlik doğrulama/yetkilendirme**' ye tıklayın.

2. Kimlik doğrulama/yetkilendirme özelliği etkinleştirilmemişse **Açık**' ı seçin.

3. **Kimlik doğrulama sağlayıcıları**altında **Microsoft hesabı**' nı seçin. Daha önce edindiğiniz uygulama (istemci) KIMLIĞINI ve istemci gizli anahtarını yapıştırın ve isteğe bağlı olarak uygulamanızın gerektirdiği tüm kapsamları etkinleştirin. Daha sonra, **Tamam**'a tıklayın.

    Varsayılan olarak, App Service kimlik doğrulaması sağlar ancak site içeriğinize ve API 'lerinize yetkili erişimi kısıtlamaz. Uygulama kodunuzda kullanıcıları yetkilendirmelisiniz.

4. Seçim Microsoft hesabı kullanıcılara erişimi kısıtlamak için, **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylemi** **Microsoft hesabıyla oturum**açmak üzere ayarlayın. Bu, tüm isteklerin doğrulanmasını gerektirir ve kimliği doğrulanmamış tüm istekler kimlik doğrulaması için Microsoft hesabı yönlendirilir.

> [!CAUTION]
> Erişimin bu şekilde kısıtlanması, uygulamanıza yönelik tüm çağrılar için geçerlidir. Bu, birçok tek sayfalı uygulamalarda olduğu gibi genel kullanıma açık bir giriş sayfası gerektiren uygulamalar için istenmeyebilir. Bu tür uygulamalar için, [burada](overview-authentication-authorization.md#authentication-flow)açıklandığı gibi **anonim isteklere izin ver (eylem yok)** tercih edilebilir ve uygulamanın kendisi el ile oturum açma işlemi başlar.

5. **Kaydet**’e tıklayın.

Artık uygulamanızda kimlik doğrulaması için Microsoft hesabı 'nı kullanmaya hazırsınız.

## <a name="related-content"> </a>İlgili içerik
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- URLs. -->

[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure portal]: https://portal.azure.com/
