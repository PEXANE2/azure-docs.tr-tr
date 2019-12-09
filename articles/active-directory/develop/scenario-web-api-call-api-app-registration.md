---
title: Web API 'Lerini çağıran bir Web API 'sini kaydetme-Microsoft Identity platform | Mavisi
description: Aşağı akış Web API 'Lerini (uygulama kaydı) çağıran bir Web API 'SI oluşturmayı öğrenin
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
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: cb5f4763e13935b99564bfcb6d8b6e7f463ed59e
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74919809"
---
# <a name="web-api-that-calls-web-apis---app-registration"></a>Web API 'Lerini çağıran Web API 'SI-uygulama kaydı

Aşağı akış Web API 'Lerini çağıran bir Web API 'si, korumalı bir Web API 'SI ile aynı kayda sahiptir. Bu nedenle, [korumalı Web API-uygulama kaydı](scenario-protected-web-api-app-registration.md)'ndaki yönergeleri izlemeniz gerekir.

Ancak, Web uygulaması artık Web API 'Lerini çağırırsa, bu, gizli bir istemci uygulaması haline gelir. Bu nedenle, gerekli olan ek kayıt bilgileri budur: uygulamanın Microsoft Identity platformu ile gizli dizileri (istemci kimlik bilgileri) paylaşması gerekir.

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-registration-client-secrets.md)]

## <a name="api-permissions"></a>API izinleri

Web uygulamaları, taşıyıcı belirtecinin alındığı Kullanıcı adına API 'Leri çağırır. Temsilci izinleri istemeleri gerekir. Ayrıntılar için bkz. [Web API 'lerine erişim Izinleri ekleme](quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulamanın kod yapılandırması](scenario-web-api-call-api-app-configuration.md)
