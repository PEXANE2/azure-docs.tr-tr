---
title: 'Öğretici: Bir web uygulamasında kimlik doğrulamayı etkinleştirme'
titleSuffix: Azure AD B2C
description: Bir ASP.NET web uygulamasında kullanıcının oturum açmasını sağlamak için Azure Active Directory B2C’nin nasıl kullanılacağını gösteren öğretici.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 10/14/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: e4b56f18bf8a2ed1c22b00b8a57efdbf06eb7fa2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78183345"
---
# <a name="tutorial-enable-authentication-in-a-web-application-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C'yi kullanarak bir web uygulamasında kimlik doğrulamayı etkinleştirme

Bu öğretici, kullanıcıları ASP.NET bir web uygulamasında oturum açmak ve kaydolmak için Azure Active Directory B2C'yi (Azure AD B2C) nasıl kullanacağınızı gösterir. Azure AD B2C, uygulamalarınızın açık standart protokoller kullanarak sosyal hesaplara, kurumsal hesaplara ve Azure Etkin Dizin hesaplarına kimlik doğrulamasını sağlar.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure AD B2C'deki uygulamayı güncelleştirme
> * Uygulamayı kullanacak şekilde örneği yapılandırma
> * Kullanıcı akışını kullanarak kaydolun

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* Uygulamanızda kullanıcı deneyimlerini etkinleştirmek için [kullanıcı akışları oluşturun.](tutorial-create-user-flows.md)
* Visual [Studio 2019'u](https://www.visualstudio.com/downloads/) **ASP.NET ve web geliştirme** iş yüküyle yükleyin.

## <a name="update-the-application-registration"></a>Başvuru kaydını güncelleştirin

Ön koşulların bir parçası olarak tamamladığınız eğitimde, Azure AD B2C'ye bir web uygulaması kaydettiniz. Bu öğreticideki örnekle iletişimi etkinleştirmek için, yeniden yönlendirme URI'si eklemeniz ve kayıtlı uygulama için bir istemci sırrı (anahtar) oluşturmanız gerekir.

### <a name="add-a-redirect-uri-reply-url"></a>Yeniden yönlendirme URI'si ekleme (yanıt URL'si)

Uygulamayı güncellemek için geçerli **Uygulamalar** deneyimini veya yeni birleştirilmiş **Uygulama kayıtları (Önizleme)** deneyimimizi kullanabilirsiniz. [Yeni deneyim hakkında daha fazla bilgi edinin.](https://aka.ms/b2cappregintro)

#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve kiracınızı içeren dizin seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
1. **Uygulamalar'ı**seçin ve ardından *webapp1* uygulamasını seçin.
1. **Yanıtla URL'nin**altında , ekle `https://localhost:44316`.
1. **Kaydet'i**seçin.
1. Özellikler sayfasında, web uygulamasını yapılandırDığınızda daha sonraki bir adımda kullanılmak üzere uygulama kimliğini kaydedin.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından Azure AD B2C kiracınızı içeren dizin'i seçin.
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. **Uygulama kayıtları (Önizleme) seçin,** **Sahip olunan uygulamalar** sekmesini seçin ve ardından *webapp1* uygulamasını seçin.
1. **Kimlik Doğrulama'yı**seçin, ardından yeni **deneyimi deneyin** 'i seçin (gösterilirse).
1. **Web'in**altında **URI ekle** `https://localhost:44316`bağlantısını seçin, enter 'u girin ve sonra **Kaydet'i**seçin.
1. **Genel Bakış**’ı seçin.
1. Web uygulamasını yapılandırDığınızda daha sonraki bir adımda kullanılmak üzere **Uygulama (istemci) kimliğini** kaydedin.

* * *

### <a name="create-a-client-secret"></a>İstemci sırrı oluşturma

Ardından, kayıtlı web uygulaması için bir istemci sırrı oluşturun. Web uygulama kodu örneği, jeton isteğinde kimliğini kanıtlamak için bunu kullanır.

[!INCLUDE [active-directory-b2c-client-secret](../../includes/active-directory-b2c-client-secret.md)]

## <a name="configure-the-sample"></a>Örneği yapılandırma

Bu öğreticide, GitHub'dan indirebileceğiniz bir örneği yapılandırırsınız. Örnek, basit bir yapılacaklar listesi sağlamak için ASP.NET kullanır. Örnek [Microsoft OWIN ara yazılım bileşenleri](https://docs.microsoft.com/aspnet/aspnet/overview/owin-and-katana/)kullanır. GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi/archive/master.zip) veya örneği kopyalayın. Örnek dosyayı, yolunun toplam karakter uzunluğu 260'tan az olan bir klasöre ayıkladığınızdan emin olun.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Aşağıdaki iki proje örnek çözümdedir:

* **TaskWebApp** - Görev listesi oluşturun ve güncelle. Örnek, **kaydolmak veya** oturum açmak için kullanıcı akışını kullanır.
* **TaskService** - Görev listesi oluşturma, okuma, güncelleştirme ve silme işlevini destekler. API, Azure AD B2C tarafından korunur ve TaskWebApp tarafından çağrılır.

Kiracınızda kayıtlı olan ve başvuru kimliğini ve daha önce kaydettiğiniz anahtarı içeren uygulamayı kullanmak için örneği değiştirirsiniz. Ayrıca oluşturduğunuz kullanıcı akışlarını da yapılandırırsınız. Örnek, *web.config* dosyasındaki yapılandırma değerlerini ayarlar olarak tanımlar.

Kullanıcı akışınız ile çalışmak için Web.config dosyasındaki ayarları güncelleştirin:

1. **B2C-WebAPI-DotNet** çözümünü Visual Studio’da açın.
1. **TaskWebApp** projesinde **Web.config** dosyasını açın.
    1. Oluşturduğunuz Azure `ida:Tenant` `ida:AadInstance` AD B2C kiracısının değerini ve adını güncelleştirin. Örneğin, `fabrikamb2c` `contoso`değiştirin.
    1. Kaydettiğiniz uygulama `ida:ClientId` kimliğinin değerini değiştirin.
    1. `ida:ClientSecret` değerini kaydettiğiniz anahtarla değiştirin. İstemci sırrı önceden tanımlanmış XML varlıkları içeriyorsa(`<`),`>`(), ampersand (),`&``"`(), veya çift tırnak (), Web.config'inize eklemeden önce istemci sırrını XML kodlayarak bu karakterlerden kaçmalısınız.
    1. Değerini `ida:SignUpSignInPolicyId` `b2c_1_signupsignin1`değiştirin.
    1. Değerini `ida:EditProfilePolicyId` `b2c_1_profileediting1`değiştirin.
    1. Değerini `ida:ResetPasswordPolicyId` `b2c_1_passwordreset1`değiştirin.

## <a name="run-the-sample"></a>Örneği çalıştırma

1. Solution Explorer'da **TaskWebApp** projesine sağ tıklayın ve ardından **StartUp Project olarak ayarla'yı**tıklatın.
1. **F5 tuşuna**basın. Varsayılan tarayıcı, `https://localhost:44316/` olan yerel web sitesi adresini açar.

### <a name="sign-up-using-an-email-address"></a>E-posta adresi kullanarak kaydolma

1. Uygulamanın kullanıcısı olarak kaydolmak için **Kaydol / Oturum Aç'ı** seçin. **b2c_1_signupsignin1** kullanıcı akışı kullanılır.
1. Azure AD B2C, kayıt bağlantısı içeren bir oturum açma sayfası sunar. Henüz bir hesabınız olmadığından, şimdi **Kaydol'u**seçin. Kaydolma iş akışında, kullanıcı kimliğini bir e-posta adresi kullanarak toplamak ve doğrulamak için bir sayfa sunar. Kaydolma iş akışı, kullanıcının parolasını ve kullanıcı akışında tanımlanan istenen öznitelikleri de toplar.
1. Geçerli bir e-posta adresi kullanın ve doğrulama kodunu kullanarak doğrulamayı gerçekleştirin. Parola ayarlayın. İstenen öznitelikler için değerleri girin.

    ![Oturum açma/kaydolma iş akışının bir parçası olarak gösterilen kaydolma sayfası](./media/tutorial-web-app-dotnet/sign-up-workflow.PNG)

1. Azure AD B2C kiracısında yerel bir hesap oluşturmak için **Oluştur'u** seçin.

Uygulama kullanıcısı artık oturum açma ve web uygulamasını kullanmak için e-posta adresini kullanabilir.

Ancak, **Yapılacaklar Listesi** özelliği, serinin bir sonraki öğreticisini tamamlayana kadar çalışmaz, [Öğretici: ASP.NET bir web API'sını korumak için Azure AD B2C'yi kullanın.](tutorial-web-api-dotnet.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure AD B2C'deki uygulamayı güncelleştirme
> * Uygulamayı kullanacak şekilde örneği yapılandırma
> * Kullanıcı akışını kullanarak kaydolun

Şimdi web uygulamasının **Yapılacaklar Listesi** özelliğini etkinleştirmek için bir sonraki öğreticiye geçin. Bu uygulamada, kendi Azure AD B2C kiracınızda bir web API uygulaması kaydeder ve ardından api kimlik doğrulaması için kiracınızı kullanmak üzere kod örneğini değiştirirsiniz.

> [!div class="nextstepaction"]
> [Öğretici: ASP.NET web API >korumak için Azure Active Directory B2C'yi kullanın](tutorial-web-api-dotnet.md)
