---
title: 'Öğretici: Yerel istemci uygulamasında kullanıcıları doğrulama'
titleSuffix: Azure AD B2C
description: Bir .NET masaüstü uygulaması için kullanıcı girişi sağlamak için Azure Active Directory B2C'nin nasıl kullanılacağı hakkında öğretici.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 10/12/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 06d27c3a3daa4702653a2063d0ac70fd094e2d74
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78186208"
---
# <a name="tutorial-authenticate-users-in-a-native-desktop-client-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C'yi kullanarak kullanıcıların yerel bir masaüstü istemcisinde kimliğini doğrulama

Bu öğretici, bir Windows Presentation Foundation (WPF) masaüstü uygulamasında oturum açmak ve kullanıcıları kaydetmek için Azure Active Directory B2C'yi (Azure AD B2C) nasıl kullanacağınızı gösterir. Azure AD B2C, uygulamalarınızın açık standart protokoller kullanarak sosyal hesaplara, kurumsal hesaplara ve Azure Etkin Dizin hesaplarına kimlik doğrulamasını sağlar.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Yerel istemci uygulamasını ekleme
> * Uygulamayı kullanacak şekilde örneği yapılandırma
> * Kullanıcı akışını kullanarak kaydolun

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

- Uygulamanızda kullanıcı deneyimlerini etkinleştirmek için [kullanıcı akışları oluşturun.](tutorial-create-user-flows.md)
- **.NET masaüstü geliştirme** ve ASP.NET ve web geliştirme iş yükleri ile [Visual Studio 2019'u](https://www.visualstudio.com/downloads/) **yükleyin.**

## <a name="add-the-native-client-application"></a>Yerel istemci uygulamasını ekleme

[!INCLUDE [active-directory-b2c-appreg-native](../../includes/active-directory-b2c-appreg-native.md)]

Uygulama **(istemci) kimliğini** daha sonraki bir adımda kullanmak üzere kaydedin.

## <a name="configure-the-sample"></a>Örneği yapılandırma

Bu öğreticide, GitHub'dan indirebileceğiniz bir örneği yapılandırırsınız. Örnek WPF masaüstü uygulaması, Kaydolma, Kaydolma ve Azure AD B2C'de korumalı web API'sini çağırabileceğini gösterir. [Bir zip dosyası indirin](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop/archive/msalv3.zip), [depoya göz atın](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop) veya GitHub’dan örneği kopyalayın.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop.git
```

Uygulamayı Azure AD B2C kiracınızla çalışacak şekilde güncelleştirmek ve varsayılan demo kiracısı yerine kullanıcı akışlarını çağırmak için:

1. Visual Studio'da **active-directory-b2c-wpf** çözümlerini`active-directory-b2c-wpf.sln`açın.
2. **Active-directory-b2c-wpf** *projesinde, App.xaml.cs* dosyasını açın ve aşağıdaki değişken tanımlarını bulun. Azure `{your-tenant-name}` AD B2C kiracı adınız ve `{application-ID}` daha önce kaydettiğiniz uygulama kimliğiyle değiştirin.

    ```csharp
    private static readonly string Tenant = "{your-tenant-name}.onmicrosoft.com";
    private static readonly string AzureAdB2CHostname = "{your-tenant-name}.b2clogin.com";
    private static readonly string ClientId = "{application-ID}";
    ```

3. İlke adı değişkenlerini, ön koşulların bir parçası olarak oluşturduğunuz kullanıcı akışlarının adlarıyla güncelleştirin. Örnek:

    ```csharp
    public static string PolicySignUpSignIn = "B2C_1_signupsignin1";
    public static string PolicyEditProfile = "B2C_1_profileediting1";
    public static string PolicyResetPassword = "B2C_1_passwordreset1";
    ```

## <a name="run-the-sample"></a>Örneği çalıştırma

Örneği oluşturmak ve çalıştırmak için **F5** tuşuna basın.

### <a name="sign-up-using-an-email-address"></a>E-posta adresi kullanarak kaydolma

1. Kullanıcı olarak kaydolmak için **Oturum Aç'ı** seçin. Bu, **B2C_1_signupsignin1** kullanıcı akışını kullanır.
2. Azure AD B2C, **şimdi Kaydol** bağlantısını içeren bir oturum açma sayfası sunar. Henüz bir hesabınız olmadığından, **şimdi kaydol** bağlantısını seçin.
3. Kaydolma iş akışında, kullanıcı kimliğini bir e-posta adresi kullanarak toplamak ve doğrulamak için bir sayfa sunar. Kaydolma iş akışı, kullanıcının parolasını ve kullanıcı akışında tanımlanan istenen öznitelikleri de toplar.

    Geçerli bir e-posta adresi kullanın ve doğrulama kodunu kullanarak doğrulamayı gerçekleştirin. Parola ayarlayın. İstenen öznitelikler için değerleri girin.

    ![Oturum açma/kaydolma iş akışının bir parçası olarak gösterilen kaydolma sayfası](./media/tutorial-desktop-app/azure-ad-b2c-sign-up-workflow.png)

4. Azure AD B2C kiracısında yerel bir hesap oluşturmak için **Oluştur'u** seçin.

Kullanıcı artık oturum açma ve masaüstü uygulamasını kullanmak için e-posta adresini kullanabilir. Başarılı bir kaydolanveya kaydolduktan sonra, belirteç ayrıntıları WPF uygulamasının alt bölmesinde görüntülenir.

![WPF masaüstü uygulamasının alt bölmesinde gösterilen belirteç ayrıntıları](./media/tutorial-desktop-app/desktop-app-01-post-signin.png)

**Arama API** düğmesini seçerseniz, bir **hata iletisi** görüntülenir. Hatayla karşılaşırsınız, çünkü geçerli durumunda, uygulama demo kiracı tarafından korunan bir API'ye erişmeye çalışıyor, `fabrikamb2c.onmicrosoft.com`. Erişim jetonunuzun belirteci yalnızca Azure AD B2C kiracınız için geçerli olduğundan, API çağrısı yetkisizdir.

Korumalı bir web API'sini kendi kiracınıza kaydetmek ve **Arama API** işlevini etkinleştirmek için bir sonraki öğreticiye devam edin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Yerel istemci uygulamasını ekleme
> * Uygulamayı kullanacak şekilde örneği yapılandırma
> * Kullanıcı akışını kullanarak kaydolun

Ardından, Arama **API** düğmesini etkinleştirmek için, WPF masaüstü uygulamasına kendi Azure AD B2C kiracınızda kayıtlı bir web API'sine erişim izni verebilirsiniz:

> [!div class="nextstepaction"]
> [Öğretici: Bir masaüstü uygulamasından Bir Düğüm.js web API'sine erişim >](tutorial-desktop-app-webapi.md)
