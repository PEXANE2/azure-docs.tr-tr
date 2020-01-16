---
title: Web API 'Lerini çağıran bir Web API 'SI oluşturma-Microsoft Identity platform | Mavisi
description: Aşağı akış Web API 'Lerini (genel bakış) çağıran bir Web API 'SI oluşturmayı öğrenin.
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
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: b67507daf8005f3f9a299b778f1fba4ce67d46d4
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76044153"
---
# <a name="scenario-a-web-api-that-calls-web-apis"></a>Senaryo: Web API 'Lerini çağıran bir Web API 'SI

Web API 'Lerini çağıran bir Web API 'SI oluşturmak için bilmeniz gerekenleri öğrenin.

## <a name="prerequisites"></a>Ön koşullar

Bu senaryoda, korumalı bir Web API 'SI Web API 'Lerini çağırdığında, "bir Web API 'sini koruma" senaryosunun üzerine oluşturulur. Bu temel senaryo hakkında daha fazla bilgi edinmek için bkz. [Senaryo: korumalı Web API 'si](scenario-protected-web-api-overview.md).

## <a name="overview"></a>Genel Bakış

- Web, Masaüstü, mobil veya tek sayfalı uygulama istemcisi (eşlik eden diyagramda temsil edilmez) korumalı bir Web API 'sini çağırır ve "Authorization" HTTP üstbilgisinde JSON Web Token (JWT) taşıyıcı belirteci sağlar.
- Korunan Web API 'SI, belirteci doğrular ve Microsoft kimlik doğrulama kitaplığı (MSAL) `AcquireTokenOnBehalfOf` Azure Active Directory yöntemini kullanarak korunan Web API 'sinin Kullanıcı adına ikinci bir Web API 'sini veya aşağı akış Web API 'sini çağırabilmesini sağlar.
- Korunan Web API 'SI ayrıca aynı kullanıcı adına diğer aşağı akış API 'Leri için belirteçleri istemek üzere `AcquireTokenSilent`de çağırabilir. `AcquireTokenSilent`, gerektiğinde belirteci yeniler.

![Web API 'sini çağıran Web API 'SI diyagramı](media/scenarios/web-api.svg)

## <a name="specifics"></a>Özelliklerini

API izinleriyle ilgili uygulama kaydı bölümü klasik bir uygulamadır. Uygulama yapılandırması, JWT taşıyıcı belirtecini bir aşağı akış API 'SI belirtecine göre değiştirmek için OAuth 2,0 adına sahip akış kullanımını içerir. Bu belirteç, Web API 'sinin denetleyicilerinde kullanılabildiği belirteç önbelleğine eklenir ve ardından aşağı akış API 'Leri çağırmak için sessizce bir belirteç alabilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kaydı](scenario-web-api-call-api-app-registration.md)
