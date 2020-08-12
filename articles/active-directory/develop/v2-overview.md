---
title: Microsoft Identity platform genel bakış-Azure
description: Microsoft Identity platform bileşenleri ve uygulamalarınıza kimlik ve erişim yönetimi (ıAM) desteği oluşturmanıza nasıl yardımcı olabileceğini öğrenin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: overview
ms.workload: identity
ms.date: 07/09/2020
ms.author: ryanwi
ms.reviewer: agirling, saeeda, benv
ms.custom: identityplatformtop40
ms.openlocfilehash: e34eeeca3b17087fb5bd43ae353e43b5ab355831
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88119564"
---
# <a name="microsoft-identity-platform-overview"></a>Microsoft Identity platformuna genel bakış

Microsoft Identity platform, kullanıcılarınızın ve müşterilerinin Microsoft kimliklerini veya sosyal hesaplarını kullanarak oturum açmasını ve kendi API 'lerinize veya Microsoft Graph gibi Microsoft API 'Lerine yetkili erişim sağlamasına yardımcı olur.

Microsoft Identity platformunu oluşturan birkaç bileşen vardır:

- **OAuth 2,0 ve OpenID Connect standart uyumlu kimlik doğrulama hizmeti** geliştiricilerin aşağıdakiler de dahil olmak üzere çeşitli kimlik türlerinin kimliklerini doğrulamasını sağlar:
  - Azure AD aracılığıyla sağlanan iş veya okul hesapları
  - Skype, Xbox ve Outlook.com gibi kişisel Microsoft hesabı
  - Azure AD B2C kullanarak sosyal veya yerel hesaplar
- **Açık kaynak kitaplıkları**: Microsoft kimlik doğrulama KITAPLıKLARı (msal) ve standartlara uyumlu diğer kitaplıklar için destek
- **Uygulama Yönetimi Portalı**: Azure Portal, diğer Azure yönetim özellikleri ile birlikte bir kayıt ve yapılandırma deneyimi.
- **Uygulama yapılandırma API 'si ve PowerShell**: DevOps görevlerinizi otomatikleştirebilmeniz IÇIN Microsoft Graph API ve PowerShell aracılığıyla uygulamalarınızın programlı yapılandırması.
- **Geliştirici içeriği**: hızlı başlangıç, Öğreticiler, nasıl yapılır kılavuzları ve kod örnekleri gibi teknik belgeler.

Geliştiriciler için, Microsoft Identity platformu, kimlik ve güvenlik alanında, passwordless kimlik doğrulaması, adım kimlik doğrulama ve koşullu erişim gibi modern yeniliklerin tümleştirilmesini sağlar. Bu tür işlevselliği kendiniz uygulamanız gerekmez: Microsoft Identity platformu ile tümleştirilmiş uygulamalar, bu tür yeniliklerden yararlanır.

Microsoft Identity platformu ile kodu bir kez yazabilir ve herhangi bir kullanıcıya ulaşabilirsiniz. Bir uygulamayı bir kez oluşturup çok sayıda platformda çalışır hale getirebilirsiniz veya istemci olarak bir kaynak uygulaması (API) olarak işlev gören bir uygulama oluşturabilirsiniz.

## <a name="getting-started"></a>Başlarken

Derlemek istediğiniz [uygulama senaryosunu](authentication-flows-app-scenarios.md) seçin. Bu senaryo yollarının her biri, bir genel bakış ile başlar ve hızlı başlangıç bağlantısı ile çalışmaya başlamanıza yardımcı olur:

- [Tek sayfalı uygulama (SPA)](scenario-spa-overview.md)
- [Kullanıcılar için oturum açan web uygulaması](scenario-web-app-sign-user-overview.md)
- [Web API’lerini çağıran web uygulaması](scenario-web-app-call-api-overview.md)
- [Korumalı web API’si](scenario-protected-web-api-overview.md)
- [Web API'lerini çağıran web API'si](scenario-web-api-call-api-overview.md)
- [Masaüstü uygulaması](scenario-desktop-overview.md)
- [Daemon uygulaması](scenario-daemon-overview.md)
- [Mobil uygulama](scenario-mobile-overview.md)

Uygulamalarınızda kimlik doğrulaması ve yetkilendirmeyi tümleştirmede Microsoft Identity platformu ile çalışırken, en yaygın uygulama senaryolarını ve kimlik bileşenlerini özetleyen bu görüntüye başvurabilirsiniz. Tam boyut görüntülemek için görüntüyü seçin.

[![Microsoft Identity platformunda çeşitli uygulama senaryolarını gösteren metro haritası](./media/v2-overview/application-scenarios-identity-platform.png)](./media/v2-overview/application-scenarios-identity-platform.svg#lightbox)

## <a name="learn-authentication-concepts"></a>Kimlik doğrulama kavramlarını öğrenin

Çekirdek kimlik doğrulamasının ve Azure AD kavramlarının bu önerilen makale kümesinde Microsoft Identity platformu için nasıl uygulanacağını öğrenin:

- [Kimlik doğrulaması temel bilgileri](./authentication-vs-authorization.md)
- [Uygulama ve hizmet sorumluları](app-objects-and-service-principals.md)
- [Kitle](v2-supported-account-types.md)
- [İzinler ve onay](v2-permissions-and-consent.md)
- [Kimlik belirteçleri](id-tokens.md)
- [Erişim belirteçleri](access-tokens.md)
- [Kimlik doğrulama akışları ve uygulama senaryoları](authentication-flows-app-scenarios.md)

## <a name="more-identity-and-access-management-options"></a>Daha fazla kimlik ve erişim yönetimi seçeneği

[Azure AD B2C](../../active-directory-b2c/overview.md) -kullanıcılarınız Facebook veya Google gibi sosyal hesaplarını kullanarak veya bir e-posta adresi ve parola kullanarak oturum açabilirler.

[Azure AD B2B](../external-identities/what-is-b2b.md) -dış KULLANıCıLARı Azure AD kiracınıza "Konuk" kullanıcıları olarak davet edin ve kimlik doğrulama için mevcut kimlik bilgilerini kullanırken yetkilendirme izinlerini atayın.

> [!TIP]
> *Azure Active Directory geliştirici platformu (v 1.0)* belgeleri mi arıyorsunuz? Bkz. [geliştiriciler için Azure Active Directory (v 1.0) genel bakış](../azuread-dev/v1-overview.md).

## <a name="next-steps"></a>Sonraki adımlar

Zaten bir Azure Active Directory kiracısına erişiminiz olan bir Azure hesabınız varsa, ancak çoğu Microsoft Identity platform geliştiricilerinin uygulamalar geliştirirken "geliştirme kiracısı" için kendi Azure AD kiracısını kullanması gerekir.

Uygulamalarınızı oluştururken kullanmak üzere kendi kiracınızı oluşturmayı öğrenin:

[Hızlı başlangıç: Azure AD kiracısı ayarlama](quickstart-create-new-tenant.md)