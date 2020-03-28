---
title: "Öğretici: ASP.NET web API'sine erişim izni"
titleSuffix: Azure AD B2C
description: ASP.NET bir web API'sini korumak ve ASP.NET bir web uygulamasından aramak için Active Directory B2C'nin nasıl kullanılacağı hakkında öğretici.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.author: mimart
ms.date: 10/14/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 23531bd4c53dc2fc4851a1e4718fca0e9c3bfc1c
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78187432"
---
# <a name="tutorial-grant-access-to-an-aspnet-web-api-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C'yi kullanarak ASP.NET web API'sine erişim izni verme

Bu öğretici, azure active directory B2C'deki (Azure AD B2C) korumalı bir web API kaynağını ASP.NET bir web uygulamasından nasıl çağırabileceğinizi gösterir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Web API'si uygulaması ekleme
> * Web API'si için kapsamları yapılandırma
> * Web API'sine izin verme
> * Uygulamayı kullanacak şekilde örneği yapılandırma

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

[Tutorial: Azure Active Directory B2C'yi kullanarak bir web uygulamasında kimlik doğrulamasını etkinleştirme](tutorial-web-app-dotnet.md)adımlarını ve ön koşulları tamamlayın.

## <a name="add-a-web-api-application"></a>Web API'si uygulaması ekleme

Web API kaynaklarının, erişim jetonu sunan istemci uygulamalarının korumalı kaynak isteklerini kabul edip yanıtlamadan önce kiracınızda kaydedilmesi gerekir.

Azure AD B2C kiracınızda bir uygulama kaydettirmek için geçerli **Uygulamalar** deneyimini veya yeni birleştirilmiş **Uygulama kayıtları (Önizleme)** deneyimimizi kullanabilirsiniz. [Yeni deneyim hakkında daha fazla bilgi edinin.](https://aka.ms/b2cappregintro)

#### <a name="applications"></a>[Uygulamalar](#tab/applications/)

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresini seçerek ve kiracınızı içeren dizin seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
3. Azure portalının sol üst köşesindeki **tüm hizmetleri** seçin ve ardından Azure **AD B2C'yi**arayın ve seçin.
4. **Uygulamalar'ı**seçin ve sonra **Ekle'yi**seçin.
5. Uygulama için bir ad girin. Örneğin, *webapi1*.
6. **Web app/ web API ekle**için **Evet'i**seçin.
7. **YanıtURL'si için,** Azure AD B2C'nin uygulamanızın istediği belirteçleri döndürmesi gereken bir bitiş noktası girin. Bu öğreticide, örnek yerel olarak çalışır `https://localhost:44332`ve 'de dinler.
8. **App ID URI**için, web API'nız için kullanılan tanımlayıcıyı girin. Tam etki alanı ile birlikte URI tanımlayıcısı sizin için oluşturulur. Örneğin, `https://contosotenant.onmicrosoft.com/api`.
9. **Oluştur'u**tıklatın.
10. Özellikler sayfasında, web uygulamasını yapılandırırken kullanacağınız uygulama kimliğini kaydedin.

#### <a name="app-registrations-preview"></a>[Uygulama kayıtları (Önizleme)](#tab/app-reg-preview/)

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Üst menüdeki **Dizin + abonelik** filtresini seçin ve ardından Azure AD B2C kiracınızı içeren dizin'i seçin.
1. Sol menüde **Azure AD B2C'yi**seçin. Veya **Tüm hizmetleri** seçin ve **Azure AD B2C'yi**arayın ve seçin.
1. **Uygulama kayıtlarını (Önizleme)** seçin ve ardından **Yeni kayıt'ı**seçin.
1. Uygulama için bir **Ad** girin. Örneğin, *webapi1*.
1. **Uri'yi Yeniden Yönlendirme**altında **Web'i**seçin ve ardından Azure AD B2C'nin uygulamanızın istediği belirteçleri döndürmesi gereken bir bitiş noktası girin. Bu öğreticide, örnek yerel olarak çalışır `https://localhost:44332`ve 'de dinler.
1. **Kaydol**’u seçin.
1. Uygulama **(istemci) kimliğini** daha sonraki bir adımda kullanmak üzere kaydedin.

* * *

## <a name="configure-scopes"></a>Kapsamları yapılandırma

Kapsamlar korumalı kaynaklara erişimi yönetmenin bir yolunu sunar. Kapsamlar web API’si tarafından kapsam tabanlı erişim denetimi uygulamak için kullanılır. Örneğin web API'sinin kullanıcıları hem okuma hem de yazma veya yalnızca okuma erişimine sahip olabilir. Bu öğreticide web API’si için okuma ve yazma izinlerini tanımlamak için kapsamları kullanacaksınız.

[!INCLUDE [active-directory-b2c-scopes](../../includes/active-directory-b2c-scopes.md)]

## <a name="grant-permissions"></a>İzinleri verme

Bir uygulamadan korumalı web API'sini çağırmak için, UYGULAMA İzNİ'ni API'ye vermeniz gerekir. Ön koşul öğreticisinde, Azure AD B2C'de *webapp1*adında bir web uygulaması oluşturdunuz. Bu uygulamayı web API'sini aramak için kullanırsınız.

[!INCLUDE [active-directory-b2c-permissions-api](../../includes/active-directory-b2c-permissions-api.md)]

Uygulamanız korumalı web API'yi aramak için kayıtlıdır. Bir kullanıcı uygulamayı kullanmak için Azure AD B2C ile kimlik doğrulaması yaptı. Uygulama, korumalı web API'sine erişmek için Azure AD B2C'den yetkilendirme hibesi alır.

## <a name="configure-the-sample"></a>Örneği yapılandırma

Artık web API'si kayıtlı ve kapsamları tanımlanmış olduğundan, web API'sini Azure AD B2C kiracınızı kullanacak şekilde yapılandırırsınız. Bu öğreticide örnek bir web API’sini yapılandıracaksınız. Örnek web API önkoşul öğretici indirilen proje dahildir.

Örnek çözümde iki proje vardır:

* **TaskWebApp** - Görev listesi oluşturun ve güncelle. Örnek, kaydolmak veya kullanıcılara oturum açmak için **kaydolma veya oturum açma** kullanıcı akışını kullanır.
* **TaskService** - Görev listesi oluşturma, okuma, güncelleştirme ve silme işlevini destekler. API, Azure AD B2C tarafından korunur ve TaskWebApp tarafından çağrılır.

### <a name="configure-the-web-application"></a>Web uygulamasını yapılandırma

1. **B2C-WebAPI-DotNet** çözümünü Visual Studio’da açın.
1. **TaskWebApp** projesinde, **Web.config'i**açın.
1. API’yi yerel olarak çalıştırmak üzere, **api:TaskServiceUrl** için localhost ayarını kullanın. Web.config’i aşağıdaki gibi değiştirin:

    ```csharp
    <add key="api:TaskServiceUrl" value="https://localhost:44332/"/>
    ```

1. API’nin URI’sini yapılandırın. Bu, web uygulamasının API isteğini yapmak için kullandığı URI'dir. Ayrıca istenen izinleri yapılandırın.

    ```csharp
    <add key="api:ApiIdentifier" value="https://<Your tenant name>.onmicrosoft.com/api/" />
    <add key="api:ReadScope" value="demo.read" />
    <add key="api:WriteScope" value="demo.write" />
    ```

### <a name="configure-the-web-api"></a>Web API’sini yapılandırma

1. **TaskService** projesinde, **Web.config'i**açın.
1. API’yi kiracınızı kullanmak için yapılandırın.

    ```csharp
    <add key="ida:AadInstance" value="https://<Your tenant name>.b2clogin.com/{0}/{1}/v2.0/.well-known/openid-configuration" />
    <add key="ida:Tenant" value="<Your tenant name>.onmicrosoft.com" />
    ```

1. Kayıtlı web API uygulamanızın Uygulama Kimliği, *webapi1*istemci kimliği ayarlayın.

    ```csharp
    <add key="ida:ClientId" value="<application-ID>"/>
    ```

1. Kaydolun ve kaydolun kullanıcı akışınız adı ile kullanıcı akışı *B2C_1_signupsignin1*ayarını B2C_1_signupsignin1 güncelleyin.

    ```csharp
    <add key="ida:SignUpSignInPolicyId" value="B2C_1_signupsignin1" />
    ```

1. Kapsam ayarını portalda oluşturduğunuzlarla eşleşecek şekilde yapılandırın.

    ```csharp
    <add key="api:ReadScope" value="demo.read" />
    <add key="api:WriteScope" value="demo.write" />
    ```

## <a name="run-the-sample"></a>Örneği çalıştırma

**TaskWebApp** ve **TaskService** projelerinin ikisini birden çalıştırmanız gerekir.

1. Çözüm Gezgini’nde, çözüme sağ tıklayıp **Başlangıç Projelerini Ayarla...** seçeneğini belirleyin.
1. **Birden Çok başlangıç projesi**seçin.
1. İki proje için de **Eylem**’i **Başlat** olarak değiştirin.
1. Yapılandırmayı kaydetmek için **Tamam'ı** tıklatın.
1. İki uygulamayı da çalıştırmak için **F5**'e basın. Her uygulama kendi tarayıcı penceresinde açılır.
    * `https://localhost:44316/`web uygulamasıdır.
    * `https://localhost:44332/` web API’sidir.

1. Web uygulamasında, web uygulamasına **kaydolmak için kaydol / kaydolun'u** seçin. Daha önce oluşturduğunuz hesabı kullanın.
1. Oturum açmadan sonra **yapılacaklar listesini** seçin ve yapılacaklar listesi öğesi oluşturun.

Yapılacaklar listesi öğesi oluşturduğunuzda, web uygulaması yapılacaklar listesi öğesini oluşturmak için web API'sine bir istekte bulunr. Korumalı web uygulamanız, Azure AD B2C tarafından korunan web API'sini çağırıyor.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Web API'si uygulaması ekleme
> * Web API'si için kapsamları yapılandırma
> * Web API'sine izin verme
> * Uygulamayı kullanacak şekilde örneği yapılandırma

> [!div class="nextstepaction"]
> [Öğretici: Azure Active Directory B2C'de uygulamalarınız için kimlik sağlayıcılar ekleme](tutorial-add-identity-providers.md)
