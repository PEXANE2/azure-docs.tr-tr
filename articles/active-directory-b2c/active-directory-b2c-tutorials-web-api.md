---
title: Öğretici-Azure Active Directory B2C bir ASP.NET Web API 'sine erişim Izni verme
description: Bir ASP.NET Web API 'sini korumak ve bir ASP.NET Web uygulamasından çağırmak için Active Directory B2C kullanma öğreticisi.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 09/19/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 77cd720ffd2763b2ad3d73559a5363989f9e3e3a
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71679286"
---
# <a name="tutorial-grant-access-to-an-aspnet-web-api-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C kullanarak bir ASP.NET Web API 'sine erişim Izni verme

Bu öğreticide, bir ASP.NET Web uygulamasından Azure Active Directory B2C (Azure AD B2C) ' de korumalı bir Web API kaynağını çağırma gösterilmektedir.

Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:

> [!div class="checklist"]
> * Web API uygulaması ekleme
> * Bir Web API 'SI için Kapsamları yapılandırma
> * Web API 'sine izin verme
> * Uygulamayı kullanmak için örneği yapılandırma

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Prerequisites

Öğreticideki adımları ve önkoşulları doldurun [: Azure Active Directory B2C kullanarak bir Web uygulamasında kimlik doğrulamasını etkinleştirme](active-directory-b2c-tutorials-web-app.md).

## <a name="add-a-web-api-application"></a>Web API uygulaması ekleme

Web API kaynakları, erişim belirteci sunan istemci uygulamalarına göre korunan kaynak isteklerini kabul etmeden ve bunlara yanıt verebilmeleri için kiracınızda kayıtlı olmalıdır.

