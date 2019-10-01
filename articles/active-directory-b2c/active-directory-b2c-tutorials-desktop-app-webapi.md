---
title: Öğretici-bir masaüstü uygulamasından Node. js web API 'sine erişim Izni verme-Azure Active Directory B2C | Microsoft Docs
description: Bir Node. js web API 'sini korumak ve bir .NET masaüstü uygulamasından çağırmak için Active Directory B2C kullanma öğreticisi.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 8c4b2d818549da07faf9ecd28f61b4ed5315f745
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71679333"
---
# <a name="tutorial-grant-access-to-a-nodejs-web-api-from-a-desktop-app-using-azure-active-directory-b2c"></a>Öğretici: Azure Active Directory B2C kullanarak bir masaüstü uygulamasından Node. js web API 'sine erişim Izni verme

Bu öğreticide, bir Windows Presentation Foundation (WPF) masaüstü uygulamasından bir Azure Active Directory B2C (Azure AD B2C) korunan Node. js web API kaynağını çağırma gösterilmektedir.

Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:

> [!div class="checklist"]
> * Web API uygulaması ekleme
> * Bir Web API 'SI için Kapsamları yapılandırma
> * Web API 'sine izin verme
> * Uygulamayı kullanmak için örneği güncelleştirme

## <a name="prerequisites"></a>Prerequisites

Öğreticideki adımları ve önkoşulları doldurun [: Azure Active Directory B2C kullanarak masaüstü uygulama kimlik doğrulamasını hesaplar Ile etkinleştirin](active-directory-b2c-tutorials-desktop-app.md).

## <a name="add-a-web-api-application"></a>Web API uygulaması ekleme

[!INCLUDE [active-directory-b2c-appreg-webapi](../../includes/active-directory-b2c-appreg-webapi.md)]

## <a name="configure-scopes"></a>Kapsamları yapılandırma

Kapsamlar, korumalı kaynaklara erişimi yönetmek için bir yol sağlar. Kapsamlar, Web API 'SI tarafından kapsam tabanlı erişim denetimi uygulamak için kullanılır. Örneğin, bazı kullanıcılar hem okuma hem de yazma erişimine sahip olabilir, ancak diğer kullanıcılar salt okuma izinlerine sahip olabilir. Bu öğreticide Web API 'SI için okuma izinleri tanımlarsınız.

[!INCLUDE [active-directory-b2c-scopes](../../includes/active-directory-b2c-scopes.md)]

## <a name="grant-permissions"></a>İzin verme

Bir uygulamadan korumalı bir Web API 'SI çağırmak için uygulamanızın API 'sine izin vermeniz gerekir. Önkoşul öğreticisinde, Azure AD B2C adlı *APP1*adlı bir Web uygulaması oluşturdunuz. Web API 'sini çağırmak için bu uygulamayı kullanın.

1. **Uygulamalar**' ı seçin ve ardından *nativeapp1*' ı seçin.
1. **API erişimi**' ni seçin ve ardından **Ekle**' yi seçin.
1. **API Seç** açılan menüsünde *webapi1*' yi seçin.
1. **Kapsamları Seç** açılan menüsünde, daha önce tanımladığınız kapsamları seçin. Örneğin, *demo. Read* ve *demo. Write*.
1. **Tamam ' ı**seçin.

Bir kullanıcı WPF Masaüstü uygulamasını kullanmak için Azure AD B2C kimliğini doğrular. Masaüstü uygulaması, korunan Web API 'sine erişmek için Azure AD B2C bir yetkilendirme izni alır.

## <a name="configure-the-sample"></a>Örneği yapılandırma

Web API 'SI kayıtlı olduğuna ve kapsamlarınızın tanımlandığından, Web API kodunu Azure AD B2C kiracınızı kullanacak şekilde yapılandırırsınız. Bu öğreticide, GitHub 'dan indirebileceğiniz örnek bir Node. js web uygulaması yapılandırırsınız.

[Bir zip dosyası indirin](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi/archive/master.zip) veya örnek Web uygulamasını GitHub 'dan kopyalayın.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi.git
```

Node. js web API 'SI örneği, Azure AD B2C API 'nin aramalarını korumasını sağlamak için Passport. js kitaplığını kullanır.

1. @No__t-0 dosyasını açın.
2. Örneği, Azure AD B2C kiracı kayıt bilgileri ile yapılandırın. Aşağıdaki kod satırlarını değiştirin:

    ```nodejs
    var tenantID = "<your-tenant-name>.onmicrosoft.com";
    var clientID = "<application-ID>";
    var policyName = "B2C_1_signupsignin1";
    ```

## <a name="run-the-sample"></a>Örneği çalıştırın

1. Node. js komut istemi başlatın.
2. Node. js örneğini içeren dizine geçin. Örneğin `cd c:\active-directory-b2c-javascript-nodejs-webapi`
3. Aşağıdaki komutları çalıştırın:
    ```
    npm install && npm update
    ```
    ```
    node index.js
    ```

### <a name="run-the-desktop-application"></a>Masaüstü uygulamasını çalıştırma

1. Visual Studio 'da **Active-Directory-B2C-WPF** çözümünü açın.
2. Masaüstü uygulamasını çalıştırmak için **F5** 'e basın.
3. [Bir masaüstü uygulaması öğreticisinde Azure Active Directory B2C kullanıcıların kimliğini doğrulamak](active-directory-b2c-tutorials-desktop-app.md)için kullanılan e-posta adresini ve parolayı kullanarak oturum açın.
4. API 'yi **çağır** düğmesine tıklayın.

Masaüstü uygulaması, Web API 'sine bir istek yapar ve oturum açan kullanıcının görünen adına bir yanıt alır. Korumalı masaüstü uygulaması, Azure AD B2C kiracınızdaki korumalı Web API 'sini çağırıyor.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, nasıl yapılacağını öğrendiniz:

> [!div class="checklist"]
> * Web API uygulaması ekleme
> * Bir Web API 'SI için Kapsamları yapılandırma
> * Web API 'sine izin verme
> * Uygulamayı kullanmak için örneği güncelleştirme

> [!div class="nextstepaction"]
> [Öğretici: Azure Active Directory B2C ' de uygulamalarınıza kimlik sağlayıcıları ekleme](tutorial-add-identity-providers.md)
