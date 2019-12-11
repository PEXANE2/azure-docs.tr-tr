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
ms.openlocfilehash: 4b357def86b77d4bbb294e2253dacfbd129998ec
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74965135"
---
# <a name="scenario-web-api-that-calls-web-apis"></a>Senaryo: Web API 'Lerini çağıran Web API 'SI

Web API 'Lerini çağıran bir Web API 'SI oluşturmak için ihtiyacınız olan her şey hakkında bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

Web API 'Lerini çağıran korumalı Web API 'SI olan bu senaryo, "bir Web API 'sini koruma" senaryosunun üzerine oluşturulur. Bu temel senaryo hakkında daha fazla bilgi edinmek için önce [Protected Web API-Scenario](scenario-protected-web-api-overview.md) bölümüne bakın.

## <a name="overview"></a>Genel Bakış

- Bir istemci (Web, Masaüstü, mobil veya tek sayfalı uygulama)-aşağıdaki diyagramda temsil edilmeyen korumalı bir Web API 'sini çağırır ve "Authorization" http üstbilgisinde bir JWT taşıyıcı belirteci sağlar.
- Korunan Web API 'SI belirteci doğrular ve MSAL `AcquireTokenOnBehalfOf` yöntemini (Azure AD 'den) Kullanıcı adına ikinci bir Web API 'SI (aşağı akış Web API 'SI olarak adlandırılır) çağırmak için başka bir belirteç kullanacak şekilde kullanır.
- Korumalı Web API 'si, bir aşağı akış API 'SI çağırmak için bu belirteci kullanır. Ayrıca, diğer aşağı akış API 'Leri için belirteçleri istemek üzere daha sonra `AcquireTokenSilent`çağırabilir (ancak yine de aynı kullanıcı adına). `AcquireTokenSilent`, gerektiğinde belirteci yeniler.

![Web API 'SINI çağıran Web API 'SI](media/scenarios/web-api.svg)

## <a name="specifics"></a>Özelliklerini

API izinleriyle ilgili uygulama kaydının bir parçası klasik olarak görüntülenir. Uygulama yapılandırması, JWT taşıyıcı belirtecini bir aşağı akış API 'SI belirtecine göre değiştirmek için OAuth 2,0 adına sahip akış kullanımını içerir. Bu belirteç, Web API 'sinin denetleyicilerinde kullanılabildiği belirteç önbelleğine eklenir ve aşağı akış API 'Lerini çağırmak için sessizce bir belirteç alabilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kaydı](scenario-web-api-call-api-app-registration.md)
