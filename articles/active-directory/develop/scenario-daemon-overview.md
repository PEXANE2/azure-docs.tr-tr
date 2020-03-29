---
title: Web API'lerini çağıran bir daemon uygulaması oluşturun - Microsoft kimlik platformu | Azure
description: Web API'lerini çağıran bir daemon uygulaması oluşturmayı öğrenin
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
ms.date: 01/31/2020
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 5718a23e5669de6ba16354a718d72b68d14bbf49
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78894545"
---
# <a name="scenario-daemon-application-that-calls-web-apis"></a>Senaryo: Web API'leri çağıran Daemon uygulaması

Web API'leri çağıran bir daemon uygulaması oluşturmak için ihtiyacınız olan her şeyi öğrenin.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="overview"></a>Genel Bakış

Başvurunuz kendisi adına bir web API aramak için bir belirteç edinebilirsiniz (bir kullanıcı adına değil). Bu senaryo daemon uygulamaları için yararlıdır. Standart OAuth 2.0 [istemci kimlik bilgilerini](v2-oauth2-client-creds-grant-flow.md) kullanır.

![Daemon uygulamaları](./media/scenario-daemon-app/daemon-app.svg)

Daemon uygulamaları için kullanım örnekleri aşağıda verilmiştir:

- Kullanıcıları sağlamak veya yönetmek veya bir dizinde toplu işlem yapmak için kullanılan web uygulamaları
- Toplu iş yapan masaüstü uygulamaları (Windows'daki Windows hizmetleri veya Linux'taki daemon işlemleri gibi) veya arka planda çalışan bir işletim sistemi hizmeti
- Belirli kullanıcıları değil, dizinleri işlemesi gereken Web API'leri

Daemon olmayan uygulamaların istemci kimlik bilgilerini kullandığı başka bir yaygın durum daha vardır: kullanıcılar adına hareket etseler bile, teknik nedenlerle bir web API'sine veya bir kaynağa kendi kimlikleri altında erişmeleri gerekir. Bir örnek, önbellek için Azure Key Vault veya Azure SQL veritabanındaki sırlara erişimdir.

Kendi kimlikleri için bir belirteç edinen uygulamalar:

- Gizli istemci uygulamalarıdır. Kaynaklara kullanıcılardan bağımsız olarak erişen bu uygulamaların kimliklerini kanıtlamaları gerekir. Ayrıca oldukça hassas uygulamalardır. Azure Etkin Dizin (Azure AD) kiracı yöneticileri tarafından onaylanması gerekir.
- Azure AD ile gizli (uygulama parolası veya sertifika) kaydettiniz. Bu sır, bir belirteç almak için Azure AD'ye yapılan çağrı sırasında iletilir.

## <a name="specifics"></a>Özellikleri

> [!IMPORTANT]
>
> - Kullanıcılar bir daemon uygulaması ile etkileşim kuramaz. Bir daemon uygulaması kendi kimliğini gerektirir. Bu tür bir uygulama, uygulama kimliğini kullanarak ve uygulama kimliğini, kimlik bilgilerini (parola veya sertifika) ve uygulama kimliği URI'yi Azure AD'ye sunarak bir erişim belirteci ister. Başarılı kimlik doğrulamasından sonra, daemon Microsoft kimlik platformu bitiş noktasından bir erişim belirteci (ve yenileme belirteci) alır. Bu belirteç daha sonra web API'yi aramak için kullanılır (ve gerektiğinde yenilenir).
> - Kullanıcılar daemon uygulamalarıyla etkileşim kuramadığından, artımlı onay mümkün değildir. Gerekli tüm API izinlerinin başvuru kaydında yapılandırılması gerekir. Uygulamanın kodu yalnızca statik olarak tanımlanmış izinler ister. Bu aynı zamanda daemon uygulamalarının artımlı onayı desteklemeyeceği anlamına gelir.

Geliştiriciler için, bu senaryonun uç-uç deneyimi aşağıdaki yönleri vardır:

- Daemon uygulamaları yalnızca Azure AD kiracılarında çalışabilir. Microsoft'un kişisel hesaplarını manipüle etmeye çalışan bir daemon uygulaması oluşturmak mantıklı olmaz. İşletmeler arasında bir uygulama geliştiricisiyseniz, kiracınızda daemon uygulamanızı oluşturursunuz. ISV'yseniz, çok kiracılı bir daemon uygulaması oluşturmak isteyebilirsiniz. Her kiracı yöneticinin onay vermesi gerekir.
- [Başvuru kaydı](./scenario-daemon-app-registration.md)sırasında, yanıt URI gerekli değildir. Azure AD ile sırları veya sertifikaları veya imzalı iddiaları paylaşmanız gerekir. Ayrıca uygulama izinleri istemeniz ve yöneticiye bu uygulama izinlerini kullanma izni vermeniz gerekir.
- [Uygulama yapılandırmasının,](./scenario-daemon-app-configuration.md) uygulama kaydı sırasında Azure AD ile paylaşılan istemci kimlik bilgilerini sağlaması gerekir.
- İstemci kimlik bilgileri akışı ile bir belirteç elde etmek için kullanılan [kapsam](scenario-daemon-acquire-token.md#scopes-to-request) statik bir kapsam olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Daemon uygulaması - uygulama kaydı](./scenario-daemon-app-registration.md)
