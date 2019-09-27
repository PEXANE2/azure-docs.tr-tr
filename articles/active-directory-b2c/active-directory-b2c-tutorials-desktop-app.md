---
title: Öğretici-yerel istemci uygulamasında kimlik doğrulamasını etkinleştirme-Azure Active Directory B2C | Microsoft Docs
description: .NET masaüstü uygulaması için Kullanıcı oturum açma bilgilerini sağlamak üzere Azure Active Directory B2C kullanma hakkında öğretici.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 3740a032db6ca9fd0fb88ce348610684d9f895bc
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71326321"
---
# <a name="tutorial-enable-authentication-in-a-native-client-application-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C kullanarak yerel istemci uygulamasında kimlik doğrulamasını etkinleştirme

Bu öğreticide, bir Windows Presentation Foundation (WPF) masaüstü uygulamasında Kullanıcı oturumu açmak ve kullanıcılara kaydolmak için Azure Active Directory B2C (Azure AD B2C) nasıl kullanılacağı gösterilmektedir. Azure AD B2C, uygulamalarınızın, açık standart protokoller kullanarak sosyal hesaplar, kurumsal hesaplar ve Azure Active Directory hesaplar için kimlik doğrulaması yapmasına olanak sağlar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Yerel istemci uygulamasını ekleme
> * Uygulamayı kullanmak için örneği yapılandırma
> * Kullanıcı akışını kullanarak kaydolma

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

- Uygulamanızdaki kullanıcı deneyimlerini etkinleştirmek için [Kullanıcı akışları oluşturun](tutorial-create-user-flows.md) .
- **.Net masaüstü geliştirme** ve **ASP.net ve Web geliştirme** Iş yükleriyle [Visual Studio 2019](https://www.visualstudio.com/downloads/) ' ü yükler.

## <a name="add-the-native-client-application"></a>Yerel istemci uygulamasını ekleme

[!INCLUDE [active-directory-b2c-appreg-native](../../includes/active-directory-b2c-appreg-native.md)]

Daha sonraki bir adımda kullanmak üzere **uygulama kimliğini** kaydedin.

## <a name="configure-the-sample"></a>Örneği yapılandırma

Bu öğreticide, GitHub 'dan indirebileceğiniz bir örnek yapılandırırsınız. Örnek WPF Masaüstü uygulaması, kaydolma, oturum açma ve Azure AD B2C ' de korumalı bir Web API 'SI çağrıları gösterir. [Bir zip dosyası indirin](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop/archive/master.zip), [depoya göz atın](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop) veya GitHub’dan örneği kopyalayın.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop.git
```

Uygulama ayarlarını değiştirmek için, adını kiracı adınızla `<your-tenant-name>` değiştirin ve >, kaydettiğiniz uygulama`<application-ID`kimliğiyle değiştirin.

1. `active-directory-b2c-wpf` çözümünü Visual Studio’da açın.
2. `active-directory-b2c-wpf` projesinde, **App.xaml.cs** dosyasını açın ve aşağıdaki güncelleştirmeleri yapın:

    ```csharp
    private static string Tenant = "<your-tenant-name>.onmicrosoft.com";
    private static string ClientId = "<application-ID>";
    ```

3. **Policysignupsignın** değişkenini, oluşturduğunuz Kullanıcı akışının adıyla güncelleştirin.

    ```csharp
    public static string PolicySignUpSignIn = "B2C_1_signupsignin1";
    ```

## <a name="run-the-sample"></a>Örneği çalıştırma

Örneği derlemek ve çalıştırmak için **F5** tuşuna basın.

### <a name="sign-up-using-an-email-address"></a>E-posta adresi kullanarak kaydolma

1. Kullanıcı olarak kaydolmak için **oturum aç** ' a tıklayın. Bu, **B2C_1_signupsignin1** Kullanıcı akışını kullanır.
2. Azure AD B2C, kayıt bağlantısı içeren bir oturum açma sayfası sunar. Henüz bir hesabınız yoksa **Hemen kaydol** bağlantısına tıklayın.
3. Kaydolma iş akışında, kullanıcı kimliğini bir e-posta adresi kullanarak toplamak ve doğrulamak için bir sayfa sunar. Kaydolma iş akışı, kullanıcının parolasını ve Kullanıcı akışında tanımlanan istenen öznitelikleri de toplar.

    Geçerli bir e-posta adresi kullanın ve doğrulama kodunu kullanarak doğrulamayı gerçekleştirin. Parola ayarlayın. İstenen öznitelikler için değerleri girin.

    ![Oturum açma/kaydolma iş akışının bir parçası olarak gösterilen kaydolma sayfası](media/active-directory-b2c-tutorials-desktop-app/sign-up-workflow.PNG)

4. Azure AD B2C kiracısında yerel bir hesap oluşturmak için **Oluştur**’a tıklayın.

Artık kullanıcı, oturum açmak ve masaüstü uygulamasını kullanmak için e-posta adresini kullanabilir.

> [!NOTE]
> **API’yi çağırma** düğmesine tıklarsanız "Yetkisiz" hatasıyla karşılaşırsınız. Bu hatayı, tanıtım kiracısındaki bir kaynağa erişmeye çalıştığınız için alırsınız. Erişim belirteci yalnızca Azure AD kiracınız için geçerli olduğundan, API çağrısı yetkilendirilmez. Kiracınız için korumalı bir web API’si oluşturmak amacıyla sonraki öğreticiye geçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Yerel istemci uygulamasını ekleme
> * Uygulamayı kullanmak için örneği yapılandırma
> * Kullanıcı akışını kullanarak kaydolma

> [!div class="nextstepaction"]
> [Öğretici: Azure Active Directory B2C kullanarak bir masaüstü uygulamasından Node. js web API 'sine erişim izni verme](active-directory-b2c-tutorials-spa-webapi.md)
