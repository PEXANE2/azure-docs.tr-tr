---
title: Web API 'Lerini çağıran masaüstü uygulamalarını kaydetme-Microsoft Identity platform | Mavisi
description: Web API 'Leri (uygulama kaydı) çağıran bir masaüstü uygulaması oluşturmayı öğrenin
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
ms.collection: M365-identity-device-management
ms.openlocfilehash: 878f942bf36fef999b90274b81eaa7735afa73e5
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2020
ms.locfileid: "76293360"
---
# <a name="desktop-app-that-calls-web-apis-app-registration"></a>Web API 'Lerini çağıran masaüstü uygulaması: uygulama kaydı

Bu makale, bir masaüstü uygulaması için uygulama kaydı özelliklerini içerir.

## <a name="supported-account-types"></a>Desteklenen hesap türleri

Bir masaüstü uygulamasında desteklenen hesap türleri, açmak istediğiniz deneyime bağlı olarak değişir. Bu ilişki nedeniyle, desteklenen hesap türleri kullanmak istediğiniz akışlara bağlıdır.

### <a name="audience-for-interactive-token-acquisition"></a>Etkileşimli belirteç alımı için hedef kitle

Masaüstü uygulamanız etkileşimli kimlik doğrulaması kullanıyorsa, kullanıcıların herhangi bir [Hesap türünden](quickstart-register-app.md#register-a-new-application-using-the-azure-portal)oturum açmasını sağlayabilirsiniz.

### <a name="audience-for-desktop-app-silent-flows"></a>Masaüstü uygulaması için hedef kitle sessiz akışlar

- Tümleşik Windows kimlik doğrulaması veya Kullanıcı adı ve parola kullanmak için uygulamanızın kendi kiracınızda kullanıcılar oturum açması gerekir, örneğin, iş kolu (LOB) geliştiricisiyseniz. Ya da Azure Active Directory kuruluşlarda, uygulamanızın bir ISV senaryosu olması durumunda kendi kiracınızdaki kullanıcılar oturum açması gerekir. Bu kimlik doğrulama akışları, Microsoft kişisel hesapları için desteklenmez.
- Cihaz kodu akışını kullanmak istiyorsanız, kullanıcıları Microsoft kişisel hesaplarıyla henüz oturum açamazsınız.
- Kullanıcılara, işletmeden ticari (B2C) yetkilisini ve ilkesini geçiren sosyal kimliklerle oturum açarsanız, yalnızca etkileşimli ve Kullanıcı adı-parola kimlik doğrulamasını kullanabilirsiniz.

## <a name="redirect-uris"></a>Yeniden Yönlendirme URI'leri

Bir masaüstü uygulamasında kullanılacak yeniden yönlendirme URI 'Leri, kullanmak istediğiniz akışa göre değişir.

- Etkileşimli kimlik doğrulama veya cihaz kod akışı kullanıyorsanız, `https://login.microsoftonline.com/common/oauth2/nativeclient`kullanın. Bu yapılandırmayı başarmak için, uygulamanız için **kimlik doğrulama** bölümünde karşılık gelen URL 'yi seçin.
  
  > [!IMPORTANT]
  > Günümüzde MSAL.NET, Windows üzerinde çalışan masaüstü uygulamalarında varsayılan olarak başka bir yeniden yönlendirme URI 'SI kullanır (`urn:ietf:wg:oauth:2.0:oob`). Gelecekte bu Varsayılanı değiştirmek istiyoruz, bu nedenle `https://login.microsoftonline.com/common/oauth2/nativeclient`kullanmanızı öneririz.

- MacOS için yerel bir amaç-C veya Swift uygulaması oluşturursanız, yeniden yönlendirme URI 'sini uygulamanızın paket tanımlayıcısına göre aşağıdaki biçimde kaydedin: msauth. app. demeti. ID >://nauth. <. app. demeti. ID > uygulamanızın paket tanımlayıcısı ile < değiştirin.
- Uygulamanız yalnızca tümleşik Windows kimlik doğrulaması veya Kullanıcı adı ve parola kullanıyorsa, uygulamanız için bir yeniden yönlendirme URI 'SI kaydetmeniz gerekmez. Bu akışlar Microsoft Identity platform v 2.0 uç noktasına gidiş dönüş yapılır. Uygulamanız belirli bir URI üzerinde geri çağrılmayacaktır.
- Cihaz kod akışı, tümleşik Windows kimlik doğrulaması ve bir Kullanıcı adı ve bir parola ile yeniden yönlendirme URI 'Leri (daemon uygulamalarında kullanılan istemci kimlik bilgisi akışı) olmayan gizli bir istemci uygulama akışından bir parolayı ayırt etmek için, bunu ifade etmeniz gerekir uygulamanız ortak bir istemci uygulamasıdır. Bu yapılandırmayı başarmak için uygulamanızın **kimlik doğrulama** bölümüne gidin. **Gelişmiş ayarlar** alt bölümünde, **varsayılan istemci türü** paragrafında, **uygulamayı ortak istemci olarak değerlendir**için **Evet** ' i seçin.

  ![Ortak istemciye izin ver](media/scenarios/default-client-type.png)

## <a name="api-permissions"></a>API izinleri

Masaüstü uygulamaları, oturum açmış kullanıcı için API 'Ler çağırır. Temsilci izinleri istemeleri gerekir. Yalnızca [Daemon uygulamalarında](scenario-daemon-overview.md)işlenen uygulama izinleri isteyemeyebilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Masaüstü uygulaması: uygulama yapılandırması](scenario-desktop-app-configuration.md)
