---
title: "Öğretici: Bir masaüstü uygulamasından Bir Düğüm.js web API'sine erişim izni"
description: Bir Düğüm.js web API'sini korumak ve bir .NET masaüstü uygulamasından aramak için Active Directory B2C'nin nasıl kullanılacağı hakkında öğretici.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 10/12/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 59670cda68f54e4c0b20b361f0688e6766acba61
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78183405"
---
# <a name="tutorial-grant-access-to-a-nodejs-web-api-from-a-desktop-app-using-azure-active-directory-b2c"></a>Öğretici - Azure Active Directory B2C kullanarak bir masaüstü uygulamasından Node.js web API'sine erişim izni verme

Bu öğretici, Azure AD B2C tarafından da korunan bir Windows Presentation Foundation (WPF) masaüstü uygulamasından Azure Active Directory B2C (Azure AD B2C) tarafından korunan bir Node.js web API'sini nasıl çağırabileceğinizi gösterir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Web API'si uygulaması ekleme
> * Web API'si için kapsamları yapılandırma
> * Web API'sine izin verme
> * Uygulamayı kullanmak için örneği güncelleştirin

## <a name="prerequisites"></a>Ön koşullar

Tutorial'deki adımları ve ön koşulları [tamamlayın: Kullanıcıları yerel bir masaüstü istemcisinde doğrulayın.](tutorial-desktop-app.md)

## <a name="add-a-web-api-application"></a>Web API'si uygulaması ekleme

[!INCLUDE [active-directory-b2c-appreg-webapi](../../includes/active-directory-b2c-appreg-webapi.md)]

## <a name="configure-scopes"></a>Kapsamları yapılandırma

Kapsamlar korumalı kaynaklara erişimi yönetmenin bir yolunu sunar. Kapsamlar web API’si tarafından kapsam tabanlı erişim denetimi uygulamak için kullanılır. Örneğin, bazı kullanıcılar hem okuma hem de yazma erişimine sahipken diğer kullanıcıların salt okunur izinleri olabilir. Bu öğreticide web API’si için okuma ve yazma izinlerini tanımlayacaksınız.

[!INCLUDE [active-directory-b2c-scopes](../../includes/active-directory-b2c-scopes.md)]

Masaüstü uygulamasını yapılandırırken `demo.read` daha sonraki bir adımda kullanmak üzere **KAPSAMLAR** altındaki değeri kaydedin. Tam kapsam değeri `https://contosob2c.onmicrosoft.com/api/demo.read`.

## <a name="grant-permissions"></a>İzinleri verme

Yerel istemci uygulamasından korumalı bir web API'sını çağırmak için, kayıtlı yerel istemci uygulama izinlerini Azure AD B2C'de kaydettiğiniz web API'sine vermeniz gerekir.

Ön koşul öğreticisinde, *nativeapp1*adlı yerel bir istemci uygulamasını kaydettiniz. Aşağıdaki adımlar, önceki bölümde *webapi1* için maruz aldığınız API kapsamları ile bu yerel uygulama kaydı nı yapılandırır. Bu, masaüstü uygulamasının Azure AD B2C'den web API'sinin kaynaklarını doğrulamak ve kapsamlı erişimi sağlamak için kullanabileceği bir erişim jetonu elde etmesine olanak tanır. Hem masaüstü uygulamasını hem de web API kod örneklerini daha sonra öğreticide yapılandırıp çalıştırAbilirsiniz.

#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. **Uygulamalar'ı**seçin ve ardından *nativeapp1'i*seçin.
1. **API erişimini**seçin ve sonra **Ekle'yi**seçin.
1. SELECT **API** açılır sayfasında *webapi1'i*seçin.
1. **Scopes** açılır günlerini seç'te, daha önce tanımladığınız kapsamları seçin. Örneğin, *demo.read* ve *demo.write*.
1. **Tamam'ı**seçin.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. **Uygulama kayıtlarını (Önizleme)** seçin ve ardından API'ye erişimi olması gereken yerel istemci uygulamasını seçin. Örneğin, *nativeapp1*.
1. **Yönet**altında, **API izinlerini**seçin.
1. **Yapılandırılmış izinler**altında, **İzin ekle'yi**seçin.
1. **API'lerim** sekmesini seçin.
1. Yerel istemci uygulamasına erişim izni verilmesi gereken API'yi seçin. Örneğin, *webapi1*.
1. **İzin** **altında, demoyu**genişletin ve daha önce tanımladığınız kapsamları seçin. Örneğin, *demo.read* ve *demo.write*.
1. **İzin Ekle'yi**seçin. Yönlendirildikçe, bir sonraki adıma geçmeden önce birkaç dakika bekleyin.
1. **(kiracı adınız) için Grant yönetici onayı**seçin.
1. Şu anda oturum açmış yönetici hesabınızı seçin veya En azından *Bulut uygulama yöneticisi* rolüne atanmış Olan Azure AD B2C kiracınızda bir hesapla oturum açın.
1. **Kabul Et**’i seçin.
1. **Yenile'yi**seçin ve ardından "Verilenler..." her iki kapsam için **durum** altında görüntülenir. İzinlerin yayılması birkaç dakika sürebilir.

* * *

