---
title: Öğretici-bir Web uygulamasında kimlik doğrulamasını etkinleştirme-Azure Active Directory B2C | Microsoft Docs
description: Bir ASP.NET web uygulamasında kullanıcının oturum açmasını sağlamak için Azure Active Directory B2C’nin nasıl kullanılacağını gösteren öğretici.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: bcfd1ef02c68de7709cb8642b94f23a6884ea156
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68464757"
---
# <a name="tutorial-enable-authentication-in-a-web-application-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C kullanarak bir Web uygulamasında kimlik doğrulamasını etkinleştirme

Bu öğreticide, bir ASP.NET Web uygulamasındaki kullanıcıları oturum açmak ve kaydolmak için Azure Active Directory (Azure AD) B2C 'nin nasıl kullanılacağı gösterilmektedir. Azure AD B2C, uygulamalarınızın, açık standart protokoller kullanarak sosyal hesaplar, kurumsal hesaplar ve Azure Active Directory hesaplar için kimlik doğrulaması yapmasına olanak sağlar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure AD B2C içinde uygulamayı güncelleştirme
> * Uygulamayı kullanmak için örneği yapılandırma
> * Kullanıcı akışını kullanarak kaydolma

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

- Uygulamanızdaki kullanıcı deneyimlerini etkinleştirmek için [Kullanıcı akışları oluşturun](tutorial-create-user-flows.md) .
- **ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 2019](https://www.visualstudio.com/downloads/) ' i yükledikten sonra.

## <a name="update-the-application"></a>Uygulamayı güncelleştirme

Önkoşulların bir parçası olarak tamamladığınız öğreticide, Azure AD B2C bir Web uygulaması eklediniz. Bu öğreticideki örnekle iletişimi etkinleştirmek için, Azure AD B2C ' de uygulamaya bir yeniden yönlendirme URI 'SI eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
4. **Uygulamalar**' ı seçin ve ardından *WebApp1* uygulamasını seçin.
5. **Yanıt URL 'si**altında, `https://localhost:44316`ekleyin.
6. **Kaydet**’i seçin.
7. Özellikler sayfasında, Web uygulamasını yapılandırırken kullanacağınız uygulama KIMLIĞINI kaydedin.
8. **Anahtarlar**' ı seçin, **anahtar oluştur**' u ve **Kaydet**' i seçin. Web uygulamasını yapılandırırken kullanacağınız anahtarı kaydedin.

## <a name="configure-the-sample"></a>Örneği yapılandırma

Bu öğreticide, GitHub 'dan indirebileceğiniz bir örnek yapılandırırsınız. Örnek, basit bir yapılacaklar listesi sağlamak için ASP.NET kullanır. Örnek, [Microsoft OWIN ara yazılım bileşenlerini](https://docs.microsoft.com/aspnet/aspnet/overview/owin-and-katana/)kullanır. GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi/archive/master.zip) veya örneği kopyalayın. Örnek dosyayı, yolunun toplam karakter uzunluğu 260'tan az olan bir klasöre ayıkladığınızdan emin olun.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi.git
```

Aşağıdaki iki proje örnek çözümde bulunur:

- **Taskwebapp** -bir görev listesi oluşturun ve düzenleyin. Örnek, kullanıcıların kaydolması veya oturum açması için kaydolma **veya oturum açma** Kullanıcı akışını kullanır.
- **Taskservice** -oluşturma, okuma, güncelleştirme ve silme görev listesi işlevlerini destekler. API Azure AD B2C tarafından korunur ve TaskWebApp tarafından çağırılır.

Örnek, kiracınızda kayıtlı olan uygulamayı kullanacak şekilde, uygulama KIMLIĞI ve daha önce kaydettiğiniz anahtar dahil olmak üzere değiştirirsiniz. Ayrıca, oluşturduğunuz Kullanıcı akışlarını da yapılandırırsınız. Örnek, yapılandırma değerlerini Web. config dosyasında ayarlar olarak tanımlar. Ayarları değiştirmek için:

1. **B2C-WebAPI-DotNet** çözümünü Visual Studio’da açın.
2. **Taskwebapp** projesinde **Web. config** dosyasını açın. `ida:Tenant` değerini oluşturduğunuz kiracının adıyla değiştirin. `ida:ClientId` değerini kaydettiğiniz uygulama kimliğiyle değiştirin. `ida:ClientSecret` değerini kaydettiğiniz anahtarla değiştirin. Web. config dosyasına eklemeden önce, istemci gizliliğini XML olarak kodlamanız gerekir.
3. **Web.config** dosyasında `ida:SignUpSignInPolicyId` değerini `b2c_1_signupsignin1` ile değiştirin. `ida:EditProfilePolicyId` değerini `b2c_1_profileediting1` ile değiştirin. `ida:ResetPasswordPolicyId` değerini `b2c_1_passwordreset1` ile değiştirin.


## <a name="run-the-sample"></a>Örneği çalıştırma

1. Çözüm Gezgini, **Taskwebapp** projesine sağ tıklayın ve ardından **Başlangıç projesi olarak ayarla**' ya tıklayın.
2. **F5**tuşuna basın. Varsayılan tarayıcı, `https://localhost:44316/` olan yerel web sitesi adresini açar.

### <a name="sign-up-using-an-email-address"></a>E-posta adresi kullanarak kaydolma

1. Uygulamanın kullanıcısı olarak kaydolmak için **kaydolun/oturum aç** ' a tıklayın. **B2c_1_signupsignin1** Kullanıcı akışı kullanılır.
2. Azure AD B2C, kayıt bağlantısı içeren bir oturum açma sayfası sunar. Henüz bir hesabınız olmadığından **Şimdi kaydolun**' ı seçin. Kaydolma iş akışında, kullanıcı kimliğini bir e-posta adresi kullanarak toplamak ve doğrulamak için bir sayfa sunar. Kaydolma iş akışı, kullanıcının parolasını ve Kullanıcı akışında tanımlanan istenen öznitelikleri de toplar.
3. Geçerli bir e-posta adresi kullanın ve doğrulama kodunu kullanarak doğrulamayı gerçekleştirin. Parola ayarlayın. İstenen öznitelikler için değerleri girin.

    ![Oturum açma/kaydolma iş akışının bir parçası olarak gösterilen kaydolma sayfası](media/active-directory-b2c-tutorials-web-app/sign-up-workflow.PNG)

4. Azure AD B2C kiracısında yerel bir hesap oluşturmak için **Oluştur**’a tıklayın.

Artık Kullanıcı, oturum açmak ve Web uygulamasını kullanmak için e-posta adresini kullanabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure AD B2C içinde uygulamayı güncelleştirme
> * Uygulamayı kullanmak için örneği yapılandırma
> * Kullanıcı akışını kullanarak kaydolma

> [!div class="nextstepaction"]
> [Öğretici: Bir ASP.NET Web API 'sini korumak için Azure Active Directory B2C kullanma](active-directory-b2c-tutorials-web-api.md)
