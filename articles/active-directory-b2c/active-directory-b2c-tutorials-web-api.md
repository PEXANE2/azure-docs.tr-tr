---
title: Öğretici-bir ASP.NET Web API 'sine erişim Izni verme-Azure Active Directory B2C | Microsoft Docs
description: Bir ASP.NET Web API 'sini korumak ve bir ASP.NET Web uygulamasından çağırmak için Active Directory B2C kullanma öğreticisi.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 339b118e48a01469312a40e6b0652a4ffb90291a
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68347138"
---
# <a name="tutorial-grant-access-to-an-aspnet-web-api-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C kullanarak bir ASP.NET Web API 'sine erişim izni verme

Bu öğreticide, bir ASP.NET Web uygulamasından Azure Active Directory (Azure AD) B2C 'de korumalı bir Web API kaynağını çağırma gösterilmektedir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Web API'si uygulaması ekleme
> * Bir Web API 'SI için Kapsamları yapılandırma
> * Web API 'sine izin verme
> * Uygulamayı kullanmak için örneği yapılandırma

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

[Öğreticideki adımları ve önkoşulları doldurun: Azure Active Directory B2C](active-directory-b2c-tutorials-web-app.md)kullanarak bir Web uygulamasında kimlik doğrulamasını etkinleştirin.

## <a name="add-a-web-api-application"></a>Web API'si uygulaması ekleme

Web API kaynakları, erişim belirteci sunan istemci uygulamalarına göre korunan kaynak isteklerini kabul etmeden ve bunlara yanıt verebilmeleri için kiracınızda kayıtlı olmalıdır.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
4. **Uygulamalar**' ı seçin ve ardından **Ekle**' yi seçin.
5. Uygulama için bir ad girin. Örneğin, *webapi1*.
6. **Web uygulaması/Web API 'Si Ekle** ve **örtük akışa Izin ver**için **Evet**' i seçin.
7. **Yanıt URL 'si**için Azure AD B2C uygulamanızın isteklerinizin belirteçleri döndürmesi gereken bir uç nokta girin. Bu öğreticide, örnek yerel olarak çalışır ve tarihinde `https://localhost:44332`dinler.
8. **Uygulama kimliği URI 'si**için, Web API 'niz için kullanılan tanımlayıcıyı girin. Tam etki alanı ile birlikte URI tanımlayıcısı sizin için oluşturulur. Örneğin: `https://contosotenant.onmicrosoft.com/api`.
9.           **Oluştur**'a tıklayın.
10. Özellikler sayfasında, Web uygulamasını yapılandırırken kullanacağınız uygulama KIMLIĞINI kaydedin.

## <a name="configure-scopes"></a>Kapsamları yapılandırma

Kapsamlar, korumalı kaynaklara erişimi yönetmek için bir yol sağlar. Kapsamlar web API’si tarafından kapsam tabanlı erişim denetimi uygulamak için kullanılır. Örneğin web API'sinin kullanıcıları hem okuma hem de yazma veya yalnızca okuma erişimine sahip olabilir. Bu öğreticide web API’si için okuma ve yazma izinlerini tanımlamak için kapsamları kullanacaksınız.

1. **Uygulamalar**' ı seçin ve ardından *webapi1*' ı seçin.
2. **Yayımlanan kapsamları**seçin.
3. **Kapsam**için girin `Hello.Read`ve açıklama için girin. `Read access to hello`
4. **Kapsam**için girin `Hello.Write`ve açıklama için girin. `Write access to hello`
5. **Kaydet**’e tıklayın.

Yayımlanan kapsamlar, Web API 'sine bir istemci uygulama izni vermek için kullanılabilir.

## <a name="grant-permissions"></a>İzinleri verme

Bir uygulamadan korumalı bir Web API 'SI çağırmak için uygulamanızın API 'sine izin vermeniz gerekir. Önkoşul öğreticisinde, Azure AD B2C adlı *WebApp1*adlı bir Web uygulaması oluşturdunuz. Web API 'sini çağırmak için bu uygulamayı kullanın.

1. **Uygulamalar**' ı seçin ve ardından *WebApp1*' ı seçin.
2. **API erişimi**' ni seçin ve ardından **Ekle**' yi seçin.
3. **API Seç** açılan menüsünde *webapi1*' yi seçin.
4. **Kapsamları Seç** açılan menüsünde, daha önce tanımladığınız **Merhaba. Read** ve **Hello. Write** kapsamlarını seçin.
5. **Tamam**'ı tıklatın.

Uygulamanız, korumalı Web API 'sini çağırmak için kaydedilir. Kullanıcı, uygulamayı kullanmak için Azure AD B2C kimliğini doğrular. Uygulama, korumalı Web API 'sine erişmek için Azure AD B2C bir yetkilendirme izni alır.

