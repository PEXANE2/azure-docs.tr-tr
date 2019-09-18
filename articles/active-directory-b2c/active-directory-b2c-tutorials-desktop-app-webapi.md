---
title: Öğretici-bir masaüstü uygulamasından Node. js web API 'sine erişim Izni verme-Azure Active Directory B2C | Microsoft Docs
description: Bir Node.js web API’sini korumak ve bir .NET masaüstü uygulamasından çağırmak için Active Directory B2C kullanmaya yönelik öğretici.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 75469d4522cea2914e0f69d5aa1850e468cb0d50
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71064847"
---
# <a name="tutorial-grant-access-to-a-nodejs-web-api-from-a-desktop-app-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C kullanarak bir masaüstü uygulamasından Node. js web API 'sine erişim izni verme

Bu öğreticide, bir Windows Presentation Foundation (WPF) masaüstü uygulamasından bir Azure Active Directory B2C (Azure AD B2C) korunan Node. js web API kaynağını çağırma gösterilmektedir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Web API'si uygulaması ekleme
> * Bir Web API 'SI için Kapsamları yapılandırma
> * Web API 'sine izin verme
> * Uygulamayı kullanmak için örneği güncelleştirme

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

[Öğreticideki adımları ve önkoşulları doldurun: Azure Active Directory B2C](active-directory-b2c-tutorials-desktop-app.md)kullanan hesaplarla Masaüstü uygulama kimlik doğrulamasını etkinleştirin.

## <a name="add-a-web-api-application"></a>Web API'si uygulaması ekleme

Web API kaynakları, erişim belirteci sunan istemci uygulamalarına göre korunan kaynak isteklerini kabul etmeden ve bunlara yanıt verebilmeleri için kiracınızda kayıtlı olmalıdır.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst menüdeki **Dizin + abonelik** filtresini seçip kiracınızı içeren dizini seçerek Azure AD B2C kiracınızı içeren dizini kullandığınızdan emin olun.
3. Azure portal sol üst köşesindeki **tüm hizmetler** ' i seçin ve ardından **Azure AD B2C**' i arayıp seçin.
4. **Uygulamalar**' ı seçin ve ardından **Ekle**' yi seçin.
5. Uygulama için bir ad girin. Örneğin, *webapi1*.
6. **Web uygulaması/Web API 'Si Ekle** ve **örtük akışa Izin ver**için **Evet**' i seçin.
7. **Yanıt URL 'si**için Azure AD B2C uygulamanızın isteklerinizin belirteçleri döndürmesi gereken bir uç nokta girin. Bu öğreticide, örnek yerel olarak çalışır ve tarihinde `https://localhost:5000`dinler.
8. **Uygulama kimliği URI 'si**için, Web API 'niz için kullanılan tanımlayıcıyı girin. Tam etki alanı ile birlikte URI tanımlayıcısı sizin için oluşturulur. Örneğin: `https://contosotenant.onmicrosoft.com/api`.
9. **Oluştur**'a tıklayın.
10. Özellikler sayfasında, Web uygulamasını yapılandırırken kullanacağınız uygulama KIMLIĞINI kaydedin.

## <a name="configure-scopes"></a>Kapsamları yapılandırma

Kapsamlar, korumalı kaynaklara erişimi yönetmek için bir yol sağlar. Kapsamlar web API’si tarafından kapsam tabanlı erişim denetimi uygulamak için kullanılır. Örneğin, bazı kullanıcılar hem okuma hem de yazma erişimine sahipken diğer kullanıcıların salt okunur izinleri olabilir. Bu öğreticide web API’si için okuma izinlerini tanımlayacaksınız.

1. **Uygulamalar**' ı seçin ve ardından *webapi1*' ı seçin.
2. **Yayımlanan kapsamları**seçin.
3. **Kapsam**için girin `Hello.Read`ve açıklama için girin. `Read access to hello`
4. **Kapsam**için girin `Hello.Write`ve açıklama için girin. `Write access to hello`
5. **Kaydet**’e tıklayın.

