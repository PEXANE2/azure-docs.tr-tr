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
ms.openlocfilehash: c9cc8e6fcc796cfc7a09cf7bc1431646a6603af0
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94442982"
---
# <a name="scenario-protected-web-api"></a>Senaryo: korumalı Web API 'SI

Bu senaryoda, bir Web API 'sini kullanıma sunma hakkında bilgi edineceksiniz. Ayrıca, Web API 'sini yalnızca kimliği doğrulanmış kullanıcıların erişebileceği şekilde nasıl koruyacağınızı öğreneceksiniz.

Web API 'nizi kullanmak için hem iş hem de okul hesaplarıyla kimliği doğrulanmış kullanıcıları etkinleştirmeniz ya da Microsoft kişisel hesaplarını etkinleştirmeniz gerekir.

## <a name="specifics"></a>Özelliklerini

Web API 'Lerini korumak için bilmeniz gereken belirli bilgiler aşağıda verilmiştir:

- Uygulama kaydınız en az bir *kapsam* veya bir *uygulama rolü* kullanıma sunmalıdır.
  - Kapsamlar, bir kullanıcı adına çağrılan Web API 'Leri tarafından sunulur.
  - Uygulama rolleri, Daemon uygulamaları tarafından çağrılan Web API 'Leri tarafından sunulur (Web API 'nizi kendi adına çağırır).
- Yeni bir Web API 'SI uygulama kaydı oluşturursanız, Web API 'niz tarafından kabul edilen [erişim belirteci sürümünü](reference-app-manifest.md#accesstokenacceptedversion-attribute) seçin `2` . Eski Web API 'Leri için kabul edilen belirteç sürümü olabilir `null` , ancak bu değer, oturum açma kitlesini yalnızca kuruluşlar ile kısıtlar ve kişisel Microsoft hesapları (MSA) desteklenmez.
- Web API 'SININ kod yapılandırması, Web API 'SI çağrıldığında kullanılan belirteci doğrulamalıdır.
- Denetleyici eylemlerindeki kodun, belirteçteki rol veya kapsamları doğrulaması gerekir.

## <a name="recommended-reading"></a>Önerilen okuma

[!INCLUDE [recommended-topics](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu senaryonun [uygulama kaydı](scenario-protected-web-api-app-registration.md)olan sonraki makaleye geçin.