Kullanıcılar WPF masaüstü uygulamasını kullanmak için Azure AD B2C ile kimlik doğrulaması yapar. Masaüstü uygulaması, korumalı web API’sine erişmek için Azure AD B2C’den bir yetkilendirme izni alır.

## <a name="configure-the-samples"></a>Örnekleri yapılandırma

Artık web API'si kayıtlı olduğuna ve kapsamları ve izinleri yapılandırıldığına göre, Azure AD B2C kiracınızı kullanacak şekilde masaüstü uygulamasını ve web API örneklerini yapılandırırsınız.

### <a name="update-the-desktop-application"></a>Masaüstü uygulamasını güncelleştirme

Bu makalenin ön koşulunda, Azure AD B2C kiracınızda kullanıcı akışıyla oturum açmayı etkinleştirmek için bir [WPF masaüstü uygulamasını](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop) değiştirdiniz. Bu bölümde, daha önce kaydettiğiniz web API'sine başvurmak için aynı uygulamayı güncellersiniz, *webapi1.*

1. Visual Studio'da **active-directory-b2c-wpf** çözümlerini`active-directory-b2c-wpf.sln`açın.
1. **Active-directory-b2c-wpf** *projesinde, App.xaml.cs* dosyasını açın ve aşağıdaki değişken tanımlarını bulun.
    1. **Demo.read** kapsamını `ApiScopes` tanımlarken değişkenin değerini daha önce kaydettiğiniz değerle değiştirin.
    1. Web API'sını (örneğin, *webapi1)* kiracınıza kaydettiğinizde daha önce kaydettiğiniz **Yeniden Yönlendirme URI** ile değişkenin değerini değiştirin. `ApiEndpoint`

    Bir örneği aşağıda verilmiştir:

    ```csharp
    public static string[] ApiScopes = { "https://contosob2c.onmicrosoft.com/api/demo.read" };
    public static string ApiEndpoint = "http://localhost:5000";
    ```

### <a name="get-and-update-the-nodejs-api-sample"></a>Node.js API örneğini alın ve güncelleyin

Ardından, GitHub'dan Düğüm.js web API kodu örneğini alın ve Azure AD B2C kiracınızda kaydettiğiniz web API'sını kullanacak şekilde yapılandırın.

GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi/archive/master.zip) veya örnek web uygulamasını kopyalayın.

```console
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi.git
```

Node.js web API’si örneği API’ye yapılan çağrıları korumak için Azure AD B2C’yi etkinleştirmek üzere Passport.js kitaplığını kullanır.

1. `index.js` dosyasını açın.
1. Bu değişken tanımlarını aşağıdaki değerlerle güncelleştirin. Daha `<web-API-application-ID>` önce kaydettiğiniz web API'sının **Uygulama (istemci) kimliğinde** değişiklik *(webapi1).* Azure `<your-b2c-tenant>` AD B2C kiracınızın adını değiştirin.

    ```nodejs
    var clientID = "<web-API-application-ID>";
    var b2cDomainHost = "<your-b2c-tenant>.b2clogin.com";
    var tenantIdGuid = "<your-b2c-tenant>.onmicrosoft.com";
    var policyName = "B2C_1_signupsignin1";
    ```
1. API'yi yerel olarak çalıştırdığınıza göre, GET yönteminin güzergahındaki yolu demo uygulamasının konumu `/` yerine `/hello`güncelleştirin:

    ```nodejs
    app.get("/",
    ```

## <a name="run-the-samples"></a>Örnekleri çalıştırma

### <a name="run-the-nodejs-web-api"></a>Düğüm.js web API çalıştırın

1. Bir Node.js komut istemi başlatın.
2. Node.js örneğini içeren dizine değiştirin. Örneğin `cd c:\active-directory-b2c-javascript-nodejs-webapi`
3. Aşağıdaki komutları çalıştırın:
    ```console
    npm install && npm update
    ```
    ```console
    node index.js
    ```

### <a name="run-the-desktop-application"></a>Masaüstü uygulamasını çalıştırma

1. Visual Studio'da **active-directory-b2c-wpf** çözümlerini açın.
2. Masaüstü uygulamasını çalıştırmak için **F5**'e basın.
3. [Bir masaüstü uygulamasında Azure Active Directory B2C ile kullanıcılar için kimlik doğrulaması gerçekleştirme öğreticisinde](tutorial-desktop-app.md) kullanılan e-posta adresi ve parolayı kullanarak oturum açın.
4. Arama **API düğmesini** seçin.

Masaüstü uygulaması, yerel olarak çalışan web API'sine bir istekte bulunmaz ve geçerli bir erişim jetonunun doğrulanması üzerine oturum açmış kullanıcının ekran adını gösterir.

![WPF masaüstü uygulamasının üst bölmesinde gösterilen görüntü adı](./media/tutorial-desktop-app-webapi/desktop-app-01-post-api-call.png)

Azure AD B2C tarafından korunan masaüstü uygulamanız, Azure AD B2C tarafından da korunan yerel olarak çalışan web API'sini çağırıyor.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Web API'si uygulaması ekleme
> * Web API'si için kapsamları yapılandırma
> * Web API'sine izin verme
> * Uygulamayı kullanmak için örneği güncelleştirin

> [!div class="nextstepaction"]
> [Öğretici: Azure Active Directory B2C'de uygulamalarınız için kimlik sağlayıcılar ekleme](tutorial-add-identity-providers.md)
