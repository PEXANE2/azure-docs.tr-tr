---
title: Web API'lerini arayan mobil uygulamaları kaydedin | Azure
titleSuffix: Microsoft identity platform
description: Web API'lerini (uygulamanın kod yapılandırması) çağıran bir mobil uygulamayı nasıl oluşturacağa öğrenin
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 75cfd304869bfb63131dfd2afed9f925c86d32fb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77132432"
---
# <a name="register-mobile-apps-that-call-web-apis"></a>Web API'lerini arayan mobil uygulamaları kaydetme

Bu makalede, oluşturduğunuz bir mobil uygulamayı kaydetmenize yardımcı olacak yönergeler yer alıyor.

## <a name="supported-account-types"></a>Desteklenen hesap türleri

Mobil uygulamalarınızın desteklediği hesap türleri, etkinleştirmek istediğiniz deneyime ve kullanmak istediğiniz akışlara bağlıdır.

### <a name="audience-for-interactive-token-acquisition"></a>Etkileşimli belirteç edinimi için hedef kitle

Çoğu mobil uygulama etkileşimli kimlik doğrulaması kullanır. Uygulamanız bu kimlik doğrulama biçimini kullanıyorsa, herhangi bir [hesap türünden](quickstart-register-app.md#register-a-new-application-using-the-azure-portal)kullanıcılaroturum açabilirsiniz.

### <a name="audience-for-integrated-windows-authentication-username-password-and-b2c"></a>Tümleşik Windows kimlik doğrulaması, kullanıcı adı-parola ve B2C için hedef kitle

Evrensel Windows Platformu (UWP) uygulamanız varsa, kullanıcıları oturum etmek için Tümleşik Windows kimlik doğrulamasını kullanabilirsiniz. Tümleşik Windows kimlik doğrulamasını veya kullanıcı adı-parola kimlik doğrulamasını kullanmak için, uygulamanızın kullanıcıları kendi iş hattı (LOB) geliştirici kiracınızda oturum açmanız gerekir. Bağımsız bir yazılım satıcısı (ISV) senaryosunda, uygulamanız Azure Etkin Dizin kuruluşlarındaki kullanıcılarda oturum açabilir. Bu kimlik doğrulama akışları Microsoft kişisel hesapları için desteklenmez.

B2C yetkisini ve ilkesini geçen sosyal kimlikleri kullanarak da oturum açabilirsiniz. Bu yöntemi kullanmak için yalnızca etkileşimli kimlik doğrulama ve kullanıcı adı-parola kimlik doğrulaması kullanabilirsiniz. Kullanıcı adı-parola kimlik doğrulaması şu anda yalnızca Xamarin.iOS, Xamarin.Android ve UWP'de desteklenir.

Daha fazla bilgi için [Senaryolar ve desteklenen kimlik doğrulama akışları](authentication-flows-app-scenarios.md#scenarios-and-supported-authentication-flows) ve [Senaryolar ile desteklenen platformlar ve dillere](authentication-flows-app-scenarios.md#scenarios-and-supported-platforms-and-languages)bakın.

## <a name="platform-configuration-and-redirect-uris"></a>Platform yapılandırması ve yeniden yönlendirme URI'leri  

### <a name="interactive-authentication"></a>Etkileşimli kimlik doğrulaması

Etkileşimli kimlik doğrulaması kullanan bir mobil uygulama oluşturduğunuzda, en kritik kayıt adımı URI'yi yeniden yönlendirmedir. [ **Kimlik Doğrulama** bıçağındaki platform yapılandırması](https://aka.ms/MobileAppReg)aracılığıyla etkileşimli kimlik doğrulama ayarlayabilirsiniz.

Bu deneyim, uygulamanızın Microsoft Authenticator (ve Android'deki Intune Company Portal) aracılığıyla tek oturum açma (SSO) elde etmesini sağlar. Ayrıca cihaz yönetim ilkelerini de destekleyecektir.

Uygulama kayıt portalı, iOS ve Android uygulamaları için aracılı yanıt URI'yi hesaplamanıza yardımcı olacak bir önizleme deneyimi sağlar:

1. Uygulama kayıt portalında Kimlik **Doğrulama'yı** > seçin**yeni deneyimi deneyin.**

   ![Yeni bir deneyim seçtiğiniz Kimlik Doğrulama bıçağı](https://user-images.githubusercontent.com/13203188/60799285-2d031b00-a173-11e9-9d28-ac07a7ae894a.png)

2. **Platform ekle'yi**seçin.

   ![Platform ekleme](https://user-images.githubusercontent.com/13203188/60799366-4c01ad00-a173-11e9-934f-f02e26c9429e.png)

3. Platform listesi desteklendiğinde **iOS'u**seçin.

   ![Mobil uygulama seçin](https://user-images.githubusercontent.com/13203188/60799411-60de4080-a173-11e9-9dcc-d39a45826d42.png)

4. Paket kimliğinizi girin ve ardından **Kaydol'u**seçin.

   ![Paket kimliğinizi girin](https://user-images.githubusercontent.com/13203188/60799477-7eaba580-a173-11e9-9f8b-431f5b09344e.png)

Adımları tamamladığınızda, aşağıdaki resimde olduğu gibi yeniden yönlendirme URI sizin için hesaplanır.

![Ortaya çıkan yeniden yönlendirme URI](https://user-images.githubusercontent.com/13203188/60799538-9e42ce00-a173-11e9-860a-015a1840fd19.png)

Yeniden yönlendirme URI'yi el ile yapılandırmayı tercih ederseniz, bunu uygulama bildirimi aracılığıyla yapabilirsiniz. İşte manifesto için önerilen biçim:

- **iOS**:`msauth.<BUNDLE_ID>://auth` 
  - Örneğin, girin`msauth.com.yourcompany.appName://auth`
- **Android**:`msauth://<PACKAGE_NAME>/<SIGNATURE_HASH>`
  - Anahtar Aracı komutu aracılığıyla sürüm tuşunu kullanarak veya hata ayıklama anahtarını kullanarak Android imza karmasını oluşturabilirsiniz.

### <a name="username-password-authentication"></a>Kullanıcı adı-parola kimlik doğrulaması

Uygulamanız yalnızca kullanıcı adı-parola kimlik doğrulaması kullanıyorsa, uygulamanız için bir yeniden yönlendirme URI'si kaydetmeniz gerekmez. Bu akış, Microsoft kimlik platformu sürüm 2.0 bitiş noktasına gidiş-dönüş yapar. Başvurunuz belirli bir URI için geri çağrılmaz. 

Ancak, uygulamanızı ortak istemci uygulaması olarak tanımlamanız gerekir. Bunu yapmak için, uygulamanızın **Kimlik Doğrulama** bölümünden başlayın. Gelişmiş **ayarlar** alt bölümünde, **Varsayılan istemci türü** paragrafında, ortak istemci olarak uygulama **davran**sorusu için **Evet'i**seçin.

## <a name="api-permissions"></a>API izinleri

Mobil uygulamalar, oturum açmış kullanıcı adına API'leri arar. Uygulamanızın temsilci izni istemesi gerekir. Bu izinlere kapsam lar da denir. İstediğiniz deneyime bağlı olarak, Azure portalı üzerinden statik olarak temsilci izni isteyebilirsiniz. Veya çalışma zamanında dinamik olarak isteyebilirsiniz. 

İzinleri statik olarak kaydederek, yöneticilerin uygulamanızı kolayca onaylamasına izin verirsiniz. Statik kayıt önerilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Kod yapılandırması](scenario-mobile-app-configuration.md)
