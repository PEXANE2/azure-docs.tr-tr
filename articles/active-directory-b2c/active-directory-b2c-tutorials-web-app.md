---
title: Öğretici-bir Web uygulamasında kimlik doğrulamasını etkinleştirme-Azure Active Directory B2C
description: Bir ASP.NET web uygulamasında kullanıcının oturum açmasını sağlamak için Azure Active Directory B2C’nin nasıl kullanılacağını gösteren öğretici.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 09/19/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: b42634aa86f210382adb1ae224c847a92d89109b
ms.sourcegitcommit: 1c9858eef5557a864a769c0a386d3c36ffc93ce4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71103318"
---
# <a name="tutorial-enable-authentication-in-a-web-application-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C kullanarak bir Web uygulamasında kimlik doğrulamasını etkinleştirme

Bu öğreticide, bir ASP.NET Web uygulamasındaki kullanıcıları oturum açmak ve kaydolmak için Azure Active Directory B2C (Azure AD B2C) nasıl kullanılacağı gösterilmektedir. Azure AD B2C, uygulamalarınızın, açık standart protokoller kullanarak sosyal hesaplar, kurumsal hesaplar ve Azure Active Directory hesaplar için kimlik doğrulaması yapmasına olanak sağlar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD B2C içinde uygulamayı güncelleştirme
> * Uygulamayı kullanmak için örneği yapılandırma
> * Kullanıcı akışını kullanarak kaydolma

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* Uygulamanızdaki kullanıcı deneyimlerini etkinleştirmek için [Kullanıcı akışları oluşturun](tutorial-create-user-flows.md) .
* **ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 2019](https://www.visualstudio.com/downloads/) ' i yükledikten sonra.

## <a name="update-the-application"></a>Uygulamayı güncelleştirme

Önkoşulların bir parçası olarak tamamladığınız öğreticide, Azure AD B2C bir Web uygulaması eklediniz. Bu öğreticideki örnekle iletişimi etkinleştirmek için, Azure AD B2C ' de uygulamaya bir yeniden yönlendirme URI 'SI eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçip kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
1. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
1. **Uygulamalar**' ı seçin ve ardından *WebApp1* uygulamasını seçin.
1. **Yanıt URL 'si**altında, `https://localhost:44316`ekleyin.
1. **Kaydet**’i seçin.
1. Özellikler sayfasında, Web uygulamasını yapılandırırken kullanacağınız uygulama KIMLIĞINI kaydedin.
1. **Anahtarlar**' ı seçin, **anahtar oluştur**' u ve **Kaydet**' i seçin. Web uygulamasını yapılandırırken kullanacağınız anahtarı kaydedin.

## <a name="configure-the-sample"></a>Örneği yapılandırma

Bu öğreticide, GitHub 'dan indirebileceğiniz bir örnek yapılandırırsınız. Örnek, basit bir yapılacaklar listesi sağlamak için ASP.NET kullanır. Örnek, [Microsoft OWIN ara yazılım bileşenlerini](https://docs.microsoft.com/aspnet/aspnet/overview/owin-and-katana/)kullanır. GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi/archive/master.zip) veya örneği kopyalayın. Örnek dosyayı, yolunun toplam karakter uzunluğu 260'tan az olan bir klasöre ayıkladığınızdan emin olun.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Aşağıdaki iki proje örnek çözümde bulunur:

* **Taskwebapp** -bir görev listesi oluşturun ve düzenleyin. Örnek, kullanıcıların kaydolması ve oturum açması için kaydolma **veya oturum açma** Kullanıcı akışını kullanır.
* **Taskservice** -oluşturma, okuma, güncelleştirme ve silme görev listesi işlevlerini destekler. API Azure AD B2C tarafından korunur ve TaskWebApp tarafından çağırılır.

Örnek, kiracınızda kayıtlı olan uygulamayı kullanacak şekilde, uygulama KIMLIĞI ve daha önce kaydettiğiniz anahtar dahil olmak üzere değiştirirsiniz. Ayrıca, oluşturduğunuz Kullanıcı akışlarını da yapılandırırsınız. Örnek, yapılandırma değerlerini *Web. config* dosyasında ayarlar olarak tanımlar.

Web. config dosyasındaki ayarları Kullanıcı akışınız ile çalışacak şekilde güncelleştirin:

1. **B2C-WebAPI-DotNet** çözümünü Visual Studio’da açın.
1. **Taskwebapp** projesinde **Web. config** dosyasını açın.
    1. `ida:Tenant` Ve`ida:AadInstance` değerlerini, oluşturduğunuz Azure AD B2C kiracının adıyla güncelleştirin. Örneğin, ile `contoso`değiştirin `fabrikamb2c` .
    1. Değerini `ida:ClientId` , kaydettiğiniz uygulama kimliğiyle değiştirin.
    1. `ida:ClientSecret` değerini kaydettiğiniz anahtarla değiştirin. Web. config dosyasına eklemeden önce, istemci gizliliğini XML olarak kodlamanız gerekir.
    1. Değerini `ida:SignUpSignInPolicyId` ile`b2c_1_signupsignin1`değiştirin.
    1. Değerini `ida:EditProfilePolicyId` ile`b2c_1_profileediting1`değiştirin.
    1. Değerini `ida:ResetPasswordPolicyId` ile`b2c_1_passwordreset1`değiştirin.

## <a name="run-the-sample"></a>Örneği çalıştırma

1. Çözüm Gezgini, **Taskwebapp** projesine sağ tıklayın ve ardından **Başlangıç projesi olarak ayarla**' ya tıklayın.
1. **F5**tuşuna basın. Varsayılan tarayıcı, `https://localhost:44316/` olan yerel web sitesi adresini açar.

### <a name="sign-up-using-an-email-address"></a>E-posta adresi kullanarak kaydolma

1. Uygulamanın bir kullanıcısı olarak kaydolmak için **kaydolun/oturum aç '** ı seçin. **B2c_1_signupsignin1** Kullanıcı akışı kullanılır.
1. Azure AD B2C, kayıt bağlantısı içeren bir oturum açma sayfası sunar. Henüz bir hesabınız olmadığından **Şimdi kaydolun**' ı seçin. Kaydolma iş akışında, kullanıcı kimliğini bir e-posta adresi kullanarak toplamak ve doğrulamak için bir sayfa sunar. Kaydolma iş akışı, kullanıcının parolasını ve Kullanıcı akışında tanımlanan istenen öznitelikleri de toplar.
1. Geçerli bir e-posta adresi kullanın ve doğrulama kodunu kullanarak doğrulamayı gerçekleştirin. Parola ayarlayın. İstenen öznitelikler için değerleri girin.

    ![Oturum açma/kaydolma iş akışının bir parçası olarak gösterilen kaydolma sayfası](media/active-directory-b2c-tutorials-web-app/sign-up-workflow.PNG)

1. Azure AD B2C kiracısında yerel hesap oluşturmak için **Oluştur** ' u seçin.

Uygulama kullanıcısı artık kendi e-posta adreslerini kullanarak oturum açabilir ve Web uygulamasını kullanabilir.

Ancak, bu yapılacaklar **listesi** özelliği, serideki bir sonraki öğreticiyi tamamlayana kadar çalışmaz, [öğretici: Bir ASP.NET Web API 'sini](active-directory-b2c-tutorials-web-api.md)korumak için Azure AD B2C kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure AD B2C içinde uygulamayı güncelleştirme
> * Uygulamayı kullanmak için örneği yapılandırma
> * Kullanıcı akışını kullanarak kaydolma

Şimdi, Web uygulamasının Yapılacaklar **listesi** özelliğini etkinleştirmek için sonraki öğreticiye geçin. Bu durumda, bir Web API uygulamasını kendi Azure AD B2C kiracınıza kaydeder ve sonra API kimlik doğrulaması için kiracınızı kullanacak şekilde kod örneğini değiştirirsiniz.

> [!div class="nextstepaction"]
> [Öğretici: Bir ASP.NET Web API 'sini korumak için Azure Active Directory B2C kullanın >](active-directory-b2c-tutorials-web-api.md)
