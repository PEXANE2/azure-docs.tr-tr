---
title: 'Hızlı başlangıç: MSAL node ile bir Node Web uygulamasına kimlik doğrulaması ekleme | Mavisi'
titleSuffix: Microsoft identity platform
description: Bu hızlı başlangıçta, Node.js için Node.js bir Web uygulamasıyla ve Microsoft kimlik doğrulama kitaplığı (MSAL) ile kimlik doğrulaması uygulamayı öğreneceksiniz.
services: active-directory
author: amikuma
manager: saeeda
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 10/22/2020
ms.author: amikuma
ms.custom: aaddev, scenarios:getting-started, languages:js, devx-track-js
ms.openlocfilehash: e223b5ae072a323ad56ed396c06580fea9b8b7ab
ms.sourcegitcommit: 2a8a53e5438596f99537f7279619258e9ecb357a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "94335256"
---
# <a name="quickstart-sign-in-users-and-get-an-access-token-in-a-node-web-app-using-the-auth-code-flow"></a>Hızlı başlangıç: kimlik doğrulama kod akışını kullanarak Kullanıcı oturumu açma ve düğüm Web uygulamasında erişim belirteci edinme

Bu hızlı başlangıçta, bir Node.js Web uygulamasının, yetkilendirme kodu akışını kullanarak kişisel hesap, iş hesabı ve okul hesaplarının kullanıcılarına nasıl oturum açıp çalıştırabileceklerini gösteren bir kod örneği çalıştırırsınız. Kod örneği Ayrıca, bir Web API 'SI çağırmak için bir erişim belirteci alma, bu durumda Microsoft Graph API 'SI de gösterir. Örneğin bir çizim için [nasıl çalıştığını](#how-the-sample-works) görün.

Bu hızlı başlangıçta yetkilendirme kodu akışıyla Node.js (MSAL node) için Microsoft kimlik doğrulama kitaplığı kullanılmaktadır.

> [!IMPORTANT]
> MSAL düğümü [!INCLUDE [PREVIEW BOILERPLATE](../../../includes/active-directory-develop-preview.md)]

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği- [ücretsiz bir Azure aboneliği oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* [Node.js](https://nodejs.org/en/download/)
* [Visual Studio Code](https://code.visualstudio.com/download) veya başka bir kod Düzenleyicisi

> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-application"></a>Hızlı başlangıç uygulamanızı kaydedin ve indirin
>
> #### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme
>
> 1. [Azure Portal](https://portal.azure.com)’ında oturum açın.
> 1. Hesabınız birden fazla kiracıya erişim veriyorsa, sağ üst kısımdaki hesabınızı seçin ve ardından Portal oturumunuzu kullanmak istediğiniz Azure AD kiracısı olarak ayarlayın.
> 1. [Uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908)’nı seçin.
> 1. **Yeni kayıt** seçeneğini belirleyin.
> 1. **Uygulamayı kaydet** sayfası görüntülendiğinde, uygulamanız için ad girin.
> 1. **Desteklenen hesap türleri** altında, **herhangi bir kurumsal dizin ve kişisel Microsoft hesabında hesaplar** ' ı seçin.
> 1. **Yeniden YÖNLENDIRME URI** değerini olarak ayarlayın `http://localhost:3000/redirect` .
> 1. **Kaydet** ’i seçin. 
> 1. Uygulamaya **genel bakış** sayfasında, daha sonra kullanılmak üzere **uygulama (istemci) kimliği** değerini aklınızda edin.
> 1. **Sertifikalar & parolalar** altında **yeni istemci parolası** ' nı seçin.  Açıklamayı boş bırakın ve varsayılan süre sonu ' nu ve ardından **Ekle** ' yi tıklatın.
> 1. Daha sonra kullanmak üzere **Istemci parolasının** **değerini** aklınızda yapın.

#### <a name="step-2-download-the-project"></a>2. Adım: Projeyi indirme

> [!div renderon="docs"]
> Node.js kullanarak projeyi bir Web sunucusuyla çalıştırmak için, [temel proje dosyalarını indirin](https://github.com/Azure-Samples/ms-identity-node/archive/main.zip).

> [!div renderon="portal" class="sxs-lookup"]
> Node.js kullanarak projeyi bir Web sunucusu ile çalıştırma

> [!div renderon="portal" class="sxs-lookup" id="autoupdate" class="nextstepaction"]
> [Kod örneğini indirin](https://github.com/Azure-Samples/ms-identity-node/archive/main.zip)

> [!div renderon="docs"]
> #### <a name="step-3-configure-your-node-app"></a>3. Adım: düğüm uygulamanızı yapılandırma
>
> Projeyi ayıklayın ve *MS-Identity-node-Main* klasörünü açın ve *index.js* dosyasını açın.
> `clientID` **Uygulamasını uygulama (ISTEMCI) kimliğiyle** ayarlayın.
> Öğesini `clientSecret` **Istemci parolasının** **değeri** ile ayarlayın.
>
>```javascript
>const config = {
>    auth: {
>        clientId: "Enter_the_Application_Id_Here",
>        authority: "https://login.microsoftonline.com/common",
>        clientSecret: "Enter_the_Client_Secret_Here"
>    },
>    system: {
>        loggerOptions: {
>            loggerCallback(loglevel, message, containsPii) {
>                console.log(message);
>            },
>            piiLoggingEnabled: false,
>            logLevel: msal.LogLevel.Verbose,
>        }
>    }
>};
> ```

> [!div renderon="docs"]
>
> `config`Bölümündeki değerleri burada açıklandığı gibi değiştirin:
>
> - `Enter_the_Application_Id_Here` , kaydettiğiniz uygulamanın **uygulama (istemci) kimliğidir** .
> - `Enter_the_Client_Secret_Here`, kaydettiğiniz uygulamanın **istemci gizli anahtarı** **değeridir** .
>
> Varsayılan `authority` değer, ana (genel) Azure bulutunu temsil eder:
>
> ```javascript
> authority: "https://login.microsoftonline.com/common",
> ```
>
> > [!TIP]
> > **Uygulama (istemci) kimliğinin** değerini bulmak için, Azure Portal uygulama kaydının **genel bakış** sayfasına gidin. Yeni bir **istemci parolası** almak veya oluşturmak için **Sertifikalar & gizlilikler** ' ın altına gidin.
>
> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>3. Adım: uygulamanız yapılandırıldı ve çalıştırılmaya hazırlanıyor
>
> [!div renderon="docs"]
>
> #### <a name="step-4-run-the-project"></a>4. Adım: projeyi çalıştırma

Node.js kullanarak projeyi çalıştırın:

1. Sunucuyu başlatmak için, proje dizini içinden aşağıdaki komutları çalıştırın:
    ```console
    npm install
    npm start
    ```
1. `http://localhost:3000/` adresine gidin.

1. Oturum açma işlemini başlatmak için **oturum aç '** ı seçin.

    İlk kez oturum açtığınızda, uygulamanın profilinize erişmesine izin vermek için onayınızı sağlamanız ve oturumunuzu açmanız istenir. Başarıyla oturum açtıktan sonra komut satırında bir günlük iletisi görürsünüz.

## <a name="more-information"></a>Daha fazla bilgi

### <a name="how-the-sample-works"></a>Örneğin nasıl çalıştığı

Örnek, çalıştırıldığında, localhost üzerinde bir Web sunucusu barındırır, bağlantı noktası 3000. Bir Web tarayıcısı bu siteye eriştiğinde, örnek kullanıcıyı hemen bir Microsoft kimlik doğrulama sayfasına yönlendirir. Bu nedenle, örnek herhangi bir *HTML* veya görüntü öğesi içermiyor. Kimlik doğrulama başarısı "Tamam" iletisini görüntüler.

### <a name="msal-node"></a>MSAL düğümü

MSAL düğüm kitaplığı, kullanıcıları imzalar ve Microsoft Identity platform tarafından korunan bir API 'ye erişmek için kullanılan belirteçleri ister. Node.js paket yöneticisini (NPM) kullanarak en son sürümü indirebilirsiniz:

```console
npm install @azure/msal-node
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Var olan bir Web uygulamasına kimlik doğrulama ekleme-GitHub kod örneği >](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/samples/msal-node-samples/standalone-samples/auth-code)
