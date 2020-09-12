---
title: Web API 'Lerini çağıran masaüstü uygulamalarını kaydetme-Microsoft Identity platform | Mavisi
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
ms.openlocfilehash: 6796ac42a10d3b976b23f5af1418b1789011d61b
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89440958"
---
# <a name="desktop-app-that-calls-web-apis-app-registration"></a>Web API 'Lerini çağıran masaüstü uygulaması: uygulama kaydı

Bu makale, bir masaüstü uygulaması için uygulama kaydı özelliklerini içerir.

## <a name="supported-account-types"></a>Desteklenen hesap türleri

Bir masaüstü uygulamasında desteklenen hesap türleri, açmak istediğiniz deneyime bağlı olarak değişir. Bu ilişki nedeniyle, desteklenen hesap türleri kullanmak istediğiniz akışlara bağlıdır.

### <a name="audience-for-interactive-token-acquisition"></a>Etkileşimli belirteç alımı için hedef kitle

Masaüstü uygulamanız etkileşimli kimlik doğrulaması kullanıyorsa, kullanıcıların herhangi bir [Hesap türünden](quickstart-register-app.md)oturum açmasını sağlayabilirsiniz.

### <a name="audience-for-desktop-app-silent-flows"></a>Masaüstü uygulaması için hedef kitle sessiz akışlar

- Tümleşik Windows kimlik doğrulaması veya Kullanıcı adı ve parola kullanmak için uygulamanızın kendi kiracınızda kullanıcılar oturum açması gerekir, örneğin, iş kolu (LOB) geliştiricisiyseniz. Ya da Azure Active Directory kuruluşlarda, uygulamanızın bir ISV senaryosu olması durumunda kendi kiracınızdaki kullanıcılar oturum açması gerekir. Bu kimlik doğrulama akışları, Microsoft kişisel hesapları için desteklenmez.
- Cihaz kodu akışını kullanmak istiyorsanız, kullanıcıları Microsoft kişisel hesaplarıyla henüz oturum açamazsınız.
- Kullanıcılara, işletmeden ticari (B2C) yetkilisini ve ilkesini geçiren sosyal kimliklerle oturum açarsanız, yalnızca etkileşimli ve Kullanıcı adı-parola kimlik doğrulamasını kullanabilirsiniz.

## <a name="redirect-uris"></a>Yeniden yönlendirme URI 'Leri

Bir masaüstü uygulamasında kullanılacak yeniden yönlendirme URI 'Leri, kullanmak istediğiniz akışa göre değişir.

- Etkileşimli kimlik doğrulama veya cihaz kod akışı kullanıyorsanız, kullanın `https://login.microsoftonline.com/common/oauth2/nativeclient` . Bu yapılandırmayı başarmak için, uygulamanız için **kimlik doğrulama** bölümünde karşılık gelen URL 'yi seçin.

  > [!IMPORTANT]
  > Günümüzde MSAL.NET, Windows () üzerinde çalışan masaüstü uygulamalarında varsayılan olarak başka bir yeniden yönlendirme URI 'SI kullanır `urn:ietf:wg:oauth:2.0:oob` . Gelecekte bu Varsayılanı değiştirmek istiyoruz. bu nedenle kullanmanızı öneririz `https://login.microsoftonline.com/common/oauth2/nativeclient` .

- MacOS için yerel bir amaç-C veya Swift uygulaması oluşturursanız, yeniden yönlendirme URI 'sini uygulamanızın paket tanımlayıcısına göre aşağıdaki biçimde kaydedin: msauth. <your.app.bundle.id>://<your.app.bundle.id>, uygulamanızın paket tanımlayıcısı ile değiştirin.
- Uygulamanız yalnızca tümleşik Windows kimlik doğrulaması veya Kullanıcı adı ve parola kullanıyorsa, uygulamanız için bir yeniden yönlendirme URI 'SI kaydetmeniz gerekmez. Bu akışlar Microsoft Identity platform v 2.0 uç noktasına gidiş dönüş yapılır. Uygulamanız belirli bir URI üzerinde geri çağrılmayacaktır.
- Cihaz kod akışını, tümleşik Windows kimlik doğrulamasını ve bir Kullanıcı adı ve bir parola ile yeniden yönlendirme URI 'Leri olmayan gizli bir istemci uygulama akışından (daemon uygulamalarında kullanılan istemci kimlik bilgisi akışı) ayırt etmek için, uygulamanızın bir ortak istemci uygulaması olduğunu ifade etmeniz gerekir. Bu yapılandırmayı başarmak için uygulamanızın **kimlik doğrulama** bölümüne gidin. **Gelişmiş ayarlar** alt bölümünde, **varsayılan istemci türü** paragrafında, **uygulamayı ortak istemci olarak değerlendir**için **Evet** ' i seçin.

  ![Ortak istemciye izin ver](media/scenarios/default-client-type.png)

## <a name="api-permissions"></a>API izinleri

Masaüstü uygulamaları, oturum açmış kullanıcı için API 'Ler çağırır. Temsilci izinleri istemeleri gerekir. Yalnızca [Daemon uygulamalarında](scenario-daemon-overview.md)işlenen uygulama izinleri isteyemeyebilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Masaüstü uygulaması: uygulama yapılandırması](scenario-desktop-app-configuration.md)
