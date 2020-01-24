---
title: Web API 'Leri çağıran bir Daemon uygulaması oluşturma-Microsoft Identity platform | Mavisi
description: Web API 'Lerini çağıran bir Daemon uygulaması derlemeyi öğrenin
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/15/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: af4b00a630c2fc7d1b806a98f537e8635807609e
ms.sourcegitcommit: af6847f555841e838f245ff92c38ae512261426a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2020
ms.locfileid: "76702258"
---
# <a name="scenario-daemon-application-that-calls-web-apis"></a>Senaryo: Web API 'Lerini çağıran Daemon uygulaması

Web API 'Lerini çağıran bir Daemon uygulaması oluşturmak için ihtiyacınız olan her şey hakkında bilgi edinin.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="overview"></a>Genel Bakış

Uygulamanız, bir Web API 'sini kendi adına (Kullanıcı adına değil) çağırmak için bir belirteç alabilir. Bu senaryo, Daemon uygulamaları için yararlıdır. Standart OAuth 2,0 [istemci kimlik bilgileri](v2-oauth2-client-creds-grant-flow.md) ver ' i kullanıyor.

![Daemon uygulamaları](./media/scenario-daemon-app/daemon-app.svg)

Daemon uygulamaları için kullanım örneklerinin bazı örnekleri aşağıda verilmiştir:

- Kullanıcıları sağlamak veya yönetmek ya da bir dizinde toplu işlem yapmak için kullanılan Web uygulamaları
- Toplu işleri gerçekleştiren masaüstü uygulamaları (Windows Hizmetleri veya Linux üzerinde Daemon 'ları işlemleri) veya arka planda çalışan bir işletim sistemi hizmeti
- Belirli kullanıcıları değil, dizinleri işlemek zorunda olan Web API 'Leri

Daemon olmayan uygulamaların istemci kimlik bilgilerini kullandıklarında bile, bir Web API 'sine veya bir kaynağa teknik nedenlerden dolayı kendi kimlik bilgilerine erişmesi gerekir. Bir örnek, anahtar kasasındaki gizli dizileri veya bir önbellek için Azure SQL veritabanı 'na erişim sağlar.

Kendi kimlikleri için belirteç elde eden uygulamalar:

- Gizli istemci uygulamaları. Kaynaklara bir kullanıcıdan bağımsız olarak eriştikleri belirtilen bu uygulamalar, kimliklerini kanıtlamaları gerekir. Ayrıca, Azure Active Directory (Azure AD) kiracı yöneticileri tarafından onaylanması gereken hassas uygulamalar da vardır.
- Azure AD ile bir gizli dizi (uygulama parolası veya sertifika) kaydettiniz. Bu gizli dizi, bir belirteç almak için Azure AD çağrısı sırasında geçirilir.

## <a name="specifics"></a>Özelliklerini

> [!IMPORTANT]
>
> - Bir Daemon uygulamasıyla Kullanıcı etkileşimi mümkün değildir. Bir Daemon uygulaması kendi kimliğini gerektirir. Bu tür bir uygulama, uygulama kimliğini kullanarak bir erişim belirteci ister ve uygulama KIMLIĞI, kimlik bilgileri (parola veya sertifika) ve uygulama KIMLIĞI URI 'sini Azure AD 'ye sunuyor. Başarılı kimlik doğrulamasından sonra, Daemon, Web API 'sini çağırmak için kullanılan (ve gerektiğinde yenilendiğinde) Microsoft Identity platform uç noktasından bir erişim belirteci (ve yenileme belirteci) alır.
> - Kullanıcı etkileşimi mümkün olmadığından, artımlı onay mümkün olmayacaktır. Tüm gerekli API izinlerinin uygulama kaydında yapılandırılması gerekir ve uygulamanın kodu yalnızca statik olarak tanımlanan izinleri ister. Bu Ayrıca, Daemon uygulamalarının artımlı onayı desteklememe anlamına gelir.

Geliştiriciler için, bu senaryoya yönelik uçtan uca deneyim aşağıdaki yönlere sahiptir:

- Daemon uygulamaları yalnızca Azure AD kiracılarında çalışabilir. Microsoft kişisel hesaplarını işlemeyi deneyen bir Daemon uygulaması oluşturmak mantıklı değildir. İş kolu (LOB) uygulaması geliştiricisiyseniz, kiracınızda Daemon uygulamanızı oluşturacaksınız. Bir ISV iseniz, çok kiracılı bir Daemon uygulaması oluşturmak isteyebilirsiniz. Her kiracı yöneticisi tarafından incelenmesi gerekir.
- [Uygulama kaydı](./scenario-daemon-app-registration.md)sırasında, **Yanıt URI 'si** gerekli değildir. Parolaların veya sertifikaların veya imzalı onayların Azure AD ile paylaşılması gerekir ve bu uygulama izinlerini kullanmak için uygulama izinleri istemeniz ve yönetici onayı vermeniz gerekir.
- Uygulama [yapılandırmasının](./scenario-daemon-app-configuration.md) , uygulama kaydı SıRASıNDA Azure AD ile paylaşılan olarak istemci kimlik bilgilerini sağlaması gerekir.
- İstemci kimlik bilgileri akışı ile belirteç almak için kullanılan [kapsamın](scenario-daemon-acquire-token.md#scopes-to-request) bir statik kapsam olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Daemon uygulaması-uygulama kaydı](./scenario-daemon-app-registration.md)
