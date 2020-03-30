---
title: Web API'lerini arayan masaüstü uygulamalarını kaydedin - Microsoft kimlik platformu | Azure
description: Web API'leri (uygulama kaydı) çağıran bir masaüstü uygulamasını nasıl oluşturursunuz öğrenin
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/09/2019
ms.author: jmprieur
ms.custom: aaddev
ms.openlocfilehash: c55fc9eb94a88dba1ab9fc915fe84bc2dd7d4d40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76702190"
---
# <a name="desktop-app-that-calls-web-apis-app-registration"></a>Web API'lerini çağıran masaüstü uygulaması: Uygulama kaydı

Bu makale, bir masaüstü uygulamasıiçin uygulama kaydı özelliklerini kapsar.

## <a name="supported-account-types"></a>Desteklenen hesap türleri

Masaüstü uygulamasında desteklenen hesap türleri, aydınlatmak istediğiniz deneyime bağlıdır. Bu ilişki nedeniyle, desteklenen hesap türleri kullanmak istediğiniz akışlara bağlıdır.

### <a name="audience-for-interactive-token-acquisition"></a>Etkileşimli belirteç edinimi için hedef kitle

Masaüstü uygulamanız etkileşimli kimlik doğrulama kullanıyorsa, kullanıcıları herhangi bir [hesap türünden](quickstart-register-app.md#register-a-new-application-using-the-azure-portal)oturum açabilirsiniz.

### <a name="audience-for-desktop-app-silent-flows"></a>Masaüstü uygulaması sessiz akışları için hedef kitle

- Tümleşik Windows Kimlik Doğrulaması veya kullanıcı adı ve parola kullanmak için, uygulamanızın kullanıcıları kendi kiracınızda oturum açması gerekir, örneğin, bir şirket şirketi (LOB) geliştiricisiyseniz. Veya Azure Etkin Dizin kuruluşlarında, uygulamanızın bir ISV senaryosuysa, kendi kiracınızdaki kullanıcılarda oturum açması gerekir. Bu kimlik doğrulama akışları Microsoft kişisel hesapları için desteklenmez.
- Aygıt kodu akışını kullanmak istiyorsanız, kullanıcıları henüz Microsoft kişisel hesaplarıyla oturum açamazsınız.
- İşletmelerarası (B2C) yetki ve ilkesini geçen sosyal kimliklere sahip kullanıcıları oturum alarğınızda, yalnızca etkileşimli ve kullanıcı adı-parola kimlik doğrulamasını kullanabilirsiniz.

## <a name="redirect-uris"></a>Uri'leri Yönlendirme

Bir masaüstü uygulamasında kullanılacak yeniden yönlendirme URI'leri, kullanmak istediğiniz akışa bağlıdır.

- Etkileşimli kimlik doğrulama veya aygıt kodu `https://login.microsoftonline.com/common/oauth2/nativeclient`akışı kullanıyorsanız, 'yi kullanın. Bu yapılandırmayı gerçekleştirmek için, uygulamanızın **Kimlik Doğrulama** bölümündeki ilgili URL'yi seçin.
  
  > [!IMPORTANT]
  > Bugün, MSAL.NET Windows'da çalışan masaüstü uygulamalarında varsayılan olarak`urn:ietf:wg:oauth:2.0:oob`başka bir yeniden yönlendirme URI kullanır ( ). Gelecekte, bu varsayılanı değiştirmek isteriz, bu nedenle `https://login.microsoftonline.com/common/oauth2/nativeclient`.

- macOS için yerel bir Objective-C veya Swift uygulaması oluşturursanız, uygulamanızın paket tanımlayıcısını temel alarak yeniden yönlendirme URI'yi aşağıdaki biçimde kaydedin: msauth.<your.app.bundle.id>://auth. <your.app.bundle.id> uygulamanızın paket tanımlayıcısıyla değiştirin.
- Uygulamanız yalnızca Entegre Windows Kimlik Doğrulaması veya kullanıcı adı ve parola kullanıyorsa, uygulamanız için yeniden yönlendirme URI'si kaydetmeniz gerekmez. Bu akışlar, Microsoft kimlik platformu v2.0 bitiş noktasına gidiş-dönüş yapar. Başvurunuz belirli bir URI için geri çağrılmaz.
- Cihaz kodu akışını, Entegre Windows Kimlik Doğrulaması'nı ve kullanıcı adı ve parolayı yeniden yönlendirme URI'leri olmayan gizli bir istemci uygulama akışından ayırt etmek için (daemon uygulamalarında kullanılan istemci kimlik bilgisi akışı), bunu ifade etmeniz gerekir başvurunuz bir kamu istemcisi uygulamasıdır. Bu yapılandırmayı gerçekleştirmek için, uygulamanız için **Kimlik Doğrulama** bölümüne gidin. Gelişmiş **ayarlar** alt bölümünde, **Varsayılan istemci türü** paragrafında, ortak istemci olarak Uygulama **Için** **Evet'i** seçin.

  ![Ortak istemciye izin ver](media/scenarios/default-client-type.png)

## <a name="api-permissions"></a>API izinleri

Masaüstü uygulamaları oturum açmış kullanıcı için API'leri arar. Temsilci lik izni istemeleri gerekir. Yalnızca [daemon uygulamalarında](scenario-daemon-overview.md)işlenen uygulama izinlerini isteyemezler.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Masaüstü uygulaması: Uygulama yapılandırması](scenario-desktop-app-configuration.md)
