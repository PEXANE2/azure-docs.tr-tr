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
ms.openlocfilehash: 699dd41024bd60b2016771d728253d938fc9ab23
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68347155"
---
# <a name="tutorial-enable-authentication-in-a-native-client-application-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C kullanarak yerel istemci uygulamasında kimlik doğrulamasını etkinleştirme

Bu öğreticide, bir Windows Presentation Foundation (WPF) masaüstü uygulamasında kullanıcının oturumunu açmak ve kullanıcıyı kaydetmek için Azure Active Directory (Azure AD) B2C’nin nasıl kullanılacağı gösterilir. Azure AD B2C, uygulamalarınızın, açık standart protokoller kullanarak sosyal hesaplar, kurumsal hesaplar ve Azure Active Directory hesaplar için kimlik doğrulaması yapmasına olanak sağlar.

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

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
4. **Uygulamalar**' ı seçin ve ardından **Ekle**' yi seçin.
5. Uygulama için bir ad girin. Örneğin, *nativeapp1*.
6. **İçerme Web uygulaması/Web API 'si**için **Hayır**' ı seçin.
7. **Yerel Istemciyi Ekle**için **Evet**' i seçin.
8. **Yeniden yönlendirme URI 'si**için, özel bir şemaya sahip geçerli bir yeniden yönlendirme URI 'si girin. Yeniden yönlendirme URI 'SI seçerken dikkat etmeniz gereken iki önemli nokta vardır:

    - **Unique** -YENIDEN yönlendirme URI 'sinin şeması her uygulama için benzersiz olmalıdır. Örnekte `com.onmicrosoft.contoso.appname://redirect/path` ,`com.onmicrosoft.contoso.appname` şemadır. Bu düzenin izlenmesi gerekir. İki uygulama aynı düzeni paylaşıyorsa, kullanıcıya bir uygulama seçme seçeneği verilir. Kullanıcı yanlış bir seçim yapıyorsa, oturum açma işlemi başarısız olur.
    - **Tam** -yeniden yönlendirme URI 'si bir şemaya ve bir yola sahip olmalıdır. Yol, etki alanından sonra en az bir eğik çizgi içermelidir. Örneğin, `//contoso/` `//contoso` çalışıp başarısız olur. Yeniden yönlendirme URI 'sinin alt çizgi gibi özel karakterler içermediğinden emin olun.

9.           **Oluştur**'a tıklayın.
10. Özellikler sayfasında, örneği yapılandırırken kullanacağınız uygulama KIMLIĞINI kaydedin.

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