## <a name="configure-the-sample"></a>Örneği yapılandırma

Web API 'SI kayıtlı olduğuna ve kapsamlarınızın tanımlandığından, Web API 'sini Azure AD B2C kiracınızı kullanacak şekilde yapılandırırsınız. Bu öğreticide örnek bir web API’sini yapılandıracaksınız. Örnek Web API 'SI, önkoşul öğreticisinde indirdiğiniz projeye dahil edilmiştir.

Örnek çözümde iki proje vardır:

Aşağıdaki iki proje örnek çözümde bulunur:

- **Taskwebapp** -bir görev listesi oluşturun ve düzenleyin. Örnek, kullanıcıların kaydolması veya oturum açması için kaydolma **veya oturum açma** Kullanıcı akışını kullanır.
- **Taskservice** -oluşturma, okuma, güncelleştirme ve silme görev listesi işlevlerini destekler. API Azure AD B2C tarafından korunur ve TaskWebApp tarafından çağırılır.

### <a name="configure-the-web-application"></a>Web uygulamasını yapılandırma

1. **B2C-WebAPI-DotNet** çözümünü Visual Studio’da açın.
2. **TaskWebApp** projesinde **Web.config**’i açın.
3. API’yi yerel olarak çalıştırmak üzere, **api:TaskServiceUrl** için localhost ayarını kullanın. Web.config’i aşağıdaki gibi değiştirin: 

    ```csharp
    <add key="api:TaskServiceUrl" value="https://localhost:44332/"/>
    ```

3. API’nin URI’sini yapılandırın. Bu, Web uygulamasının API isteğini yapmak için kullandığı URI 'dir. Ayrıca istenen izinleri yapılandırın.

    ```csharp
    <add key="api:ApiIdentifier" value="https://<Your tenant name>.onmicrosoft.com/api/" />
    <add key="api:ReadScope" value="Hello.Read" />
    <add key="api:WriteScope" value="Hello.Write" />
    ```

### <a name="configure-the-web-api"></a>Web API’sini yapılandırma

1. **TaskService** projesinde **Web.config**’i açın.
2. API’yi kiracınızı kullanmak için yapılandırın.

    ```csharp
    <add key="ida:Tenant" value="<Your tenant name>.onmicrosoft.com" />
    ```

3. İstemci kimliğini API’niz için kayıtlı Uygulama Kimliğine ayarlayın.

    ```csharp
    <add key="ida:ClientId" value="<application-ID>"/>
    ```

4. Kullanıcı akışı ayarını, kaydolma ve oturum açma Kullanıcı akışının adı ile güncelleştirin.

    ```csharp
    <add key="ida:SignUpSignInUserFlowId" value="B2C_1_signupsignin1" />
    ```

5. Kapsamlar ayarını portalda oluşturduğunuz değerle eşleşecek şekilde yapılandırın.

    ```csharp
    <add key="api:ReadScope" value="Hello.Read" />
    <add key="api:WriteScope" value="Hello.Write" />
    ```

## <a name="run-the-sample"></a>Örneği çalıştırma

**TaskWebApp** ve **TaskService** projelerinin ikisini birden çalıştırmanız gerekir. 

1. Çözüm Gezgini’nde, çözüme sağ tıklayıp **Başlangıç Projelerini Ayarla...** seçeneğini belirleyin. 
2. **Birden çok başlangıç projesi**seçin.
3. İki proje için de **Eylem**’i **Başlat** olarak değiştirin.
4. Yapılandırmayı kaydetmek için **Tamam** ' ı tıklatın.
5. İki uygulamayı da çalıştırmak için **F5**'e basın. Her uygulama kendi tarayıcı sekmesinde açılır. `https://localhost:44316/` Web uygulamasıdır.
    `https://localhost:44332/` web API’sidir.

6. Web uygulamasında, Web uygulamasında oturum açmak için **kaydolma/oturum açma ' ya** tıklayın. Daha önce oluşturduğunuz hesabı kullanın. 
7. Oturum açtıktan sonra, yapılacaklar **listesi ' ne** tıklayın ve yapılacaklar listesi öğesi oluşturun.

Bir yapılacaklar listesi öğesi oluşturduğunuzda, Web uygulaması, yapılacaklar listesi öğesini oluşturmak için Web API 'sine bir istek yapar. Korumalı Web uygulamanız, Azure AD B2C kiracınızda korunan Web API 'sini çağırıyor.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Web API'si uygulaması ekleme
> * Bir Web API 'SI için Kapsamları yapılandırma
> * Web API 'sine izin verme
> * Uygulamayı kullanmak için örneği yapılandırma

> [!div class="nextstepaction"]
> [Öğretici: Azure Active Directory B2C ' deki uygulamalarınıza kimlik sağlayıcıları ekleyin](tutorial-add-identity-providers.md)
