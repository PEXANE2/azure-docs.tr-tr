---
title: include dosyası
description: include dosyası
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 01/15/2020
ms.custom: include file
ms.openlocfilehash: cb43c8b8c952d8db6cf450a7015c22c85e7fe4b5
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76268200"
---
`objectIdType` (veya **nesne tanımlayıcı türü**), bir role verilen kimliğin türünü ifade eder. `DeviceId` ve `UserDefinedFunctionId` türlerinden ayrı olarak, nesne tanımlayıcı türleri Azure Active Directory nesnelerinin özelliklerine karşılık gelir.

Aşağıdaki tabloda, Azure dijital TWINS 'de desteklenen nesne tanımlayıcı türleri yer almaktadır:

| Tür | Açıklama |
| --- | --- |
| UserId | Bir kullanıcıya bir rol atar. |
| DeviceId | Cihaza bir rol atar. |
| DomainName | Bir etki alanı adına bir rol atar. Belirtilen etki alanı adına sahip her Kullanıcı ilgili rolün erişim haklarına sahiptir. |
| TenantId | Bir kiracıya bir rol atar. Belirtilen Azure AD kiracı KIMLIĞINE ait olan her Kullanıcı ilgili rolün erişim haklarına sahiptir. |
| Serviceprincipalıd | Hizmet sorumlusu nesne KIMLIĞINE bir rol atar. |
| Userdefinedfunctionıd | Kullanıcı tanımlı bir işleve (UDF) bir rol atar. |