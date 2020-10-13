---
title: Web API 'Lerini çağıran bir Web API 'SI oluşturma-Microsoft Identity platform | Mavisi
description: Aşağı akış Web API 'Lerini (genel bakış) çağıran bir Web API 'SI oluşturmayı öğrenin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 4dcf377797709b56b4db735dabf4d48cfae4fc06
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91257172"
---
# <a name="scenario-a-web-api-that-calls-web-apis"></a>Senaryo: Web API 'Lerini çağıran bir Web API 'SI

Web API 'Lerini çağıran bir Web API 'SI oluşturmak için bilmeniz gerekenleri öğrenin.

## <a name="prerequisites"></a>Ön koşullar

Bu senaryo, korumalı bir Web API 'sinin diğer Web API 'Lerini çağırdığı, senaryo üzerinde derlemeler [: korumalı Web API](scenario-protected-web-api-overview.md)'si.

## <a name="overview"></a>Genel Bakış

- Web, Masaüstü, mobil veya tek sayfalı uygulama istemcisi (eşlik eden diyagramda temsil edilmez) korumalı bir Web API 'sini çağırır ve "Authorization" HTTP üstbilgisinde JSON Web Token (JWT) taşıyıcı belirteci sağlar.
- Korunan Web API 'SI, belirteci doğrular ve Microsoft kimlik doğrulama kitaplığı (MSAL) `AcquireTokenOnBehalfOf` yöntemini kullanarak korunan Web API 'sinin Kullanıcı adına ikinci bir Web API 'sini veya aşağı akış Web API 'sini çağırabilmesi için Azure Active Directory (Azure AD) ' den başka bir belirteç ister.
- Korunan Web API 'SI Ayrıca `AcquireTokenSilent` aynı kullanıcı adına diğer aşağı akış API 'leri için belirteç istemek üzere daha sonra çağrı yapabilir. `AcquireTokenSilent` gerektiğinde belirteci yeniler.

![Web API 'sini çağıran Web API 'SI diyagramı](media/scenarios/web-api.svg)

## <a name="specifics"></a>Özelliklerini

API izinleriyle ilgili uygulama kaydı bölümü klasik bir uygulamadır. Uygulama yapılandırması, JWT taşıyıcı belirtecini bir aşağı akış API 'SI belirtecine göre değiştirmek için OAuth 2,0 adına sahip akış kullanımını içerir. Bu belirteç, Web API 'sinin denetleyicilerinde kullanılabildiği belirteç önbelleğine eklenir ve ardından aşağı akış API 'Leri çağırmak için sessizce bir belirteç alabilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kaydı](scenario-web-api-call-api-app-registration.md)
