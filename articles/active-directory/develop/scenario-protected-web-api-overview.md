---
title: Korumalı Web API 'SI-genel bakış
titleSuffix: Microsoft identity platform
description: Korumalı bir Web API 'SI (genel bakış) oluşturmayı öğrenin.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/14/2020
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 4e530f76c8301dc74f73b675befa6f0710aedab7
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87026637"
---
# <a name="scenario-protected-web-api"></a>Senaryo: korumalı Web API 'SI

Bu senaryoda, bir Web API 'sini kullanıma sunma hakkında bilgi edineceksiniz. Ayrıca, Web API 'sini yalnızca kimliği doğrulanmış kullanıcıların erişebileceği şekilde nasıl koruyacağınızı öğreneceksiniz.

Web API 'nizi kullanmak için hem iş hem de okul hesaplarıyla kimliği doğrulanmış kullanıcıları etkinleştirmeniz ya da Microsoft kişisel hesaplarını etkinleştirmeniz gerekir.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="specifics"></a>Özelliklerini

Web API 'Lerini korumak için bilmeniz gereken belirli bilgiler aşağıda verilmiştir:

- Uygulama kaydınız en az bir *kapsam* veya bir *uygulama rolü*kullanıma sunmalıdır.
  - Kapsamlar, bir kullanıcı adına çağrılan Web API 'Leri tarafından sunulur.
  - Uygulama rolleri, Daemon uygulamaları tarafından çağrılan Web API 'Leri tarafından sunulur (Web API 'nizi kendi adına çağırır).
- Yeni bir Web API 'SI uygulama kaydı oluşturursanız, Web API 'niz tarafından kabul edilen [erişim belirteci sürümünü](reference-app-manifest.md#accesstokenacceptedversion-attribute) seçin `2` . Eski Web API 'Leri için kabul edilen belirteç sürümü olabilir `null` , ancak bu değer, oturum açma kitlesini yalnızca kuruluşlar ile kısıtlar ve kişisel Microsoft hesapları (MSA) desteklenmez.
- Web API 'SININ kod yapılandırması, Web API 'SI çağrıldığında kullanılan belirteci doğrulamalıdır.
- Denetleyici eylemlerindeki kodun, belirteçteki rol veya kapsamları doğrulaması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kaydı](scenario-protected-web-api-app-registration.md)
