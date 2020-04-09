---
title: Web API'lerini çağıran bir web API'sı oluşturma - Microsoft kimlik platformu | Azure
description: Akış aşağı web API'lerini çağıran bir web API'sini nasıl oluşturabilirsiniz (genel bakış).
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
ms.openlocfilehash: 88a0177755fbd913bdaaf0ecf3e12c62dee294c1
ms.sourcegitcommit: d187fe0143d7dbaf8d775150453bd3c188087411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80885081"
---
# <a name="scenario-a-web-api-that-calls-web-apis"></a>Senaryo: Web API'lerini çağıran bir web API'si

Web API'leri çağıran bir web API'sı oluşturmak için bilmeniz gerekenleri öğrenin.

## <a name="prerequisites"></a>Ön koşullar

Korunan bir web API'sinin web API'lerini çağırdığı bu senaryo, "Web API'sini koruyun" senaryosunun üzerine oluşturur. Bu temel senaryo hakkında daha fazla bilgi edinmek için [senaryo: Korumalı web API'sine](scenario-protected-web-api-overview.md)bakın.

## <a name="overview"></a>Genel Bakış

- Web, masaüstü, mobil veya tek sayfalı uygulama istemcisi (eşlik eden diyagramda temsil edilmez) korumalı web API'sını çağırır ve "Yetkilendirme" HTTP üstbilgisinde bir JSON Web Belirteci (JWT) taşıyıcı belirteci sağlar.
- Korumalı web API belirteci doğrular ve korumalı web `AcquireTokenOnBehalfOf` API'sinin kullanıcı adına ikinci bir web API'sını veya aşağı akış web API'sini çağırabilmesi için Azure Etkin Dizin'den (Azure AD) başka bir belirteç istemek için Microsoft Kimlik Doğrulama Kitaplığı (MSAL) yöntemini kullanır.
- Korumalı web API'si, aynı kullanıcı adına diğer akış aşağı API'leri için belirteçleri istemek için daha sonra arayabilir. `AcquireTokenSilent` `AcquireTokenSilent`gerektiğinde belirteci yeniler.

![Web API çağıran bir web API diyagramı](media/scenarios/web-api.svg)

## <a name="specifics"></a>Özellikleri

API izinleri ile ilgili uygulama kayıt bölümü klasiktir. Uygulama yapılandırması, JWT taşıyıcı belirtecinin bir alt akış API'si için bir belirteci yle değiş tokuş etmek için OAuth 2.0 On-Behalf-Of akışını kullanmayı içerir. Bu belirteç, web API denetleyicilerinde kullanılabildiği belirteç önbelleğine eklenir ve daha sonra akış aşağı API'lerini çağırmak için sessizce bir belirteç edinebilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kaydı](scenario-web-api-call-api-app-registration.md)