Yayımlanan kapsamlar web API’sine bir istemci uygulama izni vermek için kullanılabilir.

## <a name="grant-permissions"></a>İzinleri verme

Bir uygulamadan korumalı bir Web API 'SI çağırmak için uygulamanızın API 'sine izin vermeniz gerekir. Önkoşul öğreticisinde, Azure AD B2C adlı *APP1*adlı bir Web uygulaması oluşturdunuz. Web API 'sini çağırmak için bu uygulamayı kullanın.

1. **Uygulamalar**' ı seçin ve ardından *nativeapp1*' ı seçin.
2. **API erişimi**' ni seçin ve ardından **Ekle**' yi seçin.
3. **API Seç** açılan menüsünde *webapi1*' yi seçin.
4. **Kapsamları Seç** açılan menüsünde, daha önce tanımladığınız **Merhaba. Read** ve **Hello. Write** kapsamlarını seçin.
5. **Tamam**'ı tıklatın.

Bir kullanıcı WPF Masaüstü uygulamasını kullanmak için Azure AD B2C kimliğini doğrular. Masaüstü uygulaması, korunan Web API 'sine erişmek için Azure AD B2C bir yetkilendirme izni alır.

## <a name="configure-the-sample"></a>Örneği yapılandırma

Web API 'SI kayıtlı olduğuna ve kapsamlarınızın tanımlandığından, Web API kodunu Azure AD B2C kiracınızı kullanacak şekilde yapılandırırsınız. Bu öğreticide, GitHub 'dan indirebileceğiniz örnek bir Node. js web uygulaması yapılandırırsınız.

GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi/archive/master.zip) veya örnek web uygulamasını kopyalayın.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi.git
```
Node.js web API’si örneği API’ye yapılan çağrıları korumak için Azure AD B2C’yi etkinleştirmek üzere Passport.js kitaplığını kullanır.

1. `index.js` dosyasını açın.
2. Örneği Azure AD B2C kiracı kayıt bilgileriyle yapılandırın. Aşağıdaki kod satırlarını değiştirin:

    ```nodejs
    var tenantID = "<your-tenant-name>.onmicrosoft.com";
    var clientID = "<application-ID>";
    var policyName = "B2C_1_signupsignin1";
    ```

## <a name="run-the-sample"></a>Örneği çalıştırma

1. Bir Node.js komut istemi başlatın.
2. Node.js örneğini içeren dizine değiştirin. Örneğin `cd c:\active-directory-b2c-javascript-nodejs-webapi`
3. Aşağıdaki komutları çalıştırın:
    ```
    npm install && npm update
    ```
    ```
    node index.js
    ```

### <a name="run-the-desktop-application"></a>Masaüstü uygulamasını çalıştırma

1. Visual Studio 'da **Active-Directory-B2C-WPF** çözümünü açın.
2. Masaüstü uygulamasını çalıştırmak için **F5**'e basın.
3. [Bir masaüstü uygulamasında Azure Active Directory B2C ile kullanıcılar için kimlik doğrulaması gerçekleştirme öğreticisinde](active-directory-b2c-tutorials-desktop-app.md) kullanılan e-posta adresi ve parolayı kullanarak oturum açın.
4. **API’yı Çağır** düğmesine tıklayın.

Masaüstü uygulaması, Web API 'sine bir istek yapar ve oturum açan kullanıcının görünen adına bir yanıt alır. Korumalı masaüstü uygulaması, Azure AD B2C kiracınızdaki korumalı Web API 'sini çağırıyor.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Web API'si uygulaması ekleme
> * Bir Web API 'SI için Kapsamları yapılandırma
> * Web API 'sine izin verme
> * Uygulamayı kullanmak için örneği güncelleştirme

> [!div class="nextstepaction"]
> [Öğretici: Azure Active Directory B2C ' deki uygulamalarınıza kimlik sağlayıcıları ekleyin](tutorial-add-identity-providers.md)