1. [Azure Portal](https://portal.azure.com)oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresini seçip kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
4. **Uygulamalar**' ı seçin ve ardından **Ekle**' yi seçin.
5. Uygulama için bir ad girin. Örneğin, *webapi1*.
6. **Web uygulaması/Web API 'Si Ekle** ve **örtük akışa Izin ver**için **Evet**' i seçin.
7. **Yanıt URL 'si**için Azure AD B2C uygulamanızın isteklerinizin belirteçleri döndürmesi gereken bir uç nokta girin. Bu öğreticide, örnek yerel olarak çalışır ve `https://localhost:44332` ' a dinler.
8. **Uygulama kimliği URI 'si**için, Web API 'niz için kullanılan tanımlayıcıyı girin. Etki alanı dahil tam tanımlayıcı URI sizin için oluşturulur. Örneğin, `https://contosotenant.onmicrosoft.com/api`.
9. **Oluştur**'u tıklatın.
10. Özellikler sayfasında, Web uygulamasını yapılandırırken kullanacağınız uygulama KIMLIĞINI kaydedin.

## <a name="configure-scopes"></a>Kapsamları yapılandırma

Kapsamlar, korumalı kaynaklara erişimi yönetmek için bir yol sağlar. Kapsamlar, Web API 'SI tarafından kapsam tabanlı erişim denetimi uygulamak için kullanılır. Örneğin, Web API 'sinin kullanıcıları hem okuma hem de yazma erişimine sahip olabilir veya Web API 'SI kullanıcıları yalnızca okuma erişimine sahip olabilir. Bu öğreticide, Web API 'SI için okuma ve yazma izinlerini tanımlamak üzere kapsamları kullanacaksınız.

[!INCLUDE [active-directory-b2c-scopes](../../includes/active-directory-b2c-scopes.md)]

## <a name="grant-permissions"></a>İzin verme

Bir uygulamadan korumalı bir Web API 'SI çağırmak için uygulamanızın API 'sine izin vermeniz gerekir. Önkoşul öğreticisinde, Azure AD B2C adlı *WebApp1*adlı bir Web uygulaması oluşturdunuz. Web API 'sini çağırmak için bu uygulamayı kullanın.

1. **Uygulamalar**' ı seçin ve ardından *WebApp1*' ı seçin.
1. **API erişimi**' ni seçin ve ardından **Ekle**' yi seçin.
1. **API Seç** açılan menüsünde *webapi1*' yi seçin.
1. **Kapsamları Seç** açılan menüsünde, daha önce tanımladığınız kapsamları seçin. Örneğin, *demo. Read* ve *demo. Write*.
1. **Tamam ' ı**seçin.

Uygulamanız, korumalı Web API 'sini çağırmak için kaydedilir. Kullanıcı, uygulamayı kullanmak için Azure AD B2C kimliğini doğrular. Uygulama, korumalı Web API 'sine erişmek için Azure AD B2C bir yetkilendirme izni alır.

## <a name="configure-the-sample"></a>Örneği yapılandırma

Web API 'SI kayıtlı olduğuna ve kapsamlarınızın tanımlandığından, Web API 'sini Azure AD B2C kiracınızı kullanacak şekilde yapılandırırsınız. Bu öğreticide, örnek bir Web API 'SI yapılandırırsınız. Örnek Web API 'SI, önkoşul öğreticisinde indirdiğiniz projeye dahil edilmiştir.

Örnek çözümde iki proje vardır:

Aşağıdaki iki proje örnek çözümde bulunur:

* **Taskwebapp** -bir görev listesi oluşturun ve düzenleyin. Örnek, kullanıcıların kaydolması veya oturum açması için kaydolma **veya oturum açma** Kullanıcı akışını kullanır.
* **Taskservice** -oluşturma, okuma, güncelleştirme ve silme görev listesi işlevlerini destekler. API Azure AD B2C tarafından korunur ve TaskWebApp tarafından çağırılır.

### <a name="configure-the-web-application"></a>Web uygulamasını yapılandırma

1. Visual Studio 'da **B2C-WebAPI-DotNet** çözümünü açın.
1. **Taskwebapp** projesinde **Web. config**dosyasını açın.
1. API 'yi yerel olarak çalıştırmak için **API: TaskServiceUrl**için localhost ayarını kullanın. Web. config dosyasını aşağıdaki gibi değiştirin:

    ```csharp
    <add key="api:TaskServiceUrl" value="https://localhost:44332/"/>
    ```

1. API 'nin URI 'sini yapılandırın. Bu, Web uygulamasının API isteğini yapmak için kullandığı URI 'dir. Ayrıca, istenen izinleri yapılandırın.

    ```csharp
    <add key="api:ApiIdentifier" value="https://<Your tenant name>.onmicrosoft.com/api/" />
    <add key="api:ReadScope" value="demo.read" />
    <add key="api:WriteScope" value="demo.write" />
    ```

### <a name="configure-the-web-api"></a>Web API 'sini yapılandırma

1. **Taskservice** projesinde **Web. config**dosyasını açın.
1. Kiracınızı kullanmak için API 'YI yapılandırın.

    ```csharp
    <add key="ida:AadInstance" value="https://<Your tenant name>.b2clogin.com/{0}/{1}/v2.0/.well-known/openid-configuration" />
    <add key="ida:Tenant" value="<Your tenant name>.onmicrosoft.com" />
    ```

1. İstemci KIMLIĞINI kayıtlı Web API uygulamanızın uygulama KIMLIĞI olan *webapi1*olarak ayarlayın.

    ```csharp
    <add key="ida:ClientId" value="<application-ID>"/>
    ```

1. Kullanıcı akışı ayarını, kaydolma ve oturum açma Kullanıcı akışlarınızın adıyla güncelleştirin, *B2C_1_signupsignin1*.

    ```csharp
    <add key="ida:SignUpSignInPolicyId" value="B2C_1_signupsignin1" />
    ```

1. Kapsamlar ayarını portalda oluşturduğunuz olanlarla eşleşecek şekilde yapılandırın.

    ```csharp
    <add key="api:ReadScope" value="demo.read" />
    <add key="api:WriteScope" value="demo.write" />
    ```

## <a name="run-the-sample"></a>Örneği çalıştırın

Hem **Taskwebapp** hem de **taskservice** projelerini çalıştırmanız gerekir.

1. Çözüm Gezgini, çözüme sağ tıklayıp **Başlangıç projelerini ayarla...** seçeneğini belirleyin.
1. **Birden çok başlangıç projesi**seçin.
1. Her iki projenin **eylemini** **başlatılacak**şekilde değiştirin.
1. Yapılandırmayı kaydetmek için **Tamam** 'ı tıklatın.
1. Her iki uygulamayı da çalıştırmak için **F5** tuşuna basın. Her uygulama kendi tarayıcı penceresinde açılır.
    * `https://localhost:44316/`, Web uygulamasıdır.
    * `https://localhost:44332/`, Web API 'sidir.

1. Web uygulamasında, Web uygulamasında oturum açmak için **kaydolma/oturum aç** ' ı seçin. Daha önce oluşturduğunuz hesabı kullanın.
1. Oturum açtıktan sonra, yapılacaklar **listesi** ' ni seçin ve yapılacaklar listesi öğesi oluşturun.

Bir yapılacaklar listesi öğesi oluşturduğunuzda, Web uygulaması, yapılacaklar listesi öğesini oluşturmak için Web API 'sine bir istek yapar. Korumalı Web uygulamanız Azure AD B2C tarafından korunan Web API 'sini çağırıyor.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, nasıl yapılacağını öğrendiniz:

> [!div class="checklist"]
> * Web API uygulaması ekleme
> * Bir Web API 'SI için Kapsamları yapılandırma
> * Web API 'sine izin verme
> * Uygulamayı kullanmak için örneği yapılandırma

> [!div class="nextstepaction"]
> [Öğretici: Azure Active Directory B2C ' de uygulamalarınıza kimlik sağlayıcıları ekleme](tutorial-add-identity-providers.md)
