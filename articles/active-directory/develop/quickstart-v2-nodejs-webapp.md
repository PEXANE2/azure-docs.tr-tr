---
title: Node.js Web uygulamasına OıDC oturumu ekleme-Microsoft Identity platform | Mavisi
description: OpenID Connect kullanarak Node.js Web uygulamasında kimlik doğrulamanın nasıl uygulanacağını öğrenin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 10/28/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:ASP.NET, devx-track-javascript
ms.openlocfilehash: 165f3dab2413afc09a67175bb10471c2bd53ea32
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87129177"
---
# <a name="quickstart-add-sign-in-using-openid-connect-to-a-nodejs-web-app"></a>Hızlı başlangıç: Node.js Web uygulamasına OpenID Connect ile oturum açma ekleme

Bu hızlı başlangıçta, Express ile Node.js kullanılarak oluşturulmuş bir Web uygulamasında OpenID Connect kimlik doğrulamasını ayarlamayı öğreneceksiniz. Örnek, herhangi bir platformda çalışmak üzere tasarlanmıştır.

## <a name="prerequisites"></a>Önkoşullar

Bu örneği çalıştırmak için şunlar gerekir:

* Node.js şuradan yüklensinhttp://nodejs.org/

* Bir [Microsoft hesabı](https://www.outlook.com) ya da [Microsoft 365 Geliştirici Programı](/office/developer-program/office-365-developer-program)

## <a name="register-your-application"></a>Uygulamanızı kaydetme
1. [Azure Portal](https://portal.azure.com/) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Hesabınız birden çok Azure AD kiracısında varsa:
    - Sayfanın sağ üst köşesindeki menüden profilinizi seçin ve ardından dizin ' i **değiştirin**.
    - Uygulamanızı oluşturmak istediğiniz Azure AD kiracısı için oturumunuzu değiştirin.

1. Uygulamanızı kaydetmek için [Azure Active Directory > uygulama kayıtları](https://go.microsoft.com/fwlink/?linkid=2083908) gidin.

1. **Yeni kayıt** seçeneğini belirleyin.

1. **Bir uygulamayı kaydet** sayfası göründüğünde, uygulamanızın kayıt bilgilerini girin:
    - **Ad** bölümünde, uygulamanın kullanıcılarına gösterilecek anlamlı bir ad girin. Örneğin: MyWebApp
    - **Desteklenen hesap türleri** bölümünde, **herhangi bir kurumsal dizin ve kişisel Microsoft hesabında (ör. Skype, Xbox, Outlook.com) hesaplar**' ı seçin.

    Birden fazla yeniden yönlendirme URI 'si varsa, bunları daha sonra uygulama başarıyla oluşturulduktan sonra **kimlik doğrulama** sekmesinden eklemeniz gerekir.

1. Uygulamayı oluşturmak için **Kaydet** ' i seçin.

1. Uygulamanın **genel bakış** sayfasında, **uygulama (istemci) kimlik** değerini bulun ve daha sonra için kaydedin. Uygulamayı bu projede daha sonra yapılandırmak için bu değere ihtiyacınız olacak.

1. Uygulama sayfa listesinde **Kimlik doğrulaması**'nı seçin.
    - **Yeniden yönlendirme URI 'leri** bölümünde, açılan kutudan **Web** ' i seçin ve aşağıdaki yeniden yönlendirme URI 'sini girin:`http://localhost:3000/auth/openid/return`
    - **Gelişmiş ayarlar** bölümünde, **oturum kapatma URL 'sini** olarak ayarlayın `http://localhost:3000` .
    - **Gelişmiş ayarlar > örtük verme** bölümünde, **kimlik belirteçlerini** denetleyin, bu örnek, kullanıcının oturum açması için [örtük verme akışının](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-implicit-grant-flow) etkinleştirilmesini gerektirir.

1. **Kaydet**'i seçin.

1. **Sertifikalar & gizlilikler** sayfasında, **istemci gizli** dizileri bölümünde **yeni istemci parolası**' nı seçin.
    - Bir anahtar açıklaması girin (örneğin, uygulama gizli anahtarı).
    - **1 yılda, 2 yıl içinde** bir anahtar süresi seçin veya **hiçbir zaman sona ermez**.
    - **Ekle** düğmesine tıkladığınızda, anahtar değeri görüntülenecektir. Anahtar değerini kopyalayın ve güvenli bir konuma kaydedin.

    Uygulamayı yapılandırmak için bu anahtara daha sonra ihtiyacınız olacak. Bu anahtar değeri bir daha görüntülenmez veya başka yollarla alınabilir, bu nedenle Azure portal görünür hale geldiğinde onu kaydedin.

## <a name="download-the-sample-application-and-modules"></a>Örnek uygulamayı ve modülleri indirin

Sonra, örnek depoyu kopyalayın ve NPM modüllerini yükler.

Kabuk veya komut satırınızdan:

`$ git clone git@github.com:AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git`

veya

`$ git clone https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-nodejs.git`

Proje kök dizininden komutunu çalıştırın:

`$ npm install`

## <a name="configure-the-application"></a>Uygulamayı yapılandırma

config.js içindeki parametreleri `exports.creds` belirtildiği gibi sağlayın.

* `<tenant_name>`' Da `exports.identityMetadata` Azure AD kiracı adı \* . onmicrosoft.com biçiminde güncelleştirin.
* Uygulama `exports.clientID` kaydından belirtilen uygulama kimliğiyle güncelleştirin.
* `exports.clientSecret`Uygulama kaydınızdan belirtilen uygulama gizli anahtarı ile güncelleştirin.
* `exports.redirectUrl`Uygulama kaydından belirtilen yeniden YÖNLENDIRME URI 'siyle güncelleştirin.

**Üretim uygulamaları için isteğe bağlı yapılandırma:**

* `exports.destroySessionUrl`Farklı bir kullanmak istiyorsanız config.js güncelleştirin `post_logout_redirect_uri` .

* `exports.useMongoDBSessionStore` [MongoDB](https://www.mongodb.com) veya diğer [uyumlu oturum depolarını](https://github.com/expressjs/session#compatible-session-stores)kullanmak istiyorsanız, config.js true olarak ayarlayın.
Bu örnekteki varsayılan oturum depolama alanı `express-session` . Varsayılan oturum deposu üretime uygun değil.

* `exports.databaseUri`MongoDB oturum deposunu ve farklı bir veritabanı URI 'sini kullanmak istiyorsanız güncelleştirin.

* Güncelleştirin `exports.mongoDBSessionMaxAge` . Burada, mongoDB 'de bir oturumu ne kadar süreyle saklamak istediğinizi belirtebilirsiniz. Birim saniye (sn).

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

MongoDB hizmetini başlatın. Bu uygulamada mongoDB oturum deposu kullanıyorsanız, [mongoDB 'yi yüklemeniz](http://www.mongodb.org/) ve önce hizmeti başlatmanız gerekir. Varsayılan oturum deposunu kullanıyorsanız, bu adımı atlayabilirsiniz.

Komut satırınızdan aşağıdaki komutu kullanarak uygulamayı çalıştırın.

```
$ node app.js
```

**Sunucu çıkışının anlaşılması zor mi?:** `bunyan`Bu örnekte günlüğe kaydetmek için kullanıyoruz. Ayrıca, Bunyan ' i yüklemediğiniz ve sunucuyu yukarıdaki gibi çalıştırarak, Bunyan ikilisini kullanarak, konsolu sizin için çok anlamlı hale getirir:

```
$ npm install -g bunyan

$ node app.js | bunyan
```

### <a name="youre-done"></a>İşiniz bitti!

Sunucusunda başarıyla çalışan bir sunucu olacak `http://localhost:3000` .

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
Microsoft Identity platformunun desteklediği Web uygulaması senaryosu hakkında daha fazla bilgi edinin:
> [!div class="nextstepaction"]
> [Kullanıcılar senaryosunda oturum açan Web uygulaması](scenario-web-app-sign-user-overview.md)
