---
title: Web API 'Lerini çağıran masaüstü uygulamalarını kaydetme | Mavisi
titleSuffix: Microsoft identity platform
description: Web API 'Leri (uygulama kaydı) çağıran bir masaüstü uygulaması oluşturmayı öğrenin
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 09/09/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: 8a1a2d7f5272def78cd162da1f6ac0265d4fb30b
ms.sourcegitcommit: 956dec4650e551bdede45d96507c95ecd7a01ec9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102517745"
---
# <a name="desktop-app-that-calls-web-apis-app-registration"></a>Web API 'Lerini çağıran masaüstü uygulaması: uygulama kaydı

Bu makale, bir masaüstü uygulaması için uygulama kaydı özelliklerini içerir.

## <a name="supported-account-types"></a>Desteklenen hesap türleri

Bir masaüstü uygulamasında desteklenen hesap türleri, açmak istediğiniz deneyime bağlı olarak değişir. Bu ilişki nedeniyle, desteklenen hesap türleri kullanmak istediğiniz akışlara bağlıdır.

### <a name="audience-for-interactive-token-acquisition"></a>Etkileşimli belirteç alımı için hedef kitle

Masaüstü uygulamanız etkileşimli kimlik doğrulaması kullanıyorsa, kullanıcıların herhangi bir [Hesap türünden](quickstart-register-app.md)oturum açmasını sağlayabilirsiniz.

### <a name="audience-for-desktop-app-silent-flows"></a>Masaüstü uygulaması için hedef kitle sessiz akışlar

- Tümleşik Windows kimlik doğrulaması veya Kullanıcı adı ve parola kullanmak için uygulamanızın kendi kiracınızda kullanıcılar oturum açması gerekir, örneğin, iş kolu (LOB) geliştiricisiyseniz. Ya da Azure Active Directory kuruluşlarda, uygulamanızın bir ISV senaryosu olması durumunda kendi kiracınızdaki kullanıcılar oturum açması gerekir. Bu kimlik doğrulama akışları, Microsoft kişisel hesapları için desteklenmez.
- Kullanıcılara, işletmeden ticari (B2C) yetkilisini ve ilkesini geçiren sosyal kimliklerle oturum açarsanız, yalnızca etkileşimli ve Kullanıcı adı-parola kimlik doğrulamasını kullanabilirsiniz.

## <a name="redirect-uris"></a>Yeniden yönlendirme URI 'Leri

Bir masaüstü uygulamasında kullanılacak yeniden yönlendirme URI 'Leri, kullanmak istediğiniz akışa göre değişir.

- Etkileşimli kimlik doğrulama veya cihaz kod akışı kullanıyorsanız, kullanın `https://login.microsoftonline.com/common/oauth2/nativeclient` . Bu yapılandırmayı başarmak için, uygulamanız için **kimlik doğrulama** bölümünde karşılık gelen URL 'yi seçin.

  > [!IMPORTANT]
  > `https://login.microsoftonline.com/common/oauth2/nativeclient`Yeniden yönlendirme URI 'si olarak kullanılması en iyi güvenlik uygulaması olarak önerilir.  Yeniden yönlendirme URI 'SI belirtilmemişse, MSAL.NET `urn:ietf:wg:oauth:2.0:oob` Varsayılan olarak kullanılması önerilmez.  Bu varsayılan, sonraki büyük sürümde bir son değişiklik olarak güncelleştirilir.

- MacOS için yerel bir amaç-C veya Swift uygulaması oluşturuyorsanız, yeniden yönlendirme URI 'sini uygulamanızın paket tanımlayıcısına göre aşağıdaki biçimde kaydedin: `msauth.<your.app.bundle.id>://auth` . `<your.app.bundle.id>`Uygulamanızın paket tanımlayıcısı ile değiştirin.
- Uygulamanız yalnızca tümleşik Windows kimlik doğrulaması veya Kullanıcı adı ve parola kullanıyorsa, uygulamanız için bir yeniden yönlendirme URI 'SI kaydetmeniz gerekmez. Bu akışlar Microsoft Identity platform v 2.0 uç noktasına gidiş dönüş yapılır. Uygulamanız belirli bir URI üzerinde geri çağrılmayacaktır.
- [Cihaz kod akışı](scenario-desktop-acquire-token.md#device-code-flow), [Tümleşik Windows kimlik doğrulaması](scenario-desktop-acquire-token.md#integrated-windows-authentication)ve bir Kullanıcı adı ile gizli bir istemci uygulamasından bir [parolayı ve bir kullanıcı adını ve bir parolayı](scenario-desktop-acquire-token.md#username-and-password) , bir yeniden yönlendirme URI 'si gerektiren [arka plan uygulamaları](scenario-daemon-overview.md)için kullanılan bir istemci kimlik bilgisi akışını kullanarak ayırt etmek için, bunu bir ortak istemci uygulaması olarak Bu yapılandırmayı elde etmek için:

    1. <a href="https://portal.azure.com/" target="_blank">Azure Portal</a>, **uygulama kayıtları**' de Uygulamanızı seçin ve **kimlik doğrulaması**' nı seçin.
    1. **Gelişmiş ayarlar**' da  >  **ortak istemci akışlarının**  >  **aşağıdaki mobil ve Masaüstü akışlarını etkinleştirmesine** izin ver:, **Evet**' i seçin.

        :::image type="content" source="media/scenarios/default-client-type.png" alt-text="Azure portal 'de kimlik doğrulama bölmesinde ortak istemci ayarını etkinleştir":::

## <a name="api-permissions"></a>API izinleri

Masaüstü uygulamaları, oturum açmış kullanıcı için API 'Ler çağırır. Temsilci izinleri istemeleri gerekir. Yalnızca [Daemon uygulamalarında](scenario-daemon-overview.md)işlenen uygulama izinleri isteyemeyebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu senaryonun [uygulama kodu yapılandırması](scenario-desktop-app-configuration.md)olan bir sonraki makaleye geçin.
