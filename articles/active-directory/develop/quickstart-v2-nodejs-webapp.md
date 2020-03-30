---
title: Bir Düğüm.js Web uygulamasına OIDC oturum açma - Microsoft kimlik platformu | Azure
description: OpenID Connect'i kullanarak Bir Düğüm.js web uygulamasında kimlik doğrulamayı nasıl uygulayacağınızı öğrenin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 10/28/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:ASP.NET
ms.openlocfilehash: 4aa0cce83f9adc8c648656899ec6dc12d498e26b
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77160457"
---
# <a name="quickstart-add-sign-in-using-openid-connect-to-a-nodejs-web-app"></a>Hızlı başlangıç: OpenID Connect kullanarak Bir Düğüm.js Web Uygulamasına oturum açma

Bu hızlı başlangıçta, OpenID Connect kimlik doğrulamasını Express ile Node.js kullanılarak oluşturulmuş bir web uygulamasında nasıl ayarlayacağımı öğreneceksiniz. Örnek herhangi bir platformda çalışacak şekilde tasarlanmıştır.

## <a name="prerequisites"></a>Ön koşullar

Bu örneği çalıştırmak için şunları yapmanız gerekir:

* Node.js'den yükleyinhttp://nodejs.org/

* Bir [Microsoft hesabı](https://www.outlook.com) veya [Office 365 Geliştirici Programı](/office/developer-program/office-365-developer-program)

## <a name="register-your-application"></a>Uygulamanızı kaydetme 
1. Azure [portalında](https://portal.azure.com/) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Hesabınız birden fazla Azure AD kiracısında mevcutsa:
    - Sayfanın sağ üst köşesindeki menüden profilinizi seçin ve **ardından dizini değiştirin.**
    - Oturumunuzu, uygulamanızı oluşturmak istediğiniz Azure AD kiracısına değiştirin.

1. Uygulamanızı kaydetmek için [Azure Active Directory > App kayıtlarına](https://go.microsoft.com/fwlink/?linkid=2083908) gidin.

1. **Yeni kayıt'ı seçin.**

1. Bir uygulama sayfası **kaydedin,** uygulamanızın kayıt bilgilerini girin:
    - **Ad** bölümüne, uygulamanın kullanıcılarına görüntülenecek anlamlı bir ad girin. Örneğin: MyWebApp
    - Desteklenen **hesap türleri** bölümünde, **tüm kuruluş dizini ve kişisel Microsoft hesaplarındaki (örneğin Skype, Xbox, Outlook.com) Hesapları**seçin.

    Birden fazla yönlendirme URL'si varsa, uygulama başarıyla oluşturulduktan sonra bunları **kimlik doğrulama** sekmesinden eklemeniz gerekir.

1. Uygulamayı oluşturmak için **Kaydol'u** seçin.

1. Uygulamanın Genel **Bakış** **sayfasında, Uygulama (istemci) kimlik** değerini bulun ve daha sonraya kaydedin. Uygulamayı daha sonra bu projede yapılandırmak için bu değere ihtiyacınız olur.

1. Uygulama sayfa listesinde **Kimlik doğrulaması**'nı seçin.
    - Yönlendirme **URI'leri** bölümünde, açılan kutudaki **Web'i** seçin ve aşağıdaki yeniden yönlendirme URI'yi girin:`http://localhost:3000/auth/openid/return`
    - Gelişmiş **ayarlar** **bölümünde, Giriş URL'sini** ' ye `http://localhost:3000`ayarla.
    - Örtülü hibe bölümünde **gelişmiş ayarlar >,** bu örnek kullanıcıoturum açabilmesi için [Örtülü hibe akışının](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-implicit-grant-flow) etkinleştirilmesi için **kimlik belirteçlerini** kontrol edin.

1. **Kaydet'i**seçin.

1. **Sertifikalar & sırlar** sayfasından, **İstemci sırları** bölümünde, **Yeni istemci sırrını**seçin.
    - Önemli bir açıklama girin (örneğin uygulama sırrı).
    - **1 yıl içinde, 2 yıl içinde** veya **asla sona ermez**anahtar süresi seçin.
    - **Ekle** düğmesini tıklattığınızda, anahtar değeri görüntülenir. Anahtar değerini kopyalayın ve güvenli bir yere kaydedin.

    Uygulamayı yapılandırmak için daha sonra bu anahtara ihtiyacınız olacak. Bu anahtar değer yeniden görüntülenmez veya başka yollarla alınamaz, bu nedenle Azure portalından görünür görünmez kaydedin.

## <a name="download-the-sample-application-and-modules"></a>Örnek uygulamayı ve modülleri indirin

Ardından, örnek repo'yu kopyalayıp NPM modüllerini yükleyin.

Kabuğunuzdan veya komut satırınızdan:

`$ git clone git@github.com:AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git`

or

`$ git clone https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git`

Proje kök dizini, komutu çalıştırın:

`$ npm install`  

## <a name="configure-the-application"></a>Uygulamayı yapılandırma

Parametreleri `exports.creds` talimat verildiği gibi config.js olarak sağlayın.

* .onmicrosoft.com `exports.identityMetadata` biçiminin \*Azure AD kiracı adı ile güncelleştirin. `<tenant_name>`
* Uygulama `exports.clientID` kaydından belirtilen Uygulama Kimliği ile güncelleyin.
* Uygulama `exports.clientSecret` kaydından belirtilen Uygulama sırrı ile güncelleştirin.
* Uygulama `exports.redirectUrl` kaydından belirtilen Redirect URI ile güncelleştirin.

**Üretim uygulamaları için isteğe bağlı yapılandırma:**

* Farklı `exports.destroySessionUrl` `post_logout_redirect_uri`bir kullanmak istiyorsanız, config.js güncelleştirin.

* Eğer `exports.useMongoDBSessionStore` [mongoDB](https://www.mongodb.com) veya diğer uyumlu oturum [mağazaları](https://github.com/expressjs/session#compatible-session-stores)kullanmak istiyorsanız, doğru config.js ayarlayın.
Bu örnekteki varsayılan oturum `express-session`deposu. Varsayılan oturum deposu üretim için uygun değildir.

* Güncelleme `exports.databaseUri`, mongoDB oturum deposu ve farklı bir veritabanı URI kullanmak istiyorsanız.

* Güncelleştirin. `exports.mongoDBSessionMaxAge` Burada bir oturumu mongoDB'de ne kadar süreyle tutmak istediğinizi belirtebilirsiniz. Birim ikinci(ler) dir.

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

MongoDB hizmetini başlatın. Bu uygulamada mongoDB oturum mağazası kullanıyorsanız, [önce mongoDB'yi yüklemeniz](http://www.mongodb.org/) ve hizmeti başlatmanız gerekir. Varsayılan oturum deposunu kullanıyorsanız, bu adımı atlayabilirsiniz.

Komut satırınızdan aşağıdaki komutu kullanarak uygulamayı çalıştırın.

```
$ node app.js
```

**Sunucu çıktısını anlamak zor mu?:** Bu `bunyan` örnekte günlüğe kaydetmek için kullanırız. Konsol da bunyan yüklemek ve yukarıdaki gibi sunucu çalıştırmak ama bunyan ikili ile boru sürece size çok mantıklı olmaz:

```
$ npm install -g bunyan

$ node app.js | bunyan
```

### <a name="youre-done"></a>Senin bitirdin!

Bir sunucu başarıyla `http://localhost:3000`üzerinde çalışan olacaktır.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
Microsoft kimlik platformunun desteklediği web uygulaması senaryosu hakkında daha fazla bilgi edinin:
> [!div class="nextstepaction"]
> [Kullanıcı senaryosunda işaretleyen web uygulaması](scenario-web-app-sign-user-overview.md)
