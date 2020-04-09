---
title: Microsoft kimlik platformuna (v2.0) genel bakış - Azure
description: Microsoft kimlik platformu (v2.0) bitiş noktası ve platformu hakkında bilgi edinin.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: overview
ms.workload: identity
ms.date: 05/08/2019
ms.author: ryanwi
ms.reviewer: agirling, saeeda, benv
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 2e5bbbd311d71f2925e86ae756b36de7194aa9fb
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80886254"
---
# <a name="microsoft-identity-platform-v20-overview"></a>Microsoft kimlik platformu (v2.0) genel bakış

Microsoft kimlik platformu, Azure Active Directory (Azure AD) geliştirici platformunun geliştirilmesiyle ortaya çıkmıştır. Geliştiricilerin tüm Microsoft kimliklerini oturum açan uygulamalar oluşturmasına ve Microsoft Graph veya geliştiricilerin oluşturduğu API'ler gibi Microsoft API'lerini aramak için belirteçler almalarına olanak tanır. Microsoft kimlik platformu aşağıdakilerden oluşur:

- Geliştiricilerin şu lar da dahil olmak üzere herhangi bir Microsoft kimliğinin kimliğini doğrulamasını sağlayan **OAuth 2.0 ve OpenID Connect standart uyumlu kimlik doğrulama hizmeti:**
  - İş veya okul hesapları (Azure AD aracılığıyla sağlanan)
  - Kişisel Microsoft hesapları (Skype, Xbox ve Outlook.com gibi)
  - Sosyal veya yerel hesaplar (Azure AD B2C üzerinden)
- **Açık kaynak kitaplıkları**: Microsoft Kimlik Doğrulama Kitaplıkları (MSAL) ve standartlara uygun diğer kitaplıklar için destek
- **Uygulama yönetimi portalı**: Azure portalında yerleşik bir kayıt ve yapılandırma deneyiminin yanı sıra diğer tüm Azure yönetim özelliklerinizle birlikte.
- **Uygulama yapılandırması API ve PowerShell**: Microsoft Graph API ve PowerShell aracılığıyla uygulamalarınızın programlı yapılandırmasına olanak sağlar, böylece DevOps görevlerinizi otomatikleştirebilirsiniz.
- **Geliştirici içeriği:** kavramsal ve referans dokümantasyon, hızlı başlatma örnekleri, kod örnekleri, öğreticiler ve nasıl yapılacağını niçin kılavuzları.

Geliştiriciler için Microsoft kimlik platformu, kimlik ve güvenlik alanındaki parolasız kimlik doğrulama, hızlandırma kimlik doğrulaması ve Koşullu Erişim gibi yeniliklere sorunsuz entegrasyon sunar.  Bu tür işlevleri kendiniz uygulamanız gerekmez: Microsoft kimlik platformuyla entegre edilmiş uygulamalar bu tür yeniliklerden doğal olarak yararlanır.

Microsoft kimlik platformu ile, bir kez kod yazabilir ve herhangi bir kullanıcıya ulaşabilirsiniz. Bir uygulamayı bir kez oluşturabilir ve birçok platformda çalışmasını sağlayabilir veya istemci nin yanı sıra kaynak uygulaması (API) olarak işlev gösteren bir uygulama oluşturabilirsiniz.

## <a name="getting-started"></a>Başlarken

Kimlikle çalışmanın zor olması şart değil. 

Temel bilgileri öğrenmek için microsoft [kimlik platformu videosunu](identity-videos.md) izleyin. 

Sizin için geçerli olan bir [senaryo](authentication-flows-app-scenarios.md) seçin;

- [Tek sayfalı bir uygulama oluşturma](scenario-spa-overview.md)
- [Kullanıcıların oturum açtığı web uygulaması oluşturma](scenario-web-app-sign-user-overview.md)
- [Web API'lerini çağıran web uygulaması oluşturma](scenario-web-app-call-api-overview.md)
- [Korumalı web API'si oluşturma](scenario-protected-web-api-overview.md)
- [Web API'lerini çağıran bir web API'sı oluşturma](scenario-web-api-call-api-overview.md)
- [Masaüstü uygulaması oluşturma](scenario-desktop-overview.md)
- [Daemon uygulaması oluşturun](scenario-daemon-overview.md)
- [Mobil uygulama oluşturma](scenario-mobile-overview.md)

Aşağıdaki grafikte yaygın kimlik doğrulama uygulaması senaryoları özetler – Microsoft kimlik platformunu uygulamanızla tümleştirirken başvuru olarak kullanın.

[![Microsoft kimlik platformunda uygulama senaryoları](./media/v2-overview/application-scenarios-identity-platform.png)](./media/v2-overview/application-scenarios-identity-platform.svg#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

Temel kimlik doğrulama kavramları hakkında daha fazla bilgi edinmek istiyorsanız, şu konularla başlamanızı öneririz:

- [Kimlik doğrulama akışları ve uygulama senaryoları](authentication-flows-app-scenarios.md)
- [Kimlik doğrulaması temel bilgileri](authentication-scenarios.md)
- [Başvuru ve hizmet esasları](app-objects-and-service-principals.md)
- [Kitle -lere](v2-supported-account-types.md)
- [İzinler ve onay](v2-permissions-and-consent.md)
- [Kimlik belirteçleri](id-tokens.md) ve [erişim belirteçleri](access-tokens.md)

[Microsoft Graph](https://docs.microsoft.com/graph/overview)çağıran veri açısından zengin bir uygulama oluşturun.

Uygulamanızı bir **üretim ortamına**başlatmaya hazır olduğunuzda, şu en iyi uygulamaları inceleyin:

- Uygulamanızda [günlüğe kaydetmeyi etkinleştirin.](msal-logging.md)
- Uygulamanızda telemetriyi etkinleştirin.
- Yakınlıkları etkinleştirin [ve HTTP istemcilerini özelleştirin.](msal-net-provide-httpclient.md)
- Microsoft kimlik platformu [tümleştirme denetim listesini](identity-platform-integration-checklist.md)izleyerek tümleştirmenizi test edin.

## <a name="learn-more"></a>Daha fazla bilgi edinin

Sosyal ve yerel kimliklerde işaretleyen müşteriye yönelik bir uygulama oluşturmayı planlıyorsanız, [Azure AD B2C genel görünümüne](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-add-identity-providers)bakın.
